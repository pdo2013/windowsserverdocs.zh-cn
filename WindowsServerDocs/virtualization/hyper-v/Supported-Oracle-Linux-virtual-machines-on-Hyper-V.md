---
title: 受支持的 Oracle Linux 虚拟机上的 HYPER-V
description: 列出的 Linux 集成服务和每个版本中包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c02fdb5b-62f3-43cb-a190-ab74b3ebcf77
author: shirgall
ms.author: kathydav
ms.date: 06/01/2017
ms.openlocfilehash: e353ea0b444c07557de99db4472f565decb37349
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447751"
---
# <a name="supported-oracle-linux-virtual-machines-on-hyper-v"></a>受支持的 Oracle Linux 虚拟机上的 HYPER-V

>适用于：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

以下功能分发映射指示每个版本中存在的功能。 在表后列出的已知的问题和解决方法的每个分布区。

本节内容：

* [Red Hat 兼容内核系列](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_rhc)

* [坚不可摧企业内核系列](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_uek)

* [注意](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md#BKMK_notes)

## <a name="table-legend"></a>表格图例

* **内置的**-LIS 是作为此 Linux 分发版的一部分。 LIS 中内置的内核模块版本号 (如所示**lsmod**，例如) 不同于上由 Microsoft 提供的 LIS 下载包的版本号。 不匹配并不表明内置的 LIS 中已过时。

* &#10004;的提供功能

* (*空白*)-功能不可用

* **UEK R\*x QU\*y** -Unbreakable Enterprise Kernel (UEK) 其中*x*是发行版号和*y*是每季度更新。

## <a name="BKMK_rhc"></a>Red Hat 兼容内核系列

6.x 系列的 32 位内核是 PAE 启用。 没有内置的 Oracle Linux RHCK 6.0-6.3 LIS 支持。 Oracle Linux 7.x 内核只有 64 位。

| **功能**                                                                                                              | **Windows server 版本**   | **7.5-7.6**         | **7.4**             | **6.4 6.8 和 7.0 7.3**                                             | **6.4 6.8 和 7.0 7.2**                                             | **RHCK 7.0-7.2**         | **RHCK 6.8**             | **RHCK 6.6、 6.7**        | **RHCK 6.5**              | **RHCK6.4**               |
|--------------------------------------------------------------------------------------------------------------------------|------------------------------|---------------------|---------------------|---------------------------------------------------------------------|---------------------------------------------------------------------|--------------------------|--------------------------|--------------------------|---------------------------|---------------------------|
| **可用性**                                                                                                         |                              | 内置的            | 内置的            | [LIS 4.2](https://www.microsoft.com/download/details.aspx?id=55106) | [LIS 4.1](https://www.microsoft.com/download/details.aspx?id=51612) | 内置的                 | 内置的                 | 内置的                 | 内置的                  | 内置的                  |
| **[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**                          | 2016、 2012 R2、 2012、 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| Windows Server 2016 准确的时间                                                                                        | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**              |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Jumbo 帧                                                                                                             | 2016、 2012 R2、 2012、 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| VLAN 标记和中继                                                                                                | 2016、 2012 R2、 2012、 2008 R2 | &#10004;            | &#10004;            | &#10004;（请注意 6.4 6.8 1）                                       | &#10004;（请注意 6.4 6.8 1）                                       | &#10004;                 | &#10004;注释 1          | &#10004;注释 1          | &#10004;注释 1           | &#10004;注释 1           |
| 实时迁移                                                                                                           | 2016、 2012 R2、 2012、 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| 静态 IP 注入                                                                                                      | 2016、 2012 R2，2012年          | &#10004;请注意 14    | &#10004;请注意 14    | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| vRSS                                                                                                                     | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| TCP 分段和校验和卸载                                                                                   | 2016、 2012 R2、 2012、 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 |                           |                           |
| SR-IOV                                                                                                                   | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**                    |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| VHDX 大小调整                                                                                                              | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| 虚拟光纤通道                                                                                                    | 2016、 2012 R2                | &#10004;请注意 2     | &#10004;请注意 2     | &#10004;请注意 2                                                     | &#10004;请注意 2                                                     | &#10004;请注意 2          | &#10004;请注意 2          | &#10004;请注意 2          | &#10004;请注意 2           |                           |
| 实时虚拟机备份                                                                                              | 2016、 2012 R2                | &#10004;请注意 11,3  | &#10004;请注意 11 3 | &#10004;请注意 3 4                                                  | &#10004;请注意 3 4                                                  | &#10004;请注意 3、 4、 11   | &#10004;请注意 3、 4、 11   | &#10004;请注意 3、 4、 11   | &#10004;请注意 3、 4、 5、 11 | &#10004;请注意 3、 4、 5、 11 |
| 剪裁支持                                                                                                             | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 |                          |                           |                           |
| SCSI WWN                                                                                                                 | 2016、 2012 R2                | &#10004;            |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| **[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**                      |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| PAE 内核支持                                                                                                       | 2016、 2012 R2、 2012、 2008 R2 | 不可用                 | 不可用                 | &#10004;(仅 6.x)                                                 | &#10004;(仅 6.x)                                                 | 不可用                      | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| MMIO 间隙的配置                                                                                                | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| 动态内存的热添加                                                                                                 | 2016、 2012 R2，2012年          | &#10004;请注意 8 9  | &#10004;请注意 8 9  | &#10004;请注意 7、 8、 9、 10 （请注意 6.4 6.7 的 6）                      | &#10004;请注意 7、 8、 9、 10 （请注意 6.4 6.7 的 6）                      | &#10004;请注意 6、 7、 8、 9 | &#10004;请注意 6、 7、 8、 9 | &#10004;请注意 6、 7、 8、 9 | &#10004;请注意 6、 7、 8、 9  |                           |
| 动态内存-扩大                                                                                              | 2016、 2012 R2，2012年          | &#10004;请注意 8 9  | &#10004;请注意 8 9  | &#10004;请注意 7、 9、 10 （请注意 6.4 6.7 的 6）                         | &#10004;请注意 7、 9、 10 （请注意 6.4 6.7 的 6）                         | &#10004;请注意 6、 8、 9    | &#10004;请注意 6、 8、 9    | &#10004;请注意 6、 8、 9    | &#10004;请注意 6、 8、 9     | &#10004;请注意 6、 8、 9、 10 |
| 运行时内存调整大小                                                                                                    | 2016                         |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**                        |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| Hyper V 特定视频设备                                                                                            | 2016,2012 R2、 2012、 2008 R2  | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  |                           |
| **[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**                 |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| 键 / 值对                                                                                                           | 2016、 2012 R2、 2012、 2008 R2 | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;请注意 12         | &#10004;请注意 12         | &#10004;请注意 12         | &#10004;请注意 12          | &#10004;请注意 12          |
| 不可屏蔽的中断                                                                                                   | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            | &#10004;                 | &#10004;                 | &#10004;                 | &#10004;                  | &#10004;                  |
| 从主机到来宾文件副本                                                                                             | 2016、 2012 R2                | &#10004;            | &#10004;            | &#10004;                                                            | &#10004;                                                            |                          | &#10004;                 |                          |                           |                           |
| lsvmbus 命令                                                                                                          | 2016、 2012 R2、 2012、 2008 R2 |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| HYPER-V 套接字                                                                                                          | 2016                         |                     |                     | &#10004;                                                            | &#10004;                                                            |                          |                          |                          |                           |                           |
| PCI 传递/DDA                                                                                                      | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| **[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)** |                              |                     |                     |                                                                     |                                                                     |                          |                          |                          |                           |                           |
| 使用 UEFI 启动                                                                                                          | 2016、 2012 R2                | &#10004;请注意 13    | &#10004;请注意 13    | &#10004;请注意 13                                                    | &#10004;请注意 13                                                    | &#10004;请注意 13         | &#10004;请注意 13         |                          |                           |                           |
| 安全启动                                                                                                              | 2016                         | &#10004;            | &#10004;            |                                                                     |                                                                     |                          |                          |                          |                           |                           |


## <a name="BKMK_uek"></a>坚不可摧企业内核系列

Oracle Linux Unbreakable Enterprise Kernel (UEK) 只能是 64 位，支持内置的 LIS。

|**功能**|**Windows server 版本**|**UEK R4**|**UEK R3 QU3**|**UEK R3 QU2**|**UEK R3 QU1**|
|-|-|-|-|-|-|
|**可用性**||内置的|内置的|内置的|内置的|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确的时间|2016|||||
|**[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||
|Jumbo 帧|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|实时迁移|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2016、 2012 R2，2012年|&#10004;|&#10004;|&#10004;||
|vRSS|2016、 2012 R2|&#10004;||||
|TCP 分段和校验和卸载|2016、 2012 R2、 2012、 2008 R2|&#10004;||||
|SR-IOV|2016|||||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|VHDX 大小调整|2016、 2012 R2|&#10004;|&#10004;|&#10004;||
|虚拟光纤通道|2016、 2012 R2|&#10004;|&#10004;|&#10004;||
|实时虚拟机备份|2016、 2012 R2|&#10004;请注意 3、 4、 5、 11|&#10004;请注意 3、 4、 5、 11|&#10004;请注意 3、 4、 5、 11||
|剪裁支持|2016、 2012 R2|&#10004;||||
|SCSI WWN|2016、 2012 R2|||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**|
|PAE 内核支持|2016、 2012 R2、 2012、 2008 R2|不可用|不可用|不可用|不可用|
|MMIO 间隙的配置|2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存的热添加|2016、 2012 R2，2012年|&#10004;||||
|动态内存-扩大|2016、 2012 R2，2012年|&#10004;||||
|运行时内存调整大小|2016|||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||
|Hyper V 特定视频设备|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;||
|**[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|键 / 值对|2016、 2012 R2、 2012、 2008 R2|&#10004;请注意 11、 12|&#10004;请注意 11、 12|&#10004;请注意 11、 12|&#10004;请注意 11、 12|
|不可屏蔽的中断|2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾文件副本|2016、 2012 R2|&#10004;请注意 11|&#10004;请注意 11|&#10004;请注意 11|&#10004;请注意 11|
|lsvmbus 命令|2016、 2012 R2、 2012、 2008 R2|||||
|HYPER-V 套接字|2016|||||
|PCI 传递/DDA|2016|||||
|**[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**|
|使用 UEFI 启动|2016、 2012 R2|&#10004;||||
|安全启动|2016|&#10004;||||

## <a name="BKMK_notes"></a>说明

1. 对于此 Oracle Linux 版本中，VLAN 标记的工作原理，但 VLAN 中继不执行。

2. 同时使用虚拟光纤通道设备，请确保已填充该逻辑单元号 (LUN 0) 为 0。 如果未填充 LUN 0，Linux 虚拟机可能无法以本机方式装载光纤通道设备。

3. 如果有打开的文件句柄期间实时虚拟机的备份操作，则在某些极端情况下，可能需要进行文件系统一致性检查 (fsck) 还原备份 Vhd。

4. 如果虚拟机已连接的 iSCSI 设备或直连存储 （也称为传递磁盘），实时的备份操作可能会以静默方式失败。

5. 实时的备份支持对于 Oracle Linux 6.4/6.5/UEKR3 通过提供了 QU2 和 QU3[适用于 Linux 的 HYPER-V 备份 Essentials](https://github.com/LIS/backupessentials/tree/1.0)。

6. 动态内存支持才可在 64 位虚拟机上。

7. 默认情况下，此分发中未启用热添加支持。 若要启用热添加支持需要添加下 /etc/udev/rules.d/ udev 规则，如下所示：

   1. 创建一个文件 **/etc/udev/rules.d/100-balloon.rules**。 可以使用任何其他所需的文件名称。

   2. 将以下内容添加到文件： `SUBSYSTEM=="memory", ACTION=="add", ATTR{state}="online"`

   3. 重新启动系统以启用热添加支持。

   在 Linux Integration Services 下载安装上创建此规则，规则还将删除卸载 LIS，因此必须重新创建该规则，如果卸载后需要动态内存。

8. 如果来宾操作系统上的内存运行过低，动态内存操作可能会失败。 以下是一些最佳做法：

   * 启动内存和最小内存应等于或大于的发行版供应商建议的内存量。

   * 往往会占用的系统上的整个可用内存的应用程序被限制为使用最多 80%的可用 RAM。

9. 如果在 Windows Server 2016 或 Windows Server 2012 R2 操作系统上使用动态内存，指定**启动内存**，**最小内存**，并**最大内存**128 兆字节 (MB) 的倍数中的参数。 如果不这样做可能会导致热添加失败，并可能不会看到在来宾操作系统中增加的任何内存。

10. 某些分发版中，包括那些使用 LIS 3.5 或 LIS 4.0 中，只提供膨胀的支持，并且不提供热添加支持。 在此类方案中，可以通过将启动内存参数设置为一个值，该值等于最大内存参数使用的动态内存功能。 此结果正在热添加到虚拟机在启动时间和更高版本然后根据主机的内存要求的所有必要的内存中，HYPER-V 可以自由地分配或释放内存从来宾使用膨胀。 请配置**启动内存**并**最小内存**在或更高版本的分发的建议值。

11. 默认情况下未安装 oracle Linux 的 HYPER-V 守护程序。 若要使用这些守护程序，安装 hyperv 守护程序包。 与此包冲突下载 Linux 集成服务，不应具有已下载的 LIS 的系统上安装。

12. 键/值对 (KVP) 基础结构而无需 Linux 软件更新可能无法正常运行。 请联系分发供应商联系以获取软件更新，如果你看到此功能的问题。

13. 新增服务器 2012 R2Generation 2 虚拟机具有默认情况下已启用安全启动和某些 Linux 虚拟机将不会启动除非禁用安全启动选项。 可以禁用中的安全启动**固件**部分中的虚拟机的设置**Hyper-v 管理器**也可以使用 Powershell 对其禁用：

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

    Linux Integration Services 下载可应用于现有第 2 代 Vm，但不授予第 2 代功能。

14. 如果已为虚拟机上的给定合成网络适配器配置了网络管理器，静态 IP 注入可能无法工作。 对于静态 IP 顺畅运行注入请确保任一网络管理器已关闭完全或已关闭的特定网络适配器通过其 ifcfg ethX 文件。


请参阅

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [支持 CentOS 和 Red Hat Enterprise Linux 虚拟机上的 HYPER-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [受支持的 SUSE 虚拟机上的 HYPER-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [受支持的 Ubuntu 虚拟机上的 HYPER-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [受支持的 FreeBSD 虚拟机上的 HYPER-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [HYPER-V 上运行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)
