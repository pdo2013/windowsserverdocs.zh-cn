---
title: "管理 Windows Server Essentials 中的系统运行状况"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3043f83b-389c-4f37-a1ff-85afe99314fa
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91635a58c64fbf74d3b0139be7c9c36365487319
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="manage-system-health-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的系统运行状况

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 本主题介绍如何查看和通过仪表板响应你的网络中的所有通知。  
  
> [!NOTE]
>  在 Windows Server Essentials 和 Windows Server 2012 R2 安装了 Windows Server Essentials 体验角色，服务器和网络中的客户端计算机的运行状况警报不会再显示在通知查看器中，但改用可以在查看**健康报告**的选项卡**家庭**页面。  
  
 Windows Server Essentials 积极监视每一台计算机连接到服务器和警告缺少的恶意软件防护、 客户端计算机上的过期病毒定义和其他重要的问题，需要执行相关操作管理员与 s 系统运行状况，包括重要的更新相关的问题。 这些问题将显示为中警报查看器，可以从服务器 s 仪表板或者客户端计算机 s 启动栏或 Windows Server Essentials 中, 启动警报**健康报告**Windows Server Essentials 中的选项卡。 默认情况下，通知刷新每隔 30 分钟，但您可以随时评估你的网络的通知，方法是依次单击**刷新**上或警报查看**健康报告**选项卡。  
  
 以下主题将帮助你了解、 查看和响应提醒查看器中通知和还提供配置服务器接收电子邮件中的警报通知的说明进行操作：  
  
-   [有关健康报告外接程序](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_AddIn)  
  
-   [通过使用警报查看查看通知](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_View)  
  
-   [整理警报查看器中的通知](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Organize)  
  
-   [响应通知](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Respond)  
  
-   [电子邮件通知的通知设置](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)  
  
-   [潜在计算机警报](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Potential)  
  
##  <a name="BKMK_AddIn"></a>有关健康报告外接程序  
 健康报告加载项的 Windows Server Essentials 向你提供有关 Windows Server Essentials 网络统一的信息，使您可以分发给其他人此信息。 此信息可以在查看**报告**仪表板中的选项卡。 与**报告**选项卡上，你可以执行以下操作：  
  
-   [生成的报告根据需要，或按计划](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Generate)  
  
-   [自定义报告内容](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Customize)  
  
-   [通过电子邮件发送该报告](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_emailreport)  
  
