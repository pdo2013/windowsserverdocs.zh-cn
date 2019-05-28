---
ms.assetid: 8c179884-f0d9-4c7a-973d-820119cf3c38
title: 创建规则以允许所有用户
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: abb00e14dd0b3ce7b06efba816fbd7452e7bf0f1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189415"
---
# <a name="create-a-rule-to-permit-all-users"></a>创建规则以允许所有用户

在 Windows Server 2016 中，你可以使用**访问控制策略**创建将授予所有用户访问信赖方规则。  在 Windows Server 2012 R2 中，使用**允许所有用户**Active Directory 联合身份验证服务中的规则模板\(AD FS\)，可以创建将给所有用户访问信赖的授权规则参与方。 

可以使用其他授权规则进一步限制访问。 有权从联合身份验证服务访问信赖方的用户可能仍会被信赖方拒绝服务。  
  
可以使用以下过程以使用 AD FS 管理管理单元创建声明规则\-中。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。 

## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2016"></a>若要创建一个规则以允许 Windows Server 2016 中的所有用户

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall1.PNG)

3.  右键单击**信赖方信任**想要允许访问，然后选择**编辑访问控制策略**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall2.PNG)

4. 在访问控制策略选择**授权所有人**，然后单击**应用**并**确定**。
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall3.PNG)
  
## <a name="to-create-a-rule-to-permit-all-users-in-windows-server-2012-r2"></a>若要创建一个规则以允许在 Windows Server 2012 R2 中的所有用户 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS\\信任关系\\信赖方信任**，单击你想要创建此规则的列表中的特定信任。  

3.  右\-单击选定的信任，然后单击**编辑声明规则**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall4.PNG)  

4.  在中**编辑声明规则**对话框中，单击**颁发授权规则**选项卡或**委派授权规则**选项卡\(基于的类型所需的授权规则\)，然后单击**添加规则**以启动**添加授权声明规则向导**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)  
5.  上**选择规则模板**页面上，在**声明规则模板**，选择**允许所有用户**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall6.PNG)    
6.  上**配置规则**页上，单击**完成**。  
  
7.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
