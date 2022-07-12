---
title: docker的三种容器互访方式
tags: 
  - docker
categories: 
  - 笔记
date: 2022-07-04 14:46:10
---

最近在学习nacos，为了不让自己的电脑变得杂乱无章，所以我比较推崇尽量都将服务放在docker中。以前装的服务比如mysql、redis都不涉及到容器互访，所以没有什么问题；这次使用nacos需要配置mysql持久化，nacos需要调用mysql的服务，遇到了一些问题。

起初我是这样写nacos的配置的：

```properties
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://localhost:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user=root
db.password=******
```

运行一直报错：`No datasource set`，在说下去之前，先来介绍一下docker的四种网络模式。

我们在使用docker run创建Docker容器时，可以用 --net 选项指定容器的网络模式，Docker可以有以下4种网络模式：

* bridge模式：
  使用`--net=bridge`指定；Docker 在启动时会开启一个虚拟网桥设备 docker0，容器启动后都会被桥接到 docker0 上，会为每个容器分配一个独立的Network Namespace。
* host模式：
  使用`--net=host`指定；容器和宿主机共享Network namespace。
* container模式：
  使用`--net=container:NAME_or_ID`指定；容器和另外一个容器共享Network namespace。
* none模式：
  使用`--net=none`指定；容器有独立的Network namespace，但并没有对其进行任何网络设置，如分配veth pair 和网桥连接，配置IP等。

在不添加--net参数时，默认使用的是bridge模式，也就是说，容器是互相隔离的，所以在nacos配置中使用`localhost:3306`来访问mysql是错误的。显然我们可以通过改变容器的网络模式为host或者container来解决这个问题，但是这样我觉得容器就失去了独立性。在保留容器网络模式为bridge的前提下，还有三种网络互访模式：

### 虚拟ip访问

在bridge模式下，同一个宿主机上的所有容器会在同一个网段下，相互之间是可以通信的。

运行容器hello1和hello2：

```shell
docker run --name hello1 -d redis:7.0.2
docker run --name hello2 -d redis:7.0.2
```

查看hello1容器的ip：

![image-20220704105735155](https://s2.loli.net/2022/07/12/I4zElU9k3BQ7iuf.png)

查看hello2容器的ip：

![image-20220704105945893](https://s2.loli.net/2022/07/12/n1JPGrVaWwgUREs.png)

ping一下：

![image-20220704120847776](https://s2.loli.net/2022/07/12/9B3RKicgyCtqQvD.png)



这种方式必须知道容器的ip，并且在容器重启时ip可能会改变，所以并不实用。

### link方式

运行容器的时候加上参数--link

运行第一个容器

```shell
docker run --name hello1 -d redis:7.0.2
```

运行第二个容器

```shell
docker run --name hello2 --link hello1:hello1 -d redis:7.0.2
```

--link：参数中第一个hello1是**容器名**，第二个hello1是定义的**容器别名**（使用别名访问容器），为了方便使用，一般别名默认容器名。

ping一下：

![image-20220704121534814](https://s2.loli.net/2022/07/12/UkGsVMBy6Ecn4PH.png)

这种方式对容器的启动顺序有要求，对于多个容器需要互访就会比较麻烦。

### 新建bridge网络

运行如下命令创建bridge网络：

~~~shell
docker network create <网络名>
~~~

![image-20220704121937614](https://s2.loli.net/2022/07/12/yPMr2TOfHuBVkQW.png)

然后运行容器连接到网络：

~~~
docker run -it --name <容器名> ---network <网络名> --network-alias <在网络中别名> <镜像名>
~~~

对于正在运行的容器可以使用`docker network connect`

~~~
docker network connect --alias <在网络中别名> <网络名> <容器>
~~~

比如：

~~~
docker network connect --alias mysql center mysql
docker network connect --alias nacos center nacos
~~~

这样我就可以直接在nacos配置中用别名来代替地址：

~~~properties
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://mysql:3306/nacos_config?characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useUnicode=true&useSSL=false&serverTimezone=UTC
db.user=root
db.password=root
~~~

访问nacos，发现运行正常

![image-20220704123151715](https://s2.loli.net/2022/07/12/NrM1THcnX2PzSh5.png)

> 注意：在默认的bridge网络中是无法使用别名的
