---
title: 受支持的 Ubuntu 虚拟机上的 HYPER-V
description: 列出的 Linux 集成服务和每个版本中包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 11/19/2018
ms.openlocfilehash: afba885fc49ba129c0ef452704cbfe9f9cf884ba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834038"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>受支持的 Ubuntu 虚拟机上的 HYPER-V

>适用于：Windows Server 2019，2016 年的 HYPER-V 服务器 2019，2016 年，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012，Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

Ubuntu 12.04 从开始，"linux 虚拟"包加载将作为来宾虚拟机安装了适用于使用的内核。 此包始终取决于最新的最小的通用核心映像和虚拟机使用的标头。 虽然它的使用是可选的 linux 虚拟内核将加载更少的驱动程序，并可能更快地启动和较少内存开销高于的通用映像。

若要获取充分利用 HYPER-V，请安装相应的 linux 工具和要安装工具和守护程序以使用与虚拟机的 linux 云工具程序包。 当使用的 linux 虚拟内核，加载 linux 工具虚拟和 linux 的云的工具-虚拟。

以下功能分发映射指示每个版本中的功能。 在表后列出的已知的问题和解决方法的每个分布区。

## <a name="table-legend"></a>表格图例

* **内置的**-LIS 是作为此 Linux 分发版的一部分。 对于此分布，因此未安装它，Microsoft 提供的 LIS 下载包不起作用。 LIS 中内置的内核模块版本号 (如所示**lsmod**，例如) 不同于上由 Microsoft 提供的 LIS 下载包的版本号。 不匹配并不表明内置的 LIS 中已过时。

* &#10004;的提供功能

* (*空白*)-功能不可用

|**功能**|**Windows Server 操作系统版本**|**18.10**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**可用性**||内置|内置|内置|内置|内置|
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_core)**|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确的时间|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Networking)**|||||||
|Jumbo 帧|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时迁移|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、 2016、 2012 R2，2012年|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|&#10004;注释 1|
|vRSS|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|TCP 分段和校验和卸载|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019, 2016|&#10004;|&#10004;|&#10004;|||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Storage)**||||||
|VHDX 大小调整|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|虚拟光纤通道|2019、 2016、 2012 R2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2|&#10004;请注意 2||
|实时虚拟机备份|2019、 2016、 2012 R2|&#10004;请注意 3、 4、 6|&#10004;请注意 3、 4、 5|&#10004;请注意 3、 4、 5|&#10004;请注意 3、 4、 5||
|剪裁支持|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Memory)**||||||
|PAE 内核支持|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|MMIO 间隙的配置|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存的热添加|2019、 2016、 2012 R2，2012年|&#10004;请注意 7、 8、 9|&#10004;请注意 7、 8、 9|&#10004;请注意 7、 8、 9|&#10004;请注意 7、 8、 9||
|动态内存-扩大|2019、 2016、 2012 R2，2012年|&#10004;请注意 7、 8、 9|&#10004;请注意 7、 8、 9|&#10004;请注意 7、 8、 9|&#10004;请注意 7、 8、 9||
|运行时内存调整大小|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Video)**|||||||
|HYPER-V 特定视频设备|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_Misc)**||||||
|键/值对|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;请注意 6 10|&#10004;请注意 5、 10|&#10004;请注意 5、 10|&#10004;请注意 5、 10|&#10004;请注意 5、 10|
|不可屏蔽的中断|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾文件副本|2019、 2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、 2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|HYPER-V 套接字|2019, 2016||||||
|PCI 传递/DDA|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#BKMK_gen2)**||||||
|使用 UEFI 启动|2019、 2016、 2012 R2|&#10004;请注意 11、 12|&#10004;请注意 11、 12|&#10004;请注意 11、 12|&#10004;请注意 11、 12||
|安全启动|2019, 2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="BKMK_notes"></a>说明

1. 如果静态 IP 注入可能无法工作**网络管理器**已为虚拟机上的给定 Hyper V 特定网络适配器配置。 若要确保顺畅运行静态 IP 注入请确保网络管理器完全关闭，或通过为特定的网络适配器已关闭其**ifcfg ethX**文件。

2. 同时使用虚拟光纤通道设备，请确保已填充该逻辑单元号 (LUN 0) 为 0。 如果未填充 LUN 0，Linux 虚拟机可能无法以本机方式装载光纤通道设备。

