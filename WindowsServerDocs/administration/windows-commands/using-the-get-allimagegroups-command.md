---
title: 使用 get AllImageGroups 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 917f61327a3d39ee97c5fd59072884f7844c487e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822348"
---
# <a name="using-the-get-allimagegroups-command"></a>使用 get AllImageGroups 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检索有关所有映像组服务器和这些映像组中的所有映像上的信息。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-AllImageGroups [/Server:<Server name>] [/detailed]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|[/ 详细]|从每个图像返回图像元数据。 如果不使用此参数，默认行为是返回映像名称、 说明和每个映像的文件名称。|
## <a name="BKMK_examples"></a>示例
若要查看有关映像组的信息，请键入以下项之一：
```
wdsutil /Get-AllImageGroups
wdsutil /verbose /Get-AllImageGroups /Server:MyWDSServer /detailed
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加 ImageGroup 命令](using-the-add-imagegroup-command.md)
[使用 get ImageGroup 命令](using-the-get-imagegroup-command.md)
[使用删除 ImageGroup 命令](using-the-remove-imagegroup-command.md)
[子命令： 集 ImageGroup](subcommand-set-imagegroup.md)
