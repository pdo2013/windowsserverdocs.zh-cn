---
title: 子命令来设置服务器
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da55c29d-a94a-4d73-877b-af480f906ca0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abc3fe23558f077e0ba9ac69f2641e3b8c9cde4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858378"
---
# <a name="subcommand-set-server"></a>子命令： 设置服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

配置 Windows 部署服务服务器的设置。
## <a name="syntax"></a>语法
```
wdsutil [Options] /Set-Server [/Server:<Server name>]
    [/Authorize:{Yes | No}]
    [/RogueDetection:{Yes | No}]
    [/AnswerClients:{All | Known | None}]
    [/Responsedelay:<time in seconds>]
    [/AllowN12forNewClients:{Yes | No}]
    [/ArchitectureDiscovery:{Yes | No}]
    [/resetBootProgram:{Yes | No}]
    [/DefaultX86X64Imagetype:{x86 | x64 | Both}]
    [/UseDhcpPorts:{Yes | No}]
    [/DhcpOption60:{Yes | No}]
    [/RpcPort:<Port number>]
    [/PxepromptPolicy 
        [/Known:{OptIn | Noprompt | OptOut}]
        [/New:{OptIn | Noprompt | OptOut}] 
    [/BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/N12BootProgram:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/BootImage:<Relative path>]
         /Architecture:{x86 | ia64 | x64}
    [/PreferredDC:<DC Name>]
    [/PreferredGC:<GC Name>]
    [/PrestageUsingMAC:{Yes | No}]
    [/NewMachineNamingPolicy:<Policy>]
    [/NewMachineOU]
         [/type:{Serverdomain | Userdomain | UserOU | Custom}]
         [/OU:<Domain name of OU>]
    [/DomainSearchOrder:{GCOnly | DCFirst}]
    [/NewMachineDomainJoin:{Yes | No}]
    [/OSCMenuName:<Name>]
    [/WdsClientLogging]
         [/Enabled:{Yes | No}]
         [/LoggingLevel:{None | Errors | Warnings | Info}]
    [/WdsUnattend]
         [/Policy:{Enabled | Disabled}]
         [/CommandlinePrecedence:{Yes | No}]
         [/File:<path>]
             /Architecture:{x86 | ia64 | x64}
    [/AutoaddPolicy]
         [/Policy:{AdminApproval | Disabled}]
         [/PollInterval:{time in seconds}]
         [/MaxRetry:{Retries}]
         [/Message:<Message>]
         [/RetentionPeriod]
             [/Approved:<time in days>]
             [/Others:<time in days>]
    [/AutoaddSettings]
         /Architecture:{x86 | ia64 | x64}
         [/BootProgram:<Relative path>]
         [/ReferralServer:<Server name>
         [/WdsClientUnattend:<Relative path>]
         [/BootImage:<Relative path>]
         [/User:<Owner>]
         [/JoinRights:{JoinOnly | Full}]
         [/JoinDomain:{Yes | No}]
    [/BindPolicy]
         [/Policy:{Include | Exclude}]
         [/add]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
         [/remove]
              /address:<IP or MAC address>
              /addresstype:{IP | MAC}
    [/RefreshPeriod:<time in seconds>]
    [/BannedGuidPolicy]
         [/add]
              /Guid:<GUID>
         [/remove]
             /Guid:<GUID>
    [/BcdRefreshPolicy]
         [/Enabled:{Yes | No}]
         [/RefreshPeriod:<time in minutes>]
    [/Transport]
         [/ObtainIpv4From:{Dhcp | Range}]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/ObtainIpv6From:Range]
             [/start:<start IP address>]
             [/End:<End IP address>]
         [/startPort:<start Port>
             [/EndPort:<start Port>
        [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]
        [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]    
        [/forceNative]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|[/ 服务器：<Server name>]|指定的服务器的名称。 这可以是 NetBIOS 名称或完全限定的域名 (FQDN)。 如果指定没有服务器名称，则将使用本地服务器。|
|[/ 授权: {是&#124;No}]|指定是否对此服务器中动态主机控制协议 (DHCP) 进行授权。|
|[/RogueDetection:{Yes &#124; No}]|启用或禁用 DHCP 恶意检测。|
|[/ AnswerClients: {所有&#124;已知&#124;None}]|指定此服务器将回答哪些客户端。 如果将此值设置为**已知**，必须在 active directory 域服务 (AD DS) 中预留计算机之前将回答了由 Windows 部署服务服务器。|
|[/ Responsedelay:<time in seconds>]|服务器在回答启动客户端之前将等待的时间量。 此设置不适用于预留计算机。|
|[/AllowN12forNewClients:{Yes &#124; No}]|对于 Windows Server 2008 中，指定未知客户端将不需要按 F12 键以开始网络启动。 已知客户端将收到指定的计算机的启动程序，或如果未指定，为指定的启动程序体系结构。<br /><br />对于 Windows Server 2008 R2，此选项已替换为以下命令： wdsutil /Set-Server /PxepromptPolicy /New:Noprompt|
|[/ ArchitectureDiscovery: {是&#124;No}]|启用或禁用体系结构发现。 这样便于正确不广播其体系结构基于 x64 的客户端的发现。|
|[/resetBootProgram:{Yes &#124; No}]|确定是否将客户端的只是启动而无需按 F12 键清除启动路径。|
|[/ DefaultX86X64Imagetype: {x86 &#124; x64&#124;同时}]|启动映像的控件将显示到基于 x64 的客户端。|
|[/UseDhcpPorts:{Yes &#124; No}]|指定是否应尝试将绑定到 DHCP 端口 PXE 服务器，TCP 端口 67。 如果同一台计算机上运行 DHCP 和 Windows 部署服务，应将此选项设置为**否**若要启用 DHCP 服务器以使用该端口，并设置 **/DhcpOption60**参数**是**。 此值的默认设置**是**。|
|[/DhcpOption60:{Yes &#124; No}]|指定是否在 PXE 支持，应将配置 DHCP 选项 60。 如果同一台服务器上运行 DHCP 和 Windows 部署服务，此选项设置为**是**并设置 **/UseDhcpPorts**选项设置为**否**。 此值的默认设置**否**。|
|[/RpcPort:<Port number>]|指定要用于客户端请求提供服务的 TCP 端口号。|
|[/PxepromptPolicy]|配置如何已知 （预留） 和新的客户端开始 PXE 启动。 此选项仅适用于 Windows Server 2008 R2。 设置设置，请使用以下选项：<br /><br />-[/ 已知: {OptIn&#124;选择禁用&#124;Noprompt}]-为预留的客户端设置的策略。<br />-[/New: {OptIn&#124;选择禁用&#124;Noprompt}]-为新客户端设置的策略。<br /><br />**OptIn**意味着，客户端需要按某个键按进行 PXE 启动的顺序，否则它将回退到下一个启动设备。<br /><br />**Noprompt**意味着客户端始终将 PXE 启动。<br /><br />**选择禁用**意味着客户端将 PXE 启动，除非按下了 Esc 键。|
|[/ BootProgram:<Relative path>] /Architecture: {x86 &#124; ia64 &#124; x64}|RemoteInstall 文件夹中指定的启动程序的相对路径 (例如， **boot\x86\pxeboot.n12**)，并指定引导程序的体系结构。|
|[/ N12BootProgram:<Relative path>] /Architecture: {x86 &#124; ia64 &#124; x64}|指定不要求按 F12 键的启动程序的相对路径 (例如， **boot\x86\pxeboot.n12**)，并指定引导程序的体系结构。|
|[/ 启动映像：<Relative path>] /Architecture: {x86 &#124; ia64 &#124; x64}|指定启动客户端应接收，并指定启动映像的体系结构的启动映像的相对路径。 可以为每个体系结构指定。|
|[/PreferredDC:<DC Name>]|指定应使用 Windows 部署服务的域控制器的名称。 这可以是 NetBIOS 名称或 FQDN。|
|[/PreferredGC:<GC Name>]|指定应使用 Windows 部署服务的全局编录服务器的名称。 这可以是 NetBIOS 名称或 FQDN。|
|[/ PrestageUsingMAC: {是&#124;No}]|指定 Windows 部署服务，在 AD DS 中创建计算机帐户时是否应使用的 MAC 地址而不是 GUID/UUID 标识的计算机。|
|[/NewMachineNamingPolicy:<Policy>]|指定要生成的客户端的计算机名称时使用的格式。 有关要使用的格式信息<policy>，右键单击 mmc 管理单元中的服务器，单击**属性**，并查看**目录服务**选项卡。例如， **/NewMachineNamingPolicy: %61username%#**。|
|[/NewMachineOU]|用于在 AD DS 中指定的位置将在其中创建客户端计算机帐户。 指定使用以下选项的位置。<br /><br />-[/type:Serverdomain &#124; Userdomain&#124;用户组织单位&#124;自定义] 指定的位置的类型。 **Serverdomain**与 Windows 部署服务服务器在同一个域中创建帐户。 **Userdomain**在执行安装的用户所在的域中创建帐户。 **用户组织单位**在执行安装的用户的组织单位中创建帐户。 **自定义**允许你指定自定义位置 (你还必须指定的值 **/OU**使用此选项)。<br />-[/ OU:<Domain name of OU>]-如果你指定**自定义**有关 **/type**选项，此选项指定应在其中创建计算机帐户的组织单位。|
|[/DomainSearchOrder:{GCOnly &#124; DCFirst}]|指定用于在 AD DS （全局目录或域控制器） 中搜索计算机帐户的策略。|
|[/NewMachineDomainJoin:{Yes &#124; No}]|指定应在安装过程到域中加入已不在 AD DS 中预留的计算机。 默认设置是**是**。|
|[/WdsClientLogging]|指定服务器的日志记录级别。<br /><br />-[/ 已启用: {是&#124;No}]-启用或禁用 Windows 部署服务客户端操作的日志记录。<br />-[/ LoggingLevel: {None&#124;错误&#124;警告&#124;Info}-设置日志记录级别。 **无**等效于禁用日志记录。 **错误**是最低级别的日志记录并指示仅错误将被记录。 **警告**包括警告和错误。 **信息**是日志记录的最高级别，包括错误、 警告和信息性事件。|
|[/WdsUnattend]|这些设置控制 Windows 部署服务客户端的无人参与的安装行为。 设置设置，请使用以下选项：<br /><br />-[/ 策略: {已启用&#124;已禁用}]-指定是否使用无人参与的安装。<br />-[/ CommandlinePrecedence: {是&#124;No}]-指定是否使用 Autounattend.xml 文件 （如果在客户端上存在） 或直接向 Windows 部署服务客户端使用 /Unattend 选项传递了一个无人参与的安装文件而不是在客户端安装过程中的映像无人参与文件。 默认设置是**否**。<br />-[/ 文件：<Relative path> /Architecture: {x86 &#124; ia64 &#124; x64}]-指定文件名、 路径和体系结构的无人参与文件。|
|[/AutoaddPolicy]|这些设置控制自动添加策略。 定义设置，请使用以下选项：<br /><br />-[/ 策略: {AdminApproval&#124;已禁用}]- **AdminApprove**将导致所有未知的计算机要添加到挂起队列，其中管理员可以然后查看计算机的列表并批准或拒绝每个请求，作为适当。 **已禁用**指示未知的计算机尝试将启动到服务器时进行任何其他操作。<br />-[/ PollInterval: {时间以秒为单位}]-指定时间间隔 （以秒为单位） 的网络启动程序应在其中轮询的 Windows 部署服务服务器。<br />-[/ MaxRetry: <Number>]-指定的网络引导程序应轮询的 Windows 部署服务服务器的次数。 此值，以及与 **/PollInterval**，决定了多长时间的网络引导程序将等待管理员批准或拒绝在超时之前的计算机。例如， **MaxRetry**值为 10 和一个**PollInterval** vlue 60 会指出，客户端应轮询服务器 10 次，等待 60 秒之间的尝试。 因此，客户端在 10 分钟 （10 x 60 秒 = 10 分钟） 后将超时时间。<br />-[/ 消息： <Message>]-指定向客户端在网络启动程序对话框页上显示的消息。<br />-[/RetentionPeriod]-指定在计算机处于挂起状态被自动清除前的天数。<br />-[/ 批准： <time in days>]-指定已批准计算机的保留期。 必须使用此参数与 **/RetentionPeriod**选项。<br />-[/ 其他人： <time in days>]-指定未经批准的计算机的保留期 （已拒绝或挂起）。 必须使用此参数与 **/RetentionPeriod**选项。|
|[/AutoaddSettings]|指定要应用于每台计算机的默认设置。 定义设置，请使用以下选项：<br /><br />-/Architecture: {x86 &#124; ia64 &#124; x64}-指定体系结构。<br />-[/ BootProgram: <Relative path>]-指定发送到已批准的计算机的启动程序。 如果指定不启动程序，则将使用的默认值为 （在服务器上指定） 的计算机的体系结构。<br />-[/ WdsClientUnattend: <Relative path>]-已批准的客户端应接收的无人参与文件设置的相对路径。<br />-[/ ReferralServer: <Server name>]-指定客户端将用于下载映像的 Windows 部署服务服务器。<br />-[/ 启动映像： <Relative path>]-指定已批准的客户端将接收的启动映像。<br />-[/ 用户： < 域 \ 用户&#124; User@Domain>]-在要为指定的用户提供足够的权限来将计算机加入到域的计算机帐户对象上设置权限。<br />-[JoinRights: {JoinOnly&#124;完整}]-指定要分配给用户的权限类型。 **JoinOnly**要求管理员重置计算机帐户，然后用户可以将计算机加入到域。 **完整**给用户，包括将计算机加入到域的权限授予完全访问权限。<br />-[/ JoinDomain: {是&#124;No}]-指定是否在计算机应该加入到域与此计算机帐户在 Windows 部署服务安装过程。 默认设置是**是**。|
|[/BindPolicy]|配置要在其上侦听的 PXE 提供程序的网络接口。 定义策略使用以下选项：<br /><br />-[/ 策略: {包括&#124;排除}] 的设置来包含或排除的接口列表上的地址的接口绑定策略。<br />-[/ 添加]-将接口添加到列表。 您还必须指定 /addresstype 和 /address。<br />-[/remove]-从列表中删除一个接口。 您还必须指定 /addresstype 和 /address。<br />-地址：<IP or MAC address> -指定要添加或删除的接口的 IP 或 MAC 地址。<br />-/addresstype: {IP &#124; MAC}-指示的地址中指定的类型**地址**选项。|
|[/ RefreshPeriod: <seconds>]|指定频率 （以秒为单位） 刷新其设置的服务器会。|
|[/BannedGuidPolicy]|管理已禁止使用以下选项的 Guid 的列表：<br /><br />-[/ 添加] /Guid:<GUID> -将指定的 GUID 添加到已禁止的 Guid 的列表。 具有此 GUID 的任何客户端将由其 MAC 地址改为标识。<br />-[/remove] /Guid:<GUID> -已禁止的 Guid 的列表中移除指定的 GUID。|
|[/BcdRefreshPolicy]|刷新 Bcd 文件，请使用以下选项将配置设置：<br /><br />-[/ 已启用: {是&#124;No}]-指定 Bcd 刷新策略。 当 **/启用**设置为**是**，Bcd 文件刷新指定的时间间隔。<br />-[/ RefreshPeriod:<time in minutes>]-指定刷新的 Bcd 文件的时间间隔。|
|[/Transport]|配置下列选项：<br /><br /><ul><li>[/ ObtainIpv4From: {Dhcp&#124;范围}]-指定 IPv4 地址的来源。<br /><br /><ul><li>[/ 启动： <starting Ipv4 address>]-指定的 IP 地址范围的起始位置。 此选项是必需的并且有效仅当 **/ObtainIpv4From**设置为**范围**</li><li>[结束： <Ending Ipv4 address>]-指定的 IP 地址范围的结束。 此选项是必需的并且有效仅当 **/ObtainIpv4From**设置为**范围**。</li></ul></li><li>[/ ObtainIpv6From:Range][/ 启动：<start IP address>] [/ 结束：<End IP address>] 指定 IPv6 地址的来源。 此选项仅适用于 Windows Server 2008 R2，唯一支持的值范围。</li><li>[/ startPort: <starting port>]-指定的端口范围的起始位置。</li><li>[/ EndPort: <Ending port>]-指定的端口范围的结束。</li><li>[/ 配置文件: {10Mbps &#124; 100Mbps &#124; 1Gbps&#124;自定义}]-指定要使用的网络配置文件。 此选项是运行 Windows Server 2008 仅支持的 forservers。</li><li>[/MulticastSessionPolicy] 配置多播传输的传输设置。 此命令才可用于 Windows Server 2008 R2。<br /><br /><ul><li>[/ 策略: {None&#124;自动断开连接&#124;多流}]-确定如何处理速度缓慢的客户端。 无表示所有客户端在一个会话中保持相同的速度。 自动断开连接意味着降到指定 /Threshold 任何客户端将断开连接。 多流意味着客户端将分隔到多个会话中指定的 /StreamCount。</li><li>[/ 阈值：<Speed in KBps>]-/Policy:AutoDisconnect，此选项集以 kbps 为单位的最小传输速率。 此速率降至的客户端将从多播传输断开连接。</li><li>[/ StreamCount: {2 &#124; 3}][/ 回退: {是&#124;No}]-/Policy:Multistream，对于此选项确定的会话数。 2 表示两个会话 （快速和慢速） 3 表示三个会话 (慢速、 中等，fast）)。</li><li>[/ 回退: {是&#124;No}]-确定是否已断开连接的客户端将继续使用另一种方法 （如果客户端支持） 传输。 如果使用 WDS 客户端，计算机将回退到单播。 Wdsmcast.exe 不支持回退机制。 此选项也适用于不支持多流的客户端。 在这种情况下，计算机将回退到另一种方法，而不是移动到较慢的传输会话。</li></ul></li></ul>|
## <a name="BKMK_examples"></a>示例
若要设置服务器应答已知的客户，且响应延迟为 4 分钟，请键入：
```
wdsutil /Set-Server /AnswerClients:Known /Responsedelay:4
```
若要设置服务器的启动程序和体系结构，请键入：
```
wdsutil /Set-Server /BootProgram:boot\x86\pxeboot.n12 /Architecture:x86
```
若要启用日志记录在服务器上，键入：
```
wdsutil /Set-Server /WdsClientLogging /Enabled:Yes /LoggingLevel:Warnings
```
若要启用无人参与的服务器，以及体系结构和客户端无人参与文件，类型：
```
wdsutil /Set-Server /WdsUnattend /Policy:Enabled /File:WDSClientUnattend \unattend.xml /Architecture:x86
```
若要设置预启动执行环境 (PXE) 服务器，以尝试将绑定到 TCP 端口 67 和 60，请键入：
```
wdsutil /Set-server /UseDhcpPorts:No /DhcpOption60:Yes
```
#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[使用禁用服务器命令](using-the-disable-server-command.md)
[使用启用服务器命令](using-the-enable-server-command.md)
[使用获取服务器命令](using-the-get-server-command.md)
[使用初始化服务器命令](using-the-initialize-server-command.md)
[子命令： 启动 Server](subcommand-start-server.md) 
 [子命令： 停止服务器](subcommand-stop-server.md)
[uninitialize 服务器选项](the-uninitialize-server-option.md)
