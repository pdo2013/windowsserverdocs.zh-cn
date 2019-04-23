---
title: Hyper-v 的目标应该是唯一启用的角色
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: bd03554396696a43b4821aff0f4ed893933484c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886458"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-v 的目标应该是唯一启用的角色

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 最佳实践分析程序。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*此服务器上启用了 HYPER-V 以外的角色。*  
  
在大多数情况下，它不是在运行 HYPER-V 角色的服务器上安装其他角色的一个好办法。 远程桌面虚拟化主机角色服务是一个异常，因为它是远程桌面服务角色的一部分，需要在同一服务器上安装 HYPER-V。  
  
## <a name="impact"></a>影响  
  
*HYPER-V 角色应在服务器上启用的唯一角色。*  
  
此最佳做法有助于保持可用的角色、 功能和应用程序不一定要运行的 HYPER-V 主机操作系统。 遵循此最佳做法和 Nano Server 上运行的 HYPER-V 可帮助减少你将需要因为只有 Nano 服务器的 HYPER-V 服务组件和 Windows 虚拟机监控程序将被视作软件更新的更新数。  
  
## <a name="resolution"></a>分辨率  
  
*使用服务器管理器删除除 HYPER-V 的所有角色。*  
  
服务器管理器包括删除角色向导。 此向导允许您在一次删除多个角色。 删除角色之前, 删除角色向导将检查依赖关系，以降低删除依赖于其他角色的软件的风险。 如果找到依赖项，向导会提示您批准删除其他角色、 角色服务或软件所需的已安装的角色。   
  
若要使用服务器管理器，必须以管理员身份登录到计算机。  
  
#### <a name="to-remove-a-role"></a>若要删除角色  
  
1.  通过使用快捷方式上打开服务器管理器**启动**菜单中的，在 Windows 任务栏上，或在管理工具。  
2.   在中**角色摘要**区域中的服务器管理器主窗口中，单击**删除角色**。 按照向导中的说明来删除角色。   
  
  
  


