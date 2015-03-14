iOS Good Practices
==================
iOS 最佳实践
==================

_Just like software, this document will rot unless we take care of it. We encourage everyone to help us on that – just open an issue or send a pull request!_

_这份文档就像软件项目一样，如果我们不维护它就会逐渐腐坏。欢迎大家跟我们一起来维护它——只需提交 issue 或者发 pull request 即可！_

Interested in other mobile platforms? Our [Best Practices in Android Development][android-best-practices] and [Windows App Development Best Practices][windows-app-development-best-practices] documents have got you covered.

对其他移动平台感兴趣？也许我们的[《Android 开发最佳实践》][android-best-practices]以及[《Windows App 开发 最佳实践》][windows-app-development-best-practices]能满足你。

[android-best-practices]: https://github.com/futurice/android-best-practices
[windows-app-development-best-practices]: https://github.com/futurice/windows-app-development-best-practices

## Why?
## 为什么要写这篇文档？

Getting on board with iOS can be intimidating. Neither Swift nor Objective-C are widely used elsewhere, the platform has its own names for almost everything, and it's a bumpy road for your code to actually make it onto a physical device. This living document is here to help you, whether you're taking your first steps in Cocoaland or you're curious about doing things "the right way". Everything below is just suggestions, so if you have a good reason to do something differently, by all means go for it!

iOS 开发在上手时可能会有些令人生畏。无论是 Objective-C 还是 Swift 在别处都没有广泛的应用，iOS 这个平台几乎对一切都有一套不同的叫法，而尝试把你的代码跑在真机上的过程难免磕磕碰碰。这份持续更新的文档就是来帮你的，无论你是 Cocoa 王国的新手，或是想知道“最佳做法”是什么，都可以一读。下文仅供参考，如果你有理由采取不同的做法，不用顾虑，只管做吧！

## Getting Started

## 上手

### Xcode



[Xcode][xcode] is the IDE of choice for most iOS developers, and the only one officially supported by Apple. There are some alternatives, of which [AppCode][appcode] is arguably the most famous, but unless you're already a seasoned iOS person, go with Xcode. Despite its shortcomings, it's actually quite usable nowadays!

[Xcode][xcode] 是绝大部分 iOS 开发者选择的 IDE，也是唯一一个苹果官方支持的 IDE。也有一些其他选择，最著名的可能要数 [AppCode][appcode] 了。但除非你已经对 iOS 游刃有余，否则还是用 Xcode 吧。尽管 Xcode 有一些缺点，它现在还算是相当实用的！

To install, simply download [Xcode on the Mac App Store][xcode-app-store]. It comes with the newest SDK and simulators, and you can install more stuff under _Preferences > Downloads_.

要安装 Xcode，只需在 [Mac 的 App Score][xcode-app-store] 上下载即可。它自带最新版的 SDK 和 iOS 模拟器，其他版本可以在 _Preferences > Downloads_ 处安装。

[xcode]: https://developer.apple.com/xcode/
[appcode]: https://www.jetbrains.com/objc/
[xcode-app-store]: https://itunes.apple.com/us/app/xcode/id497799835

### Project Setup

### 建立工程

A common question when beginning an iOS project is whether to write all views in code or use Interface Builder with Storyboards or XIB files. Both are known to occasionally result in working software. However, there are a few considerations:

开始一个新的 iOS 项目时，一个常见的问题是：用代码写界面还是用 Storyboard、xib 画界面。在现有的应用里，这两种做法都占有一席之地。你需要考虑以下几点：

#### Why code?
#### 用代码写界面有哪些好处？
* Storyboards are more prone to version conflicts due to their complex XML structure. This makes merging much harder than with code.
* Storyboard 的 XML 结构很复杂，所以如果用 Storyboard ，合并代码时很容易冲突，比起用代码写的界面要麻烦许多。
* It's easier to structure and reuse views in code, thereby keeping your codebase [DRY][dry].
* 用代码写界面时，构建和重用 view 更加方便，因此能保持你的 codebase 遵循[DRY 原则][dry]。
* All information is in one place. In Interface Builder you have to click through all the inspectors to find what you're looking for.
* 所有的信息都集中在一处。如果用 Interface Builder，你还得到处点开各种检查器，才能找到你要设置的属性。

[dry]: http://en.wikipedia.org/wiki/Don%27t_repeat_yourself

#### Why Storyboards?
#### 用 Storyboard 画界面有哪些好处？
* For the less technically inclined, Storyboards can be a great way to contribute to the project directly, e.g. by tweaking colors or layout constraints. However, this requires a working project setup and some time to learn the basics.
* 对技术不太熟悉的人也可以画 Storyboard，调整颜色、layout 约束，为项目做出直接贡献。不过，要做这些需要工程已经建好，并且也要了解一些基本知识。
* Iteration is often faster since you can preview certain changes without building the project.
* 开发迭代会更快，因为不需要 build 工程就能预览到做出的改动。
* In Xcode 6, custom fonts and UI elements are finally represented visually in Storyboards, giving you a much better idea of the final appearance while designing.
* 在 Xcode 6 中，在 Storyboard 里终于能看到自定义的字体和 UI 控件样式了。这让你在设计时能更好地了解界面的最终外观。
* Starting with iOS 8, [Size Classes][size-classes] allow you to design for different device types and screens without duplication.
* 从 iOS 8 开始，你可以用 Size Classes 来设计同时支持各种屏幕尺寸的界面，省去了很多重复工作。

[size-classes]: http://blog.futurice.com/adaptive-view-ios8

### Ignores

### gitignore 文件

A good first step when putting a project under version control is to have a decent `.gitignore` file. That way, unwanted files (user settings, temporary files, etc.) will never even make it into your repository. Luckily, GitHub has us covered for both [Objective-C][objc-gitignore] and [Swift][swift-gitignore].

要为一个项目添加版本控制，最好第一步就弄一个恰当的`.gitignore`文件。这样一来，不需要的文件（例如用户设置、临时文件等等）就不会进入 repository 了。幸运的是，Github 帮我们同时准备好了 [Objective-C 版][objc-gitignore] 和 [Swift 版][swift-gitignore]。

[objc-gitignore]: https://github.com/github/gitignore/blob/master/Objective-C.gitignore
[swift-gitignore]: https://github.com/github/gitignore/blob/master/Swift.gitignore

### CocoaPods

If you're planning on including external dependencies (e.g. third-party libraries) in your project, [CocoaPods][cocoapods] offers easy and fast integration. Install it like so:

如果你准备在工程里引入外部依赖（例如第三方库），[CocoaPods][cocoapods]提供了快速而便捷的集成方法。安装方法如下：

    sudo gem install cocoapods

To get started, move inside your iOS project folder and run

