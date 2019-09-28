---
title: 子命令集-DriverPackage
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11804bb6-ca29-4461-8c63-5131748cd742
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 65751cf6e03baa87c7734b318a26111652bee0a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370819"
---
# <a name="subcommand-set-driverpackage"></a>子命令： set-DriverPackage



重命名和/或启用或禁用服务器上的驱动程序包。

## <a name="syntax"></a>语法

```
WDSUTIL /Set-DriverPackage [/Server:<Server name>] {/DriverPackage:<Name> | /PackageId:<ID>} [/Name:<New Name>] [/Enabled:{Yes | No}
```

## <a name="parameters"></a>Parameters

|        参数         |                                                                                                                                                                                                               描述                                                                                                                                                                                                                |
|--------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： @no__t 名称 >] |                                                                                                                                                 指定服务器的名称。 此名称可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。                                                                                                                                                 |
| [/DriverPackage： \<Name >] |                                                                                                                                                                                       指定要修改的驱动程序包的当前名称。                                                                                                                                                                                        |
|    [/PackageId： \<ID >]    | 指定驱动程序包的 Windows 部署服务 ID。 如果无法按名称唯一地标识驱动程序包，则必须指定此选项。 若要为包查找此 ID，请单击包所在的驱动程序组（或 "**所有包**" 节点），右键单击该包，然后单击 "**属性**"。 包 ID 在 "**常规**" 选项卡上列出。例如： {DD098D20-1850-4FC8-8E35-EA24A1BEFF5E}。 |
|   [/Name： \<New Name >]    |                                                                                                                                                                                              指定驱动程序包的新名称。                                                                                                                                                                                              |
|      [/Enabled： {Yes      |                                                                                                                                                                                                                   不                                                                                                                                                                                                                    |

## <a name="BKMK_examples"></a>示例

若要更改包的设置，请键入下列内容之一：
```
WDSUTIL /Set-DriverPackage /PackageId:{4D36E972-E325-11CE-BFC1-08002BE10318} /Name:MyDriverPackage
```
```
WDSUTIL /Set-DriverPackage /DriverPackage:MyDriverPackage /Name:NewName /Enabled:Yes
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)