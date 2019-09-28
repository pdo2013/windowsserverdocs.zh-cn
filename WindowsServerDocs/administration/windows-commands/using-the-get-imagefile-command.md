---
title: 使用 get-ImageFile 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6c8136585e04caca02ab16c7b4ca11a825cf400d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392132"
---
# <a name="using-the-get-imagefile-command"></a>使用 get-ImageFile 命令



检索有关 Windows 映像（.wim）文件中包含的映像的信息。

## <a name="syntax"></a>语法

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ImageFile： \<WIM 文件路径 >|指定 .wim 文件的完整路径和文件名。|
|[/Detailed]|返回每个映像中的所有图像元数据。 如果未使用此选项，则默认行为是只返回映像名称、说明和文件名。|

## <a name="BKMK_examples"></a>示例

若要查看有关映像的信息，请键入：
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
若要查看详细信息，请键入：
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)