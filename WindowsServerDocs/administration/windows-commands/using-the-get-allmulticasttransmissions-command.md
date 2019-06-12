---
title: 使用 get AllMulticastTransmissions 命令
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8fb79-7a8a-4f0c-88f4-92bc1111c67f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b05f8802a288d80960cf79356675cb9adce9c260
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440537"
---
# <a name="using-the-get-allmulticasttransmissions-command"></a>使用 get AllMulticastTransmissions 命令

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

显示服务器上所有多播传输有关的信息。
## <a name="syntax"></a>语法
对于 Windows Server 2008:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
适用于 Windows Server 2008 R2:
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
## <a name="parameters"></a>Parameters

|        参数        |                                                                                                                                                                                                                                                                   说明                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/ 服务器：<Server name>] |                                                                                                                                                                                 指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。                                                                                                                                                                                  |
|         [/Show]         | **Windows Server 2008**<br /><br />/Show: clients-显示有关客户端计算机连接到多播传输的信息。<br /><br />**Windows Server 2008 R2**<br /><br />Show: {启动&#124;安装&#124;所有}-映像要返回的类型。                                **启动**返回仅启动图像传输。                                  **安装**返回只能在安装映像传输。 **所有**返回同时图像类型。 |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /details:clients     |                                                                                                                                                                                              仅支持 Windows Server 2008 R2。 如果存在，将显示与传输连接的客户端。                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              从列表中排除任何已停用的传输。                                                                                                                                                                                                                                               |

## <a name="BKMK_examples"></a>示例
若要查看有关所有传输的信息，请键入：
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2：`wdsutil /Get-AllMulticastTransmissions /Show:All` 若要查看除已停用传输所有传输有关的信息，请键入：
- Windows Server 2008: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2: `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  #### <a name="additional-references"></a>其他参考
  [命令行语法解答](command-line-syntax-key.md)
  [使用 /get-multicasttransmission 命令](using-the-get-multicasttransmission-command.md)
  [使用新 MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
  [使用 /remove-multicasttransmission 命令](using-the-remove-multicasttransmission-command.md)
  [子命令： /start-multicasttransmission](subcommand-start-multicasttransmission.md)
