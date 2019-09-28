---
title: Hyper-v 上支持的 SUSE 虚拟机
description: 列出每个版本中包含的 Linux integration services 和功能
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ec0e14c-4498-4bd9-8fe6-b94260198efc
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 45517c1d381ba55c819b09b53ae563092e161b1e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366735"
---
# <a name="supported-suse-virtual-machines-on-hyper-v"></a>Hyper-v 上支持的 SUSE 虚拟机

>适用于：Windows Server 2016，Hyper-v Server 2016，Windows Server 2012 R2，Hyper-v Server 2012 R2，Windows Server 2012，Hyper-v Server 2012，Windows Server 2008 R2，Windows 10，Windows 8.1，Windows 8，Windows 7.1，Windows 7

下面是一个功能分布图，它指示每个版本中的功能。 表后面列出了每个分发的已知问题和解决方法。

适用于 Hyper-v 的内置 SUSE Linux Enterprise Service 驱动程序已通过 SUSE 认证。 可在此公告中查看示例配置：[SUSE 是认证公告](https://www.suse.com/nbswebapp/yesBulletin.jsp?bulletinNumber=144176)。

## <a name="table-legend"></a>表图例

* **内置**的-.lis 作为此 Linux 分发的一部分包含在内。Microsoft 提供的 IIS 下载包不适用于此分发，因此不安装它。内置 .LIS 的内核模块版本号（如**lsmod**所示）不同于 Microsoft 提供的 .lis 下载包中的版本号。 不匹配并不表明内置的 .LIS 版本已过期。

* &#10004;-可用功能

* （*空白*）-功能不可用

SLES12 + 仅限64位。

|**功能**|**Windows Server 操作系统版本**|**SLES 15**|**SLES 12 SP3/SP4**|**SLES 12 SP2**|**SLES 12 SP1**|**SLES 11 SP4**|**SLES 11 SP3**|
|-|-|-|-|-|-|-|-|
|**可用性**||内置|内置|内置|内置|内置|内置|
|**[转储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确时间|2019、2016|&#10004;|&#10004;|&#10004;||||
|**[上网](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Jumbo 帧|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时迁移|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、2016、2012 R2、2012|&#10004;备注1|&#10004;备注1|&#10004;备注1|&#10004;备注1|&#10004;备注1|&#10004;备注1|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|TCP 分段和校验和卸载|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019、2016|&#10004;|&#10004;|&#10004;||||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||||
|VHDX 调整大小|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|虚拟光纤通道|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时虚拟机备份|2019、2016、2012 R2|&#10004;备注2、3、8|&#10004;备注2、3、8|&#10004;备注2、3、8|&#10004;备注2、3、8|&#10004;备注2、3、8|&#10004;备注2、3、8|
|剪裁支持|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|SCSI WWN|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;||||
|**[记忆](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|PAE 内核支持|2019、2016、2012 R2、2012、2008 R2|不可用|不可用|不可用|不可用|&#10004;|&#10004;|
|MMIO 间隙的配置|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存-热添加|2019、2016、2012 R2、2012|&#10004;备注5、6|&#10004;备注5、6|&#10004;备注5、6|&#10004;备注5、6|&#10004;备注4、5、6|&#10004;备注4、5、6|
|动态内存-膨胀|2019、2016、2012 R2、2012|&#10004;备注5、6|&#10004;备注5、6|&#10004;备注5、6|&#10004;备注5、6|&#10004;备注4、5、6|&#10004;备注4、5、6|
|运行时内存大小调整|2019、2016|&#10004;备注5、6|&#10004;备注5、6|&#10004;备注5、6||||
|**[显示](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Hyper-v 特定视频设备|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|**[杂](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|键/值对|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;备注7|&#10004;备注7|
|不可屏蔽中断|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾的文件复制|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;||||
|Hyper-v 套接字|2019、2016|&#10004;|&#10004;|||||
|PCI 传递/DDA|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|||
|**[第2代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|使用 UEFI 启动|2019、2016、2012 R2|&#10004;备注9|&#10004;备注9|&#10004;备注9|&#10004;备注9|&#10004;备注9||
|安全启动|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;|||

## <a name="BKMK_notes"></a>本票

1. 如果为虚拟机上的特定 Hyper-v 特定网络适配器配置了**网络管理器**，则静态 IP 注入可能不起作用。 若要确保静态 IP 注入正常运行，请确保网络管理器已完全关闭，或已通过其**ifcfg-eth0-ethX**文件在特定网络适配器上关闭网络管理器。

2. 如果在执行实时虚拟机备份操作的过程中有打开的文件句柄，则在某些角落情况下，备份的 Vhd 可能需要在还原时执行文件系统一致性检查（fsck）。

3. 如果虚拟机有连接的 iSCSI 设备或直接连接的存储（也称为传递磁盘），则实时备份操作可能会悄悄地失败。

4. 如果来宾操作系统在内存上运行得太低，动态内存操作可能会失败。 下面是一些最佳做法：

   * 启动内存和最小内存应等于或大于分发供应商建议的内存量。

   * 通常会消耗系统中的全部可用内存的应用程序，仅消耗最多 80% 的可用 RAM。

5. 动态内存支持仅适用于64位虚拟机。

6. 如果在 Windows Server 2016 或 Windows Server 2012 操作系统上使用动态内存，请以128兆字节（MB）为单位指定 "**启动内存**"、"**最小内存**" 和 "**最大内存**" 参数。 如果不这样做，可能会导致热添加失败，并且在来宾操作系统中可能看不到任何内存增长。

7. 在 Windows Server 2016 或 Windows Server 2012 R2 中，如果没有 Linux 软件更新，键/值对基础结构可能无法正常运行。 请与您的分销商联系以获取软件更新，以防您看到此功能的问题。

8. 如果多次装入单个分区，则 VSS 备份会失败。

9. 在 Windows Server 2012 R2 上，第2代虚拟机默认启用安全启动，第2代 Linux 虚拟机将不会启动，除非禁用了安全启动选项。 你可以在 Hyper-V 管理器中虚拟机设置的“固件” 部分禁用安全启动，也可以使用 Powershell 来禁用它：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

## <a name="see-also"></a>请参阅

* [Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [Hyper-v 上支持的 CentOS 和 Red Hat Enterprise Linux 虚拟机](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 Oracle Linux 虚拟机](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 Ubuntu 虚拟机](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 FreeBSD 虚拟机](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上运行 Linux 的最佳实践](Best-Practices-for-running-Linux-on-Hyper-V.md)
