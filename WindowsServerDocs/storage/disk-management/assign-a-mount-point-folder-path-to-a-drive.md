---
title: '*为驱动器分配装入点文件夹路径。'
description: 本文介绍如何为驱动器分配装入点文件夹路径（而不是驱动器号）。
keywords: 虚拟化, 安全性, 恶意软件
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: a25dac6e49d6e9aee2efa043999162262c5bb791
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870868"
---
# <a name="assign-a-mount-point-folder-path-to-a-drive"></a>为驱动器分配装入点文件夹路径

> **适用于：** Windows 10、 Windows 8.1、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

你可以使用磁盘管理为驱动器分配装入点文件夹路径（而不是驱动器号）。 装入点文件夹路径仅适用于基本或动态 NTFS 卷上的空文件夹。

## <a name="assigning-a-mount-point-folder-path-to-a-drive"></a>为驱动器分配装入点文件夹路径

-   [使用 Windows 界面](#BKMK_WINUI)
-   [使用命令行](#BKMK_CMD)

> [!NOTE]
> 你至少必须是**备份操作员**或**管理员**组的成员才能完成这些步骤。

**使用 Windows 界面为驱动器分配装入点文件夹路径：**
<a id="BKMK_WINUI"></a>

1.  在磁盘管理器中，右键单击想要为其分配装入点文件夹路径的分区或卷。 
2. 单击**更改驱动器号和路径**，然后单击**添加**。 
3. 单击**装入以下空白 NTFS 文件夹中**。
4. 键入 NTFS 卷上空文件夹的路径，或者单击**浏览**以找到它。

<a id="BKMK_CMD"></a>
#### <a name="to-assign-a-mount-point-folder-path-to-a-drive-using-a-command-line"></a>使用命令行为驱动器分配装入点文件夹路径
1.  打开命令提示符并键入 `diskpart`。

2.  在 **DISKPART** 提示符下，键入 `list volume`，并记下你想要为其分配路径的卷编号。

3.  在 **DISKPART** 提示符下，键入 `select volume <volumenumber>`。 

4. 选择想要为其分配路径的简单卷 *volumenumber*。

5.  在 **DISKPART** 提示符下，键入 `assign [mount=<path>]`。

#### <a name="to-remove-a-mount-point-folder-path-to-a-drive"></a>删除驱动器的装入点文件夹路径

-   若要删除装入点文件夹路径，请单击路径，然后单击**删除**。

<br />

| 值 | 描述 |
| --- | --- |
| <p>**list volume**</p> | <p>显示所有磁盘上的基本卷和动态卷的列表。</p> |
| <p>**选择卷**</p>        | <p>选择指定的卷（其中 <em>volumenumber</em> 是卷编号），并赋予其焦点。 如果未指定卷，则 **select** 命令会列出具有焦点的当前卷。 你可以通过编号、驱动器号或装入点文件夹路径来指定卷。 在基本磁盘上，如果选择卷，则还会赋予相应的分区焦点。</p>|
| <p>**assign**</p> | <p><ul><li> 将驱动器号或装入点文件夹路径分配给具有焦点的卷。 如果未指定驱动器号或装入点文件夹路径，则将分配下一个可用的驱动器号。 如果驱动器号或装入点文件夹路径已被占用，则会出错。</li> </p> <p><li>使用 **assign** 命令，你可以更改与可移动驱动器相关联的驱动器号。</li> </p><p><li> 你无法将驱动器号分配给引导卷或包含分页文件的卷。 此外，不能将驱动器号分配给原始设备制造商 (OEM) 分区、EFI 系统分区或基本数据分区以外的其他任何 GPT 分区。</p></li></ul> |
| <p>**mount=** <em>path</em></p> | <p>指定安装的驱动器所位于的现有空 NTFS 文件夹。</p>  |

## <a name="additional-considerations"></a>其他注意事项

-   如果你管理的是本地或远程计算机，则可以浏览该计算机上的 NTFS 文件夹。
-   装入点文件夹路径仅适用于基本或动态 NTFS 卷上的空文件夹。
-   若要修改装入点文件夹路径，请删除它，然后使用新位置创建新文件夹路径。 你无法直接修改装入点文件夹路径。
-   为驱动器分配装入点文件夹路径时，请使用**事件查看器**检查系统日志中指明装入点文件夹路径故障的任何群集服务错误或警告。 这些错误会在**源**列中列为 **ClusSvc**，在**类别**列中列为**物理磁盘资源**。
-   你还可以使用 [mountvol](https://go.microsoft.com/fwlink/?linkid=64111) 命令创建已装载的驱动器。

## <a name="see-also"></a>请参阅
-   [命令行语法表示法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


