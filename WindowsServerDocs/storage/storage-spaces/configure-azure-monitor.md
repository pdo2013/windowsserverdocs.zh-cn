---
title: 了解和配置 Azure 监视器
description: 安装程序和详细信息 Azure Monitor 是如何在 Windows Server 2016 和 2019年中配置你的存储空间直通群集的电子邮件和短信警报。
keywords: 存储空间直通，azure 监视器、 通知、 电子邮件、 短信
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 6b229696e796f176fe89ab250ab48f1d9f0d5666
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280204"
---
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>使用 Azure Monitor 将电子邮件发送运行状况服务故障

>适用于：Windows Server 2019、Windows Server 2016

Azure 监视器，从而最大化可用性和性能的应用程序提供全面的解决方案，用于收集、 分析和操作来自你的云的遥测数据，并在本地环境。 它可帮助你了解如何执行你的应用程序，并主动标识影响它们以及它们所依赖的资源的问题。

这是内部部署超聚合群集特别有用。 通过 Azure Monitor 集成，你将能够配置电子邮件、 短信 (SMS) 和其他警报，以便成功进行 ping 操作您时出错了，与你的群集 （或想要的标志基于收集的数据的一些其他活动时）。 下面，我们将简要介绍 Azure Monitor 的工作原理、 如何安装 Azure Monitor，以及如何将其配置为向你发送通知。


## <a name="understanding-azure-monitor"></a>了解 Azure 监视器

Azure Monitor 收集的所有数据都放到两种基本类型之一： 指标和日志。

1. [指标](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics)是在时间中描述的特定点的系统的某些方面的数字值。 它们是轻量和能够支持附近实时方案。 你将看到收集的 Azure Monitor 直接在 Azure 门户中其概述页中的数据。

![指标在指标资源管理器中引入的图像](media/configure-azure-monitor/metrics.png)

