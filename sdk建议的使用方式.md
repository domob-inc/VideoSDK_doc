#### 建议的使用方式

* * 1.初始化后调用checkVideoAvailable方法，检查并下载视频

```
// 初始化
_videoManager = [[IndependentVideoManager alloc] initWithPublisherID:self.publisherID andUserID:self.userID];
// 设置代理对象
_videoManager.delegate = self;
// 调用check方法
[_videoManager checkVideoAvailable];
```

* * 2.check之后，**回调一**方法中的available会返回YES/NO值

```
// 检查是否有视频
- (void)ivManager:(IndependentVideoManager *)manager isIndependentVideoAvailable:(BOOL)available {
    if (available) {
        _label.text = [NSString stringWithFormat:@"有视频"];
        NSLog(@"有视频");
    }
    else {
        _label.text = [NSString stringWithFormat:@"无视频"];
        NSLog(@"无视频");
    }
}
```

* * 3.如果available为YES，后台会加载当前视频，调用**回调三**方法，isFinished有YES/NO值

```
// 有视频时,加载是否完成。
- (void)ivManagerDidFinishLoad:(IndependentVideoManager *)manager finished:(BOOL)isFinished {
    NSLog(@"<demo>VideoDidFinishLoad--%d",isFinished);
    if (isFinished) {
        //加载完让播放按钮可用
        _playButton.enabled = YES;
        NSLog(@"视频下载完成");
    }else {
        _playButton.enabled = NO;
        NSLog(@"视频正在下载");
    }
}
```

* * 4.视频播放完成会调用**回调七**方法，可用于开发者统计播放完成和给用户发放奖励的依据

```
// 视频播放完成
- (void)ivManagerCompletePlayVideo:(IndependentVideoManager *)manager {

    NSLog(@"<demo>VideoCompletePlay");
    // 统计播放完成和发放奖励
}
```

* * 5.落地页关闭时，会执行**回调六**方法，可调用checkVideoAvailable检查到下一条视频的状态，形成一个闭环操作

```
//  视频落地页面关闭。
- (void)ivManagerDidClosed:(IndependentVideoManager *)manager {

    NSLog(@"<demo>VideoDidClosed");
    // 检查下一条视频状态
    [manager checkVideoAvailable];
}
```



