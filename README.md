iOS 最佳实践
==================
[点这里查看英文版]

_这份文档就像软件项目一样，如果我们不维护它就会逐渐腐坏。欢迎大家跟我们一起来维护它——只需提交 issue 或者发 pull request 即可！_


对其他移动平台感兴趣？也许我们的[《Android 开发最佳实践》][android-best-practices]以及[《Windows App 开发 最佳实践》][windows-app-development-best-practices]能满足你。

[android-best-practices]: https://github.com/futurice/android-best-practices
[windows-app-development-best-practices]: https://github.com/futurice/windows-app-development-best-practices
[点这里查看英文版]:https://github.com/futurice/ios-good-practices


## 为什么要写这篇文档？


iOS 开发在上手时可能会有些令人生畏。无论是 Objective-C 还是 Swift 在别处都没有广泛的应用，iOS 这个平台几乎对一切都有一套不同的叫法，而尝试把你的代码跑在真机上的过程难免磕磕碰碰。这份持续更新的文档就是来帮你的，无论你是 Cocoa 王国的新手，或是想知道“最佳做法”是什么，都可以一读。下文仅供参考，如果你有理由采取不同的做法，不用顾虑，只管做吧！

## 上手

### Xcode



[Xcode][xcode] 是绝大部分 iOS 开发者选择的 IDE，也是唯一一个苹果官方支持的 IDE。也有一些其他选择，最著名的可能要数 [AppCode][appcode] 了。但除非你已经对 iOS 游刃有余，否则还是用 Xcode 吧。尽管 Xcode 有一些缺点，它现在还算是相当实用的！

要安装 Xcode，只需在 [Mac 的 App Score][xcode-app-store] 上下载即可。它自带最新版的 SDK 和 iOS 模拟器，其他版本可以在 _Preferences > Downloads_ 处安装。

[xcode]: https://developer.apple.com/xcode/
[appcode]: https://www.jetbrains.com/objc/
[xcode-app-store]: https://itunes.apple.com/us/app/xcode/id497799835
### 建立工程

开始一个新的 iOS 项目时，一个常见的问题是：用代码写界面还是用 Storyboard、xib 画界面。在现有的应用里，这两种做法都占有一席之地。你需要考虑以下几点：

#### 用代码写界面有哪些好处？

* Storyboard 的 XML 结构很复杂，所以如果用 Storyboard ，合并代码时很容易冲突，比起用代码写的界面要麻烦许多。
* It's easier to structure and reuse views in code, thereby keeping your codebase [DRY][dry].
* 用代码写界面时，构建和重用 view 更加方便，因此能保持你的 codebase 遵循[DRY 原则][dry]。
* 所有的信息都集中在一处。如果用 Interface Builder，你还得到处点开各种检查器，才能找到你要设置的属性。

[dry]: http://en.wikipedia.org/wiki/Don%27t_repeat_yourself

#### 用 Storyboard 画界面有哪些好处？
* 对技术不太熟悉的人也可以画 Storyboard，调整颜色、layout 约束，为项目做出直接贡献。不过，要做这些需要工程已经建好，并且也要了解一些基本知识。
* 开发迭代会更快，因为不需要 build 工程就能预览到做出的改动。
* 在 Xcode 6 中，在 Storyboard 里终于能看到自定义的字体和 UI 控件样式了。这让你在设计时能更好地了解界面的最终外观。
* 从 iOS 8 开始，你可以用 Size Classes 来设计同时支持各种屏幕尺寸的界面，省去了很多重复工作。
***
* For the less technically inclined, Storyboards can be a great way to contribute to the project directly, e.g. by tweaking colors or layout constraints. However, this requires a working project setup and some time to learn the basics.
* Iteration is often faster since you can preview certain changes without building the project.

 * In Xcode 6, custom fonts and UI elements are finally represented visually in Storyboards, giving you a much better idea of the final appearance while designing.
 * Starting with iOS 8, [Size Classes][size-classes] allow you to design for different device types and screens without duplication.



[size-classes]: http://blog.futurice.com/adaptive-view-ios8




### gitignore 文件


要为一个项目添加版本控制，最好第一步就弄一个恰当的`.gitignore`文件。这样一来，不需要的文件（例如用户设置、临时文件等等）就不会进入 repository 了。幸运的是，Github 帮我们同时准备好了 [Objective-C 版][objc-gitignore] 和 [Swift 版][swift-gitignore]。

