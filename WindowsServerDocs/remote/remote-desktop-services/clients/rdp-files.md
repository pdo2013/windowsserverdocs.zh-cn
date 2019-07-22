---
title: 受支持的远程桌面 RDP 文件设置
description: 了解远程桌面的 RDP 文件设置
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: btaintor
manager: dongill
ms.author: helohr
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 37ba3ca343eadf1bde5f75993ac2dbbb0ec05bad
ms.sourcegitcommit: 286e3181ebd2cb9d7dc7fe651858a4e0d61d153f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2019
ms.locfileid: "68300702"
---
# <a name="supported-remote-desktop-rdp-file-settings"></a>受支持的远程桌面 RDP 文件设置

下表包含可与 Windows 和 HTML 客户端配合使用的受支持 RDP 文件设置的列表。 平台列中的“x”表示支持该设置。 但是，此列表不是 Windows 和 HTML5 客户端所支持的设置的完整列表。 我们将继续更新此表，以包含更多受支持的 Windows 和 HTML5 客户端以及 macOS、iOS 和 Android 客户端的 RDP 设置。

若要详细了解如何使用 PowerShell 自定义 RDP 属性，请参阅[自定义主机池的远程桌面协议属性](https://docs.microsoft.com/azure/virtual-desktop/customize-rdp-properties)。

| RDP 设置                        | 描述            | 值                 | 默认值          | Windows 虚拟桌面 | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| alternate full address:s:value | 指定远程计算机的备用名称或 IP 地址。 | 远程计算机的任何有效名称或 IP 地址，例如“10.10.15.15” | | x | x | x |
| alternate shell:s:value        | 确定在与 RDP 连接时程序是否自动启动。 此设置对应于远程桌面连接客户端“程序”选项卡上的“程序路径和文件名”框。 若要指定备用 shell，请为该值输入可执行文件的有效路径，例如“C:\ProgramFiles\Office\word.exe”。 如果启用了 RemoteApplicationMode，此设置还可确定在连接时要启动的远程应用程序的路径或别名。 | “C:\ProgramFiles\Office\word.exe” || x | x | x |
| audiocapturemode:i:value | 指示是否启用了音频输入/输出重定向。 此设置对应于远程桌面连接客户端的“远程音频”框。 | - 0：禁用本地设备的音频捕获<br>- 1：启用本地设备的音频捕获并重定向到远程会话中的音频应用程序 | 0 | x | x | |
| audiomode:i:value | 确定本地或远程计算机是否播放音频。 | - 0：在本地计算机上播放声音（在此计算机上播放）<br>- 1：在远程计算机上播放声音（在远程计算机上播放）<br>- 2：不播放声音（不要播放） | 0 | x | x | x |
| authentication level:i:value | 定义服务器身份验证级别设置。 | - 0：如果服务器身份验证失败，请在不显示警告的情况下连接到计算机（连接且不向我发出警告）<br>- 1：如果服务器身份验证失败，请不要建立连接（不要连接）<br>- 2：如果服务器身份验证失败，请显示警告并允许我连接或拒绝连接（向我发出警告）<br>- 3：未指定身份验证要求。 | 3 | x | x ||
| autoreconnection enabled:i:value | 此设置确定客户端计算机是否会在连接断开时自动尝试重新连接到远程计算机，例如当网络连接中断时。 此设置对应于远程桌面连接 (RDC) 中“选项”下的“体验”选项卡上的“如果连接中断，则重新连接”复选框  。| - 0：客户端计算机不自动尝试重新连接<br>- 1：客户端计算机自动尝试重新连接| 1 | x | x | x |
| bandwidthautodetect:i:value | 确定是否启用了自动网络类型检测 | - 0：禁用自动网络类型检测<br>- 1：启用自动网络类型检测 | 1 | x | x | x |
| compression:i:value | 此设置确定在通过 RDP 传输到本地计算机时是否启用批量压缩。|- 0：禁用 RDP 批量压缩<br>- 1：启用 RDP 批量压缩 | 1 | x | x | x |
| desktop size id:i:value | 根据一组预定义选项指定远程会话桌面的尺寸。 如果指定了 desktopheight 或 desktopwidth，则会替代此设置。| -0：640 × 480<br>- 1：800 × 600<br>- 2：1024 × 768<br>- 3：1280 × 1024<br>- 4：1600 × 1200 | 0 | x | x | x |
| desktopheight:i:value | 使用远程桌面连接进行连接时，此设置确定远程计算机上的分辨率高度（以像素为单位）。 此设置对应于 RDC 中“选项”下的“显示”选项卡上的“显示配置”滑块中的选择。 | 数值介于 200 和 2048 之间 | 默认值设置为本地计算机的分辨率 | x | x | x |
| desktopwidth:i:value | 使用远程桌面连接进行连接时，此设置确定远程计算机上的分辨率宽度（以像素为单位）。 此设置对应于 RDC 中“选项”下的“显示”选项卡上的“显示配置”滑块中的选择。 | 数值介于 200 和 4096 之间 | 默认值设置为本地计算机的分辨率 | x | x | x |
| disableclipboardredirection:i:value | 此设置确定连接到远程计算机时是否启用剪贴板重定向。 | - 0：已启用剪贴板重定向<br>- 1：未启用剪贴板重定向 | x | x | x |
| disableconnectionsharing:i:value | 确定远程桌面客户端在 RemoteApp 或桌面启动时是重新连接到任何现有的已打开连接还是启动新的连接 | - 0：重新连接到任何现有会话<br>- 1：启动新的连接 | 0 | x | x | x |
| disableprinterredirection:i:value | 此设置确定连接到远程计算机时是否启用轻松打印打印机重定向。 | - 0：已启用轻松打印打印机重定向<br>- 1：已禁用轻松打印打印机重定向 | x | x | x |
| domain:s:value | 此设置指定将用于使用远程桌面连接 (RDC) 登录远程计算机的用户帐户所在的域的名称。 此设置的值以及用户名设置的值将在 RDC 中“选项”下的“常规”选项卡上的“用户名”文本框中显示。 | 有效的域名，例如“CONTOSO” | 无默认值 | x | x | x |
| drivestoredirect:s:value | 确定客户端计算机上将被重定向并在远程会话中可用的本地磁盘驱动器。 | 未指定值：不重定向任何驱动器<br>*：重定向所有磁盘驱动器，包括稍后连接的驱动器<br> DynamicDrives：重定向稍后连接的所有驱动器<br>驱动器以及一个或多个驱动器的标签：重定向指定的驱动器| 未指定值：不重定向任何驱动器 | x | x    | |
| enablecredsspsupport:i:value | 此设置确定 RDP 是否会在凭据安全支持提供程序 (CredSSP) 可用的情况下使用它来进行身份验证。 | - 0：即使操作系统支持 CredSSP，RDP 也不会使用 CredSSP<br>- 1：如果操作系统支持 CredSSP，则 RDP 将使用 CredSSP | 1 | x | x | |
| full address:s:value | 此设置指定想要连接到的远程计算机的名称或 IP 地址 | 有效的计算机名称、IPv4 地址或 IPv6 地址。 | | x | x | x |
| gatewaycredentialssource:i:value | 指定或检索 RD 网关身份验证方法。 | - 0：要求提供密码 (NTLM)<br>- 1：使用智能卡<br>- 4：允许用户稍后选择 | 0 | x | x | x |
| gatewayhostname:s:value | 指定 RD 网关主机名。 | 有效的网关服务器地址。 ||x|x|x|
| gatewayprofileusagemethod:i:value | 指定是否使用默认 RD 网关设置 | - 0：使用管理员指定的默认配置文件模式<br>- 1：使用用户指定的显式设置 | 0 | x | x | x |
| gatewayusagemethod:i:value | 指定何时使用 RD 网关服务器 | - 0：不使用 RD 网关服务器<br>- 1：始终使用 RD 网关服务器<br>- 2：如果无法与 RD 会话主机建立直接连接，则使用 RD 网关服务器<br>- 3：使用默认 RD 网关服务器设置<br>- 4：不使用 RD 网关，对于本地地址不使用服务器<br>将此属性值设置为 0 或 4 的实际效果相同，但将此属性设置为 4 时可获得用于跳过本地地址的选项。 | | x | x | x |
| networkautodetect:i:value | 确定是否使用自动网络带宽检测。 需要设置 bandwidthautodetect 选项并与连接类型 7 相关联。 | - 0：不使用自动网络带宽检测<br> - 1：使用自动网络带宽检测 | 1 | x ||x|
| promptcredentialonce:i:value | 确定是否保存用户的凭据并将其用于 RD 网关和远程计算机。|- 0：远程会话不使用相同的凭据<br>- 1：远程会话将使用相同的凭据|1|x|x||
| redirectclipboard:i:value | 此设置确定本地计算机上的剪贴板是否将被重定向并在远程会话中可用。 此设置对应于 RDC 中“选项”下的“本地资源”选项卡上的“剪贴板”复选框 ****  。 | - 0：本地计算机上的剪贴板在远程会话中不可用<br>- 1：本地计算机上的剪贴板在远程会话中可用|1|x|x|x|
| redirectdrives:i:value | 此设置确定客户端计算机上的驱动器是否将被重定向并在远程会话中可用。 此设置对应于 RDC 中“选项”下的“本地资源”选项卡上的“更多”下的“驱动器”选择 ****  。|- 0：本地计算机上的驱动器在远程会话中不可用<br>- 1：本地计算机上的驱动器在远程会话中可用|0|x|x| |
| redirectprinters:i:value | 此设置确定当你使用远程桌面连接连接到远程计算机时，客户端计算机上配置的打印机是否将被重定向并在远程会话中可用。 此设置对应于 RDC 中“选项”下的“本地资源”选项卡上的“打印机”复选框中的选择 ****  。 | - 0：本地计算机上的打印机在远程会话中不可用<br>- 1：本地计算机上的打印机在远程会话中可用|1|x|x|x|
| redirectsmartcards:i:value | 此设置确定当你使用远程桌面连接 (RDC) 连接到远程计算机时，客户端计算机上的智能卡设备是否将被重定向并在远程会话中可用。 此设置对应于 RDC 中“选项”下的“本地资源”选项卡上的“更多”下的“智能卡”复选框中的选择 ****  。|- 0：本地计算机上的智能卡设备在远程会话中不可用<br>- 1：本地计算机上的智能卡设备在远程会话中可用|1|x|x||
| remoteapplicationcmdline:s:value | RemoteApp 的可选命令行参数。||x|x|x|
| remoteapplicationexpandcmdline:i:value| 确定 RemoteApp 命令行参数中包含的环境变量应该在本地扩展还是远程扩展。|- 0：应将环境变量扩展为本地计算机的值<br>- 1：应在远程计算机上将环境变量扩展为远程计算机的值||x|x|x|
| remoteapplicationexpandworkingdir | 确定 RemoteApp 工作目录参数中包含的环境变量应该在本地扩展还是远程扩展。 | - 0：应将环境变量扩展为本地计算机的值<br> - 1：应在远程计算机上将环境变量扩展为远程计算机的值。<br>RemoteApp 工作目录通过 shell 工作目录参数指定。||x|x|x|
|remoteapplicationfile:s:value | 指定 RemoteApp 要在远程计算机上打开的文件。<br>若要打开本地文件，还必须为源驱动器启用驱动器重定向。||x|x|x|
|remoteapplicationicon:s:value | 指定在启动 RemoteApp 时要在客户端 UI 中显示的图标文件。 如果未指定文件名，则客户端将使用标准远程桌面图标。 仅支持“.ico”文件。||x|x|x|
|remoteapplicationmode:i:value | 确定是否将 RemoteApp 连接作为 RemoteApp 会话启动。| - 0：不启动 RemoteApp 会话<br>- 1：启动 RemoteApp 会话|1|x|x|x|
|remoteapplicationname:s:value | 指定启动 RemoteApp 时客户端界面中的 RemoteApp 名称。| 例如，“Excel 2016”。|x|x|x|
|remoteapplicationprogram:s:value | 指定 RemoteApp 的别名或可执行文件名称。 | 例如，“EXCEL”。 |x|x|x|
|screen mode id:i:value | 此设置确定使用远程桌面连接连接到远程计算机时远程会话窗口是否全屏显示。 此设置对应于 RDC 中“选项”下的“显示”选项卡上的“显示配置”滑块中的选择。|- 0：远程会话将在窗口中显示<br>- 1：远程会话将全屏显示|2|x|x|x|
|smart sizing:i:value | 此设置确定客户端计算机是否可以缩放远程计算机上的内容以适应客户端计算机的窗口大小。|- 0：调整大小时，客户端窗口显示内容不缩放<br>- 1：调整大小时，客户端窗口显示内容将缩放|0|x|x||
| use multimon:i:value | 使用远程桌面连接连接到远程计算机时，此设置配置多监视器支持。|- 0：不启用多监视器支持<br>- 1：启用多监视器支持|0|x|x||
| username:s:value | 此设置指定将用于使用远程桌面连接登录远程计算机的用户帐户的名称。 此设置的值以及域设置的值将在 RDC 中“选项”下的“常规”选项卡上的“用户名”框中显示。| 任何有效的用户名。 ||x|x|x|
| videoplaybackmode:i:value| 此设置确定远程桌面连接是否将使用 RDP 高效多媒体流式处理进行视频播放。|- 0：不使用 RDP 高效多媒体流式处理进行视频播放<br>- 1：在可能的情况下使用 RDP 高效多媒体流式处理进行视频播放|1|x|x||
| workspaceid:s:value | 此设置定义与包含此设置的 RDP 文件关联的 RemoteApp 和桌面 ID。 | 有效的 RemoteApp 和桌面连接 ID|x|x||
