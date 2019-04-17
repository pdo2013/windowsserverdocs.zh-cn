---
title: "解决了在 Windows Server Essentials 监视计算机"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f1e6b377-4a24-4d28-9b25-05910914826b
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 72fe309e0e7ce6d7227cce8b7f2c5dbf018eb4a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-computer-monitoring-in-windows-server-essentials"></a>解决了在 Windows Server Essentials 监视计算机

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本主题提供监视的运行状况的计算机中警报查看器和 Windows Server Essentials 中的电子邮件通知时遇到问题的疑难解答。  
  
> [!NOTE]
>  对于从 Windows Server Essentials 社区的最新的疑难解答信息，我们建议你访问[Windows Server Essentials 论坛](https://social.technet.microsoft.com/Forums/winserveressentials/threads)。 Windows Server Essentials 论坛是一个不错的进行搜索以获取帮助，或者提出问题。  
  
##  <a name="BKMK_TS"></a>故障排除的电子邮件通知的通知  
 此部分中列出了各种使用电子邮件通知的通知时，你可能会遇到的问题。  
  
### <a name="cannot-send-the-test-email-for-the-alert"></a>不能发送警报测试电子邮件  
 **问题**你收到错误消息，指示，不能发送警报测试电子邮件。  
  
 **原因**由于任何警报通知的设置中的以下问题可能会发生此错误：  
  
-   错误 SMTP 服务器名称或端口号。  
  
-   未正确指定 SMTP 服务器需要单个套接字层 (SSL) 连接。  
  
-   SMTP server 要求进行身份验证，并输入不正确的凭据。  
  
 **解决方案**设置电子邮件通知中的任何错误。  
  
##### <a name="to-identify-issues-in-your-email-notification-settings"></a>若要确定你的电子邮件通知设置中的问题  
  
-   正确 SMTP 服务器名称、端口号和 SSL 使用量询问你的 Internet 服务提供商 (ISP)。  
  
-   查看日志文件的电子邮件通知的通知在服务器上，在以下位置：  
  
     %ProgramData%\ Microsoft \Windows Server\Logs\SharedServiceHost-AlertServiceConfig.log  
  
    > [!TIP]
    >  若要查看 ProgramData 文件夹，你必须具有隐藏显示项目。 如果你不再看到 ProgramData 文件夹中，在功能区 s**视图**选项卡上，在**显示/隐藏**组中，选择**隐藏的项目**文本框。  
  
##### <a name="to-update-your-email-notification-setup-for-alerts"></a>若要更新你的通知的电子邮件通知设置  
  
1.  在仪表板上，单击右上角要打开提醒查看器中的任何通知图标。  
  
2.  在底部警报查看器中，单击**设置电子邮件通知的通知**。  
  
3.  在**设置电子邮件通知的通知**对话框中，单击**启用**。  
  
4.  在**SMTP 设置**对话框下，更新 SMTP 设置，然后单击**确定**。  
  
5.  若要测试更新的设置，请单击**应用并发送电子邮件**。  
  
6.  在验证测试电子邮件已成功后，单击**确定**保存更新的设置。  
  
### <a name="test-email-notification-does-not-list-any-alerts"></a>测试电子邮件通知中未列出任何通知  
 **问题**即使列出警报查看器中的通知警报测试电子邮件通知不显示任何通知。  
  
 **解决方案**报告警报查看器中并非所有通知都生成电子邮件通知。 配置为升级为其健康定义文件中的电子邮件通知警报用作电子邮件发送给指定的电子邮件收件人。  
  
 在你单击时**应用并发送电子邮件**，通常，您将使用任何列出的运行状况警报收到示例电子邮件通知。 但是，如果配置发送电子邮件通知健康警报标识该测试过程中，测试电子邮件中包含该警报。  
  
### <a name="active-alerts-are-displayed-for-an-uninstalled-application"></a>活动的通知将显示为已卸载的应用程序  
 **问题**即使已卸载的应用程序和它的运行状况定义文件显示应用程序的活动警报。  
  
 **解决方案**必须手动删除已卸载的应用程序的活动的通知。 若要删除的通知，请执行以下操作。  
  
##### <a name="to-delete-an-alert-from-the-server-by-using-the-dashboard"></a>若要从服务器通过仪表板中删除警报  
  
1.  在服务器上，打开仪表板。  
  
2.  在导航窗格中，单击任何显示警报图标（关键、警告或信息）。 这将启动警报查看器。  
  
3.  在通知查看器中，右键单击你想要删除，然后单击警报**删除此警报**。
