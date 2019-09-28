---
title: 管理虚拟硬盘 (VHD)
description: 本文介绍如何管理虚拟硬盘
ms.date: 10/12/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6ffa7e9dc769b8d8c892d0af1ceae5246df62d3e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385806"
---
# <a name="manage-virtual-hard-disks-vhd"></a>管理虚拟硬盘 (VHD)

> **适用于：** Windows 10、Windows 8.1、Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍如何通过磁盘管理来创建、附加和分离虚拟硬盘。 虚拟硬盘 (VHD) 是虚拟化的硬盘文件，装载后，其外观和运行方式与物理硬盘大体相同。 它们最常与 Hyper-V 虚拟机配合使用。 

## <a name="viewing-vhds-in-disk-management"></a>在磁盘管理中查看 VHD

在磁盘管理中，VHD 看上去就像物理磁盘一样。 附加 VHD（即让其可供系统使用）后，它会显示为蓝色。 如果分离磁盘（即变为不可用），则其图标会恢复为灰色。

## <a name="creating-a-vhd"></a>创建 VHD

> [!NOTE]
> 至少必须是 **Backup Operators** 或 **Administrators** 组的成员才能完成这些步骤。

**创建 VHD**

1.  在“操作”菜单上，选择“创建 VHD”   。

2.  在“创建和附加虚拟硬盘”对话框中，指定要在物理计算机上的哪个位置存储 VHD 文件以及 VHD 的大小  。

3.  在“虚拟硬盘格式”中，选择“动态扩展”或“固定大小”，然后单击“确定”     。

## <a name="attaching-and-detaching-a-vhd"></a>附加和分离 VHD

若要使 VHD（刚才创建的 VHD 或另一个现有的 VHD）可供使用，请执行以下操作： 

1. 在“操作”菜单上，选择“附加 VHD”   。

2. 使用完全限定的路径指定 VHD 的位置。

若要分离 VHD，使其不可用，请执行以下操作：右键单击磁盘，选择“分离 VHD”，然后单击“确定”   。 分离 VHD 不会删除 VHD 或其中存储的任何数据。

## <a name="additional-considerations"></a>其他注意事项

-   用于指定 VHD 位置的路径必须是完全限定的路径，并且不能位于 \\Windows 目录中。
-   VHD 的最小大小为 3 兆字节 (MB)。
-   VHD 只能是基本磁盘。
-   因为创建 VHD 时会初始化 VHD，所以创建固定大小的大型 VHD 可能需要一些时间。
