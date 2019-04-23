---
title: 子命令 Set-transportserver
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d144ed7d461cbebcd351aa4347fde20f35736c26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886598"
---
# <a name="subcommand-set-transportserver"></a>Subcommand: set-TransportServer

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

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
|[/ 服务器：<Server name>]|指定在传输服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果不指定任何传输服务器名称，则使用本地服务器。|
|[/ObtainIpv4From:{Dhcp &#124; Range}]|设置 IPv4 地址的源，如下所示：<br /><br />-[/start: <IP address>] 设置 IP 地址范围的开始。 这是必需的和有效仅当此选项设置为**范围**。<br />-[结束： <IP address>] 设置的 IP 地址范围的结束。 这是必需的和有效仅当此选项设置为**范围**。<br />-[/ startPort: <port>] 设置的端口范围起始。<br />-[/ EndPort: <port>] 设置的端口范围的结束。|
|[/ObtainIpv6From:Range]|指定 IPv6 地址的来源。 此选项仅适用于 Windows Server 2008 R2 和唯一支持的值是**范围**。<br /><br />-[/start: <IP address>] 设置 IP 地址范围的开始。 这是必需的和有效仅当此选项设置为**范围**。<br />-[结束： <IP address>] 设置的 IP 地址范围的结束。 这是必需的和有效仅当此选项设置为**范围**。<br />-[/ startPort: <port>] 设置的端口范围起始。<br />-[/ EndPort: <port>] 设置的端口范围的结束。|
|[/ 配置文件: {10Mbps &#124; 100Mbps &#124; 1Gbps&#124;自定义}]|指定要使用的网络配置文件。 此选项才适用于运行 Windows Server 2008 或 Windows Server 2003 的服务器。|
|[/MulticastSessionPolicy]|配置多播传输的传输设置。 此命令才可用于 Windows Server 2008 R2。<br /><br />-[/ 策略: {None&#124;自动断开连接&#124;多流}] 确定如何处理速度缓慢的客户端。 **无**意味着所有客户端在一个会话中保持相同的速度。 **自动断开连接**意味着降到指定的任何客户端 **/Threshold**断开连接。 **多流**意味着客户端将分隔到多个会话中指定的 **/StreamCount**。<br />-[/ 阈值：<Speed in KBps>] 以 kbps 为单位的设置的最小传输速率 **/Policy:AutoDisconnect**。 此速率降至的客户端从多播传输断开连接。<br />-[/ StreamCount: {2 &#124; 3}] [/ 回退: {是&#124;No}] 确定的会话数 **/Policy:Multistream**。 **2**意味着两个会话 （快速和慢速），并**3**意味着三个会话 (慢速、 中等，fast）)。<br />-[/ 回退: {是&#124;No}] 确定是否已断开连接的客户端 tha 将继续传输 （如果支持客户端） 通过另一种方法。 如果使用 WDS 客户端，计算机将回退到单播。 Wdsmcast.exe 不支持回退机制。 此选项也适用于不支持的客户端**多流**。 在这种情况下，计算机将回退到另一种方法，而不是移动到较慢的传输会话。|
## <a name="BKMK_examples"></a>示例
若要设置服务器的 IPv4 地址范围，键入：
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
若要设置服务器的 IPv4 地址范围、 端口范围和配置文件，请键入：
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用禁用 TransportServer 命令](using-the-disable-transportserver-command.md)
[使用启用 TransportServer 命令](using-the-enable-transportserver-command.md)
 [使用 get TransportServer 命令](using-the-get-transportserver-command.md)
[子命令： 开始 TransportServer](subcommand-start-transportserver.md)
[子命令： 停止 TransportServer](subcommand-stop-transportserver.md)
