---
title: 管理虚拟硬盘 (VHD)
description: 本文介绍了如何管理虚拟硬盘
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 2e371710752d59ebc7a1f8aa2dad3d9189872c47
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827108"
---
# <a name="manage-virtual-hard-disks-vhd"></a>管理虚拟硬盘 (VHD)

> **适用于：** Windows 10、 Windows 8.1、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主题介绍了如何通过磁盘管理来创建、连接和断开虚拟硬盘。 虚拟硬盘 (VHD) 是虚拟化的硬盘文件，安装后，其外观和运行方式与物理硬盘大体相同。 它们最常与 Hyper-V 虚拟机配合使用。 

## <a name="viewing-vhds-in-disk-management"></a>在“磁盘管理”中查看 VHD

在“磁盘管理”中，VHD 看上去就像物理磁盘一样。 连接 VHD（即让其可供系统使用）后，它会显示为蓝色。 如果断开了磁盘（即变为不可用），则其图标会恢复为灰色。

## <a name="creating-a-vhd"></a>创建 VHD

> [!NOTE]
> 你至少必须是**备份操作员**或**管理员**组的成员才能完成这些步骤。

**若要创建的 VHD**

1.  在**操作**菜单上，选择**创建 VHD**。

2.  在**创建和连接虚拟硬盘**对话框中，指定物理计算机上要在其中存储 VHD 文件的位置以及 VHD 的大小。

3.  在**虚拟硬盘格式**中，选择**动态扩展**或**固定大小**，然后单击**确定**。

## <a name="attaching-and-detaching-a-vhd"></a>连接和断开 VHD

使 VHD（你刚才创建的 VHD 或另一个现有的 VHD）可供使用： 

1. 在**操作**菜单上，选择**连接 VHD**。

2. 使用完全限定的路径指定 VHD 的位置。

若要分离 VHD，使其不可用：右键单击该磁盘中，选择**分离 VHD**，然后单击**确定**。 断开 VHD 不会删除 VHD 或其中存储的任何数据。

## <a name="additional-considerations"></a>其他注意事项

-   为 VHD 必须完全限定，并且不能在指定位置的路径\\Windows 目录。
-   VHD 的最小大小为 3 兆字节 (MB)。
-   VHD 只能是基本磁盘。
-   因为创建 VHD 时会初始化 VHD，所以创建大型固定大小的 VHD 可能需要一段时间。
