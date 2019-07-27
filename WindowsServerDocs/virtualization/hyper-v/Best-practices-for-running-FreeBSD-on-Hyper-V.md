---
title: 在 Hyper-v 上运行 FreeBSD 的最佳做法
description: 提供有关在虚拟机上运行 FreeBSD 的建议
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c66f1c8-2606-43a3-b4cc-166acaaf2d2a
author: shirgall
ms.author: kathydav
ms.date: 01/09/2017
ms.openlocfilehash: 598087411b35dde2e4a1cb606fae6a4602fe588e
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544685"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>在 Hyper-v 上运行 FreeBSD 的最佳做法

>适用于：Windows Server 2019, Windows Server 2016, Hyper-v Server 2016, Windows Server 2012 R2, Hyper-v server 2012 R2, Windows Server 2012, Hyper-v Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

本主题包含在 Hyper-v 虚拟机上作为来宾操作系统运行 FreeBSD 的建议列表。

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>在 Hyper-v 上的 FreeBSD 10.2 中启用 CARP

通用地址冗余协议 (CARP) 允许多台主机共享相同的 IP 地址和虚拟主机 ID (VHID), 以帮助为一个或多个服务提供高可用性。 如果一个或多个主机出现故障, 其他主机会以透明方式接管, 因此用户不会注意到服务失败。若要在 FreeBSD 10.2 中使用 CARP, 请按照[FreeBSD 手册](https://www.freebsd.org/doc/en/books/handbook/carp.html)中的说明进行操作, 然后在 Hyper-v 管理器中执行以下操作。

* 验证虚拟机是否具有网络适配器, 并为其分配一个虚拟交换机。 选择虚拟机并选择 "**操作** > **设置**"。

![已选择网络适配器的虚拟机设置的屏幕截图](media/Hyper-V_Settings_NetworkAdapter.png)

* 启用 MAC 地址欺骗。 为此，请执行以下操作：

   1. 选择虚拟机并选择 "**操作** > **设置**"。

   2. 展开 "**网络适配器**", 然后选择 "**高级功能**"。

   3. 选择 "**启用 MAC 地址欺骗**"。

## <a name="create-labels-for-disk-devices"></a>为磁盘设备创建标签

在启动过程中, 将在发现新设备时创建设备节点。 这可能意味着在添加新设备时, 设备名称可能会改变。 如果在启动过程中出现根装入错误, 则应为每个 IDE 分区创建标签, 以避免冲突和更改。 若要了解如何操作, 请参阅为[磁盘设备添加标签](https://www.freebsd.org/doc/handbook/geom-glabel.html)。 下面是示例。 

> [!IMPORTANT]
> 在进行任何更改之前, 请创建 fstab 的备份副本。

1. 将系统重新启动到单用户模式。 为此, 可以选择 "启动" 菜单选项 2 (对于 FreeBSD 10.3 + 为选项 4), 或在启动提示符下执行 "启动"。

2. 在单用户模式下, 为 fstab 中列出的每个 IDE 磁盘分区 (根和交换) 创建几何标签。 下面是 FreeBSD 10.3 的示例。

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   有关几何标签的其他信息, 请参阅:为[磁盘设备添加标签](https://www.freebsd.org/doc/handbook/geom-glabel.html)。

3. 系统将继续执行多用户启动。 启动完成后, 编辑/etc/fstab 并将常规设备名称替换为各自的标签。 最终的/etc/fstab 将如下所示:

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. 现在可以重新启动系统。 如果一切正常, 会正常运行, 装载将显示:

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>使用无线网络适配器作为虚拟交换机

如果主机上的虚拟交换机基于无线网络适配器, 请使用以下命令将 ARP 过期时间减少到60秒。 否则, VM 网络可能会在一段时间后停止工作。


```
   # sysctl net.link.ether.inet.max_age=60
```


请参阅

* [Hyper-v 上支持的 FreeBSD 虚拟机](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
