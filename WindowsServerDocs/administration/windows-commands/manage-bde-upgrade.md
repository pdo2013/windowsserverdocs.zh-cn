---
title: 管理 bde 升级
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23bfa824-6ff0-44cc-9b8b-b199a769fb8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc28dfc98a66a60172c812d12a16d03c078b79e1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822278"
---
# <a name="manage-bde-upgrade"></a>管理 bde： 升级



BitLocker 版本升级。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde -upgrade [<Drive>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive>|表示驱动器号后, 接一个冒号。|
|-computername|指定将使用 bde.exe 来修改在其他计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。|
|\<名称 >|表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|显示在命令提示符下简短帮助。|
|-help 或-h|显示在命令提示符下完成的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-升级**命令升级 BitLocker 加密驱动器 C 上。
```
manage-bde –upgrade C:
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)
-   [受 BitLocker 保护的计算机从 Windows Vista 升级到 Windows 7](https://technet.microsoft.com/library/ee424325(v=ws.10).aspx)