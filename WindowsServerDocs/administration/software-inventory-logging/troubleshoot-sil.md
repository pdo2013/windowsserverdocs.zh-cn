---
title: 软件清单日志记录疑难解答
description: 介绍如何解决软件清单日志记录的常见部署问题。
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-software-inventory-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
author: brentfor
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 6ea8a336e2c40b55e2ad6b508d89db7dcb668315
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832848"
---
# <a name="troubleshoot-software-inventory-logging"></a>软件清单日志记录疑难解答 

>适用于：Windows 服务器 （半年频道），Windows Server 2019，Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

##<a name="understanding-sil"></a>了解 SIL

在开始故障排除 SIL 之前，应充分了解其组件以及其工作原理。 SIL 和 SIL 聚合器的概述以及如何使用它们来转发并报告清单数据，为提供以下视频：

1. [软件清单日志记录 (SIL) (10:57) 简介](https://channel9.msdn.com/Blogs/Regular-IT-Guy/An-Introduction-to-Software-Inventory-Logging-SIL)

2. [软件清单日志记录：设置 SIL 聚合器 (14:34)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Setting-up-SIL-Aggregator)

3. [软件清单日志记录：启用 SIL 转发 (7:20)](https://channel9.msdn.com/Blogs/windowsserver/Software-Inventory-Logging-Enabling-SIL-Forwarding)

##<a name="how-sil-data-flow-works"></a>SIL 数据如何流动的工作原理

SIL 框架具有两个主要组件和两个通道的通信。 数据流通过这两通道，并且这两个组件之间 （假定的虚拟化，或云，环境-纯粹物理环境仅需要一个通信通道的） 成功 SIL 部署。 你将需要了解的组件和数据流的 SIL 用于正确部署。 观看概述视频更高版本之后, 您将了解通过这两个通道中说明了组件和数据流的体系结构关系图。 橙色箭头指示通过 WinRM 的远程查询，绿色的虚线的箭头表示 HTTPS 发到 SIL 聚合器从 SIL 在每个 WS 结束节点中：

![](../media/software-inventory-logging/image1.png)

如果遇到 SIL 的问题，它是可能与数据的流中的中断通过通道，并在组件之间。 以下是最常见的问题相关的数据对数据流后, 接下一节中的故障排除步骤来解决每三个问题：

-   **问题 1 数据流**–**报表时使用发布 SilReport cmdlet 中的没有数据**（或通常缺少数据）。

-   **问题 2 数据流**–**过下未知的主机的多个服务器**报表中。

-   **问题 3 数据流**–**过下作为未知操作系统列出的物理主机的多个 Vm**在报表中，和/或使用时生成错误**Publish-sildata**运行 SIL 的 Windows 服务器上。

##<a name="troubleshooting-data-flow-issues"></a>故障排除数据流问题

在开始之前，你将需要知道如何很久以前 SIL 聚合器入门**开始 SilAggregator** cmdlet。

>[!IMPORTANT]
>SQL 数据多维数据集处理在凌晨 3 点本地系统时间之前，在报表中也将是任何数据。 不要继续进行故障排除步骤，直到该多维数据集处理数据。

如果要解决比上一次新报表 （或报表中缺少） 中的数据多维数据集处理，或者 （对于新安装），具有不断处理多维数据集之前执行以下步骤处理实时 SQL 数据多维数据集:

1. SQL Server 的管理员身份登录并运行**SSMS**在命令提示符。
2. 连接到数据库引擎。
3. 展开**SQL Server 代理**，然后展开**作业**。
4. 右键单击**SILStagingRefresh** ，然后选择**作业开始步骤**。
5. 单击**启动**并等待刷新进度条完成。
6. 打开 PowerShell，以管理员身份运行**发布 silreport openreport** cmdlet。

如果在报表中仍没有任何数据，继续进行故障排除的三个数据流问题。

###<a name="data-flow-issue-1"></a>数据流量问题 1 

####<a name="no-data-in-the-report-when-using-the-publish-silreport-cmdlet-or-data-is-generally-missing"></a>使用发布 SilReport cmdlet 时报表中的没有数据 （或缺少数据通常）

如果缺少数据，则它可能是由于 SQL 数据多维数据集不具有尚未处理。 如果它最近已处理并且您认为缺失的数据应已到达多维数据集处理前的聚合器，请按相反的顺序执行的数据的路径。 选择唯一的主机和一个唯一的虚拟机进行故障排除。 按相反的顺序的数据路径将是**SILA 报表** &lt; **SILA 数据库** &lt; **SILA 本地目录** &lt; **远程物理主机**或**WS VM 运行 SIL 的代理/任务**。

####<a name="check-to-see-if-data-is-in-the-database"></a>检查数据在数据库中

有两种方法来检查有数据：**Powershell**或**SSMS**。

>[!Important]
>如果由于 SILA 将数据插入数据库，具有至少一次处理多维数据集，应在报表中反映此数据。 如果在数据库中没有数据然后失败轮询的物理主机或执行任何操作正在接收通过 HTTPS，或两者。

 ####<a name="powershell"></a>PowerShell

1. 打开 PowerShell，以管理员身份运行**get silvmhost** cmdlet，然后运行**get silaggregator**。

    >[!NOTE]
    >输出**get silaggregator**始终将模拟**Windows Server 详细信息选项卡**的 Excel 报表。 不希望不同的结果。

2. Run **get-silvmhost**
 - 如果那里任何主机列出，然后添加主机使用**添加 silvmhost** cmdlet。
 - 如果主机被列为**未知**，然后转到问题 2。 d-如果列出的主机，但没有任何**datetime**下**最近轮询**列，然后再转到**问题 2**下面。

**其他相关的命令**

**Get SilAggregator-Computername&lt;将数据推送的已知服务器 fqdn&gt;**:即使之前已处理该多维数据集，这会生成有关计算机 (VM) 从数据库的信息。 因此可以使用此 cmdlet 检查数据库中的数据适用于推送 SIL 数据通过 HTTPS 之前，或不在凌晨 3 点多维数据集处理 （或如果在尚未刷新实时多维数据集，如本部分开头所述） Windows Server。

**Get SilAggregator VmHostName&lt;轮询的物理主机的 fqdn 的最近轮询列下的值时使用 Get-silvmhost cmdlet&gt;**:即使之前已处理该多维数据集，这会生成有关物理主机数据库中的信息。

####<a name="ssms"></a>SSMS

n**检查是否有从轮询的主机的数据：**
 
1. 打开**SSMS**并连接到**数据库引擎**。
2. 展开**数据库**，展开**SoftwareInventoryLogging**数据库，展开**表**，右键单击**HostInfo**表，然后选择前 1000年行。 

    如果没有数据的表中的一个或多个主机，然后轮询，/those 主机已成功至少一次。

 **检查有从虚拟机或独立服务器，通过 HTTPS 推送数据的数据：** 

1. 打开**SSMS**并连接到**数据库引擎**。
a2. 展开**数据库**，展开**SoftwareInventoryLogging**数据库，展开**表**，右键单击**VMInfo**表，然后选择前 1000年行。 

    >[!NOTE] 
    >唯一的 VM 的每一行表示一个处理**bmil**文件已成功通过 HTTPS 推送并由 SIL 聚合器进行处理。 Bmil 文件是由 SIL 的专有文件，则会创建每个我们的每个 SIL 实例，此选项仅必要时请注意 SIL 和 SILA 用于虚拟环境中。 否则仅 HTTPS 流量是预期需要/）。

 处理多维数据集后，应会在数据库中的所有数据都反映在 SIL 报告。

###<a name="data-flow-issue-2"></a>数据流量问题 2
####<a name="too-many-servers-under-unknown-host"></a>未知的主机下太多的服务器

这是可能在 SIL 聚合器未成功轮询托管虚拟机的物理主机时出现在虚拟环境。

1. 打开**PowerShell**以管理员身份运行**get silvmhost** cmdlet。

    -   如果主机被列为**未知**，则**添加 silvmhost** cmdlet 未正常工作 – 通常由于错误的访问权限添加到这些主机的凭据 (因此，未知)。 如果验证凭据，这可能意味着自动检测，但是**hosttype**并**hypervisortype**中**添加 silvmhost** cmdlet 无法识别平台。 那里一些高级故障排除步骤可用于这些情况下，但此处未介绍 （请检查事件查看器的 SIL 聚合器通道）。

    -   如果主机未列出，并**hosttype**并**hypervisortype**列出的值不是**未知**，即 Windows 和 HyperV，或 Ubuntu 和 Xen，等，但没有任何**datetime**下**最近轮询**列，然后轮询成功尚未发生。

        -   你将需要等待一小时后添加的轮询的主机发生 (假设此间隔设置为默认值-可以使用检查**get silaggregator** cmdlet)。

        -   如果已添加主机，因为已一小时，请检查轮询任务正在运行:在中**任务计划程序**，选择**软件清单日志记录聚合器**下**Microsoft** &gt; **Windows**和检查任务的历史记录。

    -   如果主机已列出，但不存在的值**RecentPoll**， **HostType**，或**HypervisorType**，可以很大程度上忽略。 这只会在 HyperV 环境中。 此数据实际来自于 Windows Server VM，识别通过 HTTPS 运行在物理主机。 这可以是标识特定的 VM 的报告，但需要挖掘数据库使用很有用**Get SilAggregatorData** cmdlet。

后正确轮询主机，你将能够看到这些物理主机 SILA 数据库中的数据没有**datetime**下**最近轮询**。 上面的问题 1 部分提供用于检索此数据的步骤。

###<a name="data-flow-issue-3"></a>数据流量问题 3
####<a name="too-many-physical-hosts-with-vms-listed-as-unknown-os"></a>与列为未知 OS 的 Vm 的物理主机数过多

1. 选择一个 Windows Server 结束节点 (VM) 您知道是一个这些主机上以管理员身份登录。
2. 以管理员身份打开 PowerShell。
3. 验证是否**SilLogging**通过使用运行**Get-sillogging** cmdlet。
 - 如果运行，尝试通过使用手动推送 SIL 数据**Publish-sildata**。

  - 如果出现错误：
     - 请确保**targeturi**已**https://** 条目中。
     - 请确保满足所有系统必备组件 
     - 确保已安装适用于 Windows Server 的所有必需的更新 （请参阅 SIL 的先决条件）。 若要检查 （仅限 WS 2012 R2) 的快速方法是查找这些使用以下 cmdlet:**Get-SilWindowsUpdate \*3060, \*3000**
     - 确保正在用于对聚合器进行身份验证的证书安装在要进行清点的本地服务器上的正确存储**SilLogging**。
     - 上 SIL 聚合器，请确保使用向聚合器进行身份验证的证书的证书指纹添加到列表使用**Set SilAggregator** **– AddCertificateThumbprint**cmdlet。
     - 如果使用企业证书，请检查启用 SIL 的服务器是否已加入为之而创建证书的域，或是否可通过根证书颁发机构进行验证。 如果证书在尝试向聚合器转发/推送数据的本地计算机上不受信任，此操作将失败并返回错误。

    如果上述所有已检查和验证，但问题仍然存在：

    - 检查用于安装 SIL 聚合器的证书正常运行，以及与 SIL 聚合器服务器本身的名称相匹配。 此外，如果企业证书用于安装 SIL 聚合器，聚合器可能需要加入到创建证书的域（如果其他计算机成功转发到同一 SIL 聚合器，则无需这些步骤）。

    -  最后，可以检查缓存的 SIL 文件尝试进行转发/推送的服务器上的以下位置**\Windows\System32\Logfiles\SIL**。 如果**SilLogging**已启动并已运行超过一小时，或**Publish-sildata**最近已运行并没有任何文件在此目录中，不是已向聚合器的日志记录成功。

如果没有错误，并在控制台上的任何输出，然后数据推送/从发布的 Windows Server 结束节点到通过 HTTPS 的 SIL 聚合器已成功。 若要以管理员身份 SIL 聚合器跟踪数据转发，登录名的路径，并检查已到达的数据文件。 转到**Program Files (x86)** &gt; **Microsoft SIL 聚合器** &gt; SILA 目录。 可以监视在真实时间到达的数据文件。

>[!NOTE] 
>多个数据文件可能会使用传输**Publish-sildata** cmdlet。 SIL 结束节点上的将缓存最多 30 天失败的推送。 在下一步的成功推送所有数据文件将都转到聚合器进行处理。 这样一来，新设置 SIL 聚合器可以显示其自己的安装程序之前也结束节点中的数据。

>[!NOTE] 
>有 SILA 遵循时处理 SILA 目录中的数据文件，仅与低流量情况的规则。 较高的流量始终会触发实时处理。 默认行为是，处理将开始或者后在目录中，或 15 分钟后到达 100 个文件。 在小型环境中的端到端故障排除时，通常必须要等待 15 分钟。

这些文件进行处理后，将看到数据库中的数据。
