---
title: 使用 AllImageGroups 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca06533-bcf5-4590-ac8e-263d6c9874f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54e302dca5014d084c7277154eb491f9e33a536b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363311"
---
# <a name="using-the-get-allimagegroups-command"></a>使用 AllImageGroups 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

检索有关服务器上的所有映像组和这些映像组中的所有映像的信息。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
|[/detailed]|返回每个图像的图像元数据。 如果未使用此参数，则默认行为是只返回每个图像的映像名称、说明和文件名。|
## <a name="BKMK_examples"></a>示例
若要查看有关映像组的信息，请键入下列内容之一：
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>其他参考
使用[ImageGroup 命令](using-the-add-imagegroup-command.md)的[命令行语法键](command-line-syntax-key.md)
 
 使用[ImageGroup 命令](using-the-get-imagegroup-command.md)
 使用[ImageGroup 命令](using-the-remove-imagegroup-command.md)@no__t 7[子命令： set-ImageGroup](subcommand-set-imagegroup.md)
