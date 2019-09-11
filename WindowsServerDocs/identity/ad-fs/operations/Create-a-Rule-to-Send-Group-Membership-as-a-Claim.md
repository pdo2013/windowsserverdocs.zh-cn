---
ms.assetid: 475e34f9-9399-43f4-a840-9dd77258e11a
title: 创建规则以声明方式发送组成员身份
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 86111f8f7da7be1d33bd6ce07385805a9a3b3df8
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865924"
---
# <a name="create-a-rule-to-send-group-membership-as-a-claim"></a>创建规则以声明方式发送组成员身份

使用 "以声明方式发送组成员身份" 规则模板\(Active Directory 联合身份验证服务\)AD FS，你可以创建一个规则，该规则使你可以选择要以声明形式发送的 Active Directory 安全组。 基于你选择的组，将仅从此规则发出一个声明。 例如，如果用户是 Domain Admins 安全组的成员，则可以使用此规则模板创建一个规则，该规则将以管理员的值发送组声明。 此规则只应用于本地 Active Directory 域中的用户。  
  
你可以使用以下过程通过 AD FS 管理 "管理单元\-来创建声明规则。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   

## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-relying-party-trust-in-windows-server-2016"></a>创建规则以将组成员身份作为声明发送到 Windows Server 2016 中的信赖方信任 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**信赖方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明颁发策略**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 "**编辑声明颁发策略**" 对话框中的 "**颁发转换规则**" 下，单击 "**添加规则**" 以启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**发送组成员身份作为声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)      

6.   在 "**声明规则名称**" 下的 "**配置规则**" 页上，在 "**用户组**" 中单击 "**浏览**"，然后选择一个组，在 "**传出声明类型**" 下选择所需的声明类型，然后在**传出声明类型**键入值。
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)   

7.  单击 "**完成**" 按钮。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。
  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>创建规则以将组成员身份作为声明发送到 Windows Server 2016 中的声明提供方信任 
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**声明提供方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 "**编辑声明规则**" 对话框中的 "**接受转换规则**" 下，单击 "**添加规则**" 以启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**发送组成员身份作为声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group3.PNG)     

6.   在 "**声明规则名称**" 下的 "**配置规则**" 页上，在 "**用户组**" 中单击 "**浏览**"，然后选择一个组，在 "**传出声明类型**" 下选择所需的声明类型，然后在**传出声明类型**键入值。 
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group4.PNG)      

7.  单击 "**完成**" 按钮。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。  




  
## <a name="to-create-a-rule-to-send-group-membership-as-a-claim-in-windows-server-2012-r2"></a>创建规则以在 Windows Server 2012 R2 中将组成员身份作为声明发送 
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS\\信任关系**" 下，单击 "**声明提供方信任**或**信赖方信任**"，然后在要创建此规则的列表中单击特定信任。  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在 "**编辑声明规则**" 对话框中，根据所编辑的信任和要在其中创建此规则的规则集，选择下列选项卡之一，然后单击 "**添加规则**" 以启动与该规则集关联的规则向导:  
  
    -   **接受转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
    
5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**以声明方式发送组成员身份**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group1.PNG)

6.  在 "**声明规则名称**" 下的 "**配置规则**" 页上，在 "**用户组**" 中单击 "**浏览**"，然后选择一个组，在 "**传出声明类型**" 下选择所需的声明类型，然后在**传出声明类型**键入值。  
![创建规则](media/Create-a-Rule-to-Send-Group-Membership-as-a-Claim/group2.PNG)  

7.  单击 **“完成”** 。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。  



## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供方信任创建声明规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
