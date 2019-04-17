---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: "创建规则应用于发送组成员作为声明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d4fb03de9de2dcca36b18ce089db11ed599de820
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>创建规则应用于发送组成员作为声明

>适用于：Windows Server 2016，Windows Server 2012 R2

作为中 Active Directory 联合身份验证服务 \(AD FS\) 的索赔规则模板使用发送组成员资格，可以创建规则可使您可以选择要将作为索赔发送 Active Directory 安全组。 此规则，根据你选择的组将激发单个声明。 例如，可以使用此规则模板创建将发送组索赔值的管理员，用户是否的域管理员安全组成员规则。 此规则应仅用于的本地 Active Directory 域中的用户。  
  
可以使用下面的过程中与广告 FS 管理的索赔规则创建 snap\ 中。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建发送作为声明在依赖方信任 Windows Server 2016 中的组成员规则 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**发送作为声明的组成员**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   在**配置规则**下页面**声明规则名称**键入的显示名称为此规则，**用户的组**单击**浏览**下选择一个组中，**传出声称类型**选择所需的索赔类型，然后在**传出声称类型**键入值。
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。
  
## <a name="to-create-a-rule-to-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建多作为声明在 Windows Server 2016 中信任索赔提供商发送组成员规则 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**发送作为声明的组成员**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   在**配置规则**下页面**声明规则名称**键入的显示名称为此规则，**用户的组**单击**浏览**下选择一个组中，**传出声称类型**选择所需的索赔类型，然后在**传出声称类型**键入值。 
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>若要创建发送组成员作为 Windows Server 2012 R2 的索赔规则 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中下,**广告 FS\\Trust 关系**，单击以下任何一**索赔提供商信任**或**信赖方信任**，然后单击你想要创建该规则列表中的特定信任。  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在**编辑索赔规则**对话框中，选择一个以下选项卡，具体取决于正在编辑，并哪个规则设置您的信任想要创建该，规则，然后单击**添加规则**启动程序与该规则集规则向导：  
  
    -   **验收转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**作为索赔发送组成员**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  在**配置规则**下页面**声明规则名称**键入的显示名称为此规则，**用户的组**单击**浏览**下选择一个组中，**传出声称类型**选择所需的索赔类型，然后在**传出声称类型**键入值。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  单击**完成**。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  



## <a name="additional-references"></a>其他参考 
[配置索赔规则](Configure-Claim-Rules.md)  
 
[清单：创建适用于信赖的方信任索赔规则](https://technet.microsoft.com/library/ee913578.aspx)  

[信任清单：创建索赔提供商的索赔规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权索赔规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
