---
title: 使用 get AllImages 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 57b81dd3dd3a24876c4401e80d08130ed5243888
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872558"
---
# <a name="using-the-get-allimages-command"></a>使用 get AllImages 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检索有关所有映像的服务器上的信息。
## <a name="syntax"></a>语法
```
wdsutil /Get-AllImages [/Server:<Server name>] /Show:{Boot | Install | LegacyRis | All} [/detailed]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/ 显示: {启动&#124;安装&#124;LegacyRis&#124;所有}|-   **启动**返回仅启动映像。<br />-   **安装**返回安装映像，以及有关包含它们的映像组的信息。<br />-   **LegacyRis**返回远程安装服务 (RIS) 映像。<br />-   **所有**返回启动映像的信息、 安装映像信息 （包括有关映像组的信息） 和 RIS 映像信息。|
|[/ 详细]|指示应返回从每个映像的所有图像元数据。 如果不使用此选项，默认行为是返回映像名称、 说明和文件的名称。|
## <a name="BKMK_examples"></a>示例
若要查看有关映像的信息，请键入以下项之一：
```
wdsutil /Get-AllImages /Show:Install
wdsutil /verbose /Get-AllImages /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用添加映像命令](using-the-add-image-command.md)
[使用复制图像命令](using-the-copy-image-command.md)
[使用导出映像命令](using-the-export-image-command.md)
[使用删除映像命令](using-the-remove-image-command.md)
[使用替换映像命令](using-the-replace-image-command.md)
 [子命令： 设置图像](subcommand-set-image.md)
