---
title: Hyper-v 上支持的 Ubuntu 虚拟机
description: 列出每个版本中包含的 Linux integration services 和功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95ea5f7c-25c6-494b-8ffd-2a77f631ee94
author: shirgall
ms.author: shirgall
ms.date: 06/13/2019
ms.openlocfilehash: ad0f79767310595244d0d57876c20b9548a81a96
ms.sourcegitcommit: e2b565ce85a97c0c51f6dfe7041f875a265b35dd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2019
ms.locfileid: "69584815"
---
# <a name="supported-ubuntu-virtual-machines-on-hyper-v"></a>Hyper-v 上支持的 Ubuntu 虚拟机

>适用于：Windows Server 2019, 2016, Hyper-v Server 2019, 2016, Windows Server 2012 R2, Hyper-v Server 2012 R2, Windows Server 2012, Hyper-v Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

从 Ubuntu 12.04 开始, 加载 "linux-虚拟" 包将安装一个适合用作来宾虚拟机的内核。 此包总是依赖于用于虚拟机的最新的最小通用内核映像和标头。 虽然它的使用是可选的, 但 linux 虚拟内核将加载更少的驱动程序, 并且启动速度更快且内存开销低于一般映像。

若要充分利用 Hyper-v, 请安装适当的 linux 工具和 linux-云工具包, 以便安装用于虚拟机的工具和守护程序。 使用 linux 虚拟内核时, 加载 linux 工具-虚拟和 linux-云工具-虚拟。

以下功能分发映射指示每个版本中的功能。 表后面列出了每个分发的已知问题和解决方法。

## <a name="table-legend"></a>表图例

* **内置**的-.lis 作为此 Linux 分发的一部分包含在内。 Microsoft 提供的 .LIS 下载包不适用于此分发版, 因此不安装它。 内置 .LIS 的内核模块版本号 (如**lsmod**所示) 不同于 Microsoft 提供的 .lis 下载包中的版本号。 不匹配并不表明内置的 .LIS 版本已过期。

* &#10004;-可用功能

* (*空白*)-功能不可用

