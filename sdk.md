#### 最快速集成方式

* * 1.初始化后调用checkVideoAvailable方法，检查并下载视频

```
// 初始化
_videoManager = [[IndependentVideoManager alloc] initWithPublisherID:self.publisherID andUserID:self.userID];
// 设置代理对象
_videoManager.delegate = self;
// 调用check方法
[_videoManager checkVideoAvailable];
```

* * 2.如果当前有视频，则调用**回调三**方法，isFinished有YES/NO值

```
// 有视频时,加载是否完成。
- (void)ivManagerDidFinishLoad:(IndependentVideoManager *)manager finished:(BOOL)isFinished {
    if (isFinished) {
        // 表示视频已缓存完毕, 可以显示播放按钮
        _playButton.hidden = NO;
    }
}
```

**注意:播放前先调用checkVideoAvailable方法,检查视频状态；在isFinished=YES时再进行播放。**

