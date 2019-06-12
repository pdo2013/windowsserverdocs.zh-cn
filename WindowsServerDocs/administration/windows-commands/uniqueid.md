---
title: uniqueid
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64235a4a-b91c-46da-b9b0-68ee90571c2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d237f4d6d3562e3787efe28ca98f9dc553d74898
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440751"
---
# <a name="uniqueid"></a>uniqueid



显示或设置的 GUID 分区表 (GPT) 标识符或主启动记录 (MBR) 签名具有焦点的磁盘。

> [!IMPORTANT]
> 此 DiskPart 命令不可在任何版本的 Windows Vista 中可用。

## <a name="syntax"></a>语法

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

## <a name="parameters"></a>Parameters

|  参数   |                                                                                             描述                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id={\<dword> |                                                                                               <GUID>}                                                                                                |
|    noerr     | 仅用于脚本。 当遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，错误会导致 DiskPart 退出，错误代码。 |

## <a name="remarks"></a>备注

-   此命令适用于基本和动态磁盘。
-   若要成功执行此命令，必须选择一个磁盘。 使用**选择的磁盘**命令选择某一磁盘，并将焦点移到它。

## <a name="BKMK_examples"></a>示例

若要显示的具有焦点的 MBR 磁盘签名，请键入：
```
uniqueid disk
```
若要设置为 5f1b2c36 具有焦点的 MBR 磁盘签名，请键入：
```
uniqueid disk id=5f1b2c36
```
若要设置为 baf784e7-6bbd-4cfb-aaac-e86c96e166ee 具有焦点的 GPT 磁盘的标识符，请键入：
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

#### <a name="additional-references"></a>其他参考

