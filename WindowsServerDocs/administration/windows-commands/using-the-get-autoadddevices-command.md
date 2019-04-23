---
title: 使用 get AutoaddDevices 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 24b4b688-55b0-4bd9-a2f5-7ef4b3dfe2f2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 337c8e76923fe243982ba9c10d18f2e5a5e7d9ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885738"
---
# <a name="using-the-get-autoadddevices-command"></a>使用 get AutoaddDevices 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示 Windows 部署服务服务器上自动添加数据库中的所有计算机。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices | ApprovedDevices}
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/Devicetype:{PendingDevices &#124; RejectedDevices &#124; ApprovedDevices}|指定要返回计算机的类型。<br /><br />-   **PendingDevices**返回的状态为挂起的数据库中的所有计算机。<br />-   **RejectedDevices**返回数据库中，有状态的拒绝所有计算机。<br />-   **ApprovedDevices**返回数据库中的有状态的批准所有计算机。|
## <a name="BKMK_examples"></a>示例
若要查看所有已批准的计算机，请键入：
```
wdsutil /Get-AutoaddDevices /Devicetype:ApprovedDevices
```
若要查看所有被拒绝的计算机，请键入：
```
wdsutil /verbose /Get-AutoaddDevices /Devicetype:RejectedDevices /Server:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用 delete AutoaddDevices 命令](using-the-delete-autoadddevices-command.md)
[使用批准 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md)
 [使用拒绝 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
