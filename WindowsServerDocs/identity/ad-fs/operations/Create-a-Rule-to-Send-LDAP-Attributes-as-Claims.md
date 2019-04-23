---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: 创建规则以声明方式发送 LDAP 属性
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9762e4bc50a1c2b862999af5269a0da376ec9a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887608"
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>创建规则以声明方式发送 LDAP 属性

>适用于：Windows Server 2016, Windows Server 2012 R2

使用发送 LDAP 属性作为 Active Directory 联合身份验证服务中的声明规则模板\(AD FS\)，可以创建一个规则，将从一种轻型目录访问协议中选择属性\(LDAP\)属性存储，如 Active Directory，以向信赖方的声明的形式发送。 例如，使用此规则模板创建声明规则，将提取属性值已经过身份验证用户能够从 a Send LDAP Attributes **displayName**并**telephoneNumber** Active目录属性，然后将这些值作为两个不同的传出声明发送。  
  
还可以使用此规则发送所有用户的组成员身份。 如果要仅发送单个组成员身份，请使用“以声明方式发送组成员身份”规则模板。 可以使用以下过程以使用 AD FS 管理管理单元创建声明规则\-中。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>若要创建一个规则以以为信赖方信任 Windows Server 2016 中的声明方式发送 LDAP 属性 

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在中**编辑声明颁发策略**对话框中的**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**以声明方式发送 LDAP 属性**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  上**配置规则**页**声明规则名称**键入此规则中，选择的显示名称**属性存储**，然后选择 LDAP 属性并将其映射到传出声明类型。 
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  单击**完成**按钮。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>若要创建一个规则以发送 LDAP 属性作为声明为 Windows Server 2016 中的声明提供方信任 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**声明提供方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在中**编辑声明规则**对话框中的**接受转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**以声明方式发送 LDAP 属性**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  上**配置规则**页**声明规则名称**键入此规则中，选择的显示名称**属性存储**，然后选择 LDAP 属性并将其映射到传出声明类型。 
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  单击**完成**按钮。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>若要创建一个规则以发送 LDAP 属性作为声明适用于 Windows Server 2012 R2  
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS FSAD\\信任关系**，单击**声明提供方信任**或**信赖方信任**，然后单击在你想要创建此规则的列表中的特定信任。  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在中**编辑声明规则**对话框中，以下选项卡中选择一个，具体取决于正在编辑的和的规则集的信任想要创建在中，此规则，然后单击**添加规则**启动规则与该规则集关联的向导：  
  
    -   **接受转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  上**选择规则模板**页面上，在**声明规则模板**，选择**以声明方式发送 LDAP 属性**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  上**配置规则**页**声明规则名称**下键入此规则的显示名称**属性存储**选择**Active Directory**，并在**映射的 LDAP 属性到传出声明类型**选择所需**LDAP 属性**对应**传出声明类型**从下拉类型\-列表。  
  
    您必须选择一个新的 LDAP 属性和为你想要为此规则的一部分发出的声明每个 Active Directory 属性不同的行上的传出声明类型对。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  单击**完成**按钮。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供程序创建声明规则信任](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
