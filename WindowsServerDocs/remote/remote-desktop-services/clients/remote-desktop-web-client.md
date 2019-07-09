---
title: 访问远程桌面 Web 客户端
description: 介绍如何登录到远程桌面 Web 客户端。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743834"
---
# <a name="access-the-remote-desktop-web-client"></a>访问远程桌面 Web 客户端

通过远程桌面 Web 客户端可以使用兼容的 Web 浏览器访问管理员向你发布的组织的远程资源（应用和桌面）。无论你身在何处，都可以与远程应用和桌面交互，就像与本地 PC 交互一样，而无需切换到不同的台式电脑。 管理员设置你的远程资源后，只需管理员向你发送的域、用户名、密码、URL，以及受支持的 Web 浏览器即可继续操作。

>[!NOTE]
>想知道 Web 客户端的新版本吗？ 请查看[远程桌面 Web 客户端的新增功能](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>使用 Web 客户端所需的条件

* 对于 Web 客户端，你将需要运行 Windows、macOS、ChromeOS 或 Linux 的 PC。 目前不支持移动设备。
* Microsoft Edge、Internet Explorer 11、Google Chrome、Safari 或 Mozilla Firefox（v55.0 及更高版本）等新式浏览器。
* 管理员向你发送的 URL。

>[!NOTE]
>Web 客户端的 Internet Explorer 版本目前没有音频。
>如果浏览器已调整大小或多次进入全屏，则 Safari 可能会显示灰色屏幕。

## <a name="start-using-the-remote-desktop-client"></a>开始使用远程桌面客户端

若要登录到客户端，请转到管理员向你发送的 URL。 在登录页上，采用格式 ```DOMAIN\username``` 输入域和用户名，输入密码，然后选择“登录”  。

>[!NOTE]
>登录到 Web 客户端，即表示你同意 PC 符合组织的安全策略。

登录后，客户端将转到“所有资源”  选项卡，一个或多个可折叠组下包含发布给你的所有项，例如“工作资源”组。 你将看到几个表示应用、桌面或包含管理员已向工作组提供的多个应用或桌面的文件夹的图标。 可以随时返回到此选项卡来启动其他资源。

若要开始使用应用或桌面，请选择要使用的项，在出现提示时输入用于登录到 Web 客户端的同一用户名和密码，然后选择“提交”  。 可能还会向你显示用于访问本地资源（如剪贴板和打印机）的同意对话框。 可以选择不重定向任意一项，或选择“允许”  以使用默认设置。 等待 Web 客户端建立连接，然后按正常方式开始使用资源。

完成后，可以通过选择屏幕顶部的工具栏中的“注销”  按钮或关闭浏览器窗口来结束会话。

## <a name="printing-from-the-remote-desktop-web-client"></a>从远程桌面 Web 客户端打印

按照以下步骤从 Web 客户端打印：

1. 按正常方式为你想要从中打印的应用启动打印过程。
2. 在提示选择打印机时，选择“远程桌面虚拟打印机”  。
3. 选择首选项后，选择“打印”  。
4. 浏览器将生成打印作业的 PDF 文件。
5. 你可以选择打开 PDF 并将其内容打印到本地打印机，或将其保存到你的 PC 以供将来使用。

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>从远程桌面 Web 客户端复制和粘贴

Web 客户端当前仅支持复制和粘贴文本。 不能将文件复制或粘贴到 Web 客户端，也不能从 Web 客户端复制或粘贴文件。 此外，只能使用 Ctrl+C  和 Ctrl+V  来复制和粘贴文本。

## <a name="get-help-with-the-web-client"></a>获取有关 Web 客户端的帮助

如果本文中的信息无法解决你遇到的问题，则可以通过向 Web 客户端的“关于”页上的地址发送电子邮件来获取有关 Web 客户端的帮助。