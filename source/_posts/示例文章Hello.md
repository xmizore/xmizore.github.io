---
title: 示例文章Hello
tags: 
  - Jvav
---
平时开发需要多台 Linux 设备搭建分布式环境，但因为穷，只能在实验室电脑上用虚拟机搭建，正好体验一把 Arch Linux 的安装，于是昨天就用 VMware 安装了 Arch Linux（好像也没传闻中那么困难）不过晚些时候沙雕网友叫我用更方便的 WSL2，于是今天就试了试，从此告别 VMware。
| 序号  |    姓名    |   学号    |   哈哈   |  是你吗 |
| :---: | :--------: | :-------: | :------: | ------: |
|   1   |    张杰    |   2233    | ddd21543 |  dddddd |
| adsda |    ads     | ddddddddd |  dasda   |    adad |
|  ads  | dasdsadasd |    as     |  adasd   | ddasdad |
|  ad   |     sd     |   dasd    |   dasd   |   sadas |
|  ads  |     da     |   adsad   |  adasd   |    adad |

WSL (Windows Subsystem for Linux) 是 Windows 自带的 Linux 子系统功能，目的是在 Windows 上运行 GNU/Linux 环境，包括大多数命令行工具和一些应用程序，没有传统的虚拟机（如 VMware）或 DualBoot 设置的开销。WSL2 是 WSL 的第二个版本，比第一个版本有更高的性能，具体差别见官方文档 [Comparing WSL and WSL2](https://docs.microsoft.com/en-us/windows/wsl/compare-versions)。这篇文章简单记录下 WSL2 的启用方式，你也可以直接根据 [官方文档](https://docs.microsoft.com/en-us/windows/wsl/about) 来操作。

## 启用 WSL2

目前官方给了自动和手动两种启用方式，较新的 Windows10 和 Windows11 方式。`<div class= "theme-switcher">😳</div>`

### 自动

以管理员身份打开 PowerShell 或 CMD 运行下面命令即可。

~~~html
<header>
  <ul class="nav">
    <li class="nav-child">
      <a href="/">首页</a>
    </li>
    <li class="nav-child">
      <a href="/category">分类</a>
    </li>
    <li class="nav-child">
      <a href="/tags">标签</a>
    </li>
    <li class="nav-child">
      <a href="/about">关于</a>
    </li>
    <li class="nav-child">
      <a href="/collection">收藏</a>
    </li>
  </ul>
  <div class="theme-switcher">😳</div>
</header>
~~~

使用图形界面的话，首先打开 “控制面板”，然后点击 “程序”，再点击 “启用或关闭 Windows 功能”，勾选 “适用于 Linux 的 Windows 子系统” 和 “虚拟机平台”，最后点击 “确定”。然后会要求重启，立刻重启即可。

#### 更新 Linux 内核包

在官方给的这个 [链接](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi) 下载新的 Linux 内核安装程序，打开根据提示安装即可。

#### 设置 WSL2 为默认使用版本

同样的，使用 PowerShell 和 CMD 运行下面命令
