---
title: 访问远程桌面 Web 客户端
description: 介绍如何登录到远程桌面 web 客户端。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/20/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: f4433ad592219d6ed15b28fd0514790b078525fd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849368"
---
# <a name="access-the-remote-desktop-web-client"></a>访问远程桌面 Web 客户端

远程桌面 web 客户端，可以使用兼容的 web 浏览器访问你组织的远程资源 （应用和桌面） 发布到你的管理员联系。您可以与远程应用和桌面就像对无论身在何处，而无需切换到不同的台式计算机的本地 PC 进行交互。 一旦你的管理员设置了远程资源，只需要是域、 用户名、 密码，URL 发送，且受支持的 web 浏览器中，你的管理员，并且就可以开始。

>[!NOTE]
>Web 客户端的新版本感到好奇？ 请查看[什么是远程桌面 web 客户端的新增功能？](web-client-whatsnew.md)

## <a name="what-youll-need-to-use-the-web-client"></a>你将需要使用 web 客户端

* 对于 web 客户端，你将需要运行 Windows、 macOS、 ChromeOS 或 Linux 的电脑。 目前不支持移动设备。
* Microsoft Edge、 Internet Explorer 11、 Google Chrome、 Safari、 或 Mozilla Firefox 等的现代浏览器 (v55.0 及更高版本)。
* URL 你的管理员向你发送。

>[!NOTE]
>Web 客户端的 Internet Explorer 版本目前没有音频。
>如果浏览器调整大小或进入全屏多次，safari 可能显示灰色屏幕。

## <a name="start-using-the-remote-desktop-client"></a>开始使用远程桌面客户端

若要登录到客户端，请转到你的管理员向你发送的 URL。 在登录页上，输入你的域和用户名称的格式```DOMAIN\username```，输入你的密码，并选择**登录**。

>[!NOTE]
>在登录到 web 客户端，即表示你同意您的 PC 符合您组织的安全策略。

登录后，客户端会转到**的所有资源**选项卡，其中包含的所有项发布到你在一个或多个可折叠组，如"工作资源"组下。 你将看到几个图标表示应用程序、 桌面或包含多个应用程序或桌面管理员已向工作组提供的文件夹。 您可以继续阅读此选项卡在任何时候启动其他资源。

若要开始使用应用程序或桌面，选择你想要使用，相同的用户名和密码用于登录到 web 客户端如果系统提示，请输入的项，然后选择**提交**。 您可能还会出现同意对话框来访问本地资源，如剪贴板和打印机。 您可以选择不重定向其中一种，或选择**允许**以使用默认设置。 等待建立连接，web 客户端，然后启动便可正常使用资源。

完成后，可以通过选择结束会话**注销**屏幕或关闭浏览器窗口的顶部工具栏中的按钮。

## <a name="printing-from-the-remote-desktop-web-client"></a>从远程桌面 web 客户端打印

请执行以下步骤从 web 客户端打印：

1. 可为你想要从打印应用程序的正常开始打印进程。
2. 当系统提示您选择打印机，选择**远程桌面虚拟打印机**。
3. 选择您的首选项，然后选择**打印**。
4. 在浏览器将生成您的打印作业的 PDF 文件。
5. 您可以选择打开 pdf 文件和打印其内容与本地打印机，或将其保存到您的 PC 以供将来使用。

## <a name="copy-and-paste-from-the-remote-desktop-web-client"></a>从远程桌面 web 客户端复制和粘贴

Web 客户端目前支持复制和粘贴纯文本。 不能复制或粘贴到和来自 web 客户端文件。 此外，仅可以使用**Ctrl + C**并**Ctrl + V**进行复制和粘贴文本。

## <a name="get-help-with-the-web-client"></a>获取有关 web 客户端的帮助

如果您遇到不能通过这篇文章中的信息来解决了问题，可以通过向 web 客户端的关于页面上的地址发送电子邮件与 web 客户端获取帮助。