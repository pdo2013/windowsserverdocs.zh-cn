---
title: 远程桌面客户端常见问题
description: 有关远程桌面客户端常见问题
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 785a18cf-a5d0-4bc2-95e4-9ef53ee8f65a
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/16/2018
ms.localizationpriority: medium
ms.openlocfilehash: ec1b0a17c578f2d8ac55d1704af6b267b6bb8e5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865928"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>有关远程桌面客户端常见问题

>适用于：Windows 10，Windows 8.1，Windows Server 2012 R2 和 Windows Server 2016

现在，已在你的设备 （Android、 Mac、 iOS 或 Windows） 上设置远程桌面客户端，可能有问题。 以下是有关远程桌面客户端最常见问题的解答。 

- [设置](#Setting-up)
- [连接、 网关和网络](#connection-gateway-and-networks)
- [web 客户端](#web-client)
- [监视器、 音频和鼠标](#monitors-audio-and-mouse)
- [Mac 硬件](#mac-client---hardware-questions)
- [具体的错误消息](#specific-errors)

这些问题大多数适用于所有客户端，但有几个客户端特定项。

如果你有希望我们回答的其他问题，将其留作为反馈在这篇文章。

## <a name="setting-up"></a>设置

### <a name="which-pcs-can-i-connect-to"></a>可以连接到哪些 Pc？

请查看[支持的配置](remote-desktop-supported-config.md)一文，了解有关你可以连接到哪些 Pc 的信息。

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>如何设置一台 PC 为远程桌面？

我有我的设备设置，但我认为 PC 的就绪。 帮助？

首先，您看到远程桌面安装程序向导？ 它将引导您完成远程访问准备您的 PC。 在您的 PC 获得所有工具的下载和运行设置。 

否则，如果想要手动执行操作，请继续阅读。

适用于 Windows 10，请执行以下操作：

1. 在你想要连接到设备上，打开**设置**。
2. 选择**系统**，然后**远程桌面**。
3. 使用滑块可启用远程桌面。
4. 一般情况下，它是让 PC 唤醒状态和可发现性，便于建立连接。 单击**显示其设置**转到您的 PC，您可以在其中更改此设置的电源设置。
   > [!NOTE]
   > 无法连接到 PC 处于睡眠状态或休眠状态，因此请确保设置睡眠和休眠远程 PC 上的设置为**从不**。 （休眠不在所有 Pc 上可用。）


请记下此计算机上的名称**如何连接到这台电脑**。 你将需要它来配置客户端。

您可以授权特定用户可以访问这台电脑-若要执行此操作，请单击**选择用户可以远程访问这台电脑**。
管理员组的成员自动具有访问权限。

对于 Windows 8.1，请按照的说明进行操作，以允许远程连接中的[连接到使用远程桌面连接的另一个桌面](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8)。



## <a name="connection-gateway-and-networks"></a>连接、 网关和网络

### <a name="why-cant-i-connect-using-remote-desktop"></a>为何无法连接使用远程桌面？

以下是一些可能的解决方案时尝试连接到远程电脑可能会遇到的常见问题。 如果这些解决方案不起作用，您可以在找到更多帮助[Microsoft Community 网站](https://go.microsoft.com/fwlink/p/?LinkId=242079)。

- **找不到远程电脑。** 请确保具有正确的 PC 名称，然后进行检查，请参阅是否你输入该名称是否正确。 如果仍然无法连接，请尝试使用远程 PC 的 IP 地址而不电脑名称。
- **没有与网络问题。** 请确保具有 internet 连接。 
- **可能被防火墙阻止远程桌面端口。** 如果正在使用 Windows 防火墙，请按照下列步骤：

   1. 打开 Windows 防火墙。 
   2. 单击**允许应用或功能通过 Windows 防火墙**。 
   3. 单击**更改设置**。 您可能会要求输入管理员密码或确认你的选择。
   4. 下**允许的应用程序和功能**，选择**远程桌面**，然后点击或单击**确定**。

   如果使用其他防火墙，请确保远程桌面 (通常为 3389) 端口处于打开状态。
- **远程连接可能不会设置远程 PC 上。** 若要解决此问题，向上重新滚动到[如何设置一台 PC 为远程桌面？](#how-do-i-set-up-a-pc-for-remote-desktop)本主题中的问题。
- **远程 PC 可能只允许连接的电脑网络级身份验证设置。** 
- **远程 PC 可能已关闭。** 无法连接到已关闭、 处于休眠状态，PC 或休眠状态，因此请确保设置睡眠和休眠远程 PC 上的设置为**从不**（休眠不在所有 Pc 上可用。）。

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>为什么无法找到或连接到我的电脑？
检查下列项目：
- 是 PC 以及被唤醒？
- 未输入正确的名称或 IP 地址？

   > [!IMPORTANT]
   > 使用 PC 名称需要在网络正确通过 DNS 名称解析。 在很多家庭网络中，必须使用 IP 地址而不是主机名来连接。
- 不同网络上是 PC？ 未配置 PC 让外部通过建立的连接？  请查看[允许您从网络外部的电脑访问](remote-desktop-allow-outside-access.md)有关的帮助。
- 要连接到受支持的 Windows 版本？ 

   > [!NOTE]
   > 没有第三方软件的情况下不支持 Windows XP Home、 Windows Media Center Edition、 Windows Vista Home 和 Windows 7 家庭版或初学者。

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>为什么不能登录到远程电脑？
如果可以看到远程 PC 的登录屏幕，但无法登录，你可能不具有已添加到远程桌面用户组或具有远程 PC 上的管理员权限的任何组。 向系统管理员为你执行此操作。

### <a name="which-connection-methods-are-supported-for-company-networks"></a>支持的公司网络的连接方法？
如果你想要访问 office 桌面从公司网络之外，你的公司必须提供远程访问的一种方法。 远程桌面客户端目前支持以下功能：

- 终端服务器网关或远程桌面网关
- 远程桌面 Web 访问
- VPN （通过 iOS 内置 VPN 选项）

### <a name="vpn-doesnt-work"></a>不能使用 VPN

VPN 问题可以有几个原因。 第一步是验证 VPN 作为您的 PC 或 Mac 计算机在同一网络上正常工作。 如果不能使用 PC 或 Mac 进行测试，你可以尝试访问公司 intranet 网页上使用你的设备的浏览器。

要检查的其他事项：
- **3g 网络阻止或损坏了 VPN。** 有几个 3g 提供世界中的人为阻止或损坏 3g 流量。 验证 VPN 连接正常工作的一分钟内。
- **L2TP 或 PPTP Vpn。** 如果在您的 VPN 使用 L2TP 或 PPTP，请将发送所有流量中都设置为 ON 的 VPN 配置。
- **VPN 配置不正确。** 错误配置的 VPN 服务器可以是 VPN 连接，永远不会起作用或一段时间后停止工作的原因。 请确保使用同一网络上的 iOS 设备的 web 浏览器或在电脑或 Mac 测试，如果发生这种情况。

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>如何测试 VPN 是否正常工作？
验证在设备上启用 VPN。 可以通过在内部网络上转到网页或使用 web 服务，仅可通过 VPN 来测试你的 VPN 连接。

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>如何配置 L2TP 或 PPTP VPN 连接？
如果在您的 VPN 使用 L2TP 或 PPTP，请务必设置**发送的所有流量**到**ON**在 VPN 配置。

## <a name="web-client"></a>web 客户端

### <a name="which-browsers-can-i-use"></a>可以使用哪些浏览器？

Web 客户端支持 Microsoft Edge、 Internet Explorer 11 Mozilla Firefox (v55.0 及更高版本)，Safari 和 Google Chrome。

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>我可以使用哪些 Pc 来访问 web 客户端？

Web 客户端支持 Windows、 macOS、 Linux 和 ChromeOS。 目前不支持移动设备。

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>可以在远程桌面部署中无网关使用的 web 客户端？

否。 客户端需要远程桌面网关连接。 不知道这意味着？ 咨询有关你的管理员。

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>远程桌面 web 客户端是否将远程桌面 Web 访问页面？

否。 远程桌面 web 客户端托管的远程桌面 Web 访问网页的 url 不同。 可以使用 web 客户端或 Web 访问网页浏览器中查看的远程资源。

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>可以在另一个 web 页面中嵌入 web 客户端？

目前不支持此功能。

## <a name="monitors-audio-and-mouse"></a>监视器、 音频和鼠标

### <a name="how-do-i-use-all-of-my-monitors"></a>如何使用所有我监视器？
若要使用两个或多个屏幕，请执行以下操作：

1. 右键单击你想要启用多个屏幕，然后单击远程桌面**编辑**。
2. 启用**使用的所有监视器**并**全屏**。

### <a name="is-bi-directional-sound-supported"></a>支持的双向声音？
远程桌面客户端不支持声音上游 （从客户端到服务器，麦克风）。

### <a name="what-can-i-do-if-the-sound-wont-play"></a>如果不会播放声音，我可以做什么？
注销会话 （不只是断开连接，一直注销），并再次登录。

## <a name="mac-client---hardware-questions"></a>Mac 客户端的硬件问题
### <a name="is-retina-resolution-supported"></a>是否支持 retina 分辨率？
是的远程桌面客户端支持 retina 分辨率。

### <a name="how-do-i-enable-secondary-right-click"></a>如何启用辅助右键单击？
为了使在打开的会话有三个选项中右键单击的使用：

- 标准 PC 两个按钮的 USB 鼠标
- Apple 神奇鼠标：若要启用右键单击，单击**系统首选项**在停靠窗口，单击**鼠标**，然后启用**辅助单击**。
- Apple 神奇触控板或 MacBook 触控板：若要启用右键单击，单击**系统首选项**在停靠窗口，单击**鼠标**，然后启用**辅助单击**。

### <a name="is-airprint-supported"></a>AirPrint 支持？
否，远程桌面客户端不支持 AirPrint。 （这适用于 Mac 和 iOS 客户端。）

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>不正确的字符并显示在会话中
如果使用国际键盘，你可能会看到问题显示在会话中的字符是其中的字符执行匹配 Mac 键盘上键入。

这可能出现在以下方案：

- 使用远程会话不能识别的键盘。 远程桌面无法识别键盘，它默认为使用远程 PC 上一次使用的语言。
- 正在连接到远程 PC 上之前断开连接的会话和远程 PC 使用与语言的不同的键盘语言你当前尝试使用。

可以通过手动将键盘语言设置为远程会话来修复此问题。 请参阅下一节中的步骤。

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>语言设置如何影响键盘的远程会话中？
有许多类型的 Mac 键盘布局。 其中一些是 Mac 的特定布局或为其可能不可用的是到远程处理的 Windows 版本上的完全匹配项的自定义布局。 远程会话将键盘映射到的最匹配的远程 PC 上提供了键盘语言版本。 

如果你的 Mac 键盘布局设置为语言键盘 （例如，法语-PC） 应正确映射所有键和键盘的 PC 版本应该可以正常工作。

如果 Mac 键盘布局设置为键盘 （例如，法语） 的 Mac 版本的远程会话将映射您到法语语言的 PC 版本。 一些你习惯于用来在 OSX 上的 Mac 键盘快捷方式不会在远程 Windows 会话中。

如果您的键盘布局设置为一种语言 （例如，加拿大法语） 的变体和远程会话不能将您映射到该确切的变体，远程会话会将您映射到最接近的语言 （例如，法语）。 一些你习惯于用来在 OSX 上的 Mac 键盘快捷方式不会在远程 Windows 会话中。

如果您的键盘布局设置为远程会话不能完全匹配的布局，远程会话将默认为您提供上次与该 PC 一起使用的语言。 在这种情况下，或在需要更改以匹配 Mac 键盘，在远程会话的语言的情况下可以在远程会话的语言，与你想要使用，如下所示的一个最接近的匹配中手动设置键盘语言。

使用以下说明更改远程桌面会话内的键盘布局：

**在 Windows 10 或 Windows 8:**

1. 从在远程会话中，打开区域和语言。 单击**启动 > 设置 > 时间和语言**。 打开**区域和语言**。
2. 添加你想要使用的语言。 然后关闭区域和语言选项窗口中。
3. 现在，在远程会话中，您会看到语言之间切换的功能。 （在右侧的远程会话中，时钟附近。）单击你想要切换到的语言 (如**Eng**)。

您可能需要关闭并重新启动应用程序当前使用的键盘更改才会生效。


## <a name="specific-errors"></a>特定错误

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>为什么收到"权限不足"错误？
您不能访问你想要连接到的会话。 最可能的原因是您尝试连接到管理会话。 只有管理员才有权连接到控制台。 验证控制台交换机的远程桌面的高级设置中处于关闭状态。 如果这不是问题根源，请联系系统管理员联系以获得进一步帮助。

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>客户端为什么说是没有 CAL？
当远程桌面客户端连接到远程桌面服务器时，服务器会发出远程桌面服务客户端访问许可证 (RDS CAL) 存储的客户端。 当客户端连接时，它将再次使用其 RDS CAL 和服务器不会发出另一个许可证。 如果在设备上的 RDS CAL 已丢失或损坏，服务器会颁发另一个许可证。 达到许可的设备的最大数目时服务器将不发出新的 RDS Cal。 与网络管理员联系以获得帮助。

### <a name="why-did-i-get-an-access-denied-error"></a>为什么未收到"拒绝访问"错误？
在连接尝试期间，"拒绝访问"错误是生成通过远程桌面网关和凭据不正确的结果。 验证你的用户名和密码。 如果连接工作之前，并且最近发生了错误，您可能已更改你的 Windows 用户帐户密码并尚未它尚未更新中的远程桌面设置。

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>有什么作用"RPC 错误 23014"或"错误 0x59e6"平均？
情况下**RPC 错误 23014**或**等待几分钟后重试错误 0x59E6**，RD 网关服务器已达到最大活动连接数。 具体取决于 Windows 版本在 RD 网关上运行的最大连接数不同：Windows Server 2008 R2 Standard 实现限制为 250 的连接数。 Windows Server 2008 R2 Foundation 实现限制为 50 的连接数。 所有其他 Windows 实现允许无限的数量的连接。

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>"分析 NTLM 质询失败"错误是什么意思？
在远程 PC 上的配置不正确导致此错误。 请确保在远程 PC 上的 RDP 安全级别设置设置为"客户端兼容。 （与系统管理员联系如果需要帮助执行此操作。）

### <a name="what-does-tsrap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>什么不"TS_RAP 您不能连接到给定主机"意味着？
当资源授权策略的网关服务器上停止你的用户名连接到远程电脑时，会发生此错误。 这可以在以下情况：

- 远程 PC 名称是与网关的名称相同。 然后，当您尝试连接到远程电脑，连接将转到网关相反，您可能没有访问权限。 如果需要连接到网关，请使用外部网关名称作为 PC 名称。 而是使用"localhost"或 IP 地址 (127.0.0.1) 或内部服务器名称。
- 你的用户帐户不是远程访问的用户组的成员。