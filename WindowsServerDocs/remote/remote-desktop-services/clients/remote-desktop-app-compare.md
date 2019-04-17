---
title: 远程桌面的比较的客户端应用
description: 了解如何的不同远程桌面应用进行比较谈到受支持的特性和功能。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 12efe858-6b76-4e08-9f72-b9603aceb0fc
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/22/2018
ms.localizationpriority: medium
ms.openlocfilehash: 8a80c0330731e0874528c45c518401bc02803450
ms.sourcegitcommit: 22ed786067cf630e956aa60301a95bc7961b7660
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/18/2019
ms.locfileid: "9171221"
---
# 比较的客户端应用

>适用于：Windows 10、Windows 8.1、Windows Server 2012 R2、Windows Server 2016

我们通常会要求的不同远程桌面客户端应用如何相互进行比较。 它们都不要执行相同的操作？ 下面是这些问题的答案。

## 重定向支持

下表比较对设备和远程桌面连接的应用、 通用应用、 Android 应用、 iOS 应用、 macOS 应用和 web 客户端上的其他重定向的支持。 这些表介绍了可以一次在远程会话中访问的重定向。 

如果你远程到个人桌面，有其他可以在会话中**的其他设置**配置的重定向。 如果你的远程桌面或应用由组织管理，管理员可以启用或禁用通过组策略设置的重定向。

### 输入重定向

| 重定向 | 远程桌面<br> 左侧的“连接” | 通用 | Android | iOS | macOS | web 客户端 |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| 键盘    | X                             | X         | X       | X   | X     | X          |
| 鼠标       | X                             | X         | X       | X *    | X     | X          |
| 触控       | X                             | X         | X       | X   |       |            |
| Other       | 笔                           |           |         |     |       |            |
* 查看[受支持的输入设备，远程桌面 iOS beta 版客户端的列表](remote-desktop-ios.md#supported-input-devices)。

### 端口重定向   

| 重定向 | 远程桌面 <br>左侧的“连接” | 通用 | Android | iOS | macOS | Web 客户端 |
|-------------|-------------------------------|-----------|---------|-----|-------|------------|
| 串行端口 | X                             |           |         |     |       |            |
| USB         | X                             |           |         |     |       |            |

启用 USB 端口重定向时，在远程会话中自动识别到 USB 端口连接的任何 USB 设备。

### 其他重定向 （设备等）



| 重定向         | 远程桌面连接 | 通用   | Android | iOS         | macOS                                    | Web 客户端    |
|---------------------|---------------------------|-------------|---------|-------------|------------------------------------------|---------------|
| 相机             | X                         |             |         |             |                                          |               |
| 剪贴板           | X                         | 文本、 图像 | 文本    | 文本、 图像 | X                                        | 文本          |
| 本地驱动器/存储 | X                         |             | X       |             | x                                        |               |
| 位置            | X                         |             |         |             |                                          |               |
| 麦克风         | X                         |X            |         |             | X                                        |               |
| 打印机            | X                         |             |         |             | X （仅张）                            | PDF 打印     |
| 扫描仪            | X                         |             |         |             |                                          |               |
| 智能卡         | X                         |             |         |             | X （不受支持的 Windows 身份验证） |               |
| 扬声器            | X                         | X           | X       | X           | X                                        | X （除了 IE) |

* 对于打印机重定向-macOS 应用默认情况下支持发布者照排机打印机驱动程序。 它们不支持重定向本机打印机驱动程序。

### 受支持的 RDP 设置
此表包含可能与 Windows 和 HTML 客户端使用的受支持 RDP 文件设置的列表。 "X"中的平台列指示了设置受。 请注意，这不是受支持的 Windows 和 HTML5 的客户端的设置是详尽列表。 我们将继续更新此表包含受支持的更多 RDP 设置为 Windows 和 HTML5 客户端，以及 macOS、 iOS 和 Android 客户端。