[objc-gitignore]: https://github.com/github/gitignore/blob/master/Objective-C.gitignore
[swift-gitignore]: https://github.com/github/gitignore/blob/master/Swift.gitignore

### CocoaPods

如果你准备在工程里引入外部依赖（例如第三方库），[CocoaPods][cocoapods]提供了快速而便捷的集成方法。安装方法如下：

    sudo gem install cocoapods

To get started, move inside your iOS project folder and run

开始的第一步是进入你的工程目录，然后运行

    pod init

这样会创建一个 Podfile，在这里集中管理所有的依赖。把你的依赖添加到 Profile 里，然后运行

    pod install

来安装这些库，并且把它们和你自己的工程一起放进一个 workspace 里。在 commit 的时候，一般[推荐把依赖在你的 repo 里安装好之后再 commit][committing-pods]，最好不要让每个开发者 checkout 之后还要自己跑一遍`pod install`。

要注意，从此以后，打开工程的时候就要打开`.xcworkspace`文件了，不要再打开`.xcproject`，否则代码编译不通过。下面这条命令

    pod update

会把所有的 pod 都更新到 Podfile 允许的最新版本。你可以使用一系列的[符号][cocoapods-pod-syntax]来准确指定你对版本的要求。

[cocoapods]: http://www.cocoapods.org
[cocoapods-pod-syntax]: http://guides.cocoapods.org/syntax/podfile.html#pod
[committing-pods]: https://www.dzombak.com/blog/2014/03/including-pods-in-source-control.html

### 工程结构

既然把这些数以百计的源文件都保存在同一目录下，根据工程结构来建立一个目录结构是个好主意。例如，你可以使用以下的结构：

    ├─ Models
    ├─ Views
    ├─ Controllers
    ├─ Stores
    ├─ Helpers

首先，在 Xcode 的 Project Navigator（左边栏）里，把这些目录建立为 group（小小的黄色“文件夹”），建在与工程的同名的 group 下。然后，把每一个 group 与工程路径下实际的文件夹链接起来，方法是选中 group，打开右边栏的 File Inspector，点击小小的灰色文件夹 icon，然后在工程目录下创建一个新的子文件夹，名称与 group 相同。

#### 本地化

从最开始就要把所有的文案放在本地化文件里。这不仅有利于翻译，也能让你更快地找到面向用户的文字。你可以在 build scheme 里添加一个 launch 参数，指定在某种语言下启动 app，例如：

    -AppleLanguages (Finnish)

对于更复杂的翻译，比如与名词的数量有关的复数形式（如 "1 person" 对应 "3 people"），你应该使用[`.stringsdict` 格式][stringsdict-format]来替换普通的`localizable.strings`文件。只要你能习惯这种奇特的语法，你就拥有了一个强大的工具，可以根据需要（[例如俄语或阿拉伯语的规则][language-plural-rules]）把名词变为“一个”、“一些”、“少数”和“许多”等复数形式。

更多关于本地化的信息，请参考 2012 年 2 月 HelsinkiOS 大会上的[这些幻灯片][l10n-slides]。其中的大部分演讲至少到 2014 年 10 月为止仍然是不过时的。

[stringsdict-format]: https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/StringsdictFileFormat/StringsdictFileFormat.html
[language-plural-rules]: http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html
[l10n-slides]: https://speakerdeck.com/hasseg/localization-practicum

#### 常量

把整个 app 范围的常量定义在一个`Constants.h`文件里，然后在 prefix header 里加入这个文件。

相比使用 `#define` 定义的预处理宏，使用真正的常量更好：

    static CGFloat const XYZBrandingFontSizeSmall = 12.0f;
    static NSString * const XYZAwesomenessDeliveredNotificationName = @"foo";

真正的常量是类型安全的，拥有更明确的作用域，不能在后续的代码中重新定义也不能取消定义，并且在 debugger 中可用。

### 分支策略

App 发布的时候把 release 代码从原有的分支上隔离出来，并且加上适当的 tag，是很好的做法，对于向公众分发（比如通过 App Store）的 app 这一点尤其重要。同时，涉及到大量 commit 的 feature 应该在独立的分支上完成。[`git-flow`][gitflow-github]是一个帮助你遵守这些原则的工具。它只是在 Git 的分支和 tag 命令上简单加了一层包装，就可以帮助维护一套适当的分支结构，对于团队协作尤为有用。所有的开发都应该在 feature 对应的分支上完成（小改动在`develop`分支上），给 release 打上 app 版本的 tag，然后 commit 到 master 分支时只能用下面这条命令：

    git flow release finish <version>

