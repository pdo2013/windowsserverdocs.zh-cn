---
title: 远程桌面客户端常见问题解答
description: 有关远程桌面客户端的常见问题
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 5b1dd3b728f941d9c3732abccf19363cf631284e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387750"
---
# <a name="frequently-asked-questions-about-the-remote-desktop-clients"></a>有关远程桌面客户端的常见问题

>适用于：Windows 10、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2

你已经在设备（Android、Mac、iOS 或 Windows）上设置了远程桌面客户端，现在可能会有疑问。 以下是有关远程桌面客户端的最常见问题的解答。 

- [设置](#setting-up)
- [连接、网关和网络](#connection-gateway-and-networks)
- [Web 客户端](#web-client)
- [监视器、音频和鼠标](#monitors-audio-and-mouse)
- [Mac 硬件](#mac-client---hardware-questions)
- [特定错误消息](#specific-errors)

其中大部分问题适用于所有客户，但有部分客户特定项目。

如果你还有希望我们回答的其他问题，请在本文的反馈部分留下相关问题。

## <a name="setting-up"></a>设置

### <a name="which-pcs-can-i-connect-to"></a>我可以连接到哪些电脑？

查看[受支持的配置](remote-desktop-supported-config.md)一文，了解有关可以连接到的电脑的信息。

### <a name="how-do-i-set-up-a-pc-for-remote-desktop"></a>如何为远程桌面设置电脑？

我设置了设备，但我认为电脑尚未准备就绪。 能帮帮我吗？

首先，你是否看到过“远程桌面设置”向导？ 它将指导你帮助电脑为远程访问做好准备。 在电脑上下载并运行该工具以完成所有设置。 

或者，如果你更喜欢手动操作，请继续阅读。

对于 Windows 10，请执行以下操作：

1. 在想要连接到的设备上，打开“设置”  。
2. 依次选择“系统”和“远程桌面”   。
3. 使用滑块启用远程桌面。
4. 一般情况下，最好让电脑保持唤醒和可检测到的状态，以便于连接。 单击“显示设置”以转到电脑的电源设置，可在此处更改此设置  。
   > [!NOTE]
   > 无法连接到处于睡眠或休眠状态的电脑，因此请确保远程电脑上的睡眠和休眠设置设为“从不”  。 （并非所有电脑都提供休眠模式。）


记下“如何连接到此电脑”下此电脑的名称  。 需要用它来配置客户端。

可以授予特定用户访问此电脑的权限 - 为此，请单击“选择可以远程访问此电脑的用户”  。
Administrators 组的成员自动拥有访问权限。

对于 Windows 8.1，请按照[使用远程桌面连接连接到另一个桌面](https://support.microsoft.com/en-us/help/17463/windows-7-connect-to-another-computer-remote-desktop-connection#1TC=windows-8)中的说明允许远程连接。



## <a name="connection-gateway-and-networks"></a>连接、网关和网络

### <a name="why-cant-i-connect-using-remote-desktop"></a>为什么无法使用远程桌面？

以下是尝试连接到远程电脑时可能会遇到的常见问题的一些可能解决方案。 如果这些解决方案不起作用，可以在 [Microsoft 社区网站](https://go.microsoft.com/fwlink/p/?LinkId=242079)上找到更多帮助。

- **无法找到远程电脑。** 确保拥有正确的电脑名称，然后检查是否正确输入了该名称。 如果仍然无法连接，请尝试使用远程电脑的 IP 地址而不是电脑名称。
- **网络出现问题。** 确保具有 Internet 连接。 
- **远程桌面端口可能被防火墙阻止。** 如果使用 Windows 防火墙，请按照以下步骤执行操作：

  1. 打开 Windows 防火墙。 
  2. 单击“允许应用或功能通过 Windows 防火墙”  。 
  3. 单击“更改设置”  。 系统可能会要求输入管理员密码或确认选择。
  4. 在“允许的应用和功能”下，选择“远程桌面”，然后点击或单击“确定”    。

     如果使用其他防火墙，请确保远程桌面端口（通常为 3389）已打开。
- **远程电脑上可能未设置远程连接。** 若要修复此问题，请向上滚动至本主题中的[如何为远程桌面设置电脑？](#how-do-i-set-up-a-pc-for-remote-desktop)这个问题。
- **远程电脑可能仅允许连接已设置网络级别身份验证的电脑。** 
- **远程电脑可能已关闭。** 无法连接到处于已关闭、睡眠或休眠状态的电脑，因此请确保远程电脑上的睡眠和休眠设置设为“从不”（并非所有电脑都提供休眠模式）  。

### <a name="why-cant-i-find-or-connect-to-my-pc"></a>为什么找不到或无法连接到我的电脑？

检查下列项目：

- 电脑是否已打开并处于唤醒状态？
- 是否输入了正确的名称或 IP 地址？

   > [!IMPORTANT]
   > 使用电脑名称要求网络通过 DNS 正确解析名称。 在许多家庭网络中，必须使用 IP 地址（而不是主机名）进行连接。
- 电脑是否位于不同的网络上？ 是否将电脑配置为允许外部连接通过？  查看[允许从你的网络外部访问你的电脑](remote-desktop-allow-outside-access.md)以获取帮助。
- 是否连接到受支持的 Windows 版本？ 

   > [!NOTE]
   > 如果没有第三方软件，则不支持 Windows XP 家庭版、Windows 媒体中心版、Windows Vista 家庭版和 Windows 7 家庭版或简易版。

### <a name="why-cant-i-sign-in-to-a-remote-pc"></a>为什么无法登录到远程电脑？

如果可以看到远程电脑的登录屏幕但无法登录，则可能未被添加到远程桌面用户组或远程电脑上具有管理员权限的任何组。 要求系统管理员对你进行添加。

### <a name="which-connection-methods-are-supported-for-company-networks"></a>公司网络支持哪些连接方法？

如果想要从公司网络外部访问办公桌面，则公司必须为你提供远程访问方式。 RD 客户端目前支持以下方式：

- 终端服务器网关或远程桌面网关
- 远程桌面 Web 访问
- VPN（通过 iOS 内置 VPN 选项）

### <a name="vpn-doesnt-work"></a>VPN 无法使用

VPN 问题可能有多种原因。 第一步是验证 VPN 是否在与电脑或 Mac 计算机相同的网络上运行。 如果无法使用电脑或 Mac 进行测试，则可以尝试使用设备的浏览器访问公司 Intranet 网页。

需要检查的其他事项：
- **3G 网络阻止 VPN 或导致 VPN 损坏。** 全球有多家 3G 提供商似乎会阻止 3G 流量或导致其损坏。 对 VPN 连接是否正常工作进行一分钟以上的验证。
- **L2TP 或 PPTP VPN。** 如果在 VPN 中使用 L2TP 或 PPTP，请在 VPN 配置中将“发送所有流量”设置为“开”。
- **VPN 配置错误。** 错误配置的 VPN 服务器可能是 VPN 连接从不工作或在一段时间后停止工作的原因。 如果发生这种情况，请确保使用 iOS 设备的 Web 浏览器或同一网络上的电脑或 Mac 进行测试。

### <a name="how-can-i-test-if-vpn-is-working-properly"></a>如何测试 VPN 是否正常工作？

验证设备上是否已启用 VPN。 可以通过转到内部网络上的网页或使用仅通过 VPN 提供的 Web 服务来测试 VPN 连接。

### <a name="how-do-i-configure-l2tp-or-pptp-vpn-connections"></a>如何配置 L2TP 或 PPTP VPN 连接？

如果在 VPN 中使用 L2TP 或 PPTP，请确保在 VPN 配置中将“发送所有流量”设置为“开”   。

## <a name="web-client"></a>Web 客户端

### <a name="which-browsers-can-i-use"></a>我可以使用哪些浏览器？

Web 客户端支持 Microsoft Edge、Internet Explorer 11、Mozilla Firefox（v55.0 及更高版本）、Safari 和 Google Chrome。

### <a name="what-pcs-can-i-use-to-access-the-web-client"></a>我可以使用哪些电脑来访问 Web 客户端？

Web 客户端支持 Windows、macOS、Linux 和 ChromeOS。 目前不支持移动设备。

### <a name="can-i-use-the-web-client-in-a-remote-desktop-deployment-without-a-gateway"></a>我是否可以在没有网关的远程桌面部署中使用 Web 客户端？

否。 客户端需要远程桌面网关才能连接。 不知道这意味着什么？ 询问管理员相关信息。

### <a name="does-the-remote-desktop-web-client-replace-the-remote-desktop-web-access-page"></a>远程桌面 Web 客户端是否会取代远程桌面 Web 访问页面？

否。 远程桌面 Web 客户端在与远程桌面 Web 访问页面不同的 URL 中托管。 可以使用 Web 客户端或 Web 访问页面在浏览器中查看远程资源。

### <a name="can-i-embed-the-web-client-in-another-web-page"></a>是否可以将 Web 客户端嵌入另一个网页？

目前不支持此功能。

## <a name="monitors-audio-and-mouse"></a>监视器、音频和鼠标

### <a name="how-do-i-use-all-of-my-monitors"></a>如何使用我的所有监视器？
若要使用两个或更多屏幕，请执行以下操作：

1. 右键单击想要为其启用多个屏幕的远程桌面，然后单击“编辑”  。
2. 启用“使用所有监视器”和“全屏”   。

### <a name="is-bi-directional-sound-supported"></a>是否支持双向声音？
远程桌面客户端不支持上游的声音（从客户端到服务器、麦克风）。

### <a name="what-can-i-do-if-the-sound-wont-play"></a>如果声音无法播放，我该怎么办？
注销会话（不要仅断开连接，应完全注销），然后再次登录。

## <a name="mac-client---hardware-questions"></a>Mac 客户端 - 硬件问题
### <a name="is-retina-resolution-supported"></a>是否支持 Retina 分辨率？
是，远程桌面客户端支持 Retina 分辨率。

### <a name="how-do-i-enable-secondary-right-click"></a>如何启用辅助右键单击？
若要在打开的会话中使用右键单击，你有三个选项：

- 标准电脑双按钮 USB 鼠标
- Apple Magic Mouse：若要启用右键单击，请单击停靠窗口中的“系统偏好设置”，然后单击“鼠标”，接下来启用“辅助单击”    。
- Apple Magic Trackpad 或 MacBook 触控板：若要启用右键单击，请单击停靠窗口中的“系统偏好设置”，然后单击“鼠标”，接下来启用“辅助单击”    。

### <a name="is-airprint-supported"></a>是否支持 AirPrint？
否，远程桌面客户端不支持 AirPrint。 （对于 Mac 和 iOS 客户端同样适用。）

### <a name="why-do-incorrect-characters-appear-in-the-session"></a>为什么会话中会出现错误字符？
如果使用国际键盘，则可能会发现会话中显示的字符与在 Mac 键盘上键入的字符之间存在匹配问题。

以下方案中可能会发生这种情况：

- 正在使用远程会话无法识别的键盘。 当远程桌面无法识别键盘时，它默认使用远程电脑上次使用的语言。
- 正在连接到远程电脑上之前断开连接的会话，并且该远程电脑使用的键盘语言与你当前尝试使用的语言不同。

可以通过手动设置远程会话的键盘语言来解决此问题。 请参阅下一部分中的相关步骤。

### <a name="how-do-language-settings-affect-keyboards-in-a-remote-session"></a>语言设置如何影响远程会话中的键盘？
有许多类型的 Mac 键盘布局。 其中一些是 Mac 特定的布局或自定义布局，这些布局在远程登录到的 Windows 版本中可能没有完全匹配项。 远程会话将键盘映射到远程电脑上可用的最佳匹配键盘语言。 

如果 Mac 键盘布局设置为电脑版本的语言键盘（例如，法语 - 电脑），则所有键应正确映射，且键盘应正常工作。

如果 Mac 键盘布局设置为 Mac 版本的键盘（例如，法语），则远程会话会将你映射到电脑版本的法语。 在 OSX 上习惯使用的某些 Mac 键盘快捷方式在远程 Windows 会话中无法使用。

如果键盘布局设置为某种语言的变体（例如，加拿大法语），且如果远程会话无法将你映射到该具体变体，则远程会话会将你映射到最接近的语言（例如，法语）。 在 OSX 上习惯使用的某些 Mac 键盘快捷方式在远程 Windows 会话中无法使用。

如果键盘布局设置为远程会话完全无法匹配的布局，则远程会话将默认为你提供上次使用该电脑时使用的语言。 在这种情况下，或在需要更改远程会话的语言以匹配 Mac 键盘的情况下，可以按照以下说明，手动将远程会话中的键盘语言设置为与想要使用的语言最接近的匹配语言。

使用以下说明更改远程桌面会话中的键盘布局：

**在 Windows 10 或 Windows 8 上：**

1. 在远程会话内部打开“区域和语言”。 单击“开始”>“设置”>“时间和语言”  。 打开“区域和语言”  。
2. 添加想要使用的语言。 然后关闭“区域和语言”窗口。
3. 现在，在远程会话中，将能够在语言之间切换。 （在远程会话的右侧，靠近时钟的位置。）单击想要切换到的语言（例如“英语”  ）。

可能需要关闭并重启当前使用的应用程序才能使键盘更改生效。


## <a name="specific-errors"></a>特定错误

### <a name="why-do-i-get-an-insufficient-privileges-error"></a>为什么会收到“权限不足”错误？
你未被允许访问想要连接到的会话。 最有可能的原因是你正在尝试连接到管理员会话。 仅允许管理员连接到控制台。 在远程桌面的高级设置中验证控制台开关是否已关闭。 如果这不是问题的根源，请与系统管理员联系，以获得进一步的帮助。

### <a name="why-does-the-client-say-that-there-is-no-cal"></a>为什么客户端表示没有 CAL？
当远程桌面客户端连接到远程桌面服务器时，服务器会颁发由客户端存储的远程桌面服务客户端访问许可证 (RDS CAL)。 每当客户端再次连接时，服务器都将使用其 RDS CAL，且不会颁发另一个许可证。 如果设备上的 RDS CAL 丢失或损坏，服务器将颁发另一个许可证。 达到许可设备的最大数量时，服务器不会颁发新的 RDS CAL。 请与网络管理员联系，以获取帮助。

### <a name="why-did-i-get-an-access-denied-error"></a>为什么收到“拒绝访问”错误？
“拒绝访问”错误由远程桌面网关生成，且它是连接尝试期间凭据不正确导致的结果。 验证用户名和密码。 如果连接之前正常工作且错误在最近发生，则可能已更改 Windows 用户帐户密码，但尚未在远程桌面设置中对其进行更新。

### <a name="what-does-rpc-error-23014-or-error-0x59e6-mean"></a>“RPC 错误 23014”或“错误 0x59e6”意味着什么？
对于 **RPC 错误 23014** 或**错误 0x59E6 等待几分钟后重试**，其表示 RD 网关服务器已达到最大活动连接数。 根据 RD 网关上运行的 Windows 版本，最大连接数有所不同：Windows Server 2008 R2 Standard 实现将连接数限制为 250。 Windows Server 2008 R2 Foundation 实现将连接数限制为 50。 所有其他 Windows 实现允许不限数量的连接。

### <a name="what-does-the-failed-to-parse-ntlm-challenge-error-mean"></a>“分析 NTLM 质询失败”错误意味着什么？
此错误由远程电脑上的错误配置引起。 确保远程电脑上的 RDP 安全级别设置设为“客户端兼容”。 （如果需要帮助，请与系统管理员联系。）

### <a name="what-does-ts_rap-you-are-not-allowed-to-connect-to-the-given-host-mean"></a>“TS_RAP 不允许连接到给定主机”意味着什么？
当网关服务器上的资源授权策略阻止你的用户名连接到远程电脑时，会发生此错误。 在以下实例中可能会发生这种情况：

- 远程电脑名称与网关名称​​相同。 之后，当你尝试连接到远程电脑时，连接将转到你可能无权访问的网关。 如果需要连接到网关，请不要使用外部网关名称作为电脑名称， 而是使用“localhost”或 IP 地址 (127.0.0.1) 或内部服务器名称。
- 用户帐户不是远程访问用户组的成员。