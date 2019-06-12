---
title: iSCSI 目标服务器可伸缩性限制
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server-threshold
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: d912047ab0e3136c6dc05064f3a28aaaafd36c79
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447722"
---
# <a name="iscsi-target-server-scalability-limits"></a>iSCSI 目标服务器可伸缩性限制

适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题提供支持和测试 Microsoft iSCSI 目标服务器限制在 Windows Server 上。 下表显示的经过测试的支持范围和在可用时，是否强制实施限制。

## <a name="general-limits"></a>常规限制

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>项目</p></th>
<th><p>支持限制</p></th>
<th><p>强制实施？</p></th>
<th><p>备注</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>每个 iSCSI 目标服务器的 iSCSI 目标实例</p></td>
<td><p>256</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>iSCSI 逻辑单元 (Lu) 或每个 iSCSI 目标服务器的虚拟磁盘</p></td>
<td><p>512</p></td>
<td><p>否</p></td>
<td><p>所包含的测试配置：每个使用平均超过目标的目标实例和具有每个目标的一个 LU 256 目标实例的 8 Lu。</p></td>
</tr>
<tr class="odd">
<td><p>iSCSI Lu 或每个 iSCSI 虚拟磁盘的目标实例</p></td>
<td><p>256 (Windows Server 2012 上 128)</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>可以同时连接到 iSCSI 目标实例的会话</p></td>
<td><p>544 (Windows Server 2012 上的 512)</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>每个 LU 快照</p></td>
<td><p>512</p></td>
<td><p>是</p></td>
<td><p>不存在的每个应用程序独立的 iSCSI 卷 512 个快照限制。</p></td>
</tr>
<tr class="even">
<td><p>本地装入的虚拟磁盘或快照，每个存储设备</p></td>
<td><p>32</p></td>
<td><p>是</p></td>
<td><p>本地装入的虚拟磁盘 don&#39;t 产品/服务的任何特定于 iSCSI 的功能和是否已弃用-有关详细信息，请参阅<a href="https://technet.microsoft.com/library/dn303411.aspx">Features Removed or Deprecated in Windows Server 2012 R2</a>。</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>容错能力限制

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>项目</p></th>
<th><p>支持限制</p></th>
<th><p>强制实施？</p></th>
<th><p>备注</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>故障转移群集节点</p></td>
<td><p>8 (Windows Server 2012 上的 5)</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多个活动群集节点</p></td>
<td><p>支持</p></td>
<td> 
<p>不可用</p></td>
<td><p>故障转移群集中的每个活动节点与其他节点作为可能的所有者节点拥有不同的 iSCSI 目标服务器的群集的实例。</p></td>
</tr>
<tr class="odd">
<td><p>错误恢复级别 (ERL)</p></td>
<td><p>0</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>每个会话的连接</p></td>
<td><p>1</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>可以同时连接到 iSCSI 目标实例的会话</p></td>
<td><p>544 (Windows Server 2012 上的 512)</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多路径输入/输出 (MPIO)</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>MPIO 路径</p></td>
<td><p>4</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>将独立的 iSCSI 目标服务器转换为聚集的 iSCSI 目标服务器，反之亦然</p></td>
<td><p>不支持</p></td>
<td><p>否</p></td>
<td><p>ISCSI 目标实例和虚拟磁盘配置数据，包括快照元数据在转换过程都将丢失。</p></td>
</tr>
</tbody>
</table>

