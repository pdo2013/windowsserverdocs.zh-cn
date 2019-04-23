---
title: HYPER-V 上运行 Linux 的最佳做法
description: 提供有关运行 Linux 的虚拟机上的建议
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a08648eb-eea0-4e2b-87fb-52bfe8953491
author: shirgall
ms.author: kathydav
ms.date: 3/1/2019
ms.openlocfilehash: 190a5e5d32140d6fa688bb9de98d05ec2f9783c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838678"
---
# <a name="best-practices-for-running-linux-on-hyper-v"></a>HYPER-V 上运行 Linux 的最佳做法

>适用于：Windows Server 2019，Windows Server 2016 中，HYPER-V Server 2016 中，Windows Server 2012 R2 中，Hyper-V Server 2012 R2 中，Windows Server 2012 中，Hyper-V Server 2012 的 Windows Server 2008 R2、 Windows 10、 Windows 8.1，Windows 8，Windows 7.1、 Windows 7

本主题包含有关 HYPER-V 上运行 Linux 虚拟机建议的列表。

## <a name="tuning-linux-file-systems-on-dynamic-vhdx-files"></a>优化动态 VHDX 文件上的 Linux 文件系统

即使在文件系统主要是为空时，某些 Linux 文件系统可能会占用大量的实际磁盘空间。 若要减少的动态 VHDX 文件的实际磁盘空间使用情况，请考虑以下建议：

* 在创建 VHDX 时，使用 1 MB BlockSizeBytes （从默认值 32 MB） 在 PowerShell 中，例如：

```Powershell
PS > New-VHD -Path C:\MyVHDs\test.vhdx -SizeBytes 127GB -Dynamic -BlockSizeBytes 1MB
```

* 为 ext3 优先 ext4 格式，因为 ext4 是比 ext3 与动态 VHDX 文件一起使用时有效的更多空间。

* 当创建文件系统指定组为 4096，例如的数：

```bash
# mkfs.ext4 -G 4096 /dev/sdX1

```

## <a name="grub-menu-timeout-on-generation-2-virtual-machines"></a>第 2 代虚拟机上的 grub 菜单超时时间