3. 如果没有打开的文件处理期间实时虚拟机的备份操作，则在某些极端情况下，备份 Vhd 可能需要进行文件系统一致性检查 (`fsck`) 上还原。

4. 如果虚拟机已连接的 iSCSI 设备或直连存储 （也称为传递磁盘），实时的备份操作可能会以静默方式失败。

5. 在长期支持 (LTS) 版本为最新 Linux Integration Services 使用最新的虚拟硬件支持 (HWE) 内核。

   若要安装 Azure 优化内核 14.04、 16.04 和 18.04 上，为根 （sudo） 运行以下命令：

   ```bash
   # apt-get update
   # apt-get install linux-azure

   ```

   12.04 没有单独的虚拟内核。 若要安装上 12.04 泛型 HWE 内核，请为根 （sudo） 运行以下命令：

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty

   ```

   在 Ubuntu 12.04 以下 HYPER-V 守护程序中单独安装包中：

   * **VSS 快照守护程序**-创建实时 Linux 虚拟机备份时需要此守护程序。
   * **KVP 守护程序**-此守护程序允许设置和查询内部和外部键/值对。
   * **fcopy 守护程序**-此守护程序实现的文件复制到主机和来宾之间的服务。

   若要安装上 12.04 KVP 守护程序，请为根 （sudo） 运行以下命令。

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty

   ```

   每当更新内核时，必须重新启动虚拟机来使用它。

6. 在 Ubuntu 18.10 上使用最新的虚拟内核具有最新的 HYPER-V 功能。

   若要安装 18.10 上的虚拟内核，请为根 （sudo） 运行以下命令：

   ```bash
   # apt-get update
   # apt-get install linux-azure

   ```

   每当更新内核时，必须重新启动虚拟机来使用它。

7. 动态内存支持才可在 64 位虚拟机上。

8. 如果来宾操作系统上的内存运行过低，动态内存操作可能会失败。 以下是一些最佳做法：

   * 启动内存和最小内存应等于或大于的发行版供应商建议的内存量。

   * 往往会占用的系统上的整个可用内存的应用程序被限制为使用最多 80%的可用 RAM。

9. 如果在 Windows Server 2019、 Windows Server 2016 或 Windows Server 2012/windows server 2012 R2 操作系统上使用动态内存，指定**启动内存**，**最小内存**，和**最大内存**128 兆字节 (MB) 的倍数中的参数。 如果不这样做可能会导致热添加失败，并可能看不到来宾操作系统上增加任何内存。

10. 在 Windows Server 2019、 Windows Server 2016 或 Windows Server 2012 R2 中，键/值对基础结构可能无法正常运行而无需 Linux 软件更新。 请联系分发供应商联系以获取软件更新，如果你看到此功能的问题。

11. 在 Windows Server 2012 R2，第 2 代虚拟机具有默认情况下和某些 Linux 除非禁用安全启动选项，否则不会启动虚拟机已启用安全启动。 可以禁用中的安全启动**固件**部分中的虚拟机的设置**Hyper-v 管理器**也可以使用 Powershell 对其禁用：

   ```Powershell
   Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off

   ```

12. 然后再尝试将现有生成 2 VHD 虚拟机的 VHD 创建新的第 2 代虚拟机复制，请执行以下步骤：

   1. 登录到现有的第 2 代虚拟机。

   2. 将目录更改为启动 EFI 目录：

      ```bash
      # cd /boot/efi/EFI

      ```

   3. 中的 ubuntu 目录复制到名为 boot 的新目录：

      ```bash
      # sudo cp -r ubuntu/ boot

      ```

   4. 将目录更改为新创建的启动目录：

      ```bash
      # cd boot

      ```

   5. 重命名 shimx64.efi 文件：

      ```bash
      # sudo mv shimx64.efi bootx64.efi

      ```

## <a name="see-also"></a>请参阅

* [支持 CentOS 和 Red Hat Enterprise Linux 虚拟机上的 HYPER-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [受支持的 Oracle Linux 虚拟机上的 HYPER-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [受支持的 SUSE 虚拟机上的 HYPER-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [HYPER-V 上运行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [第 2 代中的 Ubuntu 14.04 VM-Ben Armstrong 的虚拟化博客](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
