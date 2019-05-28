---
title: 使用添加 DriverPackage 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ac9e8d5-63ec-4ce8-86fc-85d28011050b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7e96391ee8dce0b77f00f51d7cb78ff9b8bf242
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564657"
---
# <a name="using-the-add-driverpackage-command"></a>使用添加 DriverPackage 命令



将驱动程序包添加到服务器。

## <a name="syntax"></a>语法

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|InfFile:\<Inf 文件路径 >|指定要添加的.inf 文件的完整路径。|
|/ 服务器：\<服务器名称 >|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。|
|/ 体系结构: {x86 | ia64 | x64}|指定驱动程序包的体系结构。|
|[/ DriverGroup:\<组名称 >]|指定可向其添加包的驱动程序组的名称。|
|[/Name:\<友好名称 >]|状态为驱动程序包的友好名称。|

## <a name="BKMK_examples"></a>示例

若要添加驱动程序包，请键入以下项之一：
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

