title: 在Linode服务器上搭建Shadowsocks
date: 2015-09-22 21:15:09
tags:
- 梯子
- 工具
- shadowsocks
categories:
- 工具
---

## 搭建Shadowsocks

>昨晚突然感觉好多东西都无法访问，与其购买VPN等不靠谱的产品不如自己搭建一套，鉴于在前一段Github下载Shadowsocks之前，fork了一下代码，故准备在新加坡的服务器上搭建自己的梯子，以下是简单步骤

1. 启动一个国外的服务器实例，目前大部分都是docker容器，故买了一个linode的$10/m的服务器
2. 实例安装Centos 7.0的系统，并且启动
<!-- more -->
3. 更新软件源，并且安装相关的lib
    ```
    yum update
    yum install epel-release 
    yum update 
    yum install python-setuptools m2crypto supervisor 
    easy_install pip 
    pip install shadowsocks
    ```
4. 配置shadowsock的配置文件，在/etc文件夹下创建配置文件
    ```
    # file name : shadowsocks.json
    # file path : /etc/shadowsocks.json
    vim /etc/shadowsocks.json
    # file content
    {
        "server":"0.0.0.0",
        "server_port":8388,
        "local_port":1080,
        "password":"yourpassword",
        "timeout":600,
        "method":"aes-256-cfb"
    }
    ```
5. 后台运行启动
    ```
    nohup ssserver -c /etc/shadowsocks.json &
    # 然后ctrl + z
    bg 1
    ```
6. 配置客户端进行使用

>使用shadowsocks客户端，服务器端口和客户端端口与配置文件相同
