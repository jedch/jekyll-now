---
layout: post
title: Python3常用第三方库简介
category: python
---
由于工作中使用的个别库依赖python2，目前没有升级到python3，所以我一直没有转到python3。但是迟早是要转的，所以下了很大的决心，以后主要使用python3吧。大部分第三方库对2和3都是支持的，下面其实算是一个个人使用或者感兴趣的库的集合。

## anaconda
这个准确说不应该叫做一个库，它实际上包含了常用的第三方库的一个集合。主要有numpy, scipy, pandas, pyqt5等等。以后需要安装第三方库，使用conda install命令就可以安装了。conda能安装的库与pip能安装的库大部分是重叠的，但是有少数只是支持其中一方，正好互为补充了。国内大多用清华的源，但是2019年4月左右，清华因为授权问题关闭了anaconda源。当时换清华源的时候，页面有很详细的说明，按照指令操作就能成功，如今要恢复官方源，却不提供任何说明，小白表示清华也这么幽默哈。其实一个命令就可以消除清华源而恢复用官方源了：conda config --remove-key channels。另外，需要添加官方的一些channels，如conda-forge,msys2,bioconda,menpo,pytorch等等，命令是conda config --add channels conda-forge，其它类似。

## pyenv
版本管理的。
## ConfigParser
读取配置文件。
