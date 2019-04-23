---
title: 使用添加 AllDriverPackages 子命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6c7a9e95ebd36209d5729f81b7eae9e2660b3606
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890688"
---
# <a name="using-the-add-alldriverpackages-subcommand"></a>使用添加 AllDriverPackages 子命令



将添加到服务器的文件夹中存储的所有驱动程序包。

## <a name="syntax"></a>语法

```
WDSUTIL /Add-AllDriverPackages /FolderPath:<Folder Path> [/Server:<Server name>] [/Architecture:{x86 | ia64 | x64}] [/DriverGroup:<Group Name>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ FolderPath:\<文件夹路径 >|指定包含驱动程序包的.inf 文件的文件夹的完整路径。|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或 FQDN。 如果指定没有服务器名称，则使用本地服务器。|
|[/ 体系结构: {x86 | ia64 | x64}]|指定要添加的驱动程序包的体系结构。 其他体系结构的驱动程序包将被忽略。|
|[/ DriverGroup:\<组名称 >]|指定可向其添加包的驱动程序组的名称。|

## <a name="BKMK_examples"></a>示例

若要添加驱动程序包，请键入以下项之一：
```
WDSUTIL /verbose /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers" /Architecture:x86
```
```
WDSUTIL /Add-AllDriverPackages /FolderPath:"C:\Temp\Drivers\Printers" /DriverGroup:"Printer Drivers"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[Add-WdsDriverPackage](https://technet.microsoft.com/library/dn283440.aspx)