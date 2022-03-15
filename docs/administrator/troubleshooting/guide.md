---
sidebar_position: 1
slug: /troubleshooting
---

# 指南

#### SFTP无法登录？

检查账号和密码是正确，请保证[服务器安全组](/zh/tech-instance.md)的22端口是开启的

#### Windows远程桌面连接失败？

检查账号和密码是正确，请保证[服务器安全组](/zh/tech-instance.md)的3389端口是开启的，下图是排查方法  
![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/aliyun/aliyun-guzhangpaichu.png)

#### 服务器无法重启？

请联系云平台官方修复

#### *http://服务器公网IP* 无法打开软件的初始化界面？

最常见的原因如下：

* 服务器[安全组**80** 端口](/zh/tech-instance.md)没有开启
* 你所安装的镜像不支持此类访问
* 安装的不是目标镜像
* 你的服务器网络故障
* 产品本身的故障导致
* 其他

不管哪种原因，一旦无法访问，请第一时刻联系我们[人工支持](https://support.websoft9.com/zh/contact.html)

#### 已经替换了默认目录的文件，仍然指向Websoft9演示页面？

请清空浏览器缓存 或 换一个浏览器测试

#### 域名解析迟迟没有生效？

解析生效之后，本地访问可能由于缓存问题导致仍然没有生效，请清空浏览器缓存和DNS缓存

#### phpMyAdmin 出现 Error during session...错误？

错误原因：PHP 的 session.save_path 路径目录的权限设置不正确。  
解决方案：打开WinSCP，运行如下命令即可  
~~~
chown -R root:nginx /var/lib/php/session
echo 'chown nginx. -R /var/lib/php' >> /etc/cron.daily/0yum-daily.cron
~~~

#### Nginx出现502错误

Nginx应用服务器出现502错误的原因很多，但是基本都是资源不够造成的。 包括：内存不足，CPU超标，硬盘满了，另外可能也有程序导致php-fpm停止运行。对应的的解决办法：

*   内存和CPU超标，通过重启一下php-fpm 和nginx mysql 三个服务可以临时解决，如果是1核1g的配置且经常出现502错误的话，建议升级
*   硬盘满了的话，会导致MySQL停止服务，需要进行硬盘扩容
*   php-fpm服务停止或者报错也会出现502，需要重启php-fpm

#### 网站速度很慢？

带宽不足以及服务器满负荷运转是最常见的原因，详细分析参考[此处](/zh/#网站访问很慢？)。

#### 连接SFTP，出现Disconnected...publickey

错误原因：选用的是密钥对作为（与root账号密码有区别）登录凭证，而密钥对如果每日有下载到本地是无法在WinSCP等工具中直接使用的

解决方案：设置WinSCP为秘钥对登录 或 云控制台更改登录凭证方式

#### HTTP故障代码有哪些？

如果某项请求发送到您的服务器要求显示您网站上的某个网页（例如用户通过浏览器访问您的网页或 Googlebot 抓取网页时），WEB服务器将会返回 HTTP 状态码响应请求。
此状态代码提供关于请求状态的信息， 告诉 Googlebot 关于您的网站和请求的网页的信息。
一些常见的状态代码:
```
200 服务器成功返回内容
301/2 永久/临时重定向
304 未修改 Not Modified
307 重定向中保留原始数据
404 请求的页面不存在
500 服务器内部错误
503 服务器暂时不可用
```
您也可以访问 [W3C 页面](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) 获取更多HTTP 状态码信息的完整列表。


#### 升级或安装扩展时网络超时？

参考[此处](/zh/#升级或安装扩展时网络超时？)

#### 如何判断端口是否放通？
除了检查本机防火墙和云控制台安全组之外，可以通过 **telnet** 去连接

#### root账户修改文件权限， 报错 "Operation not permitted"，不允许操作
使用root修改文件权限，如下。查看文件属性也不是 i 属性导致。

```
[root@iZ2ze3eh720u9x5c1hdhwyZ yanbian]# chmod 750 index.php
chmod: changing permissions of ‘index.php’: Operation not permitted
[root@iZ2ze3eh720u9x5c1hdhwyZ yanbian]# lsattr index.php
-------------e-- index.php

```

出现这种情况有可能是云平台的安全防护导致。
如 阿里云的云安全中心有【网页防篡改】保护，如果为文件/目录添加了该保护，在系统中修改文件/目录 权限就会被禁止。

![](https://libs.websoft9.com/Websoft9/blog/zh/2020/12/linux-safe-websoft9.png)


#### 磁盘满了如何查询哪些文件比较大？

```
# 查看当前目录下各文件、文件夹的大小
du -h –max-depth=1 *

# 查询当前目录总大小
du -sh

# 显示直接子目录文件及文件夹大小统计值
du -h –max-depth=0 *
```

#### 如何查询服务器日志？

运行命令`tailf /var/log/messages`

#### 服务启动失败怎么办？

当linux服务启动失败的时候，系统会提示我们使用 `journalctl -xe` 命令来查询详细信息，定位服务不能启动的原因。


#### 同一IP反复刷新页面导致服务器403错误处理
mod_evasive是Apache防御攻击的模块，有助于防止DoS、DDoS以及对Apache服务器的暴力攻击。它可以在攻击期间提供规避行动，并通过电子邮件和系统日志工具报告滥用行为。该模块的工作原理是创建一个IP地址和URI的内部动态表，并拒绝以下任何一个IP地址：
- 每秒请求同一页多次
- 每秒对同一个孩子发出50多个并发请求
- 暂时列入黑名单时提出任何要求

如果满足上述任何条件，则发送403响应并记录IP地址。
##### 查看Apache模块清单
```
apachectl -M
```
##### 修改配置项
在conf.d目录下找到mod_evasive.conf文件，进行配置（根据网站安全实际需求来）
![](https://libs.websoft9.com/Websoft9/blog/zh/2020/12/Apache-403-mod_evasive-conf-websoft9.png)

#### Windows 容器无法连接网络？

在使用阿里云官方提供的 Windows 2019 数据中心版（含 Container） 的时候发现网络不通，后面采用如下的办法解决了此问题：

1. 远程桌面登录到服务器
2. 到【网络管理】中禁用本地网络，此时远程桌面断开
3. 到阿里云 VNC 连接中重新远程到服务器，重新开启被禁用的本地网络

以上解决办法的根本原因未知。  

#### Docker service 无法启动？

先通过 `systemctl status docker` 和 `journalctl -xe` 查看错误日志。

* 如果错误日志是  Unit docker.socket entered failed state，表明系统缺少 docker 用户组，运行 `groupadd docker` 增加用户组

#### 容器无法启动？

最常见的原因是用户没有按照该容器的要求，设置必须的环境变量，导致容器启动失败

```shell
#查看容器日志，name是容器名称
sudo docker logs name
```

#### 容器端口映射失败？

如果用户无法把握宿主机（服务器）操作系统端口与容器端口的映射关系、可用性情况，请开启端口自动映射

#### 应用容器无法远程访问？

导致这个问题的可能原因有三点：

1. 端口没有正确映射到宿主机
2. 容器内部拒绝远程访问
3. 服务器安全组对应的端口没有开放