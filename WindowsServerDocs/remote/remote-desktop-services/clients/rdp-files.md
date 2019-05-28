---
title: 支持的远程桌面 RDP 文件设置
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
ms.openlocfilehash: fbec5c27b7cb3ef0ef750a5ebd7d7ea2654c47b3
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65983457"
---
# <a name="supported-remote-desktop-rdp-file-settings"></a>支持远程桌面 RDP 文件设置

下表包含支持与 Windows 和 HTML 客户端可以使用的 RDP 文件设置的列表。 平台列中的"x"指示支持设置。 但是，此列表并不受支持的 Windows 和 HTML5 客户端设置的完整列表。 我们将继续更新此表，以包括更多受支持的 RDP 设置的 Windows 和 HTML5 客户端，以及 macOS、 iOS 和 Android 客户端。

| RDP 设置                        | 描述            | 值                 | 默认值          | Windows 虚拟桌面 | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| 备用完整 s:value 地址： | 指定的备用名称或远程计算机的 IP 地址。 | 任何有效的名称或 IP 地址的远程计算机，例如"10.10.15.15" | | x | x | x |
| 备用 shell: s:value        | 确定是否自动启动程序时通过 RDP 连接。 此设置对应于程序路径和文件名框的程序选项卡上的远程桌面连接客户端。 若要指定可选的 shell，输入值，如"C:\ProgramFiles\Office\word.exe"的可执行文件的有效路径。 此设置还确定哪个路径或如果启用 RemoteApplicationMode 在连接时启动的远程应用程序的别名。 | "C:\ProgramFiles\Office\word.exe" || x | x | x |
| audiocapturemode:i:value | 指示是否启用音频的输入/输出重定向。 此设置对应于远程桌面连接客户端的远程音频框。 | - 0:禁用从本地设备的音频捕获<br>- 1:启用从本地设备和重定向到远程会话中的音频应用程序的音频捕获 | 0 | x | x | |
| audiomode:i:value | 确定本地或远程计算机播放音频。 | - 0:本地计算机 （在此计算机上播放） 上播放声音<br>- 1:远程计算机 （播放远程计算机上） 上播放声音<br>- 2:不播放的声音 （无法播放） | 0 | x | x | x |
| 身份验证级别： i: | 定义服务器身份验证级别设置。 | - 0:如果服务器身份验证失败，连接到计算机而不发出警告 (连接和不向我发出警告)<br>- 1:如果服务器身份验证失败，未建立连接 （不连接）<br>- 2:如果服务器身份验证失败，显示一条警告，并允许我连接或拒绝的连接 （警告我）<br>- 3:指定没有身份验证要求。 | 3 | x | x ||
| 已启用的服务： i: | 此设置确定是否在客户端计算机将自动尝试重新连接到远程计算机，如果删除连接，如网络连接中断时。 如果连接已删除的复选框下的体验选项卡上，此设置对应于重新连接**选项**中远程桌面连接 (RDC)。| - 0:客户端计算机不会自动尝试重新连接<br>- 1:客户端计算机会自动尝试重新连接| 1 | x | x | x |
| bandwidthautodetect:i:value | 确定是否启用了自动网络类型检测 | - 0:禁用自动网络类型检测<br>- 1:启用自动网络类型检测 | 1 | x | x | x |
| compression:i:value | 此设置确定到本地计算机通过 RDP 传输时是否启用大容量压缩。|- 0:禁用 RDP 批量压缩<br>- 1:启用 RDP 批量压缩 | 1 | x | x | x |
| 桌面大小 id: i: | 指定一组预定义选项从在远程会话桌面的尺寸。 如果指定 desktopheight 或 desktopwidth 重写此设置。| -0:640×480<br>- 1:800×600<br>- 2:1024×768<br>- 3:1280×1024<br>- 4:1600×1200 | 0 | x | x | x |
| desktopheight:i:value | 通过使用远程桌面连接进行连接时，此设置确定在远程计算机上的解析高度 （以像素为单位）。 此设置对应于中显示配置滑块中 RDC 选项下的显示选项卡上的选项。 | 200 和 2048年之间的数字值 | 默认值设置为本地计算机上的分辨率 | x | x | x |
| desktopwidth:i:value | 通过使用远程桌面连接进行连接时，此设置确定在远程计算机上的解析宽度 （以像素为单位）。 此设置对应于中显示配置滑块中 RDC 选项下的显示选项卡上的选项。 | 200 和 4096 之间的数字值 | 默认值设置为本地计算机上的分辨率 | x | x | x |
| disableclipboardredirection:i:value | 此设置确定是否连接到远程计算机时启用剪贴板重定向。 | - 0:启用剪贴板重定向<br>- 1:未启用剪贴板重定向 | x | x | x |
| disableconnectionsharing:i:value | 确定是否在远程桌面客户端重新连接到任何打开的现有连接或启动 RemoteApp 或桌面时启动新的连接 | - 0:重新连接到任何现有会话<br>- 1:启动新的连接 | 0 | x | x | x |
| disableprinterredirection:i:value | 此设置确定是否连接到远程计算机时启用轻松打印打印机重定向。 | - 0:启用了简单的打印打印机重定向<br>- 1:轻松打印打印机重定向已禁用 | x | x | x |
| domain:s:value | 此设置指定将用于使用远程桌面连接 (RDC) 登录到远程计算机上的用户帐户所在的域的名称。 此设置，以及用户名设置的值的值会在用户的名称文本框中 RDC 选项下的常规选项卡中显示。 | 一个有效的域名，例如"CONTOSO" | 没有默认值 | x | x | x |
| drivestoredirect:s:value | 确定客户端计算机上的本地磁盘驱动器将重定向并可在远程会话中。 | 未指定值： 不将任何驱动器重定向<br>* :重定向所有磁盘驱动器，包括更高版本连接的驱动器<br> DynamicDrives： 重定向更高版本连接任何驱动器<br>驱动器和一个或多个驱动器的标签： 将指定的驱动器重定向| 未指定值： 不将任何驱动器重定向 | x | x    | |
| enablecredsspsupport:i:value | 此设置确定 RDP 是否将使用凭据安全支持提供程序 (CredSSP) 身份验证是否可用。 | - 0:即使操作系统支持 CredSSP，RDP 不会使用 CredSSP，<br>- 1:如果操作系统支持 CredSSP，RDP 将使用 CredSSP | 1 | x | x | |
| 完整地址： s:value | 此设置指定的名称或你想要连接到远程计算机的 IP 地址 | 有效的计算机名、 IPv4 地址或 IPv6 地址。 | | x | x | x |
| gatewaycredentialssource:i:value | 指定或检索的 RD 网关身份验证方法。 | - 0:要求提供密码 (NTLM)<br>- 1:使用智能卡<br>- 4:允许用户选择更高版本 | 0 | x | x | x |
| gatewayhostname:s:value | 指定的 RD 网关主机名称。 | 有效的网关服务器地址。 ||x|x|x|
| gatewayprofileusagemethod:i:value | 指定是否使用默认 RD 网关设置 | - 0:使用由管理员指定的默认配置文件模式，<br>- 1:使用指定用户显式设置， | 0 | x | x | x |
| gatewayusagemethod:i:value | 指定何时使用 RD 网关服务器 | - 0:不使用 RD 网关服务器<br>- 1:始终使用 RD 网关服务器<br>- 2:如果无法建立直接连接到 RD 会话主机，使用的 RD 网关服务器<br>- 3:使用默认 RD 网关服务器设置<br>- 4:不使用 RD 网关，将跳过本地地址的服务器<br>设置此属性的值为 0 或 4 仍是实际上等同，但此属性设置为 4 启用绕过本地地址的选项。 | | x | x | x |
| networkautodetect:i:value | 确定使用自动网络带宽检测。 需要设置选项 bandwidthautodetect 和与连接类型 7。 | - 0:不使用自动网络带宽检测<br> - 1:使用自动网络带宽检测 | 1 | x ||x|
| promptcredentialonce:i:value | 确定是否保存并用于 RD 网关和远程计算机用户的凭据。|- 0:远程会话将不使用相同的凭据<br>- 1:远程会话将使用相同的凭据|1|x|x||
| redirectclipboard:i:value | 此设置确定是否在本地计算机上剪贴板将重定向并可在远程会话中。 此设置对应于 **剪贴板** RDC 中选项下的本地资源选项卡上的复选框。 | - 0:本地计算机上的剪贴板不是在远程会话中可用<br>- 1:本地计算机上的剪贴板是在远程会话中可用|1|x|x|x|
| redirectdrives:i:value | 此设置确定是否在客户端计算机上的驱动器会重定向和在远程会话中可用。 此设置对应于用驱动器的所选 **更多** RDC 中选项下的本地资源选项卡上。|- 0:本地计算机上的驱动器不是在远程会话中可用<br>- 1:本地计算机上的驱动器是在远程会话中可用|0|x|x| |
| redirectprinters:i:value | 此设置确定是否在客户端计算机上配置打印机会重定向，当您连接到远程计算机使用远程桌面连接在远程会话中可用。 此设置对应于中的选定 **打印机** RDC 中选项下的本地资源选项卡上的复选框。 | - 0:在本地计算机上的打印机不是在远程会话中可用<br>- 1:在本地计算机上的打印机是在远程会话中可用|1|x|x|x|
| redirectsmartcards:i:value | 此设置确定是否对客户端计算机上的智能卡设备会重定向，当您连接到远程计算机使用远程桌面连接 (RDC) 在远程会话中可用。 此设置对应于中的选定 **智能卡** RDC 中选项下的本地资源选项卡的详细信息下的复选框。|- 0:在本地计算机上的智能卡设备不是在远程会话中可用<br>- 1:在本地计算机上的智能卡设备是在远程会话中可用|1|x|x||
| remoteapplicationcmdline:s:value | RemoteApp 的可选命令行参数。||x|x|x|
| remoteapplicationexpandcmdline:i:value| 确定本地或远程方式是否应展开 RemoteApp 命令行参数中包含的环境变量。|- 0:应为本地计算机的值扩展环境变量<br>- 1:应为远程计算机的值在远程计算机上扩展的环境变量||x|x|x|
| remoteapplicationexpandworkingdir | 确定本地或远程方式是否应展开 RemoteApp 工作目录参数中包含的环境变量。 | - 0:应为本地计算机的值扩展环境变量<br> - 1:应为远程计算机的值在远程计算机上扩展环境变量。<br>通过命令行程序工作目录参数指定了 RemoteApp 工作目录。||x|x|x|
|remoteapplicationfile:s:value | 指定要通过 RemoteApp 远程计算机上打开的文件。<br>对于要打开的本地文件，还必须启用源驱动器的驱动器重定向。||x|x|x|
|remoteapplicationicon:s:value | 指定要启动 RemoteApp 时在客户端 UI 中显示的图标文件。 如果不指定任何文件名称，客户端将使用标准的远程桌面图标。 支持唯一".ico"文件。||x|x|x|
|remoteapplicationmode:i:value | 确定是否作为 RemoteApp 会话启动 RemoteApp 的连接。| - 0:不启动 RemoteApp 会话<br>- 1:启动 RemoteApp 会话|1|x|x|x|
|remoteapplicationname:s:value | 启动 RemoteApp 时，客户端界面中指定远程应用程序的名称。| 例如，"Excel 2016。"|x|x|x|
|remoteapplicationprogram:s:value | 指定与 RemoteApp 的别名或可执行文件名称。 | 例如，"EXCEL。" |x|x|x|
|屏幕模式 id: i: | 此设置确定使用远程桌面连接连接到远程计算机时，远程会话窗口是否显示全屏显示。 此设置对应于中显示配置滑块中 RDC 选项下的显示选项卡上的选项。|- 0:远程会话将在窗口中显示<br>- 1:远程会话将显示全屏|2|x|x|x|
|智能调整大小： i: | 此设置确定客户端计算机可以扩展以适应窗口大小的客户端计算机在远程计算机上的内容。|- 0:客户端窗口显示无法扩展重设大小时<br>- 1:重设大小时，将缩放客户端窗口中显示|0|x|x||
| 使用 multimon:i:value | 通过使用远程桌面连接连接到远程计算机时，此设置配置多监视器支持。|- 0:未启用多监视器支持<br>- 1:启用多监视器支持|0|x|x||
| username:s:value | 此设置指定将用于使用远程桌面连接登录到远程计算机上的用户帐户的名称。 此设置，以及域设置的值的值会在用户名框中 RDC 选项下的常规选项卡中显示。| 任何有效的用户名。 ||x|x|x|
| videoplaybackmode:i:value| 此设置确定是否远程桌面连接将使用 RDP 高效多媒体适用于流式处理视频播放。|- 0:不要使用 RDP 高效多媒体适用于流式处理视频播放<br>- 1:使用 RDP 高效多媒体适用于流式处理视频播放在可能的情况|1|x|x||
| workspaceid:s:value | 此设置定义 RemoteApp 和桌面 ID 与包含此设置的 RDP 文件相关联。 | 一个有效的 RemoteApp 和桌面连接 ID|x|x||
