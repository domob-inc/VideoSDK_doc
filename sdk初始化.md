#### 初始化

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



