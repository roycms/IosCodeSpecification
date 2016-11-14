## 代码结构

Pragma marks 是给方法分组很好的方法，特别是在 view controller 中。下面是一个在 view controller 中常见的结构
```objc
#import "SomeModel.h"
#import "SomeView.h"
#import "SomeController.h"
#import "SomeStore.h"
#import "SomeHelper.h"
#import <SomeExternalLibrary/SomeExternalLibraryHeader.h>

static NSString * const XYZFooStringConstant = @"FoobarConstant";
static CGFloat const XYZFooFloatConstant = 1234.5;

@interface XYZFooViewController () <XYZBarDelegate>

@property (nonatomic, copy, readonly) Foo *foo;

@end

@implementation XYZFooViewController

#pragma mark - Lifecycle

- (instancetype)initWithFoo:(Foo *)foo;
- (void)dealloc;
#pragma mark - View Lifecycle

- (void)viewDidLoad;
- (void)viewWillAppear:(BOOL)animated;

#pragma mark - Layout

- (void)makeViewConstraints;

#pragma mark - Public Interface

- (void)startFooing;
- (void)stopFooing;

#pragma mark - User Interaction

- (void)foobarButtonTapped;
#pragma mark - XYZFoobarDelegate

- (void)foobar:(Foobar *)foobar didSomethingWithFoo:(Foo *)foo;

#pragma mark - Internal Helpers

- (NSString *)displayNameForFoo:(Foo *)foo;

@end

```