| RDP 设置                        | 说明            | 值                 | 默认值          | Windows 虚拟桌面 | Windows | HTML5   |
|------------------------------------|------------------------|------------------------|:----------------------:|:-----------------------:|:-------:|:-------:|
| 备用完整地址： s:value | 指定备用名称或远程计算机的 IP 地址。 | 任何有效的名称或远程计算机，例如，"10.10.15.15"的 IP 地址 | | x | x | x |
| 备用 shell: s:value        | 确定是否计划自动启动时使用 RDP 连接。 此设置框对应的程序路径和文件名称程序选项卡上的远程桌面连接客户端。 若要指定备用 shell，输入的值，例如""C:\ProgramFiles\Office\word.exe""的可执行文件的有效的路径。 此设置还确定路径或要开始在连接时，如果 RemoteApplicationMode 处于启用状态的远程应用程序的别名。 | 例如，"C:\ProgramFiles\Office\word.exe" || x | x | x |
| audiocapturemode:i:value | 指示是否已启用音频输入/输出重定向。 此设置框对应的远程音频的远程桌面连接客户端。 | 从本地设备; (0) 禁用音频捕获（1） 启用从本地设备和重定向到音频应用程序在远程会话中的音频捕获 | 0 | x | x | |
| audiomode:i:value | 确定哪些计算机 （即，本地或远程） 播放音频。 | 在本地计算机 （在此计算机上的游戏）; (0) 播放声音（1） 远程计算机 （远程计算机上的游戏）; 上播放声音（2） 不播放的声音 （无法播放） | 0 | x | x | x |
| 身份验证级别： i:value | 定义的服务器身份验证级别设置。 | (0) 如果服务器身份验证失败，连接到没有警告计算机 (Connect 和不再提示);（1） 如果服务器身份验证失败，不会建立的连接 （未连接）;（2） 如果服务器身份验证失败，显示一条警告，并允许我连接或拒绝的连接 （警告我）;（指定 3） 没有身份验证要求。 | 3 | x | x ||
| 启用 autoreconnection: i:value | 此设置确定是否在客户端计算机将自动尝试重新连接到远程计算机，如果连接中断;例如，在没有网络连接中断时。 如果连接是放置的选项在远程桌面连接 (RDC) 下的体验选项卡上的复选框，此设置对应于重新连接。| (0) 的客户端计算机不会自动尝试重新连接;（1） 客户端计算机自动尝试重新连接| 1 | x | x | x |
| bandwidthautodetect:i:value | 确定是否已启用自动网络类型检测 | (0) 禁用自动网络类型检测;（1） 启用自动网络类型检测 | 1 | x | x | x |
| 压缩： i:value | 此设置确定到本地计算机，通过 RDP 传输时是否启用批量压缩。|(0) 禁用 RDP 批量压缩;（1） 启用 RDP 批量压缩 | 1 | x | x | x |
| 桌面大小 id: i:value | 指定从一组预定义的选项在远程会话桌面的尺寸。 如果指定 desktopheight 或 desktopwidth 重写此设置。| (0) 640 x 480;（1) 800 x 600;（2) 1024 x 768;（3) 1280 x 1024;（4) 1600 x 1200 | 0 | x | x | x |
| desktopheight:i:value | 通过使用远程桌面连接 (RDC) 连接时，此设置确定远程计算机上的分辨率高度 （以像素为单位）。 此设置对应于 RDC 中的选项卡 underOptions 上再次显示 configurationslider 中的选择。 | 200 到 2048年数值 | 默认值设置为本地计算机上的分辨率 | x | x | x |
| desktopwidth:i:value | 通过使用远程桌面连接 (RDC) 连接时，此设置确定远程计算机上的分辨率宽度 （以像素为单位）。 此设置对应于 RDC 中的选项卡 underOptions 上再次显示 configurationslider 中的选择。 | 200 4096 数值 | 默认值设置为本地计算机上的分辨率 | x | x | x |
| disableclipboardredirection:i:value | 此设置确定是否连接到远程计算机时启用剪贴板重定向。 | 已启用 (0) 剪贴板重定向;（未启用 1） 剪贴板重定向 | x | x | x |
| disableconnectionsharing:i:value | 确定是否远程桌面客户端重新连接到任何现有的开放连接或启动远程应用程序或桌面时启动一个新的连接 | (0) 重新连接到任何现有会话;（1） 初始化新连接 | 0 | x | x | x |
| disableprinterredirection:i:value | 此设置确定是否连接到远程计算机时启用轻松打印打印机重定向。 | 已启用 (0) 轻松打印打印机重定向;（禁用 1） 轻松打印打印机重定向 | x | x | x |
| 域： s:value | 此设置指定将用于使用远程桌面连接 (RDC) 登录到远程计算机上的用户帐户所在的域的名称。 此设置，以及用户名设置的值的值将出现在 theGeneraltab underOptionsin RDC 认证 nametext 框中。 | 有效的域名，例如"CONTOSO" | 没有默认值 | x | x | x |
| drivestoredirect:s:value | 确定客户端计算机上的本地磁盘驱动器将重定向和远程会话中可用。 | 没有指定的值执行操作重定向任何驱动器;*-将所有磁盘驱动器，包括更高版本; 连接的驱动器重都定向DynamicDrives-重定向的更高版本; 连接任何驱动器该驱动器，然后为一个或多个驱动器的标签重定向的指定的驱动器。| 没有指定的值不重定向任何驱动器 | x | x    | |
| enablecredsspsupport:i:value | 此设置确定是否 RDP 将使用凭据安全支持提供程序 (CredSSP) 进行身份验证是否可用。 | (0) RDP 将使用 CredSSP，即使操作系统支持 CredSSP;（如果操作系统支持 CredSSP，1) RDP 将使用 CredSSP | 1 | x | x | |
| 完整的地址： s:value | 此设置指定的名称或想要连接到远程计算机的 IP 地址 | 有效的计算机名称、 IPv4 地址或 IPv6 地址-指定你想要连接的远程计算机。 | | x | x | x |
| gatewaycredentialssource:i:value | 指定或检索 RD 网关身份验证方法。 | (0) 询问密码 (NTLM);（1） 使用智能卡;（4） 允许用户选择更高版本 | 0 | x | x | x |
| gatewayhostname:s:value | 指定 RD 网关主机名。 | 有效的网关服务器地址。 ||x|x|x|
| gatewayprofileusagemethod:i:value | 指定是否使用默认 RD 网关设置 | (0) 使用的默认配置文件模式，所指定的管理员;（1） 所指定的用户使用显式设置。 | 0 | x | x | x |
| gatewayusagemethod:i:value | 指定何时使用 RD 网关服务器 | (0) 不使用 RD 网关服务器;（1） 始终使用 RD 网关服务器;（2） 如果无法进行直接连接到 RD 会话主机; 使用 RD 网关服务器（3） 使用默认 RD 网关服务器设置;（4） 不使用 RD 网关，跳过服务器用于本地地址;设置此属性值为 0 或 4 等效有效地，但此属性设置为 4 中启用了选项来绕过本地地址。 | | x | x | x |
| networkautodetect:i:value | 确定是否使用或者不使用自动网络带宽检测。 需要设置 optionbandwidthautodetectto 和关联 withconnection type7。 | (0) 不使用自动网络带宽检测;（1） 使用自动网络带宽检测 | 1 | x ||x|
| promptcredentialonce:i:value | 确定用户的凭据是否是保存并用于 RD 网关和远程计算机。|(0) 的远程会话不会使用相同的凭据;（1） 远程会话将使用相同的凭据|1|x|x||
| redirectclipboard:i:value | 此设置确定是否在本地计算机上的剪贴板将重定向和远程会话中可用。 此设置对应于上 theLocal Resourcestab underOptionsin RDC theClipboardcheck 复选框。 | 本地计算机上的 (0) 剪贴板不可在远程会话中;（1） 剪贴板本地计算机上位于远程会话|1|x|x|x|
| redirectdrives:i:value | 此设置确定是否在客户端计算机上的驱动器将重定向和远程会话中可用。 此设置对应于选择 forDrivesunderMoreon theLocal Resourcestab underOptionsin RDC。|(0) 本地计算机上的驱动器中不可用远程会话中;（本地计算机上 1） 驱动器是在远程会话中可用|0|x|x| |
| redirectprinters:i:value | 此设置确定是否在客户端计算机上配置的打印机会重定向和当你连接到远程计算机使用远程桌面连接 (RDC) 远程会话中可用。 此设置对应于上 theLocal Resourcestab underOptionsin RDC thePrinterscheck 框中的选项。 | (0) 在本地计算机上的打印机不可用在远程会话中;（本地计算机上 1） 的打印机位于远程会话|1|x|x|x|
| redirectsmartcards:i:value | 此设置确定是否在客户端计算机上的智能卡设备将重定向和当你连接到远程计算机使用远程桌面连接 (RDC) 在远程会话中可用。 此设置对应于所选内容中 theSmart cardscheck 框上 theLocal Resourcestab underOptionsin RDC underMore。|(0) 本地计算机上的智能卡设备不可用在远程会话中;（本地计算机上 1） 的智能卡设备是在远程会话中可用|1|x|x||
| remoteapplicationcmdline:s:value | RemoteApp 的可选命令行参数。||x|x|x|
| remoteapplicationexpandcmdline:i:value| 确定是否应在本地或远程展开 RemoteApp 命令行参数中包含的环境变量。|(0) 环境变量应展开本地计算机，则的值为（1） 环境变量应扩展到远程计算机的值在远程计算机上||x|x|x|
| remoteapplicationexpandworkingdir | 确定是否应在本地或远程展开 RemoteApp 工作目录参数中包含的环境变量。 | (0) 环境变量应展开本地计算机，则的值为（1） 环境变量应扩展到远程计算机的值在远程计算机上。 注意： 通过 shell 工作目录参数指定 RemoteApp 工作目录。||x|x|x|
|remoteapplicationfile:s:value | 指定要通过 RemoteApp 在远程计算机上打开的文件。 注意： 对于要打开的本地文件，你还必须启用源驱动器的驱动器重的定向。||x|x|x|
|remoteapplicationicon:s:value | 指定要在启动 RemoteApp 时在客户端 UI 中显示的图标文件。 如果未不指定任何文件名，客户端将使用标准的远程桌面图标。 仅".ico"文件才受支持。||x|x|x|
|remoteapplicationmode:i:value | 确定是否为一个 RemoteApp 会话启动 RemoteApp 连接。| (0) 不启动 RemoteApp 会话;（1） 启动 RemoteApp 会话|1|x|x|x|
|remoteapplicationname:s:value | 启动远程应用程序时，在客户端界面指定 RemoteApp 的名称。| 例如，"Excel 2016"|x|x|x|
|remoteapplicationprogram:s:value | 指定 RemoteApp 的别名或可执行文件名称。 | 例如，"EXCEL" |x|x|x|
|屏幕模式 id: i:value | 此设置确定是否在远程会话窗口全屏显示时通过远程桌面连接 (RDC) 连接到远程计算机。 此设置对应于卡 underOptionsin RDC 上再次显示 configurationslider 中的选项。|（1） 远程会话将显示在窗口。（2） 远程会话将显示全屏模式|2|x|x|x|
|智能大小调整： i:value | 此设置确定在客户端计算机可以扩展内容，以适应窗口大小的客户端计算机在远程计算机上。|(0) 的客户端窗口中显示不进行缩放调整大小;（调整大小进行缩放 1） 的客户端窗口中显示|0|x|x||
| 使用 multimon:i:value | 通过使用远程桌面连接 (RDC) 连接到远程计算机时，此设置配置多监视器支持。|(0) 不启用多监视器支持;（1） 启用多监视器支持|0|x|x||
| username:s:value | 此设置指定将用于使用远程桌面连接 (RDC) 登录到远程计算机上的用户帐户的名称。 此设置，以及域设置值的值将显示在用户 namebox theGeneraltab underOptionsin RDC 上。| 任何有效的用户名。 ||x|x|x|
| videoplaybackmode:i:value| 此设置确定将 RDP 高效多媒体流式传输视频播放使用远程桌面连接 (RDC)。|(0) 不使用 RDP 高效多媒体流式传输视频播放;（1） 使用 RDP 高效多媒体流式传输视频播放时可能|1|x|x||
| workspaceid:s:value | 此设置可定义 RemoteApp 和桌面 ID 与包含此设置的 RDP 文件相关联。 | 有效的远程应用程序和桌面连接 ID|x|x||