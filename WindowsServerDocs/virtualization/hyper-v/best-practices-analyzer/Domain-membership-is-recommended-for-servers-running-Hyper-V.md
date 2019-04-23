---
title: 域成员身份建议对运行 HYPER-V 的服务器
description: 此最佳实践分析工具规则的文本的联机版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2f4578e5-0848-46b4-a50b-7dbd480b80bf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e9db1d28cfe1ae4afd6c5dc1a93253c83fc42113
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860898"
---
# <a name="domain-membership-is-recommended-for-servers-running-hyper-v"></a>域成员身份建议对运行 HYPER-V 的服务器

>适用于：Windows Server 2016


  
*有关最佳做法和扫描的详细信息，请参阅*[最佳做法分析器](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|配置|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*此服务器是工作组的成员。*  
  
## <a name="impact"></a>影响  
  
*没有为此服务器能集中管理。*  
  
将此计算机加入到域允许通过标识、 安全性和审核策略的集中式的管理。  
  
## <a name="resolution"></a>分辨率  
  
*如果有可用的域环境，此服务器加入到该域。*  
  
> [!IMPORTANT]  
> 我们建议您查看此计算机以确定是否有安全隐患的将此计算机加入到域上的虚拟机中运行的工作负荷。 如果任何虚拟机的虚拟化的域控制器，请参阅[规划虚拟化域控制器的考虑因素](https://go.microsoft.com/fwlink/?LinkId=190192)(https://go.microsoft.com/fwlink/?LinkId=190192)。  
  
将计算机加入到域需要域以及的计算机上的权限：   
- 在计算机上，将需要是 Administrators 组的成员的用户帐户。 使用此类型的帐户，登录，或者在系统提示时的帐户提供用户名和密码。   
- 在域中，将需要有权将计算机加入到域的用户帐户。 系统将提示您输入用户名和密码。  
  
有关说明，请参阅[将计算机加入到域](https://go.microsoft.com/fwlink/?LinkId=190193)(https://go.microsoft.com/fwlink/?LinkId=190193)。  
  


