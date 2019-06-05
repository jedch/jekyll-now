---
layout: post
title: Home Assistant智能家居结合Node RED的实现
category: python
---
参考[HA官方网站](https://www.home-assistant.io)和[瀚思论坛](https://bbs.hassbian.com/forum.php)，在树莓派上安装了Home Assistant。当时没有记录，现在基本算是回忆录了，不过老年人的回忆不可靠啊。希望下次重装可以回来修补。

1. 简介。Home Assistant（后面简称HA）和Node RED都可以独立作为智能家居的集成平台，前者由python实现，后者是基于node.js的，而这是一个服务器端的JavaScript解释器。嗯，有关JavaScript的说法我是网上抄的，概念太多真不懂。我略微有一点python基础，当然倾向于使用HA了，但是瀚思论坛上有大佬极力推荐Node RED，据说它能简便的接入更多的传感器，能接入PLC，而且可以把它集成到HA中。对HA控制PLC我是有一点憧憬的，而且我相信大佬们的选择。所以就有了我记录这篇文章的背景了。
2. HA的安装参考官方网站，英文的，慢慢啃。我手头有一个树莓派3B（不是最新，目前2019年6月5日最新的是3B+），这个是在官方支持的硬件列表里的。官方[Installation](https://www.home-assistant.io/docs/installation/)页面提到了多种硬件和三种推荐安装方法，就是Hass.io，Docker和Hassbian三种。从简便程度和有常规操作系统这两个角度，我没有选择上面三种方法，我在树莓派上已经安装了Raspbian，这是一种以Debian为基础的简化版Linux发行版，挺适合树莓派。那么现在要做的就是在一个通用的Linux发行版上安装HA了。实际安装步骤可以参考[Advanced Guide](https://www.home-assistant.io/docs/installation/raspberry-pi/)，我是本地机器连接显示器及键鼠安装，所有就没有使用ssh。我不太喜欢虚拟环境，所以没有完全遵照上面的网页，但是我到底干了什么，忘记了。最后装好了。提示符前面不带homeassistan的，确实没有虚拟环境。可能我是用pip安装的HA？存疑吧。
3. Node RED的安装遵照官方网站的[Running on Raspberry Pi](https://nodered.org/docs/hardware/raspberrypi)页面就可以，实际上保持联网，在命令行下执行了一个命令就搞定`bash <(curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered)`。接着找到HA的配置文件，我的在/usr/share的一个目录下，见鬼。sudo nano configuration.yaml，在文件最后添加几行`panel_iframe:``node_red:``title:'node_red'``icon:'mdi:vector-polyline'``url:'http://localhost:1880'`，不要从原始网站直接复制，要复制纯文本。之后重启树莓派，打开浏览器，输入`localhost:8123`就可以打开HA了。后面剩下的就是添加传感器，配置参数了。修行吧。