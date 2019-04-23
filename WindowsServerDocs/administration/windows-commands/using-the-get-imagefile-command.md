---
title: 使用 get ImageFile 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bbe5ece95d1f9821a27b96e56bc34576a0f5f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827618"
---
# <a name="using-the-get-imagefile-command"></a>使用 get ImageFile 命令



检索有关 Windows 映像 (.wim) 文件中包含的映像的信息。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ ImageFile:\<WIM 文件路径 >|指定的.wim 文件的完整路径和文件名。|
|[/ 详细]|从每个图像返回图像的所有元数据。 如果不使用此选项，默认行为是返回映像名称、 说明和文件的名称。|

## <a name="BKMK_examples"></a>示例

若要查看有关映像的信息，请键入：
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
若要查看详细的信息，请键入：
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)