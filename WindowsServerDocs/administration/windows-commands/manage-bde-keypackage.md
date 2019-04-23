---
title: 管理 bde KeyPackage
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fb49ce8fbe4be076151b203560e62f44a78c9d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886898"
---
# <a name="manage-bde-keypackage"></a>管理-bde:KeyPackage



生成一个驱动器的密钥包。 密钥包可以与修复工具结合使用，以修复损坏的驱动器。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive>|表示驱动器号后, 接一个冒号。|
|-ID|创建密钥包密钥保护程序使用此 ID 值指定的标识符。|
|-path|要在其中保存创建的密钥包的位置。|
|-computername|指定将使用 bde.exe 来修改在其他计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。|
|\<名称 >|表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|显示在命令提示符下简短帮助。|
|-help 或-h|显示在命令提示符下完成的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-KeyPackage**命令来创建密钥的驱动器 C 由 GUID 标识的密钥保护程序为基础包，并将密钥包保存到 F:\Folder。
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path "f:\Folder"
```

> [!TIP]
> 使用**管理 bde – 保护程序 – 获取**以及你想要创建的密钥包以获取可用的 Guid 作为 ID 值的列表的驱动器号。

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)
-   [管理 bde](manage-bde.md)