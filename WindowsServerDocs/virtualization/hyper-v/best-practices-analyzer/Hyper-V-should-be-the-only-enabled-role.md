---
title: Hyper-v 应该是唯一启用的角色
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9b16a3be1e2f842c251ff3ab31d467ef7f128c8a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364757"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-v 应该是唯一启用的角色

>适用于：Windows Server 2016

有关最佳实践和扫描的详细信息，请参阅 [最佳实践分析程序](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*在此服务器上启用了除 Hyper-v 之外的角色。*  
  
在大多数情况下，在运行 Hyper-v 角色的服务器上安装其他角色并不是一个好办法。 远程桌面虚拟化主机角色服务是个例外，因为它是远程桌面服务角色的一部分，需要在同一台服务器上安装 Hyper-v。  
  
## <a name="impact"></a>影响  
  
*Hyper-v 角色应是在服务器上启用的唯一角色。*  
  
此最佳做法有助于使主机操作系统不受运行 Hyper-v 所需的角色、功能和应用程序的控制。 遵循此最佳做法并在 Nano Server 上运行 Hyper-v 有助于减少所需的更新数，因为只有 Nano Server、Hyper-v 服务组件和 Windows 虚拟机监控程序将受到软件更新的限制。  
  
## <a name="resolution"></a>分辨率  
  
*使用服务器管理器删除 Hyper-v 以外的所有角色。*  
  
服务器管理器包括删除角色向导。 此向导允许您一次删除多个角色。 删除角色之前，删除角色向导会检查依赖关系，以降低删除其他角色所依赖的软件的风险。 如果找到了依赖项，则向导会提示您批准删除已安装角色所需的其他角色、角色服务或软件。   
  
若要使用服务器管理器，必须以管理员身份登录计算机。  
  
#### <a name="to-remove-a-role"></a>删除角色  
  
1.  使用 "**开始**" 菜单、Windows 任务栏或 "管理工具" 中的快捷方式打开服务器管理器。  
2.   在服务器管理器主窗口的 "**角色摘要**" 区域中，单击 "**删除角色**"。 按照向导中的说明删除角色。   
  
  
  


