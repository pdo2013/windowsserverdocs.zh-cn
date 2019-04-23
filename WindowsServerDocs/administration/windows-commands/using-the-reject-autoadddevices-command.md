---
title: 使用拒绝 AutoaddDevices 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ea25a4b2-5fad-4360-9c47-c2c9df7ea31f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: af46aec7c8f02b3600983b66bd1b0ac6f5dd1dcc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852558"
---
# <a name="using-the-reject-autoadddevices-command"></a>使用拒绝 AutoaddDevices 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

拒绝等待管理批准的计算机。 启用自动添加策略后，未知的计算机 （即未预留） 可以安装映像之前，将需要管理批准。 您可以使用此策略启用**PXE 响应**服务器的属性页的选项卡。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Reject-AutoaddDevices [/Server:<Server name>] /RequestId:<Request ID or ALL>
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/RequestId:<Request ID &#124; ALL>|指定分配给挂起计算机的请求 ID。 若要拒绝所有挂起的计算机，指定**所有**。|
## <a name="BKMK_examples"></a>示例
若要拒绝一台计算机，键入：
```
wdsutil /Reject-AutoaddDevices /RequestId:12
```
若要拒绝所有计算机，请键入：
```
wdsutil /verbose /Reject-AutoaddDevices /Server:MyWDSServer /RequestId:ALL
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用批准 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md)
[使用 delete AutoaddDevices 命令](using-the-delete-autoadddevices-command.md)
 [使用 get AutoaddDevices 命令](using-the-get-autoadddevices-command.md)
