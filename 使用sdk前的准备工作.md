#### 1.获取PublisherID

可前往多盟官网（[http://www.domob.cn](http://www.domob.cn)\), 完善公司\(个人\)开发者信息, 添加App来获取App对应的 Publisher ID。

#### 2.获取视频SDK

方式：[https://github.com/domob-inc/DomobVideoSDK](https://github.com/domob-inc/DomobVideoSDK) github下载到SDK

#### 3.集成SDK到项目中

将IndependentVideoSDK文件夹添加至工程项目中，包含内容如下图：

![](/assets/sdk文件.png)

#### 4.添加SDK依赖库

若要SDK正常工作，需要在项目的Build Phase添加相关依赖库，如下图所示：

![](/assets/lib.png)

#### 5.添加-ObjC

Build Settings搜索并在other link flags 选项添加-ObjC，如下图：

![](/assets/Obic.png)