## <a name="network-limits"></a>网络限制

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>项目</p></th>
<th><p>支持限制</p></th>
<th><p>强制实施？</p></th>
<th><p>备注</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>活动的网络适配器的最大数目</p></td>
<td><p>8</p></td>
<td><p>否</p></td>
<td><p>适用于网络适配器专用于 iSCSI 流量，而不是在设备中的网络适配器的总数。</p></td>
</tr>
<tr class="even">
<td><p>门户 （IP 地址） 支持</p></td>
<td><p>64</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>网络端口速度</p></td>
<td><p>1Gbps，则为 10 Gbps，40Gbps，56 Gbps (Windows Server 2012 R2 和更高版本仅)</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>IPv4</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPv6</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>TCP 卸载</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td><p>利用大型发送 （分段）、 校验和、 中断裁决和 RSS 卸载</p></td>
</tr>
<tr class="odd">
<td><p>iSCSI 卸载</p></td>
<td><p>不支持</p></td>
<td><br/><p>不可用</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>Jumbo 帧</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>IPSec</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CRC 卸载</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td></td>
</tr>
</tbody>
</table>

## <a name="iscsi-virtual-disk-limits"></a>iSCSI 虚拟磁盘的限制

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<thead>
<tr class="header">
<th><p>项目</p></th>
<th><p>支持限制</p></th>
<th><p>强制实施？</p></th>
<th><p>备注</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>从 iSCSI 发起程序将虚拟磁盘从基本磁盘转换为动态磁盘 </p></td>
<td><p>是</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>虚拟硬盘格式</p></td>
<td><p>.vhdx (Windows Server 2012 R2 和更高版本仅)</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>最小格式的 VHD 大小</p></td>
<td><p>.vhdx:3 MB</p>
<p>.vhd:8 MB</p></td>
<td><p>是</p></td>
<td><p>适用于所有受支持的 VHD 类型： 父，差异磁盘和修复。</p></td>
</tr>
<tr class="even">
<td><p>父 VHD 的最大大小</p></td>
<td><p>.vhdx:64 TB</p>
<p>.vhd:2 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>固定的 VHD 的最大大小</p></td>
<td><p>.vhdx:64 TB</p>
<p>.vhd:16 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>差异 VHD 的最大大小</p></td>
<td><p>.vhdx:64 TB</p>
<p>.vhd:2 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>固定格式 VHD</p></td>
<td><p>支持</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>差异 VHD 格式</p></td>
<td><p>支持</p></td>
<td><p>否</p></td>
<td><p>无法创建差异 VHD 基于 iSCSI 虚拟磁盘的快照。</p></td>
</tr>
<tr class="odd">
<td><p>每个父 VHD 差异 Vhd 数</p></td>
<td><p>256</p></td>
<td><p>否 （是取决于 Windows Server 2012)</p></td>
<td><p>两个级别 （孙级.vhdx 文件） 是深度的.vhdx 文件; 的最大值一个级别 （子.vhd 文件） 是深度的.vhd 文件的最大值。</p></td>
</tr>
<tr class="even">
<td><p>VHD 动态格式</p></td>
<td><p>.vhdx:是</p>
<p>.vhd:是 （Windows Server 2012 上的否）</p></td>
<td><p>是</p></td>
<td><p>取消映射并不是&#39;支持的 t。</p></td>
</tr>
<tr class="odd">
<td><p>（托管的 VHD 的卷） 的 exFAT/FAT32/FAT</p></td>
<td><p>不支持</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>CSV v2</p></td>
<td><p>不支持</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>ReFS</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>NTFS</p></td>
<td><p>支持</p></td>
<td><p>不可用</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Non-Microsoft CFS</p></td>
<td><p>不支持</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>精简预配</p></td>
<td><p>否</p></td>
<td><p>不可用</p></td>
<td><p>动态 Vhd 支持，但取消映射并不是&#39;支持的 t。</p></td>
</tr>
<tr class="odd">
<td><p>逻辑单元收缩</p></td>
<td><p>是 (Windows Server 2012 R2 和更高版本仅)</p></td>
<td><p>不可用</p></td>
<td><p>使用<a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">调整大小 iSCSIVirtualDisk</a>收缩 LUN。</p></td>
</tr>
<tr class="even">
<td><p>克隆逻辑单元</p></td>
<td><p>不支持</p></td>
<td><p>不可用</p></td>
<td><p>通过使用差异 Vhd，可以快速克隆磁盘数据。</p></td>
</tr>
</tbody>
</table>

