---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: "创建规则通过或筛选传入的声明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>创建规则通过或筛选传入的声明

>适用于：Windows Server 2016，Windows Server 2012 R2

在 Active Directory 联合身份验证服务 \(AD FS\) 使用穿透或筛选器传入声称规则模板时，你可以通过传入的所有声明与所选的索赔类型。 你也可以筛选与所选的索赔类型传入声明的值。 例如，可以使用此规则模板创建规则将其发送所有传入组索赔。 你还可以使用此规则发送仅用户结尾的主要名称 \(UPN\) 索赔@fabrikam。  
  
可以使用下面的过程中与广告 FS 管理的索赔规则创建 snap\ 中。  
  
在会员**管理员**，或等效，在本地计算机上的最低要求完成此过程。  查看有关使用相应的帐户的详细信息，并进行分组在会员身份[本地和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)。   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建规则通过或筛选依赖方信任在 Windows Server 2016 上的传入声明 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**穿透或筛选器传入的声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在**配置规则**下页面**声明规则名称**键入的显示名称为此规则，**传入声称类型**在列表中，选择索赔类型，然后选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **只有特定声称值通过**  
  
    -   **通过与特定电子邮件职务值匹配的索赔值**  
  
    -   **通过将启动带有特定值的索赔值**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建规则以通过或筛选索赔提供商信任在 Windows Server 2016 上传入的声明 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**穿透或筛选器传入的声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在**配置规则**下页面**声明规则名称**键入的显示名称为此规则，**传入声称类型**在列表中，选择索赔类型，然后选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **只有特定声称值通过**  
  
    -   **通过与特定电子邮件职务值匹配的索赔值**  
  
    -   **通过将启动带有特定值的索赔值**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>若要创建规则通过或筛选在 Windows Server 2012 R2 传入声明

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

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**穿透或筛选器传入的声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  在**配置规则**下页面**声明规则名称**键入的显示名称为此规则，**传入声称类型**在列表中，选择索赔类型，然后选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **只有特定声称值通过**  
  
    -   **通过与特定电子邮件职务值匹配的索赔值**  
  
    -   **通过将启动带有特定值的索赔值**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  单击**完成**按钮。  
  
8.  在**编辑索赔规则**对话框中，单击**确定**保存规则。  



  
## <a name="additional-references"></a>其他参考  
[配置索赔规则](Configure-Claim-Rules.md)  
  
[何时使用一种穿过或筛选索赔规则](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
