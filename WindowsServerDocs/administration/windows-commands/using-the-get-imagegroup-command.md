---
title: 使用 get ImageGroup 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9dcb76155dc1044730673ed46a53cad57441a246
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862598"
---
# <a name="using-the-get-imagegroup-command"></a>使用 get ImageGroup 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检索有关映像组和其中的映像的信息。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
mediaGroup:<Image group name>|指定的映像组的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|[/ 详细]|返回每个图像的图像元数据。 如果此参数不是使用，默认行为是返回映像名称、 说明和文件的名称。|
## <a name="BKMK_examples"></a>示例
若要查看有关映像组的信息，请键入：
```
wdsutil /Get-ImageGroumediaGroup:ImageGroup1
```
若要查看包括的元数据的信息，请键入：
```
wdsutil /verbose /Get-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 ImageGroup 命令](using-the-add-imagegroup-command.md)
[使用 get AllImageGroups 命令](using-the-get-allimagegroups-command.md)
 [使用删除 ImageGroup 命令](using-the-remove-imagegroup-command.md)
[子命令： 集 ImageGroup](subcommand-set-imagegroup.md)
