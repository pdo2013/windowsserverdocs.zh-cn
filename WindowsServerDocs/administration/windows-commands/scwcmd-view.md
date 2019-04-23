---
title: Scwcmd 视图
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2ef1dd72903108edd6c5fb450c536a9325fcf546
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889548"
---
# <a name="scwcmd-view"></a>Scwcmd: view

> 适用于：Windows Server 2012 R2, Windows Server 2012

使用指定的.xsl 转换来呈现一个.xml 文件。 此命令也可用于使用不同的视图显示安全配置向导 (SCW).xml 文件。

## <a name="syntax"></a>语法

```
scwcmd view /x:<Xmlfile.xml> [/s:<Xslfile.xsl>]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/x:\<Xmlfile.xml>|指定要查看的.xml 文件。 必须指定此参数。|
|/s:\<Xslfile.xsl>|指定要应用到的.xml 文件作为呈现过程的一部分的.xsl 转换。 此参数是可选的 SCW.xml 文件。 当**视图**命令用于呈现一个 SCW 的.xml 文件，它将自动尝试加载指定的.xml 文件的正确的默认转换。 如果指定.xsl 转换，则必须依据： 假定为.xsl 转换相同的目录中的.xml 文件是编写转换。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

Scwcmd.exe 才可在运行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的计算机上。

## <a name="BKMK_Examples"></a>示例

若要查看使用 Policyview.xsl 转换 Policyfile.xml，请键入：
```
scwcmd view /x:C:\policies\Policyfile.xml /s:C:\viewers\Policyview.xsl
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)