2. [日志](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs)包含不同类型的数据组织到具有不同的每种类型的属性集的记录。 如事件和跟踪遥测数据存储为日志此外的性能数据，以便它都可以合并以进行分析。 可以使用分析日志数据由 Azure Monitor 收集[查询](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview)用于快速检索、 整合和分析收集的数据。 可以创建和测试使用的查询[Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals)在 Azure 门户，然后将其直接使用这些工具分析数据或保存查询供[可视化效果](https://docs.microsoft.com/azure/azure-monitor/visualizations)或[警报规则](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview)。

![日志引入 log analytics 中的图像](media/configure-azure-monitor/logs.png)

我们将如何配置这些警报具有以下更多详细信息。

## <a name="configuring-health-service"></a>配置运行状况服务

需要执行的第一件事情是配置群集。 如您所知[运行状况服务](../../failover-clustering/health-service-overview.md)可提高日常监视和运行存储空间直通的群集的操作体验。 

如上面看到的 Azure Monitor 从在群集运行每个节点收集日志。 因此，我们必须配置运行状况服务写入到事件通道，它恰巧是：

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

若要配置运行状况服务，请运行：

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

运行上述设置运行状况设置 cmdlet 时，我们想要开始写入的事件会导致*Microsoft-Windows-运行状况/Operational*事件通道。

## <a name="configuring-log-analytics"></a>配置 Log Analytics

现在，你已安装程序在群集上正确日志的记录下, 一步是正确配置 log analytics。

若要提供概述， [Azure Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows)可以收集数据直接从你的数据中心或其他云环境中物理或虚拟 Windows 计算机到单个存储库进行详细的分析和关联。

若要了解受支持的配置，请查看[受支持的 Windows 操作系统](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems)并[网络防火墙配置](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements)。

如果还没有 Azure 订阅，创建[免费帐户](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)在开始之前。

### <a name="login-in-to-azure-portal"></a>登录到 Azure 门户中

登录到 Azure 门户[ https://portal.azure.com ](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

### <a name="create-a-workspace"></a>创建工作区

下面列出的步骤的更多详细信息，请参阅[Azure Monitor 文档](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer)。

1. 在 Azure 门户中，单击**所有服务**。 在资源列表中，键入**Log Analytics**。 开始键入时，列表筛选器基于您的输入。 选择**的 Log Analytics**。<br><br> 

   ![Azure 门户](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. 单击**创建**，然后选择以下各项的选项：

   * 为新提供的名称**Log Analytics 工作区**，如*DefaultLAWorkspace*。 
   * 选择**订阅**要链接到通过选择下拉列表中，如果选择的默认值不合适。
   * 有关**资源组**，选择现有资源组包含一个或多个 Azure 虚拟机。 <br><br>

      ![创建 Log Analytics 资源边栏选项卡](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. 上提供所需的信息后**Log Analytics 工作区**窗格中，单击**确定**。  

尽管对信息进行验证和创建工作区，您可以下面跟踪操作进度**通知**菜单中。 

### <a name="obtain-workspace-id-and-key"></a>获取工作区 ID 和密钥
安装 Microsoft 监视代理的 Windows 之前，需要工作区 ID 和密钥的 Log Analytics 工作区。  安装向导来正确配备代理，并确保它可以与 Log Analytics 成功通信需要此信息。  

1. 在 Azure 门户中，单击**所有服务**左上角中找到。 在资源列表中，键入**Log Analytics**。 开始键入时，列表筛选器基于您的输入。 选择**的 Log Analytics**。
2. 在 Log Analytics 工作区列表中，选择*DefaultLAWorkspace*之前创建。
3. 选择**高级设置**。<br><br> ![Log Analytics 高级设置](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. 选择**连接的源**，然后选择**Windows 服务器**。   
5. 右侧的值**工作区 ID**并**主键**。 暂时保存两个-它们复制并粘贴到暂时喜爱的编辑器。   

## <a name="installing-the-agent-on-windows"></a>在 Windows 上安装代理
以下步骤安装和配置 Microsoft Monitoring Agent。 **请确保在群集中每个服务器上安装此代理，并指示你想在 Windows 启动时运行代理。**

1. 上**Windows 服务器**页上，选择相应**下载 Windows 代理**要下载根据 Windows 操作系统的处理器体系结构版本。
2. 运行安装程序以在计算机上安装代理。
2. 在“欢迎”  页上，单击“下一步”  。
3. 上**许可条款**页上，阅读许可协议，然后单击**我同意**。
4. 上**目标文件夹**页上，更改或保留默认安装文件夹，然后单击**下一步**。
5. 上**代理安装选项**页上，选择将代理连接到 Azure Log Analytics，然后单击**下一步**。   
6. 上**Azure Log Analytics**页上，执行以下步骤：
   1. 粘贴**工作区 ID**并**工作区密钥 （主密钥）** 之前复制。    
    a. 如果计算机需要通过代理服务器与 Log Analytics 服务进行通信，请单击**高级**并提供的 URL 和端口号的代理服务器。  如果你的代理服务器要求身份验证，请键入用户名和密码以使用代理服务器进行身份验证，然后单击**下一步**。  
7. 单击**下一步**提供必要的配置设置完成。<br><br> ![粘贴工作区 ID 和主键](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. 上**已准备好安装**页上，查看你的选择，然后单击**安装**。
9. 上**配置已成功完成**页上，单击**完成**。

完成后， **Microsoft Monitoring Agent**将出现在**控制面板**。 可以查看你的配置，并验证代理已连接到 Log Analytics。 当连接时，在**Azure Log Analytics**选项卡上，代理将显示一条消息：**Microsoft Monitoring Agent 已成功连接到 Microsoft Log Analytics 服务。** 

![MMA 到 Log Analytics 的连接状态](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

若要了解受支持的配置，请查看[受支持的 Windows 操作系统](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems)并[网络防火墙配置](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements)。

## <a name="collecting-event-and-performance-data"></a>收集事件和性能数据

Log Analytics 可以从 Windows 事件日志以及用于更长的长期分析和报告，指定的性能计数器收集事件，并检测到某个特定条件时采取操作。  请按照以下步骤开始配置 Windows 事件日志，以及几个常见的性能计数器中收集事件。  

1. 在 Azure 门户中，单击**更多服务**左下角上找到。 在资源列表中，键入**Log Analytics**。 开始键入时，列表筛选器基于您的输入。 选择**的 Log Analytics**。
2. 选择**高级设置**。<br><br> ![Log Analytics 高级设置](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. 选择**数据**，然后选择**Windows 事件日志**。  
4. 在这里，通过键入以下名称，然后单击加号添加运行状况服务事件通道 **+** 。  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. 在表中，检查的严重级别**错误**并**警告**。   
6. 单击**保存**要保存配置的页的顶部。
7. 选择**Windows 性能计数器**若要启用的 Windows 计算机上性能计数器集合。 
8. 在第一次配置新的 Log Analytics 工作区的 Windows 性能计数器时，你可以选择快速创建几个常见计数器。 它们会列出每个旁边的复选框。<br> ![选中的默认 Windows 性能计数器](media/configure-azure-monitor/windows-perfcounters-default.png)<br> 单击**添加所选的性能计数器**。  它们是添加，使用 10 秒收集示例间隔预设。  
9. 单击**保存**要保存配置的页的顶部。

## <a name="creating-alerts-based-on-log-data"></a>创建基于日志数据的警报

如果你已完成到目前为止，群集应发送日志和性能计数器到 Log Analytics。 下一步是创建定期自动运行日志搜索的警报规则。 如果日志搜索的结果符合特定条件，然后将触发警报并向你发送电子邮件或文本通知的。 我们来研究一下下面。

### <a name="create-a-query"></a>创建查询

首先打开日志搜索门户。   

1. 在 Azure 门户中，单击**所有服务**。 在资源列表中，键入**监视器**。 开始键入时，列表筛选器基于您的输入。 选择**监视器**。
2. 在监视器导航菜单中，选择**Log Analytics** ，然后选择一个工作区。

检索要使用的某些数据的最快方法是返回表中的所有记录的简单查询。 在搜索框中键入以下查询，然后单击搜索按钮。  

```
Event
```

在默认列表视图中，返回数据，您可以看到返回总记录数。

![简单的查询](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

在屏幕左侧是可用于查询中添加筛选而无需直接修改的筛选器窗格。  多个记录属性显示该记录的类型，并且可以选择一个或多个属性值来缩小搜索结果范围。

选中的复选框旁边**错误**下**EVENTLEVELNAME**或键入以下命令以将结果限制为错误事件。

```
Event | where (EventLevelName == "Error")
```

![Filter](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

您关心的事件所做的适当查询后，将它们保存为下一步。

### <a name="create-alerts"></a>创建警报
现在，让我们看一下用于创建警报示例。

1. 在 Azure 门户中，单击**所有服务**。 在资源列表中，键入**Log Analytics**。 开始键入时，列表筛选器基于您的输入。 选择**的 Log Analytics**。
2. 在左侧窗格中，选择**警报**，然后单击**新警报规则**顶部的页后，可以创建新的警报。<br><br> ![创建新的警报规则](media/configure-azure-monitor/alert-rule-02.png)<br>
3. 对于第一个步骤中下,**创建警报**部分中，要为资源，选择 Log Analytics 工作区，因为这是基于日志的警报信号。  通过选择特定于筛选的结果**订阅**从下拉列表中，如果你有多个帐户，其中包含之前创建的 Log Analytics 工作区。  筛选器**资源类型**通过选择**Log Analytics**从下拉列表。  最后，选择**资源** **DefaultLAWorkspace** ，然后单击**完成**。<br><br> ![创建警报的步骤 1 任务](media/configure-azure-monitor/alert-rule-03.png)<br>
4. 下一节**警报条件**，单击**添加条件**若要选择已保存的查询，然后指定警报规则遵循的逻辑。
5. 使用以下信息配置警报：  
   a. 从**基于**下拉列表中，选择**公制度量单位**。  公制度量单位将超过指定的阈值的值与在查询中创建一个警报，以便每个对象。  
   b. 有关**条件**，选择**大于**并指定 thershold。  
   c. 然后，定义何时触发警报。 例如，你可以选择**连续违规数**并从下拉列表中选择**大于**值为 3。  
   d. 在下，评估基于部分，修改**期**值设为**30**分钟并**频率**为 5。 此规则将每隔五分钟运行一次，并返回从当前时间在过去 30 分钟内创建的记录。  将时间段设置为更广泛的窗口的数据滞后时间可能帐户，并确保查询返回数据，以避免假负永远不会触发警报。  
6. 单击**完成**完成警报规则。<br><br> ![配置警报信号](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. 现在移动到第二个步骤中，提供你的警报中的名称**警报规则名称**字段中，如**上的所有错误事件警报**。  指定**描述**详细介绍有关警报的具体信息，然后选择**严重 （严重性级别 0）** 有关**严重性**从提供的选项的值。
8. 若要立即激活警报规则创建时，接受默认值**创建后的启用规则**。
9. 对于第三个和最后一个步骤中，您指定**操作组**，这可确保每次警报触发时，并可用于定义每个规则时，采取同样的操作。 使用以下信息配置新的操作组：  
   a. 选择**新操作组**并**添加操作组**窗格会显示。  
   b. 有关**操作组名称**，指定一个名称，如**IT 操作的通知**和一个**短名称**如**itops n**。  
   c. 验证的默认值**订阅**并**资源组**正确无误。 如果没有，从下拉列表选择正确。   
   d. 在操作部分中，指定操作名称，如**发送电子邮件**并在**操作类型**选择**电子邮件/短信/推送/语音**从下拉列表。 **电子邮件/短信/推送/语音**属性窗格将打开到用于提供额外信息的权限。  
   e. 上**电子邮件/短信/推送/语音**窗格中，选择并设置您的首选项。 例如，启用**电子邮件**并提供有效的电子邮件发送到的消息的 SMTP 地址。  
   f. 单击 **“确定”** 以保存你的更改。<br><br> 

    ![创建新操作组](media/configure-azure-monitor/action-group-properties-01.png)

10. 单击**确定**若要完成的操作组。 
11. 单击**创建警报规则**完成警报规则。 它会立即开始运行。<br><br> ![完成创建新的警报规则](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="test-alerts"></a>测试警报

有关参考，这是警报示例如下所示：

![警报电子邮件示例](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
- 有关详细信息，请阅读[Azure Monitor 文档](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata)。
