## 代码结构

Pragma marks 是给方法分组很好的方法，特别是在 view controller 中,下面是一个在 view controller 中常见的结构
```objc
#import "SomeModel.h"
#import "SomeView.h"
#import "SomeController.h"
#import "SomeStore.h"
#import "SomeHelper.h"
#import <SomeExternalLibrary/SomeExternalLibraryHeader.h>

#pragma mark - 静态成员的定义
static NSString * const XYZFooStringConstant = @"FoobarConstant";
static CGFloat const XYZFooFloatConstant = 1234.5;

@interface XYZFooViewController () <XYZBarDelegate>
#pragma mark - 私有成员的定义
@property (nonatomic, copy, readonly) Foo *foo;
@end

@implementation XYZFooViewController

#pragma mark - Lifecycle ViewController生命周期
- (instancetype)initWithFoo:(Foo *)foo;
- (void)dealloc;

#pragma mark - View Lifecycle view 页面 生命周期 
- (void)viewDidLoad;
- (void)viewWillAppear:(BOOL)animated;

#pragma mark - Layout UI准备和布局相关
- (void)prepareUI;
- (void)makeViewConstraints;

#pragma mark - Public Interface 公共接口
- (void)startFooing;
- (void)stopFooing;

#pragma mark - User Interaction 用户交互 按钮事件相关
- (void)foobarButtonTapped;

#pragma mark - XYZFoobarDelegate Delegate代理 
- (void)foobar:(Foobar *)foobar didSomethingWithFoo:(Foo *)foo;

#pragma mark - Internal Helpers 内部公用方法
- (NSString *)displayNameForFoo:(Foo *)foo;

#pragma mark - Lazy loading UI控件懒加载
- (void)prepareUI;
- (void)makeViewConstraints;

@end

```
