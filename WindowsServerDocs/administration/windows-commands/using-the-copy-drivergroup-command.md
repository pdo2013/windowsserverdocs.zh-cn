---
title: 使用 DriverGroup 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c08ce616c9b0e2bf79c7f13f922e27d7f7f7ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363590"
---
# <a name="using-the-copy-drivergroup-command"></a>使用 DriverGroup 命令



复制服务器上的现有驱动程序组，包括筛选器、驱动程序包和启用/禁用状态。

## <a name="syntax"></a>语法

```
WDSUTIL /Copy-DriverGroup [/Server:<Server name>] /DriverGroup:<Source Group Name> /GroupName:<New Group Name>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|[/Server： @no__t 名称 >]|指定服务器的名称。 此名称可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。|
|/DriverGroup： \<Source 组名称 >|指定源驱动程序组的名称。|
|/GroupName： \<New 组名称 >|指定新驱动程序组的名称。|

## <a name="BKMK_examples"></a>示例

若要复制驱动程序组，请键入下列内容之一：
```
WDSUTIL /Copy-DriverGroup /Server:MyWdsServer /DriverGroup:PrinterDrivers /GroupName:X86PrinterDrivers
```
```
WDSUTIL /Copy-DriverGroup /DriverGroup:PrinterDrivers /GroupName:ColorPrinterDrivers
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)