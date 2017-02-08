#### IndependentVideoManager.h中其他属性和方法解读:

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
 *  用于展示sotre或者展示类广告的控制器
 */
@property(nonatomic,assign)UIViewController *rootViewController;

#pragma mark - init 初始化相关方法
/**
 *  使用PublisherID 初始化视频IndependentVideoManager
 *  Create IndependentVideoManager with your own Publisher ID
 *
 *  @param publisherID 媒体ID
 *  @return IndependentVideoManager
 */
- (instancetype)initWithPublisherID:(NSString *)publisherID;

/**
 *  使用PublisherID和应用当前登陆用户的UserID初始化IndependentVideoManager
 *   Create IndependentVideoManager with your own Publisher ID and User ID.
 *
 *  @param publisherID 媒体ID
 *  @param userID      应用中唯一标识用户的ID, 可传入nil
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

#pragma mark - independent video present 视频展现相关方法
/**
 *  使用App的rootViewController来弹出并显示视频。
 *  Present independent video in ModelView way with App's rootViewController.
 */
- (void)presentIndependentVideo;

/**
 *  使用开发者传入的UIViewController来弹出视频。
 *  Present IndependentVideo with developer's controller.
 *
 *  @param controller UIViewController
 *  @param type 积分墙类型
 */
- (void)presentIndependentVideoWithViewController:(UIViewController *)controller;

#pragma mark - independent video status 检查是否有视频
/**
 *  检查是否有视频广告
 *  check independent video available.
 */
- (void)checkVideoAvailable;
```



