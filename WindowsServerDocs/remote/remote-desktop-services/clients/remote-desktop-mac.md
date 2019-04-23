---
title: 开始使用 Mac 上的远程桌面
description: 了解如何设置 Mac 的远程桌面客户端
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 10/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: e8c5da1960d0e3129b5520e65c2d5ecf45eef778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886948"
---
# <a name="get-started-with-remote-desktop-on-mac"></a>开始使用 Mac 上的远程桌面

>适用于：Windows 10，Windows 8.1，Windows Server 2012 R2 和 Windows Server 2016

可以使用 Mac 的远程桌面客户端能够从 Mac 计算机使用 Windows 应用程序、 资源和台式计算机。 使用以下信息来开始的请查看[常见问题解答](remote-desktop-client-faq.md)如果有问题。

>[!Note]
> - 想了解 macOS 客户端的新版本？ 请查看[What's new for Mac 上的远程桌面？](mac-whatsnew.md)
> - Mac 客户端在运行 macOS 10.10 及更高版本的计算机上运行。
> - 在本文中的信息适用于 Mac 客户端的 Mac 应用商店中可用的版本的完整版本主要。 通过下载我们的预览版应用试用新功能：[测试版客户端发行说明](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409)。

## <a name="get-the-remote-desktop-client"></a>获取远程桌面客户端
请执行以下步骤，若要开始使用你的 Mac 上的远程桌面：

1. 下载中的 Microsoft 远程桌面客户端[Mac App Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12)。
2. [将您的 PC 设置为接受远程连接](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop)。 （如果您跳过此步骤中，您无法连接到您的电脑。）
3. 添加远程桌面连接或远程资源。 使用连接来直接连接到 Windows PC 和远程资源，若要使用的 RemoteApp 程序、 基于会话的桌面或虚拟桌面发布在本地使用 RemoteApp 和桌面连接。 此功能是通常在企业环境中可用。

## <a name="what-about-the-mac-beta-client"></a>Mac beta 客户端如何呢？
我们要测试新功能在 HockeyApp 上我们预览通道上。 想要将其签出？ 转到[适用于 Mac 的 Microsoft 远程桌面](https://go.microsoft.com/fwlink/?LinkID=619698)然后单击**下载**。 不需要创建帐户或登录到 HockeyApp 下载 beta 版客户端。

如果已有客户端，您可以检查更新，以确保拥有最新版本。 在 beta 版客户端，单击**Microsoft 远程桌面 Beta**顶部，并单击**检查更新**。 

## <a name="add-a-remote-desktop-connection"></a>添加远程桌面连接
若要创建的远程桌面连接：

1. 在连接中心中，单击**+**，然后单击**桌面**。
2. 输入以下信息：
   - **电脑名称**-计算机的名称。
      - 这可以是 Windows 计算机名称 (在中找到**系统**设置)，域名或 IP 地址。
      - 您还可以添加端口信息到末尾的此名称，如*MyDesktop:3389*。
   - **用户帐户**-添加用于访问远程 PC 的用户帐户。
      - 对于加入 Active Directory (AD) 的计算机或本地帐户，使用以下格式之一： *user_name*， *domain\user_name*，或*user_name@domain.com*。
      - 有关 Azure Active Directory (AAD) 加入的计算机，使用以下格式之一：*AzureAD\user_name*或*AzureAD\user_name@domain.com*。
      - 此外可以选择是否需要密码。
      - 在管理多个具有相同的用户名称的用户帐户时，设置一个友好名称以区分帐户。
      - 管理应用程序的首选项中保存的用户帐户。 

3. 此外可以设置连接的这些可选设置：
   - 设置的友好名称 
   - 添加网关
   - 将声音输出设置
   - 交换鼠标按钮
   - 启用管理员模式
   - 将本地文件夹重定向到远程会话
   - 正向本地打印机
   - 正向智能卡
4. 单击“保存” 。

若要启动连接，只需双击它。 同样适用于远程资源。

### <a name="export-and-import-connections"></a>导出和导入连接
您可以导出的远程桌面连接定义和在不同的设备上使用它。 在单独保存远程桌面。RDP 文件。

1. 在连接中心中，右键单击远程桌面。
2. 单击**导出**。
3. 浏览到要保存远程桌面的位置。RDP 文件。
4. 单击 **“确定”**。

使用以下步骤导入远程桌面。RDP 文件。

1. 在菜单栏中，单击**文件 > 导入**。
2. 浏览到。RDP 文件。
3. 单击“打开” 。

## <a name="add-a-remote-resource"></a>添加远程资源
远程资源是 RemoteApp 程序、 基于会话的桌面和使用 RemoteApp 和桌面连接发布的虚拟桌面。

- 将显示链接到 RD Web 访问服务器，您可以访问 RemoteApp 和桌面连接到该 URL。
- 列出已配置 RemoteApp 和桌面连接。

若要添加的远程资源：

1. 在连接中心中，单击**+**，然后单击**添加远程资源**。 
2. 输入远程资源的信息：
   - **源 URL** -RD Web 访问服务器的 URL。 这会告知客户端要搜索与你的电子邮件地址关联的 RD Web 访问服务器还可以在此字段 – 中输入你的公司电子邮件帐户。
   - **用户名称**-要用于连接到 RD Web 访问服务器的用户名。
   - **密码**-要用于连接到 RD Web 访问服务器的密码。
3. 单击“保存” 。


将在连接中心中显示的远程资源。


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>连接到 RD 网关访问内部资源

远程桌面网关 （RD 网关），可以从任何位置连接到公司网络上的远程计算机在 Internet 上。 可以创建和管理中的首选项或设置新的桌面连接时的应用程序网关。

若要设置首选项中的新网关：

1. 在连接中心中，单击**首选项 > 网关**。 
2. 单击**+** 输入以下信息的表底部的按钮：
  - **服务器名称**– 你想要用作网关的计算机的名称。 这可以是 Windows 计算机名称、 Internet 域名或 IP 地址。 此外可以添加端口信息的服务器名称 (例如：**RDGateway:443**或**10.0.0.1:443**)。
  - **用户名称**-用户名和密码以用于连接到的远程桌面网关。 您还可以选择**使用连接凭据**与用于远程桌面连接使用相同的用户名和密码。


## <a name="manage-your-user-accounts"></a>管理用户帐户

当连接到桌面或远程资源时，您可以保存要再次通过选择的用户帐户。 可以通过使用远程桌面客户端来管理你的用户帐户。

若要创建新的用户帐户：

1. 在连接中心中，单击**设置** > **帐户**。
2. 单击**将用户帐户添加**。
3. 输入以下信息：
   - **用户名称**-要保存以用于远程连接的用户的名称。 可以在任何以下格式输入用户名： 域 \ 用户名，user_name 或user_name@domain.com。
   - **密码**-您指定的用户的密码。 要用于保存要用于远程连接每个用户帐户需要具有与之关联的密码。
   - **友好名称**-如果你使用不同的密码，使用相同的用户帐户设置一个用于区分这些用户帐户的友好名称。
4. 点击**保存**，然后点击**设置**。

## <a name="customize-your-display-resolution"></a>自定义显示分辨率
可以为远程桌面会话指定的显示分辨率。

1. 在连接中心中，单击**首选项**。
2. 单击**解析**。 
3. 单击**+**。
4. 输入分辨率高度和宽度，并单击**确定。**

若要删除分辨率，请选择它，然后依次**-**。

**显示具有单独的空格**如果正在运行 Mac OS X 10.9 和禁用**显示包含单独的空格**Mavericks 中 (**系统首选项 > 的任务控制**)，您需要配置使用相同的选项的远程桌面客户端中的此设置。

### <a name="drive-redirection-for-remote-resources"></a>远程资源的驱动器重定向
驱动器重定向被支持远程资源，以便您可以保存使用本地到 mac 的远程应用程序创建的文件 重定向的文件夹始终是主目录在远程会话中显示为网络驱动器。

> [!NOTE]
> 若要使用此功能，管理员需要在服务器上设置适当的设置。


## <a name="use-a-keyboard-in-a-remote-session"></a>在远程会话中使用键盘

Mac 键盘布局不同于 Windows 键盘布局。 

- Mac 键盘上的命令键等于 Windows 键。
- 若要执行在 Mac 使用的命令按钮的操作，你将需要在 Windows 中使用的控制按钮 (例如：复制 = Ctrl + C)。
- 可以通过此外按下键在会话中激活功能键 (例如：FN + F1）。
- Mac 键盘上的空格键右侧的 Alt 键等于 Windows 中的 Alt Gr/右 Alt 键。

默认情况下，远程会话将作为操作系统运行客户端使用相同的键盘区域设置。 (如果你的 Mac 运行的英语-我们 OS 中，将用于远程会话。 如果未使用的 OS 键盘区域设置，检查在远程 PC 上设置和更改该设置键盘手动。 请参阅[远程桌面客户端常见问题](remote-desktop-client-faq.md)有关键盘和区域设置的详细信息。


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>支持远程桌面网关可插入身份验证和授权

Windows Server 2012 R2 引入了对新身份验证方法、 远程桌面网关可插入身份验证和授权，这为更大的灵活性的自定义身份验证例程提供支持。 你可以使用 Mac 客户端的现在此身份验证模式。 

> [!IMPORTANT]
> 不支持 Windows 8.1 之前的自定义身份验证和授权模型，尽管上述文章讨论了它们。

若要了解有关此功能的详细信息，请查看[ http://aka.ms/paa-sample ](http://aka.ms/paa-sample)。


> [!TIP]
> 问题和提出的意见始终是受欢迎的。 但是，请执行操作，无法发布的请求通过使用注释功能在本文末尾的故障排除帮助。 相反，请转到[远程桌面客户端论坛](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc)并启动新线程。 有功能建议？ 告诉我们[客户端 user voice 论坛](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android)。

