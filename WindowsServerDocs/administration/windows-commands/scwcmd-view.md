---
title: Scwcmd 视图
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7995959a-d93e-4865-a6a0-2ab18c2bb47f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a69db16696f42950af97b62ba6f28c4083954137
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371190"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> 适用于：Windows Server 2012 R2、Windows Server 2012

使用指定的 .xsl 转换呈现 .xml 文件。 此命令可用于通过使用不同的视图显示安全配置向导（SCW） .xml 文件。

## <a name="syntax"></a>语法

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/x： \<Xmlfile >|指定要查看的 .xml 文件。 必须指定此参数。|
|/s： \<Xslfile >|指定作为呈现过程的一部分应用于 .xml 文件的 .xsl 转换。 对于 SCW .xml 文件，此参数是可选的。 当使用**view**命令呈现某个 SCW .xml 文件时，它会自动尝试为指定的 .xml 文件加载正确的默认转换。 如果指定了 .xsl 转换，则必须假定 .xml 文件位于与 .xsl 转换相同的目录中，以写入转换。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

Scwcmd 仅适用于运行 Windows Server 2008 R2、Windows Server 2008 或 Windows Server 2003 的计算机。

## <a name="BKMK_Examples"></a>示例

若要使用 Policyview 转换查看 Policyfile，请键入：
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)