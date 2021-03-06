# PLPlayerKit

PLPlayerKit 是为 **pili流媒体云服务** 提供的一套播放直播流的 SDK, 旨在解决 iOS 端快速、轻松实现 iOS 设备播放直播流，便于 **pili流媒体云服务** 的开发者专注于产品业务本身，而不必在技术细节上花费不必要的时间。


## 内容摘要

- [1 快速开始](#1-快速开始)
	- [1.1 配置工程](#1.1-配置工程)
	- [1.2 示例代码](#1.2-示例代码)
- [2 第三方库](#2-第三方库)
- [3 系统要求](#3-系统要求)
- [4 版本历史](#4-版本历史)

## 1 快速开始

### 1.1 配置工程

- 配置你的 Podfile 文件，添加如下配置信息

```
pod 'PLPlayerKit'
```

- 安装 CocoaPods 依赖

```
pod update
```
or
```
pod install
```

- Done! 运行你工程的 workspace

### 1.2 示例代码

#### 1.2.1 可定制的播放控件

在需要的地方添加

```Objective-C
#import <PLPlayerKit/PLPlayerKit.h>
```

参数配置

```Objective-C
	NSMutableDictionary *parameters = [@{} mutableCopy];
	
	// 对于 iPhone 建议关闭逐行扫描，默认是开启的
	if (UI_USER_INTERFACE_IDIOM() == UIUserInterfaceIdiomPhone) {
		parameters[PLMovieParameterDisableDeinterlacing] = @(YES);
	}
	
	// 当业务需要在播放器初始化完成后自动开始播放，可以在初始化参数中设定
	parameters[PLMovidParameterAutoPlayEnable] = @(YES);
```

初始化

```Objective-C
	// 初始化 PLVideoPlayerController
	PLVideoPlayerController *playerController = [PLVideoPlayerController videoPlayerControllerWithContentURL:url parameters:parameters];
	
	// 获取到播放界面
	UIView *playerView = playerController.playerView;
```

开始／暂停操作

```Objective-C
	// 播放
	[playerController play];
	
	// 停止
	[playerController pause];
```

播放器状态获取

```
// 实现 <PLVideoPlayerControllerDelegate> 来控制流状态的变更
- (void)videoPlayerController:(PLVideoPlayerController *)controller playerStateDidChange:(PLVideoPlayerState)state {
	// 这里会返回流的各种状态，你可以根据状态做 UI 定制及各类其他业务操作
}

- (void)videoPlayerControllerDecoderHasBeenReady:(PLVideoPlayerController *)controller {
	// 解码器初始化完成, 与 videoPlayerController:playerStateDidChange: 方法中返回 PLVideoPlayerStateReady 状态属于同一情况，你可以紧紧实现 videoPlayerController:playerStateDidChange: 方法
}

- (void)videoPlayerController:(PLVideoPlayerController *)playerController failureWithError:(NSError *)error {
	// 当出现错误时，你会在这里收到回调，暂且只有解码器初始化错误会返回
}

- (void)videoPlayerController:(PLVideoPlayerController *)playerController positionDidChange:(NSTimeInterval)position {
	// 视频进度变更时都会触发这个回调
}

```

#### 1.2.2 配置参数

```
// 逐行扫描
PLMovieParameterDisableDeinterlacing

// 最小缓存时长
PLMovieParameterMinBufferedDuration

// 最大缓存时长
PLMovieParameterMaxBufferedDuration

// Player contentMode
PLMovieParameterFrameViewContentMode

// 初始化后是否自动开始播放
PLMovidParameterAutoPlayEnable
```

## 2 包含的第三方库

- ffmpeg

## 3 系统要求

- iOS Target : >= iOS 6

## 4 版本历史

- 1.2.9 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.9.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.9.md))
	- 修复进入后台后崩溃的问题
	- 更新 example 中 player 代码，支持横竖屏旋转操作
- 1.2.8 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.8.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.8.md))
	- 添加播放进度回调方法
	- 修复 seekTo 后流状态不正确的问题
- 1.2.7 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.7.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.7.md))
	- 添加播放器状态属性
	- 添加解码器初始化完成后回调
	- 添加播放器状态回调
	- 添加初始化后自动播放参数
- 1.2.6 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.6.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.6.md))
	- 添加设置播放位置的操作
	- 添加了快进、快退的操作
	- 添加总播放时长的属性
	- 添加获取音量的属性
	- 添加获取当前播放位置的属性
	- 添加静音操作
- 1.2.5 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.5.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.5.md))
	- 修复与部分其他库头文件冲突的问题
- 1.2.4 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.4.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.4.md))
	- 添加了 ```PLMovieParameterFrameViewContentMode``` 参数
	- 修复与部分其他库头文件冲突的问题
	- 修复 Player contentMode 无法更改的问题
- 1.2.3 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.3.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.3.md))
	- 修复初始化占用主线程导致卡顿的问题
	- 修复错误回调无效的问题
- 1.2.2 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.2.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.2.md))
	- 修复 lib 未更新导致的 crash
- 1.2.1 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.1.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.1.md))
	- 添加 failue 情况下的回调，返回 NSError 对象
	- 移除 PLVideoPlayerViewController，请直接使用 PLVideoPlayerController 进行定制
- 1.2.0 ([Release Notes](https://github.com/pili-io/PLPlayerKit/blob/master/ReleaseNotes/release-notes-1.2.0.md) && [API Diffs](https://github.com/pili-io/PLPlayerKit/blob/master/APIDiffs/api-diffs-1.2.0.md))
	- 极大缩小 lib 大小
	- 增加可定制的播放控件 PLVideoPlayerController
- 1.1.2
	- 拆分 Flat lib
	- 添加了 x86_64 支持，便于在 iPhone 6 Plus 模拟器下调试使用
- 1.1.1
	- 对库引用做了些修改
- 1.1.0
	- 发布 CocoaPods 版本