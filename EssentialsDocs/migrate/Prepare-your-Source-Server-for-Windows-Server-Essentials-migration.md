---
title: 适用于 Windows Server Essentials 中准备好源服务器 migration1
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5861ae9-77cb-4d37-b4c5-8f0757213385
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 929c7506c78667646e429c4f28df7e5642c575ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841148"
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>适用于 Windows Server Essentials 中准备好源服务器 migration1

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

完成以下初步步骤，以确保将源服务器上的设置和数据成功迁移到目标服务器。  
  
#### <a name="to-prepare-for-migration"></a>准备迁移的步骤  
  

1.  [备份源服务器](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [安装最新 service pack](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [评估源服务器的运行状况](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [在源服务器上运行迁移准备工具](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [创建计划以迁移业务线应用程序](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [备份源服务器](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [安装最新 service pack](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [评估源服务器的运行状况](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [在源服务器上运行迁移准备工具](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [创建计划以迁移业务线应用程序](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a> 备份源服务器  
 在开始迁移过程之前，请备份源服务器。 进行备份有助于保护你的数据在迁移过程中出现不可恢复错误的情况下不意外丢失。  
  
##### <a name="to-back-up-the-source-server"></a>备份源服务器  
  
1.  对源服务器执行完全备份。 有关备份 Windows Small Business Server 2011 Essentials 的详细信息，请参阅[了解有关设置服务器备份的详细信息](https://technet.microsoft.com/library/server-backup-support-1.aspx)。  
  
2.  验证备份是否成功运行。 要测试备份的完整性，请从备份中选择随机文件，将它们还原到备用位置，然后确认还原后的文件是否与原始文件相同。  
  
###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a> 安装最新 service pack  
 在迁移之前，必须在源服务器上安装最新更新和 Service Pack。  
  
###  <a name="BKMK_UseWindowsBestPracticeAnalyzer"></a> 评估源服务器的运行状况  
 请务必在开始迁移之前评估源服务器的运行状况。 使用以下过程以确保更新为最新版本、生成系统运行状况报告，并运行 Windows Server 解决方案最佳做法分析器 (BPA)。  
  
#### <a name="download-and-install-critical-and-security-updates"></a>下载并安装重要更新和安全更新  
 在源服务器上安装重要更新和安全更新有助于确保迁移成功，而且有助于在迁移过程中保护你的网络。  
  
###### <a name="to-check-for-the-latest-updates"></a>检查最新更新  
  
1.  在源服务器上，依次单击“开始” 、“所有程序” ，然后单击“Windows Update” 。  
  
2.  单击“检查更新” 。  
  
3.  如果找到更新，则请单击“安装更新”。  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>检查警报查看器中的关键错误  
 你可以检查仪表板上的警报查看器以查看任何关键错误。  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>运行 Windows Server 解决方案最佳做法分析器  
 可以在开始执行迁移过程之前，运行 Windows Server 解决方案最佳做法分析器 (BPA) 来验证服务器、网络或域上不存在任何问题。 BPA 从以下来源收集配置信息：  
  
-   Active Directory® Windows Management Instrumentation (WMI)  
  
-   注册表  
  
-   Internet Information Services (IIS) 元数据库  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>使用 Windows Server 解决方案 BPA 分析源服务器  
  
1.  在 Microsoft 下载中心下载并安装 [Windows Server 解决方案最佳做法分析器](https://www.microsoft.com/en-us/download/details.aspx?id=15556)。  
  
2.  下载完毕后，单击“开始”、指向“所有程序”，然后单击“SBS 最佳做法分析器工具”。  
  
    > [!NOTE]
    >  在扫描服务器之前检查更新。  
  
3.  在导航窗格中，单击“启动扫描”。  
  
4.  在详细信息窗格中，键入扫描标签，然后单击“开始扫描” 。 扫描标签是指扫描报告名称，例如 **SBS BPA Scan 1Jul2012**。  
  
5.  扫描完毕后，单击“查看该最佳实践扫描的报告”。  
  
 收集有关服务器配置信息之后，Windows Server 解决方案 BPA 验证信息正确，然后向管理员呈现一个按严重性排序的信息和问题列表。 该列表描述每个问题并提供建议或可能的解决方案。 可以使用三种报告类型：  
  
|报告类型|描述|  
|-----------------|-----------------|  
|列表报告|在一维列表中显示报告。|  
|目录树报告|在分层列表中显示报告。|  
|其他报告|显示运行时日志等报告。|  
  
 要查看某个问题的描述和解决方案，请在报告中单击该问题。 并非所有由 Windows SBS 2011 Essentials BPA 报告的问题会影响迁移，但应尽可能多的问题解决，以确保迁移成功。  
  
####  <a name="BKMK_SynchronizeTheSourceServerTimeWithAnExternalTimeSource"></a> 使源服务器时间与外部时间源同步  
 源服务器上的时间与目标服务器上的时间差异必须设置为在 5 分钟之内，并且两个服务器上的日期和时区必须相同。 如果源服务器正在虚拟机中运行，则主机服务器上的日期、时间和时区必须与源服务器和目标服务器上日期、时间和时区匹配。 若要帮助确保成功安装 Windows Server Essentials，必须同步到 Internet 上的网络时间协议 (NTP) 服务器的源服务器时间。  
  
###### <a name="to-synchronize-the-source-server-time-with-the-ntp-server"></a>将源服务器时间与 NTP 服务器同步的步骤  
  
1.  使用域管理员帐户和密码登录源服务器。  
  
2.  单击**启动**，单击**运行**，类型**cmd**中文本框中，然后按 ENTER。  
  
3.  在命令提示符处，键入 w32tm /config /syncfromflags: / 可靠： 没有/更新，并按 ENTER。  
  
4.  在命令提示符处，键入 net stop w32time，然后按 ENTER。  
  
5.  在命令提示符处，键入 net start w32time，然后按 ENTER。  
  
> [!IMPORTANT]
>  在 Windows Server Essentials 安装时，您有机会可以验证目标服务器上的时间并更改它，如有必要。 确保目标服务器时间与源服务器上设置的时间的差距在五分钟之内。 当安装完成后，目标服务器将与 NTP 同步。 所有加入域的计算机（包括源服务器）均与目标服务器同步，目标服务器承担了主域控制器 (PDC) 仿真器主机的角色。  
  
###  <a name="BKMK_MPT"></a> 在源服务器上运行迁移准备工具  
 如果不先在源服务器上运行迁移准备工具，则无法执行迁移模式安装。 此工具旨在准备要迁移到 Windows Server Essentials 在源服务器和域。  
  
> [!IMPORTANT]
>  运行迁移准备工具之前，备份源服务器。 迁移准备工具对架构执行的所有更改都不可恢复。 如果在迁移期间遇到问题，将源服务器还原到运行此工具之前的状态的唯一办法是从系统备份还原服务器。  
  
 要运行迁移准备工具，必须是 Enterprise Admins 组、Schema Admins 组和 Domain Admins 组的成员。  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>验证你是否拥有在源服务器上运行此工具的适当权限  
  
1.  在源服务器上，依次单击 **“开始”**、**“管理工具”**，然后单击 **“Active Directory 用户和计算机”**。  
  
2.  在控制台树中，单击以展开域，然后单击“用户”。  
  
3.  右键单击正用于迁移的管理员帐户，然后单击 **“属性”**。  
  
4.  单击 **“隶属于”** 选项卡，然后验证 Enterprise Admins、Schema Admins 和 Domain Admins 是否在 **“隶属于”** 文本框中列出。  
  
5.  如果没有列出这些组，请单击 **“添加”**，然后添加未列出的每个组。  
  
    > [!NOTE]
    >  -   如果 Netlogon 服务未启动，则可能会收到权限错误。  
    > -   你必须注销然后登录回服务器才能使所做更改生效。  
  
     可以使用 Windows 更新代理的最新版本确保服务器更新过程正常运行。  
  
 可以使用 Windows 更新代理的最新版本确保服务器更新过程正常运行。  
  
 在源服务器上安装 Windows 更新代理之前，必须首先安装 Windows PowerShell 2.0 和 Microsoft Baseline Configuration Analyzer 2.0。  
  
-   若要下载并安装 Windows PowerShell 2.0，请参阅[文章 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) Microsoft 知识库中。  
  
-   若要下载并安装 Microsoft Baseline Configuration Analyzer 2.0，请参阅[Microsoft Baseline Configuration Analyzer 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484)在 Microsoft 下载中心获取。  
  
-   若要下载并安装最新版本的 Windows 更新代理，请参阅 Microsoft 知识库[文章 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493)。  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>在源服务器上安装并运行迁移准备工具  
  
1.  在源服务器上的 DVD 驱动器中插入 Windows Server Essentials DVD1。  
  
2.  打开 Windows 资源管理器，浏览到 DVD 的 **\support\tools** 文件夹，然后双击 **sourcetool.msi** 文件。  
  
    > [!NOTE]
    >  -   如果已在服务器上安装了迁移准备工具，请从“开始”菜单运行此工具。  
    > -   为确保获得最佳的可能迁移体验，建议你始终选择安装最新的更新。  
  
     向导将在源服务器上安装“迁移准备工具”。 安装完成后，“迁移准备工具”将自动运行并安装最新更新。  
  
3.  在“迁移准备工具”中，选择“我已备份并准备好继续”，然后单击“下一步”。  
  
    > [!WARNING]
    >  如果你收到一条与修补程序安装相关的错误消息，请参阅方法 2:重命名 Catroot2 文件夹中[文章 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) Microsoft 知识库中。  
  
     迁移准备工具会通过扩展 Active Directory 架构来准备源域以进行迁移。 在完成任务之后，单击“下一步”以继续。  
  
4.  在准备好源域之后，迁移准备工具会扫描源服务器以确定两种类型的潜在问题。  
  
    -   **错误**可以阻止迁移或导致迁移失败的源服务器上发现的问题。 按照错误消息中的说明修复问题，然后单击“重新扫描”。  
  
    -   **警告**可能会导致功能问题，在迁移期间源服务器上发现的问题。 强烈建议在继续进行迁移之前，按照错误消息中的说明修复问题。  
  
     在修复或确认所有问题之后，单击“下一步”。  
  
5.  在迁移准备工具中，单击“完成”。  
  
6.  迁移准备工具完成时，可能提示您重新启动源服务器，然后才能开始迁移到 Windows Server Essentials。  
  
> [!NOTE]
>  必须完成的迁移准备工具在源服务器上成功运行在目标服务器上安装 Windows Server Essentials 的两周内。 否则，将阻止在目标服务器上的 Windows Server Essentials 的安装。 如果发生这种情况，则必须再次在源服务器上运行迁移准备工具。  
  
###  <a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a> 创建计划以迁移业务线应用程序  
 业务线 (LOB) 应用程序是运转企业的关键计算机应用程序。 LOB 应用程序包括会计、供应链管理和资源计划应用程序。  
  
 在你计划迁移 LOB 应用程序时，请咨询 LOB 应用程序提供商来确定迁移每个应用程序的适当方法。 你还必须找到用于在目标服务器上安装 LOB 应用程序的介质。  
  
> [!NOTE]
>  如果使用 Windows Small Business Server 2011 Essentials SDK 开发自定义的系统运行状况或通知外接程序，并且你想要继续使用外接程序与 Windows Server Essentials，还必须更新外接程序，并将其部署到目标服务器上。  
  