## <a name="snapshot-limits"></a>快照数量上限

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>项目</p></th>
<th><p>支持限制</p></th>
<th><p>备注</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>快照创建</p></td>
<td><p>支持</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照还原</p></td>
<td><p>支持</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>可写快照</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照 – 将转换为完整</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照 – online 回滚</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照 – 将转换为可写</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照-重定向</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照-固定</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>本地装载</p></td>
<td><p>支持</p></td>
<td><p>本地装载的 iSCSI 虚拟磁盘不推荐使用的详细信息，请参阅<a href="https://technet.microsoft.com/library/dn303411.aspx">Features Removed or Deprecated in Windows Server 2012 R2</a>。 动态磁盘快照不能在本地装载。</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>iSCSI 目标服务器可管理性和备份

如果你想要从应用程序服务器的 iSCSI 虚拟磁盘上创建卷数据的卷影副本 （VSS 打开文件快照），或者你想要管理 iSCSI 虚拟磁盘，需要使用虚拟磁盘服务 (VDS) 硬件中的较旧的应用 （如 Diskraid 命令中）提供程序，你想要创建快照或使用 VDS 管理应用程序在服务器上安装 iSCSI 目标存储提供程序。

ISCSI 目标存储提供程序是 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012; 中的角色服务此外可以下载并安装[低级别应用程序服务器的 iSCSI 目标存储提供程序 (VDS/VSS)](http://www.microsoft.com/download/details.aspx?id=34759)以下操作系统，只要 Windows Server 2012 上运行 iSCSI 目标服务器上：

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

请注意，是否你想要从远程服务器使用 VSS 或 VDS iSCSI 目标服务器是托管运行 Windows Server 2012 R2 的服务器或更高版本，远程服务器必须也运行相同版本的 Windows Server，并且具有 iSCSI 目标存储提供程序角色服务安装 e。 另请注意，所有版本的 Windows 上应安装的 iSCSI 目标存储提供程序角色服务只有一个版本。

有关 iSCSI 目标存储提供程序的详细信息，请参阅[iSCSI 目标存储 (VDS/VSS) 提供程序](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx)。

## <a name="tested-compatibility-with-iscsi-initiators"></a>ISCSI 发起程序与已测试兼容性

我们已测试的 iSCSI 目标服务器与以下 iSCSI 发起程序软件：

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>发起方</p></td>
<td><p>Windows Server 2012 R2</p></td>
<td><p>Windows Server 2012</p></td>
<td><p>备注</p></td>
</tr>
<tr class="even">
<td><p>Windows Server 2012 R2</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Windows Server 2012 中，Windows Server 2008 R2 和 Windows Server 2008 中，Windows Server 2003</p></td>
<td><p>已验证</p></td>
<td><p>已验证</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare vSphere 5</p></td>
<td></td>
<td><p>已验证</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VMWare ESXi 5.0</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4.1</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 6.x</p></td>
<td><p>已验证</p></td>
<td></td>
<td><p>必须注销会话并重新登录，检测已调整大小的虚拟磁盘。</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 和 5</p></td>
<td><p>已验证</p></td>
<td><p>已验证</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>SUSE Linux Enterprise Server 10</p></td>
<td></td>
<td><p>已验证</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Oracle Solaris 11.x</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

我们已测试以下 iSCSI 发起程序的 iSCSI 目标服务器托管的虚拟磁盘从执行无盘启动：

  - Windows Server 2012 R2

  - Windows Server 2012

  - 在具有 iPXE PCIe NIC

  - 具有 iPXE 的 CD 或 USB 磁盘

## <a name="see-also"></a>请参阅

以下列表提供了有关 iSCSI 目标服务器和相关技术的附加资源。

- [iSCSI 目标块存储概述](iscsi-target-server.md)

- [iSCSI 目标启动概述](iscsi-boot-overview.md)

- [Windows Server 中存储](../storage.md)

