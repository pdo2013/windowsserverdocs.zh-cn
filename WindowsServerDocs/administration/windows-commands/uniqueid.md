---
title: uniqueid
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 64f097766daa4c87ec84f42dd53f49792a160bb9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363907"
---
# <a name="uniqueid"></a>uniqueid



显示或设置具有焦点的磁盘的 GUID 分区表（GPT）标识符或主启动记录（MBR）签名。

> [!IMPORTANT]
> 在任何版本的 Windows Vista 中，此 DiskPart 命令均不可用。

## <a name="syntax"></a>语法

```
uniqueid disk [id={<dword> | <GUID>}] [noerr]
```

## <a name="parameters"></a>Parameters

|  参数   |                                                                                             描述                                                                                              |
|--------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id = {\<dword > |                                                                                               <GUID>}                                                                                                |
|    noerr     | 仅用于脚本编写。 遇到错误时，DiskPart 继续处理命令，就像未发生错误一样。 如果没有此参数，则错误会导致 DiskPart 退出并出现错误代码。 |

## <a name="remarks"></a>备注

-   此命令适用于基本磁盘和动态磁盘。
-   必须选择磁盘才能使此命令成功。 使用 "**选择磁盘**" 命令选择磁盘，并将焦点移动到该磁盘。

## <a name="BKMK_examples"></a>示例

若要显示具有焦点的 MBR 磁盘的签名，请键入：
```
uniqueid disk
```
若要将具有焦点的 MBR 磁盘的签名设置为5f1b2c36，请键入：
```
uniqueid disk id=5f1b2c36
```
若要将具有焦点的 GPT 磁盘的标识符设置为 baf784e7-6bbd-4cfb-aaac-e86c96e166ee，请键入：
```
uniqueid disk id=baf784e7-6bbd-4cfb-aaac-e86c96e166ee
```

#### <a name="additional-references"></a>其他参考

