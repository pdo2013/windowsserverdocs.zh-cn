---
title: "对于 Windows Server Essentials migration1 准备源服务器"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="prepare-your-source-server-for-windows-server-essentials-migration1"></a>对于 Windows Server Essentials migration1 准备源服务器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

完成以下预备的步骤，以确保该设置和源服务器上的数据成功迁移到目标服务器。  
  
#### <a name="to-prepare-for-migration"></a>若要准备迁移  
  

1.  [备份你的源服务器](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [安装最新的 service pack](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [评估源服务器的运行状况](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [运行源服务器上的迁移准备工具](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [创建计划迁移业务线应用程序](Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

1.  [备份你的源服务器](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_BackUpYourSourceServerToPrepareForMigration)  
  
2.  [安装最新的 service pack](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_InstallTheMostRecentServicePacksToPrepareForMigration)  
  
3.  [评估源服务器的运行状况](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_UseWindowsBestPracticeAnalyzer)  
  
4.  [运行源服务器上的迁移准备工具](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_MPT)  
  
5.  [创建计划迁移业务线应用程序](../migrate/Prepare-your-Source-Server-for-Windows-Server-Essentials-migration.md#BKMK_PlanToMigrateLineOfBusinessApplications)  

  
###  <a name="BKMK_BackUpYourSourceServerToPrepareForMigration"></a>备份你的源服务器  
 迁移进程在开始之前备份源代码服务器。 使出错无法恢复时迁移期间出现意外丢失保护你的数据的备份可以帮助。  
  
##### <a name="to-back-up-the-source-server"></a>若要备份源服务器  
  
1.  执行源代码服务器完整备份。 有关 Windows 小型企业服务器 2011 年软件包备份的详细信息，请参阅[了解更多有关设置服务器备份](https://technet.microsoft.com/library/server-backup-support-1.aspx)。  
  
2.  验证已成功运行备份。 要测试的备份完整性，请从你的备份选择随机文件、 将其还原到另一个位置，然后确认还原的文件是原始文件一样。  
  
###  <a name="BKMK_InstallTheMostRecentServicePacksToPrepareForMigration"></a>安装最新的 service pack  
 在前迁移源服务器上必须安装最新的更新和 service pack。  
  
###  <a name="BKMK_UseWindowsBestPracticeAnalyzer"></a>评估源服务器的运行状况  
 请务必开始迁移之前评估源服务器的运行状况。 使用以下步骤确保这些更新是最新，以生成系统运行状况报告，并运行 Windows Server 解决方案最佳实践分析器 (BPA)。  
  
#### <a name="download-and-install-critical-and-security-updates"></a>下载和安装关键安全更新  
 安装关键和安全更新有助于确保迁移将成功，并且帮助保护你的网络，迁移过程源服务器上。  
  
###### <a name="to-check-for-the-latest-updates"></a>若要检查有最新的更新  
  
1.  从源服务器上，单击**开始**，单击**所有程序**，然后单击**Windows 更新**。  
  
2.  单击**检查更新**。  
  
3.  如果找到更新，请单击**安装更新**。  
  
#### <a name="check-the-alert-viewer-for-critical-errors"></a>检查有严重错误警报查看器  
 你可以检查有任何重要错误仪表板上的警报查看器。  
  
#### <a name="run-the-windows-server-solutions-best-practices-analyzer"></a>运行 Windows Server 解决方案的最佳实践分析  
 你可以运行 Windows Server 解决方案最佳做法分析器 (BPA) 以验证开始迁移过程之前有域、服务器或网络上的任何问题。 BPA 收集来源以下配置的信息：  
  
-   Active Directory® Windows 管理 Instrumentation (WMI)  
  
-   注册表  
  
-   Internet 信息服务 (IIS) 配置数据库  
  
###### <a name="to-use-the-windows-server-solutions-bpa-to-analyze-your-source-server"></a>若要使用 Windows Server 解决方案 BPA 分析源服务器  
  
1.  下载并安装[Windows Server 解决方案最佳实践分析](https://www.microsoft.com/en-us/download/details.aspx?id=15556)在 Microsoft 下载中心提供。  
  
2.  在下载完成后，单击**开始**，指向**所有程序**，然后单击**SBS 最佳实践分析工具**。  
  
    > [!NOTE]
    >  扫描服务器之前，请检查更新。  
  
3.  在导航窗格中，单击**开始扫描**。  
  
4.  详细信息窗格中，键入扫描标签，然后单击**开始扫描**。 扫描标签是扫描报告的名称，例如，**SBS BPA 扫描 2012 年 7 月 1**。  
  
5.  扫描完成后，单击**查看此最佳做法扫描的报告**。  
  
 后收集关于 server 配置的信息，Windows Server 解决方案 BPA 验证信息是正确且管理员然后提供的信息和问题按严重性排序列表。 列表中介绍了每个问题，并提供建议，或者可能的解决方案。 三种的报告类型均可用：  
  
|报告类型|描述|  
|-----------------|-----------------|  
|列表报表|维列表中显示的报告。|  
|树报告|在列表中分层显示报告。|  
|其他报告|显示报告，如运行时间日志。|  
  
 若要查看说明以及问题的解决方案，请单击报告中的问题。 并非所有 Windows SBS 2011 Essentials bpa 报告的问题会影响迁移，但你应，以确保成功迁移解决尽可能多的问题。  
  
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
  
###  <a name="BKMK_MPT"></a>运行源服务器上的迁移准备工具  
 你无法执行迁移模式安装，而无需先源服务器上运行迁移准备工具。 此工具旨在准备你的源 Server 和域迁移到 Windows Server Essentials。  
  
> [!IMPORTANT]
>  运行迁移准备工具之前，请备份你的源服务器。 是不可逆的迁移准备工具到架构所做的所有更改。 迁移期间遇到了问题，请的唯一方法源服务器回到状态运行该工具前服务器从备份进行还原系统。  
  
 若要运行迁移准备工具，你必须的企业管理员组、架构管理员组中，并域管理员组成员。  
  
##### <a name="to-verify-that-you-have-the-appropriate-permissions-to-run-the-tool-on-the-source-server"></a>若要验证你有源服务器上运行该工具相应的权限  
  
1.  在源服务器上，单击**开始**，单击**管理工具**，然后单击**Active Directory 用户和计算机**。  
  
2.  控制台树中，单击以展开你域，，然后单击**用户**。  
  
3.  右键单击的管理员帐户，你正在使用的迁移，然后单击**属性**。  
  
4.  单击**隶属于**选项卡，然后再验证列入企业管理员架构管理员，域管理员**的成员**文本框。  
  
5.  如果未列出组，请单击**添加**，然后添加未列出的每组。  
  
    > [!NOTE]
    >  -   如果 Netlogon 服务未启动，你可能会收到一个权限错误。  
    > -   你必须注销并重新登录以使更改生效服务器。  
  
     你可以使用最新版本的 Windows 更新代理以确保服务器更新进程的工作正常。  
  
 你可以使用最新版本的 Windows 更新代理以确保服务器更新进程的工作正常。  
  
 源服务器上安装 Windows 更新代理之前，必须首先安装 Windows PowerShell 2.0 和 Microsoft 基准配置分析器 2.0。  
  
-   若要下载和安装 Windows PowerShell 2.0，请参阅[文章 968929](https://go.microsoft.com/fwlink/p/?LinkId=241483) Microsoft 知识库文章中。  
  
-   若要下载并安装 Microsoft 基准配置分析器 2.0，请参阅[Microsoft 基准配置分析器 2.0](https://go.microsoft.com/fwlink/p/?LinkId=241484)在 Microsoft 下载中心提供。  
  
-   若要下载和安装最新版本的 Windows 更新代理，请参阅[文章 949104](https://go.microsoft.com/fwlink/p/?LinkId=237493) Microsoft 知识库文章中。  
  
##### <a name="to-install-and-run-the-migration-preparation-tool-on-the-source-server"></a>若要安装和运行源服务器上的迁移准备工具  
  
1.  插入 Windows Server Essentials DVD1 源服务器上的 DVD 驱动器中。  
  
2.  打开 Windows 资源管理器中，浏览到**\support\tools**文件夹的 DVD，然后双击**sourcetool.msi**文件。  
  
    > [!NOTE]
    >  -   如果在服务器上已安装迁移准备工具，运行从该工具**开始**菜单。  
    > -   若要确保准备最佳可能迁移体验，建议你始终选择要安装最新的更新。  
  
     该向导将迁移准备工具安装源服务器上。 安装完成后，迁移准备工具会自动运行，并安装最新的更新。  
  
3.  在迁移准备工具中，选择**我具有备份，并且已准备好继续**，然后单击**下一步**。  
  
    > [!WARNING]
    >  如果你收到一条修补程序安装到相关的错误消息，请参阅方法 2：重命名 Catroot2 文件夹中[文章 822798](https://go.microsoft.com/FWLink/p/?LinkID=118672) Microsoft 知识库文章中。  
  
     迁移准备工具扩展 Active Directory 架构将迁移源域操作会准备。 在完成任务后，单击**下一步**以继续。  
  
4.  在准备好源域后, 迁移准备工具扫描源服务器找出潜在问题两种类型。  
  
    -   **错误**源服务器，可以阻止迁移或导致失败迁移上发现的问题。 按照错误消息，以解决问题，然后单击中的说明进行操作**再次扫描**。  
  
    -   **警告**迁移过程中可能会导致正常工作的问题源服务器上发现的问题。 强烈建议你按照修复之前迁移问题的错误消息中的说明进行操作。  
  
     修复，或认可的所有问题后，单击**下一步**。  
  
5.  在迁移准备工具中，单击**完成**。  
  
6.  迁移准备工具完成后，系统可能提示你重新开始进行迁移到 Windows Server Essentials 之前启动源服务器。  
  
> [!NOTE]
>  你必须先完成迁移准备工具源服务器上成功运行目标服务器上安装 Windows Server Essentials 两个星期内。 否则，将阻止安装时，Windows Server Essentials 目标服务器上。 如果发生这种情况，你必须重新源服务器上运行迁移准备工具。  
  
###  <a name="BKMK_PlanToMigrateLineOfBusinessApplications"></a>创建计划迁移业务线应用程序  
 业务线 (LOB) 应用程序是一个重要计算机应用至关重要的业务运营。 LOB 应用包括会计、 供应链管理和资源规划的应用程序。  
  
 当你计划迁移 LOB 应用时，请咨询 LOB 应用提供商合作，以确定适当迁移每个应用程序的方法。 必须找到用于目的地服务器上安装 LOB 应用程序的媒体。  
  
> [!NOTE]
>  如果你使用 Windows 小型企业服务器 2011 Essentials SDK 开发自定义的系统运行状况或警报接程序，并且你想要继续使用 Windows Server Essentials 外接程序，您还必须更新外接程序，并将其部署到目标服务器上。  
  
