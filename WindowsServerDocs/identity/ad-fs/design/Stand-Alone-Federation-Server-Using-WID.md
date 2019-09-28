---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: 使用 WID 的独立联合服务器
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5fa89a4a57c618fd711234b8770a35750f3099bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358963"
---
# <a name="stand-alone-federation-server-using-wid"></a>使用 WID 的独立联合服务器

Active Directory 联合身份验证服务 \(AD FS @ no__t-2 中的备用 @ no__t-0alone 联合服务器包含一个联合身份验证服务配置为使用 Windows 内部数据库 \(WID @ no__t-4 的单一服务器。 此 AD FS 拓扑适用于测试实验室。 对于生产环境，我们不建议使用它，因为它限制仅有一个联合服务器，并且不能用于扩展到更多服务器。  
  
如果要将其他联合服务器添加到测试实验室，则必须部署此部分后面所述的任何其他拓扑，从头开始重新生成联合身份验证服务。 因此，我们建议你将此拓扑用于专用测试网络中的测试实验室或证明 @ no__t-0of @ no__t-1concept 环境，其中一台联合服务器就已足够，如下图所示。  
  
![使用 WID 的服务器](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>测试实验室注意事项  
本部分介绍有关在测试实验室环境中与此拓扑关联的目标受众、权益和限制的各种注意事项。  
  
### <a name="who-should-use-this-topology"></a>谁应该使用此拓扑？  
  
-   信息技术 \(IT @ no__t 专业人员或 IT 架构师，他们想要评估或开发这项技术的概念证明  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的好处是什么？  
  
-   在测试实验室环境中易于设置  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑的限制是什么？  
  
-   每个联合身份验证服务只有一个联合服务器 @no__t 0no 功能，可向场 @ no__t 扩展  
  
-   非冗余 \(only AD FS 配置数据库的单个实例存在 @ no__t-1  
  

## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
