# 手欠升级 `Xcode` `12` 导致 `carthage` 构建只包含 `RxSwift` 的 `Cartfile` 失败

<https://blog.csdn.net/hu_zhenghui/article/details/108744317>

2020年9月16日 `Apple` 发布了 `Xcode` `12`，前一个版本是 `11.7`，秉承了 `Apple` 一贯的新版本坑人的传统，果然安装后各种崩，删减后最小重现代码如下。

<https://github.com/huzhenghui/swift-awesome/blob/master/carthage/RxSwift/justfile>

<https://github.com/huzhenghui/swift-awesome/blob/master/carthage/RxSwift/Cartfile>

`Cartfile` 中只包含一个包

```plain
github "ReactiveX/RxSwift" ~> 5.0
```

初始化为 `macOS` 平台

```bash
carthage bootstrap --no-build --verbose --platform macOS --no-use-binaries
```

编译为 `macOS` 平台

```bash
carthage build --platform macOS --no-use-binaries --verbose
```

编译成功后查看编译结果

```bash
ls -1 ./Carthage/Build/Mac
```

输出为

```plain
RxBlocking.framework
RxBlocking.framework.dSYM
RxCocoa.framework
RxCocoa.framework.dSYM
RxRelay.framework
RxRelay.framework.dSYM
RxSwift.framework
RxSwift.framework.dSYM
RxTest.framework
RxTest.framework.dSYM
```

如果初始化为 `iOS` 平台

```bash
carthage bootstrap --no-build --verbose --platform iOS --no-use-binaries
```

编译为 `iOS` 平台

```bash
carthage build --platform iOS --no-use-binaries --verbose
```

则报错

```plain
/usr/bin/xcrun lipo -create ~/Library/Caches/org.carthage.CarthageKit/DerivedData/12.0_12A7209/RxSwift/5.1.1/Build/Intermediates.noindex/ArchiveIntermediates/RxBlocking/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/RxBlocking.framework/RxBlocking ~/Library/Caches/org.carthage.CarthageKit/DerivedData/12.0_12A7209/RxSwift/5.1.1/Build/Products/Release-iphonesimulator/RxBlocking.framework/RxBlocking -output ./Carthage/Build/iOS/RxBlocking.framework/RxBlocking
```

血泪教训提醒各位，`Apple` 产品要养成仔细看升级说明的习惯，升级越多就越要躲得远远的。

```plain
Xcode 12 includes Swift 5.3 and SDKs for iOS 14, iPadOS 14, tvOS 14, watchOS 7, and macOS Catalina

Platform features
• App Clips are a small part of your app that’s discoverable at the moment it’s needed, loads in seconds, and launches quickly
• WidgetKit uses SwiftUI to build beautiful new widgets that users can install directly on their iPhone home screen
• StoreKit testing framework and transaction manager make it easy to test and debug in-app purchases

Refined user interface
• Document tabs open any type of document in a lightweight editor tab, including logs, asset catalogs, and UI files
• Navigator fonts are now resizable based on the system setting, or can be manually configured
• Code completion has a new, simplified interface that is faster, and makes it easier to choose the correct code
• Organizer is completely redesigned, and reports new app metrics such as hitches in animation and scrolling

Swift and SwiftUI
• Performance for SwiftUI has been improved throughout, and new Lazy views can efficiently handle enormous data sets
• SwiftUI Views can be turned into reusable components that appear in the Xcode library and in code completions
• Swift Package Manager supports resources and localizations, making it great for sharing SwiftUI components
• Swift compiler’s improved diagnostics make it much easier to understand coding mistakes, especially in SwiftUI code
```