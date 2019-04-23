---
title: 用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明
description: 介绍使用 Linux 和 FreeBSD 虚拟机上时，会影响核心组件，如网络、 存储、 内存的功能
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: 944f8e9d902953ab4d6da0750603a2c40fa9e96d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844888"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>用于在 HYPER-V 上的 Linux 和 FreeBSD 虚拟机的功能说明

>适用于：Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

使用 Linux 和 FreeBSD 虚拟机上时，本文将介绍组件，如 core、 网络、 存储和内存中的可用功能。

## <a name="BKMK_core"></a>核心

|**功能**|**说明**|
|-|-|
|集成的关闭|借助此功能，管理员可以关闭虚拟机从 HYPER-V 管理器。 有关详细信息，请参阅[操作系统关闭](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown)。|
|时间同步|此功能可确保在虚拟机的维护时间保持的维护时间与同步主机上。 有关详细信息，请参阅[时间同步](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time)。|
|Windows Server 2016 准确的时间|此功能允许来宾使用的 Windows Server 2016 准确的时间的功能，从而提高了与主机到 1 毫秒的时间同步准确性。 有关详细信息，请参阅[Windows Server 2016 准确的时间](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|Multiprocessing 支持|借助此功能，虚拟机可以使用多个处理器的主机上配置多个虚拟 Cpu。|
|检测信号|借助此功能，主机可以跟踪虚拟机的状态。 有关详细信息，请参阅[检测信号](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat)。|
|集成的鼠标支持|使用此功能，可以在虚拟机的桌面上使用鼠标和虚拟机还使用 Windows Server 桌面和 HYPER-V 控制台之间无缝地鼠标。|
|HYPER-V 特定存储设备|此功能授予对附加到虚拟机的存储设备的高性能访问权限。|
|HYPER-V 特定网络设备|此功能授予给附加到虚拟机的网络适配器的高性能访问权限。|

## <a name="BKMK_Networking"></a>网络

|**功能**|**说明**|
|-|-|
|Jumbo 帧|使用此功能，管理员可以增加网络帧超过 1500 个字节，这将导致显著提高网络性能的大小。|
|VLAN 标记和中继|此功能，可配置的虚拟机的虚拟 LAN (VLAN) 流量。|
|实时迁移|借助此功能，可以从一个主机到另一台主机迁移虚拟机。 有关详细信息，请参阅[虚拟机实时迁移概述](https://technet.microsoft.com/library/hh831435.aspx)。|
|静态 IP 注入|使用此功能，可以复制虚拟机的静态 IP 的地址，它已故障转移后到一台主机上其副本。 此类 IP 复制可确保网络工作负荷继续无缝故障转移事件之后。|
|（虚拟接收方扩展） 的 vRSS|从虚拟网络适配器的负载分散在虚拟机中的多个虚拟处理器。有关详细信息，请参阅[Windows Server 2012 R2 中的虚拟接收方缩放](https://technet.microsoft.com/library/dn383582.aspx)。|
|TCP 分段和校验和卸载|传输分段和校验和工作从来宾 CPU 到主机虚拟交换机或网络适配器在网络数据传输过程。|
|大型接收卸载 （进行 lro 操作）|增加的聚合成较大的缓冲区，减少 CPU 开销的多个数据包通过高带宽连接的入站的吞吐量。|
|SR-IOV|单根 I/O 设备使用 DDA 允许来宾访问权限，允许以降低的延迟和更高的吞吐量的特定 NIC 卡的部分。 SR-IOV 需要主机上的最新的物理函数 (PF) 驱动程序和在来宾机器上的虚拟函数 (VF) 驱动程序。|

## <a name="BKMK_Storage"></a>存储

|**功能**|**说明**|
|-|-|
|VHDX 大小调整|使用此功能，管理员可以调整大小已附加到虚拟机的大小固定的.vhdx 文件。 有关详细信息，请参阅[联机虚拟硬盘大小调整概述](https://technet.microsoft.com/library/dn282286.aspx)。|
|虚拟光纤通道|使用此功能，虚拟机可以识别的光纤通道设备和本机装载。 有关详细信息，请参阅 [Hyper-V 虚拟光纤通道概述](https://technet.microsoft.com/library/hh831413.aspx)。|
|实时虚拟机备份|此功能便于零故障时间的实时虚拟机的备份。<br /><br />请注意，如果虚拟机具有虚拟硬盘 (Vhd) 中承载的远程存储，如服务器消息块 (SMB) 共享或 iSCSI 卷备份操作不成功。 此外，请确保备份目标不在同一个卷作为备份的卷。|
|剪裁支持|剪裁提示通知某些以前分配的扇区不再需要的应用程序，并且可以清除的驱动器。 应用程序的文件提供使大的空间分配，然后自助管理分配到该文件，例如，虚拟硬盘文件时，通常使用此过程。|
|SCSI WWN|Storvsc 驱动程序的端口和设备连接到虚拟机的节点从提取全球通用名称 (WWN) 的信息并创建相应 sysfs 文件。 |

## <a name="BKMK_Memory"></a>内存

|**功能**|**说明**|
|-|-|
|PAE 内核支持|物理地址扩展 (PAE) 技术允许 32 位内核以访问大于 4 GB 的物理地址空间。 较旧的 Linux 分发版，如 RHEL 5.x 用于发布已启用的 PAE 单独内核。 更新的分发版如 RHEL 6.x 有预建 PAE 支持。|
|MMIO 间隙的配置|使用此功能，设备制造商可以配置内存映射 I/O (MMIO) 间隙的位置。 MMIO 间隙通常用于将设备的只是足够的操作系统 (JeOS) 和为设备提供支持的软件的实际基础结构之间的可用物理内存。|
|动态内存的热添加|主机可以动态增加或减少可到虚拟机的内存量，在操作时。 设置之前，管理员在虚拟机设置面板中启用动态内存，并为虚拟机指定的启动内存、 最小内存和最大内存。 可以更改当虚拟机处于无法禁用动态内存的操作，并仅的最小值和最大值设置。 （它是指定这些内存大小为 128 MB 的倍数的最佳做法。）<br /><br />当虚拟机首次启动可用内存是否等于**启动内存**。 由于应用程序工作负荷增加内存需求的 HYPER-V 可能会动态分配到虚拟机的热添加机制，通过更多的内存如果内核的该版本支持的。 最大分配的内存量的上限的值**最大内存**参数。<br /><br />Hyper-v 管理器内存选项卡将显示分配给虚拟机的内存量，但虚拟机中的内存统计信息将显示最高的分配的内存量。<br /><br />有关详细信息，请参阅[HYPER-V 动态内存概述](https://technet.microsoft.com/library/hh831766.aspx)。<br /><br />|
|动态内存-扩大|主机可以动态增加或减少可到虚拟机的内存量，在操作时。 设置之前，管理员在虚拟机设置面板中启用动态内存，并为虚拟机指定的启动内存、 最小内存和最大内存。 可以更改当虚拟机处于无法禁用动态内存的操作，并仅的最小值和最大值设置。 （它是指定这些内存大小为 128 MB 的倍数的最佳做法。）<br /><br />当虚拟机首次启动可用内存是否等于**启动内存**。 由于应用程序工作负荷增加内存需求的 HYPER-V 可能会动态分配到虚拟机通过热添加机制 （上述） 的更多内存。 当内存需求减少的 HYPER-V 可能会自动取消设置从通过气球机制的虚拟机的内存。 HYPER-V 将取消设置以下内存**最小内存**参数。<br /><br />Hyper-v 管理器内存选项卡将显示分配给虚拟机的内存量，但虚拟机中的内存统计信息将显示最高的分配的内存量。<br /><br />有关详细信息，请参阅[HYPER-V 动态内存概述](https://technet.microsoft.com/library/hh831766.aspx)。<br /><br />|
|运行时内存调整大小|管理员可以设置可用内存量到虚拟机过程中操作，增加 （"热添加内存"） 或减少它 （"热删除"）。 内存返回到 HYPER-V 通过气球驱动程序 （请参阅"动态内存-扩大"）。 气球驱动程序维护的最小可用内存后扩大，名为"floor"，因此分配的内存量不能降低以下的当前需求以及此 floor 量。 Hyper-v 管理器内存选项卡将显示分配给虚拟机的内存量，但虚拟机中的内存统计信息将显示最高的分配的内存量。 （它是内存值指定为 128 MB 的倍数的最佳做法。）|

## <a name="BKMK_Video"></a>Video

|**功能**|**说明**|
|-|-|
|Hyper V 特定视频设备|此功能为虚拟机提供高性能图形和卓越的解决方法。 此设备不提供增强会话模式或 RemoteFX 的功能。|

## <a name="BKMK_Misc"></a>杂项

|**功能**|**说明**|
|-|-|
|KVP （键-值对） exchange|此功能所提供键/值对 (KVP) exchange 服务的虚拟机。 管理员通常使用 KVP 机制可以执行读取和写入虚拟机上的自定义数据操作。 有关详细信息，请参阅[数据交换：使用键-值对，以便共享信息的 HYPER-V 主机和来宾之间](https://technet.microsoft.com/library/dn798287.aspx)。|
|不可屏蔽的中断|借助此功能，管理员可以向虚拟机发出非屏蔽中断 (NMI)。 NMIs 可获取已变得无响应，由于应用程序错误的操作系统的故障转储。 重启后，可以诊断这些故障转储。|
|从主机到来宾文件副本|此功能允许文件将从主机物理计算机复制到来宾虚拟机而无需使用网络适配器。 有关详细信息，请参阅[来宾服务](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest)。|
|lsvmbus 命令|此命令将获取有关设备上的 HYPER-V 虚拟机总线 (VMBus) 的信息类似于 lspci 等信息命令。|
|HYPER-V 套接字|这是主机和来宾操作系统之间的更多通信通道。 若要加载和使用 HYPER-V 套接字内核模块，请参阅[使您自己的集成服务](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service)。|
|PCI 传递/DDA|使用 Windows Server 2016 管理员可以通过 PCI Express 设备通过传递离散设备分配机制。 常见设备是网卡、 图形卡和特殊存储设备。 虚拟机将需要相应的驱动程序使用公开的硬件。 硬件必须分配给虚拟机，才能使用。<br /><br />有关详细信息，请参阅[离散设备分配-说明和背景](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)。<br /><br />DDA 是 SR-IOV 网络的先决条件。 虚拟端口需要分配给虚拟机和虚拟机必须使用正确的虚拟函数 (VF) 驱动程序设备多路复用。|

## <a name="BKMK_gen2"></a>第 2 代虚拟机

|**功能**|**说明**|
|-|-|
|使用 UEFI 启动|此功能允许虚拟机，以启动使用统一可扩展固件接口 (UEFI)。<br /><br />有关详细信息，请参阅[第 2 代虚拟机概述](https://technet.microsoft.com/library/dn282285.aspx)。|
|安全启动|此功能允许虚拟机，若要使用基于 UEFI 安全启动模式。 当虚拟机在安全模式下启动时，使用 UEFI 数据存储区中存在的签名验证各个操作系统组件。<br /><br />有关详细信息，请参阅[安全启动](https://technet.microsoft.com/library/dn486875.aspx)。|

## <a name="see-also"></a>请参阅

* [支持 CentOS 和 Red Hat Enterprise Linux 虚拟机上的 HYPER-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [在 HYPER-V 上支持的 Debian 虚拟机](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [受支持的 Oracle Linux 虚拟机上的 HYPER-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [受支持的 SUSE 虚拟机上的 HYPER-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [受支持的 Ubuntu 虚拟机上的 HYPER-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [受支持的 FreeBSD 虚拟机上的 HYPER-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [HYPER-V 上运行 Linux 的最佳做法](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [HYPER-V 上运行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)