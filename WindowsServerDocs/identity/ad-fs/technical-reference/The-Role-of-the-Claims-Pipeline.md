---
ms.assetid: ffb9d63c-ba7c-4ad1-b814-6db67f98c943
title: "索赔管道的作用"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5076a686b5d0b9a539f6cad8594aaf84dccc3edb
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

# <a name="the-role-of-the-claims-pipeline"></a>索赔管道的作用
在 Active Directory 联合身份验证服务 \(AD FS\) 索赔管道表示索赔必须遵守通过联合身份验证服务，然后才能发出的路径。 联合身份验证服务管理整个 to\ end\ 端过程的完成索赔管道，还包括由索赔规则引擎的索赔规则处理的各个阶段排索赔。  
  
有关索赔规则的详细信息，请参阅[声称规则的角色](The-Role-of-Claim-Rules.md)。 有关如何索赔规则引擎处理规则的详细信息，请参阅[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。  
  
下一节讨论联合身份验证服务监督更详细地的过程。  
  
## <a name="claims-pipeline-process"></a>索赔管道进程  
索赔管道进程包含三个级别 high\ 阶段。 在此过程中的每个阶段初始化索赔规则引擎到进程索赔规则特定于该阶段。 这些阶段包含 \ (顺序他们 occur\):  
  
1.  接受传入索赔，此阶段索赔管道中的用于从令牌解压缩传入的索赔并消除不预期或受信任的索赔。 他们会提取后，构成验收转换规则验收规则设置的运行提供商信任索赔。 可以使用这些规则通过或添加新的索赔，然后使用在索赔管道后续阶段。 输出的此阶段用作输入到第二个和第三个阶段。  
  
2.  在授权索赔申请者 — 索赔引擎使用此阶段发出允许或拒绝根据是否允许令牌申请者获得令牌给定依赖方或不的索赔。 但是，发生这种构成颁发授权规则授权规则设置或设置委派授权规则之前信赖的方信任在运行。  
  
3.  发出传出索赔，此阶段用于发出传出索赔，并向他们发送沿管道了成安全标记打包。 但是，发生这种组成为信赖的方信任设置颁发转换规则颁发规则正在运行之前，这将确定正是声明将颁发作为传出索赔。  
  
上述所有三个阶段进行处理的索赔规则，但使用规则另一组。 每个阶段如上所述，有一组关联的基于传入索赔颁发规则 \(the acceptance rules\) 或目标服务颁发 claimincludes \ (授权和颁发 rules\)。  
  
索赔 token\ 无关但传输通过封装安全令牌在该网络。 索赔规则工作对无论传入或传出安全令牌格式的索赔。  
  
通过信赖方需要规则包含 administrator\ 定义的逻辑的索赔引擎将接受传入索赔、授予其访问权根据申请者身份的索赔并发出索赔的索赔。 最终，它是确定索赔将转到已被索赔排列通过索赔管道后，将会发布的安全标记的索赔引擎。  
  
下图所示的索赔管道负责金绿色的叶以便最终将通过信赖的方信任发送颁发索赔通过各种管道阶段索赔的整个 to\ end\ 端过程。 图中的传出声明表示颁发的索赔。  
  
![广告 FS 角色](media/adfs2_pipeline.gif)  
  
虽然它不会显示在图上很执行每个阶段实际处理规则的索赔引擎。 有关详细信息，请参阅[的索赔引擎角色](The-Role-of-the-Claims-Engine.md)。  
  

