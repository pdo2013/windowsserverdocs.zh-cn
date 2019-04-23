---
title: break
description: Windows 命令主题**break_2** -取消关联从 VSS 卷影副本卷并使其成为普通的卷的可访问性。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de2b6c95-1c2e-4a43-bec5-341a9014371b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 49516539ae603e2c93b3fc395c77786be790d663
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886328"
---
# <a name="break"></a>break



取消关联从 VSS 卷影副本卷并使其常规卷的可访问性。 然后可使用的驱动器号 （如果指定） 或卷名称访问该卷。 如果使用不带参数，**中断**在命令提示符下显示的帮助。

> [!NOTE]
> 导入后，此命令将仅适用于硬件卷影副本。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
break [writable] <SetID>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[writable]|启用读/写访问的卷上。|
|\<SetID>|指定卷影副本集的 ID。|

## <a name="remarks"></a>备注

-   公开的卷，如它们源自的卷影副本是默认情况下以只读的。
-   卷影副本 ID，它将存储为一个环境变量的别名**加载元数据**命令，可在*SetID*参数。

## <a name="BKMK_examples"></a>示例

若要使卷影复制的别名名称 Alias1 在操作系统中的可写卷的可访问性，请键入：
```
break writable %Alias1%
```

> [!NOTE]
> 卷的访问权限直接对，如果没有记录至今已有的卷影副本卷的硬件提供程序。

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)