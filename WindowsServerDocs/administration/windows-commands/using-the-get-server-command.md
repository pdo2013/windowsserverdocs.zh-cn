---
title: 使用 get-Server 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bef60db4-d58d-4304-ab4b-be53dd3271c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c7e0ee4529858b16cdc63ea1d6d358a190b8b1a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392117"
---
# <a name="using-the-get-server-command"></a>使用 get-Server 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

检索指定 Windows 部署服务服务器中的信息。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，则使用本地服务器。|
|/Show： {Config &#124; Images &#124; All}|指定要返回的信息的类型。<br /><br />-   **Config**返回配置信息。<br />@no__t 0 的**图像**返回有关映像组、启动映像和安装映像的信息。<br />-   **全部**返回配置信息和图像信息。|
|[/detailed]|可以将此选项与 **/show： Images**或 **/show： all**一起使用，以指示应返回每个图像中的所有图像元数据。 如果未使用 **/detailed**选项，则默认行为是返回映像名称、说明和文件名。|
## <a name="BKMK_examples"></a>示例
若要查看有关服务器的信息，请键入：
```
wdsutil /Get-Server /Show:Config
```
若要查看有关服务器的详细信息，请键入：
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
 使用[disable](using-the-disable-server-command.md)-server 命令 
 使用[enable](using-the-enable-server-command.md)-Server 命令 
 使用[Initialize-](using-the-initialize-server-command.md)server 命令 
[子命令： set-server](subcommand-set-server.md)
[子命令： start-Server](subcommand-start-server.md)1[子命令： Stop-server](subcommand-stop-server.md)3["取消初始化-服务器" 选项](the-uninitialize-server-option.md)
