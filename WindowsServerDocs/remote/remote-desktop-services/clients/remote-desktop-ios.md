---
title: 在 iOS 上开始使用远程桌面
description: 了解如何为 iOS 设置远程桌面客户端
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: lizap
manager: dongill
ms.author: elizapo
date: 01/13/2017
ms.localizationpriority: medium
ms.openlocfilehash: 71fe969de4d21f7fa3c134b0f80fc7f69e5b2da8
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446703"
---
# <a name="get-started-with-remote-desktop-on-ios"></a>在 iOS 上开始使用远程桌面

>适用于：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

可以使用适用于 iOS 的远程桌面客户端从 iOS 设备（iPhone 和 iPad）处理 Windows 应用、资源和桌面。

请参考以下信息开始。 如果有任何问题，请查看[常见问题解答](remote-desktop-client-faq.md)。

> [!NOTE]
> - 想知道 iOS 客户端的新版本吗？ 请查看 [iOS 上的远程桌面的新增功能](ios-whatsnew.md)
> - iOS 客户端支持运行 iOS 6.x 及更高版本的设备。

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>获取远程桌面客户端并开始使用

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>从 iOS 应用商店下载远程桌面客户端
按照以下步骤开始在 iOS 设备上使用远程桌面：

1. 从 [iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8) 下载 Microsoft 远程桌面客户端。
2. [设置电脑接受远程连接](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop)。
3. 添加[远程桌面连接](#add-a-remote-desktop-connection)或[远程资源](#add-a-remote-resource)。 可以使用连接直接连接到 Windows 电脑和远程资源，以使用 RemoteApp 程序、基于会话的桌面或是使用 RemoteApp 和桌面连接在本地发布的虚拟桌面。 此功能通常在企业环境中可用。

### <a name="download-the-remote-desktop-ios-beta-client"></a>下载远程桌面 iOS Beta 客户端
在 iOS 设备上，按照[这些说明](https://aka.ms/rdiosbeta)下载远程桌面 iOS Beta 客户端。

### <a name="add-a-remote-desktop-connection"></a>添加远程桌面连接

若要创建远程桌面连接： 
1. 在“连接中心”点击 +  ，然后点击“添加电脑或服务器”  。
2. 输入远程桌面连接的以下信息：
   - **电脑名称** - 计算机的名称。 可以是 Windows 计算机名、Internet 域名或 IP 地址。 还可以向电脑名称追加端口信息（例如，MyDesktop:3389  或 10.0.0.1:3389  ）。
   - **用户名** – 访问远程电脑要使用的用户名。 可以使用以下格式：user_name  、domain\user_name  或 <em>user_name@domain.com</em>。 此外可以指定是否提示输入用户名和密码。
3. 还可以设置以下附加选项：
   - **易记名称（可选）** – 连接到电脑所需的易记忆的名称。 可以使用任意字符串，但如果不指定易记名称，将显示电脑名称。
   - **网关（可选）** – 希望用于连接到 Internet 企业内部网络中的虚拟桌面、RemoteApp 程序和基于会话的桌面的远程桌面网关。 从系统管理员那里获取有关网关的信息。
   - **声音** – 选择要在远程会话期间用于音频的设备。 可以选择在本地设备、远程设备上播放声音或完全不播放。
   - **交换鼠标按钮** – 每当鼠标手势通过鼠标左键发送一条命令时，就会通过鼠标右键发送相同的命令。 这在远程电脑配置为左手鼠标模式时是必需的。
   - **管理模式** - 在运行 Windows Server 2003 或更高版本的服务器上连接到管理会话。
4. 点击“保存”  。

需要编辑这些设置？ 按住你想要编辑的桌面，然后点击设置图标。 

### <a name="add-a-remote-resource"></a>添加远程资源
远程资源包括 RemoteApp 程序、基于会话的桌面和使用 RemoteApp 和桌面连接发布的虚拟桌面。

- URL 会显示指向使你可以访问 RemoteApp 和桌面连接的 RD Web 访问服务器的链接。
- 配置的 RemoteApp 和桌面连接会列出。

若要添加远程资源：

1. 在“连接中心”屏幕上，点击 +  ，然后点击“添加远程资源”  。 
2. 输入远程资源信息：
   - **源 URL** - RD Web 访问服务器的 URL。 也可以在此字段输入公司电子邮件帐户 - 这将告知客户端搜索与电子邮件地址关联的 RD Web 访问服务器。
   - **用户名称** - 要用于连接到的 RD Web 访问服务器的用户名。
   - **密码** - 要用于连接到的 RD Web 访问服务器的密码。
3. 点击“保存”  。

将在“连接中心”显示远程资源。


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>连接到 RD 网关以访问内部资产

远程桌面网关（RD 网关）允许你从 Internet 上的任何位置连接到企业网络上的远程计算机。 可以使用远程桌面客户端创建和管理网关。

若要设置新网关：

1. 在“连接中心”，点击“设置”>“网关”  。 
2. 点击“添加远程桌面网关”  。
3. 输入以下信息：
   - **服务器名称** – 要用作网关的计算机的名称。 可以是 Windows 计算机名、Internet 域名或 IP 地址。 此外可以向服务器名称添加端口信息（例如：RDGateway:443  或 10.0.0.1:443  ）。
   - **用户名** - 要用于连接到的远程桌面网关的用户名和密码。 还可以选择“使用连接凭据”  以使用与用于远程桌面连接的凭据相同的用户名和密码。


## <a name="manage-your-user-accounts"></a>管理用户帐户 

当连接到桌面或远程资源时，可以保存用户帐户以便再次选择。 可以使用远程桌面客户端管理用户帐户。

若要创建新的用户帐户：

1. 在“连接中心”，点击“设置”  ，然后点击“用户名”  。
2. 点击“添加用户帐户”  。
3. 输入以下信息：
   - **用户名** - 要保存以用于远程连接的用户名。 可以使用以下任意格式输入用户名：user_name、domain\user_name 或 user_name@domain.com。
   - **密码** - 指定的用户密码。 要保存用于远程连接的每个用户帐户都需要与之关联的密码。
4. 点击“保存”  ，然后点击“设置”  。
5. 点击“完成”  以保存新配置。

若要删除用户帐户：

1. 在“连接中心”，点击“设置”>“用户名”  。
2. 从右到左轻扫行以选择用户。
3. 点击“删除”  。



## <a name="navigate-the-remote-desktop-session"></a>导航远程桌面会话
启动远程桌面会话时，可使用工具来导航会话。

### <a name="start-a-remote-desktop-connection"></a>启动远程桌面连接

1. 点击远程桌面连接以启动远程桌面会话。 
2. 如果要求验证远程桌面证书，请点击“接受”  。 可以选择通过将“不再询问是否连接到此计算机”  开关切换为“开”  来始终接受证书。 

### <a name="connection-bar"></a>连接栏

可通过连接栏访问其他导航控件。 

- **平移控件**：平移控件使屏幕能够放大和移动。 请注意，使用直接触摸才能使用平移控件。
   - 启用/禁用平移控件：点击连接栏中的平移图标显示平移控件和缩放屏幕。 再次点击连接栏中的平移图标来隐藏控件，并使屏幕返回到原始分辨率。
   - 使用平移控件：点击并按住平移控件，然后按你想要移动屏幕的方向拖动。
   - 移动平移控件：双击并按住平移控件来移动屏幕上的控件。
- **连接名称**：显示当前连接名称。 点击要显示会话选项栏的连接名称。
- **键盘**：点击键盘图标以显示或隐藏键盘。 显示键盘时，将自动显示平移控件。
- **移动连接栏**：点击并按住连接栏，然后拖放到屏幕顶部的新位置。

### <a name="session-selection"></a>会话选择
可以同时在不同的电脑上打开多个连接。 点击连接栏以在屏幕左侧显示会话选择栏。 通过会话选择栏可以查看打开的连接并在它们之间切换。 

- 在打开的远程资源会话中的应用之间切换。

    连接到远程资源时，可以通过点击扩展器菜单并从可用项列表中选择来在该会话中打开的应用程序之间切换。
- 启动新会话

  可以从当前连接中启动新应用程序或桌面会话：点击“启动新会话”  ，然后选择从可用项列表中选择。

- 中断会话的连接

  若要中断会话的连接，请点击会话磁贴左侧的 X。

### <a name="command-bar"></a>命令栏

命令栏替换了版本 8.0.1 中启动的实用程序栏。 可以在鼠标模式之间切换，并从命令栏返回到连接中心。

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>在远程会话中使用触摸手势和鼠标模式

客户端使用标准触摸手势。 还可以使用触摸手势在远程桌面上复制鼠标操作。 下表定义了可用鼠标模式。

> [!NOTE]
> 与 Windows 8 或更高版本交互时，支持直接触摸模式下的本机触摸手势。 有关 Windows 8 手势的详细信息，请参阅[触摸：轻扫、点击等](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond)。

| 鼠标模式    | 鼠标操作      | 手势                                                    |
|---------------|----------------------|------------------------------------------------------------|
| 直接触摸  | 左键单击           | 1 指点击                                               |
| 直接触摸  | 右键单击          | 1 指点击并按住                                      |
| 鼠标指针 | 左键单击           | 1 指点击                                               |
| 鼠标指针 | 左键单击并拖动  | 1 指双击并按住，然后拖动                    |
| 鼠标指针 | 右键单击          | 2 指点击                                               |
| 鼠标指针 | 右键单击并拖动 | 2 指双击并按住，然后拖动                    |
| 鼠标指针 | 鼠标滚轮          | 2 指点击并按住，然后上下拖动                |
| 鼠标指针 | Zoom                 | 捏合 2 个手指进行放大或张开 2 个手指进行缩小 |

## <a name="supported-input-devices"></a>支持的输入设备

[远程桌面 iOS beta 客户端](https://aka.ms/rdiosbeta)支持 Swiftpoint GT 和 ProPoint 物理鼠标。 Swiftpoint 为 iOS beta 客户端用户提供 GT 的[专享折扣](https://www.swiftpoint.com/microsoft/)。

iOS 客户端当前仅支持 Swiftpoint 鼠标。 有关将来支持其他设备的新闻，请参阅 [iOS 客户端的新增功能](ios-whatsnew.md)页和 [iOS 应用商店](https://aka.ms/rdios)。

## <a name="use-a-keyboard-in-a-remote-session"></a>在远程会话中使用键盘

可以在远程会话中使用屏幕键盘或物理键盘。

对于屏幕键盘，使用键盘上方的栏右边缘的按钮来在标准和其他键盘之间切换。

如果为你的 iOS 设备启用了蓝牙，客户端会自动检测蓝牙键盘。

请注意，由于 OS 上的限制，Ctrl、选项和函数等特殊键将无法按预期与蓝牙键盘结合使用。 以下键起作用：

- 字母数字键
- 光标键
- Tab：Tab 起作用，但 Shift+Tab 不起作用
- Home/Pos1：Alt+向左键 = Home
- End：Alt+向右键 = End
- Page Up：Alt+向上键 = Page Up
- Page Down：Alt+向下键 = Page Down
- 全选：Command+A = Ctrl+A（在大多数程序中全选）
- 剪切：Command+X = Ctrl+X（在大多数程序中剪切）
- 复制：Command+C = Ctrl+C（在大多数程序中复制）
- 粘贴：Command+V = Ctrl+V（在大多数程序中粘贴）
- 符号：Alt+字母数字键将生成不同的符号，具体取决于配置的语言

> [!TIP]
> 欢迎提出问题和意见。 但是，请不要使用本文末尾的评论功能来请求获取故障排除帮助。 而是转到[远程桌面客户端论坛](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc)并启动一个新线程。 有功能建议？ 请在[客户端用户心声论坛](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android)告诉我们。

