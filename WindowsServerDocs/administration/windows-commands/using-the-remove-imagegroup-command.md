---
title: 使用删除 ImageGroup 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: df6af7fbd19cc95dfbffa0a5c0e2a4b3e33fb82c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877888"
---
# <a name="using-the-remove-imagegroup-command"></a>使用删除 ImageGroup 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

从服务器中删除映像组。
## <a name="syntax"></a>语法
```
wdsutil [Options] /remove-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
mediaGroup:<Image group name>|指定要删除的映像组的名称|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要删除的映像组，请键入以下项之一：
```
wdsutil /remove-ImageGroumediaGroup:ImageGroup1
wdsutil /verbose /remove-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer 
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 ImageGroup 命令](using-the-add-imagegroup-command.md)
[使用 get AllImageGroups 命令](using-the-get-allimagegroups-command.md)
 [使用 get ImageGroup 命令](using-the-get-imagegroup-command.md)
[子命令： 集 ImageGroup](subcommand-set-imagegroup.md)
