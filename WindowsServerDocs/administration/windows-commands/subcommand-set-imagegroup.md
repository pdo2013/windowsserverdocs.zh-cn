---
title: 子命令集-ImageGroup
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10023e493ae4db51783b7401c12bc1605145b86c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370790"
---
# <a name="subcommand-set-imagegroup"></a>子命令： set-ImageGroup

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

更改映像组的属性。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
mediaGroup： <Image group name>|指定映像组的名称。|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定，将使用本地服务器。|
|[/Name： @no__t]|指定映像组的新名称。|
|[/Security： @no__t]|以安全描述符定义语言（SDDL）格式指定映像组的新安全描述符。|
## <a name="BKMK_examples"></a>示例
若要设置映像组的名称，请键入：
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:"New Image Group Name"
```
若要为映像组指定各种设置，请键入：
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:"New Image Group Name" 
/Security:"O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)"
```
#### <a name="additional-references"></a>其他参考
使用[ImageGroup 命令](using-the-add-imagegroup-command.md)的[命令行语法键](command-line-syntax-key.md)
 
 使用[AllImageGroups 命令](using-the-get-allimagegroups-command.md)
 使用[ImageGroup 命令](using-the-get-imagegroup-command.md)
[使用 ImageGroup 命令](using-the-remove-imagegroup-command.md)
