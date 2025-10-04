---
title: '解决 Clangd 中 OpenCV 头文件报错'
date: 2023-10-26
permalink: /posts/2023/10/clangd-opencv-fix/
lang: zh
ref: clangd-opencv-fix
tags:
  - 瞎捣鼓
---

> 工欲善其事，必先利其器。

### 现象
当光标悬浮到 `opencv2/opencv.hpp` 上的时候，会得到如下报错：
`xmmintrin.h In included file: definition of builtin function '_mm_getcsr'`

虽然这个问题无关痛痒，因为 clangd 关于 cv 的提示仍然可以使用，但是使用 OpenCV 的文件就会被标红,对于我这样的强迫症来说，这是非常难以接受的。

### 解决过程
刚开始查网上资料，网友对此的解释是 gcc 的库 clang 识别不了，大家的解决方式是修改 clangd 配置的，尝试使用 Disagnostics 的 Suppress 功能抑制 builtin_definition 的报错提示，然后对于我的情况而言，这样是行不通的，于是这件事一直被耽搁了。

突然有一天，我发现当 clangd 读不到 compile_commands.json 的时候，根据我 Fallback Flags 的配置
```
-I/opt/ros/noetic/include
-I/usr/include/eigen3
-I/usr/include/pcl-1.10
-I/usr/include/opencv4
```
OpenCV 头文件上的红线报错就消失了，我十分的不解，于是就开始打开 VsCode 的 Output 查看在这个情况下 clangd 的编译命令和我的 compile_commands.json 有何不同。

结果可以发现，默认情况下 clangd 使用的编译器是 /usr/bin/clang，**尽管我甚至没有装 clang，这个目录也是没有任何东西的**，而从 compile_commands.json 中得到的编译器是 /usr/bin/c++。

再仔细研究一下我发现，在使用 /usr/bin/clang 这个虚空编译器的时候，`xmmintrin.h` 被导航到的目录不再是 gcc 下的 include 中，而是 `/usr/lib/llvm-12/lib/clang/12.0.0/include/xmmintrin.h`，即 clangd 的库中。一看，发现这个 clang 和 gcc 下的这个文件有着不同的实现，那大抵能够解释了。

### 解决方案
直接在 clangd.arguments 中，设置 --query_driver=/usr/bin/clang 即可。

此外，为了能够进入 OpenCV 头文件后还能够正常导航，我们需要在 $HOME/.config/clangd/config.yaml 的 CompileFlag 中加入 -I/usr/include/opencv4。

### 思考
很多事情其实通过自己的思考是可以解决的，当某些东西在网上几乎查不到解决方案的时候，不妨试着自己去做做。

clangd 究竟是如何编译文件的呢？为何我使用的虚空编译器也能正常让其运作？
