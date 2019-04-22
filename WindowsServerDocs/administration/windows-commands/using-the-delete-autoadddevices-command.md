---
title: 使用删除 AutoaddDevices 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dcaca6a-212e-4c36-98e3-00938eef6b9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8b3375418c5ce0b02e187e292cac5b168f0de5dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813498"
---
# <a name="using-the-delete-autoadddevices-command"></a>使用删除 AutoaddDevices 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

删除处于挂起状态，将被拒绝，或从自动添加数据库已批准的计算机。 此数据库存储在服务器上的这些计算机的信息。
## <a name="syntax"></a>语法
```
wdsutil /delete-AutoaddDevices [/Server:<Server name>] /Devicetype:{PendingDevices | RejectedDevices |ApprovedDevices}
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|/Devicetype:{PendingDevices &#124; RejectedDevices &#124;ApprovedDevices}|指定要从数据库中删除计算机的类型。 这可以是任何以下三种类型：<br /><br />-   **PendingDevices**返回的状态为挂起的数据库中的所有计算机。<br />-   **RejectedDevices**返回数据库中，有状态的拒绝所有计算机。<br />-   **ApprovedDevices**返回的状态的所有计算机批准。|
## <a name="BKMK_examples"></a>示例
若要删除所有拒绝的计算机，请键入：
```
wdsutil /delete-AutoaddDevices /Devicetype:RejectedDevices
```
若要删除所有已批准的计算机，请键入：
```
wdsutil /verbose /delete-AutoaddDevices /Server:MyWDSServer /Devicetype:ApprovedDevices
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用批准 AutoaddDevices 命令](using-the-approve-autoadddevices-command.md)
[使用 get AutoaddDevices 命令](using-the-get-autoadddevices-command.md)
 [使用拒绝 AutoaddDevices 命令](using-the-reject-autoadddevices-command.md)
