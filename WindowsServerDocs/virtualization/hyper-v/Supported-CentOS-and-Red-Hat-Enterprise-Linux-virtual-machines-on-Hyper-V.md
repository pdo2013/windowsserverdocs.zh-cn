---
title: 支持 CentOS 和 Red Hat Enterprise Linux 虚拟机上的 HYPER-V
description: 为受支持的 CentOS 和 Red Hat Enterprise 分发版列表版本的 Linux 集成服务
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4bf8783d-dee5-4b3e-8cce-2b11b117c189
author: danihalfin
ms.author: daniha
ms.date: 12/20/2017
ms.openlocfilehash: 6c9ed85c2249a4671e52eb7d512298a75f53b309
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222676"
---
# <a name="supported-centos-and-red-hat-enterprise-linux-virtual-machines-on-hyper-v"></a>支持 CentOS 和 Red Hat Enterprise Linux 虚拟机上的 HYPER-V

>适用于：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

以下功能分发映射指示内置和可下载版本的 Linux Integration Services 中存在的功能。 在表后列出的已知的问题和解决方法的每个分布区。

HYPER-V （自 Red Hat Enterprise Linux 6.4 之后可用） 的内置 Red Hat Enterprise Linux 集成服务驱动程序便足以针对 Red Hat Enterprise Linux 来宾运行的 HYPER-V 主机上使用高性能综合设备。为此，请使用通过 Red Hat 认证这些内置驱动程序。 在此 Red Hat 网页上，可以查看经过认证的配置：[Red Hat 认证目录](https://access.redhat.com/ecosystem/search/#/ecosystem/Red%20Hat%20Enterprise%20Linux?sort=sortTitle%20asc&vendors=Microsoft&category=Server)。 但并不需要下载并安装从 Microsoft 下载中心，Linux Integration Services 包，这样做因此可能会限制你的 Red Hat 支持 Red Hat 知识库文章 1067年中所述：[Red Hat 知识库 1067年](https://access.redhat.com/articles/1067)。

由于内置 LIS 支持和可下载的 LIS 支持之间的潜在冲突升级内核时，禁用自动更新、 卸载 LIS 可下载程序包、 更新内核、 重新启动，并发布最新的 LIS，然后安装并再次重新启动。

>[!NOTE]
>官方 Red Hat Enterprise Linux 证书信息，请通过[Red Hat 客户门户](https://access.redhat.com/ecosystem/search/#/category/Server?sort=sortTitle%20asc&query=windows%20server&ecosystem=Red%20Hat%20Enterprise%20Linux)。

本节内容：

* [RHEL/CentOS 7.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_7x)

* [RHEL/CentOS 6.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_6x)

* [RHEL/CentOS 5.x Series](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_5x)

* [注意](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>表格图例

* **内置的**-LIS 是作为此 Linux 分发版的一部分。 LIS 中内置的内核模块版本号 (如所示**lsmod**，例如) 不同于上由 Microsoft 提供的 LIS 下载包的版本号。 不匹配并不表示内置的 LIS 中已过时。

* &#10004;的提供功能

* (*空白*)-功能不可用

## <a name="BKMK_7x"></a>RHEL/CentOS 7.x Series

本系列只有 64 位内核。

|**功能**|**Windows Server 版本**|**7.5-7.6**|**7.3-7.4**|**7.0-7.2**|**7.5-7.6**|**7.4**|**7.3**|**7.2**|**7.1**|**7.0**|
|-|-|-|-|-|-|-|-|-|-|-|
|**可用性**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|内置的|内置的|内置的|内置的|内置的|内置的||
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确的时间|2019, 2016|&#10004;|&#10004;|||||||
|**[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Jumbo 帧|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时迁移|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、 2016、 2012 R2，2012年|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|
|vRSS|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|TCP 分段和校验和卸载|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;|||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||
|VHDX 大小调整|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|虚拟光纤通道|2019、 2016、 2012 R2|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|
|实时虚拟机备份|2019、 2016、 2012 R2|&#10004;请注意 5|&#10004;请注意 5|&#10004;请注意 5|&#10004;请注意 4 5|&#10004;请注意 4 5|&#10004;请注意 4 5|&#10004;请注意 4 5|&#10004;请注意 4 5|&#10004;请注意 4 5|
|剪裁支持|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|||
|SCSI WWN|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||
|PAE 内核支持|2019、 2016、 2012 R2、 2012、 2008 R2|不可用|不可用|不可用|不可用|不可用|不可用|不可用|不可用|不可用|
|MMIO 间隙的配置|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存的热添加|2019、 2016、 2012 R2，2012年|&#10004;请注意 8、 9、 10|&#10004;请注意 8、 9、 10|&#10004;请注意 8、 9、 10|&#10004;请注意 9 10|&#10004;请注意 9 10|&#10004;请注意 9 10|&#10004;请注意 9 10|&#10004;请注意 9 10|&#10004;请注意 8、 9、 10|
|动态内存-扩大|2019、 2016、 2012 R2，2012年|&#10004;请注意 8、 9、 10|&#10004;请注意 8、 9、 10|&#10004;请注意 8、 9、 10|&#10004;请注意 9 10|&#10004;请注意 9 10|&#10004;请注意 9 10|&#10004;请注意 9 10|&#10004;请注意 9 10|&#10004;请注意 8、 9、 10|
|运行时内存调整大小|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||
|Hyper V 特定视频设备|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||
|键 / 值对|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|不可屏蔽的中断|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾文件副本|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|||||||
|HYPER-V 套接字|2019, 2016|&#10004;|&#10004;|&#10004;|||||||
|PCI 传递/DDA|2019, 2016|&#10004;|&#10004;||&#10004;|&#10004;|&#10004;||||
|**[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|||||
|使用 UEFI 启动|2019、 2016、 2012 R2|&#10004;请注意 14|&#10004;请注意 14|&#10004;请注意 14|&#10004;请注意 14|&#10004;请注意 14|&#10004;请注意 14|&#10004;请注意 14|&#10004;请注意 14|&#10004;请注意 14|
|安全启动|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="BKMK_6x"></a>RHEL/CentOS 6.x Series

在本系列的 32 位内核是 PAE 启用。 没有内置 LIS 支持针对 RHEL/CentOS 6.0-6.3。

|**功能**|**Windows Server 版本**|**6.4-6.10**|**6.0-6.3**|**6.10, 6.9, 6.8**|**6.6, 6.7**|**6.5**|**6.4**|
|-|-|-|-|-|-|-|-|
|**可用性**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|内置的|内置的|内置的|内置的|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确的时间|2019, 2016||||||
|**[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Jumbo 帧|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|
|实时迁移|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、 2016、 2012 R2，2012年|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|
|vRSS|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|TCP 分段和校验和卸载|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|SR-IOV|2019, 2016|||||||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|VHDX 大小调整|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|虚拟光纤通道|2019、 2016、 2012 R2|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3||
|实时虚拟机备份|2019、 2016、 2012 R2|&#10004;请注意 5|&#10004;请注意 5|&#10004;请注意 4 5|&#10004;请注意 4 5|&#10004;请注意 4、 5、 6|&#10004;请注意 4、 5、 6|
|剪裁支持|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;||||
|SCSI WWN|2019、 2016、 2012 R2|&#10004;|&#10004;|||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|PAE 内核支持|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|MMIO 间隙的配置|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存的热添加|2019、 2016、 2012 R2，2012年|&#10004;请注意 7、 9、 10|&#10004;请注意 7、 9、 10|&#10004;请注意 7、 9、 10|&#10004;请注意 7、 8、 9、 10|&#10004;请注意 7、 8、 9、 10||
|动态内存-扩大|2019、 2016、 2012 R2，2012年|&#10004;请注意 7、 9、 10|&#10004;请注意 7、 9、 10|&#10004;请注意 7、 9、 10|&#10004;请注意 7、 9、 10|&#10004;请注意 7、 9、 10|&#10004;请注意 7、 9、 10、 11|
|运行时内存调整大小|2019, 2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Hyper V 特定视频设备|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|**[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|键 / 值对|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;请注意 12|&#10004;请注意 12|&#10004;请注意 12、 13|&#10004;请注意 12、 13|
|不可屏蔽的中断|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾文件副本|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|lsvmbus 命令|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;||||||
|HYPER-V 套接字|2019, 2016|&#10004;|&#10004;|||||
|PCI 传递/DDA|2019, 2016|||||||
|**[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|使用 UEFI 启动|2012 R2|||||||
||2019, 2016|&#10004;请注意 14|&#10004;请注意 14|&#10004;请注意 14||||
|安全启动|2019, 2016||||||

## <a name="BKMK_5x"></a>RHEL/CentOS 5.x Series

此序列具有支持的 32 位 PAE 内核可用。 5.9 之前没有内置 RHEL/CentOS 的 LIS 支持。

|**功能**|**Windows Server 版本**|5.2 -5.11|**5.2-5.11**|**5.9 - 5.11**|
|-|-|-|-|-|
|**可用性**||[LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106)|[LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612)|内置的|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确的时间|2019, 2016||||
|**[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Jumbo 帧|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|
|实时迁移|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、 2016、 2012 R2，2012年|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|
|vRSS|2019、 2016、 2012 R2||||
|TCP 分段和校验和卸载|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;||
|SR-IOV|2019, 2016||||||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|VHDX 大小调整|2019、 2016、 2012 R2|&#10004;|&#10004;||
|虚拟光纤通道|2019、 2016、 2012 R2|&#10004;请注意 3|&#10004;请注意 3||
|实时虚拟机备份|2019、 2016、 2012 R2|&#10004;请注意 5、 15|&#10004;请注意 5|&#10004;请注意 4、 5、 6|
|剪裁支持|2019、 2016、 2012 R2||||
|SCSI WWN|2019、 2016、 2012 R2||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|PAE 内核支持|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|MMIO 间隙的配置|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|
|动态内存的热添加|2019、 2016、 2012 R2，2012年||||
|动态内存-扩大|2019、 2016、 2012 R2，2012年|&#10004;请注意 7、 9、 10、 11|&#10004;请注意 7、 9、 10、 11||
|运行时内存调整大小|2019, 2016||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Hyper V 特定视频设备|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;||
|**[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|键 / 值对|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;||
|不可屏蔽的中断|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|
|从主机到来宾文件副本|2019、 2016、 2012 R2|&#10004;|&#10004;||
|lsvmbus 命令|2019、 2016、 2012 R2、 2012、 2008 R2||||
|HYPER-V 套接字|2019, 2016||||
|PCI 传递/DDA|2019, 2016||||||
|**[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|使用 UEFI 启动|2019、 2016、 2012 R2||||
|安全启动|2019, 2016||||

## <a name="BKMK_notes"></a>说明

1. 这对于 RHEL/CentOS 发行，VLAN 标记的工作原理，但 VLAN 中继却没有。

2. 如果已为虚拟机上的给定合成网络适配器配置了网络管理器，静态 IP 注入可能无法工作。 对于静态 IP 顺畅运行注入请确保任一网络管理器已关闭完全或已关闭的特定网络适配器通过其 ifcfg ethX 文件。

3. 在 Windows Server 2012 R2 使用虚拟光纤通道设备时，请确保已填充该逻辑单元号 (LUN 0) 为 0。 如果未填充 LUN 0，Linux 虚拟机可能无法以本机方式装载光纤通道设备。

4. 内置的 LIS 中为"hyperv 守护程序"包必须安装此功能。

5. 如果有打开的文件句柄期间实时虚拟机的备份操作，则在某些极端情况下，可能需要进行文件系统一致性检查 (fsck) 还原备份 Vhd。 如果虚拟机已连接的 iSCSI 设备或直连存储 （也称为传递磁盘），实时的备份操作可能会以静默方式失败。

6. 首选的 Linux Integration Services 下载时，实时备份支持的 RHEL/CentOS 5.9-5.11/6.4/6.5 它也可通过[适用于 Linux 的 HYPER-V 备份 Essentials](https://github.com/LIS/backupessentials/tree/1.0)。

7. 动态内存支持才可在 64 位虚拟机上。

8. 默认情况下，此分发中未启用热添加支持。 若要启用热添加支持需要添加下 /etc/udev/rules.d/ udev 规则，如下所示：

   1. 创建一个文件 **/etc/udev/rules.d/100-balloon.rules**。 可以使用任何其他所需的文件名称。

   2. 将以下内容添加到文件： `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. 重新启动系统以启用热添加支持。

   在 Linux Integration Services 下载安装上创建此规则，规则还将删除卸载 LIS，因此必须重新创建该规则，如果卸载后需要动态内存。

9. 如果来宾操作系统上的内存运行过低，动态内存操作可能会失败。 以下是一些最佳做法：

   * 启动内存和最小内存应等于或大于的发行版供应商建议的内存量。

   * 往往会占用的系统上的整个可用内存的应用程序被限制为使用最多 80%的可用 RAM。

10. 如果在 Windows Server 2016 或 Windows Server 2012 R2 操作系统上使用动态内存，指定**启动内存**，**最小内存**，并**最大内存**128 兆字节 (MB) 的倍数中的参数。 如果不这样做可能会导致热添加失败，并可能不会看到在来宾操作系统中增加的任何内存。

11. 某些分发版中，包括那些使用 LIS 4.0 和 4.1，只提供膨胀的支持，并且不提供热添加支持。 在此类方案中，可以通过将启动内存参数设置为一个值，该值等于最大内存参数使用的动态内存功能。 此结果正在热添加到虚拟机在启动时间和更高版本然后根据主机的内存要求的所有必要的内存中，HYPER-V 可以自由地分配或释放内存从来宾使用膨胀。 请配置**启动内存**并**最小内存**在或更高版本的分发的建议值。

12. 若要启用键/值对 (KVP) 基础结构，请从 RHEL ISO 安装 hypervkvpd 或 hyperv 守护程序 rpm 包。 或者可以直接从 RHEL 存储库安装包。

13. 键/值对 (KVP) 基础结构而无需 Linux 软件更新可能无法正常运行。 请联系分发供应商联系以获取软件更新，如果你看到此功能的问题。

14. 在 Windows Server 2012 R2 第 2 代虚拟机具有默认情况下已启用安全启动，除非禁用安全启动选项，否则将不会启动某些 Linux 虚拟机。 可以禁用中的安全启动**固件**部分中的虚拟机的设置**Hyper-v 管理器**也可以使用 Powershell 对其禁用：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

   Linux Integration Services 下载可应用于现有第 2 代 Vm，但不授予第 2 代功能。

15. 在 Red Hat Enterprise Linux 或 CentOS 5.2，5.3，和 5.4 的文件系统冻结功能不可用，因此实时虚拟机备份也不是可用。

请参阅

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Hyper-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [受支持的 Oracle Linux 虚拟机上的 HYPER-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [受支持的 SUSE 虚拟机上的 HYPER-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [受支持的 Ubuntu 虚拟机上的 HYPER-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [受支持的 FreeBSD 虚拟机上的 HYPER-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [HYPER-V 上运行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Red Hat 硬件认证](https://hardware.redhat.com/&quicksearch=Microsoft)