开始的第一步是进入你的工程目录，然后运行

    pod init

This creates a Podfile, which will hold all your dependencies in one place. After adding your dependencies to the Podfile, you run

这样会创建一个 Podfile，在这里集中管理所有的依赖。把你的依赖添加到 Profile 里，然后运行

    pod install

to install the libraries and include them as part of a workspace which also holds your own project. It is generally [recommended to commit the installed dependencies to your own repo][committing-pods], instead of relying on having each developer running `pod install` after a fresh checkout.

来安装这些库，并且把它们和你自己的工程一起放进一个 workspace 里。在 commit 的时候，一般[推荐把依赖在你的 repo 里安装好之后再 commit][committing-pods]，最好不要让每个开发者 checkout 之后还要自己跑一遍`pod install`。

Note that from now on, you'll need to open the `.xcworkspace` file instead of `.xcproject`, or your code will not compile. The command

要注意，从此以后，打开工程的时候就要打开`.xcworkspace`文件了，不要再打开`.xcproject`，否则代码编译不通过。下面这条命令

    pod update

will update all pods to the newest versions permitted by the Podfile. You can use a wealth of [operators][cocoapods-pod-syntax] to specify your exact version requirements.

会把所有的 pod 都更新到 Podfile 允许的最新版本。你可以使用一系列的[符号][cocoapods-pod-syntax]来准确指定你对版本的要求。

[cocoapods]: http://www.cocoapods.org
[cocoapods-pod-syntax]: http://guides.cocoapods.org/syntax/podfile.html#pod
[committing-pods]: https://www.dzombak.com/blog/2014/03/including-pods-in-source-control.html

### Project Structure

### 工程结构

To keep all those hundreds of source files ending up in the same directory, it's a good idea to set up some folder structure depending on your architecture. For instance, you can use the following:

既然把这些数以百计的源文件都保存在同一目录下，根据工程结构来建立一个目录结构是个好主意。例如，你可以使用以下的结构：

    ├─ Models
    ├─ Views
    ├─ Controllers
    ├─ Stores
    ├─ Helpers

First, create them as groups (little yellow "folders") within the group with your project's name in Xcode's Project Navigator. Then, for each of the groups, link them to an actual directory in your project path by opening their File Inspector on the right, hitting the little gray folder icon, and creating a new subfolder with the name of the group in your project directory.

首先，在 Xcode 的 Project Navigator（左边栏）里，把这些目录建立为 group（小小的黄色“文件夹”），建在与工程的同名的 group 下。然后，把每一个 group 与工程路径下实际的文件夹链接起来，方法是选中 group，打开右边栏的 File Inspector，点击小小的灰色文件夹 icon，然后在工程目录下创建一个新的子文件夹，名称与 group 相同。

#### Localization

#### 本地化

Keep all user strings in localization files right from the beginning. This is good not only for translations, but also for finding user-facing text quickly. You can add a launch argument to your build scheme to launch the app in a certain language, e.g.

从最开始就要把所有的文案放在本地化文件里。这不仅有利于翻译，也能让你更快地找到面向用户的文字。你可以在 build scheme 里添加一个 launch 参数，指定在某种语言下启动 app，例如：

    -AppleLanguages (Finnish)

For more complex translations such as plural forms that depending on a number of items (e.g. "1 person" vs. "3 people"), you should use the [`.stringsdict` format][stringsdict-format] instead of a regular `localizable.strings` file. As soon as you've wrapped your head around the crazy syntax, you have a powerful tool that knows how to make plurals for "one", some", "few" and "many" items, as needed [e.g. in Russian or Arabic][language-plural-rules].

对于更复杂的翻译，比如与名词的数量有关的复数形式（如 "1 person" 对应 "3 people"），你应该使用[`.stringsdict` 格式][stringsdict-format]来替换普通的`localizable.strings`文件。只要你能习惯这种奇特的语法，你就拥有了一个强大的工具，可以根据需要（[例如俄语或阿拉伯语的规则][language-plural-rules]）把名词变为“一个”、“一些”、“少数”和“许多”等复数形式。

Find more information about localization in [these presentation slides][l10n-slides] from the February 2012 HelsinkiOS meetup. Most of the talk is still relevant in October 2014.

更多关于本地化的信息，请参考 2012 年 2 月 HelsinkiOS 大会上的[这些幻灯片][l10n-slides]。其中的大部分演讲至少到 2014 年 10 月为止仍然是不过时的。

[stringsdict-format]: https://developer.apple.com/library/prerelease/ios/documentation/MacOSX/Conceptual/BPInternational/StringsdictFileFormat/StringsdictFileFormat.html
[language-plural-rules]: http://www.unicode.org/cldr/charts/latest/supplemental/language_plural_rules.html
[l10n-slides]: https://speakerdeck.com/hasseg/localization-practicum

#### Constants

#### 常量

Keep app-wide constants in a `Constants.h` file that is included in the prefix header.

把整个 app 范围的常量定义在一个`Constants.h`文件里，然后在 prefix header 里 include 这个文件。

Instead of preprocessor macro definitions (via `#define`), use actual constants:

不要再用 `#define` 这类预处理宏定义了，改成用 actual constant：

    static CGFloat const XYZBrandingFontSizeSmall = 12.0f;
    static NSString * const XYZAwesomenessDeliveredNotificationName = @"foo";

Actual constants are type-safe, have more explicit scope (they’re not available in all imported/included files until undefined), cannot be redefined or undefined in later parts of the code, and are available in the debugger.

Actual constant 是类型安全的，作用域更加明确（括号里的暂时不翻译，我不确定是我理解错了，还是作者写错了），不能在后续的代码中重新 define 也不能 #undef，并且在 debugger 中可用。

### Branching Model

### 分支策略

Especially when distributing an app to the public (e.g. through the App Store), it's a good idea to isolate releases to their own branch with proper tags. Also, feature work that involves a lot of commits should be done on its own branch. [`git-flow`][gitflow-github] is a tool that helps you follow these conventions. It is simply a convenience wrapper around Git's branching and tagging commands, but can help maintain a proper branching structure especially for teams. Do all development on feature branches (or on `develop` for smaller work), tag releases with the app version, and commit to master only via

App 发布的时候把 release 代码从原有的分支上隔离出来，并且加上适当的 tag，是很好的做法，对于向公众分发（比如通过 App Store）的 app 这一点尤其重要。同时，涉及到大量 commit 的 feature 应该在独立的分支上完成。[`git-flow`][gitflow-github]是一个帮助你遵守这些原则的工具。它只是在 Git 的分支和 tag 命令上简单加了一层包装，就可以帮助维护一套适当的分支结构，对于团队协作尤为有用。所有的开发都应该在 feature 对应的分支上完成（小改动在`develop`分支上），给 release 打上 app 版本的 tag，然后 commit 到 master 分支时只能用下面这条命令：

    git flow release finish <version>