[gitflow-github]: https://github.com/nvie/gitflow

## Common Libraries

## 常用的库

一般来说，在工程里添加外部依赖要谨慎。当然，眼下某个第三方库能漂亮地解决你的问题，但或许不久之后就陷入了维护的泥淖，最后随着下一版 OS 的发布全线崩溃。另一种情况是，原先只能通过引用外部库来实现的 feature，突然官方 API 也支持了。在设计良好的项目里，把第三方库替换为官方的实现花不了多少功夫，但在将来会大有裨益。永远要优先考虑用苹果官方的框架（也是最好的框架）来解决问题！

因此，这一章有意写得比较简短。下面介绍的第三方库主要用来减少模板代码（例如 Auto Layout）或者用来解决复杂的、需要大量测试的问题，例如计算日期。随着你对 iOS 越来越精通，务必要四处看看它们的源码，熟悉它们所使用的底层框架。你会发现做好这些就能减轻许多重担了。

### AFNetworking


大约 99.95% 的 iOS 开发者都使用这个网络库。尽管`NSURLSession`已经非常强大了，但一旦涉及到实际管理请求队列时，`AFNetworking`仍然立于不败之地，而现代的 app 基本都会有这个需求。

### DateTools


一条常识是，[不要自己写日期计算][timezones-youtube]。幸运的是，有 DateTools 这样一个基于 MIT 协议、充分测试过的第三方库，基本能满足所有日期方面的要求。

[timezones-youtube]: https://www.youtube.com/watch?v=-5wpm-gesOY

### Auto Layout 相关的库

如果你习惯用代码写 view，你很可能用过这两种诡异的语法——常规的`NSLayoutConstraint`工厂，以及所谓的[可视化语言][visual-format-language]。前者极其冗长，而后者是基于字符串的，完全躲过了编译检查。

而 [Masonry][masonry-github] 的解决方法是：引入自己定义的 DSL 来创建、更新和替换约束。Swift 有一个类似的库 [Cartography][cartography-github]，是建立在这门语言强大的运算符重载基础上的。保守一些的库有 [FLKAutoLayout][flkautolayout-github]，它对原生 API 加了一层整洁而不奇异的包装。

[visual-format-language]: https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/VisualFormatLanguage/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH3-SW1
[masonry-github]: https://www.github.com/Masonry/Masonry
[cartography-github]: https://github.com/robb/Cartography
[flkautolayout-github]: https://github.com/floriankugler/FLKAutoLayout

## 架构

* [Model-View-Controller-Store (MVCS)][mvcs]
    * 这是苹果默认的架构(MVC)上增加了一个 Store 层，用来吐出 Model，处理网络请求、缓存等。
  
    * 每个 Store 暴露给 view controller 的或者是`RACSignal`，或者是返回值为`void`、参数带有自定义的 completion block 的方法。
* [Model-View-ViewModel (MVVM)][mvvm]
    * MVVM 是为了解决“巨大的 view controller”而生，它把`UIViewController`的子类看做 View 层的一部分，用 ViewModel 维护所有的状态来给 ViewController 瘦身。
   
    * 对于 Cocoa 开发者是一个很新的概念，但是[正在引起][cocoasamurai-rac] [越来越多的关注][raywenderlich-mvvm]
* [View-Interactor-Presenter-Entity-Routing (VIPER)][viper]
    * 相当特别的架构，大型项目可能值得参考，尤其是即使用 MVVM 还是比较凌乱，以及对需要重点考虑可测试性的情况。

[mvcs]: http://programmers.stackexchange.com/questions/184396/mvcs-model-view-controller-store
[mvvm]: http://www.objc.io/issue-13/mvvm.html
[cocoasamurai-rac]: http://cocoasamurai.blogspot.de/2013/03/basic-mvvm-with-reactivecocoa.html
[raywenderlich-mvvm]: http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1
[viper]: http://www.objc.io/issue-13/viper.html

### "通知" 模型

