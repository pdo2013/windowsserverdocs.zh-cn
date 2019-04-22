---
title: 子命令集 ImageGroup
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d0c7ba47148ba6f8295ab720dd0118759ac9346c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822978"
---
# <a name="subcommand-set-imagegroup"></a>子命令： 集 ImageGroup

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

更改映像组的属性。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
mediaGroup:<Image group name>|指定的映像组的名称。|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果未指定，将使用本地服务器。|
|[/Name:<New image group name>]|指定映像组的新名称。|
|[/ 安全：<SDDL>]|安全描述符定义语言 (SDDL) 格式指定的映像组中，新的安全描述符。|
## <a name="BKMK_examples"></a>示例
若要设置的映像组名称，请键入：
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:"New Image Group Name"
```
若要指定映像组的各种设置，请键入：
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:"New Image Group Name" 
/Security:"O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)"
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 ImageGroup 命令](using-the-add-imagegroup-command.md)
[使用 get AllImageGroups 命令](using-the-get-allimagegroups-command.md)
 [使用 get ImageGroup 命令](using-the-get-imagegroup-command.md)
[使用删除 ImageGroup 命令](using-the-remove-imagegroup-command.md)
