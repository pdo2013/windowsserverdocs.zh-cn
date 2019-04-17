---
ms.assetid: 66664b80-2590-46c0-bfca-82402088e42c
title: "创建规则作为索赔发送 LDAP 属性"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e9762e4bc50a1c2b862999af5269a0da376ec9a1
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-send-ldap-attributes-as-claims"></a>创建规则作为索赔发送 LDAP 属性

>适用于：Windows Server 2016，Windows Server 2012 R2

作为中 Active Directory 联合身份验证服务 \(AD FS\) 的索赔规则模板使用发送 LDAP 属性，可以创建将便捷目录访问协议 \(LDAP\) 特性官方商城内，如 Active Directory 发送到依赖方索赔作为选择属性规则。 例如，你可以使用此规则模板创建发送 LDAP 属性，如索赔规则，将解压缩从授权的用户特性值**显示名称**和**telephoneNumber** Active Directory 属性，然后将这些值发送作为两种不同的传出索赔。  
  
你还可以使用此规则向所有用户的组成员发送。 如果你想要发送仅个别的组成员资格，用作索赔规则模板发送组成员。 可以使用下面的过程中与广告 FS 管理的索赔规则创建 snap\ 中。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。  

## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-relying-party-trust-in-windows-server-2016"></a>若要创建规则依赖方信任在 Windows Server 2016 提起的索赔作为发送 LDAP 属性 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**作为索赔发送 LDAP 属性**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)    

6.  在**配置规则**下页面**声明规则名称**键入规则，选择显示名称**特性官方商城**，，选择 LDAP 属性，然后将其映射到传出索赔类型。 
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)    

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-a-claims-provider-trust-in-windows-server-2016"></a>若要创建规则以在 Windows Server 2016 信任索赔提供商提起的索赔的形式发送 LDAP 属性 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**作为索赔发送 LDAP 属性**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap1.PNG)       

6.  在**配置规则**下页面**声明规则名称**键入规则，选择显示名称**特性官方商城**，，选择 LDAP 属性，然后将其映射到传出索赔类型。 
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap2.PNG)      

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  

 
  
## <a name="to-create-a-rule-to-send-ldap-attributes-as-claims-for-windows-server-2012-r2"></a>若要创建规则作为 Windows Server 2012 R2 的索赔发送 LDAP 属性  
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中下,**广告 FSAD FS\\Trust 关系**，单击以下任何一**索赔提供商信任**或**信赖方信任**，然后单击你想要创建该规则列表中的特定信任。  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)  
  
4.  在**编辑索赔规则**对话框中，选择一个以下选项卡，具体取决于正在编辑，并哪个规则设置您的信任想要创建该，规则，然后单击**添加规则**启动程序与该规则集规则向导：  
  
    -   **验收转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG) 
  
5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**作为索赔发送 LDAP 属性**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap3.PNG)  
  
6.  上**配置规则**下页面**声明规则名称**下键入的显示名称为此规则，**特性官方商城**选择**Active Directory**下,**传出到映射的 LDAP 属性声称类型**选择所需**LDAP 特性**和相应**传出声称类型**drop\ 下拉列表中的类型。  
  
    你必须选择一个新 LDAP 特性和传出为你想要为此规则的一部分发出的索赔每个 Active Directory 特性不同行的索赔类型配对。  
![创建规则](media/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims/ldap4.PNG)    
7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  

## <a name="additional-references"></a>其他参考 
[配置索赔规则](Configure-Claim-Rules.md)  
 
[清单：创建适用于信赖的方信任索赔规则](https://technet.microsoft.com/library/ee913578.aspx)  

[信任清单：创建索赔提供商的索赔规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权索赔规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
