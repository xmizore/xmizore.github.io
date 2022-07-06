---
title: insnow主题食用指南
tags: 
  - hexo
categories: 
  - 随便写点咯
date: 2022-07-06 16:59:10
---
首先关于insnow这个主题：我用过许多hexo主题，比如Next、Material、Icarus，甚至尝试过Typecho的Miracle，这些主题我用一段时间就腻了，原因就是我总觉得我的网站不属于自己，这种在别人网站上写文章的感觉让我很长一段时间都不想写文章。直到我发现了我的好朋友[XJJ](https://xjj.pub)写的[hugo-theme-minima](https://github.com/Mivinci/hugo-theme-minima)主题，主题非常简约，没有什么封面图、背景音乐、花里胡哨的动画效果，我很快被这种简约至上的想法吸引，我也想写一个类似minima的hexo主题，但是呢，我也不想完全照抄，所以我借鉴了minima并且添加了自己的想法写出了insnow。

有趣的是，hugo-theme-minima借鉴了[hexo-theme-minima](https://github.com/adisaktijrs/hexo-theme-minima)，这一开始我是没注意到的，hexo-theme-insnow借鉴了hugo-theme-minima。

### hexo配置

hexo的配置中主要配置基本信息和对prismjs的支持，不然无法正常显示代码块。

~~~yaml
title: I'm Mizore 
subtitle: 我是一个小可爱 😊
description: 一段对博客的介绍
author: Mizore

# 开启对prismjs的支持
prismjs:
  enable: true
  preprocess: false
  line_number: true 
  tab_replace: ''
  
theme: insnow
~~~

### 主题配置

收藏页面在主题配置中配置，格式如下

~~~yaml
#收藏页面配置
collection:
  - title: 怎么和最喜欢的人在一起？
    link: https://xjj.pub/idea/e/
    author: XJJ
    site: https://xjj.pub
  - title: 十大经典排序算法总结
    link: https://javaguide.cn/cs-basics/algorithms/10-classical-sorting-algorithms.html
    author: JavaGuide
    site: https://javaguide.cn/
~~~

### 新建页面

分类、标签、关于、收藏页面都需要新建page

~~~shell
hexo new page categories
hexo new page tags
hexo new page about
hexo new page collections
~~~

并且在`index.md`头部添加layout信息，例如tags页面：

~~~
---
title: 标签
layout: tags
---

~~~

别忘了自己的关于页面中的文章是编辑about的index.md，比如我的：

~~~
---
title: 关于
layout: about
date: 2022-05-12 16:50:36

---

你好，我是 Mizore，也叫雨夹雪。

如果你想和我交个朋友，请联系我：

email：mizoreyo@outlook.com

qq: 1123391100

~~~

在这些都弄好以后，博客应该能正确展示了，如果还有其他问题，可以参照我的blog源码仓库[mizoreyo/mizore-blog-hexo](https://github.com/mizoreyo/mizore-blog-hexo)

进行配置或者联系我。
