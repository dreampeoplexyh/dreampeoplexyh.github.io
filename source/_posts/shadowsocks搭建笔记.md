---
title: shadowsocks搭建笔记
date: 2018-08-14 23:08:00
tags:
---

![](shadowsocks搭建笔记/shadowsocks.png)

<!--more-->
### 简介
```
shadowsocks简介
Centos安装4.17内核
shadowsocks搭建过程
安装BBR谷歌加速
```
### **0x00 shadowsocks简介**
shadowsocks：https://vc2tea.com/whats-shadowsocks/这里面有比较详细的解答  简单来说是一个正向代理，可以欺骗绕过GFW的过滤规则，但是现在流量特征也比较明显，也可能之后被识别成无用吧。


### **0x01 CentOs安装4.17内核**
很多时候那个Centos服务器不是那个内核4.17的，这时候就需要去改变Centos服务器的内核了
确定是否是4.17内核
```
uname -r
```
不是则导入elrepo安装源
```
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
```

安装4.17内核
```
sudo yum --enablerepo=elrepo-kernel install -y kernel-ml
```

确认安装结果
```
rpm -qa | grep kernel-ml
```

列出当前grub2启动菜单的所有项
```
awk -F\' '$1=="menuentry " {print i++ ":" $2}' /etc/grub2.cfg
```

结果实例如下
```
0:CentOS Linux (4.17.0-1.el7.elrepo.x86_64) 7 (Core)
1:CentOS Linux (3.10.0-862.3.2.el7.x86_64) 7 (Core)
2:CentOS Linux (3.10.0-862.2.3.el7.x86_64) 7 (Core)
3:CentOS Linux (0-rescue-6bdeb3da718c4577872532063c9d3ab2) 7 (Core)
```

我们需要将新的内核设置为启动项，在结果中编号为0，那么执行以下命令
```
sudo grub2-set-default 0
```
重启系统
```
systemctl reboot
```


重启完成后确认内核版本
```
uname -r
4.17.0-1.el7.elrepo.x86_64
```

### **0x02 shadowsocks搭建过程**
**方案一：**
```
yum -y install wget

wget -N --no-check-certificate https://raw.githubusercontent.com/ToyoDAdoubi/doubi/master/ssr.sh && chmod +x ssr.sh && bash ssr.sh
```


**提示：全程跟着脚本提示走**


### **0x03 安装BBR谷歌加速**
**方案一**
```
yum -y install wget

wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

chmod +x bbr.sh

./bbr.sh
```

**方案二：**
执行以下命令开启内核BBR功能
```
echo 'net.core.default_qdisc=fq' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_congestion_control=bbr' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

使用以下命令确认BBR是否启用
```
sysctl net.ipv4.tcp_available_congestion_control
```
输出结果应为
```
net.ipv4.tcp_available_congestion_control = bbr cubic reno
```
再输入以下命令
```
sysctl -n net.ipv4.tcp_congestion_control
```
输出结果应为
```
bbr
```
最后确认内核模块是否启用
```
lsmod | grep bbr
```
输出结果应为
```
tcp_bbr                16384  0
```

全剧终  shadowsocks搭建完成

参考连接：
http://www.ebanban.com/?p=562

https://github.com/Alvin9999/new-pac/wiki/%E8%87%AA%E5%BB%BAss%E6%9C%8D%E5%8A%A1%E5%99%A8%E6%95%99%E7%A8%8B

https://github.com/iMeiji/shadowsocks_install/wiki/shadowsocks-optimize

https://github.com/iMeiji/shadowsocks_install/pulse