---
ms.assetid: 33b80a3f-67f3-4da7-ac4a-7fd2232fbd5d
title: "使用 WID 独立联盟服务器"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 9ec4150a7d3adfaac786219d253e1d0898c18204
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="stand-alone-federation-server-using-wid"></a>使用 WID 独立联盟服务器

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在 Active Directory 联合身份验证服务 \(AD FS\) stand\ 单独联合服务器包含联合身份验证服务主机配置为使用 Windows 内部数据库 \(WID\) 一台服务器。 此广告 FS 拓扑适用于的测试实验。 我们不建议这样做生产环境因为它的限制为仅一个联合身份验证的服务器，并且不能用于扩展到更多的服务器。  
  
如果你想要将其他联合身份验证的服务器添加到你的测试实验，您必须通过部署任何稍后提到在此部分中的其他拓扑重新生成从头联合身份验证服务。 因此，我们建议你在其中一个联盟服务器足以，在下图所示的专用测试网络中的测试实验或 proof\ of\ 概念环境使用此拓扑。  
  
![使用 WID 服务器](media/FedServerWID.gif)  
  
## <a name="test-lab-considerations"></a>测试实验注意事项  
本部分介绍有关的目标的受众、 好处，以及与的测试实验环境此拓扑相关联的限制的各种事项。  
  
### <a name="who-should-use-this-topology"></a>谁应使用此拓扑？  
  
-   技术的信息 \(IT\) 专业或 IT 师想要制定概念，该技术用于证明或评估  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>使用此拓扑的优势是什么？  
  
-   轻松测试实验环境设置  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>使用此拓扑限制有哪些？  
  
-   只有一个联盟服务器每联合身份验证服务 \ （扩展到 farm\ 没有功能）  
  
-   不冗余 \ （仅的单个实例广告 FS 配置数据库 exists\）  
  

## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