|**功能**|**Windows Server 操作系统版本**|**18.10/19.04**|**18.04 LTS**|**16.04 LTS**|**14.04 LTS**|**12.04 LTS**|
|-|-|-|-|-|-|-|
|**可用性**||内置|内置|内置|内置|内置|
|**[转储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016 准确时间|2019、2016|&#10004;|&#10004;|&#10004;|||
|**[上网](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**|||||||
|Jumbo 帧|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|VLAN 标记和中继|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时迁移|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、2016、2012 R2、2012|&#10004;备注1|&#10004;备注1|&#10004;备注1|&#10004;备注1|&#10004;备注1|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|TCP 分段和校验和卸载|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|SR-IOV|2019、2016|&#10004;|&#10004;|&#10004;|||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||||||
|VHDX 调整大小|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|虚拟光纤通道|2019、2016、2012 R2|&#10004;备注2|&#10004;备注2|&#10004;备注2|&#10004;备注2||
|实时虚拟机备份|2019、2016、2012 R2|&#10004;备注3、4、6|&#10004;备注3、4、5|&#10004;备注3、4、5|&#10004;备注3、4、5||
|剪裁支持|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|SCSI WWN|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[记忆](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||
|PAE 内核支持|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|MMIO 间隙的配置|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存-热添加|2019、2016、2012 R2、2012|&#10004;备注7、8、9|&#10004;备注7、8、9|&#10004;备注7、8、9|&#10004;备注7、8、9||
|动态内存-膨胀|2019、2016、2012 R2、2012|&#10004;备注7、8、9|&#10004;备注7、8、9|&#10004;备注7、8、9|&#10004;备注7、8、9||
|运行时内存大小调整|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[显示](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**|||||||
|Hyper-v 特定视频设备|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|**[杂](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||
|键/值对|2019、2016、2012 R2、2012、2008 R2|&#10004;备注6、10|&#10004;备注5、10|&#10004;备注5、10|&#10004;备注5、10|&#10004;备注5、10|
|不可屏蔽中断|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾的文件复制|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|lsvmbus 命令|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;||
|Hyper-v 套接字|2019、2016||||||
|PCI 传递/DDA|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||
|**[第2代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||
|使用 UEFI 启动|2019、2016、2012 R2|&#10004;注释11、12|&#10004;注释11、12|&#10004;注释11、12|&#10004;注释11、12||
|安全启动|2019、2016|&#10004;|&#10004;|&#10004;|&#10004;||

## <a name="notes"></a>说明

1. 如果为虚拟机上的特定 Hyper-v 特定网络适配器配置了**网络管理器**, 则静态 IP 注入可能不起作用。 若要确保静态 IP 注入正常运行, 请确保网络管理器已完全关闭, 或已通过其**ifcfg-eth0-ethX**文件在特定网络适配器上关闭网络管理器。

2. 使用虚拟光纤通道设备时, 请确保已填充逻辑单元号 0 (LUN 0)。 如果尚未填充 LUN 0, Linux 虚拟机可能无法以本机方式装入光纤通道设备。

3. 如果在执行实时虚拟机备份操作的过程中有打开的文件句柄, 则在某些角落情况下, 备份的 vhd 可能需要在还原时执行文件`fsck`系统一致性检查 ()。

4. 如果虚拟机有连接的 iSCSI 设备或直接连接的存储 (也称为传递磁盘), 则实时备份操作可能会悄悄地失败。

5. 在长期支持 (LTS) 版本中, 最新的 Linux Integration Services 使用最新的虚拟硬件支持 (HWE) 内核。

   若要在14.04、16.04 和18.04 上安装 Azure 优化内核, 请运行以下命令作为根 (或 sudo):

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   12.04 没有单独的虚拟内核。 若要在12.04 上安装通用 HWE 内核, 请以 root (或 sudo) 身份运行以下命令:

   ```bash
   # apt-get update
   # apt-get install linux-generic-lts-trusty
   ```

   在 Ubuntu 12.04 上, 以下 Hyper-v 守护程序位于单独安装的包中:

   * **VSS 快照守护**程序-创建实时 Linux 虚拟机备份需要此守护程序。
   * **KVP 后台**程序-此守护程序允许设置和查询内部和外部密钥值对。
   * **fcopy 后台**程序-此后台程序在主机和来宾之间实现文件复制服务。

   若要在12.04 上安装 KVP 后台程序, 请运行以下命令作为根 (或 sudo)。

   ```bash
   # apt-get install hv-kvp-daemon-init linux-tools-lts-trusty linux-cloud-tools-generic-lts-trusty
   ```

   每当更新内核时, 都必须重新启动虚拟机才能使用。

6. 在 Ubuntu 18.10 或19.04 上, 使用最新的虚拟内核以获得最新的 Hyper-v 功能。

   若要在18.10 或19.04 上安装虚拟内核, 请以 root (或 sudo) 身份运行以下命令:

   ```bash
   # apt-get update
   # apt-get install linux-azure
   ```

   每当更新内核时, 都必须重新启动虚拟机才能使用。

7. 动态内存支持仅适用于64位虚拟机。

8. 如果来宾操作系统的内存过低, 则动态内存操作可能会失败。 下面是一些最佳做法:

   * 启动内存和最小内存应等于或大于分发供应商建议的内存量。

   * 通常会消耗系统中的全部可用内存的应用程序, 仅消耗最多 80% 的可用 RAM。

9. 如果使用的是 Windows Server 2019、Windows Server 2016 或 Windows Server 2012/2012 R2 操作系统上的动态内存, 请指定 "**启动内存**"、"**最小内存**" 和 "**最大内存**" 参数, 以128兆字节 (MB) 为倍数。 如果不这样做, 可能会导致热添加失败, 并且在来宾操作系统上可能看不到任何内存增长。

10. 在 Windows Server 2019、Windows Server 2016 或 Windows Server 2012 R2 中, 如果没有 Linux 软件更新, 键/值对基础结构可能会无法正常工作。 请与您的分销商联系以获取软件更新, 以防您看到此功能的问题。

11. 在 Windows Server 2012 R2 上, 第2代虚拟机默认启用安全启动, 某些 Linux 虚拟机将无法启动, 除非禁用了安全启动选项。 你可以在**Hyper-v 管理器**中虚拟机设置的 "**固件**" 部分禁用安全启动, 也可以使用 Powershell 禁用它:

    ```Powershell
    Set-VMFirmware -VMName "VMname" -EnableSecureBoot Off
    ```

12. 尝试复制现有第2代 VHD 虚拟机的 VHD 以创建新的第2代虚拟机之前, 请执行以下步骤:

    1. 登录到现有第2代虚拟机。

    2. 将目录更改为启动 EFI 目录:

       ```bash
       # cd /boot/efi/EFI
       ```

    3. 将 ubuntu 目录复制到名为 boot 的新目录中:

       ```bash
       # sudo cp -r ubuntu/ boot
       ```

    4. 将目录更改为新创建的启动目录:

       ```bash
       # cd boot
       ```

    5. 重命名 shimx64 文件:

       ```bash
       # sudo mv shimx64.efi bootx64.efi
       ```

## <a name="see-also"></a>请参阅

* [Hyper-v 上支持的 CentOS 和 Red Hat Enterprise Linux 虚拟机](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 Oracle Linux 虚拟机](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上支持的 SUSE 虚拟机](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Hyper-v 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)

* [在 Hyper-v 上运行 Linux 的最佳实践](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Set-vmfirmware](https://technet.microsoft.com/library/dn464287.aspx)

* [第2代 VM 中的 Ubuntu 14.04-Ben Armstrong 的虚拟化博客](https://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/09/ubuntu-14-04-in-a-generation-2-vm.aspx)
