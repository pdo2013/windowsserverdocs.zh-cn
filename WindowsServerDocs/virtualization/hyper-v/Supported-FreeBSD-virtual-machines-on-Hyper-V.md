---
title: 受支持的 FreeBSD 虚拟机上的 HYPER-V
description: 列出的 Linux 集成服务和每个版本中包含的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 930e758f-bd50-46b4-a3a4-9857110f17b4
author: shirgall
ms.author: kathydav
ms.date: 08/30/2017
ms.openlocfilehash: a398334700f7c292732207919b73a33145a6aae9
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222695"
---
# <a name="supported-freebsd-virtual-machines-on-hyper-v"></a>受支持的 FreeBSD 虚拟机上的 HYPER-V

>适用于：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

以下功能分发映射指示每个版本中的功能。 在表后列出的已知的问题和解决方法的每个分布区。

## <a name="table-legend"></a>表格图例

* **内置的**-BIS （FreeBSD 集成服务） 是作为此 FreeBSD 版本的一部分。

* &#10004;的提供功能

* (*空白*)-功能不可用

|**功能**|**Windows Server 操作系统版本**|**11.1/11.2**|**11.0**|**10.3**|**10.2**|**10.0 - 10.1**|**9.1 - 9.3, 8.4**|
|-|-|-|-|-|-|-|-|
|**可用性**||内置的|内置的|内置的|内置的|内置的|[端口](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/) |
|**[Core](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#core)**|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004; |
|Windows Server 2016 准确的时间|2016|&#10004;||||||
|**[网络](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#networking)**||||||||
|Jumbo 帧|2016、 2012 R2、 2012、 2008 R2|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|&#10004;请注意 3|
|VLAN 标记和中继|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|实时迁移|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|静态 IP 注入|2016、 2012 R2，2012年|&#10004;请注意 4|&#10004;请注意 4|&#10004;请注意 4|&#10004;请注意 4|&#10004;请注意 4|&#10004;|
|vRSS|2016、 2012 R2|&#10004;|&#10004;|||||
|TCP 分段和校验和卸载|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;|||
|大型接收卸载 （进行 lro 操作）|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;||||
|SR-IOV|2016|||||||
|**[存储](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#storage)**||注释 1|注释 1|注释 1|注释 1|请注意 1、 2|请注意 1、 2|
|VHDX 大小调整|2016、 2012 R2|&#10004;请注意 7|&#10004;请注意 7|||||
|虚拟光纤通道|2016、 2012 R2|||||||
|实时虚拟机备份|2016、 2012 R2|&#10004;||||||
|剪裁支持|2016、 2012 R2|&#10004;||||||
|SCSI WWN|2016、 2012 R2|||||||
|**[Memory](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#memory)**||||||||
|PAE 内核支持|2016、 2012 R2、 2012、 2008 R2|||||||
|MMIO 间隙的配置|2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|动态内存的热添加|2016、 2012 R2，2012年|||||||
|动态内存-扩大|2016、 2012 R2，2012年|||||||
|运行时内存调整大小|2016|||||||
|**[Video](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#video)**||||||||
|HYPER-V 特定视频设备|2016、 2012 R2、 2012、 2008 R2|||||||
|**[杂项](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#miscellaneous)**||||||||
|键/值对|2016、 2012 R2、 2012、 2008 R2|&#10004;|&#10004;|&#10004;|&#10004;备注 6|&#10004;请注意 5、 6|&#10004;备注 6|
|不可屏蔽的中断|2016、 2012 R2|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|从主机到来宾文件副本|2016、 2012 R2|||||||
|lsvmbus 命令|2016、 2012 R2、 2012、 2008 R2|||||||
|HYPER-V 套接字|2016|||||||
|PCI 传递/DDA|2016|&#10004;||||||
|**[第 2 代虚拟机](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md#generation-2-virtual-machines)**||||||||
|使用 UEFI 启动|2016、 2012 R2|&#10004;||||||
|安全启动|2016|||||||

## <a name="BKMK_notes"></a>说明

1. 建议向[标签磁盘设备]( https://www.freebsd.org/doc/handbook/geom-glabel.html)以避免在启动期间装载错误根源。

2. 可能无法识别的虚拟 DVD 驱动器，当 BIS 驱动程序已加载 FreeBSD 8.x 和 9.x 未启用旧版 ATA 驱动程序通过以下命令。
    ```sh
    # echo ‘hw.ata.disk_enable=1’ >> /boot/loader.conf
    # shutdown -r now
    ```

3. 9126 是支持的最大 MTU 大小。

4. 在故障转移方案中，不能设置副本服务器中的静态 IPv6 地址。 改为使用 IPv4 地址。

5. KVP 提供的 FreeBSD 10.0 上的端口。 请参阅[FreeBSD 10.0 端口](https://svnweb.freebsd.org/ports/branches/2015Q1/emulators/hyperv-is/)上 FreeBSD.org 有关详细信息。

6. KVP 可能不适用于 Windows Server 2008 R2。

7. 以确保在 FreeBSD 11.0 中正常运行 VHDX 联机大小调整工作，则需要解决一个几何 bug 后主机调整 VHDX 磁盘的大小固定的 11.0 +，-打开磁盘进行写入，并运行"gpart"作为恢复以下特殊的手动步骤。
    ```sh
    # dd if=/dev/da1 of=/dev/da1 count=0
    # gpart recover da1
    ```
**其他说明**:10 稳定和 11 稳定的功能矩阵是 FreeBSD 11.1 版本相同。 在添加、 FreeBSD 10.2 和早期版本 (10.1，10.0，9.x、 8.x) 是生命周期结束。 请参阅[此处](https://security.freebsd.org/)的受支持的版本和最新的安全建议的最新列表。

**其他说明**:10 稳定和 11 稳定的功能矩阵是 FreeBSD 11.1 版本相同。 在添加、 FreeBSD 10.2 和早期版本 (10.1，10.0，9.x、 8.x) 是生命周期结束。 请参阅[此处](https://security.freebsd.org/)的受支持的版本和最新的安全建议的最新列表。

## <a name="see-also"></a>请参阅

* [用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明](Feature-Descriptions-for-Linux-and-FreeBSD-virtual-machines-on-Hyper-V.md)
* [HYPER-V 上运行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)
