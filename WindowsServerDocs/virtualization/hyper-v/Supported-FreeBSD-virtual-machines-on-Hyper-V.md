---
title: Hyper-v 上支持的 FreeBSD 虚拟机
description: 列出每个版本中包含的 Linux integration services 和功能
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 08/30/2017
ms.openlocfilehash: b7b02e1ec93d6255412a89e7e7d7b8246cf5e50e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365506"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>Hyper-v 上支持的 FreeBSD 虚拟机

>适用于：Windows Server 2016，Hyper-v Server 2016，Windows Server 2012 R2，Hyper-v Server 2012 R2，Windows Server 2012，Hyper-v Server 2012，Windows Server 2008 R2，Windows 10，Windows 8.1，Windows 8，Windows 7.1，Windows 7

以下功能分发映射指示每个版本中的功能。 表后面列出了每个分发的已知问题和解决方法。

## <a name="table-legend"></a>表图例

* **内置**的 BIS （FreeBSD Integration Service）作为此 FreeBSD 版本的一部分包含在内。

* &#10004;-可用功能

* （*空白*）-功能不可用

|**功能**|**Windows Server 操作系统版本**|**11.1/11。2**|**11.0**|**10.3**|**10.2**|**10.0-10。1**|**9.1-9.3、8。4**|
|-|-|-|-|-|-|-|-|
|**可用性**||内置|内置|内置|内置|内置|[端口](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[转储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Windows Server 2016 准确时间|2019、2016|&#10004;||||||
|**[上网](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Jumbo 帧|2019、2016、2012 R2、2012、2008 R2|&#10004;备注3|&#10004;备注3|&#10004;备注3|&#10004;备注3|&#10004;备注3|&#10004;备注3|
|VLAN 标记和中继|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时迁移|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2019、2016、2012 R2、2012|&#10004;备注4|&#10004;备注4|&#10004;备注4|&#10004;备注4|&#10004;备注4|&#10004;|
|vRSS|2019、2016、2012 R2|&#10004;|&#10004;|||||
|TCP 分段和校验和卸载|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|大型接收卸载（LRO）|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2019、2016|||||||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||备注1|备注1|备注1|备注1|备注1、2|备注1、2|
|VHDX 调整大小|2019、2016、2012 R2|&#10004;备注7|&#10004;备注7|||||
|虚拟光纤通道|2019、2016、2012 R2|||||||
|实时虚拟机备份|2019、2016、2012 R2|&#10004;||||||
|剪裁支持|2019、2016、2012 R2|&#10004;||||||
|SCSI WWN|2019、2016、2012 R2|||||||
|**[记忆](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|PAE 内核支持|2019、2016、2012 R2、2012、2008 R2|||||||
|MMIO 间隙的配置|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存-热添加|2019、2016、2012 R2、2012|||||||
|动态内存-膨胀|2019、2016、2012 R2、2012|||||||
|运行时内存大小调整|2019、2016|||||||
|**[显示](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|Hyper-v 特定视频设备|2019、2016、2012 R2、2012、2008 R2|||||||
|**[杂](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|键/值对|2019、2016、2012 R2、2012、2008 R2|&#10004;|&#10004;|&#10004;|&#10004;备注6|&#10004;备注5、6|&#10004;备注6|
|不可屏蔽中断|2019、2016、2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾的文件复制|2019、2016、2012 R2|||||||
|lsvmbus 命令|2019、2016、2012 R2、2012、2008 R2|||||||
|Hyper-v 套接字|2019、2016|||||||
|PCI 传递/DDA|2019、2016|&#10004;||||||
|**[第2代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|使用 UEFI 启动|2019、2016、2012 R2|&#10004;||||||
|安全启动|2019、2016|||||||

## <a name="BKMK_notes"></a>本票

1. 建议在启动过程中为[磁盘设备添加标签]( https://www.freebsd.org/doc/handbook/geom-glabel.html)以避免出现根本装入错误。

2. 在 FreeBSD 1.x 和2.x 上加载 BIS 驱动程序时，可能无法识别虚拟 DVD 驱动器，除非通过以下命令启用旧版 ATA 驱动程序。
    ```sh
    # echo ‘hw.ata.disk_enable=1' >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126是受支持的最大 MTU 大小。

4. 在故障转移方案中，你不能在副本服务器中设置静态 IPv6 地址。 请改用 IPv4 地址。

5. KVP 由 FreeBSD 10.0 上的端口提供。 有关详细信息，请参阅 FreeBSD.org 上的[FreeBSD 10.0 端口](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/)。

6. KVP 在 Windows Server 2008 R2 上可能不起作用。

7. 若要使 VHDX 联机大小调整在 FreeBSD 11.0 中正常工作，需要一个特殊的手动步骤来解决在 11.0 + 中修复的几何 bug，主机调整 VHDX 磁盘的大小后，打开要写入的磁盘，然后运行 "gpart 恢复"，如下所示。
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
   **其他说明**：10稳定和11稳定的功能矩阵与 FreeBSD 11.1 版本相同。 此外，FreeBSD 10.2 和早期版本（10.1、10.0、1.x、2.x）的生存期已结束。 有关支持的版本和最新安全建议的最新列表，请参阅[此处](https://security.freebsd.org/)。

**其他说明**：10稳定和11稳定的功能矩阵与 FreeBSD 11.1 版本相同。 此外，FreeBSD 10.2 和早期版本（10.1、10.0、1.x、2.x）的生存期已结束。 有关支持的版本和最新安全建议的最新列表，请参阅[此处](https://security.freebsd.org/)。

## <a name="see-also"></a>请参阅

* [Hyper-v 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [在 Hyper-v 上运行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
