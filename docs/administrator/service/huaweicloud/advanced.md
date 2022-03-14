---
sidebar_position: 3
---

# 进阶

## 核心原理

### API/CLI

华为云提供了原生 API/CLI 和 OpenStack API/CLI 两种方式。  


## 问题解答

#### 如何启用Linux系统的root账号？

华为云默认已经开启root账号

#### 如何列出Websoft9在华为云云市场上的所有产品？

通过 [Websoft9华为云店铺](https://marketplace.huaweicloud.com/seller/e57458aa054b430fb2f82a066105f986) 查看我们在华为云上的所有镜像，也可以通过搜索关键字“websoft9”列出

#### 如何通过华为云控制台获取镜像文档？

登录华为云控制台，依次打开：云市场->已购买的服务，列出所有服务以及文档链接
![](https://libs.websoft9.com/Websoft9/DocsPicture/zh/huaweicloud/huaweicloud-getdocfromorder-websoft9.png)

#### 华为云磁盘支持扩容吗？

系统盘和数据盘均支持扩容。当前EVS只支持扩大容量，不支持缩小容量。

#### 如何在无需付出高成本的情况下享受服务器的高带宽（>100M）?

只要服务器的带宽付费方式为**按流量计费**即可享受最大300M的带宽，付费方式是后付费

#### 云硬盘备份与快照的区别？
云硬盘备份以及快照为存储在云硬盘中的数据提供冗余备份，确保高可靠性，两者的主要区别[参考](https://support.huaweicloud.com/productdesc-evs/evs_01_0048.html)

#### 切换操作系统有什么要注意的？

切换操作系统提供以用户选择的镜像进行重装系统的功能。

- 切换操作系统会清除系统盘数据，包括系统盘上的系统分区和所有其它分区，请做好数据备份。
- 切换操作系统成功后云服务器会自动开机。
- 部分操作系统不支持挂载SCSI磁盘，切换操作系统后，可能会导致原弹性云服务器上挂载的SCSI磁盘不可用。查看支持列表。
- 如果云服务器一键式重置密码功能未生效，建议安装密码重置插件开启一键重置密码功能。如何安装。
- 切换操作系统后，当前操作系统内的个性化设置（如DNS、主机名等）将被重置，需重新配置。

#### 如何试用Websoft9的镜像？

部分镜像的商品详情页面提供了官方的演示地址。如果没有演示地址，请按需购买（按小时付费）试用，只需付出几毛钱/小时的成本。

#### 鲲鹏是什么类型的服务器？

鲲鹏原指华为在2019年1月初发布的一款兼容ARM指令集的服务器芯片鲲鹏920，性能强悍，配备了64个物理核心，单核实力从CPU算力benchmark的角度对比，大约持平于同期X86的主流服务器芯片，整体多核多线程算力较同期的X86芯片更强大。 当前鲲鹏的含义已经有所延伸，鲲鹏不再仅仅局限于鲲鹏系列服务芯片，更是包含了服务器软件在新的计算架构平台上的完整软硬件生态和云服务生态。