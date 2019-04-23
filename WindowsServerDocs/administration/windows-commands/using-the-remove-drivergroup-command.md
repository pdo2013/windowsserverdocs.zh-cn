---
title: 使用删除 DriverGroup 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1fefe9df-9782-433c-8abe-3f1a35e50da2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b56f162861caf4493550f9e063065e9544e52eae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885748"
---
# <a name="using-the-remove-drivergroup-command"></a>使用删除 DriverGroup 命令



从服务器中删除驱动程序组。

## <a name="syntax"></a>语法

```
WDSUTIL /Remove-DriverGroup /DriverGroup:<Group Name> [/Server:<Server name>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ DriverGroup:\<组名称 >|指定要删除的驱动程序组的名称。|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。|

## <a name="BKMK_examples"></a>示例

若要删除的驱动程序组，请键入以下项之一：
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers
```
```
WDSUTIL /Remove-DriverGroup /DriverGroup:PrinterDrivers /Server:MyWdsServer
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)