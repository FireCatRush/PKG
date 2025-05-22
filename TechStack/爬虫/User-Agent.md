# 什么是UserAgent

UserAgent 又称[使用者代理](https://zh.wikipedia.org/zh-tw/%E7%94%A8%E6%88%B7%E4%BB%A3%E7%90%86)，是网站与伺服器用来辨识使用者行为的标记。所谓的使用者代理，意思是我们所操作的电脑、手机等装置，以及上面用来浏览网页的浏览器，其实是代理我们的身分去呼叫网站的终端程式，所以这些工具被称为「代理」 。

当使用者浏览网站时，浏览器(也就是这个代理) 会根据当下的装置类型、版本号、作业系统等资讯，产生一个UserAgent 字串，每次浏览网页时，就会带着这个字串以`User-Agent`Header 的形式发送给伺服器，让伺服器可以得知现在这个人使用的装置的各类资讯。

发送出来的Header 可能长这样：
```
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.0.0 Safari/537.36
```
其中，从里面可以看到几个资讯
- **Macintosh** : 表示这是Mac 电脑
- **Mac OS X 10_15_7** : 表示作业系统为OSX 10.15.7
- **Intel Mac** : 表示是Intel 晶片的Mac
- **AppleWebKit** : 浏览器核心使用KHTML / WebKit
- **Chrome/127.0.0.0** : 浏览器是Chrome 的127 版

User Agent中文名为用户代理，简称 UA，它是一个特殊字符串头，使得服务器能够识别客户使用的操作系统及版本、CPU 类型、浏览器及版本、浏览器渲染引擎、浏览器语言、浏览器插件等。

常常要用server抓资料时，都会碰到直接使用wget和curl被服务器拒绝的状况。通常简单加个user-agent伪装一下就会过了。
# UserAgent 的用途

UserAgent 既然可以辨识使用者的装置类型与版本，那用途其实就非常广，常见的有

- 依照装置或浏览器提供不同功能
- 辨识装置是电脑、平板还是手机，显示不同版面
- 辨识是否是爬虫，是否阻挡
- 了解使用者的操作系统与浏览器，协助软体除错用
- 分析使用者来源与行为，还有装置与作业系统的市占率...等等

在软体研发上站了非常重要的地位。因为少了UserAgent，就很难掌握使用者的轮廓与装置类型，开发与除错也会变得非常的困难。


# 浏览器查看UserAgent的方法
1、通过JS事件来查询
在浏览器地址栏中输入以下代码：
```javascript
javascript:alert(navigator.userAgent)
```

2、如果您用的是Chrome谷歌浏览器，还可以在地址栏中输入：
```
about:version
```

查询到更详细的用户代理（UserAgent）信息。还包括浏览器版本、WebKit内核版本。

3、还有一种比较麻烦的用chrome开发者模式查询的方法：
- 首先打开chrome开发者模式，在网页上右键，选择检查（快捷键：Ctrl+Shift+I）
- 在Network中找到对应Name下的网站名，在Headers中最后可以看到user-agent
- 这个方法的好处是你可以找到不同设备的user-agent，因为chrome开发者模式可以模拟不同设备，点击左上角图标，然后在这边可以选择模拟的设备，这时再访问网页就是以iPhoneX的user-agent来访问的