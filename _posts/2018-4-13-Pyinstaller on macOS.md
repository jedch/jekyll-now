---
layout: post
title: 在macOS系统下用pyinstaller打包PyQt5应用
category: python
---
系统环境：macOS High Sierra 10.13.4，python3.6.4，PyQt5.6.0，PyInstaller3.3.1。请确保系统中已经成功安装上述软件。

## 编辑python代码
在编辑器中输入下列代码，保存为pyqt2app.py。如果直接用`python pyqt2app.py`运行，可以弹出一个Hello world！的窗口。
> import sys  
> from PyQt5.QtWidgets import QApplication,QLabel  
>   
> app = QApplication(sys.argv)  
> w = QLabel("Hello world!")  
> w.show()  
> sys.exit(app.exec_())

## 使用pyinstaller打包
网上macOS下的打包教程很少，而且有些试验不成功。经过测试，下面的两步方法，在我的机器上能够成功打包。

1. pyinstaller -Dyw pyqt2app.py
2. pyinstaller -Fy pyqt2app.spec

在目录dist目录下可以找到pyqt2app.app文件，直接运行即可。至此，整个打包过程完成。需要说明的是，我选择打包成一个单独文件，如果你有其它需求，看看说明书吧。

## 附录：pyinstaller参数及用法
### 参数
* -F　　　　制作独立的可执行程序
* -D　　　　制作出的档案存放在同一个文件夹下（默认值）
* -K　　　　包含TCL/TK（对于使用了TK的，最好加上这个选项，否则在未安装TK的电脑上无法运行）
* -w　　　  制作窗口程序
* -c　　　　制作命令行程序（默认）
* -X　　　　制作使用UPX压缩过的可执行程序（推荐使用这个选项，需要下载UPX包，解压后upx.exe放在Python(非PyInstaller)安装目录下，下载upx308w.zip）
* -o DIR　　指定输出SPEC文件路径（这也决定了最后输出的exe文件路径）
* --icon=[ICO文件路径] 指定程序图标
* -v [指定文件] 指定程序版本信息
* -n [指定程序名] 指定程序名称

### 用法示例
留空