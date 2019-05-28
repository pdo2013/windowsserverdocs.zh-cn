---
title: 在 HYPER-V 上支持的 Debian 虚拟机
description: 列出的 Linux 集成服务和每个版本中包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3cc62c10-02a3-4633-960c-23bf91a45bd5
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 129783dc980be6e471ecadb2cdbffee900e3396e
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222830"
---
# <a name="supported-debian-virtual-machines-on-hyper-v"></a>在 HYPER-V 上支持的 Debian 虚拟机

>适用于：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

以下功能分发映射指示每个版本中存在的功能。 在表后列出的已知的问题和解决方法的每个分布区。

## <a name="table-legend"></a>表格图例

* **内置的**-LIS 是作为此 Linux 分发版的一部分。 由 Microsoft 提供的 LIS 下载包不起作用，因此未安装此分配。 LIS 中内置的内核模块版本号 (如所示**lsmod**，例如) 不同于上由 Microsoft 提供的 LIS 下载包的版本号。 不匹配并不表示内置的 LIS 中已过时。

* &#10004;的提供功能

* (*空白*)-功能不可用

|**功能**|**Windows Server 操作系统版本**|**9.0-9.6 (stretch)**|**8.0-8.11 (jessie)**|**7.0 7.11 (wheezy)**|
|-|-|-|-|-|
|**可用性**||内置的|内置的|内置的 （备注 6）|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确的时间|2019, 2016|&#10004;请注意 8||
|**[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|
|Jumbo 帧|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|实时迁移|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、 2016、 2012 R2，2012年|||
|vRSS|2019、 2016、 2012 R2|&#10004;请注意 8|||
|TCP 分段和校验和卸载|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;请注意 8|||
|SR-IOV|2019, 2016|&#10004;请注意 8||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**|
|VHDX 大小调整|2019、 2016、 2012 R2|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|
|虚拟光纤通道|2019、 2016、 2012 R2|||
|实时虚拟机备份|2019、 2016、 2012 R2|&#10004;请注意 4 5|&#10004;请注意 4 5|&#10004;请注意 4|
|剪裁支持|2019、 2016、 2012 R2|&#10004;请注意 8|||
|SCSI WWN|2019、 2016、 2012 R2|&#10004;请注意 8||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|PAE 内核支持|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|
|MMIO 间隙的配置|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|
|动态内存的热添加|2019、 2016、 2012 R2，2012年|&#10004;请注意 8|||
|动态内存-扩大|2019、 2016、 2012 R2，2012年|&#10004;请注意 8|||
|运行时内存调整大小|2019, 2016|&#10004;请注意 8|||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|
|Hyper V 特定视频设备|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;||
|**[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**|
|键 / 值对|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;请注意 4|&#10004;请注意 4||
|不可屏蔽的中断|2019、 2016、 2012 R2|&#10004;|&#10004;|
|从主机到来宾文件副本|2019、 2016、 2012 R2|&#10004;请注意 4|&#10004;请注意 4||
|lsvmbus 命令|2019、 2016、 2012 R2、 2012、 2008 R2|||
|HYPER-V 套接字|2019, 2016|&#10004;请注意 8|||
|PCI 传递/DDA|2019, 2016|&#10004;请注意 8|||
|**[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|使用 UEFI 启动|2019、 2016、 2012 R2|&#10004;请注意 7|&#10004;请注意 7||
|安全启动|2019, 2016|||

## <a name="BKMK_notes"></a>说明

1. 不支持在 Vhd 上创建文件系统大于 2 TB。

2. 在上 Windows Server 2008 R2 SCSI 磁盘 8 不同项中创建/sd *。

3. Windows Server 2012 R2 具有 8 个核心或更多的 VM 将具有所有路由到单个 vCPU 的中断。

4. 从开始 Debian 8.3 手动安装 Debian 包"hyperv 的守护程序"包含的键 / 值对、 fcopy 和 VSS 守护程序。 Hyperv 守护程序包必须来自 Debian 7.x 和 8.0 8.2 [Debian backports](https://wiki.debian.org/Backports)。

5. 实时虚拟机备份不会使用了 ext2 文件系统。 Debian 安装程序创建的默认布局包括 ext2 文件系统，则必须自定义布局不创建此文件系统类型。

6. Debian 7.x 不受支持，并使用较旧的内核，而内核纳入[Debian backports](https://wiki.debian.org/Backports) Debian 7.x 具有提升的 HYPER-V 功能。

7. 在 Windows Server 2012 R2 第 2 代虚拟机具有默认情况下已启用安全启动，除非禁用安全启动选项，否则将不会启动某些 Linux 虚拟机。 可以禁用中的安全启动**固件**部分中的虚拟机的设置**Hyper-v 管理器**也可以使用 Powershell 对其禁用：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```
8. 最新的上游内核功能才可用通过使用包括内核[Debian backports](https://wiki.debian.org/Backports)。

请参阅

* [支持 CentOS 和 Red Hat Enterprise Linux 虚拟机上的 HYPER-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [受支持的 Oracle Linux 虚拟机上的 HYPER-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [受支持的 SUSE 虚拟机上的 HYPER-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [受支持的 Ubuntu 虚拟机上的 HYPER-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [受支持的 FreeBSD 虚拟机上的 HYPER-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [HYPER-V 上运行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)
