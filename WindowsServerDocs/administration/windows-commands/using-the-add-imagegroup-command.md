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
ms.assetid: 6ca88671-51de-4924-b969-88f3dfd84270
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e870bd5435e1aa2b155fee880d32c0d784ac398
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363677"
---
# <a name="using-the-add-imagegroup-command"></a>使用 ImageGroup 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

将映像组添加到 Windows 部署服务服务器。
## <a name="syntax"></a>语法
```
wdsutil [Options] /add-ImageGroumediaGroup:<Image group name> [/Server:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
mediaGroup： <Image group name>|指定要添加的映像组的名称。|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
## <a name="BKMK_examples"></a>示例
若要添加映像组，请键入下列内容之一：
```
wdsutil /add-ImageGroumediaGroup:ImageGroup2
wdsutil /verbose /add-ImageGroumediaGroup:"My Image Group" /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
 使用[AllImageGroups 命令](using-the-get-allimagegroups-command.md)
 使用[ImageGroup 命令](using-the-get-imagegroup-command.md)
 使用[ImageGroup 命令](using-the-remove-imagegroup-command.md)
[子命令： set-ImageGroup](subcommand-set-imagegroup.md)
