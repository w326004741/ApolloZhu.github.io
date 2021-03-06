---
title: 在 Swift 工程中使用 OpenGL 框架 GLFW 的配置过程
date: 2016-11-14 11:08:55
tags:
- tips
- macOS
- Swift
categories:
- 瞎捣鼓
---

苹果丢到了 OpenGL 全面推 Metal，但我当初想要学 OpenGL，找到了一个使用 Swift 语言的教程 —— [SwiftGL](https://github.com/SwiftGL)，主要内容是使用 Swift 3 在 macOS Sierra (10.12) 上开发基于 glfw3 的 GUI 程序（虽然是通过命令行调起的）。可惜最后还是没能坚持下来，但当时摸爬滚打，翻了不少博客，甚至还差点把 SwiftGL 的库用 Swift 3 重写，最后才研究出来的出来的配置方法丢了就太可惜了，特此记录分享。

<!-- more -->

## 编译 glfw

- 下载 [GitHub 上的 glfw](https://codeload.github.com/glfw/glfw/zip/master)，解压
- 打开 [CMake](https://cmake.org/) App 准备编译
- *Browse Source* 选择 `glfw-master`
- *Browse Build* 随便选择一个临时的文件夹
- 点击 *Configure*, 从 *generator* 的下拉列表中的选择 *Xcode*，点击 *Done*
- CMake 处理完（Configuring done）之后再点一遍 *Configure*
- 点击 *Generate*，点击 *Open Project*
- Xcode 中把 Scheme 换为 `install`，编译并运行

## 配置工程

完成之后，打开自己的工程文件。

{% note warning %}
以下操作每个工程只需要 ***一次***。加入的文件可以在 Target 之间共享。
{% endnote %}

- 打开 Finder, `Shift`+`Command`+`G`，输入 `/usr/local/lib/libglfw3.a`，确认
- 把文件拖进工程
	- copy items if needed
	- create groups
	- 添加

{% note danger %}
针对每个工程中每个需要 `glfw` 的 Target，***都需要*** 完成下面的步骤。
{% endnote %}

- 切换到 *Build Settings*，搜索 `sea`
- 把 `/usr/local/include` 添加到 *Header Search Paths*
- 把 `/usr/local/lib` 添加到 *Library Search Paths*
- 如果没有桥接头文件（Bridging Header），创建一个
- 把以下内容添加到桥接头文件中

```C++
#include <GLFW/glfw3.h>
#include <GLFW/glfw3native.h>
#include <GLKit/GLKit.h>
```

{% note success %}
`glfw` 配置完毕，享受你 Swifty 的 OpenGL 之旅吧。
{% endnote %}
