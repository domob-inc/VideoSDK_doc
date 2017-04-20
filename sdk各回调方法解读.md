#### IndependentVideoManagerDelegate回调方法解读：

可以通过实现IndependentVideoManagerDelegate中定义的方法，来跟踪视频生命周期的各个阶段。所有这些方法也都定义在IndependentVideoManager.h中，**所有代理方法都是可选实现的, ** 具体如下：

```
#pragma mark - video present callback 视频生命周期各个阶段的回调(八个回调方法)

/**
 *  回调一:
 *  调用checkVideoAvailable的回调方法
 *  check independent video available.
 *  available YES/NO 表示当前请求,服务端是否有视频广告数据返回。返回YES,会自动下载当前视频。
 *  @param IndependentVideoManager
 *  @param available
 */
- (void)ivManager:(IndependentVideoManager *)manager isIndependentVideoAvailable:(BOOL)available;

/**
 *  回调二:
 *  开始加载视频数据。
 *  Independent video starts to fetch ad info.
 *  @param manager IndependentVideoManager
 */ 
- (void)ivManagerDidStartLoad:(IndependentVideoManager *)manager;

/**
 *  回调三:
 *  加载是否完成,带参数回调。
 *  Fetching independent video .
 *  isFinished YES/NO 代表下载是否完成
 *  @param manager IndependentVideoManager
 */
- (void)ivManagerDidFinishLoad:(IndependentVideoManager *)manager finished:(BOOL)isFinished;

/**
 *  回调四:
 *  加载失败。可能的原因由error部分提供，例如网络连接失败、被禁用等。
 *  Failed to load independent video.
 *  @param manager IndependentVideoManager
 *  @param error   error
 */
- (void)ivManager:(IndependentVideoManager *)manager failedLoadWithError:(NSError *)error;

/**
 *  回调五:
 *  视频界面被呈现出来时，回调该方法。
 *  Called when independent video will be presented.
 *  @param manager IndependentVideoManager
 */
- (void)ivManagerWillPresent:(IndependentVideoManager *)manager;

/**
 *  回调六:
 *  页面关闭。
 *  Independent video closed.
 *  @param manager IndependentVideoManager
 */
- (void)ivManagerDidClosed:(IndependentVideoManager *)manager;

/**
 *  回调七:
 *  当视频播放完成后，回调该方法。
 *  Independent video complete play
 *  @param manager IndependentVideoManager
 */
- (void)ivManagerCompletePlayVideo:(IndependentVideoManager *)manager;

/**
 *  回调八:
 *  播放视频失败
 *  Paly independent video failed.
 *  @param manager IndependentVideoManager
 *  @param error
 */
- (void)ivManagerPlayIndependentVideo:(IndependentVideoManager *)manager withError:(NSError *)error;
```



