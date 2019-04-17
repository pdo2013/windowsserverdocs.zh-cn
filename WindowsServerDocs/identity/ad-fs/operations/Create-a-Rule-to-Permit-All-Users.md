---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: "创建一个规则以允许所有用户"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: de85af27e699242977054420178dd3c424b2ddb3
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-permit-all-users"></a>创建一个规则以允许所有用户

>适用于：Windows Server 2016，Windows Server 2012 R2

在 Windows Server 2016，可以使用**访问控制策略**创建将授予所有用户访问依赖方规则。  在 Windows Server 2012 R2，使用**允许所有用户**规则模板 Active Directory 联合身份验证服务 \(AD FS\)，你可以创建将授予所有用户访问依赖方授权规则。 

你可以使用其他授权规则进一步限制访问。 用户有权访问依赖方从联合身份验证服务可能仍会拒绝服务依赖方。  
  
可以使用下面的过程与广告 FS 管理的索赔规则创建 snap\ 中。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>若要创建一个规则以允许所有用户在 Windows Server 2016

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  右键单击**依赖方信任**你想要允许访问权限，然后选择**编辑访问控制策略**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. 上访问控制策略选择**允许任何人**，然后单击**应用**和**确定**。
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>若要创建一个规则以允许所有用户在 Windows Server 2012 R2 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS\\Trust Relationships\\Relying 方信任**，单击你想要创建该规则列表中的特定信任。  

3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  在**编辑索赔规则**对话框中，单击**颁发授权规则**选项卡或**委派授权规则**选项卡 \ （基于授权规则类型你 require\），然后单击**添加规则**开始**添加授权索赔规则向导**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**允许所有用户**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  在**配置规则**页上，单击**完成**。  
  
7.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  

## <a name="additional-references"></a>其他参考 
[配置索赔规则](Configure-Claim-Rules.md)  
 
[清单：创建适用于信赖的方信任索赔规则](https://technet.microsoft.com/library/ee913578.aspx)  
  
[何时使用授权索赔规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