以下是组件之间互发通知的一些常见手段：
* __Delegation:__ _(一对一)_ 苹果官方经常用这个模式（有些人认为用得太泛滥了）。主要用于回传，比如从模态框回传数据。
* __Callback blocks:__ _(一对一)_ 耦合更松，同时能让相关联的代码在一起。并且，消息发出者数量很多时比 delegation 更方便。
* __Notification Center:__ _(一对多)_ 可能是一个对象给多个观察者发出“通知”时最常用的方法。耦合非常松，甚至可以把通知发到全局，不需要对调度者的引用。
* __Key-Value Observing (KVO):__ _(一对多)_ 不需要被观测的对象主动“发出通知”，只需要被观测的键（属性）支持 _Key-Value Coding (KVC)_ 。这种模式比较含混，而且标准 API 比较繁复，所以一般不推荐使用。
* __Signals:__ _(一对多)_ 这是[ReactiveCocoa][reactivecocoa-github]的核心，它允许结合关键内容的链式调用，用这种方法逃离[回调深渊（嵌套过多的回调）][elm-escape-from-callback-hell]。

[elm-escape-from-callback-hell]: http://elm-lang.org/learn/Escape-from-Callback-Hell.elm

### Models

要确保你的 model 是不可变的，它们用来把远程 API 的语义和类型转换为 app 适用的语义和类型。Github 的 [Mantle](https://github.com/Mantle/Mantle) 是个不错的选择。

### Views

使用 Auto Layout 布局时，要记得在 View 类里加上：

    + (BOOL)requiresConstraintBasedLayout
    {
        return YES;
    }

不然，系统可能不会如期调用`-updateConstraints`，而导致奇怪的 bug。

### Controllers

要使用依赖注入，也就是说，应该把 Controller 需要的数据用参数传进来，而不要把所有状态信息都保存在单例里。后者仅当这些状态 _的确_ 是全局的情况下才适用。

```
+ [[FooDetailsViewController alloc] initWithFoo:(Foo *)foo];
```

## 网络请求

### 传统方法：使用自定义回调 block

```
// GigStore.h

typedef void (^FetchGigsBlock)(NSArray *gigs, NSError *error);

- (void)fetchGigsForArtist:(Artist *)artist completion:(FetchGigsBlock)completion


// GigsViewController.m

[[GigStore sharedStore] fetchGigsForArtist:artist completion:^(NSArray *gigs, NSError *error) {
    if (!error) {
        // Do something with gigs
    }
    else {
        // :(
    }
];
```

这样虽可行，但是如果要发起几个链式请求，很容易导致回调深渊。


### Reactive 的方法：使用 RAC signal


如果你身陷回调深渊，可以看看[ReactiveCocoa (RAC)][reactivecocoa-github]。这是一个多功能、多用途的库，它可以改变[整个 app ][groceryList-github] 的写法。但你也可以仅在适合用它的时候，零散地用一用。


[Teehan+Lax][teehan-lax-rac]以及[NSHipster][nshipster-rac]很好地介绍了 RAC 概念（以及整个 FRP 的概念）。

[grocerylist-github]: https://github.com/jspahrsummers/GroceryList
[teehan-lax-rac]: http://www.teehanlax.com/blog/getting-started-with-reactivecocoa/
[nshipster-rac]: http://nshipster.com/reactivecocoa/

```
// GigStore.h

- (RACSignal *)gigsForArtist:(Artist *)artist;


// GigsViewController.m

[[GigStore sharedStore] gigsForArtist:artist]
    subscribeNext:^(NSArray *gigs) {
        // Do something with gigs
    } error:^(NSError *error) {
        // :(
    }
];
```

在这里我们可以把 gig 信号与其他信号结合，因此可以在展示 gig 之前做一些修改、过滤等处理。

## Assets

使用 [Asset catalogs][asset-catalogs] 是管理工程中视觉素材的最好方法。这里既可以添加 iPhone 和 iPad 共用的素材，也可以添加针对特定设备（4寸屏 iPhone，iPhone Retina，iPad 等等）的素材，并且会根据名称来自动提供恰当的素材。教会你的设计师（们）怎么在这里添加并 commit 素材，可以帮你节省许多时间，再也不用把素材从邮件或者别的什么渠道导进代码库里了。同时，这样做也可以让他们即刻看到自己的改动，可以根据需要进行迭代。

[asset-catalogs]: https://developer.apple.com/library/ios/recipes/xcode_help-image_catalog-1.0/Recipe.html


### 使用位图

Asset catalog 只会暴露出一套图片的名字，省略了每张图片实际的文件名。这样，类似`button_large@2x.png`这类文件的命名空间仅限于 asset 内部，很好地避免了 asset 的命名冲突。然而，命名 asset 时遵循一些原则可以让生活更轻松：

```
IconCheckmarkHighlighted.png // Universal, non-Retina
IconCheckmarkHighlighted@2x.png // Universal, Retina
IconCheckmarkHighlighted~iphone.png // iPhone, non-Retina
IconCheckmarkHighlighted@2x~iphone.png // iPhone, Retina
IconCheckmarkHighlighted-568h@2x~iphone.png // iPhone, Retina, 4-inch
IconCheckmarkHighlighted~ipad.png // iPad, non-Retina
IconCheckmarkHighlighted@2x~ipad.png // iPad, Retina
```


其中的`-568h`、`@2x`、`~iphone`以及`~ipad`这些标示符本身不是必需的，但是如果在文件名里加上它们，把文件拖动到 asset 时就能自动落到正确的“格子”上，因此能避免难以察觉的错误拖放。
### 使用矢量图

你可以把设计师设计的原始的[矢量图 (PDFs)][vector-assets]放进 asset catalog，让 Xcode 来自动生成位图。这样能减少工程的复杂度（减少文件个数）。

[vector-assets]: http://martiancraft.com/blog/2014/09/vector-images-xcode6/

## 编码风格

### 命名


Apple 非常注意在 API 中保持命名一致性，有时候有点过于冗长了。做 Cocoa 开发时要遵循[Apple的命名规范][cocoa-coding-guidelines]，这样能让加入项目的新人轻松许多。

[cocoa-coding-guidelines]: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html

以下是几条看了就能用上的基本规则：

以 _动词_ 开头的方法表示它执行的操作会造成一些影响，但是不返回任何值。

`- (void)loadView;`
`- (void)startAnimating;`


相反的是，以 _名词_ 开头的方法返回一个对象，但不会造成额外的影响。

`- (UINavigationItem *)navigationItem;`
`+ (UILabel *)labelWithText:(NSString *)text;`


尽可能地区分这两种方法会有很多好处，也就是说，如果一个方法是处理数据的，就不要让它造成额外的影响，反过来也一样。这样可以让造成影响的代码块保持紧凑，因此可以帮助理解代码，并且有利于 debug。


### 代码结构

[Pragma marks](http://nshipster.com/pragma/)是给方法分组很好的方法，特别是在 view controller 中。下面是一个在 view controller 中常见的结构：

```

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


最重要的是要让这些分块标记在工程里所有的类里保持一致。

### External Style Guides

### 其他风格指南

Futurice（作者所在的公司）并没有公司范围的编码风格指南。不过，仔细研究一下其他开发社区的 Objective-C 风格指南会非常有用，尽管有些部分可能是只对特定公司有效或者比较主观的。

* [GitHub](https://github.com/github/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [The New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Ray Wenderlich](https://github.com/raywenderlich/objective-c-style-guide)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)

## 诊断

### 编译警告

建议你尽量把编译警告都打开，并且像对待 error 一样对待 warning。[这份幻灯片][warnings-slides] 论证了这一点。幻灯片里同时还讲了如何在特定文件里或者特定的代码段里忽略特定的 warning。


一句话，在 build setting 的 _“Other Warning Flags”_ 里至少要加入以下两个值：

- `-Wall` _（开启非常多额外的 warning）_
- `-Wextra` _（开启许多额外的 warning）_


同时打开 build setting 里的 _“Treat warnings as errors”_ 。

[warnings-slides]: https://speakerdeck.com/hasseg/the-compiler-is-your-friend


### Clang 静态分析器

Clang 编译器（也就是 XCode 使用的编译器）有一个 _静态分析器(static analyer)_ ，用来执行代码控制流和数据流的分析，可以发现许多编译器检查不出的问题。


你可以在 Xcode 的 _Product → Analyze_ 里手动运行分析器。

分析器可以运行“shallow”和“deep”两种模式。后者要慢得多，但是有跨方法的控制流分析以及数据流分析，因此能发现更多问题。


建议：


- 开启分析器的 _全部_ 检查（方法是在 build setting 的“Static Analyzer”部分开启所有选项）


- 在 build setting 里，对 release 的 build 配置开启 _“Analyze during ‘Build’”_ 。（真的，一定要这样做——你不会记得手动跑分析器的。）


- 把 build setting 里的 _“Mode of Analysis for ‘Analyze’”_ 设为 _Shallow (faster)_


- 把 build setting 里的 _“Mode of Analysis for ‘Build’”_ 设为 _Deep_


### [Faux Pas](http://fauxpasapp.com/)


由我们的员工 [Ali Rantakari][ali-rantakari-twitter] 创作的 Faux Pas 是一个出色的静态 error 检测工具。它能分析你的代码库，找出你全然不知的错误。在发布任何 iOS（或 Mac）app 之前务必要运行它一次！

_(Note: all Futurice employees get a free license to this — just ask Ali.)_

_(注意：所有 Futurice 的员工都能得到一份免费的许可——只要问 Ali 要就行了。)_

[ali-rantakari-twitter]: https://twitter.com/AliRantakari

### Debugging


当 app crash 的时候，默认情况下 Xcode 并不会进入 debugger。要想进入 debugger，添加一个 Exception Breakpoint（点击 Xcode 的 Debug Navigator 底部的“+”号），遇到 exception 的时候就会暂停执行。在大部分情况下，你都能看到导致 exception 的那行代码。这种方法会捕捉到任何 exception，包括已经做了处理的 exception。如果 Xcode 常常会停在正常的 exception（比如第三方库里的）上，选择 _Edit Breakpoint_ 然后在 _Exception_ 下拉框选择 _Objective-C_ 可以减轻这种情况。

在 view 的 debug 方面，[Reveal][reveal] 和 [Spark Inspector][spark-inspector] 是两个强大的可视化检查器，可以节约你大量的时间，尤其是用 Auto Layout 时想知道消失的视图去哪儿了的情况。Xcode 也免费提供了[一个类似的东西][xcode-view-debugging]，不过只支持 iOS 8+，并且略有些不够完善。

[reveal]: http://revealapp.com/
[spark-inspector]: http://sparkinspector.com
[xcode-view-debugging]: https://developer.apple.com/library/ios/recipes/xcode_help-debugger/using_view_debugger/using_view_debugger.html

### 评估


Xcode 自带一套评估工具，叫做 Instruments。它包含众多的评估内存使用、CPU、网络连接、图像等方面的工具。它本身是个庞然大物，但一个比较简单直接的用途是用 Allocations instrument 来检测内存泄露。只需在 Xcode 中选择 _Product_ > _Profile_ ，选择 Allocations instrument，点击 Record 按钮，然后从 Allocation Summary 中过滤出一些有用的字符串，比如 app 里你自己写的类的类名前缀。在 Persistant 一栏中的计数显示了每个对象有多少个实例。如果某个类的实例个数一直胡乱增长，就说明有内存泄露。


另外值得注意的是 Instrument 有一个 Automation 工具，用来把 UI 交互录制为 JavaScript 文件并且重放。[UI Auto Monkey][ui-auto-monkey] 是一个脚本，它借助 Automation 在你的 app 上随机点击、清扫、旋转，对压力测试/浸泡测试可能会有帮助。

[ui-auto-monkey]: https://github.com/jonathanpenn/ui-auto-monkey

## 统计


强烈推荐在你的 app 里加上一个统计框架，它能帮助你看到用户实际上是怎么用你的 app 的。X 功能有价值吗？按钮 Y 太难找到了吗？要回答这些问题，可以把点击事件、计时以及其他可测的信息发送到一个能收集并可视化这些信息的服务，比如[Google Tag Manager][google-tag-manager]。Google Tag Manager 比 Google Analytics 更灵活一些，它在 app 和 Analytics 之间插了一个数据层，因此不须更新 app 就可以通过 web service 更改数据逻辑。

[google-tag-manager]: http://www.google.com/tagmanager/

一种很好的做法是加一个轻量的辅助 class，比如 `XYZAnalyticsHelper`，用来把 app 内部的 model 和数据格式（XYZModel，NSTimeInterval 等）翻译成以字符串为主的数据层。 

```

- (void)pushAddItemEventWithItem:(XYZItem *)item editMode:(XYZEditMode)editMode
{
    NSString *editModeString = [self nameForEditMode:editMode];

    [self pushToDataLayer:@{
        @"event": "addItem",
        @"itemIdentifier": item.identifier,
        @"editMode": editModeString
    }];
}

```

这样有一个额外的好处，就是可以在需要时清除掉整个统计框架，而 app 其余的部分不会受任何影响。

### Crash Logs

### 崩溃日志

首先应该让 app 把崩溃日志发送到某个服务器上，这样你才能看得到。可以自己实现这个功能（用[PLCrashReporter][plcrashreporter]结合自己的后台），但推荐使用已有的服务，比如下面这些：

* [Crashlytics](http://www.crashlytics.com)
* [HockeyApp](http://hockeyapp.net)
* [Crittercism](https://www.crittercism.com)
* [Splunk MINTexpress](https://mint.splunk.com)

[plcrashreporter]: https://www.plcrashreporter.org


设置好这些之后，要确保每次发布都要 _保存 Xcode archive (`.xcarchive`)_ 。Archive 里包含编译出的二进制文件以及 debug symbol（`dSYM`），你需要这些数据来解析这个版本 app 的崩溃报告。



## 编译构建


### 编译配置

即使最简单的 app 也有不同的构建方式。Xcode 提供的最基本的区别是 _debug_ 和 _release_ 模式。后者的编译时优化要强很多，代价是损失了 debug 的可能性。苹果建议你开发时使用 _debug_ 模式，提交到 App Store 的包用 _release_ 模式编译。默认的模式（在 Xcode 里的运行/停止按钮旁边的下拉菜单可以更改）就是这么设置的，Run 用 _debug_ ，Archive 用 _release_ 。

不过，对于真实的应用，这样还是过于简单了。你可以——不，是[_应该_][futurice-environments]有几套不同的环境，分别用于测试、更新和其他与服务相关的操作。每套环境都可以有自己的 base URL，log 级别，bundle identifier（这样就可以同时安装），provision profile 等。因此，简单的 debug/release 不能满足要求。你可以在 Xcode 工程设置的“Info”一栏里添加更多的编译配置。

[futurice-environments]: https://blog.futurice.com/five-environments-you-cannot-develop-without

#### 编译配置的`xcconfig`文件


编译配置一般是在 Xcode 的界面里设置的，不过你也可以使用 _配置文件_ （“`.xcconfig` 文件”）来设置。这样做的好处是：

- 你可以添加注释来进行解释；


- 你可以 `#include` 其他编译配置文件，帮助避免重复：


    
    - 如果你有一些所有配置通用的设置，添加一个 `Common.xcconfig` 文件，然后把它 `#include` 到其他文件里；
    

    
    - 比如说你想要加一个在“Debug”基础上开启编译优化的配置，只需 `#include "MyApp_Debug.xcconfig"`，然后覆盖相应的设置
    

- 合并和解决冲突更简单一些。


更多关于本话题的信息，可以参考[这些幻灯片][xcconfig-slides]。

[xcconfig-slides]: https://speakerdeck.com/hasseg/xcode-configuration-files

### Targets


Target 的概念比 project 低一个级别，也就是说，一个 project 可以有数个 target，这些 target 的设置可以覆盖 project 的设置。粗略地说，每个 target 对应代码库里的“一个 app”。举个例子，你可能针对不同国家的 App Store 有不同的 app（都是从同一个代码库编译出来的）。每个 app 都需要开发/更新/release 的编译配置，因此用编译配置来处理会比 target 更好一些。一个 app 只有一个 target 完全不足为奇。

### Schemes


Scheme 告诉 Xcode 在 Run、Test、Profile、Analyze 和 Archive 时分别应该干什么。基本上，以上每个操作的 scheme 对应一个 target 和一套编译配置。你也可以传递启动参数，比如 app 运行的语言（对于测试本地化很方便！）或者设置一些 debug 用的诊断 flag。

Scheme 的推荐命名方式是 `MyApp (<Language>) [Environment]`：

    MyApp (English) [Development]
    MyApp (German) [Development]
    MyApp [Testing]
    MyApp [Staging]
    MyApp [App Store]


对于大部分环境其中的语言是不需要的，因为 app 有可能通过 Xcode 之外的途径安装，比如 TestFlight，这样启动参数就会被忽略。这种情况下，只能手动设置设备语言来测试本地化。


## 部署


把应用安装到 iOS 设备上可算不上简单直接。尽管如此，在这里会介绍几个核心概念；理解这些概念，会对你的部署有很大的帮助。


### 签名

只要你想把应用跑在真实的设备上（相对于模拟器而言），你就需要在编译时用一个苹果颁发的 _证书_ 来签名。每个证书对应一对公钥/私钥，私钥保存在你的 Mac 的钥匙串中。证书有两种：

* __开发证书：__ 团队里的每个开发者都可以有自己的开发证书，是通过请求获得的。Xcode 可以自动完成这项工作，不过最好还是不要点击那个神奇的“Fix issue”按钮，而是自己做一遍来理解这个过程到底做了什么。要把开发环境打的包安装到设备上就需要开发证书。


* __分发证书：__ 可以有多个，不过最好还是限制为每个组织一个，然后通过内部渠道分享它相关联的密钥。要发布到 App Store 或者企业的内部“app store”，就需要这个证书。


### Provisioning

除了证书之外，还有 __provisioning profiles__ ，它就是关联证书和设备的一环。它同样有两种，分别用于开发和分发这两种不同目的：


* __Development provisioning profile:__ 它包括被授权安装、运行 app 的设备列表。同时它与一个或多个开发证书相关联，每个开发证书对应一个可以使用这个 profile 的开发者。这种 profile 可以与特定 app 绑定，但是对于开发的用途，大部分用通配的 profile 即可，App ID 以星号（*）结尾。

* __Distribution provisioning profile:__ 有 3 种分发的途径，每种都有一种不同的使用情景。每个 distribution profile 与一个分发证书相关联，证书过期即失效。

    
    * __Ad-Hoc:__ 与开发证书相同，它包含可以安装 app 的设备白名单。这种 profile 可以用来在每年最多 100 个设备上做 beta 测试。想要更为顺畅的体验，增加至 1000 个不同的用户，你可以使用苹果新推出的[TestFlight][testflight]服务。Supertop 上对它的优势和问题有[一个很好的总结][testflight-discussion]。
    
    
    * __App Store:__ 这种 profile 没有设备列表，因为任何人都可以通过苹果的官方分发渠道安装 app。发布到 App Store 会需要这种 profile。
    

    * __Enterprise:__ 如同 App Store 类型一样，没有设备白名单，任何人都可以通过企业的内部“app store”来安装 app。

[testflight]: https://developer.apple.com/testflight/
[testflight-discussion]: http://blog.supertop.co/post/108759935377/app-developer-friends-try-testflight


要把所有的证书和 profile 同步到你的机器上，到 Xcode 的 Preferences 里的 Accounts，在这里添加你的 Apple ID，然后双击 team 名称。底部有一个刷新按钮，但有时需要重启 Xcode 才能正常刷新。

#### Debugging Provisioning

有时候你需要 debug 一个 provisioning 问题。例如，Xcode 可能拒绝把包安装到设备上，因为设备不在（development 或 ad-hoc 的）profile 的设备列表上。在这种情况下，你可以使用 Craig Hockenberry 优秀的[Provisioning][provisioning]插件，定位到`~/Library/MobileDevice/Provisioning Profiles`，选择`.mobileprovision`文件然后按空格键，启动 Finder 的快速搜索功能。它会展示出非常丰富的信息，包括设备、授权、证书 和 App ID 等。

[provisioning]: https://github.com/chockenberry/Provisioning


### 上传


[iTunes Connect][itunes-connect] 是苹果 App Store 上 app 的管理平台。要上传一个包，Xcode 6 需要用一个开发者账号的 Apple ID 来签名。这里如果你有多个开发者账号，想要分别上传他们的 app，可能遇到一些麻烦，因为不知为何 _一个特定的 Apple ID 只能与一个 iTunes Connect 账号相关联_ 。一个替代方法是，为每个 iTunes Connect 账号都创建一个新的 Apple ID，然后使用 Application Loader 代替 Xcode 来上传包。这样就把打包签名与上传 `.app` 文件的过程解耦了。


上传包之后，保持耐心，可能一个小时后这个版本的 app 才会出现在 Builds 一栏。当它出现以后，你可以把它与 app 的版本信息链接起来，然后提交审核。

[itunes-connect]: https://itunesconnect.apple.com

## App内购买（IAP）


验证 app 内购买的收据时，请记得进行以下检查：

- __真伪性:__ 购买收据确实来自苹果；
- __完整性:__ 收据没有被篡改；
- __应用匹配:__ 收据里的 bundle ID 符合你的 app 的 bundle ID；
- __产品匹配：__ 收据里的 product ID 符合你预期的 product ID；
- __最新性:__ 你之前没有见过相同的收据 ID


设计你的 IAP 系统时，尽量把售卖的内容存储在 server 端，然后仅当收到有效的、通过以上所有检查的收据后，才把内容提供给 client 端。这样的设计阻碍了常规的盗版机制，并且——既然验证是在 server 端进行的——你可以利用苹果的 HTTP 收据验证服务，而不是自己解析收据的 `PKCS #7` / `ASN.1` 格式。

关于这个话题的更多信息，可以参考[Futurice blog: 在你的 iOS app 里验证 app 内购买][futu-blog-iap]。

[futu-blog-iap]: http://futurice.com/blog/validating-in-app-purchases-in-your-ios-app

[reactivecocoa-github]: https://github.com/ReactiveCocoa/ReactiveCocoa