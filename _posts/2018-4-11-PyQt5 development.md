---
layout: post
title: PyQt5开发入门
category: python
---
系统环境：macOS High Sierra 10.13.4，python3.6.4，PyQt5.6.0， QtDesigner5.10.1。请确保系统中已经成功安装上述软件。

## 创建界面文件ui
使用Qt设计师，按照自己的需要设计界面。在合适的位置添加Label，Button，Lineedit等控件，根据需要使用Grid或水平或竖直等布局，排列好各个控件。编辑伙伴关系，编辑tab顺序。最后保存为dialog.ui文件。

## 把ui文件转换为py文件
在终端输入下列命令，把ui文件转换为python可以直接调用的py文件。
> pyuic5 -o ui_dialog.py dialog.ui

## 调用转换的py文件
在main.py文件中调用刚生成的ui_dialog.py文件。（markdown中代码怎么缩进？）

> from PyQt5.QtWidgets import (QApplication,QDialog)  
> import ui_dialog.py  # <---调用
> 
> class ThreeStep(QDialog,ui_hesanDlg.Ui_Dialog):  
>     def __init__(self,parent=None):  
>         super(ThreeStep,self).__init__(parent)  
>         self.setupUi(self)  
> 
> if __name__ == '__main__':  
>     import sys  
>     app = QApplication(sys.argv)  
>     form = ThreeStep()  
>     form.show()  
>     app.exec_()