---
title: 使用断开客户端命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 876bbe6c-76ab-4de5-879b-d2066e700326
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbb96d64b47ec72ff0710bfb3684257c1bda2d04
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363468"
---
# <a name="using-the-disconnect-client-command"></a>使用断开客户端命令



断开客户端与多播传输或命名空间的连接。 除非指定 **/force**，否则客户端将回退到其他传输方法（如果客户端支持）。

## <a name="syntax"></a>语法

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ClientId： \<Client ID >|指定要断开连接的客户端的 ID。 若要查看客户端的 ID，请键入**WDSUTIL/get-multicasttransmission/show：** client。|
|[/Server： @no__t 名称 >]|指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，则使用本地服务器。|
|/Force|完全停止安装，不使用回退方法。 请注意，Wdsmcast.exe 不支持任何回退机制。 如果未使用此选项，则默认行为如下所示：</br>-如果你使用的是 Windows 部署服务客户端，客户端将使用单播继续安装。</br>-如果使用的不是 Windows 部署服务客户端，则安装将失败。</br>重要提示：你应慎用此选项，因为安装将失败，并且计算机可能处于不可用状态。|

## <a name="BKMK_examples"></a>示例

若要断开客户端连接，请键入：
```
WDSUTIL /Disconnect-Client /ClientId:1
```
若要断开客户端的连接并强制安装失败，请键入：
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)