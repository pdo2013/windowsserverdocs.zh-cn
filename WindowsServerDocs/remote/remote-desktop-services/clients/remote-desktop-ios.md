---
title: 在 iOS 上开始使用远程桌面
description: 了解如何设置适用于 iOS 的远程桌面客户端
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
ms.openlocfilehash: 1a1939c7fd6d25d756369c85e4adaa6c15195b37
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889628"
---
# <a name="get-started-with-remote-desktop-on-ios"></a>在 iOS 上开始使用远程桌面

>适用于：Windows 10，Windows 8.1，Windows Server 2012 R2 和 Windows Server 2016

可以使用适用于 iOS 的远程桌面客户端以使用 Windows 应用程序、 资源和台式计算机从 iOS 设备 （Iphone 和 Ipad）。

使用以下信息来开始。 请务必查看[常见问题解答](remote-desktop-client-faq.md)如果有任何问题。

> [!NOTE]
> - IOS 客户端的新版本感到好奇？ 请查看[什么是用于远程桌面在 iOS 上的新功能？](ios-whatsnew.md)
> - IOS 客户端支持运行 iOS 的设备 6.x 和更高版本。

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>获取远程桌面客户端并开始使用它

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>从 iOS 应用商店下载远程桌面客户端
请按照下列步骤在 iOS 设备上开始使用远程桌面：

