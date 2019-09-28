---
title: ver
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e48b3b1061edf793c88693b3353753c6a4cedcfc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362720"
---
# <a name="ver"></a>ver



显示操作系统的版本号。

此命令在 Windows 命令提示符（Cmd.exe）中受支持，但在 PowerShell 中不受支持。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
ver
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/?|在命令提示符下显示帮助。|

## <a name="BKMK_examples"></a>示例

若要从命令行界面（cmd.exe）获取操作系统的版本号，请键入：

```
ver
```

Ver 命令在 PowerShell 中不起作用。 若要从 PowerShell 获取操作系统版本，请键入：

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)
