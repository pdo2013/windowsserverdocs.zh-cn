---
title: 在 Hyper-v 上运行 Linux 的最佳实践
description: 提供在虚拟机上运行 Linux 的建议
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: 3488bbc1e295a68befc7044b83379bd65a5f28df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365566"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>在 Hyper-v 上运行 Linux 的最佳实践

>适用于：Windows Server 2019, Windows Server 2016, Hyper-v Server 2016, Windows Server 2012 R2, Hyper-v server 2012 R2, Windows Server 2012, Hyper-v Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

本主题包含在 Hyper-v 上运行 Linux 虚拟机的建议列表。

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>优化动态 VHDX 文件上的 Linux 文件系统

即使文件系统主要为空，某些 Linux 文件系统也可能会消耗大量的实际磁盘空间。 若要减少动态 VHDX 文件的实际磁盘空间使用情况，请考虑以下建议：

* 创建 VHDX 时，请在 PowerShell 中使用 BlockSizeBytes （来自默认32MB），例如：

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* Ext4 格式优先于 ext3，因为在与动态 VHDX 文件一起使用时，ext4 比 ext3 更节省空间。

* 创建 filesystem 时，请将组数指定为4096，例如：

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>第2代虚拟机上的 Grub 菜单超时

由于从第2代虚拟机的模拟中删除了旧硬件，因此，将显示 grub 菜单倒计时计时器，以便显示该 grub 菜单，立即加载默认条目。 在将 grub 固定到使用支持 EFI 的计时器之前，请修改 **/boot/grub/grub.conf**、/**etc/default/grub**，或等效于使用 "timeout = 100000" 而不是默认值 "timeout = 5"。

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>第2代虚拟机上的 PxE 启动

由于第2代虚拟机中不存在 PIT 计时器，因此到 PxE TFTP 服务器的网络连接可能会提前终止，并阻止加载服务器从服务器读取 Grub 配置和加载内核。

在 RHEL 1.x 上，可以使用旧版 grub v 0.97 EFI 引导程序，而不是 grub2，如下所述： [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

在 RHEL 1.x 以外的 Linux 分发版上，可以遵循类似的步骤配置 grub v 0.97，以便从 PxE 服务器加载 Linux 内核。

此外，在 RHEL/CentOS 6.6 键盘和鼠标输入无法与预安装内核一起使用，这会阻止在菜单中指定安装选项。 必须将串行控制台配置为允许选择安装选项。

* 在 PxE 服务器上的**efidefault**文件中，添加以下内核参数 **"console = ttyS1"**

* 在 Hyper-v 中的 VM 上，使用以下 PowerShell cmdlet 设置 COM 端口：

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

将 kickstart 文件指定到预安装内核还将避免在安装过程中需要键盘和鼠标输入。

## <a name="use-static-mac-addresses-with-failover-clustering"></a>在故障转移群集中使用静态 MAC 地址

应使用故障转移群集部署的 Linux 虚拟机配置每个虚拟网络适配器的静态媒体访问控制（MAC）地址。 在某些版本的 Linux 中，故障转移后可能会丢失网络配置，因为已将新的 MAC 地址分配给虚拟网络适配器。 若要避免丢失网络配置，请确保每个虚拟网络适配器都有一个静态 MAC 地址。 可以通过在 Hyper-v 管理器或故障转移群集管理器中编辑虚拟机的设置来配置 MAC 地址。

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>使用 Hyper-v 特定的网络适配器，而不是旧的网络适配器

配置并使用虚拟以太网适配器，该适配器是一种具有增强性能的 Hyper-v 特定网卡。 如果旧网络适配器和 Hyper-v 特定网络适配器均连接到虚拟机，则**ifconfig**的输出中的网络名称可能会显示随机值（如 **_tmp12000801310**）。 若要避免此问题，请在 Linux 虚拟机中使用 Hyper-v 特定的网络适配器时，删除所有旧版网络适配器。

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>使用 i/o 计划程序 NOOP 获得更好的磁盘 i/o 性能

Linux 内核具有四个不同的 i/o 计划程序，用不同的算法对请求重新排序。 NOOP 是一个先进先出队列，该队列通过虚拟机监控程序做出的计划决策。 在 Hyper-v 上运行 Linux 虚拟机时，建议使用 NOOP 作为计划程序。 若要更改特定设备的计划程序，请在启动加载程序的配置（例如/etc/grub.conf）中，将**电梯 = noop**添加到内核参数，然后重新启动。

## <a name="numa"></a>NUMA

早于2.6.37 的 Linux 内核版本不支持具有更大 VM 大小的 Hyper-v 上的 NUMA。 此问题主要影响使用上游 Red Hat 2.6.32 内核的旧发行版，并已修复 Red Hat Enterprise Linux （RHEL）6.6 （2.6.32-504）。 运行早于2.6.37 的自定义内核的系统或 2.6.32-504 之前的基于 RHEL 的内核必须在中的内核命令行上设置启动参数 `numa=off`。 有关详细信息，请参阅[Red HAT KB 436883](https://access.redhat.com/solutions/436883)。

## <a name="reserve-more-memory-for-kdump"></a>为 kdump 保留更多内存

如果转储捕获内核在启动时出现死机，请为内核保留更多内存。 例如，在 Ubuntu grub 配置文件中将参数**crashkernel = 384M-： 128M**更改为**crashkernel = 384M-： 256M** 。

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>缩小 VHDX 或扩展 VHD 和 VHDX 文件可能导致 GPT 分区表错误

Hyper-v 允许压缩虚拟磁盘（VHDX）文件，而不考虑磁盘上可能存在的任何分区、卷或文件系统数据结构。 如果 VHDX 的结束时间收缩到在分区结束之前的 VHDX 位置，则数据可能会丢失，分区可能会损坏，或者在读取分区时返回的数据无效。

调整 VHD 或 VHDX 大小后，管理员应使用诸如 fdisk 或 parted 的实用工具来更新分区、卷和文件系统结构，以反映磁盘大小的变化。 收缩或扩展包含 GUID 分区表（GPT）的 VHD 或 VHDX 的大小会在分区管理工具用于检查分区布局时产生警告，并会警告管理员修复第一个和第二个 GPT 标头。 此手动步骤可以安全地执行，而不会丢失数据。

## <a name="see-also"></a>请参阅

* [Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [在 Hyper-v 上运行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [部署 Hyper-v 群集](https://technet.microsoft.com/library/jj863389.aspx)

* [创建适用于 Azure 的 Linux 映像](https://docs.microsoft.com/azure/virtual-machines/linux/create-upload-generic)

* [在 Azure 上优化 Linux VM](https://docs.microsoft.com/azure/virtual-machines/linux/optimization)
