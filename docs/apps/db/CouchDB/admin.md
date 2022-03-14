---
sidebar_position: 2
slug: /couchdb/admin
tags:
  - CouchDB
  - Cloud Native Database
---

# 维护参考

## 系统参数

CouchDB 预装包包含 CouchDB 运行所需一序列支撑软件（简称为“组件”），下面列出主要组件名称、安装路径、配置文件地址、端口、版本等重要的信息。

### 路径

#### CouchDB

CouchDB 安装目录： */data/couchdb*  
CouchDB 日志目录： */data/logs/couchdb*  

#### Nginx

Nginx vhost 配置文件: */etc/nginx/sites-available/default.conf*  
Nginx main 配置文件: */etc/nginx/nginx.conf*  
Nginx 日志文件路径 : */var/log/nginx/*

### 端口号

在云服务器中，通过 **[安全组设置](https://support.websoft9.com/docs/faq/zh/tech-instance.html)** 来控制（开启或关闭）端口是否可以被外部访问。 

通过命令`netstat -tunlp` 看查看相关端口，下面列出可能要用到的端口：

| 名称 | 端口号 | 用途 |  必要性 |
| --- | --- | --- | --- |
| HTTP | 5984 | 通过 HTTP 访问 CouchDB 控制台 | 必选 |
| HTTP | 80 | 通过 nginx转发后HTTP 访问 | 可选 |
| HTTP | 443 | 通过HTTPs 访问 CouchDB 控制台 | 可选 |


### 版本号

组件版本号可以通过云市场商品页面查看。但部署到您的服务器之后，组件会自动进行更新导致版本号有一定的变化，故精准的版本号请通过在服务器上运行命令查看：

```shell
# Check all components version
sudo cat /data/logs/install_version.txt

# Linux Version
lsb_release -a

# Nginx Version
nginx -v

# CouchDB version
cat /data/wwwroot/couchdb/releases/*/couchdb.rel  |sed -n 3p | awk -F '"' '{print $4}'
```

### 服务

使用由Websoft9提供的 CouchDB 部署方案，可能需要用到的服务如下：

#### CouchDB

```shell
sudo systemctl start couchdb
sudo systemctl stop couchdb
sudo systemctl restart couchdb
sudo systemctl status couchdb

```

#### Nginx

```shell
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl status nginx
```

## 备份

### 全局自动备份

所有的云平台都提供了全局自动备份功能，基本原理是基于**磁盘快照**：快照是针对于服务器的磁盘来说的，它可以记录磁盘在指定时间点的数据，将其全部备份起来，并可以实现一键恢复。

```
- 备份范围: 将操作系统、运行环境、数据库和应用程序
- 备份效果: 非常好
- 备份频率: 按小时、天、周备份均可
- 恢复方式: 云平台一键恢复
- 技能要求：非常容易
- 自动化：设置策略后全自动备份
```

不同云平台的自动备份方案有一定的差异，详情参考 [云平台备份方案](https://support.websoft9.com/docs/faq/zh/tech-instance.html)

### 程序手工备份

程序手工本地备份是通过**下载应用程序源码和导出数据库文件**实现最小化的备份方案。

下面以列表的方式介绍这种备份：
```
- 备份范围: 数据库和应用程序
- 备份效果: 一般
- 备份频率: 一周最低1次，备份保留30天
- 恢复方式: 重新导入
- 技能要求：非常容易
- 自动化：无
```
通用的手动备份操作步骤如下：

1. 通过 WinSCP 将网站目录（*/data/wwwroot/*）**压缩后**再完整的下载到本地
2. 通过 phpMyAdmin 逐个导出数据库
   ![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/mysql/phpmyadmin-export-websoft9.png)
3. 将程序文件和数据库文件放到同一个文件夹，根据日期命名
4. 备份工作完成

### CouchDb如何进行备份？

CouchDB备份涉及到3种不同的文件：数据库文件，配置文件，日志文件
详情参考官方备份文档：[Backing up CouchDB](https://docs.couchdb.org/en/latest/maintenance/backups.html)

## 恢复

## 升级

### 系统级更新

运行一条更新命令，即可完成系统级（包含rethinkdb小版本更新）更新：

``` shell
#For Ubuntu&Debian
apt update && apt upgrade -y

#For Centos&Redhat
yum update -y
```
> 本部署包已预配置一个用于自动更新的计划任务。如果希望去掉自动更新，请删除对应的 Cron

### CouchDB升级

详情参考官方升级文档：[Upgrading CouchDB](https://docs.couchdb.org/en/latest/install/upgrading.html)



## 故障处理

此处收集使用 CouchDB 过程中最常见的故障，供您参考

> 大部分故障与云平台密切相关，如果你可以确认故障的原因是云平台造成的，请参考[云平台文档](https://support.websoft9.com/docs/faq/zh/tech-instance.html)

#### 如何查看错误日志？

日志文件路径为：`/data/logs`。检索关键词 **Failed** 或者 **error** 查看错误

#### CouchDB服务无法启动？

1. 运行`systemctl status couchdb`，便可以查看启动状态和错误

2. 打开日志文件：*/data/logs/couchdb*，检索 **failed** 关键词，分析错误原因


#### 在Chrome下修改密码后报错？

这个并不是服务器端的问题，只要更新浏览器即可。


## 常见问题

#### 是否可以通过命令行修改CouchDB后台密码？

不可以，可以登陆进入user管理中修改
![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/couchdb/couchdb-pw-websoft9.png)

#### 数据库用户对应的密码是多少？

密码存放在服务器相关文件中：`/credentials/password.txt`

#### 是否有可视化的数据库管理工具？

有，本身就是数据库，访问地址：*http://服务器公网IP/_utils*


#### 是否可以修改CouchDB的源码路径？

不可以

#### 如何修改上传的文件权限?

```shell
# 拥有者
chown -R apache.apache /data/wwwroot/
# 读写执行权限
find /data/wwwroot/ -type d -exec chmod 750 {} \;
find /data/wwwroot/ -type f -exec chmod 640 {} \;
```

#### CouchDb如何进行备份？

CouchDB备份涉及到3种不同的文件：数据库文件，配置文件，日志文件
详情参考官方备份文档：[Backing up CouchDB](https://docs.couchdb.org/en/latest/maintenance/backups.html)
