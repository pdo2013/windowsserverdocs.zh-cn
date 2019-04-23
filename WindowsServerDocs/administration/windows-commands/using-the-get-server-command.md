---
title: 使用获取服务器命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: da8bf0fc6e31bd8d0079933f1d7c529c4fe96f42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870978"
---
# <a name="using-the-get-server-command"></a>使用获取服务器命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

从指定的 Windows 部署服务服务器中检索信息。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-Server [/Server:<Server name>] /Show:{Config | Images | All} [/detailed]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则使用本地服务器。|
|/ 显示: {配置&#124;映像&#124;所有}|指定要返回的信息的类型。<br /><br />-   **配置**返回配置信息。<br />-   **映像**返回有关映像组、 启动映像和安装映像的信息。<br />-   **所有**返回配置信息和图像信息。|
|[/ 详细]|可以使用此选项与 **/Show:Images**或 **/Show:All**以指示应返回从每个映像的所有图像元数据。 如果 **/ 详细**未使用选项时，默认行为是返回的映像名称、 说明和文件名称。|
## <a name="BKMK_examples"></a>示例
若要查看有关服务器的信息，请键入：
```
wdsutil /Get-Server /Show:Config
```
若要查看有关服务器的详细的信息，请键入：
```
wdsutil /verbose /Get-Server /Server:MyWDSServer /Show:All /detailed
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用禁用服务器命令](using-the-disable-server-command.md)
[使用启用服务器命令](using-the-enable-server-command.md)
[使用初始化服务器命令](using-the-initialize-server-command.md)
[子命令： 设置服务器](subcommand-set-server.md)
[子命令： 启动 Server](subcommand-start-server.md) 
 [子命令： 停止服务器](subcommand-stop-server.md)
[uninitialize 服务器选项](the-uninitialize-server-option.md)
