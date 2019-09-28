---
title: iSCSI 目标服务器的可伸缩性限制
TOCTitle: iSCSI Target Server Scalability Limits
ms.prod: windows-server
ms.technology: storage-iscsi
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 09/11/2018
ms.openlocfilehash: d92ed347288bc9a0dd3893148a31152ae8b8a313
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393989"
---
# <a name="iscsi-target-server-scalability-limits"></a>iSCSI 目标服务器的可伸缩性限制

适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题提供 Windows Server 上支持的和经过测试的 Microsoft iSCSI 目标服务器限制。 下表显示了经过测试的支持限制，并在适用情况下显示了是否强制限制。

## <a name="general-limits"></a>一般限制

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
<th><p>施加?</p></th>
<th><p>注释</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>每个 iSCSI 目标服务器的 iSCSI 目标实例数</p></td>
<td><p>256</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>iSCSI 逻辑单元（Lu）或每个 iSCSI 目标服务器的虚拟磁盘</p></td>
<td><p>512</p></td>
<td><p>否</p></td>
<td><p>测试配置包括：每个目标实例8个 Lu，平均超过64个目标，每个目标有一个 LU 的256目标实例。</p></td>
</tr>
<tr class="odd">
<td><p>iSCSI Lu 或每个 iSCSI 目标实例的虚拟磁盘</p></td>
<td><p>256（Windows Server 上的 128 2012）</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>可以同时连接到 iSCSI 目标实例的会话</p></td>
<td><p>544（Windows Server 上的 512 2012）</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>每个 LU 的快照数</p></td>
<td><p>512</p></td>
<td><p>是</p></td>
<td><p>每个独立 iSCSI 应用程序卷的快照数限制为512。</p></td>
</tr>
<tr class="even">
<td><p>本地装入的虚拟磁盘或每个存储设备的快照</p></td>
<td><p>32</p></td>
<td><p>是</p></td>
<td><p>本地装入的虚拟磁盘&#39;不提供任何特定于 iSCSI 的功能，已弃用-有关详细信息，请参阅<a href="https://technet.microsoft.com/library/dn303411.aspx">Windows Server 2012 R2 中删除或弃用的功能</a>。</p></td>
</tr>
</tbody>
</table>

## <a name="fault-tolerance-limits"></a>容错限制

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
<th><p>施加?</p></th>
<th><p>注释</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>故障转移群集节点</p></td>
<td><p>8（Windows Server 2012 上的5）</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多个活动群集节点</p></td>
<td><p>支持</p></td>
<td> 
<p>不可用</p></td>
<td><p>故障转移群集中的每个活动节点都拥有不同的 iSCSI 目标服务器群集实例，其他节点充当可能的所有者节点。</p></td>
</tr>
<tr class="odd">
<td><p>错误恢复级别（ERL）</p></td>
<td><p>0</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>每会话的连接数</p></td>
<td><p>1</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>可以同时连接到 iSCSI 目标实例的会话</p></td>
<td><p>544（Windows Server 上的 512 2012）</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>多路径输入/输出（MPIO）</p></td>
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
<td><p>将独立 iSCSI 目标服务器转换为群集 iSCSI 目标服务器，反之亦然</p></td>
<td><p>不支持</p></td>
<td><p>否</p></td>
<td><p>ISCSI 目标实例和虚拟磁盘配置数据（包括快照元数据）在转换过程中丢失。</p></td>
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
<th><p>施加?</p></th>
<th><p>注释</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>活动网络适配器的最大数目</p></td>
<td><p>8</p></td>
<td><p>否</p></td>
<td><p>适用于专用于 iSCSI 通信的网络适配器，而不是设备中网络适配器的总数。</p></td>
</tr>
<tr class="even">
<td><p>支持的门户（IP 地址）</p></td>
<td><p>64</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>网络端口速度</p></td>
<td><p>1Gbps，10 Gbps，40Gbps，56 Gbps （仅限 Windows Server 2012 R2 和更高版本）</p></td>
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
<td><p>利用大规模发送（分段）、校验和、中断裁决和 RSS 卸载</p></td>
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

