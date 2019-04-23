---
title: 使用复制 DriverGroup 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0aaf6fa5-8b5b-4a1e-ae9b-8b5c6d89f571
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68d9c6f4ca78991bb4c286042a6172211161dd1e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842078"
---
# <a name="using-the-copy-drivergroup-command"></a>使用复制 DriverGroup 命令



复制包括筛选器、 驱动程序包和已启用/禁用状态的服务器上的现有驱动程序组。

## <a name="syntax"></a>语法

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。|
|/ DriverGroup:\<源组名称 >|指定源驱动程序组的名称。|
|/ GroupName:\<新的组名称 >|指定新的驱动程序组的名称。|

## <a name="BKMK_examples"></a>示例

若要复制的驱动程序组，请键入以下项之一：
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)