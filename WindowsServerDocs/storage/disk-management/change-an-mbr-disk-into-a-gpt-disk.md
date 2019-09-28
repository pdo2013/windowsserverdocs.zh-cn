---
title: 将主启动记录 (MBR) 磁盘更改为 GUID 分区表 (GPT) 磁盘
description: 介绍如何将主启动记录 (MBR) 磁盘更改为 GUID 分区表 (GPT) 磁盘
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6bd97802fbef342520e92a857a1a53acf3e8d7a3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385940"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>将 MBR 磁盘转换为 GPT 磁盘

> **适用于：** Windows 10、Windows 8.1、Windows Server（半年频道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

主启动记录 (MBR) 磁盘使用标准 BIOS 分区表。 GUID 分区表 (GPT) 磁盘使用统一可扩展固件接口 (UEFI)。 GPT 磁盘的一个优势是每个磁盘上可以超过四个分区。 对于大于 2 TB 的磁盘，也需要 GPT。

只要磁盘不包含分区或卷，就可以将磁盘从 MBR 更改为 GPT 分区样式。

> [!NOTE]
> 在转换某个磁盘之前，请备份其上的所有数据，并关闭任何正在访问该磁盘的程序。

> [!NOTE]
> 至少必须是“备份操作员”  或“管理员”  组的成员才能完成这些步骤。

## <a name="converting-using-the-windows-interface"></a>使用 Windows 界面转换

1.  备份或移动你想要转换为 GPT 磁盘的基本 MBR 磁盘上的数据。

2.  如果该磁盘包含任何分区或卷，请右键单击每一项，然后单击“删除分区”或“删除卷”   。

3.  右键单击你想要更改为 GPT 磁盘的 MBR 磁盘，然后单击“转换成 GPT 磁盘”  。

## <a name="converting-using-a-command-line"></a>使用命令行转换

使用以下步骤将空的 MBR 磁盘转换为 GPT 磁盘。 也可以使用 MBR2GPT.EXE 工具，不过这有点复杂 - 有关更多详细信息，请参阅[将 MBR 分区转换成 GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt)。

1.  备份或移动你想要转换为 GPT 磁盘的基本 MBR 磁盘上的数据。

2.  右键单击“命令提示符”并选择“以管理员身份运行”，打开提升的命令提示符   。

3. 键入 `diskpart`。 如果该磁盘未包含任何分区或卷，请跳到步骤 6。

4.  在 **DISKPART** 提示符下，键入 `list disk`。 请记下要转换的磁盘编号。

5.  在 **DISKPART** 提示符下，键入 `select disk <disknumber>`。

6.  在 DISKPART  提示符下，键入 `clean`。

    > [!NOTE]
    > 运行 clean  命令将删除磁盘上的所有分区或卷。

7.  在 DISKPART  提示符下，键入 `convert gpt`。

| 值  | 描述  |
| ----- | ---- |
| **list disk** | 显示磁盘列表和有关磁盘的信息，例如磁盘大小、可用空间量、磁盘是基本磁盘还是动态磁盘，以及磁盘是使用主启动记录 (MBR) 还是 GUID 分区表 (GPT) 分区样式。 用星号 (*) 标记的磁盘具有焦点。 |
| select disk  *disknumber* | 选择指定的磁盘（其中 *disknumber* 是磁盘编号），并赋予其焦点。 |
| **clean** | 从具有焦点的磁盘中删除所有分区或卷。  |
| **convert gpt**| 将具有主启动记录 (MBR) 分区样式的空基本磁盘转换为具有 GUID 分区表 (GPT) 分区样式的基本磁盘。 |

## <a name="see-also"></a>另请参阅

-   [命令行语法表示法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)