---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: 声明管道的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd5ca15a9e098f60c6c82980e3148ebcf9ee9932
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869343"
---
# <a name="the-role-of-the-claims-pipeline"></a>声明管道的角色
Active Directory 联合身份验证服务\(ADFS\)中的声明管道表示声明在发出之前必须遵循联合身份验证服务的路径。 该联合身份验证服务管理通过声明管道\-的\-各个阶段（也包括声明规则引擎处理声明规则）对声明进行流动的整个端到端过程。  
  
有关声明规则的详细信息，请参阅[声明规则的角色](The-Role-of-Claim-Rules.md)。 有关声明规则引擎如何处理规则的详细信息，请参阅 [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  
以下部分更详细地讨论联合身份验证服务所监督的过程。  
  
## <a name="claims-pipeline-process"></a>声明管道过程  
声明管道过程由三个高级\-阶段组成。 此过程中的每个阶段都初始化声明规则引擎以处理特定于该阶段的声明规则。 这些阶段\(的出现\)顺序如下：  
  
1.  接受传入声明 — 声明管道中的此阶段用于从令牌中提取传入声明并消除不需要或受信任的声明。 提取这些声明之后，会运行组成声明提供方信任的接受转换规则集的接受规则。 这些规则可以用于传递或添加新声明，这些声明随后可以在声明管道的后续阶段中使用。 此阶段的输出用作第二个和第三个阶段的输入。  
  
2.  授权声明请求者 — 此阶段由声明引擎用于基于是否允许令牌请求者获取给定信赖方的令牌来发出允许或拒绝声明。 但是，需要先运行组成信赖方信任的发出授权规则集或委派授权规则集的授权规则，然后才能进行此阶段。  
  
3.  发出传出声明 — 此阶段用于发出传出声明并沿管道发送它们（它们在管道中会打包为安全令牌）。 但是，需要先运行组成信赖方信任的发出转换规则集的发出规则（这会确定将作为传出声明发出的声明），然后才能进行此阶段。  
  
上面所有三个阶段都执行声明规则处理，但使用不同的规则集。 如上所述，每个阶段都有一组关联的规则，这些规则基于传入声明的颁发者\(，这些规则\)基于传入声明的颁发者，即为其颁发\(授权的 claimincludes 的接受规则或目标服务。颁发规则\)。  
  
声明是令牌\-不可知的，但会通过封装在安全令牌中的网络进行传输。 声明规则对声明进行操作，而不考虑传入或传出安全令牌的格式。  
  
声明规则包含管理员\-定义的逻辑，声明引擎通过该逻辑接受传入声明、基于请求者的身份授权声明，并颁发信赖方所需的声明。 最后，由声明引擎确定哪些声明将进入在声明流过声明管道之后颁发的安全令牌中。  
  
如下图所示，声明管道负责将声明流过各个管道阶段的整个\-端\-到端过程，以便最终获得发出的声明，该声明将通过信赖参与方信任。 图中的传出声明表示发出的声明。  
  
![AD FS 角色](media/adfs2_pipeline.gif)  
  
虽然未显示在图中，不过是由声明引擎在每个阶段执行规则的实际处理。 有关详细信息，请参阅 [The Role of the Claims Engine](The-Role-of-the-Claims-Engine.md)。  
  