1. 下载中的 Microsoft 远程桌面客户端[iTunes](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8)。
2. [将您的 PC 设置为接受远程连接](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop)。
3. 添加[远程桌面连接](#add-a-remote-desktop-connection)或[远程资源](#add-a-remote-resource)。 使用连接来连接到直接使用的 RemoteApp 程序的 Windows PC 和远程资源，基于会话的桌面或虚拟机发布到在本地使用 RemoteApp 和桌面连接。 此功能是通常在企业环境中可用。

### <a name="download-the-remote-desktop-ios-beta-client"></a>下载远程桌面 iOS Beta 客户端
在 iOS 设备中，请按照[这些说明](https://aka.ms/rdiosbeta)下载远程桌面 iOS Beta 客户端。

### <a name="add-a-remote-desktop-connection"></a>添加远程桌面连接

若要创建的远程桌面连接： 
1. 在连接中心点击**+**，然后点击**添加 PC 或服务器**。
2. 输入远程桌面连接的以下信息：
  - **电脑名称**– 计算机的名称。 这可以是 Windows 计算机名称、 Internet 域名或 IP 地址。 您还可以向 PC 名称追加端口信息 (例如， **MyDesktop:3389**或**10.0.0.1:3389**)。
  - **用户名称**– 要访问远程 PC 使用的用户名。 您可以使用以下格式： *user_name*， *domain\user_name*，或*user_name@domain.com*。 此外可以指定是否提示输入用户名和密码。
3. 此外可以设置以下附加选项：
  - **友好名称 （可选）** – 适用于要连接到 PC 易于记忆的名称。 可以使用任意字符串，但如果不指定友好名称，PC 名称将显示。
  - **网关 （可选）** – 你想要用于连接到虚拟机、 RemoteApp 程序和内部企业网络上的基于会话的桌面的远程桌面网关。 从您的系统管理员获取有关网关的信息。
  - **声音**– 选择要在远程会话期间用于音频设备。 您可以选择播放声音或完全不响应本地设备，远程设备上。
  - **交换鼠标按钮**– 每当鼠标手势将发送一条命令使用鼠标左键，而是发送相同的命令，用鼠标右键。 这是必需的如果远程 PC 配置为使用左手鼠标模式。
  - **管理员模式下**-连接到运行 Windows Server 2003 或更高版本的服务器上的管理会话。
4. 点击**保存**。

需要编辑这些设置？ 按住你想要编辑的桌面，然后点击设置图标。 

### <a name="add-a-remote-resource"></a>添加远程资源
远程资源是 RemoteApp 程序、 基于会话的桌面和使用 RemoteApp 和桌面连接发布的虚拟桌面。

- 将显示链接到 RD Web 访问服务器，您可以访问 RemoteApp 和桌面连接到该 URL。
- 列出已配置 RemoteApp 和桌面连接。

若要添加的远程资源：

1. 在连接中心屏幕上，点击**+**，然后点击**添加远程资源**。 
2. 输入远程资源的信息：
   - **源 URL** -RD Web 访问服务器的 URL。 这会告知客户端要搜索与你的电子邮件地址关联的 RD Web 访问服务器还可以在此字段 – 中输入你的公司电子邮件帐户。
   - **用户名称**-要用于连接到 RD Web 访问服务器的用户名。
   - **密码**-要用于连接到 RD Web 访问服务器的密码。
3. 点击**保存**。

将在连接中心中显示的远程资源。


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>连接到 RD 网关访问内部资源

远程桌面网关 （RD 网关），可以从任何位置连接到公司网络上的远程计算机在 Internet 上。 可以创建和管理网关使用远程桌面客户端。

若要设置新的网关：

1. 在连接中心，点击**设置 > 网关**。 
2. 点击**添加远程桌面网关**。
3. 输入以下信息：
  - **服务器名称**– 你想要用作网关的计算机的名称。 这可以是 Windows 计算机名称、 Internet 域名或 IP 地址。 此外可以添加端口信息的服务器名称 (例如：**RDGateway:443**或**10.0.0.1:443**)。
  - **用户名称**-用户名和密码以用于连接到的远程桌面网关。 您还可以选择**使用连接凭据**与用于远程桌面连接使用相同的用户名和密码。


## <a name="manage-your-user-accounts"></a>管理用户帐户 

当连接到桌面或远程资源时，您可以保存要再次通过选择的用户帐户。 可以通过使用远程桌面客户端来管理你的用户帐户。

若要创建新的用户帐户：

1. 在连接中心，点击**设置**，然后点击**用户名**。
2. 点击**将用户帐户添加**。
3. 输入以下信息：
   - **用户名称**-要保存以用于远程连接的用户的名称。 可以在任何以下格式输入用户名： 域 \ 用户名，user_name 或user_name@domain.com。
   - **密码**-您指定的用户的密码。 要用于保存要用于远程连接每个用户帐户需要具有与之关联的密码。
4. 点击**保存**，然后点击**设置**。
5. 点击**完成**以保存新配置。

若要删除的用户帐户：

1. 在连接中心，点击**设置 > 用户名**。
2. 轻扫从右到左，若要选择的用户的行。
3. 点击**删除**。



## <a name="navigate-the-remote-desktop-session"></a>导航远程桌面会话
启动时的远程桌面会话，有工具可用于导航会话。

### <a name="start-a-remote-desktop-connection"></a>启动远程桌面连接

1. 点击要启动的远程桌面会话的远程桌面连接。 
2. 如果您要求验证远程桌面的证书，请点击**接受**。 您可以选择始终接受通过滑动**不要再询问我是否连接到此计算机**切换到**ON**。 

### <a name="connection-bar"></a>连接栏

连接栏可以访问其他导航控件。 

- **平移控件**:平移控件启用屏幕放大和移动。 请注意平移控件才可以使用直接触摸。
   - 启用/禁用平移控件：点击连接栏显示平移控件和缩放屏幕中的平铺图标。 点击连接栏，再次以隐藏控件并返回到其原始分辨率的屏幕中平铺图标。
   - 使用平移控件：点击按住平移控件，然后你想要移动屏幕的方向拖动。
   - 移动平移控件：双点击并按住移动屏幕上的控件的平移控件。
- **连接名称**:显示当前的连接名称。 点击要显示会话选项栏的连接名称。
- **键盘**:点击键盘图标以显示或隐藏键盘。 显示键盘时，将自动显示平移控件。
- **移动连接栏**:点击并按住连接栏，然后拖放到屏幕顶部的新位置。

### <a name="session-selection"></a>会话所选内容
您可以同时打开到不同的电脑的多个连接。 点击连接栏，以在屏幕的左侧显示会话选项栏。 会话选项栏，可查看打开的连接并在它们之间切换。 

- 在会话中打开的远程资源的应用之间进行切换。

    当连接到远程资源时，您可以通过点击扩展器菜单上，从可用的项目列表中选择该会话中打开的应用程序之间进行切换。
- 启动新的会话

  当前连接中，可以启动新的应用程序或桌面会话从： 点击**新启动**，然后选择从可用的项目列表。

- 断开连接的会话

  若要断开连接会话点击 X 会话磁贴的左侧。

### <a name="command-bar"></a>命令栏

在命令栏替换为从版本 8.0.1 栏实用程序。 可以在鼠标模式之间切换，并从命令栏返回到连接中心。

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>在远程会话中使用触控笔势和鼠标模式

客户端使用标准的触摸手势。 触摸手势还可用于复制在远程桌面上的鼠标操作。 下表中定义可用的鼠标模式。

> [!NOTE]
> 交互与 Windows 8 或更高版本中直接触摸模式支持本机触摸手势。 有关在 Windows 8 上的详细信息请参阅手势[触摸：轻扫，点击，及更高版本](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond)。

| 鼠标模式    | 鼠标操作      | 手势                                                    |
|---------------|----------------------|------------------------------------------------------------|
| 直接触摸  | 单击鼠标左键           | 1 指点击                                               |
| 直接触摸  | 右键单击          | 1 指点击并按住                                      |
| 鼠标指针 | 单击鼠标左键           | 1 指点击                                               |
| 鼠标指针 | 左键单击并拖动  | 1 手指双点击并按住，然后拖动                    |
| 鼠标指针 | 右键单击          | 2 个手指点击                                               |
| 鼠标指针 | 右键单击并拖动 | 2 个手指双点击并按住，然后拖动                    |
| 鼠标指针 | 鼠标滚轮          | 2 个手指点击并按住，然后向上或向下拖动                |
| 鼠标指针 | Zoom                 | 捏合 2 根手指放大或分布 2 个手指，以便缩小 |

## <a name="supported-input-devices"></a>支持的输入的设备

[远程桌面 iOS beta 客户端](https://aka.ms/rdiosbeta)支持 Swiftpoint GT 和 ProPoint 物理鼠标。 提供 Swiftpoint[专享的折扣](https://www.swiftpoint.com/microsoft/)iOS beta 客户端用户 GT 上。

IOS 客户端目前仅支持 Swiftpoint 鼠标。 请参阅[什么是 iOS 客户端中的新增功能](ios-whatsnew.md)页和[iOS 应用商店](https://aka.ms/rdios)新闻有关的其他设备在将来的支持。

## <a name="use-a-keyboard-in-a-remote-session"></a>在远程会话中使用键盘

你可以使用屏幕键盘或实际键盘在远程会话中的。

为屏幕键盘，使用上的按钮在键盘上方的栏的右边缘来切换标准和其他键盘。

如果为你的 iOS 设备启用了蓝牙，客户端会自动检测蓝牙键盘。

请注意，由于在 OS 上的限制，如 Ctrl、 选项和函数的特殊键将无法按预期工作与蓝牙键盘。 以下密钥的工作原理：

- 字母数字键
- 光标键
- 选项卡上：选项卡上的工作原理，但按 Shift + Tab 不起作用
- 主页 / Pos1:Alt + 向左键 = 主页
- 结束时间：Alt + 向右键 = 结束
- 向上翻页：Alt + 向上键 = 向上翻页
- 向下翻页：Alt + 向下 = 向下翻页
- 选择所有：命令 + A = Ctrl + A （在大多数程序中的全选）
- 剪切：命令 + X = Ctrl + X （在大多数程序中剪切）
- 复制：命令 + C = Ctrl + C （在大多数程序中的复制）
- 粘贴：命令 + V = Ctrl + V （粘贴在大多数程序中）
- 符号：Alt + 字母数字键将生成不同的符号，具体取决于配置的语言

> [!TIP]
> 问题和提出的意见始终是受欢迎的。 但是，请执行操作，无法发布的请求通过使用注释功能在本文末尾的故障排除帮助。 相反，请转到[远程桌面客户端论坛](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc)并启动新线程。 有功能建议？ 告诉我们[客户端 user voice 论坛](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android)。

