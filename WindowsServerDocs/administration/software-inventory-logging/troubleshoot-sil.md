---
title: 软件清单日志记录疑难解答
description: 描述如何解决常见的软件清单日志记录部署问题。
ms.custom: na
ms.prod: windows-server
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: fb6e6fbba835e049748ca8578f24a1ff7fc750bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382897"
---
# <a name="troubleshoot-software-inventory-logging"></a>软件清单日志记录疑难解答 

>适用于：Windows Server （半年频道），Windows Server 2019，Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2

## <a name="understanding-sil"></a>了解 SIL

在开始对 SIL 进行故障排除之前，应充分了解其组件及其工作原理。 以下视频概述了 SIL 和 SIL 聚合器，以及如何使用它们来转发和报告清单数据：

1. [软件清单日志记录简介（SIL）（10:57）](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [软件清单日志记录：设置 SIL 聚合器（14:34）](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [软件清单日志记录：启用 SIL 转发（7:20）](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

## <a name="how-sil-data-flow-works"></a>SIL 数据流的工作原理

SIL 框架包含两个主要组件和两个通信通道。 在两个通道之间以及这两个组件之间流动的数据是成功的 SIL 部署所必需的（这假定虚拟化或云环境-纯粹的物理环境只需要一个通信通道）。 你需要了解 SIL 的组件和数据流，以便正确部署。 观看以上概述视频后，你将看到体系结构图表，其中阐释了这两个通道上的组件和数据流。 橙色箭头表示通过 WinRM 进行远程查询，绿色虚线箭头指示从每个 WS 结束节点中的 SIL 到 SIL 聚合器的 HTTPS 发布：

![](../media/software-inventory-logging/image1.png)

如果遇到与 SIL 有关的问题，则很可能与通道和组件之间的数据流中断相关。 下面是与数据流相关的最常见问题，接下来的部分通过故障排除步骤来解决这三个问题：

-   数据流**问题 1** –**使用 SilReport cmdlet 时报表中不存在数据**（或一般缺少数据）。

-   数据流**问题 2** -报表中**未知主机下的服务器太多**。

-   数据流**问题 3** -在报表中**列为未知 OS 的物理主机下的虚拟机太多**，以及/或者在运行 SIL 的 Windows 服务器上使用**get-sildata**时生成的错误。

## <a name="troubleshooting-data-flow-issues"></a>解决数据流问题

在开始之前，需要知道 SIL 聚合器开始之前如何开始**set-silaggregator** cmdlet。

>[!IMPORTANT]
>在3AM 本地系统时间处理 SQL 数据多维数据集之前，报表中将不会有任何数据。 在多维数据集处理数据之前，不要继续执行故障排除步骤。

如果对报表中的数据（或报表中缺少的数据）进行故障排除，该数据比上次处理多维数据集的时间新，或在处理完多维数据集之前（对于全新安装），请按照以下步骤进行操作：:

1. 以 SQL Server 管理员身份登录，并在命令提示符下运行**SSMS** 。
2. 连接到数据库引擎。
3. 展开**SQL Server 代理**然后展开 "**作业**"。
4. 右键单击**SILStagingRefresh** ，然后选择 "**在步骤开始作业**"。
5. 单击 "**启动**"，等待刷新进度栏完成。
6. 以管理员身份打开 PowerShell 并运行**silreport-openreport** cmdlet。

如果报表中仍没有数据，请继续排查三个数据流问题。

### <a name="data-flow-issue-1"></a>数据流问题1 

#### <a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>使用 SilReport cmdlet 时报表中不存在数据（或一般数据缺失）

如果数据丢失，则很可能是因为 SQL 数据多维数据集尚未处理。 如果已对其进行了最近的处理，但你认为丢失的数据应在处理多维数据集之前到达聚合器，请按照相反的顺序执行数据的路径。 选取唯一主机和唯一的 VM 进行故障排除。 相反，数据路径将为**SILA Report** &lt; **SILA database** &lt; **SILA local directory** &lt; **远程物理主机**或**WS VM 运行 SIL agent/task**。

#### <a name="check-to-see-if-data-is-in-the-database"></a>查看数据库中是否有数据

可以通过两种方式检查数据：**Powershell**或**SSMS**。

>[!Important]
>如果多维数据集至少处理了一次，因为 SILA 将数据插入到数据库中，则此数据应反映在报表中。 如果数据库中没有数据，则轮询物理主机失败，或者不是通过 HTTPS 接收任何内容，或者两者都不是。

 #### <a name="powershell"></a>PowerShell

1. 以管理员身份打开 PowerShell 并运行**add-silvmhost** cmdlet，然后运行**set-silaggregator**。

    >[!NOTE]
    >**Set-silaggregator**的输出将始终模拟 Excel 报表的 " **Windows Server 详细信息" 选项卡**。 不需要其他结果。

2. 运行**add-silvmhost**
   - 如果未列出任何主机，则使用**add-silvmhost** cmdlet 添加主机。
   - 如果主机列出为 "**未知**"，则请参阅 "问题 2"。 
   如果主机已列出但最近的 "**轮询**" 列下没有**日期时间**，请参阅下面的 "**问题 2** "。

**其他相关命令**

**Set-silaggregator-已知服务器的&lt;Computername fqdn 推送数据&gt;** ：这会在处理完多维数据集之前，从数据库中生成有关计算机（VM）的信息。 因此，此 cmdlet 可用于检查数据库中的数据，以便 Windows Server 在3AM （或者在此部分开头所述的情况下，如果未按本部分开头所述的方式实时刷新多维数据集）上推送 SIL 数据。

**使用 add-silvmhost cmdlet &lt;&gt;时，set-silaggregator-VmHostName 已轮询的物理主机的 fqdn，其中存在 "最近轮询" 列下的值**：即使在处理多维数据集之前，也会在数据库中生成有关物理主机的信息。

#### <a name="ssms"></a>SSMS

n**检查要轮询的主机中的数据：**
 
1. 打开**SSMS**并连接到**数据库引擎**。
2. 展开 "**数据库**"，展开 " **SoftwareInventoryLogging** " 数据库，展开 "**表**"，右键单击 " **HostInfo** " 表，然后选择 "前1000行"。 

    如果表中有一个或多个主机的数据，则为该主机至少进行一次的轮询。

   **检查通过 HTTPS 推送数据的 Vm 或独立服务器上的数据：** 

3. 打开**SSMS**并连接到**数据库引擎**。
   a2. 展开 "**数据库**"，展开 " **SoftwareInventoryLogging** " 数据库，展开 "**表**"，右键单击 " **VMInfo** " 表，然后选择前1000行。 

    >[!NOTE] 
    >唯一 VM 的每一行都表示一个已处理的**bmil**文件成功通过 HTTPS 推送并由 SIL 聚合器处理。 Bmil 文件是 SIL 使用的专有文件，每个 SIL 实例都会创建一个文件，请注意，仅在虚拟环境中使用 SIL 和 SILA 时，这才是必需的。 否则，仅 HTTPS 流量是必需的/预期的。

   处理完多维数据集后，数据库中的所有数据都应在 SIL 报告中反映出来。

### <a name="data-flow-issue-2"></a>数据流问题2
#### <a name="too-many-servers-under-unknown-host"></a>未知主机下的服务器太多

当 SIL 聚合器未能成功轮询托管虚拟机的物理主机时，可能会出现这种情况。

1. 以管理员身份打开**PowerShell**并运行**add-silvmhost** cmdlet。

    -   如果主机列出为 "**未知**"，则**add-silvmhost** cmdlet 将无法正常工作，这通常是由于为访问这些主机而添加了错误的凭据（因而是未知的）。 但如果验证凭据，则可能表示**add-silvmhost** cmdlet 中的**hosttype**和**hypervisortype**的自动检测无法识别该平台。 在这些情况下，可以使用高级故障排除步骤，但此处未介绍这些步骤（请检查 EventViewer SIL 聚合器通道）。

    -   如果主机已列出，且**hosttype**和**hypervisortype**列的值**未知**，即 Windows 和 HyperV，或 Ubuntu 和 Xen 等，但 "**最近轮询**" 列下没有 "**日期时间**"，则轮询尚未成功。

        -   添加主机进行轮询后，需要等待一小时（假设此时间间隔设置为默认值–可以使用**set-silaggregator** cmdlet 进行检查）。

        -   如果自添加主机以来已经过了一小时，请检查轮询任务是否正在运行：在**任务计划程序**中，选择 " **Microsoft** &gt; **Windows** " 下的 "**软件清单日志记录聚合**器"，然后检查任务的历史记录。

    -   如果列出了主机，但没有**RecentPoll**、 **HostType**或**HypervisorType**的值，这可能会很大程度上被忽略。 这只会在 HyperV 环境中发生。 此数据实际上来自 Windows Server VM，它标识了它通过 HTTPS 运行的物理主机。 这对于识别报告的特定 VM 很有用，但需要使用**SilAggregatorData** cmdlet 来挖掘数据库。

正确轮询主机后，你将能够在 SILA 数据库中查看这些物理主机的数据，其中在 "**最近轮询**" 下有一个**日期时间**。 上面的 "问题 1" 部分提供了用于检索此数据的步骤。

### <a name="data-flow-issue-3"></a>数据流问题3
#### <a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>将 Vm 列为未知 OS 的物理主机过多

1. 选择你知道的一个 Windows Server 终端节点（VM）在其中一台主机上，以管理员身份登录。
2. 以管理员身份打开 PowerShell。
3. 使用**set-sillogging** Cmdlet 验证**set-sillogging**是否正在运行。
   - 如果正在运行，请尝试使用**get-sildata**手动推送 SIL 数据。

   - 如果出现错误：
     - 确保**targeturi**在条目中具有**https://** 。
     - 确保满足所有先决条件 
     - 确保已安装 Windows Server 的所有必需更新（请参阅 SIL 的先决条件）。 若要快速检查（仅限 WS 2012 R2），请使用以下 cmdlet 来查找这些内容：**Get-silwindowsupdate \*3060， \*3000**
     - 确保用于对聚合器进行身份验证的证书安装在要使用**set-sillogging**进行清点的本地服务器上的正确存储中。
     - 在 SIL 聚合器上，请确保使用**set-silaggregator** **– AddCertificateThumbprint** cmdlet 将用于向聚合器进行身份验证的证书的证书指纹添加到列表。
     - 如果使用企业证书，请检查启用 SIL 的服务器是否已加入为之而创建证书的域，或是否可通过根证书颁发机构进行验证。 如果证书在尝试向聚合器转发/推送数据的本地计算机上不受信任，此操作将失败并返回错误。

     如果已检查并验证上述所有内容，但问题仍然存在：

     - 检查用于安装 SIL 聚合器的证书是否正常，以及是否与 SIL 聚合器服务器本身的名称相匹配。 此外，如果企业证书用于安装 SIL 聚合器，聚合器可能需要加入到创建证书的域（如果其他计算机成功转发到同一 SIL 聚合器，则无需这些步骤）。

     -  最后，可以在尝试转发/推送、 **\Windows\System32\Logfiles\SIL**的服务器上查看缓存的 SIL 文件所在的位置。 如果**set-sillogging**已启动并运行超过1小时，或者最近运行过**get-sildata** ，且此目录中没有任何文件，则不会成功记录到聚合器。

如果没有错误，并且在控制台上没有输出，则通过 HTTPS 从 Windows Server 结束节点到 SIL 聚合器的数据推送/发布已成功。 若要跟踪数据的路径，请以管理员身份登录到 SIL 聚合器，并检查收到的数据文件。 请参阅**Program Files （x86）** &gt; **Microsoft SIL 聚合** &gt;器 SILA directory。 您可以实时查看数据文件。

>[!NOTE] 
>可以使用**get-sildata** cmdlet 传输多个数据文件。 结束节点上的 SIL 会将失败的推送缓存多达30天。 在下一次成功推送时，所有数据文件都将发送到聚合器进行处理。 通过这种方式，新设置的 SIL 聚合器可以在其自身安装之前，从结束节点中显示数据。

>[!NOTE] 
>在 SILA 目录中处理只在低流量情况下相关的数据文件时，SILA 规则。 高流量将始终实时触发处理。 默认行为是，处理将在100个文件到达目录之后或在15分钟后开始。 在小型环境中进行端到端故障排除时，通常需要等待15分钟。

处理这些文件后，您将看到数据库中的数据。
