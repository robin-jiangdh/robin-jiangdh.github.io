---
title: 简历
 
---
# 个人信息
 - 姜东红/男/1991 
- Email：me@robinjiang.com  
- 本科/安徽理工大学
- 工作年限：7年
- 技术博客：http://blog.robinjiang.com 

---

# 工作经历

- 深圳市格狮优科技有限公司 （ 2015年5月 ~ 2020年4月 ）

   主要参与加密货币交易所的研发

- 深圳市江胜科技有限公司 （ 2017年3月 ~ 2018年5月 ）

   主要参与基于速卖通和亚马逊平台的跨境电子商务ERP解决方案的研发

- 深圳市收立方科技有限公司 （ 2015年6月 ~ 2017年3月 ）
   主要参企业移动媒体平台、跨境电子商务解决方案(ERP和商城)的研发

- 深圳市中为世纪科技有限公司 （ 2014年6月 ~ 2015年6月 ）
   参与公司电子商务平台的搭建以及业务系统的维护工作
   
- 富泰华工业（深圳）有限公司 （ 2013年7月 ~ 2014年6月 ）
   参与内部信息系统维护，并参与了事业群门户网站建设
# 项目经历
### 交易系统微服务架构升级 
这是一套针对加密货币的交易系统，核心业务模块包括用户管理，资产管理，风控系统，挂单系统，撮合系统以及消息推送系统。整体架构从上而下依次是 应用层(包括web站点，app,h5站点以及api服务)，api网关层，接口层，服务层(核心业务和后台服务)和数据层。

在本系统中，我主要负责用户管理模块，撮合模块，消息推送系统以及api网关层相关的设计和开发工作。其中行情服务是撮合和消息推送系统的关联共用模块。
1、 用户模块为基于identityserver4进行开发的，并通过api为网关模块提供api鉴权的数据支持；
2、 关于行情服务，主要接受来自挂单系统的数据和撮合系统的结果作为数据源，用于计算实时盘口行情，并提供给推送服务和撮合系统用作下一轮撮合的初始数据。
3、 关于推送服务，以来自行情服务器的行情数据，来自资产服务的用户资产变更信息以及用户系统的站内信等内容作为信息源，根据客户端选择订阅的消息来进行推送数据。譬如，用户登陆后，客户端会发起一个资产信息订阅的请求，其后关于该客户的所有资产变更消息将通过signalr推送到客户端。
4、 撮合模块，需要两个数据源，分别是新挂单数据和盘口数据，盘口数据主要来自行情服务器。基于数据保护原则，大部分异常都会使当前挂单会被回滚，以保证当前用户资产的安全性。正常情况下完成撮合后会获取一个撮合结果，其中包含变更的盘口信息及新增的成交单信息，和一笔已经被处理的挂单信息。等撮合结果落库后，该消息才会被传递到行情服务用于下一步处理。此消息属于可失败的消息，最终行情服务会订阅最新成交单信息，如果最新成交单与行情服务下的最新成交单不符合，会自动纠错并重建该交易对部分或全部行情

### 使用IdentityServer4构建基于OAuth2和SSO的开放平台 
1、整合现有用户数据，并基于IdentityServer4重新实现用户登录，授权等相关操作；
2、考虑到客户端类型的不同，登陆时需要提供不同的登录方案，除了提供用于登录的api外还提供了基于网页跳转登录的接口；
3、由于idsvr4提供的原始access_token过长，在网关服务层提供了一token映射的功能，即通过guid生成唯一的32位字符串来代替完整的access_token。如果客户端接收需要完整的access_token的话，则可以继续使用完整的token来处理，但向服务端请求的过程中只接受精简的token；
4，关于token验证的缓存问题，除了idsvr提供了一套缓存机制外，在api网关下也提供了一套缓存机制，默认情况下，api网关下的缓存有效时间小于idsvr的缓存时间。并且这两套缓存机制物理隔离。 
5、为用户行为提供消息订阅机制，业务系统可以订阅此类消息并处理。例如推送中心可以订阅用户登录状态改变消息，将客户未读信息标记后通过signalr推送到客户端上。
6、基于日志系统的需求，用户登录后应给予一个唯一追踪id用于追踪用户在系统内的活动；

### 全球交易助手ERP项目 
这是一个针对速卖通商家店铺的一个综合管理erp项目，包含订单模块，物流模块，平台（店铺）模块、仓库模块、采购模块和客服模块构成。本项目基于微服务架构进行开发，并通过统一的api网关进行访问。 
在本系统中，除了参与早期项目架构设计外，主要负责物流模块的设计与实现。物流模块下核心功能主要包括物流授权管理，批量报关设置，基本物流信息维护，常用报关维护，拣货单模板，运费模板，运费计算方案，面单打印。考虑到对接的物流供应商数量巨多，基于提供者模式设计了物流apiproxy，用于简化业务系统对物流商api的操作。由于需要提供客户端和web端两种打印方案，所以默认以json格式提供面单数据，客户端和web端可以自行渲染和打印，后期基于效率考虑，也在服务端增加了pdf打印方案，但效果不是很理想



---
# 技能清单 

以下均为我熟练使用的技能

- Api网关：kong/Ocelot
- Web框架：ASP.NET MVC/ASP.NET WebAPI/NancyFx /ASP.NET Core
- Web服务器 ：Nginx/IIS/Owin(selfHost/katana/NancyFx)/Apache
- ORM框架：Dapper/EntityFramework/ADO.NET
- 前端框架：JQuery/Bootstrap/VueJS
- 数据库相关：MySQL/SQLServer/Mongodb 
- 单元测试：xUnit/Moq/Jetbrain dotCover
- 测试相关：Jmeter/AbTest
- 云和开放平台：AWS/Azure/Aliyun
- 定时任务相关：hangfire/Quartz.net
- OAuth2相关：IdentityServer4/DotNetOpenAuth/Aspnet OAuth
- 自动化构建系统:cakebuild
- 代码质量管理：SonarQube
- 其他：Gitlab/Gitlab CI /Cakebuild/TopShelf/SwaggerUI/Swaggger-Codegen/Powerbuilder