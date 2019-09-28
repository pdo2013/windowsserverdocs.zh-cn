---
title: 使用 AllDevices 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5824b3d2-2df1-4ed6-a289-e6c47c13fea2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b7d2ce709c7e3fbaf7ab4f0e49be14c98ba1cd9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401977"
---
# <a name="using-the-get-alldevices-command"></a>使用 AllDevices 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示所有预留计算机的 Windows 部署服务属性。 预留计算机是已链接到 active directory 域服务中的计算机帐户的物理计算机。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/forest： {Yes &#124; No}]|指定 Windows 部署服务应返回整个林中的计算机还是本地域中的计算机。 默认设置为 "**否**"，这意味着只会返回本地域中的计算机。|
|[/ReferralServer： @no__t]|仅返回为指定服务器预留的那些计算机。|
## <a name="BKMK_examples"></a>示例
若要查看所有计算机，请键入下列内容之一：
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法键](command-line-syntax-key.md)
 子命令：使用 "设备"[命令](using-the-get-device-command.md)
 时[使用添加设备命令](using-the-add-device-command.md)[设置设备](subcommand-set-device.md)

