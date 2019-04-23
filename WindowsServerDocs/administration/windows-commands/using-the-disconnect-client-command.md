---
title: 使用客户端断开连接命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b89f6c2ff6d41230afd0a2b251ad6982dfa235b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841748"
---
# <a name="using-the-disconnect-client-command"></a>使用客户端断开连接命令



从多播的传输或命名空间断开某个客户端。 除非指定，否则 **/force**，客户端将回退到另一种传输方法 （如果它受支持客户端）。

## <a name="syntax"></a>语法

```
WDSUTIL /Disconnect-Client /ClientId:<Client ID> [/Server:<Server name>] [/Force]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/ ClientId:\<客户端 ID >|指定要断开连接的客户端 ID。 若要查看客户端 ID，请键入**WDSUTIL /get-multicasttransmission /show: clients**。|
|[/ 服务器：\<服务器名称 >]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则使用本地服务器。|
|[/Force]|完全停止安装，且不使用回退方法。 请注意，Wdsmcast.exe 不支持任何回退机制。 如果不使用此选项，默认行为是按如下所示：</br>-如果使用的 Windows 部署服务客户端，客户端将使用单播继续安装。</br>-如果不使用 Windows 部署服务客户端，安装将失败。</br>重要提示：应谨慎使用此选项，因为安装将失败，并且计算机可能处于不可用状态。|

## <a name="BKMK_examples"></a>示例

若要断开连接客户端，请键入：
```
WDSUTIL /Disconnect-Client /ClientId:1
```
若要断开客户端，并强制安装失败，请键入：
```
WDSUTIL /Disconnect-Client /Server:MyWDSServer /ClientId:1 /Force
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)