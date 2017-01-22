#### 1.获取PublisherID

可前往多盟官网（[http://www.domob.cn](http://www.domob.cn)\), 完善公司\(个人\)开发者信息, 添加App来获取App对应的 Publisher ID。

#### 2.获取视频SDK

方式1：在官网下载SDK包

方式2：[https://github.com/domob-inc/DomobVideoSDK](https://github.com/domob-inc/DomobVideoSDK) github也可下载到SDK

#### 3.集成SDK到项目中

将IndependentVideoSDK文件夹添加至工程项目中，包含内容如下图：

![](/assets/sdk文件.png)

#### 4.添加SDK依赖库

若要SDK正常工作，需要在项目的Build Phase添加相关依赖库，如下图所示：

![](/assets/lib.png)

#### 5.添加-Objc

Build Settings搜索并在other link flags 选项添加-ObjC，如下图：

![](/assets/Obic.png)

#### 6.使用SDK

若要在应用中添加视频广告，只需要简单的几步：

* 导入IndependentVideoManager.h 头文件。

* 声明一个IndependentVideoManager的实例。

* 使用您从多盟官网获得的PublisherId来初始化IndependentVideoManager。

* 设置IndependentVideoManager的代理。

* 调用checkVideoAvailable检查并缓存视频

* 在接收到各个回调里进行逻辑处理

#### 7.代码演示

* ##### 初始化

```
#import "IndependentVideoManager.h"

@interface ViewController: UIViewController <IndependentVideoManagerDelegate> {

    IndependentVideoManager *_manager;

}

@implementation ViewController

 
- (void)viewDidLoad {
    [super viewDidLoad];

    // SDK初始化操作,并设置Delegate对象，一般为self。

    _manager = [[IndependentVideoManager alloc] initWithPublisherID:@"申请的publishId" andUserID:nil];    

    _manager.delegate = self;
    
    // 检查并下载视频
    [_manager checkVideoAvailable];

}

```



* ##### IndependentVideoManagerDelegate回调方法解读：

你可以通过实现IndependentVideoManagerDelegate中定义的方法，来跟踪视频生命周期的各个阶段。所有这些方法也都定义在IndependentVideoManager.h中，具体如下：

```
#pragma mark - video present callback 视频生命周期各个阶段的回调

/**
 * 调用checkVideoAvailable的回调方法
 * check independent video available.
 * available YES/NO 表示当前请求,服务端是否有视频广告数据返回。返回YES,会自动下载当前视频。
 * @param IndependentVideoManager
 * @param available
 */
- (void)ivManager:(IndependentVideoManager *)manager isIndependentVideoAvailable:(BOOL)available;

/**
 *  开始加载视频数据。
 *  Independent video starts to fetch ad info.
 *  @param manager IndependentVideoManager
 */ 
- (void)ivManagerDidStartLoad:(IndependentVideoManager *)manager;

/**
 *  加载是否完成,带参数回调。
 *  Fetching independent video .
 *  isFinished YES/NO 代表下载是否完成
 *  @param manager IndependentVideoManager
 */
- (void)ivManagerDidFinishLoad:(IndependentVideoManager *)manager finished:(BOOL)isFinished;

/**
 *  加载失败。可能的原因由error部分提供，例如网络连接失败、被禁用等。
 *  Failed to load independent video.
 *  @param manager IndependentVideoManager
 *  @param error   error
 */
- (void)ivManager:(IndependentVideoManager *)manager failedLoadWithError:(NSError *)error;

/**
 *  视频界面被呈现出来时，回调该方法。
 *  Called when independent video will be presented.
 *  @param manager IndependentVideoManager
 */
- (void)ivManagerWillPresent:(IndependentVideoManager *)manager;

/**
 *  页面关闭。
 *  Independent video closed.
 *  @param manager IndependentVideoManager
 */
- (void)ivManagerDidClosed:(IndependentVideoManager *)manager;

/**
 *  当视频播放完成后，回调该方法。
 *  Independent video complete play
 *  @param manager IndependentVideoManager
 */
- (void)ivManagerCompletePlayVideo:(IndependentVideoManager *)manager;

/**
 *  播放视频失败
 *  Paly independent video failed.
 *  @param manager IndependentVideoManager
 *  @param error
 */
- (void)ivManagerPlayIndependentVideo:(IndependentVideoManager *)manager withError:(NSError *)error;

```

* ##### IndependentVideoManager.h中其他属性和方法解读:

```

/**
 *  SDK的代理对象,一般为self
 */
@property(nonatomic,weak)id<IndependentVideoManagerDelegate>delegate;

/**
 *  是否禁用StoreKit库提供的应用内打开store页面的功能，采用跳出应用打开OS内置AppStore。
 *  默认为NO，即使用StoreKit。
 */
@property (nonatomic, assign) BOOL disableStoreKit;

/**
 *  是否禁用在播放完成后弹出弹框，默认为NO，即不禁用，则弹出
 */
@property (nonatomic, assign) BOOL disableShowAlert;

/**
 *  4G网络是否直接下载，默认YES，为下载(表示不弹出提示)
 */
@property (nonatomic, assign) BOOL should4GDownload;

/**
 *非激励视频(unreawding) 开发者配置的视频展现时间
 */
@property (nonatomic, assign) NSTimeInterval unrewardingTime;
/**
 *  用于展示sotre或者展示类广告
 */
@property(nonatomic,unsafe_unretained)UIViewController *rootViewController;

#pragma mark - init 初始化相关方法
/**
 *  使用Publisher ID初始化积分墙
 *  Create IndependentVideoManager with your own Publisher ID
 *
 *  @param publisherID 媒体ID
 *
 *  @return IndependentVideoManager
 */
- (instancetype)initWithPublisherID:(NSString *)publisherID;
/**
 *  使用Publisher ID和应用当前登陆用户的User ID初始化IndependentVideoManager
 *   Create IndependentVideoManager with your own Publisher ID and User ID.
 *
 *  @param publisherID 媒体ID
 *  @param userID      应用中唯一标识用户的ID
 *
 *  @return IndependentVideoManager
 */
- (instancetype)initWithPublisherID:(NSString *)publisherID andUserID:(NSString *)userID;

/**
 *  更新登陆用户的User ID
 *  Update User ID.
 *
 *  @param userID      应用中唯一标识用户的ID
 */
- (void)updateUserID:(NSString *)userID;

#pragma mark - independent video present 积分墙展现相关方法
/**
 *  使用App的rootViewController来弹出并显示列表积分墙。
 *  Present independent video in ModelView way with App's rootViewController.
 *
 *  @param type 积分墙类型
 */
- (void)presentIndependentVideo;
/**
 *  使用开发者传入的UIViewController来弹出显示积分墙。
 *  Present IndependentVideo with developer's controller.
 *
 *  @param controller UIViewController
 *  @param type 积分墙类型
 */
- (void)presentIndependentVideoWithViewController:(UIViewController *)controller;

#pragma mark - independent video status 检查视频积分墙是否可用
/**
 *  是否有视频广告可以播放
 *  check independent video available.
 */
- (void)checkVideoAvailable;
```





