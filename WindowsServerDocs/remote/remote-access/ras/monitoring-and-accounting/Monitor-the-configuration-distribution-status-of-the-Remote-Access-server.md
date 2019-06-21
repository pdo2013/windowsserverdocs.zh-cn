---
title: 监视远程访问服务器的配置分发状态
description: 本主题是指南的适用于远程访问监视和记帐 Windows Server 2016 中的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de285d13-9e54-4c46-88f0-607182e5e3dc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab5deea9d594d6e9570d2472b5628a3d5fb8d616
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282728"
---
# <a name="monitor-the-configuration-distribution-status-of-the-remote-access-server"></a>监视远程访问服务器的配置分发状态

>适用于：Windows 服务器 （半年频道），Windows Server 2016

**注意：** Windows Server 2012 将 DirectAccess 和远程访问服务 (RAS) 合并到单个远程访问角色。  
  
远程访问管理控制台将来自所有受监视服务器的配置版本进行比较，以验证它们是否匹配以及是否使用最新的配置版本。 这将显示是否已将最新的配置版本（在组策略对象或 GPO 中指定）分发到所有服务器，以及它是否已成功应用到服务器。  
  
### <a name="to-use-the-monitoring-dashboard-to-monitor-the-configuration-distribution"></a>使用监视仪表板来监视配置分发  
  
1.  在“服务器管理器”  中，单击“工具”  ，然后单击“远程访问管理”  。  
  
2.  单击“仪表板”  以导航到“远程访问管理控制台”  中的“远程访问仪表板”  。  
  
3.  在监视仪表板上，请注意位于顶部中心的“配置状态”  磁贴。 此磁贴显示配置分发的当前状态。  
  
下表显示了由“配置状态”  磁贴生成的消息、其含义以及必要的管理操作（如果有）。  
  
|||||  
|-|-|-|-|  
|Severity|Message|含义|该进行什么操作？|  
|成功|已成功分发配置。|GPO 中的配置已成功应用到服务器。|不需要执行任何操作。|  
|警告|未从域控制器检索到服务器 [*服务器名称*] 的配置。 GPO 未链接。|GPO 中的配置尚未传播到服务器。 这可能是因为 GPO 未链接到服务器。|将 GPO 链接到应用于服务器的管理作用域，或者在暂存 GPO 方案中，手动从暂存 GPO 导出设置并将它们导入生产 GPO。 有关暂存 Gpo 的详细信息，请参阅**具有有限权限管理远程访问 Gpo**中[Step-1-Plan-the-DirectAccess-Infrastructure](../../directaccess/single-server-advanced/Step-1-Plan-the-DirectAccess-Infrastructure.md)。 有关 GPO 暂存步骤，请参阅**具有有限权限配置远程访问 Gpo**中[步骤 1:配置 DirectAccess 基础结构](../../directaccess/single-server-advanced/Step-1-Configuring-DirectAccess-Infrastructure.md)。|  
|警告|尚未从域控制器检索到服务器 [*服务器名称*] 的配置。|GPO 中的配置尚未传播到服务器。<br /><br />可能需要花费长达 10 分钟的时间来传播新配置。|允许花更多时间将策略更新到服务器。|  
|错误|未从域控制器检索到服务器 [*服务器名称*] 的配置。|GPO 中的配置未传播到该服务器，并且自配置更改之后已过去超过 10 分钟的时间。|这可能在下列任一种情况中发生：<br /><br />-服务器到域后，若要更新的策略已没有连接。 要强制执行策略更新的服务器上，可以运行"gpupdate /force"。<br />的要检索更新的配置，可能需要 GPO 复制。<br />-没有任何可写域控制器在 Active Directory 站点中的远程访问服务器。<br /><br />等待 GPO 复制到所有域控制器，然后使用 Windows PowerShell cmdlet **Set-DAEntryPointDC** 将入口点与远程访问服务器中 Active Directory 的可写域控制器相关联。|  
|警告|已从域控制器检索到服务器 [*服务器名称*] 的配置，但尚未应用。|GPO 中的配置已传播到服务器，但尚未应用。<br /><br />可能在长达 15 分钟后才会应用配置。|允许花更多时间将配置完全应用到服务器。|  
|错误|不能应用从域控制器检索的服务器 [*服务器名称*] 的配置。|GPO 中的配置已传播到服务器但未成功应用，并且自配置更改之后已过去超过 15 分钟的时间。|这可能在下列任一种情况中发生：<br /><br />1.当前正在应用配置。 它显示为错误的原因在于可能需要花费很长时间才能从 GPO 中检索到配置。<br />    若要验证是否由于该原因，请使用**任务计划程序**并导航到 Microsoft\Windows\RemoteAccess 以验证**RAConfigTask**当前正在运行。<br />2.如果 **RAConfigTask** 当前并未运行，则可能未能在服务器上应用配置。<br />    请在远程访问服务器操作通道下的“事件查看器”  中检查错误，该通道位于 \Applications and Services Logs\Microsoft\Windows\RemoteAccess-RemoteAccessServer。<br />    请在远程访问管理控制台的“操作状态”  中检查错误。 有关详细信息，请参阅[监视远程访问服务器及其组件的操作状态](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md)。|  
|错误|已从域控制器检索到多站点服务器的配置。 该配置并没有在所有服务器上匹配。|多站点部署中的服务器 GPO 配置版本之间存在不一致。<br /><br />理想情况下，用于所有入口点的所有服务器 GPO 将具有相同的全局配置，但由于某种原因，它们并未同步。|当配置更改失败并且未成功回滚时，可能发生此情况。<br /><br />应该从所有服务器 GPO 已同步的备份状态中还原 GPO。 有关可用脚本的信息，请参阅[备份和还原远程访问配置](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6)。|  
  


