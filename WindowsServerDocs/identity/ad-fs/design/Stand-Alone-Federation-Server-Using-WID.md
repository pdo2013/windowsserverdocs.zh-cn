---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: 使用 WID 的独立联合服务器
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 31e2e1b04383adc8bec12e7290a7acec80e0402f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190787"
---
# <a name="stand-alone-federation-server-using-wid"></a>使用 WID 的独立联合服务器

独立\-Active Directory 联合身份验证服务中的单独的联合身份验证服务器\(AD FS\)由单个服务器承载联合身份验证服务配置为使用 Windows 内部数据库组成\(WID\). 此 AD FS 拓扑适用于测试实验室。 我们不建议这样做用于生产环境因为它的限制为只有一个联合身份验证服务器，它不能用于扩展到更多的服务器。  
  
如果你想要将其他联合服务器添加到测试实验室，必须部署任何更高版本在本部分中提到的其他拓扑重新生成从零开始的联合身份验证服务。 因此，我们建议你使用此拓扑的测试实验室或概念证明\-的\-中你在其中单一联合服务器是否合适，如下图中所示的专用测试网络环境。  
  
![使用 WID 服务器](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>测试实验室注意事项  
本部分介绍有关目标的受众、 权益和限制与此拓扑用于测试的实验室环境关联的各种注意事项。  
  
### <a name="who-should-use-this-topology"></a>应使用此拓扑的用户？  
  
-   信息技术\(IT\)专业人员或 IT 架构师想要评估或开发此技术的概念证明  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的好处是什么？  
  
-   轻松地在测试实验室环境中进行设置  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑的限制是什么？  
  
-   每个联合身份验证服务只有一个联合身份验证服务器\(没有任何功能，若要纵向扩展到服务器场\)  
  
-   没有重复\(单个实例的 AD FS 配置数据库存在\)  
  

## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
