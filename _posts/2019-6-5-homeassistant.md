---
layout: post
title: Home Assistant智能家居结合Node RED的实现
category: python
---
参考[HA官方网站](https://www.home-assistant.io)，[瀚思论坛](https://bbs.hassbian.com/forum.php)和[HAChina](https://www.hachina.io/)网站，在树莓派上安装了Home Assistant。

1. 下载[Hassbian镜像](https://github.com/home-assistant/pi-gen/releases/latest)。用balenaEtcher程序将img镜像(不建议直接使用zip文件)写入TF卡，官方建议卡容量32GB以上。使用网线连接网络。将TF卡插入树莓派3B，通电开机，开始安装Home Assistant。官方说安装需要约10分钟，安装完毕会提示登录。默认用户名pi，密码raspberry，最好登录后用passwd修改rasp4。但是我实际操作发现，很快（分分钟）就提示登录，然而安装并未完成，需要等待足够长时间（半小时？）。用同一个局域网的电脑浏览器打开hassbian.local:8123，可以看到homeassistant（以下简称HA）的登录页面，至此算安装成功了。如果您发现在30分钟左右后无法访问该网页，请检查您是否有文件/home/homeassistant/.homeassistant/，如果此位置没有文件，则使用此命令手动运行安装程序：sudo systemctl start install_homeassistant.service。页面提示创建HA用户，随意，如ji13，密码如fa14。简单设置，即可进入欢迎页面，页面很简洁，只有用户名，天气，日出日落等。剩下的工作就是个性化配置和添加组件的事情了。以后一般操作都用ssh登录，当然，这与在树莓派上直接操作本质上没有区别，只是这样可以省略一个显示器和一套键盘鼠标。

2. 添加小米智能插座。小米的插座首先需要查找产品的token，网上有用miio软件查看等诸多方法，但是根据试验最靠谱的是在手机上安装米家5.4.54版本，并关闭自动更新，以后也不要手动更新这个软件了。按照米家的正常操作，添加插座成功后再退出。查看手机/sdcard/SmartHome/logs/Plub_Devicemanager/文件夹，打开yyyy-mm-dd.txt的日志文件，搜索关键字“插座”，可以找到32位token，对应ip地址，记录下来备用。如果以后网络变更，需要重新连接查找token。进入树莓派/home/homeassistant/.homeassistant目录，运行sudo nano configuration.yaml添加上面获取的ip和token信息。

```python
    switch:
      - platform: xiaomi_miio
        host: 192.168.8.110
        token: f1f23da4d7e2e39c98a1d38da3010abc
```
运行harestart重启服务，即可使用插座了。顺便提一句，hassbian系统中已经配置好一些可用的命令，如hastart,hastop, hastatus, halog, hashell。

3. 添加小米体重秤。与添加插座优点不同，这里需要的是蓝牙设备的MAC地址，按压打开体重秤，在树莓派中运行sudo hcitool lescan，即可扫描到体重秤地址，而且这个地址永远不变（惊不惊喜？）。安装mosquitto套件吧，sudo hassbian-config install mosquitto，用户名mosq，密码mos4。嗯，安装完后顺手升级一下hass工具本身sudo hassbian-config upgrade hassbian-script。根据提示知道MQTT运行地址hassbian.local:1883，记住，以后配置要用。添加自动启动，sudo systemctl enable mosquitto.service。如果发现地址占用错误，用ps查看mosquitto进程，kill就可以启动了。HA配置文件添加

```python
    mqtt:
        broker: 192.168.8.106
        port: 1883
        username: mosq
        password: mos4
```

安装bluepy：先安装必备环境sudo apt-get install python3-pip libglib2.0-dev，接着sudo pip3 install bluepy，sudo pip3 install paho-mqtt成功。还有一个问题，局域网机器上浏览器无法打开hassbian.local:1883，在服务器端（即树莓派）显示socket error on client错误。怎么解决？？？在瀚思论坛下载ABC大神的miscalegw.py，复制到/usr/share/bin。修改文件/etc/crontab，把下面这行加到文件末尾*/01 * * * * root python3 /usr/local/bin/miscalegw.py，意味没分钟运行这个命令，当然可以修改01为其它，如30表示三十分钟运行一次。修改HA配置文件，加入

```python
    sensor:
      - platform: mqtt
        name: "miscale"
        state_topic: "miscale/weight/kg"
        value_template: "{{ value }}"
        unit_of_measurement: "kg"
        
```
嗯，还有自动化播报，等以后研究。重启HA，可以看到体重秤，但是没有数字显示，估计是miscalegw.py程序本身问题。两个星期，总算把体重秤连接上了。


4. 简介。Home Assistant（后面简称HA）和Node RED都可以独立作为智能家居的集成平台，前者由python实现，后者是基于node.js的，而这是一个服务器端的JavaScript解释器。嗯，有关JavaScript的说法我是网上抄的，概念太多真不懂。我略微有一点python基础，当然倾向于使用HA了，但是瀚思论坛上有大佬极力推荐Node RED，据说它能简便的接入更多的传感器，能接入PLC，而且可以把它集成到HA中。对HA控制PLC我是有一点憧憬的，而且我相信大佬们的选择。所以就有了我记录这篇文章的背景了。
5. Node RED的安装遵照官方网站的[Running on Raspberry Pi](https://nodered.org/docs/hardware/raspberrypi)页面就可以，实际上保持联网，在命令行下执行了一个命令就搞定`bash <(curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered)`。接着找到HA的配置文件，我的在/usr/share的一个目录下，见鬼。sudo nano configuration.yaml，在文件最后添加几行`panel_iframe:``node_red:``title:'node_red'``icon:'mdi:vector-polyline'``url:'http://localhost:1880'`，不要从原始网站直接复制，要复制纯文本。之后重启树莓派，打开浏览器，输入`localhost:8123`就可以打开HA了。后面剩下的就是添加传感器，配置参数了。修行吧。