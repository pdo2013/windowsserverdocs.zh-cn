---
title: 使用 AllDriverPackages 子命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ba6641c1-d7e9-43a9-9819-702dad5484ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8290a95dd53718b200d10b6804d312abe95e257
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363887"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>使用 AllDriverPackages 子命令



将文件夹中存储的所有驱动程序包添加到服务器。

## <a name="syntax"></a>语法

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>Parameters

|          参数           |                                                              描述                                                              |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
|  /FolderPath： @no__t 0Folder 路径 >  |                      指定包含驱动程序包 .inf 文件的文件夹的完整路径。                      |
|   [/Server： @no__t 名称 >]   | 指定服务器的名称。 此名称可以是 NetBIOS 名称或 FQDN。 如果未指定服务器名称，则使用本地服务器。 |
|     [/Architecture： {x86      |                                                                 ia64                                                                  |
| [/DriverGroup： \<Group Name >] |                             指定应将包添加到其中的驱动程序组的名称。                             |

## <a name="BKMK_examples"></a>示例

若要添加驱动程序包，请键入下列内容之一：
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)