## <a name="iscsi-virtual-disk-limits"></a>iSCSI 虚拟磁盘限制

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
<th><p>施加?</p></th>
<th><p>注释</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>从将虚拟磁盘从基本磁盘转换为动态磁盘的 iSCSI 发起程序 </p></td>
<td><p>是</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>虚拟硬盘格式</p></td>
<td><p>.vhdx （仅限 Windows Server 2012 R2 和更高版本）</p>
<p>.vhd</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VHD 最小格式大小</p></td>
<td><p>vhdx3 MB</p>
<p>硬盘8 MB</p></td>
<td><p>是</p></td>
<td><p>适用于所有受支持的 VHD 类型： parent、差分和 fixed。</p></td>
</tr>
<tr class="even">
<td><p>父 VHD 最大大小</p></td>
<td><p>vhdx64 TB</p>
<p>硬盘2 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>固定 VHD 最大大小</p></td>
<td><p>vhdx64 TB</p>
<p>硬盘16 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>差异 VHD 最大大小</p></td>
<td><p>vhdx64 TB</p>
<p>硬盘2 TB</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>VHD 固定格式</p></td>
<td><p>支持</p></td>
<td><p>否</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>VHD 差异格式</p></td>
<td><p>支持</p></td>
<td><p>否</p></td>
<td><p>快照不能用于基于 VHD 的差异 iSCSI 虚拟磁盘。</p></td>
</tr>
<tr class="odd">
<td><p>每个父 VHD 的差异 Vhd 数</p></td>
<td><p>256</p></td>
<td><p>否（在 Windows Server 2012 上为 "是"）</p></td>
<td><p>两个级别的深度（孙级文件）是 .vhdx 文件的最大值;一个深度级别（子 .vhd 文件）是 .vhd 文件的最大值。</p></td>
</tr>
<tr class="even">
<td><p>VHD 动态格式</p></td>
<td><p>vhdx是</p>
<p>硬盘是（在 Windows Server 2012 上为 No）</p></td>
<td><p>是</p></td>
<td><p>&#39;不支持取消映射。</p></td>
</tr>
<tr class="odd">
<td><p>exFAT/FAT32/FAT （VHD 的托管卷）</p></td>
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
<td><p>非 Microsoft CFS</p></td>
<td><p>不支持</p></td>
<td><p>是</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>精简预配</p></td>
<td><p>否</p></td>
<td><p>不可用</p></td>
<td><p>支持动态 Vhd，但不支持取消&#39;映射。</p></td>
</tr>
<tr class="odd">
<td><p>逻辑单元收缩</p></td>
<td><p>是（仅限 Windows Server 2012 R2 和更高版本）</p></td>
<td><p>不可用</p></td>
<td><p>使用<a href="https://docs.microsoft.com/powershell/module/iscsitarget/resize-iscsivirtualdisk">convert-iscsivirtualdisk</a>可收缩 LUN。</p></td>
</tr>
<tr class="even">
<td><p>逻辑单元克隆</p></td>
<td><p>不支持</p></td>
<td><p>不可用</p></td>
<td><p>可以使用差异 Vhd 快速克隆磁盘数据。</p></td>
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
<th><p>注释</p></th>
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
<td><p>快照–转换为完整</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照-联机回滚</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照–转换为可写</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>快照-重定向</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="even">
<td><p>快照固定</p></td>
<td><p>不支持</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>本地装载</p></td>
<td><p>支持</p></td>
<td><p>已弃用本地装载的 iSCSI 虚拟磁盘-有关详细信息，请参阅<a href="https://technet.microsoft.com/library/dn303411.aspx">Windows Server 2012 R2 中删除或弃用的功能</a>。 动态磁盘快照不能在本地装载。</p></td>
</tr>
</tbody>
</table>

## <a name="iscsi-target-server-manageability-and-backup"></a>iSCSI 目标服务器可管理性和备份

如果要从应用程序服务器中的 iSCSI 虚拟磁盘上创建卷影副本（VSS 文件快照），或想要使用需要虚拟磁盘服务（VDS）硬件的旧应用（例如 Diskraid 命令）来管理 iSCSI 虚拟磁盘提供程序，请将 iSCSI 目标存储提供程序安装在要从中拍摄快照的服务器上，或使用 VDS 管理应用程序。

ISCSI 目标存储提供程序是 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 中的角色服务。如果 iSCSI 目标服务器运行在 Windows Server 2012 上，你还可以在以下操作系统上下载和安装[适用于下级应用程序服务器的 Iscsi 目标存储提供程序（VDS/VSS）](http://www.microsoft.com/download/details.aspx?id=34759) ：

  - Windows Storage Server 2008 R2

  - Windows Server 2008 R2

  - Windows HPC Server 2008 R2

  - Windows HPC Server 2008

请注意，如果 iSCSI 目标服务器由运行 Windows Server 2012 R2 或更高版本的服务器托管，并且你想要从远程服务器使用 VSS 或 VDS，则远程服务器还必须运行同一版本的 Windows Server，并将 iSCSI 目标存储提供程序角色 servic已安装 e。 另请注意，在所有版本的 Windows 上，只应安装 iSCSI 目标存储提供程序角色服务的一个版本。

有关 iSCSI 目标存储提供程序的详细信息，请参阅[Iscsi 目标存储（VDS/VSS）提供程序](http://blogs.technet.com/b/filecab/archive/2012/10/08/iscsi-target-storage-vds-vss-provider.aspx)。

## <a name="tested-compatibility-with-iscsi-initiators"></a>已测试与 iSCSI 发起程序的兼容性

我们已通过以下 iSCSI 发起程序测试了 iSCSI 目标服务器软件：

<table>
<colgroup>
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
<col style="width: 25%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>初始</p></td>
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
<td><p>Windows Server 2012，Windows Server 2008 R2，Windows Server 2008，Windows Server 2003</p></td>
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
<td><p>VMWare ESXi 5。0</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>VMWare ESX 4。1</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>CentOS 1。x</p></td>
<td><p>已验证</p></td>
<td></td>
<td><p>必须注销会话并重新登录才能检测已调整大小的虚拟磁盘。</p></td>
</tr>
<tr class="even">
<td><p>Red Hat Enterprise Linux 6</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
<tr class="odd">
<td><p>RedHat Enterprise Linux 5 和5</p></td>
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
<td><p>Oracle Solaris 11. x</p></td>
<td><p>已验证</p></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>

我们还测试了以下 iSCSI 发起程序，它们从 iSCSI 目标服务器托管的虚拟磁盘执行无盘启动：

  - Windows Server 2012 R2

  - Windows Server 2012

  - PCIe NIC 与 iPXE

  - CD 或 USB 磁盘与 iPXE

## <a name="see-also"></a>请参阅

以下列表提供了有关 iSCSI 目标服务器和相关技术的附加资源。

- [iSCSI 目标块存储概述](iscsi-target-server.md)

- [iSCSI 目标启动概述](iscsi-boot-overview.md)

- [Windows Server 中的存储](../storage.md)

