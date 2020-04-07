---
title: wyam:基于dotnet的静态内容生成器
Published: 03/10/2020
tags: ['wyam','静态内容生成器'] 
---


> vps到期了，然后疫情原因，一直没时间去管，去续费的时候才发现之前写的东西全丢了，现在只能挨个找之前的备份，痛定思痛，准备找个大概率不会挂机的服务商了，然后就盯上了github-page,用于托管点博客是够了，而且咱这种也不靠博客盈利的，纯粹就是找个垃圾桶写文章而已，够了够了

> 劝退指南，目前作者团队已经放弃wyam去开发另外一个项目了[statiq](https://github.com/statiqdev),也是一个静态内容生成器，目前该项目还不完整，demo都还不完善
>
> 目前本博就是基于wyam的，如果需要demo的话，可以去这里看 
>
> > 本博客 https://github.com/robin-jiangdh/robin-jiangdh.github.io
> >
> > 官方网站： https://github.com/Wyamio/Wyam.web

## Wyam的介绍

Wyam.io官网上的自我介绍基本上把Wyam是什么说的很清楚了，我就简单在这里翻译一下。

> Wyam是与众不同的。它不是Jekyll的克隆（并不代表Jekyll有任何问题），它不是设计来生成博客的（虽然也能很好的胜任此任务）。Waym是一个静态内容生成器，可以用于生成网站、文档、电子书和其他更多的内容。由于它的所有东西都是通过很多灵活的模块（你也可以编写自己的模块）串在一起，所以唯一的限制是你的想象力。
>
> > 自带吐槽技能啊，这个Wyam对标的有点像docfx，不过目标跨的太大，扯着蛋了，后面又另起炉灶了，不过如果你只是用blog功能的话还是可以的，目前算是功能完善
> >
> > update 2020-03-24:
> >
> > 用了两周，目前图片，标签系统或多或少有点bug，tags真心一团糟，我fork了一份自己在维护

在它的特性当中，尤其让我看中的是：

- [配置文件](http://wyam.io/getting-started/configuration)使用C#脚本写就，这完全是得益于Roslyn的强大
- 简单直接的[元数据](http://wyam.io/modules/meta)使用方式
- 支持多种模板引擎和语言，尤其直接内置[Razor](http://wyam.io/modules/razor)的支持（且Razor的支持是基于ASP.NET MVC 6的源代码的，未来会支持TagHelper） 。当然也有Markdown支持或者扩展自己的模板语言支持。
- [集成Web Server](http://wyam.io/getting-started/usage)方便在编写模板的时候进行预览
- 完全[支持Nuget](http://wyam.io/getting-started/configuration#nuget)，可以在执行生成的过程中，自动下载依赖的Nuget包
- 更为重要的，它支持[嵌入运行](http://wyam.io/knowledgebase/embedded-use)
- 相对完整清晰的文档

Waym其实借鉴了现有其他静态内容生成器的优点和设计，比如FrontMatter的支持（通过Yaml实现）。目前还只是[v2.2.9](https://github.com/Wyamio/Wyam/releases/tag/v2.2.9)，但是功能完成度还是比较高了，并且你也可以直接pull request参与贡献。源代码地址是：https://github.com/Wyamio/Wyam



