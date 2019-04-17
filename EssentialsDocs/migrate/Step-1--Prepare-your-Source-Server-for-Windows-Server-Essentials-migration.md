---
title: "第 1 步： 准备 Windows Server Essentials 的源 Server 迁移"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 244c8a06-04c6-4863-8b52-974786455373
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 2efb1badde6d0ca11bc3b7526fdfb377d9f95d3f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="step-1-prepare-your-source-server-for-windows-server-essentials-migration"></a>第 1 步： 准备 Windows Server Essentials 的源 Server 迁移

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

本主题介绍如何备份源服务器上，评估源服务器系统运行状况，安装最新的 service pack 和修复程序，并验证网络配置。  
  
## <a name="to-prepare-for-migration"></a>若要准备迁移  
 完成以下预备的步骤，以确保该设置和源服务器上的数据成功迁移到目标服务器。  
  
1.  [备份你的源服务器](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [安装最新的 service pack](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [删除日志服务帐户设置为](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_DeleteSvcAcctSetting)  
  
4.  [评估源服务器的运行状况](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_EvaluateHealth)  
  
5.  [创建计划迁移业务线应用程序](Step-1--Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MigrateLOB)  
  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>备份你的源服务器  
 迁移进程在开始之前备份源代码服务器。 使出错无法恢复时迁移期间出现意外丢失保护你的数据的备份可以帮助。  
  
##### <a name="to-back-up-the-source-server"></a>若要备份源服务器  
  
1.  使用下表中的资源之一指导您执行源代码服务器完整备份。  
  
2.  验证已成功运行备份。 要测试的备份完整性，请从你的备份选择随机文件、 将其还原到另一个位置，然后确认还原的文件是原始文件一样。  
  
   |产品|资源|
   |---|---|
   |Windows 小型企业 Server 2003|[备份和还原 Windows 小型企业 Server 2003](https://msdn.microsoft.com/library/cc875809.aspx) 
   |Windows 小型企业 Server 2008|[备份和还原 Windows 小型企业服务器 2008年上的数据](https://technet.microsoft.com/library/cc527505\(WS.10\).aspx)
   |Windows Server 2008 基础|[备份和恢复](https://technet.microsoft.com/library/cc754097\(WS.10\).aspx)  
   |Windows 小型企业服务器 2011年软件包|[了解有关服务器备份设置的详细信息](https://technet.microsoft.com/library/server-backup-support-1.aspx)
   |Windows 小型企业服务器 2011年标准|[管理服务器备份](https://technet.microsoft.com/library/cc527488.aspx)  
   |Windows Server Essentials|[管理备份和还原在 Windows Server Essentials](https://technet.microsoft.com/library/jj713536.aspx)

###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>安装最新的 service pack  
 在前迁移源服务器上必须安装最新的更新和 service pack。  
  
###  <a name="BKMK_DeleteSvcAcctSetting"></a>删除日志服务帐户设置为  
 如果你从 Windows 小型企业 Server 2003 或 Windows Server 2003 迁移的删除**登录作为一项服务**帐户从组策略设置。  
  
##### <a name="to-delete-the-log-on-as-a-service-account-setting"></a>若要删除日志服务帐户设置为  
  
1.  若要打开**组策略管理**工具，单击**开始**，单击**控制面板**，单击**管理工具**，然后单击**组策略管理**。  
  
2.  右键单击**默认域控制器策略**，然后单击**编辑**。  
  
3.  导航到**计算机配置 windows 设置本地策略用户权限分配**。  
  
4.  在详细信息窗格中，双击**登录作为一项服务**。  
  
5.  清除**定义这些策略设置**复选框。  
  
6.  删除 \\\localhost\SYSVOL\\ < domainname\ > \scripts\SBS_LOGIN_SCRIPT.bat。  
  
###  <a name="BKMK_EvaluateHealth"></a>评估源服务器的运行状况  
 请务必开始迁移之前评估源服务器的运行状况。 使用以下步骤确保这些更新是最新，以生成系统运行状况报告，并运行 Windows Server 解决方案最佳实践分析器 (BPA)。  
  
#### <a name="download-and-install-critical-and-security-updates"></a>下载和安装关键安全更新  
 安装关键和安全更新有助于确保迁移将成功，并且帮助保护你的网络，迁移过程源服务器上。  
  
###### <a name="to-check-for-the-latest-updates"></a>若要检查有最新的更新  
  
1.  从源服务器上，单击**开始**，单击**所有程序**，然后单击**Windows 更新**。  
  
2.  单击**检查更新**。  
  
3.  如果找到更新，请单击**安装更新**。  
  
#### <a name="run-the-best-practices-analyzer"></a>运行最佳实践分析  
 你可以运行验证开始迁移进程之前没有任何问题域、 服务器或网络中，最佳做法分析器 (BPA)。 BPA 收集来源以下配置的信息：  
  
-   Active Directory 的 Windows 管理 Instrumentation (WMI)  
  
-   注册表  
  
-   Internet 信息服务 (IIS)  
  
###### <a name="to-use-the-bpa-to-analyze-your-source-server"></a>若要使用 BPA 分析源服务器  
  
1.  下表指向 Microsoft 下载中心，你可以在此处下载并安装的源代码服务器的最佳实践分析器 (BPA) 提供的链接。  
  
   |如果运行的你的源服务器|你可以从 BPA 工具|
   |---|---|
   |Windows SBS 2003|[Microsoft Windows 小型企业 Server 2003 最佳做法分析工具网站](https://www.microsoft.com/download/details.aspx?id=5334)
   |Windows SBS 2008|[Microsoft Windows 小型企业服务器 2008年最佳做法分析器网站](https://www.microsoft.com/download/details.aspx?id=6231)  
   |Windows 要或 Windows SBS 2011 标准|[Windows Server 解决方案最佳实践分析网站](https://www.microsoft.com/download/details.aspx?id=15556) 
   |Windows Server Essentials 或 Windows Server 2012|服务器仪表板  
  
2.  在下载完成后，单击**开始**，指向**所有程序**，然后单击**SBS 最佳实践分析工具**。  
  
    > [!NOTE]
    >  扫描服务器之前，请检查更新。  
  
3.  在导航窗格中，单击**开始扫描**。  
  
     如果你的源服务器运行的 Windows Server Essentials，请执行以下操作：  
  
    1.  登录到目标服务器以 administrator 身份，，然后打开仪表板。  
  
    2.  在仪表板中，单击**设备**选项卡。  
  
    3.  在 <**服务器** >**任务**窗格中，单击**最佳实践分析**。  
  
4.  详细信息窗格中，键入扫描标签，然后单击**开始扫描**。 扫描标签是扫描报告的名称，例如， **SBS BPA 扫描 2013 年 7 月 1**。  
  
5.  扫描完成后，单击**查看此最佳做法扫描的报告**。  
  
 BPA 工具会收集有关服务器配置信息后，它可验证的信息是正确和管理员然后提供的信息和问题按严重性排序列表。 列表中介绍了每个问题，并提供建议，或者可能的解决方案。 三种的报告类型均可用：  
  
|报告类型|描述
|-----------------|----------------- 
|列表报表|维列表中显示的报告。 
|树报告|在列表中分层显示报告。

若要查看说明以及问题的解决方案，请单击报告中的问题。 并非所有 BPA 工具报告的问题会影响迁移，但你应该可以解决尽可能多的尽可能以确保成功迁移问题。  
  
####  <a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a>使用外部时间源同步源服务器时间  
 源服务器上的时间必须 5 分钟的目标服务器上的时间内设置为，并且日期和时区必须相同两个服务器上。 如果源服务器在虚拟机上运行，日期、 时间和主服务器上的时区必须匹配的源服务器和目的地的服务器。 为了帮助确保成功安装了 Windows Server Essentials，必须同步到网络时协议 (NTP) 服务器，在 Internet 上的源服务器时间。  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>若要同步源服务器时间与 NTP 服务器  
  
1.  登录到提供了域管理员帐户和密码源服务器。  
  
2.  单击**开始**，单击**运行**，类型**cmd**在文本框中并按 ENTER。  
  
3.  在命令提示符下，键入 w32tm /config /syncfromflags:domhier / 可靠： 没有/更新，并按 ENTER。  
  
4.  在命令提示符下，键入网络停止 w32time，然后按 ENTER。  
  
5.  在命令提示符下，键入网络开始 w32time，然后按 ENTER。  
  
> [!IMPORTANT]
>  Windows Server Essentials 安装期间，你有机会来验证目标服务器上的时间，并更改它，如有必要。 确保在 5 分钟源服务器上设置的时间内的时间。 安装完成后，NTP 目标服务器同步。 所有已加入域的计算机，包括源服务器，同步到目的地服务器，担任主域控制器 (PDC) 仿真器母版。  
  
###  <a name="BKMK_MigrateLOB"></a>创建计划迁移业务线应用程序  
 业务线 (LOB) 应用程序是一个重要计算机应用至关重要的业务运营。 LOB 应用包括会计、 供应链管理和资源规划的应用程序。  
  
 当你计划迁移 LOB 应用时，请咨询 LOB 应用提供商合作，以确定适当迁移每个应用程序的方法。 必须找到用于目的地服务器上安装 LOB 应用程序的媒体。  
  
> [!NOTE]
>  如果你使用 Windows 小型企业服务器 2011 Essentials SDK 开发自定义的系统运行状况或警报接程序，并且你想要继续使用 Windows Server Essentials 外接程序，您还必须更新外接程序，并将其部署到目标服务器上。  
  
  
### <a name="create-a-plan-to-migrate-email-hosted-on-windows-sbs-2011-windows-sbs-2008-and-windows-sbs-2003"></a>创建计划迁移 Windows SBS 2011、 Windows SBS 2008 和 Windows SBS 2003 上托管的电子邮件  
 在 Windows SBS 2011、 Windows SBS 2008 和 Windows SBS 2003，通过 Microsoft Exchange Server 提供电子邮件。 但是，Windows Server Essentials 不提供收件箱电子邮件服务。 如果你当前正在使用运行 Windows SBS 2011、 Windows SBS 2008 或 Windows SBS 2003 服务器拥有你的公司 s 电子邮件，你将需要迁移到备用本地或托管的解决方案。  
  
> [!NOTE]
>  更新，并且你的源服务器准备迁移后，我们建议你创建的备份更新服务器，才能继续迁移过程。  
  
#### <a name="migrate-email-to-microsoft-office-365"></a>将电子邮件迁移到 Microsoft Office 365  
 如果您已选择将您的域用作电子邮件解决方案 Microsoft Office 365、 遵循的指南中[与切换 Exchange 迁移 cloud 迁移所有邮箱](http://help.outlook.com/140/ms.exch.ecp.emailmigrationwizardexchangelearnmore.aspx)以启动到 Office 365 的电子邮件迁移。 我们建议你在安装 Windows Server Essentials 之前完成电子邮件迁移。  
  
> [!NOTE]
>  删除源服务器上的本地 Exchange Server 步骤就是必需的如果你想要与 Office 365 集成 Windows Server Essentials。 有关如何迁移到 Office 365 Exchange Server 公共文件夹中的信息，请参阅博客文章[Office 365 的 Microsoft Exchange 2013 公共文件夹迁移脚本](http://blogs.technet.com/b/fmustafa/archive/2013/04/11/microsoft-exchange-2013-public-folders-migration-scripts-for-office-365.aspx)。  
>   
>  完成安装后，你应打开 Windows Server Essentials 中的 Office 365 集成功能通过运行**使用 Microsoft Office 365 集成**任务。  
  
> [!IMPORTANT]
>  若要允许连接到 Exchange 服务器在源代码服务器上运行的 Office 365 迁移工具，你必须启用 RPC 通过 HTTP 源服务器上。 有关如何通过 HTTP 启用 RPC 的信息，请参阅[如何通过小型企业 Server 2003 （标准或高级） 中首次 HTTP 的部署 RPC](https://technet.microsoft.com/library/bb123622%28EXCHG.65%29.aspx)。 如果您通过 HTTP 启用 RPC 后，不能成功运行 Office 365 迁移工具，请查看**ValidPorts**设置在注册表中 HKEY_LOCAL_MACHINE\Software\Microsoft\Rpc\RpcProxy，并确保已列出了源代码服务器的完整的域名 (FQDN)。 如果未列出 FQDN，将其添加手动使用下面的示例：  
>   
>  远程。 *contoso*.com:6001-6002; 远程。 *contoso*.com:6004 (替换*contoso*与你的域名)。  
  
#### <a name="migrate-email-to-another-on-premises-exchange-server"></a>向其他本地 Exchange Server 迁移电子邮件  
 了解如何进行迁移到另一个电子邮件本地 Exchange 服务器，请参阅[与 Windows Server Essentials 集成 On 本地 Exchange Server](https://technet.microsoft.com/library/jj200172.aspx)。 我们建议设置新的本地 Exchange Server 后安装 Windows Server Essentials，然后完成的电子邮件之前降级源 Server 迁移。  
  
> [!NOTE]
>  Windows 小型企业服务器 POP3 接头未附带 Exchange Server。 你将电子邮件数据迁移到另一个 Exchange Server 后，你可以不再使用 POP3 接头功能。  
  
> [!NOTE]
>  更新，并且你的源服务器准备迁移后，你应该继续迁移进程之前创建更新服务器的备份。  
  
## <a name="next-steps"></a>后续步骤  
 已准备好迁移到 Windows Server Essentials 源服务器。  现在转到[第 2 步： 为新的副本域控制器安装 Windows Server Essentials](Step-2--Install-Windows-Server-Essentials-as-a-new-replica-domain-controller.md)。  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

