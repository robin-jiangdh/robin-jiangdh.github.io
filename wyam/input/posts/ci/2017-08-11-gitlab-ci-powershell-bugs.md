---
title: gitlab ci与Powershell 乱码
Published: 08/11/2017
tags: ['gitlab-ci','powershell','msbuild'] 
---
存在多种情况下的乱码，应用级别的乱码和控制台乱码
应用级别的乱码类似如下：

> error MSB1009: ��Ŀ�ļ������ڡ�

这类需要配置应用的中文输出
> 修改cmd的代码页即可。
一种方法是在cmd的属性中修改默认代码页为437，第二种是用chcp 437命令修改。
不过这两种解决方法的副作用是msbuild的输出会变为英文。
更好的解决方法暂时还没倒腾出来。

第二种，控制台乱码，这个难治，而且原因千奇百怪

参考方案如下：

[解决windowspowershell中文显示问号及乱码问题](https://blog.csdn.net/weixin_43426860/article/details/83348284)

[Windows 10 powershell 中文乱码解决方案](https://www.cnblogs.com/weihanli/p/fix-Chinese-characters-display-abnormal.html)

如果以上两种方案都不适用与你的化，那么放大招了，gitlab-ci的宿主系统切换成windows server2016, 实测有效