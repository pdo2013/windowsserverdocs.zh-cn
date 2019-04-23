---
title: ver
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 384a5e8adb6c8304033f7dc645184ff2b674ae39
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887168"
---
# <a name="ver"></a>ver



显示操作系统的版本号。

在 Windows 命令提示符 (Cmd.exe)，但在 PowerShell 中不支持此命令。

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

若要从命令行界面 (cmd.exe) 获取操作系统的版本号，请键入：

```
ver
```

Ver 命令不会在 PowerShell 中无效。 若要从 PowerShell 获取 OS 版本，请键入：

```powershell
$PSVersionTable.BuildVersion
````


#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)
