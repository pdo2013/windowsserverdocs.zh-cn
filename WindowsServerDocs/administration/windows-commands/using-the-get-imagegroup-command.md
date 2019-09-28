---
title: 使用 ImageGroup 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0fc25aca-a529-44ee-bc8e-96bc8affb458
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d6531a3b69840a0a4910b2effdd3e349b76edf2c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363117"
---
# <a name="using-the-get-imagegroup-command"></a>使用 ImageGroup 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

检索有关映像组及其内图像的信息。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
mediaGroup： <Image group name>|指定映像组的名称。|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
|[/detailed]|返回每个图像的图像元数据。 如果未使用此参数，则默认行为是只返回映像名称、说明和文件名。|
## <a name="BKMK_examples"></a>示例
若要查看有关映像组的信息，请键入：
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
若要查看包括元数据的信息，请键入：
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>其他参考
使用[ImageGroup 命令](using-the-add-imagegroup-command.md)的[命令行语法键](command-line-syntax-key.md)
 
 使用[AllImageGroups 命令](using-the-get-allimagegroups-command.md)
 使用[ImageGroup 命令](using-the-remove-imagegroup-command.md)@no__t 7[子命令： set-ImageGroup](subcommand-set-imagegroup.md)
