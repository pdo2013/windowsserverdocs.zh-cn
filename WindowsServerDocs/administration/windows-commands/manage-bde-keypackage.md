---
title: manage-bde Ms-fve-keypackage
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a0a1b4fd0fff1153a0f778eca105ecfc618a4689
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374055"
---
# <a name="manage-bde-keypackage"></a>manage-bde.exeMs-fve-keypackage



为驱动器生成密钥包。 此密钥包可以与修复工具结合使用来修复损坏的驱动器。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<Drive >|表示驱动器号后跟一个冒号。|
|-ID|使用带有此 ID 值指定的标识符的密钥保护程序创建密钥包。|
|-路径|在其中保存创建的密钥包的位置。|
|-computername|指定 Manage-bde.exe 将用于修改另一台计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。|
|\<名称 >|表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。|
|-? 或 /?|在命令提示符下显示 brief Help。|
|-help 或-h|在命令提示符下显示完整的帮助。|

## <a name="BKMK_Examples"></a>示例

下面的示例演示如何使用 **-ms-fve-keypackage**命令，基于 GUID 标识的密钥保护程序为驱动器 C 创建密钥包，并将密钥包保存到 F:\Folder。
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path "f:\Folder"
```

> [!TIP]
> 使用 manage-bde **–保护程序–获取**要为其创建密钥包的驱动器号，以获取用作 ID 值的可用 guid 列表。

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)