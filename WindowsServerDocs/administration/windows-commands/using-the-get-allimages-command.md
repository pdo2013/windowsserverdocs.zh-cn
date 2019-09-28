---
title: 使用 AllImages 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19de3720-4315-415a-8dc6-486caa0b2100
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5122a5660031d503795715c0005b404f910d6626
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363497"
---
# <a name="using-the-get-allimages-command"></a>使用 AllImages 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

检索有关服务器上所有映像的信息。
## <a name="syntax"></a>语法
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。|
|/Show： {Boot &#124; Install &#124; LegacyRis &#124; All}|-   **boot**仅返回启动映像。<br />@no__t**安装**会返回安装映像以及包含它们的映像组的相关信息。<br />-   **LegacyRis**仅返回远程安装服务（RIS）映像。<br />-   **全部**返回启动映像信息、安装映像信息（包括映像组的相关信息）和 RIS 映像信息。|
|[/detailed]|指示应返回每个图像中的所有图像元数据。 如果未使用此选项，则默认行为是只返回映像名称、说明和文件名。|
## <a name="BKMK_examples"></a>示例
若要查看有关图像的信息，请键入下列内容之一：
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>其他参考
[命令行语法关键字](command-line-syntax-key.md)
 使用 "[添加图像](using-the-add-image-command.md)" 命令 
 使用 "[复制映像](using-the-copy-image-command.md)" 命令 
 使用 "[导出](using-the-export-image-command.md)-映像" 命令 @no__t-@no__t[7 使用](using-the-remove-image-command.md)[替换图像命令](using-the-replace-image-command.md)1[子命令：设置-图像](subcommand-set-image.md)
