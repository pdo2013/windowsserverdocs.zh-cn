---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: 创建规则以将 LDAP 属性作为声明发送
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d391c77ba309c84e2f8f8d0676b71b7c198fc241
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865943"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>创建规则以将 LDAP 属性作为声明发送


使用 "以声明方式发送 LDAP 属性" 规则模板\(Active Directory 联合身份验证服务\)AD FS，你可以创建一个规则，该规则将从轻型目录访问协议\(LDAP\)中选择属性要作为声明发送给信赖方的属性存储，如 Active Directory。 例如，你可以使用此规则模板创建一个发送 LDAP 属性作为声明规则，该规则将从**displayName**和**telephoneNumber** Active Directory 属性中提取经过身份验证的用户的属性值，然后将这些属性值发送到值作为两个不同的传出声明。  
  
你还可以使用此规则发送所有用户的组成员身份。 如果要仅发送单个组成员身份，请使用“以声明方式发送组成员身份”规则模板。 你可以使用以下过程通过 AD FS 管理 "管理单元\-来创建声明规则。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>创建规则以在 Windows Server 2016 中将 LDAP 属性作为信赖方信任的声明发送 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**信赖方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明颁发策略**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 "**编辑声明颁发策略**" 对话框中的 "**颁发转换规则**" 下，单击 "**添加规则**" 以启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**以声明方式从列表发送 LDAP 属性**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  在 "**声明规则名称**" 下的 "**配置规则**" 页面上，键入此规则的 "显示名称"，选择 "**属性存储**"，然后选择 LDAP 属性，并将其映射到 "传出声明类型"。 
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  单击 "**完成**" 按钮。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>创建规则以在 Windows Server 2016 中将 LDAP 属性作为声明提供方信任的声明发送 
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**声明提供方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 "**编辑声明规则**" 对话框中的 "**接受转换规则**" 下，单击 "**添加规则**" 以启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**以声明方式从列表发送 LDAP 属性**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  在 "**声明规则名称**" 下的 "**配置规则**" 页面上，键入此规则的 "显示名称"，选择 "**属性存储**"，然后选择 LDAP 属性，并将其映射到 "传出声明类型"。 
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  单击 "**完成**" 按钮。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>创建将 LDAP 属性作为 Windows Server 2012 R2 的声明进行发送的规则  
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FSAD FS\\信任关系**" 下，单击 "**声明提供方**信任或**信赖方信任**"，然后在要创建此规则的列表中单击特定信任。  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在 "**编辑声明规则**" 对话框中，根据所编辑的信任和要在其中创建此规则的规则集，选择下列选项卡之一，然后单击 "**添加规则**" 以启动与该规则集关联的规则向导:  
  
    -   **接受转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "**以声明方式从列表发送 LDAP 属性**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  在 "**声明规则名称**" 下的 "**配置规则**" 页上，键入此规则的显示名称，在 "**属性存储**" 下选择 " **Active Directory**"，在 "**将 LDAP 属性映射到传出声明类型**" 下选择所需的下拉\-列表中的**LDAP 属性**和对应的**传出声明类型**类型。  
  
    对于要作为此规则的一部分发出声明的每个 Active Directory 属性，必须在不同的行上选择新的 LDAP 属性和传出声明类型对。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  单击 "**完成**" 按钮。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供方信任创建声明规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
