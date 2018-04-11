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