---
title: 使用 get AllDevices 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e5f51bcc2332cced906be1eec3265541ffd2d225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886368"
---
# <a name="using-the-get-alldevices-command"></a>使用 get AllDevices 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示所有预留计算机的 Windows 部署服务的属性。 为预留的计算机是物理计算机的已链接到 active directory 域服务中的计算机帐户。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Get-AllDevices [/forest:{Yes | No}] [/ReferralServer:<Server name>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/forest:{Yes &#124; No}]|指定 Windows 部署服务是否应返回在整个林或本地域中的计算机。 默认设置是**否**，将返回仅在本地域中的计算机的含义。|
|[/ReferralServer:<Server name>]|返回为指定的服务器预留这些计算机。|
## <a name="BKMK_examples"></a>示例
若要查看所有计算机，请键入以下项之一：
```
wdsutil /Get-AllDevices
wdsutil /verbose /Get-AllDevices /forest:Yes /ReferralServer:MyWDSServer
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[子命令： 设置设备](subcommand-set-device.md)
[使用添加设备命令](using-the-add-device-command.md)
[使用 get 设备命令](using-the-get-device-command.md)