[gitflow-github]: https://github.com/nvie/gitflow

## Common Libraries

## 常用的库

Generally speaking, make it a conscious decision to add an external dependency to your project. Sure, this one neat library solves your problem now, but maybe later gets stuck in maintenance limbo, with the next OS version that breaks everything being just around the corner. Another scenario is that a feature only achievable with external libraries suddenly becomes part of the official APIs. In a well-designed codebase, switching out the implementation is a small effort that pays off quickly. Always consider solving the problem using Apple's extensive (and mostly excellent) frameworks first!

一般来说，在工程里添加外部依赖要谨慎。当然，眼下某个第三方库能漂亮地解决你的问题，但或许不久之后就陷入了维护的泥淖，最后随着下一版 OS 的发布全线崩溃。另一种情况是，原先只能通过引用外部库来实现的 feature，突然官方 API 也支持了。在设计良好的 codebase 里，把第三方库替换为官方的实现花不了多少功夫，但在将来会大有裨益。永远要优先考虑用苹果官方的框架（也是最好的框架）来解决问题！

Therefore this section has been deliberately kept rather short. The libraries featured here tend to reduce boilerplate code (e.g. Auto Layout) or solve complex problems that require extensive testing, such as date calculations. As you become more proficient with iOS, be sure to dive into the source here and there, and acquaint yourself with their underlying Apple frameworks. You'll find that those alone can do a lot of the heavy lifting.

因此，这一章有意写得比较简短。下面介绍的第三方库主要用来减少模板代码（例如 Auto Layout）或者用来解决复杂的、需要大量测试的问题，例如计算日期。随着你对 iOS 越来越精通，务必要四处看看它们的源码，熟悉它们所使用的底层框架。你会发现做好这些就能减轻许多重担了。

### AFNetworking

A perceived 99.95 percent of iOS developers use this network library. While `NSURLSession` is surprisingly powerful by itself, `AFNetworking` remains unbeaten when it comes to actually managing a queue of requests, which is pretty much a requirement in any modern app.

大约 99.95% 的 iOS 开发者都使用这个网络库。尽管`NSURLSession`已经非常强大了，但一旦涉及到实际管理请求队列时，`AFNetworking`仍然立于不败之地，而现代的 app 基本都会有这个需求。

### DateTools

