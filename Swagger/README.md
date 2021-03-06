# 指南

## 为什么要引入接口管理

**早期后端时代：**前端只用出前端静态界面html给后端，后端开发人员使用jsp、freemarker等模板引擎将数据渲染到页面中，动态页面的渲染通过后端完成，和前端基本不涉及接口交互。前后端内容部署在一起。

**前后端分离时代：**

- 后端：后端控制层、服务层、数据访问层。【后端团队】
- 前端：前端控制层、视图层【前端团队】

前端和后端实现分离，中间的交互通过接口交互，松耦合，依赖接口开发，mock数据，谁也不用等着谁。

​		围绕接口交互的相关问题就出现了，api接口指定，api文档管理、给服务发api请求、给前端mock数据、项目文档管理、文档版本管理等等，围绕这些问题的接口管理工具就孕育而生了。

## Swagger指南

[官网入口](https://swagger.io/)

[介绍以及使用](https://www.jianshu.com/p/349e130e40d5)

“Swagger是一个规范和完整的框架，用于生成、描述、调用和可视化RESTful风格的Web服务。”简单来说，Swagger是一个功能强大的接口管理工具，并且提供了多种编程语言的前后端分离解决方案。Swagger主要包含了以下4个部分： 

1. Swagger可以直接嵌入项目中，通过开发时编写注释，自动生成接口文档； 
2. Swagger包含了Swagger Editor，它是使用yaml语言的Swagger API的编辑器，支持导出yaml和json格式的接口文件； 
3. Swagger包含了Swagger UI，它将Swagger Editor编辑好的接口文档以html的形式展示出来； 
4. Swagger支持根据定义的接口导出各种语言的服务端或客户端代码。 
    其中1和4是更加面向开发的内容，开发团队要有自动生成文档的需求，在开发和自测中遵循前后端分离。而2和3是相对可以独立出来的、可供QA人员参考的接口文档管理方案，也是我们主要关注的部分。 
    Swagger提供了Swagger Editor和Swagger  UI的在线demo，如下图。可以看出，Swagger可以完整地定义一个接口的内容，包括各个参数、返回值的具体结构、类型，Swagger  Editor可以实时进行编辑并在线调试。编辑好的API可以导出为json文件，使用Swagger UI打开即可以看到更美观的接口文档。 

 Swagger Editor和SwaggerUI的本地部署十分简单，这两者都可以直接从Github上下载源码，将其部署到本地Tomcat服务器上，然后通过浏览器访问即可。官方还提供了其他几种部署方式，具体步骤在帮助文档中有详细说明，这里不再赘述



## 同类产品

### RAP2

​		RAP的升级版本。

​		RAP是阿里的一套完整的可视化接口管理工具，可以定义接口结构，动态生成模拟数据，校验真实接口正确性。不仅如此，RAP围绕接口定义，提供了一系列包括团队管理、项目管理、文档版本管理、mock插件等服务。 
 有关RAP的使用，RAP官网提供了非常详细的wiki和视频教程。与Swagger需要使用标记语言编写不同，RAP可以完全可视化地定义项目相关信息，定义接口的请求响应等等，学习成本较低。RAP还为后端开发人员提供了校验接口的功能，为前端开发人员提供了mock数据的工具等。

### DOClever

[官网入口](http://doclever.cn)

接口项目管理 在线接口文档,项目版本管理

Restful,Query,Header,Body,Raw信息一应俱全JSON层次采用可视化编辑,结构清晰.
                                    项目版本和接口快照保证你可以实时回溯到任何状态.https,接口加密，文件上传，so easy！
                                    独有的proxy技术加持为您冲破内网的束缚..                                

- 1  接口快照回滚,项目版本控制
- 2  兼容最新版Swagger,PostMan等平台数据
- 3  接口文档自动在线生成
- 4  Restful,Query,Header,Body,Raw信息一应俱全,独有的proxy技术加持为您冲破内网的束缚

视化接口管理工具 ,可以分析接口结构，校验接口正确性， 围绕接口定义文档，通过一系列自动化工具提升我们的协作效率。

主要特性：

　　• 可以对接口信息进行编辑管理，支持 get,post,put,delete,patch 五种方法，支持  https 和 https 协议，并且支持 query，body，json，raw，rest，formdata 的参数可视化编辑。同时对  json 可以进行无限层次可视化编辑。并且，状态码，代码注入，markdown 文档等附加功能应有尽有。

　　• 接口调试运行，可以对参数进行加密，从 md5 到 aes 一应俱全，返回参数与模型实时分析对比，给出不一致的地方，找出接口可能出现的问题。如果你不想手写文档，那么试试接口的数据生成功能，可以对接口运行的数据一键生成文档信息。

　　• mock 的无缝整合，DOClever 自己就是一个 mock 服务器，当你把接口的开发状态设置成已完成，本地 mock 便会自动请求真实接口数据，否则返回事先定义好的 mock 数据。

　　• 支持 postman，rap，swagger 的导入，方便你做无缝迁移，同时也支持 html 文件的导出，方便你离线浏览！

　　• 项目版本和接口快照功能并行，你可以为一个项目定义 1.0，1.1，1.2 版本，并且可以自由的在不同版本间切换回滚，再也不怕接口信息的遗失，同时接口也有快照功能，当你接口开发到一半或者接口需求变更的时候，可以随时查看之前编辑的接口信息。

　　•  自动化测试功能，目前市面上类似平台的接口自动化测试大部分都是伪自动化，对于一个复杂的场景，比如获取验证码，登陆，获取订单列表，获取某个特定订单详情这样一个上下文关联的一系列操作无能为力。而 DOClever 独创的自动化测试功能，只需要你编写极少量的 javascript  代码便可以在网页里完成这样一系列操作，同时，DOClever  还提供了后台定时批量执行测试用例并把结果发送到团队成员邮箱的功能，你可以及时获取接口的运行状态。

　　• 团队协作功能，很多类似的平台这样的功能是收费的，但是 DOClever 觉得好东西需要共享出来，你可以新建一个团队，并且把团队内的成员都拉进来，给他们分组，给他们分配相关的项目以及权限，发布团队公告等等。

### YApi

　YApi 是高效、易用、功能强大的 api  管理平台，旨在为开发、产品、测试人员提供更优雅的接口管理服务。它可以帮助开发者轻松创建、发布、以及维护API。除此之外，YApi  还为用户提供了优秀的交互体验，开发人员只需利用平台提供的接口数据写入工具以及简单的点击操作就可以实现接口的管理。

特性:

- 基于 Json5 和 Mockjs 定义接口返回数据的结构和文档，效率提升多倍
- 扁平化权限设计，即保证了大型企业级项目的管理，又保证了易用性
- 类似 postman 的接口调试
- 自动化测试, 支持对 Response 断言
- MockServer 除支持普通的随机 mock 外，还增加了 Mock 期望功能，根据设置的请求过滤规则，返回期望数据
- 支持 postman, har, swagger 数据导入
- 免费开源，内网部署，信息再也不怕泄露了

### eolinker

开箱即用的API研发管理方案，无需繁琐的配置，支持读取代码注解生成API文档，或者是通过UI界面快速创建全面的API文档。通过Mock API、API变更通知、版本管理等服务，让团队更敏捷。

项目地址：[https://www.eolinker.com](https://link.zhihu.com/?target=https%3A//www.eolinker.com/)

疑问：github上面源码已经被删除，需要下载源码自己搭建的就不要想了

### showdoc

　　一个非常适合IT团队的在线API文档、技术文档工具

### postman