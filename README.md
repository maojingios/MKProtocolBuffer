# ProtocolBuffer最新操作记录

## 概述

Protocol Buffer（简称PB）是Google出品的一种轻量 & 高效的结构化数据存储格式（详细原理Google下即可）。

## 使用总结

* 相比常用Json和XML，性能更好（数据更小、结构更合理、数据交换更快）；
* 最重要的是使用PB，对开发者设计数据结构的过程有很好的提示规范作用（个人体会😀）。

## Mac下环境搭建

提示：搭建环境前请FQ下（各位懂得！）。

* 安装`HOMEBREW`(已安装跳过)，打开终端输入指令:
`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

* Protocol Buffer 安装包，链接：`https://github.com/google/protobuf/releases` ；

* 主要介绍iOS端，故下载`protobuf-objectivec-3.4.0.tar.gz`（当前最新），将压缩包解压，放在合适的地方；

* 在终端输入，`brew install autoconf automake libtool curl`指令；

* 在终端，`cd`进解压包`protobuf-objectivec-3.4.0`文件；

* 输入`./autogen.sh`,运行脚本（此处需FQ，加载资源）;

* 输入`./configure`,运行脚本;

* 输入`make`,编译;

* 输入`make check`,检查依赖包是否完整，终端会输出7个检查项，都显示pass即可;

* 输入`make install`,安装PB;

* 输入`proto --version`不报错，输出版本信息，即可。

## 生成.h、.m文件

* 使用终端`cd`到存放`.proto`文件的文件夹（事先创建好）；

* 输入`touch Person.proto`命令；

* 输入`vim Person.proto`,按`i`进入编辑状态，按照PB语法设计数据，示例：

		syntax = "proto3";//默认是proto2,二者区别，可查阅PB文档
		
		message Person {
		    string name = 1;
		    string age = 2;
		}

* 按`Esc`，`:wq`回车，在输入`protoc --plugin=/usr/local/bin/protoc-gen-objc Person.proto --objc_out="./"`,如果没有语法或其他异常，请前往文件夹查看生成的`.h`和`.m`文件。

* 在工程`target`->`Build Phrases`->`Compile Sources`->给`Person.pbobjc.m`设置`-fno-objc-arc`.

## 导入工程（手动）

* 在上面`protobuf-objectivec-3.4.0.tar.gz`解压包里面拷贝`objectivec`文件夹下面的全部源码文件，粘贴到工程建中名为`ProtocolBuffer`的文件夹（事先创建）；

*  点按`Xcode`左下角`+`Add a new file ,点击`Add Files To 你的工程名字`，在弹出的文件搜索界面，进入上一步存放PB源码的`ProtocolBuffer`文件夹，选中`ProtocolBuffers_iOS.xcodeproj`并添加至工程（另外一个ProtocolBuffers_OSX.xcodeproj，不用理会，尝试导入，xcode8.3会崩溃）；

* 在工程`target`->`Build Phrases`->`Compile Sources`->`Link Binary With Libraries`添加`libTestSingleSourceBuild.a`静态库

* 在工程`target`->`Build Settings`-> `Rez Search Path` 设置`$(PROJECT)/ProtocolBuffer`

* 在工程`target`->`Build Settings`-> `Header Search Path` 设置`$(PROJECT)/ProtocolBuffer`

## 问题

* 关于`GPBProtocolBuffers_RuntimeSupport.h`等文件找不到的问题？

		刚开始使用cocoapods导入PB到工程，发现每次它导入的版本为1.9.11版本，与下载最新版本PB相比确实少了很多文件，最后便改为手动导入。

* 关于`GPBProtocolBuffers_RuntimeSupport.h`等文件在工程中确实存在，但编译时任然提示找不到文件？

		老问题，参照上面“导入工程”最后两个步骤，保证路径设置正确。

* 关于拖进工程.h和.m文件不支持ARC问题？

		参照上面“生成.h和.m文件”最后一步。
