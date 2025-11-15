---
title: "nixpkgs中stdenv的bootstrap过程"
date: 2025-11-14T09:22:11+08:00
draft: true
---


# 学习nixpkgs bootstrap
这篇文章主要记录学习[nixpkgs bootstrap intro](https://trofi.github.io/posts/240-nixpkgs-bootstrap-intro.html)这篇文章的过程，如果其中有涉及到其他博客内容，我会将相关链接贴到文章最后。


其实我对c编译器自举这个事情本来就比较感兴趣，由于c/c++的工具链相对古老，而且没有语言的包管理，语言生态都是由不同的小工具东拼西凑出来的。所以想要从头构建出一个C/C++编译器相对困难很多，但是这也是不可避免的事情。
毕竟C/C++历史悠久，即使像是go这种原生完全不依赖C生态，连标准库都不依赖libc的语言，也会有CGO的存在来复用C/C++的库。所以说当今时代的计算机领域，如果想要构建出一套自己的“软件集合”，一直向上追溯的话，GCC+glibc似乎就是一切的根源，当然clang/musl也是一种选择，但那就不是一篇文章能够覆盖的内容量了。

## Overview

nixpkgs是现在事实上最大的单一软件仓库（2025），可以很简单的复用已有软件。也得益于nix强制要求构建时声明构建依赖，并且自动解算运行时依赖，我们可以使用工具简单的获得特定软件包的构建和运行时依赖树：
``` bash
$ nixgraph --depth=99 --out gcc_runtime_deps.png 'nixpkgs#stdenv.cc'
$ nixgraph --depth=99 --out gcc_buildtime_deps.png --buildtime 'nixpkgs#stdenv.cc'
```
很好，运行时依赖看起来非常的合理：
![运行时依赖](gcc_runtime_deps.svg)


然而下图是gcc的构建依赖关系(希望你有一些心理准备)：

[构建依赖](gcc_buildtime_deps.svg)

# stdenv
首先我们要了解一下stdenv，这个概念在nixpkgs至关重要。stdenv可以看作cc
![stage1-gcc](stage1-gcc.svg)
![stage2-gcc](stage2-gcc.svg)

![stage3-gcc](stage3-gcc.svg)
![stage4-gcc](stage4-gcc.svg)
![stdenv-gcc](final-gcc.svg)


# TO BE CONTINUE
