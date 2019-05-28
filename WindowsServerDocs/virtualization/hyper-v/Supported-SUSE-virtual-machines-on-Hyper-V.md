---
title: 受支持的 SUSE 虚拟机上的 HYPER-V
description: 列出的 Linux 集成服务和每个版本中包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: d7b6d3adb4841ea827c56309307549c911a439ea
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222813"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>受支持的 SUSE 虚拟机上的 HYPER-V

>适用于：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

下面是一个功能分发映射，以指示每个版本中的功能。 在表后列出的已知的问题和解决方法的每个分布区。

通过 SUSE 的认证的 HYPER-V 的内置 SUSE Linux Enterprise 服务驱动程序。 此公告中，可以查看一个示例配置：[SUSE 是认证公告](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176)。

## <a name="table-legend"></a>表格图例

* **内置的**-LIS 是作为此 Linux 分发版的一部分。Microsoft 提供的 LIS 下载包并不适用于此分布，因此未安装。LIS 中内置的内核模块版本号 (如所示**lsmod**，例如) 不同于上由 Microsoft 提供的 LIS 下载包的版本号。 不匹配并不表明内置的 LIS 中已过时。

* &#10004;的提供功能

* (*空白*)-功能不可用

SLES12 + 只能是 64 位。

|**功能**|**Windows Server 操作系统版本**|**SLES 15**|**SLES 12 SP3/SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**可用性**||内置|内置|内置|内置|内置|内置|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确的时间|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Jumbo 帧|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时迁移|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、 2016、 2012 R2，2012年|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|
|vRSS|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|TCP 分段和校验和卸载|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;||||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||||
|VHDX 大小调整|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|虚拟光纤通道|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时虚拟机备份|2019、 2016、 2012 R2|&#10004;请注意 2、 3、 8|&#10004;请注意 2、 3、 8|&#10004;请注意 2、 3、 8|&#10004;请注意 2、 3、 8|&#10004;请注意 2、 3、 8|&#10004;请注意 2、 3、 8|
|剪裁支持|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SCSI WWN|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|PAE 内核支持|2019、 2016、 2012 R2、 2012、 2008 R2|不可用|不可用|不可用|不可用|&#10004;|&#10004;|
|MMIO 间隙的配置|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存的热添加|2019、 2016、 2012 R2，2012年|&#10004;请注意 5、 6|&#10004;请注意 5、 6|&#10004;请注意 5、 6|&#10004;请注意 5、 6|&#10004;请注意 4、 5、 6|&#10004;请注意 4、 5、 6|
|动态内存-扩大|2019、 2016、 2012 R2，2012年|&#10004;请注意 5、 6|&#10004;请注意 5、 6|&#10004;请注意 5、 6|&#10004;请注意 5、 6|&#10004;请注意 4、 5、 6|&#10004;请注意 4、 5、 6|
|运行时内存调整大小|2019, 2016|&#10004;请注意 5、 6|&#10004;请注意 5、 6|&#10004;请注意 5、 6||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Hyper V 特定视频设备|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|键/值对|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;请注意 7|&#10004;请注意 7|
|不可屏蔽的中断|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾文件副本|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;||||
|HYPER-V 套接字|2019, 2016|&#10004;|&#10004;|||||
|PCI 传递/DDA|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|使用 UEFI 启动|2019、 2016、 2012 R2|&#10004;请注意 9|&#10004;请注意 9|&#10004;请注意 9|&#10004;请注意 9|&#10004;请注意 9||
|安全启动|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="BKMK_notes"></a>说明

1. 如果静态 IP 注入可能无法工作**网络管理器**已为虚拟机上的给定 Hyper V 特定网络适配器配置。 若要确保顺畅运行静态 IP 注入请确保网络管理器完全关闭，或通过为特定的网络适配器已关闭其**ifcfg ethX**文件。

2. 如果有打开的文件句柄期间实时虚拟机的备份操作，则在某些极端情况下，可能需要进行文件系统一致性检查 (fsck) 还原备份 Vhd。

3. 如果虚拟机已连接的 iSCSI 设备或直连存储 （也称为传递磁盘），实时的备份操作可能会以静默方式失败。

4. 如果来宾操作系统上的内存运行过低，动态内存操作可能会失败。 以下是一些最佳做法：

   * 启动内存和最小内存应等于或大于的发行版供应商建议的内存量。

   * 往往会占用的系统上的整个可用内存的应用程序被限制为使用最多 80%的可用 RAM。

5. 动态内存支持才可在 64 位虚拟机上。

6. 如果在 Windows Server 2016 或 Windows Server 2012 操作系统上使用动态内存，指定**启动内存**，**最小内存**，并**最大内存**128 兆字节 (MB) 的倍数中的参数。 如果不这样做可能会导致热添加失败，您可能看不到来宾操作系统中增加的任何内存。

7. 在 Windows Server 2016 或 Windows Server 2012 R2 中，键/值对基础结构可能无法正常运行而无需 Linux 软件更新。 请联系分发供应商联系以获取软件更新，如果你看到此功能的问题。

8. 如果单个分区装载多次，VSS 备份将失败。

9. 在 Windows Server 2012 R2 中，除非禁用安全启动选项，否则将不启动第 2 代虚拟机具有默认情况下生成 2 Linux 虚拟机已启用安全启动。 你可以在 Hyper-V 管理器中虚拟机设置的“固件”  部分禁用安全启动，也可以使用 Powershell 来禁用它：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>请参阅

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [支持 CentOS 和 Red Hat Enterprise Linux 虚拟机上的 HYPER-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [受支持的 Oracle Linux 虚拟机上的 HYPER-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [受支持的 Ubuntu 虚拟机上的 HYPER-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [受支持的 FreeBSD 虚拟机上的 HYPER-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [HYPER-V 上运行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)
