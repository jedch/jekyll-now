---
layout: post
title: CentOS7安装与配置
category: python
---
近日在Ubuntu下编译一个程序，被库的依赖问题弄得焦头烂额。转头找centos解决问题吧。虽然我已经很习惯很喜欢u，但是别人的程序是在c下开发的。希望转用centos能让许多依赖问题迎刃而解吧。

1. 在MacOS下准备CentOS7安装U盘。下载centos镜像，命名为c7.iso。插入u盘，用系统磁盘工具，抹掉，选Mac日志。命令显示u盘号，diskutil list，一般为/dev/disk2。卸载u盘，diskutil unmountDisk /dev/disk2。用dd命令复制，sudo dd if=c7.iso of=/dev/rdisk2 bs=10m。这条命令是对的，而且复制很快。最后出现records in，records out，就完成了。

2. 安装CentOS。启动需要安装的电脑，修改bios设置，让usb hdd在硬盘之前启动。插入上面制作号的U盘，F10保存配置，重启。进入界面，选择install。后面的配置简单，需要注意的是有一步在左侧选择gnome，在右侧勾选对应的模块。分区先用自动，复选回收，点完成，全部删除，回收空间。再手动，选标准。装好重启，注意选上含pinyin字样的中文。

3. 配置系统。CentOs的有些版本不自动连接有线，很大的坑，可以更改设置。用轮换系统语言中英文的方法，设置文件夹名为英文，如果有什么问题，查看/etc/xdg文件夹。到清华mirror按照提示添加centos，epel源。yum安装dash to dock插件，可能需要重启，通过“应用程序”-“优化”-“扩展”-Dash to dock打开启用。用yumex搜索安装no topleft hot corner插件，重启，像上面一样启用。安装cmake3，及gui。与win系统时间冲突问题，sudo gedit /etc/adjtime，把第三行UTC改为LOCAL，保存重启，修改系统时间为正确的当地时间。开发的话，系统自带g++版本太低，升级吧，下载gcc-8.3.0.tar.bz2，解压tar jxvf gcc-8.3.0.tar.bz2，安装yum install gmp-devel mpfr-devel libmpc-devel，建立编译目录mkdir gcc-5.4.0-build，进入cd gcc-5.4.0-build，配置../gcc-5.4.0/configure --enable-languages=c,c++,fortran --disable-multilib，编译安装make && make install，配置在/et/profile中加入两行export PATH=/usr/local/bin:$PATH和export LD_LIBRARY_PATH=/usr/local/lib64:$LD_LIBRARY_PATH，可能需要重启。
4. 文件系统支持。确保已经安装过EPEL库，再在终端安装Nux Dextop库rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm，更新源。用yum安装ntfs-3g，fuse-exfat，exfat-utils。重启系统。

5. ROOT6在CentOS下可以直接从yum安装二进制。不想折腾的本条后面都不用看了。折腾的继续。命令行一键搞定依赖库，sudo yum install git cmake gcc-c++ gcc binutils libX11-devel libXpm-devel libXft-devel libXext-devel gcc-gfortran openssl-devel pcre-devel mesa-libGL-devel mesa-libGLU-devel glew-devel ftgl-devel mysql-devel fftw-devel cfitsio-devel graphviz-devel avahi-compat-libdns_sd-devel libldap-dev python-devel libxml2-devel gsl-static。清理软件包的过程有点长，耐心等待。最后下载root6的源代码编译安装。为什么要折腾呢？

6. Geant4的库。三类安装方法。第一，yum安装：。第二，cmake配置，make，make install三部曲安装下列文件：。第三，直接make，make install安装的文件：。