由于正在从中删除第 2 代虚拟机中的模拟的旧硬件，grub 菜单倒计时计时器向下计数过快 grub 菜单将显示立即加载默认条目。 修复 grub 使用 EFI 支持计时器，直到修改 **/boot/grub/grub.conf**，/**等/默认/grub**，或具有等效于"超时 = 100000"而不是默认值"超时 = 5"。

## <a name="pxe-boot-on-generation-2-virtual-machines"></a>第 2 代虚拟机上的 PxE 启动

因为不存在 Generation 2 Virtual Machines 中 PIT 计时器，则 PxE TFTP 服务器与网络连接可以提前终止，并防止从读取 Grub 配置，然后从服务器加载内核引导加载程序。

在 RHEL 6.x，旧 grub v0.97 EFI 引导加载程序可用来代替 grub2，如下所述： [https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

在 Linux 分发版之外 RHEL 6.x，可以执行类似的步骤来配置 grub v0.97 从 PxE 服务器加载的 Linux 内核。

此外，在 RHEL/CentOS 6.6 键盘和鼠标输入将无法工作与预安装内核这样可防止在菜单中指定的安装选项。 必须将串行控制台配置为允许选择安装选项。

* 在中**efidefault** PxE 服务器上的文件中，添加以下内核参数 **"控制台 = ttyS1"**

* 在 VM 上的 HYPER-V 中，安装程序使用此 PowerShell cmdlet 的 COM 端口：

```Powershell
Set-VMComPort -VMName <Name> -Number 2 -Path \\.\pipe\dbg1

```

指定对预安装内核 kickstart 文件还会避免键盘和鼠标输入在安装过程的需求。

## <a name="use-static-mac-addresses-with-failover-clustering"></a>使用静态 MAC 地址与故障转移群集

将使用故障转移群集部署的 Linux 虚拟机应配置每个虚拟网络适配器的静态媒体访问控制 (MAC) 地址。 在某些版本的 Linux，网络配置可能会丢失在故障转移后因为新的 MAC 地址分配给虚拟网络适配器。 若要避免丢失的网络配置，请确保每个虚拟网络适配器具有静态 MAC 地址。 可以通过编辑虚拟机的 HYPER-V 管理器或故障转移群集管理器中的设置来配置 MAC 地址。

## <a name="use-hyper-v-specific-network-adapters-not-the-legacy-network-adapter"></a>使用 Hyper V 特定网络适配器，不是旧版网络适配器

配置和使用虚拟以太网适配器，这是 Hyper V 特定网络卡的增强的性能。 如果旧版和 Hyper V 特定网络适配器附加到虚拟机，网络中的输出的名称**ifconfig-a**可能会显示随机值如 **_tmp12000801310**。 若要避免此问题，请删除所有旧版网络适配器时使用的 Linux 虚拟机中的 Hyper V 特定网络适配器。

## <a name="use-io-scheduler-noop-for-better-disk-io-performance"></a>为更好的磁盘 I/O 性能使用 I/O 计划程序 NOOP

Linux 内核具有四个不同的 I/O 计划程序进行重新排序具有不同的算法的请求。 NOOP 是传递由虚拟机监控程序的计划决定的先入先出队列。 建议 NOOP 计划程序运行时要使用 Linux 虚拟机上的 HYPER-V。 若要更改的特定设备时，在启动加载程序的配置中的计划程序 (/ etc/grub.conf，例如)，添加**电梯 = noop**到内核参数以及然后重新启动。

## <a name="numa"></a>NUMA

Linux 内核版本更早版本于 2.6.37 不支持 NUMA 的 HYPER-V 上具有更大的 VM 大小。 此问题主要影响旧分发版使用上游 Red Hat 2.6.32 内核和已修复在 Red Hat Enterprise Linux (RHEL) 6.6 (内核-2.6.32-504)。 运行自定义内核版本低于 2.6.37 或较旧的基于 RHEL 的内核比 2.6.32-504 必须设置启动参数的系统`numa=off`grub.conf 中的内核命令行上。 有关详细信息，请参阅[Red Hat KB 436883](https://access.redhat.com/solutions/436883)。

## <a name="reserve-more-memory-for-kdump"></a>保留 kdump 更多的内存

如果转储捕获内核会得到与上启动出现紧急情况，保留更多内核的内存。 例如，更改参数**crashkernel = 384M-:128M**到**crashkernel = 384M-:256M** Ubuntu grub 配置文件中。

## <a name="shrinking-vhdx-or-expanding-vhd-and-vhdx-files-can-result-in-erroneous-gpt-partition-tables"></a>收缩 VHDX 或扩展 VHD 和 VHDX 文件可能会导致错误的 GPT 分区表

HYPER-V 允许收缩而不考虑任何分区、 卷或文件系统数据结构可能存在于磁盘上的虚拟硬盘 (VHDX) 文件。 如果 VHDX 收缩到 VHDX 末尾派分区结束之前，可能会丢失数据，则分区的内容可能已损坏或无效的数据会变得可以读取该分区时返回。

后调整大小的 VHD 或 VHDX，管理员应使用 fdisk 之类的实用程序或 parted 更新分区、 卷和文件系统结构，以反映此更改中磁盘的大小。 收缩或扩展的 VHD 或 VHDX 具有 GUID 分区表 (GPT) 的大小将导致警告时使用分区管理工具来检查分区布局和管理员会收到警告，若要解决的第一个和辅助 GPT 标头。 这一手动步骤可以安全地执行而不丢失数据。

## <a name="see-also"></a>请参阅

* [Windows 上的 HYPER-V 的受支持的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)

* [HYPER-V 上运行 FreeBSD 的最佳做法](Best-practices-for-running-FreeBSD-on-Hyper-V.md)

* [部署的 HYPER-V 群集](https://technet.microsoft.com/library/jj863389.aspx)

* [创建适用于 Azure 的 Linux 映像](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/create-upload-generic)

* [优化 Azure 上 Linux VM](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/optimization)
