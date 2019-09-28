---
title: 使用 DriverPackage 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f5370d301f5fec15f4812b3d65588297d179455d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363749"
---
# <a name="using-the-add-driverpackage-command"></a>使用 DriverPackage 命令



向服务器中添加驱动程序包。

## <a name="syntax"></a>语法

```
WDSUTIL /Add-DriverPackage /InfFile:<Inf File path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>] [/Name:<Friendly Name>]
```

## <a name="parameters"></a>Parameters

|          参数           |                                                              描述                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|   InfFile： \<Inf 文件路径 >   |                                           指定要添加的 .inf 文件的完整路径。                                            |
|    /Server： @no__t 名称 >    | 指定服务器的名称。 此名称可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。 |
|      /Architecture： {x86      |                                                                 ia64                                                                  |
| [/DriverGroup： \<Group Name >] |                             指定应将包添加到的驱动程序组的名称。                              |
|   [/Name： \<Friendly Name >]   |                                           指出驱动程序包的友好名称。                                            |

## <a name="BKMK_examples"></a>示例

若要添加驱动程序包，请键入下列内容之一：
```
WDSUTIL /verbose /Add-DriverPackage /InfFile:"C:\Temp\Display.inf"
```
```
WDSUTIL /Add-DriverPackage /Server:MyWDSServer /InfFile:"C:\Temp\Display.inf" /Architecture:x86 /DriverGroup:x86Drivers /Name:"Display Driver"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

