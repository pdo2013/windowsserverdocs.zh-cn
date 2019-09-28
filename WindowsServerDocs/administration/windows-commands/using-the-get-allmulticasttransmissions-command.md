---
title: 使用 AllMulticastTransmissions 命令
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 644684ffb356ef07120bc391e3d3da2daf768eaf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363340"
---
# <a name="using-the-get-allmulticasttransmissions-command"></a>使用 AllMulticastTransmissions 命令

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

显示有关服务器上所有多播传输的信息。
## <a name="syntax"></a>语法
对于 Windows Server 2008：
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:Clients] [/ExcludedeletePending]
```
对于 Windows Server 2008 R2：
```
wdsutil /Get-AllMulticastTransmissions [/Server:<Server name>] [/Show:{Boot | Install | All}] [/details:Clients]  [/ExcludedeletePending]
```
## <a name="parameters"></a>Parameters

|        参数        |                                                                                                                                                                                                                                                                   说明                                                                                                                                                                                                                                                                    |
|-------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [/Server： @no__t] |                                                                                                                                                                                 指定服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定服务器名称，将使用本地服务器。                                                                                                                                                                                  |
|         /Show         | **Windows Server 2008**<br /><br />/Show：客户端-显示有关连接到多播传输的客户端计算机的信息。<br /><br />**Windows Server 2008 R2**<br /><br />Show： {Boot &#124; Install &#124; All}-要返回的图像的类型。                                **Boot**仅返回启动映像传输。                                  **安装**只返回安装图像传输。 **All**返回这两个图像类型。 |
|                         |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|    /details：客户端     |                                                                                                                                                                                              仅支持 Windows Server 2008 R2。 如果存在，则将显示连接到传输的客户端。                                                                                                                                                                                               |
| [/ExcludedeletePending] |                                                                                                                                                                                                                                              从列表中排除任何已停用的传输。                                                                                                                                                                                                                                               |

## <a name="BKMK_examples"></a>示例
若要查看有关所有传输的信息，请键入：
- Windows Server 2008： `wdsutil /Get-AllMulticastTransmissions`
- Windows Server 2008 R2：`wdsutil /Get-AllMulticastTransmissions /Show:All` 若要查看有关除已停用传输之外的所有传输的信息，请键入：
- Windows Server 2008： `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:Clients /ExcludedeletePending`
- Windows Server 2008 R2： `wdsutil /Get-AllMulticastTransmissions /Server:MyWDSServer /Show:All /details:Clients /ExcludedeletePending`
  #### <a name="additional-references"></a>其他参考
  使用[MulticastTransmission 命令](using-the-get-multicasttransmission-command.md)的[命令行语法键](command-line-syntax-key.md)
   
   使用[MulticastTransmission 命令](using-the-new-multicasttransmission-command.md)
  [使用 MulticastTransmission 命令](using-the-remove-multicasttransmission-command.md)
  [子命令： MulticastTransmission](subcommand-start-multicasttransmission.md)
