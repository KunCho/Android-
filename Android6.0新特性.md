# Android 6.0 中新的新技术

### 权限管理
权限管理是 Android M 最大的改变，权限管理更加精细，并且由以前的安装时静态授权，改为现在的运行时动态授权。大家对 Android 的权限吐槽已久，Android 应该能极大的改善这方面的问题。主要改变有：

- 系统设置中可以对 APP 各个权限单独控制
- 权限根据内容进行分组了
- 普通权限还是在安装时授权
- 其他权限在运行时系统弹窗授权，并且要解析使用这个权限的目的

### APP Linking
这是一个把 APP 和网页直接打通的技术，能够让 APP 能够直接来处理你的网站普通的 URL 链接，来展示你对应的网站内容。这绝对是一个值得关注的改进，Web 和 APP 之间缝隙将越来越小。这对既有网站又有 APP 的应用来说非常有利，例如知乎和淘宝等。

有点类似于之前的 APP 的 Deep link，可以通过特殊的 Schema 也可以让 APP 直接打开对应的内容。APP Linking 的特点是，只要使用传统的 URL 就可以，而且是根据 URL 的域名对应特定的 APP 的。

开发者需要做的是在 AndroidManifest.xml 做一下对应的声明即可。如果需要让系统默认用你的 APP 打开对应的 URL 的话，还需要网站配合提供 assetlinks.json。

[Android M 的＂App Links＂实现详解](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0609/3022.html) 

[Android M App Links: 实现, 缺陷以及解决办法。](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0718/3200.html)   

### APP 数据自动备份
支持 APP 数据自动备份到 Google Drive，在你换手机的时候，能够直接把 APP 的数据恢复到你的手机上，你还可以配置些数据那些数据需要或者不需要备份。而且不用写任何代码就自动实现了。

然而这些对国内的开发者来说，并没有什么用。国内厂商的 ROM 有些已经有或者准备会跟上，到时候都能享受到这样的便利。

### 指纹解锁
Android 官方支持指纹认证，可以用在解锁，或者任何需要验证用户的地方，例如支付。提供了新的 API [FingerprintManager](http://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html)，让第三方 APP 来用来获得指纹认证的功能。具体使用方法可以参考这个[实例](https://github.com/googlesamples/android-FingerprintDialog)。

### 直接分享
直接分享是在 APP 内直接弹出一个选择分享到其他应用的中的对象的列表，中间省略了选择需要分享的 APP，选择“联系人”之类的操作。Android 中分享已经做得很好了，这里又更进一步简化了分享操作。如果要让你的 APP 支持被直接分享，需要实现一个 [ChooserTargetService](http://developer.android.com/reference/android/service/chooser/ChooserTargetService.html)，并且实现对应的处理分享 Intent 的 Activity。具体使用可以参考[这里](http://stackoverflow.com/a/30721038/1260562)。

### 支持蓝牙触控笔
系统内置支持蓝牙触控笔，这样 Android 系统就默认支持高大上的触控笔了。并提供了 API 让你的 APP 来响应触控笔事件。

### 低功耗蓝牙扫描优化
优化了低功耗蓝牙扫描优化的扫描。现在低功耗蓝牙的应用越来越多，很多 APP 都需要扫描设备，扫描设备是一个非常重的操作，希望这次改动，能够带来一些改善。
### 支持主题化的 ColorStateLists
使用 context.getColorStateList(int id) 来获取当前主题对应的 ColorStateLists。
### 相机 API
提供 API setTorchMode()来直接开关闪光灯，并且可以监听闪光灯的开光状态，以前很多 APP 已经支持用闪光灯来做手电筒，现在官方提供 API 来做这样的事情了。

从 Android 5.0 开始，就提供了一套全新的相机 API Camera2，这里在此基础上添加了处理相关 API。
### 其他
- 有语音交互 API
-  Hotspot 2.0 支持
-  4k 屏支持
-  [语音](http://developer.android.com/preview/api-overview.html#audio)和[视频](http://developer.android.com/preview/api-overview.html#video) API 的改进
-  Android 企业用户特性，例如多用户支持，静默安装等