As a general rule, [don't write your date calculations yourself][timezones-youtube]. Luckily, in DateTools you get an MIT-licensed, thoroughly tested library that covers pretty much all your calendary needs.

一条常识是，[不要自己写日期计算][timezones-youtube]。幸运的是，有 DateTools 这样一个基于 MIT 协议、充分测试过的第三方库，基本能满足所有日期方面的要求。

[timezones-youtube]: https://www.youtube.com/watch?v=-5wpm-gesOY

### Auto Layout Libraries

### Auto Layout 相关的库

If you prefer to write your views in code, chances are you've met either of Apple's awkward syntaxes – the regular 'NSLayoutConstraint' factory or the so-called [Visual Format Language][visual-format-language]. The former is extremely verbose and the latter based on strings, which effectively prevents compile-time checking.

如果你习惯用代码写 view，你很可能用过这两种诡异的语法——常规的`NSLayoutConstraint`工厂，以及所谓的[可视化语言][visual-format-language]。前者极其冗长，而后者是基于字符串的，完全躲过了编译检查。

[Masonry][masonry-github] remedies this by introducing its own DSL to make, update and replace constraints. A similar approach for Swift is taken by [Cartography][cartography-github], which builds on the language's powerful operator overloading features. For the more conservative, [FLKAutoLayout][flkautolayout-github] offers a clean, but rather non-magical wrapper around the native APIs.

而 [Masonry][masonry-github] 的解决方法是：引入自己定义的 DSL 来创建、更新和替换约束。Swift 有一个类似的库 [Cartography][cartography-github]，是建立在这门语言强大的运算符重载基础上的。保守一些的库有 [FLKAutoLayout][flkautolayout-github]，它对原生 API 加了一层整洁而不奇异的包装。

[visual-format-language]: https://developer.apple.com/library/ios/documentation/userexperience/conceptual/AutolayoutPG/VisualFormatLanguage/VisualFormatLanguage.html#//apple_ref/doc/uid/TP40010853-CH3-SW1
[masonry-github]: https://www.github.com/Masonry/Masonry
[cartography-github]: https://github.com/robb/Cartography
[flkautolayout-github]: https://github.com/floriankugler/FLKAutoLayout

## Architecture

## 架构

* [Model-View-Controller-Store (MVCS)][mvcs]
    * This is the default Apple architecture (MVC), extended by a Store layer that vends Model instances and handles the networking, caching etc.
    * 这是苹果默认的架构(MVC)上增加了一个 Store 层，用来吐出 Model，处理网络请求、缓存等。
    * Every Store exposes to the view controllers either `RACSignal`s or `void`-returning methods with custom completion blocks
    * 每个 Store 暴露给 view controller 的或者是`RACSignal`，或者是返回值为`void`、参数带有自定义的 completion block 的方法。
* [Model-View-ViewModel (MVVM)][mvvm]
    * Motivated by "massive view controllers": MVVM considers `UIViewController` subclasses part of the View and keeps them slim by maintaining all state in the ViewModel
    * MVVM 是为了解决“巨大的 view controller”而生，它把`UIViewController`的子类看做 View 层的一部分，用 ViewModel 维护所有的状态来给 ViewController 瘦身。
    * Quite new concept for Cocoa developers, but [gaining][cocoasamurai-rac] [traction][raywenderlich-mvvm]
    * 对于 Cocoa 开发者是一个很新的概念，但是[正在引起][cocoasamurai-rac] [越来越多的关注][raywenderlich-mvvm]
* [View-Interactor-Presenter-Entity-Routing (VIPER)][viper]
    * Rather exotic architecture that might be worth looking into in larger projects, where even MVVM feels too cluttered and testability is a major concern
    * 相当特别的架构，大型项目可能值得参考，尤其是即使用 MVVM 还是比较凌乱，以及对需要重点考虑可测试性的情况。

[mvcs]: http://programmers.stackexchange.com/questions/184396/mvcs-model-view-controller-store
[mvvm]: http://www.objc.io/issue-13/mvvm.html
[cocoasamurai-rac]: http://cocoasamurai.blogspot.de/2013/03/basic-mvvm-with-reactivecocoa.html
[raywenderlich-mvvm]: http://www.raywenderlich.com/74106/mvvm-tutorial-with-reactivecocoa-part-1
[viper]: http://www.objc.io/issue-13/viper.html

### “Event” Patterns

### "通知" 模型

These are the idiomatic ways for components to notify others about things:

以下是组件之间互发通知的一些常见手段：

* __Delegation:__ _(one-to-one)_ Apple uses this a lot (some would say, too much). Use when you want to communicate stuff back e.g. from a modal view.
* __Delegation:__ _(一对一)_ 苹果官方经常用这个模式（有些人认为用得太泛滥了）。主要用于回传，比如从模态框回传数据。
* __Callback blocks:__ _(one-to-one)_ Allow for a more loose coupling, while keeping related code sections close to each other. Also scales better than delegation when there are many senders.
* __Callback blocks:__ _(一对一)_ 耦合更松，同时能让相关联的代码在一起。并且，消息发出者数量很多时比 delegation 更方便。
* __Notification Center:__ _(one-to-many)_ Possibly the most common way for objects to emit “events” to multiple observers. Very loose coupling — notifications can even be observed globally without reference to the dispatching object.
* __Notification Center:__ _(一对多)_ 可能是一个对象给多个观察者发出“通知”时最常用的方法。耦合非常松，甚至可以把通知发到全局，不需要对调度者的引用。
* __Key-Value Observing (KVO):__ _(one-to-many)_ Does not require the observed object to explicitly “emit events” as long as it is _Key-Value Coding (KVC)_ compliant for the observed keys (properties). Usually not recommended due to its implicit nature and the cumbersome standard library API.
* __Key-Value Observing (KVO):__ _(一对多)_ 不需要被观测的对象主动“发出通知”，只需要被观测的键（属性）支持 _Key-Value Coding (KVC)_ 。这种模式比较含混，而且标准 API 比较繁复，所以一般不推荐使用。
* __Signals:__ _(one-to-many)_ The centerpiece of [ReactiveCocoa][reactivecocoa-github], they allow chaining and combining to your heart's content, thereby offering a way out of [callback hell][elm-escape-from-callback-hell].
* __Signals:__ _(一对多)_ 这是[ReactiveCocoa][reactivecocoa-github]的核心，它允许结合关键内容的链式调用，用这种方法逃离[回调深渊（嵌套过多的回调）][elm-escape-from-callback-hell]。

[elm-escape-from-callback-hell]: http://elm-lang.org/learn/Escape-from-Callback-Hell.elm

### Models

Keep your models immutable, and use them to translate the remote API's semantics and types to your app. Github's [Mantle](https://github.com/Mantle/Mantle) is a good choice.

要确保你的 model 是不可变的，它们用来把远程 API 的语义和类型转换为 app 适用的语义和类型。Github 的 [Mantle](https://github.com/Mantle/Mantle) 是个不错的选择。

### Views

When laying out your views using Auto Layout, be sure to add the following to your class:

使用 Auto Layout 布局时，要记得在 View 类里加上：

    + (BOOL)requiresConstraintBasedLayout
    {
        return YES;
    }

Otherwise you may encounter strange bugs when the system doesn't call `-updateConstraints` as you would expect it to.

不然，系统可能不会如期调用`-updateConstraints`，而导致奇怪的 bug。

### Controllers

Use dependency injection, i.e. pass any required objects in as parameters, instead of keeping all state around in singletons. The latter is okay only if the state _really_ is global.

要使用依赖注入，也就是说，应该把 Controller 需要的数据用参数传进来，而不要把所有状态信息都保存在单例里。后者仅当这些状态 _的确_ 是全局的情况下才适用。

```
+ [[FooDetailsViewController alloc] initWithFoo:(Foo *)foo];
```

## Networking
## 网络请求

### Traditional way: Use custom callback blocks
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

This works, but can quickly lead to callback hell if you need to chain multiple requests.

这样虽可行，但是如果要发起几个链式请求，很容易导致回调深渊。

### Reactive way: Use RAC signals

### Reactive 的方法：使用 RAC signal

If you find yourself in callback hell, have a look at [ReactiveCocoa (RAC)][reactivecocoa-github]. It's a versatile and multi-purpose library that can change the way people write [entire apps][groceryList-github], but you can also use it sparingly where it fits the task.

如果你身陷回调深渊，可以看看[ReactiveCocoa (RAC)][reactivecocoa-github]。这是一个多功能、多用途的库，它可以改变[整个 app ][groceryList-github] 的写法。但你也可以仅在适合用它的时候，零散地用一用。

There are good introductions to the concept of RAC (and FRP in general) on [Teehan+Lax][teehan-lax-rac] and [NSHipster][nshipster-rac].

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

This allows us to transform or filter gigs before showing them, by combining the gig signal with other signals.

在这里我们可以把 gig 信号与其他信号结合，因此可以在展示 gig 之前做一些修改、过滤等处理。

## Assets

[Asset catalogs][asset-catalogs] are the best way to manage all your project's visual assets. They can hold both universal and device-specific () assets and will automatically serve the correct ones for a given name. Teaching your designer(s) how to add and commit things there (Xcode has its own built-in Git client) can save a lot of time that would otherwise be spent copying stuff from emails or other channels to the codebase. It also allows them to instantly try out their changes and iterate if needed.

使用 [Asset catalogs][asset-catalogs] 是管理工程中视觉素材的最好方法。这里既可以添加 iPhone 和 iPad 共用的素材，也可以添加针对特定设备（4寸屏 iPhone，iPhone Retina，iPad 等等）的素材，并且会根据名称来自动提供恰当的素材。教会你的设计师（们）怎么在这里添加并 commit 素材，可以帮你节省许多时间，再也不用把素材从邮件或者别的什么渠道导进代码库里了。同时，这样做也可以让他们即刻看到自己的改动，可以根据需要进行迭代。

[asset-catalogs]: https://developer.apple.com/library/ios/recipes/xcode_help-image_catalog-1.0/Recipe.html

### Using Bitmap Images
### 使用位图

Asset catalogs expose only the names of image sets, abstracting away the actual file names within the set. This nicely prevents asset name conflicts, as files such as `button_large@2x.png` are now namespaced inside their image sets. However, some discipline when naming assets can make life easier:

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

The modifiers `-568h`, `@2x`, `~iphone` and `~ipad` are not required per se, but having them in the file name when dragging the file to an image set will automatically place them in the right "slot", thereby preventing assignment mistakes that can be hard to hunt down.

其中的`-568h`、`@2x`、`~iphone`以及`~ipad`这些标示符本身不是必需的，但是如果在文件名里加上它们，把文件拖动到 asset 时就能自动落到正确的“格子”上，因此能避免难以察觉的错误拖放。

### Using Vector Images
### 使用矢量图

You can include the original [vector graphics (PDFs)][vector-assets] produced by designers into the asset catalogs, and have Xcode automatically generate the bitmaps from that. This reduces the complexity of your project (the number of files to manage.)

你可以把设计师设计的原始的[矢量图 (PDFs)][vector-assets]放进 asset catalog，让 Xcode 来自动生成位图。这样能减少工程的复杂度（减少文件个数）。

[vector-assets]: http://martiancraft.com/blog/2014/09/vector-images-xcode6/

## Coding Style
## 编码风格

### Naming
### 命名

Apple pays great attention to keeping naming consistent, if sometimes a bit verbose, throughout their APIs. When developing for Cocoa, you make it much easier for new people to join the project if you follow [Apple's naming conventions][cocoa-coding-guidelines].

Apple 非常注意在 API 中保持命名一致性，有时候有点过于冗长了。做 Cocoa 开发时要遵循[Apple的命名规范][cocoa-coding-guidelines]，这样能让加入项目的新人轻松许多。

[cocoa-coding-guidelines]: https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html

Here are some basic takeaways you can start using right away:

以下是几条看了就能用上的基本规则：

A method beginning with a _verb_ indicates that it performs some side effects, but won't return anything:

以 _动词_ 开头的方法表示它执行的操作会造成一些影响，但是不返回任何值。

`- (void)loadView;`
`- (void)startAnimating;`

Any method starting with a _noun_, however, returns that object and should do so without side effects:

相反的是，以 _名词_ 开头的方法返回一个对象，但不会造成额外的影响。

`- (UINavigationItem *)navigationItem;`
`+ (UILabel *)labelWithText:(NSString *)text;`

It pays off to keep these two as separated as possible, i.e. not perform side effects when you transform data, and vice versa. That will keep your side effects contained to smaller sections of the code, which makes it more understandable and facilitates debugging.

尽可能地区分这两种方法会有很多好处，也就是说，如果一个方法是处理数据的，就不要让它造成额外的影响，反过来也一样。这样可以让造成影响的代码块保持紧凑，因此可以帮助理解代码，并且有利于 debug。

### Structure

### 代码结构

[Pragma marks](http://nshipster.com/pragma/) are a great way to group your methods, especially in view controllers. Here is a common structure that works with almost any view controller:

[Pragma marks](http://nshipster.com/pragma/)是给方法分组很好的方法。

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

The most important point is to keep these consistent across your project's classes.

最重要的是要让这些分块标记在工程里所有的类里保持一致。

### External Style Guides

### 其他风格指南

Futurice does not have company-level guidelines for coding style. It can however be useful to peruse the Objective-C style guides of other development shops, even if some bits can be quite company-specific or opinionated:

Futurice（作者所在的公司）并没有公司范围的编码风格指南。不过，仔细研究一下其他开发社区的 Objective-C 风格指南会非常有用，尽管有些部分可能是只对特定公司有效或者比较主观的。

* [GitHub](https://github.com/github/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [The New York Times](https://github.com/NYTimes/objective-c-style-guide)
* [Ray Wenderlich](https://github.com/raywenderlich/objective-c-style-guide)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [Luke Redpath](http://lukeredpath.co.uk/blog/2011/06/28/my-objective-c-style-guide/)

## Diagnostics

## 诊断

### Compiler warnings

### 编译警告

It is recommended that you enable as many compiler warnings as possible, and treat warnings as errors. This recommendation is justified in [these presentation slides][warnings-slides]. The slides also contain information on how to suppress certain warnings in specific files, or in specific sections of code.

建议你尽量把编译警告都打开，并且像对待 error 一样对待 warning。[这份幻灯片][warnings-slides] 论证了这一点。幻灯片里同时还讲了如何在特定文件里或者特定的代码段里忽略特定的 warning。

In short, add at least these values to the _“Other Warning Flags”_ build setting:

一句话，在 build setting 的 _“Other Warning Flags”_ 里至少要加入以下两个值：

- `-Wall` _（开启非常多额外的 warning）_
- `-Wextra` _（开启许多额外的 warning）_

Also enable the _“Treat warnings as errors”_ build setting.

同时打开 build setting 里的 _“Treat warnings as errors”_ 。

[warnings-slides]: https://speakerdeck.com/hasseg/the-compiler-is-your-friend

### Clang Static Analyzer

### Clang 静态分析器

The Clang compiler (which Xcode uses) has a _static analyzer_ that performs control and data flow analysis on your code and checks for lots of errors that the compiler cannot.

Clang 编译器（也就是 XCode 使用的编译器）有一个 _静态分析器(static analyer)_ ，用来执行代码控制流和数据流的分析，可以发现许多编译器检查不出的问题。

You can manually run the analyzer from the _Product → Analyze_ menu item in Xcode.

你可以在 Xcode 的 _Product → Analyze_ 里手动运行分析器。

The analyzer can work in either “shallow” or “deep” mode. The latter is much slower but may find more issues due to cross-function control and data flow analysis.

分析器可以运行“shallow”和“deep”两种模式。后者要慢得多，但是有跨方法的控制流分析以及数据流分析，因此能发现更多问题。

Recommendations:

建议：

- Enable _all_ of the checks in the analyzer (by enabling all of the options in the “Static Analyzer” build setting sections)

- 开启分析器的 _全部_ 检查（方法是在 build setting 的“Static Analyzer”部分开启所有选项）

- Enable the _“Analyze during ‘Build’”_ build setting for your release build  configuration to have the analyzer run automatically during release builds. (Seriously, do this — you’re not going to remember to run it manually.)

- 在 build setting 里，对 release 的 build 配置开启 _“Analyze during ‘Build’”_ 。（真的，一定要这样做——你不会记得手动跑分析器的。）

- Set the _“Mode of Analysis for ‘Analyze’”_ build setting to _Shallow (faster)_

- 把 build setting 里的 _“Mode of Analysis for ‘Analyze’”_ 设为 _Shallow (faster)_

- Set the _“Mode of Analysis for ‘Build’”_ build setting to _Deep_

- 把 build setting 里的 _“Mode of Analysis for ‘Build’”_ 设为 _Deep_


### [Faux Pas](http://fauxpasapp.com/)

Created by our very own [Ali Rantakari][ali-rantakari-twitter], Faux Pas is a fabulous static error detection tool. It analyzes your codebase and finds issues you had no idea even existed. Be sure to run this before shipping any iOS (or Mac) app!

由我们的员工 [Ali Rantakari][ali-rantakari-twitter] 创作的 Faux Pas 是一个出色的静态 error 检测工具。它能分析你的代码库，找出你全然不知的错误。在发布任何 iOS（或 Mac）app 之前务必要运行它一次！

_(Note: all Futurice employees get a free license to this — just ask Ali.)_

_(注意：所有 Futurice 的员工都能得到一份免费的许可——只要问 Ali 要就行了。)_

[ali-rantakari-twitter]: https://twitter.com/AliRantakari

### Debugging

When your app crashes, Xcode does not break into the debugger by default. To achieve this, add an exception breakpoint (click the "+" at the bottom of Xcode's Debug Navigator) to halt execution whenever an exception is raised. In many cases, you will then see the line of code responsible for the exception. This catches any exception, even handled ones. If Xcode keeps breaking on benign exceptions in third party libraries e.g., you might be able to mitigate this by choosing _Edit Breakpoint_ and setting the _Exception_ drop-down to _Objective-C_. 

当 app crash 的时候，默认情况下 Xcode 并不会进入 debugger。要想进入 debugger，添加一个 Exception Breakpoint（点击 Xcode 的 Debug Navigator 底部的“+”号），遇到 exception 的时候就会暂停执行。在大部分情况下，你都能看到导致 exception 的那行代码。这种方法会捕捉到任何 exception，包括已经做了处理的 exception。如果 Xcode 常常会停在正常的 exception（比如第三方库里的）上，选择 _Edit Breakpoint_ 然后在 _Exception_ 下拉框选择 _Objective-C_ 可以减轻这种情况。

For view debugging, [Reveal][reveal] and [Spark Inspector][spark-inspector] are two powerful visual inspectors that can save you hours of time, especially if you're using Auto Layout and want to locate views that are collapsed or off-screen. Granted, Xcode offers [something very similar][xcode-view-debugging] for free, but it's iOS 8+ only and feels somewhat less polished.

在 view 的 debug 方面，[Reveal][reveal] 和 [Spark Inspector][spark-inspector] 是两个强大的可视化检查器，可以节约你大量的时间，尤其是用 Auto Layout 时想知道消失的视图去哪儿了的情况。Xcode 也免费提供了[一个类似的东西][xcode-view-debugging]，不过只支持 iOS 8+，并且略有些不够完善。

[reveal]: http://revealapp.com/
[spark-inspector]: http://sparkinspector.com
[xcode-view-debugging]: https://developer.apple.com/library/ios/recipes/xcode_help-debugger/using_view_debugger/using_view_debugger.html

### Profiling
### 评估

Xcode comes with a profiling suite called Instruments. It contains a myriad of tools for profiling memory usage, CPU, network communications, graphics and much more. It's a complex beast, but one of its more straight-forward use cases is tracking down memory leaks with the Allocations instrument. Simply choose _Product_ > _Profile_ in Xcode, select the Allocations instrument, hit the Record button and filter the Allocation Summary on some useful string, like the prefix of your own app's class names. The count in the Persistent column then tells you how many instances of each object you have. Any class for which the instance count increases indiscriminately indicates a memory leak.

Xcode 自带一套评估工具，叫做 Instruments。它包含众多的评估内存使用、CPU、网络连接、图像等方面的工具。它本身是个庞然大物，但一个比较简单直接的用途是用 Allocations instrument 来检测内存泄露。只需在 Xcode 中选择 _Product_ > _Profile_ ，选择 Allocations instrument，点击 Record 按钮，然后从 Allocation Summary 中过滤出一些有用的字符串，比如 app 里你自己写的类的类名前缀。在 Persistant 一栏中的计数显示了每个对象有多少个实例。如果某个类的实例个数一直胡乱增长，就说明有内存泄露。

Also good to know is that Instruments has an Automation tool for recording and playing back UI interactions as JavaScript files. [UI Auto Monkey][ui-auto-monkey] is a script that will use Automation to randomly pummel your app with taps, swipes and rotations which can be useful for stress/soak testing.

另外值得注意的是 Instrument 有一个 Automation 工具，用来把 UI 交互录制为 JavaScript 文件并且重放。[UI Auto Monkey][ui-auto-monkey] 是一个脚本，它借助 Automation 在你的 app 上随机点击、清扫、旋转，对压力测试/浸泡测试可能会有帮助。

[ui-auto-monkey]: https://github.com/jonathanpenn/ui-auto-monkey

## Analytics
## 统计

Including some analytics framework in your app is strongly recommended, as it allows you to gain insights on how people actually use it. Does feature X add value? Is button Y too hard to find? To answer these, you can send events, timings and other measurable information to a service that aggregates and visualizes them – for instance, [Google Tag Manager][google-tag-manager]. The latter is more versatile than Google Analytics in that it inserts a data layer between app and Analytics, so that the data logic can be modified through a web service without having to update the app.

强烈推荐在你的 app 里加上一个统计框架，它能帮助你看到用户实际上是怎么用你的 app 的。X 功能有价值吗？按钮 Y 太难找到了吗？要回答这些问题，可以把点击事件、计时以及其他可测的信息发送到一个能收集并可视化这些信息的服务，比如[Google Tag Manager][google-tag-manager]。Google Tag Manager 比 Google Analytics 更灵活一些，它在 app 和 Analytics 之间插了一个数据层，因此不须更新 app 就可以通过 web service 更改数据逻辑。

[google-tag-manager]: http://www.google.com/tagmanager/

A good practice is to create a slim helper class, e.g. `XYZAnalyticsHelper`, that handles the translation from app-internal models and data formats (XYZModel, NSTimeInterval, …) to the mostly string-based data layer:

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

This has the additional advantage of allowing you to swap out the entire Analytics framework behind the scenes if needed, without the rest of the app noticing.

这样有一个额外的好处，就是可以在需要时清除掉整个统计框架，而 app 其余的部分不会受任何影响。

### Crash Logs

### 崩溃日志

First you should make your app send crash logs onto a server somewhere so that you can access them. You can implement this manually (using [PLCrashReporter][plcrashreporter] and your own backend) but it’s recommended that you use an existing service instead — for example one of the following:

首先应该让 app 把崩溃日志发送到某个服务器上，这样你才能看得到。可以自己实现这个功能（用[PLCrashReporter][plcrashreporter]结合自己的后台），但推荐使用已有的服务，比如下面这些：

* [Crashlytics](http://www.crashlytics.com)
* [HockeyApp](http://hockeyapp.net)
* [Crittercism](https://www.crittercism.com)
* [Splunk MINTexpress](https://mint.splunk.com)

[plcrashreporter]: https://www.plcrashreporter.org

Once you have this set up, ensure that you _save the Xcode archive (`.xcarchive`)_ of every build you release. The archive contains the built app binary and the debug symbols (`dSYM`) which you will need to symbolicate crash reports from that particular version of your app.

设置好这些之后，要确保每次发布都要 _保存 Xcode archive (`.xcarchive`)_ 。Archive 里包含编译出的二进制文件以及 debug symbol（`dSYM`），你需要这些数据来解析这个版本 app 的崩溃报告。


## Building
## 编译构建

### Build Configurations

### 编译配置

Even simple apps can be built in different ways. The most basic separation that Xcode gives you is that between _debug_ and _release_ builds. For the latter, there is a lot more optimization going on at compile time, at the expense of debugging possibilities. Apple suggests that you use the _debug_ build configuration for development, and create your App Store packages using the _release_ build configuration. This is codified in the default scheme (the dropdown next to the Play and Stop buttons in Xcode), which commands that _debug_ be used for Run and _release_ for Archive.

即使最简单的 app 也有不同的构建方式。Xcode 提供的最基本的区别是 _debug_ 和 _release_ 模式。后者的编译时优化要强很多，代价是损失了 debug 的可能性。苹果建议你开发时使用 _debug_ 模式，提交到 App Store 的包用 _release_ 模式编译。默认的模式（在 Xcode 里的运行/停止按钮旁边的下拉菜单可以更改）就是这么设置的，Run 用 _debug_ ，Archive 用 _release_ 。

However, this is a bit too simple for real-world applications. You might – no, [_should!_][futurice-environments] – have different environments for testing, staging and other activities related to your service. Each might have its own base URL, log level, bundle identifier (so you can install them side-by-side), provisioning profile and so on. Therefore a simple debug/release distinction won't cut it. You can add more build configurations on the "Info" tab of your project settings in Xcode.

不过，对于真实的应用，这样还是过于简单了。你可以——不，是[_应该_][futurice-environments]有几套不同的环境，分别用于测试、更新和其他与服务相关的操作。每套环境都可以有自己的 base URL，log 级别，bundle identifier（这样就可以同时安装），provision profile 等。因此，简单的 debug/release 不能满足要求。你可以在 Xcode 工程设置的“Info”一栏里添加更多的编译配置。

[futurice-environments]: https://blog.futurice.com/five-environments-you-cannot-develop-without

#### `xcconfig` files for build settings

#### 编译配置的`xcconfig`文件

Typically build settings are specified in the Xcode GUI, but you can also use _configuration settings files_ (“`.xcconfig` files”) for them. The benefits of using these are:

编译配置一般是在 Xcode 的界面里设置的，不过你也可以使用 _配置文件_ （“`.xcconfig` 文件”）来设置。这样做的好处是：

- You can add comments to explain things

- 你可以添加注释来进行解释；

- You can `#include` other build settings files, which helps you avoid repeating yourself:

- 你可以 `#include` 其他编译配置文件，帮助避免重复：

    - If you have some settings that apply to all build configurations, add a `Common.xcconfig` and `#include` it in all the other files
    
    - 如果你有一些所有配置通用的设置，添加一个 `Common.xcconfig` 文件，然后把它 `#include` 到其他文件里；
    
    - If you e.g. want to have a “Debug” build configuration that enables compiler optimizations, you can just `#include "MyApp_Debug.xcconfig"` and override one of the settings
    
    - 比如说你想要加一个在“Debug”基础上开启编译优化的配置，只需 `#include "MyApp_Debug.xcconfig"`，然后覆盖相应的设置
    
- Conflict resolution and merging becomes easier

- 合并和解决冲突更简单一些。

Find more information about this topic in [these presentation slides][xcconfig-slides].

更多关于本话题的信息，可以参考[这些幻灯片][xcconfig-slides]。

[xcconfig-slides]: https://speakerdeck.com/hasseg/xcode-configuration-files

### Targets

A target resides conceptually below the project level, i.e. a project can have several targets that may override its project settings. Roughly, each target corresponds to "an app" within the context of your codebase. For instance, you could have country-specific apps (built from the same codebase) for different countries' App Stores. Each of these will need development/staging/release builds, so it's better to handle those through build configurations, not targets. It's not uncommon at all for an app to only have a single target.

Target 的概念比 project 低一个级别，也就是说，一个 project 可以有数个 target，这些 target 的设置可以覆盖 project 的设置。粗略地说，每个 target 对应代码库里的“一个 app”。举个例子，你可能针对不同国家的 App Store 有不同的 app（都是从同一个代码库编译出来的）。每个 app 都需要开发/更新/release 的编译配置，因此用编译配置来处理会比 target 更好一些。一个 app 只有一个 target 完全不足为奇。

### Schemes

Schemes tell Xcode what should happen when you hit the Run, Test, Profile, Analyze or Archive action. Basically, they map each of these actions to a target and a build configuration. You can also pass launch arguments, such as the language the app should run in (handy for testing your localizations!) or set some diagnostic flags for debugging.

Scheme 告诉 Xcode 在 Run、Test、Profile、Analyze 和 Archive 时分别应该干什么。基本上，以上每个操作的 scheme 对应一个 target 和一套编译配置。你也可以传递启动参数，比如 app 运行的语言（对于测试本地化很方便！）或者设置一些 debug 用的诊断 flag。

A suggested naming convention for schemes is `MyApp (<Language>) [Environment]`:

Scheme 的推荐命名方式是 `MyApp (<Language>) [Environment]`：

    MyApp (English) [Development]
    MyApp (German) [Development]
    MyApp [Testing]
    MyApp [Staging]
    MyApp [App Store]

For most environments the language is not needed, as the app will probably be installed through other means than Xcode, e.g. TestFlight, and the launch argument thus be ignored anyway. In that case, the device language should be set manually to test localization.

对于大部分环境其中的语言是不需要的，因为 app 有可能通过 Xcode 之外的途径安装，比如 TestFlight，这样启动参数就会被忽略。这种情况下，只能手动设置设备语言来测试本地化。

## Deployment

## 部署

Deploying software on iOS devices isn't exactly straightforward. That being said, here are some central concepts that, once understood, will help you tremendously with it.

把应用安装到 iOS 设备上可算不上简单直接。尽管如此，在这里会介绍几个核心概念；理解这些概念，会对你的部署有很大的帮助。

### Signing

### 签名

Whenever you want to run software on an actual device (as opposed to the simulator), you will need to sign your build with a __certificate__ issued by Apple. Each certificate is linked to a private/public keypair, the private half of which resides in your Mac's Keychain. There are two types of certificates:

只要你想把应用跑在真实的设备上（相对于模拟器而言），你就需要在编译时用一个苹果颁发的 _证书_ 来签名。每个证书对应一对公钥/私钥，私钥保存在你的 Mac 的钥匙串中。证书有两种：

* __Development certificate:__ Every developer on a team has their own, and it is generated upon request. Xcode might do this for you, but it's better not to press the magic "Fix issue" button and understand what is actually going on. This certificate is needed to deploy development builds to devices.

* __开发证书：__ 团队里的每个开发者都可以有自己的开发证书，是通过请求获得的。Xcode 可以自动完成这项工作，不过最好还是不要点击那个神奇的“Fix issue”按钮，而是自己做一遍来理解这个过程到底做了什么。要把开发环境打的包安装到设备上就需要开发证书。

* __Distribution certificate:__ There can be several, but it's best to keep it to one per organization, and share its associated key through some internal channel. This certificate is needed to ship to the App Store, or your organization's internal "enterprise app store".

* __分发证书：__ 可以有多个，不过最好还是限制为每个组织一个，然后通过内部渠道分享它相关联的密钥。要发布到 App Store 或者企业的内部“app store”，就需要这个证书。


### Provisioning

Besides certificates, there are also __provisioning profiles__, which are basically the missing link between devices and certificates. Again, there are two types to distinguish between development and distribution purposes:

除了证书之外，还有 __provisioning profiles__ ，它就是关联证书和设备的一环。它同样有两种，分别用于开发和分发这两种不同目的：

* __Development provisioning profile:__ It contains a list of all devices that are authorized to install and run the software. It is also linked to one or more development certificates, one for each developer that is allowed to use the profile. The profile can be tied to a specific app, but for most development purposes it's perfectly fine to use the wildcard profile, whose App ID ends in an asterisk (*).

* __Development provisioning profile:__ 它包括被授权安装、运行 app 的设备列表。同时它与一个或多个开发证书相关联，每个开发证书对应一个可以使用这个 profile 的开发者。这种 profile 可以与特定 app 绑定，但是对于开发的用途，大部分用通配的 profile 即可，App ID 以星号（*）结尾。

* __Distribution provisioning profile:__ There are three different ways of distribution, each for a different use case. Each distribution profile is linked to a distribution certificate, and will be invalid when the certificate expires.

* __Distribution provisioning profile:__ 有 3 种分发的途径，每种都有一种不同的使用情景。每个 distribution profile 与一个分发证书相关联，证书过期即失效。

    * __Ad-Hoc:__ Just like development profiles, it contains a whitelist of devices the app can be installed to. This type of profile can be used for beta testing on 100 devices per year. For a smoother experience and up to 1000 distinct users, you can use Apple's newly acquired [TestFlight][testflight] service. Supertop offers a good [summary of its advantages and issues][testflight-discussion].
    
    * __Ad-Hoc:__ 与开发证书相同，它包含可以安装 app 的设备白名单。这种 profile 可以用来在每年最多 100 个设备上做 beta 测试。想要更为顺畅的体验，增加至 1000 个不同的用户，你可以使用苹果新推出的[TestFlight][testflight]服务。Supertop 上对它的优势和问题有[一个很好的总结][testflight-discussion]。
    
    * __App Store:__ This profile has no list of allowed devices, as anyone can install it through Apple's official distribution channel. This profile is required for all App Store releases.
    
    * __App Store:__ 这种 profile 没有设备列表，因为任何人都可以通过苹果的官方分发渠道安装 app。发布到 App Store 会需要这种 profile。
    
    * __Enterprise:__ Just like App Store, there is no device whitelist, and the app can be installed by anyone with access to the enterprise's internal "app store", which can be just a website with links. This profile is available only on Enterprise accounts.
    
    * __Enterprise:__ 如同 App Store 类型一样，没有设备白名单，任何人都可以通过企业的内部“app store”来安装 app。

[testflight]: https://developer.apple.com/testflight/
[testflight-discussion]: http://blog.supertop.co/post/108759935377/app-developer-friends-try-testflight

To sync all certificates and profiles to your machine, go to Accounts in Xcode's Preferences, add your Apple ID if needed, and double-click your team name. There is a refresh button at the bottom, but sometimes you just need to restart Xcode to make everything show up.

要把所有的证书和 profile 同步到你的机器上，到 Xcode 的 Preferences 里的 Accounts，在这里添加你的 Apple ID，然后双击 team 名称。底部有一个刷新按钮，但有时需要重启 Xcode 才能正常刷新。

#### Debugging Provisioning

Sometimes you need to debug a provisioning issue. For instance, Xcode may refuse to install the build to an attached device, because the latter is not on the (development or ad-hoc) profile's device list. In those cases, you can use Craig Hockenberry's excellent [Provisioning][provisioning] plugin by browsing to `~/Library/MobileDevice/Provisioning Profiles`, selecting a `.mobileprovision` file and hitting Space to launch Finder's Quick Look feature. It will show you a wealth of information such as devices, entitlements, certificates, and the App ID.

有时候你需要 debug 一个 provisioning 问题。例如，Xcode 可能拒绝把包安装到设备上，因为设备不在（development 或 ad-hoc 的）profile 的设备列表上。在这种情况下，你可以使用 Craig Hockenberry 优秀的[Provisioning][provisioning]插件，定位到`~/Library/MobileDevice/Provisioning Profiles`，选择`.mobileprovision`文件然后按空格键，启动 Finder 的快速搜索功能。它会展示出非常丰富的信息，包括设备、授权、证书 和 App ID 等。

[provisioning]: https://github.com/chockenberry/Provisioning

### Uploading

### 上传

[iTunes Connect][itunes-connect] is Apple's portal for managing your apps on the App Store. To upload a build, Xcode 6 requires an Apple ID that is part of the developer account used for signing. This can make things tricky when you are part of several developer accounts and want to upload their apps, as for mysterious reasons _any given Apple ID can only be associated with a single iTunes Connect account_. One workaround is to create a new Apple ID for each iTunes Connect account you need to be part of, and use Application Loader instead of Xcode to upload the builds. That effectively decouples the building and signing process from the upload of the resulting `.app` file.



After uploading the build, be patient as it can take up to an hour for it to show up under the Builds section of your app version. When it appears, you can link it to the app version and submit your app for review.

[itunes-connect]: https://itunesconnect.apple.com

## In-App Purchases (IAP)

When validating in-app purchase receipts, remember to perform the following checks:

- __Authenticity:__ That the receipt comes from Apple
- __Integrity:__ That the receipt has not been tampered with
- __App match:__ That the app bundle ID in the receipt matches your app’s bundle identifier
- __Product match:__ That the product ID in the receipt matches your expected product identifier
- __Freshness:__ That you haven’t seen the same receipt ID before.

Whenever possible, design your IAP system to store the content for sale server-side, and provide it to the client only in exchange for a valid receipt that passes all of the above checks. This kind of a design thwarts common piracy mechanisms, and — since the validation is performed on the server — allows you to use Apple’s HTTP receipt validation service instead of interpreting the receipt `PKCS #7` / `ASN.1` format yourself.

For more information on this topic, check out the [Futurice blog: Validating in-app purchases in your iOS app][futu-blog-iap].

[futu-blog-iap]: http://futurice.com/blog/validating-in-app-purchases-in-your-ios-app

[reactivecocoa-github]: https://github.com/ReactiveCocoa/ReactiveCocoa