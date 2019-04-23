---
title: 使用 get 设备命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1da79286-7e1d-45f2-aea2-d446e16a6911
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 117720c5102da5713c1b0bb5e59cbcc099f3aa8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871028"
---
# <a name="using-the-get-device-command"></a>使用 get 设备命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检索预留计算机 （即，已内联到 active directory 域服务中计算机帐户物理计算机有关 Windows 部署服务信息。
## <a name="syntax"></a>语法
```
wdsutil /Get-Device {/Device:<Device name> | /ID:<MAC or UUID>} [/Domain:<Domain>] [/forest:{Yes | No}]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/ 设备：<Device name>|指定的计算机 (SAMAccountName) 的名称。|
|/ID:<MAC or UUID>|指定的 MAC 地址或计算机的 UUID (GUID)，如以下示例所示。 请注意，一个有效的 GUID 必须在两种格式的二进制字符串或 GUID 字符串之一<br /><br />-   **二进制字符串**: /ID:ACEFA3E81F20694E953EB2DAA1E8B1B6<br />-   **MAC 地址**:00B056882FDC （无短划线） 或 00-B0-56-88-2F-DC （带短划线）<br />-   **GUID 字符串**: /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6|
|[/ 域：<Domain>]|指定要在其中搜索的预留计算机的域。 此参数的默认值为本地域。|
|[/forest:{Yes &#124; No}]|指定 Windows 部署服务是否应搜索整个林或本地域。 默认值是**否**，这意味着将搜索仅在本地域。|
## <a name="BKMK_examples"></a>示例
若要通过使用计算机名称获取信息，请键入：
```
wdsutil /Get-Device /Device:computer1
```
若要使用的 MAC 地址获取信息，请键入：
```
wdsutil /verbose /Get-Device /ID:"00-B0-56-88-2F-DC" /Domain:MyDomain
```
若要使用此 GUID 字符串获取信息，请键入：
```
wdsutil /verbose /Get-Device /ID:E8A3EFAC-201F-4E69-953-B2DAA1E8B1B6 /forest:Yes
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[子命令： 设置设备](subcommand-set-device.md)
[使用添加设备命令](using-the-add-device-command.md)
[使用get AllDevices 命令](using-the-get-alldevices-command.md)
