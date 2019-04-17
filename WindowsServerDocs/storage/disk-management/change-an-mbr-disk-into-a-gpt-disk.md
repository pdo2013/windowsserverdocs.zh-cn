---
title: "将主启动记录 (MBR) 磁盘更改为 GUID 分区表 (GPT) 磁盘"
description: "介绍如何将主启动记录 (MBR) 磁盘更改为 GUID 分区表 (GPT) 磁盘"
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f28d151d3b1d6b19b9ca330c5ff8576a53acbfa5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-master-boot-record-mbr-disk-into-a-guid-partition-table-gpt-disk"></a>将主启动记录 (MBR) 磁盘更改为 GUID 分区表 (GPT) 磁盘

> **适用于：**Windows 10、Windows 8.1、Windows Server（半年频道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

主启动记录 (MBR) 磁盘使用标准 BIOS 分区表。 GUID 分区表 (GPT) 磁盘使用统一可扩展固件接口 (UEFI)。 GPT 磁盘的一个优势是每个磁盘上可以超过四个分区。 对于大于 2 TB 的磁盘，也需要 GPT。

只要磁盘不包含分区或卷，就可以将磁盘从 MBR 更改为 GPT 分区样式。
你不能在可移动介质上使用 GPT 分区样式，也不能在连接到群集服务所使用的共享 SCSI 或光纤通道总线的群集磁盘上使用。

> [!NOTE]
> 转换磁盘之前，请关闭这些磁盘上运行的任何程序。

## <a name="changing-an-mbr-disk-into-a-gpt-disk"></a>将 MBR 磁盘更改为 GPT 磁盘

-   [使用 Windows 界面](#BKMK_WINUI)
-   [使用命令行](#BKMK_CMD)

> [!NOTE]
> 你至少必须是**备份操作员**或**管理员**组的成员才能完成这些步骤。

<a id="BKMK_WINUI"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-the-windows-interface"></a>使用 Windows 界面将 MBR 磁盘更改为 GPT 磁盘

1.  备份或移动你想要转换为 GPT 磁盘的基本 MBR 磁盘上的数据。

2.  如果该磁盘包含任何分区或卷，请右键单击每一项，然后单击**删除分区**或**删除卷**。

3.  右键单击你想要更改为 GPT 磁盘的 MBR 磁盘，然后单击**转换成 GPT 磁盘**。

<a id="BKMK_CMD"></a>
#### <a name="to-change-an-mbr-disk-into-a-gpt-disk-using-a-command-line"></a>使用命令行将 MBR 磁盘更改为 GPT 磁盘

1.  备份或移动你想要转换为 GPT 磁盘的基本 MBR 磁盘上的数据。

2.  通过右键单击**命令提示符**，然后选择**以管理员身份运行**，以打开提升的命令提示符。

3. 键入 `diskpart`。 如果该磁盘未包含任何分区或卷，请跳到步骤 6。

4.  在 **DISKPART** 提示符下，键入 `list disk`。 请记下要转换的磁盘编号。

5.  在 **DISKPART** 提示符下，键入 `select disk <disknumber>`。

6.  在 **DISKPART** 提示符下，键入 `clean`。

> [!NOTE]
> 运行 **clean** 命令将删除磁盘上的所有分区或卷。

7.  在 **DISKPART** 提示符下，键入 `convert gpt`。

<br />

| 值  | 说明  |
| ----- | ----|
| <p>**list disk**</p> | <p>显示磁盘列表和有关磁盘的信息，例如磁盘大小、可用空间量、磁盘是基本磁盘还是动态磁盘，以及磁盘是使用主启动记录 (MBR) 还是 GUID 分区表 (GPT) 分区样式。 用星号 (*) 标记的磁盘具有焦点。</p> |
| <p>**select disk** <em>disknumber</em></p> | <p>选择指定的磁盘（其中 <em>disknumber</em> 是磁盘编号），并赋予其焦点。</p> |
| <p>**clean**</p> | <p>从具有焦点的磁盘中删除所有分区或卷。</p>  |
| <p>**convert gpt**</p>| <p>将具有主启动记录 (MBR) 分区样式的空基本磁盘转换为具有 GUID 分区表 (GPT) 分区样式的基本磁盘。</p> |

## <a name="see-also"></a>另请参阅

-   [命令行语法表示法](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


