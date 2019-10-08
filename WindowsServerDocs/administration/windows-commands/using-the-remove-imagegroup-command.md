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
ms.assetid: 5b2c9813-5df2-4272-8449-26f3bb16f82b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30e4c2c7c5cf2668d62e96d8d2a54dc33e3d2a55
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71960960"
---
# <a name="using-the-remove-imagegroup-command"></a>使用 ImageGroup 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

从服务器中删除映像组。
## <a name="syntax"></a>语法
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
mediaGroup： <Image group name>|指定要删除的映像组的名称|
|[/Server： <Server name>]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要删除映像组，请键入下列内容之一：
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer 
```
#### <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)  
[使用 ImageGroup 命令](using-the-add-imagegroup-command.md)  
[使用 AllImageGroups 命令](using-the-get-allimagegroups-command.md)  
[使用 ImageGroup 命令](using-the-get-imagegroup-command.md)  
[子命令： set-ImageGroup](subcommand-set-imagegroup.md)  