> [!NOTE]
>  **Windows Server Essentials:**可下载健康报告加载项以从 Windows Server Essentials [Microsoft 下载中心](https://go.microsoft.com/fwlink/p/?LinkId=266342)。  
>   
>  **Windows Server Essentials:**默认情况下，健康报告加载项集成与 Windows Server Essentials 或 Windows Server 2012 R2 与 Windows Server Essentials 体验角色安装，并且运行状况报告将显示在**健康报告**仪表板 s 的选项卡**家庭**页面。  
  
###  <a name="BKMK_Generate"></a>生成的报告根据需要，或按计划  
 健康报告外接程序安装并重新启动仪表板一个新选项卡之后,**报告**添加到仪表板。 你可以根据需要随时健康报告生成方法是依次单击**生成健康报告**任务上**报告**选项卡。  
  
 将生成一健康报告后，在列表窗格中，具体由的日期和时间生成的报告将创建一个新项。 若要打开某个项目，你可以在列表窗格中，双击它，或者你可以选择它，然后单击**打开健康报告**任务窗格中。 该报告将显示在新窗口中 HTML 格式。  
  
 除了手动生成的报告，你可能还想要计划每天或每小时自动生成的报告。 若要执行此操作，请在任务窗格中，单击**自定义健康报告设置**，然后单击**计划和电子邮件**选项卡。**计划**功能处于关闭状态，默认情况下，并且可以通过选择将其打开**在计划的时间生成健康报告**复选框。  
  
###  <a name="BKMK_Customize"></a>自定义报告内容  
 健康报告包含以下：  
  
-   **重要警报和警告**这是第一致的重要提醒和警报仪表板上查看器在你看到的警告。 健康报告中不包含的信息警告。  
  
-   **非常重要的事件日志中的错误**应用程序和服务日志进行扫描，并将显示在最近的 24 小时内已登录的错误**详细信息**报告部分。  
  
-   **服务器备份**上次服务器备份信息中显示**详细信息**报告部分。  
  
-   **自动启动服务不运行**当时生成的报告，如果未运行程序自动启动的服务，该服务的信息将列入**详细信息**报告部分。  
  
-   **更新**可以看到更新服务器和中的所有客户端计算机的状态**详细信息**部分。  
  
-   **存储**显示在列表中的驱动器，并且它们的容量**详细信息**部分。  
  
 在运行状况报告中，首先查看**摘要**，然后针对这些商品与错误红色或黄色警告图标，单击**详细信息**链接查看有关项目的详细信息位于同一行上。  
  
 如果你不感兴趣一些数据点报告中包括默认情况下，你可以通过单击来自定义报告内容**自定义健康报告设置**中任务栏，然后单击**内容**选项卡清除你不再希望在报告中看到的内容的复选框。 例如，如果你拥有自己的服务器备份计划，并且不再想要看到有关服务器备份警告，你可能不服务器备份报告通过清除**服务器备份**复选框。  
  
###  <a name="BKMK_emailreport"></a>通过电子邮件发送该报告  
 无需登录到阅读报告仪表板上是适用于某些管理员，尤其是在他们有多个服务器管理仍带来不便。 电子邮件功能的打开之后将生成一份,，电子邮件将发送到的内容报告的指定的电子邮件地址列表。 管理员可以轻松地查看此报告从任何设备或任何客户端应用程序中，并确保在其最佳状态运行的服务器。  
  
 在**自定义健康报告设置**对话框下，启用电子邮件、 更改 SMTP 设置，并指定的一组电子邮件收件人之后, 你将注意到，新的任务将显示在任务窗格：**电子邮件健康报告**。 关于 SMTP 设置的详细信息，请参阅[设置电子邮件通知的通知](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Email)。  
  
 你可以选择现有的报告，然后单击**电子邮件健康报告**。 也可以生成新的报告，并使其自动发送给你的收件箱。 如果配置了可自动生成的报告的日程安排，该报告将自动发送到你的收件箱后每天 （或每隔一小时），按计划正在生成。  
  
##  <a name="BKMK_View"></a>通过使用警报查看查看通知  
 此部分中讨论了如何使用仪表板或启动栏打开警报查看器以查看服务器网络上的所有计算机的运行状况。  
  
#### <a name="to-open-the-alert-viewer-by-using-the-dashboard"></a>若要通过仪表板中打开警报查看器  
  
1.  打开的面板。  
  
2.  在导航窗格中，单击任何显示的通知图标 （关键、 警告或信息）。 这将打开警报查看器。  
  
#### <a name="to-open-the-alert-viewer-from-the-launchpad"></a>若要从启动栏打开警报查看器  
  
1.  从计算机的连接到服务器上，打开启动栏。 如果系统要求，登录到你的用户名和密码与启动栏。  
  
2.  单击任何若要打开提醒查看器中，启动栏底部显示警报图标 （关键、 警告和信息），然后按照说明详细信息窗格中的通知查看器来解决该警报。  
  
##  <a name="BKMK_Organize"></a>整理警报查看器中的通知  
 你可以整理警报查看器中的通知，并使其显示根据其严重性 （严重、 警告或信息） 或计算机名称。  
  
#### <a name="to-organize-alerts-in-the-alert-viewer"></a>若要整理警报查看器中的通知  
  
1.  打开的面板。  
  
2.  在导航窗格中，单击任何显示的通知图标 （关键、 警告或信息）。 这将打开警报查看器。  
  
3.  展开**组织**下拉列表，然后再执行下列情况之一：  
  
    1.  选择**由计算机的筛选器**，并单击你想要为其查看通知的计算机名称。 将显示在仅支持选定计算机警报查看通知。  
  
    2.  选择**按的通知类型筛选**，并单击你想要为其查看通知的通知类型 （关键、 警告或信息）。 这将通知查看器中显示所选通知类型。  
  
##  <a name="BKMK_Respond"></a>响应通知  
 当你遇到了一个警报时，您可以选择执行以下任一操作：  
  
-   [解决了警报](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Resolve)  
  
-   [忽略警报](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_3)  
  
-   [启用通知](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_5)  
  
-   [删除通知](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_4)  
  
###  <a name="BKMK_Resolve"></a>解决了警报  
 按照分辨率说明警报查看器中的解决该警报。 解决提醒后，它是仍查看显示在通知直到刷新。  
  
###  <a name="BKMK_3"></a>忽略警报  
 你可以选择忽略发出警告，如果你想要在以后响应它。 当你忽略了一个警报时，仍列出警报查看器，并但禁用并显示为灰色。 不包括在计算机的总体健康评估忽略的警报。 若要解决忽略的提醒，首先需要启用通知。  
  
##### <a name="to-ignore-an-alert"></a>若要忽略警报  
  
1.  从计算机中连接到 Windows Server Essentials 服务器，打开启动栏。  
  
2.  在启动栏中，单击任何显示警报图标 （关键、 警告和信息）。 这将打开警报查看器。  
  
3.  在警报查看器中选择你想要忽略，提醒，然后在**任务**部分中，单击**忽略警报**。  
  
 若要响应已禁用的通知，你将需要第一次启用通知。  
  
###  <a name="BKMK_5"></a>启用通知  
 你可以启用你选择忽略之前警报。 启用了该通知后，可以解决它或按需将其删除。 为禁用标记为忽略时，会显示一个警告。 启用你之前已禁用的通知时，它将处于活动状态，并再次包含在计算机的总体健康评估。  
  
##### <a name="to-enable-an-alert"></a>若要启用的提醒  
  
1.  从计算机中连接到服务器，打开启动栏。  
  
2.  在启动栏中，单击任何显示警报图标 （关键、 警告和信息） 若要打开提醒查看器。  
  
3.  在警报查看器中右键单击你想要启用，然后单击警报**启用警报**。  
  
###  <a name="BKMK_4"></a>删除通知  
 如果你不希望解决或忽略它，你可以删除警报。 可以使用上启动栏警报查看可以删除你的计算机上生成的通知。 如果你删除某个通知服务器在下一个网络健康评估周期再次检测到问题，它将生成新的警报。  
  
##### <a name="to-delete-an-alert"></a>若要删除的通知  
  
1.  从计算机中连接到服务器，打开启动栏。  
  
2.  在启动栏中，单击任何显示警报图标 （关键、 警告和信息） 若要打开提醒查看器。  
  
3.  在通知查看器中，右键单击你想要删除，然后单击警报**删除通知**。  
  
##  <a name="BKMK_Email"></a>电子邮件通知的通知设置  
 您可以配置服务器通过电子邮件有关出现的警报通知你。 这些警告音电子邮件通知包含信息的网络问题并解决其步骤中，相当于警报查看器中显示的信息。 以编程方式进行一些网络健康评估。  
  
 配置服务器来发送电子邮件中的通知时，警报期间网络健康评估发现发送电子邮件通知。 但是，在电子邮件中报告报告警报查看器中并非所有通知。  
  
 每隔 30 分钟，在服务器上，它评估网络中的通知警报电子邮件评估任务运行。 发送电子邮件已设置的电子邮件通知发生的任何通知。 如果该通知将在接下来的评估周期，以避免频繁电子邮箱仍处于活动状态，不会发送第二个电子邮件。 但是，如果在将来警报评估周期内检测到新提醒时，电子邮件通知就已发送，其中包括新的和之前的警报。  
  
###  <a name="BKMK_list"></a>导致电子邮件通知中的通知  
 当您设置了服务器来发送电子邮件通知的通知警报查看器中的以下警报导致电子邮件通知：  
  
-   在客户端计算机备份存在错误。  
  
-   重新启动的服务器。  
  
-   一个或多个服务都不运行。  
  
-   未运行 Windows Server 存储服务。  
  
-   你试用期已经结束。  
  
-   现在激活。  
  
-   源服务器关机。  
  
-   BPA 扫描结果包含错误。  
  
-   BPA 扫描结果包含警告。  
  
-   证书不是适用于任何地方访问。  
  
-   随时随地访问证书已过期。  
  
-   在路由器配置不正确。  
  
-   Web 服务器没有正确配置。  
  
-   远程桌面服务无法正确配置。  
  
-   防火墙无法正确配置。  
  
-   Internet 域名已过期。  
  
-   Internet 域名无法更新。  
  
-   许可证错误： 森林信任查看。  
  
-   许可证错误： 域控制器查看。  
  
-   许可证错误： FSMO 角色查看。  
  
-   许可证错误： 强制 FSMO 策略。  
  
-   许可证错误： 强制加载策略。  
  
-   许可证错误： Active Directory 域服务。  
  
-   你的 Office 365 订阅已过期。  
  
-   Office 365 验证不成功。  
  
-   不正确的密码的策略。  
  
-   密码同步服务无法与 Office 365 同步了用户密码。  
  
-   更改你的 Windows 密码。  
  
-   你的 Office 365 密码不是你的 Windows 密码相同。  
  
-   无法连接到 Exchange Server。  
  
-   Microsoft Exchange Server 有问题。  
  
-   未连接一个或多个中的硬盘服务器备份。  
  
-   备份硬盘驱动器的服务器备份没有足够的可用空间。  
  
-   服务器备份未成功，因为无法采取驱动器快照。  
  
-   未成功完成定期的备份。  
  
-   丢失了一个或多个预定义的服务器文件夹。  
  
-   一个或多个服务器硬盘驱动器可用空间不足。  
  
-   无法运行的 VSS Writer 存储服务。  
  
-   硬盘上的低存储容量。  
  
-   一个或多个驱动器不起作用，并且处于离线状态。  
  
###  <a name="BKMK_SMTP"></a>若要由 Windows Server Essentials 中的电子邮件发送警报通知你服务器上配置 SMTP  
 此部分中讨论如何配置服务器来发送电子邮件通知的通知。  
  
> [!NOTE]
>  你可以下载健康报告加载项以从 Windows Server Essentials [Microsoft 下载中心](https://go.microsoft.com/fwlink/p/?LinkId=266342)。  
  
##### <a name="to-set-up-email-notification-for-alerts"></a>若要设置电子邮件通知的通知  
  
1.  从**仪表板**，打开**警报查看**。  
  
2.  在**警报查看器**，单击**设置电子邮件的警报通知**。  
  
3.  在**设置电子邮件通知的通知**窗口中，单击**启用**。  
  
4.  在**SMTP 设置**窗口中，请执行以下操作：  
  
    1.  对于**从电子邮件地址**，从提醒你想要使用的发送电子邮件的电子邮件地址的类型。 此电子邮件地址将显示为警报通知发送方的地址。  
  
    2.  对于**SMTP 服务器名称**中**从电子邮件地址**文本框中，键入你步 4a 中指定的 SMTP 服务器的名称。 （参见表 1 有关的某些 SMTP 服务器名称列表）。  
  
    3.  对于**SMTP 端口**，键入端口号 SMTP 服务器使用发送和接收电子邮件。 （参见第 1 表所使用的某些 SMTP 服务器端口号）。  
  
    4.  选择**此服务器要求安全 (SSL) 连接**如果 SMTP server 使用 SSL （参见第 1 表）。  
  
    5.  选择**此服务器需要验证**SMTP 服务器是否需要用户名和密码信息 （参见第 1 表）。 如果你选中此复选框、 键入的用户名和密码中所输入你的电子邮件地址**从电子邮件地址**在步骤 4a 字段，然后单击**确定**。  
  
    > [!NOTE]
    >  你可以从你的 Internet 服务提供商获取关于 SMTP 服务器名称、 端口号和 SSL 使用情况信息。  
  
     **表 1**示例的 SMTP 服务器名称、 身份验证和 SSL 加密要求端口号  
  
    |SMTP 服务器|所需的 SSL|所需的身份验证|端口号|帐户名称登录/名|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|是的|是的|587|身份验证的域名称和密码提供完整的电子邮件地址。|  
    |smtp.live.com|是的|是的|587|身份验证的域名称和密码提供完整的电子邮件地址。|  
    |smtp.comcast.net|是的|不|587|身份验证的域名称和密码提供完整的电子邮件地址。|  
    |smtp.mail.yahoo.com|不|是的|25|提供的电子邮件地址将没有域名称的用户名。|  
  
5.  在**设置通知的通知**，对于**电子邮件收件人**，键入你想要通过电子邮件接收警报通知的人员的电子邮件地址。 确保你分号 （;） 的分隔每个电子邮件地址。  
  
6.  若要验证你是否配置 SMTP 服务器设置正确发送电子邮件通知的通知，请单击**应用并发送电子邮件**。  
  
    > [!NOTE]
    >  在你单击时**应用并发送电子邮件**，通常，您将使用任何列出的运行状况警报收到示例电子邮件通知。 但是，如果配置发送电子邮件通知健康警报标识该测试过程，此警报包含在测试电子邮件中。  
  
### <a name="configuring-smtp-on-your-server-to-send-health-reports-in-windows-server-essentials"></a>若要在 Windows Server Essentials 发送健康报告你服务器上配置 SMTP  
 此部分中讨论了如何配置为服务器 SMTP 设置，以便你可以接收通过电子邮件的运行状况报告。  
  
> [!NOTE]
>  默认情况下，健康报告加载项集成与 Windows Server Essentials 或 Windows Server 2012 R2 与 Windows Server Essentials 体验角色安装，并且运行状况报告将显示在**健康报告**仪表板 s 的选项卡**家庭**页面。  
  
##### <a name="to-set-up-email-notification-for-health-reports"></a>若要设置的运行状况报告电子邮件通知  
  
1.  从**仪表板**，单击**报告**选项卡。  
  
2.  在**健康报告任务**任务窗格中，单击**自定义健康报告设置**。  
  
3.  在**自定义健康报告设置**窗口中，单击**计划和电子邮件**选项卡。  
  
4.  在**计划和电子邮件**选项卡上，在**电子邮件**部分中，请执行以下操作：  
  
    1.  单击**启用**，并从报告要发送健康使用的电子邮件地址的类型。 此电子邮件地址将显示为通过电子邮件发送健康报告发件人的地址。  
  
        1.  对于**SMTP 服务器名称**，键入 SMTP 服务器的名称。 （参见表 1 有关的某些 SMTP 服务器名称列表）。  
  
        2.  对于**SMTP 端口**，键入端口号 SMTP 服务器使用发送和接收电子邮件。 （参见第 1 表所使用的某些 SMTP 服务器端口号）。  
  
        3.  选择**此服务器要求安全 (SSL) 连接**如果 SMTP server 使用 SSL （参见第 1 表）。  
  
        4.  选择**此服务器需要验证**SMTP 服务器是否需要用户名和密码信息 （参见第 1 表）。 如果你选中此复选框、 键入的用户名和密码中所输入你的电子邮件地址**从电子邮件地址**在步骤 4a 字段，然后单击**确定**。  
  
            > [!NOTE]
            >  你可以从你的 Internet 服务提供商获取关于 SMTP 服务器名称、 端口号和 SSL 使用情况信息。  
  
            > [!NOTE]
            >  Microsoft 强烈建议你使用 SSL，因为该报告可能包含恶意方可用于检测到漏洞的服务器状态 (例如： 缺少 windows 更新)。 启用 SSL 将加密公交中的数据，并减少公开服务器漏洞的风险。  
  
5.  启用电子邮件、 后**更改 SMTP 设置**链接将显示。 此外新任务，**电子邮件健康报告**，获取显示**健康报告任务**。  
  
     **表 1**示例的 SMTP 服务器名称、 身份验证和 SSL 加密要求端口号  
  
    |SMTP 服务器|所需的 SSL|所需的身份验证|端口号|帐户名称登录/名|  
    |-----------------|------------------|-----------------------------|-----------------|------------------------------|  
    |smtp.gmail.com|是的|是的|587|身份验证的域名称和密码提供完整的电子邮件地址。|  
    |smtp.live.com|是的|是的|587|身份验证的域名称和密码提供完整的电子邮件地址。|  
    |smtp.comcast.net|是的|不|587|身份验证的域名称和密码提供完整的电子邮件地址。|  
    |smtp.mail.yahoo.com|不|是的|25|提供的电子邮件地址将没有域名称的用户名。|  
  
6.  在**自定义健康报告设置**，对于**自动将健康报告发送给以下电子邮件收件人：**，键入你想要通过电子邮件接收健康报告的人员的电子邮件地址。 确保你分号 （;） 的分隔每个电子邮件地址。  
  
7.  以验证你是否配置 SMTP 服务器设置正确仪表板上的 Helath 报告选项卡发送健康报告通过电子邮件，选择报告，然后单击**电子邮件健康报告**从任务窗格。  
  
##  <a name="BKMK_Potential"></a>潜在计算机警报  
 本部分讨论了解和管理特定于您的计算机已连接到服务器和出现在你的计算机启动栏中的通知。  
  
 下表列出了一些可生成和显示上警报查看它们是否适用于你的计算机的计算机警报。  
  
|通知标题|通知的影响和分辨率|  
|-----------------|---------------------------------|  
|网络防火墙当前状态提供减少的保护此计算机。|未经授权的人员或软件可能能够访问该计算机，如果未打开 Windows 防火墙。|  
|病毒保护处于关闭状态，不安装，或者未保持最新状态。|在你的计算机上数据受到威胁如果**病毒防护**安全设置已关闭或不会更新。 [为了保护你的计算机](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect)，按照提示的步骤操作。|  
|间谍软件和垃圾的软件保护处于关闭状态，不安装，或者未保持最新状态。|在你的计算机上数据受到威胁如果**间谍软件和不需要的软件防护**关闭或不会更新。 [为了保护你的计算机](Manage-System-Health-in-Windows-Server-Essentials.md#BKMK_Protect)，按照提示的步骤操作。|  
|Windows 更新处于关闭状态。|你将不能从中受益更新的新的和更正功能，除非 Windows 更新已打开。 若要打开 Windows 更新，在通知查看器中，单击**打开 Windows 更新**。<br /><br /> 如果**打开 Windows 更新**任务未显示，您不登录到警报的计算机。 必须登录到计算机的警报通知查看器中运行此任务中。|  
|应安装重要更新。|你将不能从中受益更新的新的和更正功能，除非 Windows 更新已打开。 若要打开 Windows 更新，在通知查看器中，单击**打开 Windows 更新**。<br /><br /> 如果**打开 Windows 更新**任务未显示，您不登录到警报的计算机。 必须登录到计算机的警报通知查看器中运行此任务中。|  
|重新启动以应用更新。|你将无法受益于这些更新的新的和更正功能，直到它们的应用。 保存你的所有数据并重新启动以应用更新的计算机。|  
|硬盘驱动器可用空间不足。|如果不是可用空间，你将不能用于保存的其他信息。 为了提高计算机上的可用空间，请考虑以下选项：<br /><br /> 添加新的硬盘。<br /><br /> -运行**磁盘清理**删除旧和临时文件。<br /><br /> -你将文件移动到共享文件夹另一台计算机上。<br /><br /> -存档可移动媒体，例如 CD、 DVD 或外部硬盘上的文件。|  
|**文件历史记录**代理服务器上的未正确配置为在此计算机上运行。|不能创建文件历史记录备份。|  
|一个或多个服务都不运行。||  
|更改你的 Windows 密码。||  
|你的 Microsoft Office 365 密码不是你的 Windows 密码相同。||  
  
###  <a name="BKMK_Protect"></a>为了保护你的计算机  
  
1.  打开安全中心。  
  
2.  确定已安装的病毒保护状态。  
  
3.  执行以下的任务，具体取决于保护状态之一：  
  
    -   如果它未启用、 启用它。  
  
    -   如果不是最新状态，完成更新签名的过程。  
  
    -   如果未安装病毒保护，请考虑安装它。  
  
## <a name="see-also"></a>请参阅  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)