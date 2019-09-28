---
title: 子命令集-TransportServer
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f91cca044d4c2922791ccb63b0e3cec4af685f17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370740"
---
# <a name="subcommand-set-transportserver"></a>子命令： set-TransportServer

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

设置传输服务器的配置设置。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Set-TransportServer [/Server:<Server name>]
     [/ObtainIpv4From:{Dhcp | Range}]
        [/start:<starting IP address>]
        [/End:<Ending IP address>]
     [/ObtainIpv6From:Range]\n\
        [/start:<start IP address>]\n\
        [/End:<End IP address>]      
     [/startPort:<starting port>
        [/EndPort:<starting port>
     [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]    
     [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/Server： @no__t]|指定传输服务器的名称。 此名称可以是 NetBIOS 名称或完全限定的域名（FQDN）。 如果未指定传输服务器名称，则使用本地服务器。|
|[/ObtainIpv4From： {Dhcp &#124;范围}]|设置 IPv4 地址的源，如下所示：<br /><br />-[/start： @no__t] 设置 IP 地址范围的起始 IP 地址范围。 这是必需的，仅当此选项设置为 "**范围**" 时才有效。<br />-[/End： @no__t] 设置 IP 地址范围的结束。 这是必需的，仅当此选项设置为 "**范围**" 时才有效。<br />-[/startPort： @no__t] 设置端口范围的开头。<br />-[/EndPort： @no__t] 设置端口范围的结尾。|
|[/ObtainIpv6From： Range]|指定 IPv6 地址的源。 此选项仅适用于 Windows Server 2008 R2，唯一受支持的值为**范围**。<br /><br />-[/start： @no__t] 设置 IP 地址范围的起始 IP 地址范围。 这是必需的，仅当此选项设置为 "**范围**" 时才有效。<br />-[/End： @no__t] 设置 IP 地址范围的结束。 这是必需的，仅当此选项设置为 "**范围**" 时才有效。<br />-[/startPort： @no__t] 设置端口范围的开头。<br />-[/EndPort： @no__t] 设置端口范围的结尾。|
|[/Profile： {10Mbps &#124; 100mbps &#124; 1Gbps &#124; Custom}]|指定要使用的网络配置文件。 此选项仅适用于运行 Windows Server 2008 或 Windows Server 2003 的服务器。|
|[/MulticastSessionPolicy]|配置多播传输的传输设置。 此命令仅适用于 Windows Server 2008 R2。<br /><br />-[/Policy： {None &#124; AutoDisconnect &#124; Multistream}] 确定如何处理速度缓慢的客户端。 "**无**" 表示将所有客户端保持在一个会话中的速度相同。 **AutoDisconnect**表示断开连接到指定 **/Threshold**以下的任何客户端。 **Multistream**表示客户端将被划分为 **/StreamCount**指定的多个会话。<br />-[/Threshold： @no__t] 设置/Policy 的最小传输速率（KBps） **： AutoDisconnect**。 低于此速率的客户端将与多播传输断开连接。<br />-[/StreamCount： {2 &#124; 3}] [/Fallback： {Yes &#124; No}] 确定/policy 的会话数 **： Multistream**。 **2**表示两个会话（快速和缓慢）， **3**表示三个会话（慢、中、快）。<br />-[/Fallback： {Yes &#124; No}] 确定是否断开客户端的连接将使用另一种方法继续传输（如果客户端支持）。 如果你使用的是 WDS 客户端，则计算机将回退为单播。 Wdsmcast.exe 不支持回退机制。 此选项也适用于不支持**Multistream**的客户端。 在这种情况下，计算机将回退到其他方法，而不是移到较慢的传输会话。|
## <a name="BKMK_examples"></a>示例
若要设置服务器的 IPv4 地址范围，请键入：
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
若要为服务器设置 IPv4 地址范围、端口范围和配置文件，请键入：
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
#### <a name="additional-references"></a>其他参考
使用[TransportServer 命令](using-the-disable-transportserver-command.md)的[命令行语法键](command-line-syntax-key.md)
 @no__t[-3 使用](using-the-enable-transportserver-command.md)TransportServer 命令 
 使用[命令](using-the-get-transportserver-command.md)
[子命令：TransportServer](subcommand-start-transportserver.md)
[子命令： TransportServer](subcommand-stop-transportserver.md)
