---
title: HYPER-V 上运行 FreeBSD 的最佳做法
description: 提供有关虚拟机上运行 FreeBSD 的建议
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
ms.openlocfilehash: 6320ceb86093146592a54ab34b013f334f43ddb4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447772"
---
# <a name="best-practices-for-running-freebsd-on-hyper-v"></a>HYPER-V 上运行 FreeBSD 的最佳做法

>适用于：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

本主题列出了针对 HYPER-V 虚拟机上作为来宾操作系统中运行 FreeBSD 的建议。

## <a name="enable-carp-in-freebsd-102-on-hyper-v"></a>启用在 HYPER-V 上的 FreeBSD 10.2 CARP

公用地址冗余协议 (CARP) 允许多个主机共享相同的 IP 地址和虚拟主机 ID (VHID) 以帮助为一个或多个服务提供高可用性。 如果一个或多个主机发生故障，其他主机以透明方式接管以便用户不会注意到服务失败。若要使用 FreeBSD 10.2 CARP，按照中的说明[FreeBSD 手册](https://www.freebsd.org/doc/en/books/handbook/carp.html)并执行以下操作中的 HYPER-V 管理器。

* 验证虚拟机网络适配器并且它具有分配的虚拟交换机。 选择虚拟机，然后选择**操作** > **设置**。

![网络适配器处于选中状态的虚拟机设置的屏幕截图](media/Hyper-V_Settings_NetworkAdapter.png)

* 启用 MAC 地址欺骗。 为此，请执行以下操作：

   1. 选择虚拟机，然后选择**操作** > **设置**。

   2. 展开**网络适配器**，然后选择**高级功能**。

   3. 选择**启用 MAC 地址欺骗**。

## <a name="create-labels-for-disk-devices"></a>为磁盘设备创建标签

在启动期间，一旦发现新设备的创建设备节点。 这可能意味着添加新设备时，可以更改设备名称。 如果在启动期间遇到根装载错误，则应创建为每个 IDE 分区，以避免冲突和更改的标签。 若要了解如何操作，请参阅[标记磁盘设备](https://www.freebsd.org/doc/handbook/geom-glabel.html)。 下面是示例。 

> [!IMPORTANT]
> 进行任何更改之前请您 fstab 的备份副本。

1. 为单用户模式重新启动系统。 这可以通过选择启动菜单选项 2 FreeBSD 10.3 + 来实现 (FreeBSD 选项 4 8.x)，或从启动提示执行启动-s。

2. 在单用户模式下，为每个你 fstab （根和交换） 中列出的 IDE 磁盘分区中创建几何标签。 下面是 FreeBSD 10.3 的示例。

   ```bash
   # cat  /etc/fstab
   # Device           Mountpoint      FStype  Options   Dump   Pass#
   /dev/da0p2         /               ufs     rw        1       1
   /dev/da0p3         none            swap    sw        0       0

   # glabel  label rootfs  /dev/da0p2
   # glabel  label swap   /dev/da0p3
   # exit
   ```

   在可在几何标签上的其他信息：[标记磁盘设备](https://www.freebsd.org/doc/handbook/geom-glabel.html)。

3. 系统将继续与多用户启动。 启动完成后，编辑 /etc/fstab 和传统设备名称，替换为其各自的标签。 最终 /etc/fstab 将如下所示：

   ```
   # Device                Mountpoint      FStype  Options         Dump    Pass#
   /dev/label/rootfs       /               ufs     rw              1       1
   /dev/label/swap         none            swap    sw              0       0
   ```

4. 现在可以重新启动系统。 如果一切进展顺利，它就会出现通常会显示装载：

   ```
   # mount
   /dev/label/rootfs on / (ufs, local, journaled soft-updates)
   devfs on /dev (devfs, local, mutilabel)
   ```

## <a name="use-a-wireless-network-adapter-as-the-virtual-switch"></a>使用无线网络适配器的虚拟交换机

如果在主机上的虚拟交换机基于无线网络适配器，减少 ARP 过期时间为 60 秒以下的命令。 否则 VM 的网络可能会在一段时间后停止运行。


```
   # sysctl net.link.ether.inet.max_age=60
```


请参阅

* [受支持的 FreeBSD 虚拟机上的 HYPER-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)
