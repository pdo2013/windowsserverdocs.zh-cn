---
title: 将动态磁盘更改回基本磁盘
description: 介绍如何将动态磁盘转换回基本磁盘。
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d018b417aeba21e2b1303fd9e72b58b4e2fc334f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836048"
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>将动态磁盘更改回基本磁盘

> **适用于：** Windows 10、 Windows 8.1、 Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

本主题介绍如何删除动态磁盘上的所有内容，然后将其转换回基本磁盘。 从 Windows 已弃用动态磁盘，我们不建议不再使用它们。 相反，我们建议使用基本磁盘或使用较新[存储空间](https://support.microsoft.com/help/12438/windows-10-storage-spaces)技术时所需为池磁盘一起为更大的卷。 如果要镜像 Windows 的引导卷，则可能需要使用硬件 RAID 控制器，例如许多主板上包含的 RAID 控制器。

> [!WARNING]
> 若要将动态磁盘转换回基本磁盘，你必须删除磁盘中的所有卷，并永久擦除磁盘上的所有数据。 请确保备份所有要保留的数据，然后再继续操作。

## <a name="changing-a-dynamic-disk-back-to-a-basic-disk"></a>将动态磁盘更改回基本磁盘

-   [使用 Windows 界面](#BKMK_WINUI)
-   [使用命令行](#BKMK_CMD)

> [!NOTE]
> 你至少必须是**备份操作员**或**管理员**组的成员才能完成这些步骤。

<a href="" id="BKMK_WINUI"></a>
#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-the-windows-interface"></a>使用 Windows 界面将动态磁盘更改回基本磁盘
1.  备份要从动态磁盘转换为基本磁盘的磁盘上的所有卷。

2.  在“磁盘管理”中，右键单击你想要转换为基本磁盘的动态磁盘上的每个卷，然后针对磁盘上的每个卷单击**删除卷**。

3.  删除了磁盘上的所有卷后，右键单击该磁盘，然后单击**转换成基本磁盘**。


<a href="" id="BKMK_CMD"></a>
#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-a-command-line"></a>使用命令行将动态磁盘更改回基本磁盘

1.  备份要从动态磁盘转换为基本磁盘的磁盘上的所有卷。

2.  打开命令提示符并键入 `diskpart`。

3.  在 **DISKPART** 提示符下，键入 `list disk`。 请记下要转换为基本磁盘的磁盘编号。

4.  在 **DISKPART** 提示符下，键入 `select disk <disknumber>`。

5.  在 **DISKPART** 提示符下，键入 `detail disk <disknumber>`。

6.  对于磁盘上的每个卷，在 **DISKPART** 提示符下，键入 `select volume= <volumenumber>`，然后键入 `delete volume`。

7.  在 **DISKPART** 提示符下，键入 `select disk <disknumber>`，并指定你希望转换为基本磁盘的磁盘编号。

8.  在 **DISKPART** 提示符下，键入 `convert basic`。
 
<br /> <br />

| ReplTest1  | 描述 |
| --- |---|
| <p>**list disk**</p>                         | <p>显示磁盘列表和有关磁盘的信息，例如磁盘大小、可用空间量、磁盘是基本磁盘还是动态磁盘，以及磁盘是使用主启动记录 (MBR) 还是 GUID 分区表 (GPT) 分区样式。 用星号 (*) 标记的磁盘具有焦点。</p> |
| <p>**选择磁盘** <em>disknumber</em></p>   | <p>选择指定的磁盘（其中 <em>disknumber</em> 是磁盘编号），并赋予其焦点。</p>  |
| <p>**明细数据磁盘** <em>disknumber</em></p>   | <p>显示所选磁盘的属性和该磁盘上的卷。</p>  |
| <p>**选择卷** <em>disknumber</em></p> | <p>选择指定的卷（其中 <em>disknumber</em> 是卷编号），并赋予其焦点。 如果未指定卷，则 **select** 命令会列出具有焦点的当前卷。 你可以通过编号、驱动器号或装入点路径来指定卷。 在基本磁盘上，如果选择卷，则还会赋予相应的分区焦点。</p> |
| <p>**删除卷**</p>                     | <p>删除所选的卷。 你无法删除系统卷、启动卷或任何包含活动分页文件或崩溃转储（内存转储）的卷。</p> |
| <p>**将基本转换**</p> | <p>将空动态磁盘转换为基本磁盘。</p>  |

## <a name="additional-considerations"></a>其他注意事项

-   将磁盘更改回基本磁盘之前，磁盘不得包含任何卷或数据。 如果你想要保留数据，请先将数据备份或移动到另一个卷上，然后再将磁盘转换为基本磁盘。
-   将动态磁盘更改回基本磁盘之后，你在该磁盘上只能创建分区和逻辑驱动器。

## <a name="see-also"></a>请参阅

-   [命令行语法表示法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


