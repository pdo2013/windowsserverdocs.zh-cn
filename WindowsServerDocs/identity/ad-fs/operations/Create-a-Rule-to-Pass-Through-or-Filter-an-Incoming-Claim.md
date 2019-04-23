---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: 创建规则，以传递或筛选传入声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 50f50cd4e096b107a2b58ac05328ff8ed413f2dc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860268"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>创建规则，以传递或筛选传入声明

>适用于：Windows Server 2016, Windows Server 2012 R2

使用传递或筛选传入声明规则模板在 Active Directory 联合身份验证服务\(AD FS\)，可以通过使用所选的声明类型的所有传入声明。 此外可以使用所选的声明类型筛选传入声明的值。 例如，可以使用此规则模板来创建将发送所有传入组声明的规则。 此外可以使用此规则以发送用户主体名称\(UPN\)结尾的声明@fabrikam。  
  
可以使用以下过程以使用 AD FS 管理管理单元创建声明规则\-中。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建规则，以传递或筛选传入声明上了信赖方信任在 Windows Server 2016 

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在中**编辑声明颁发策略**对话框中的**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**传递或筛选传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  上**配置规则**页**声明规则名称**中键入此规则的显示名称**传入声明类型**在列表中，选择一种声明类型，然后选择其中一个以下选项，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **仅特定声明值传递**  
  
    -   **仅与特定电子邮件后缀值匹配的声明值直通**  
  
    -   **仅特定值开头的声明值直通**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  单击**完成**按钮。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建规则，以传递或筛选传入声明上 Windows Server 2016 中的声明提供方信任 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**声明提供方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在中**编辑声明规则**对话框中的**接受转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**传递或筛选传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  上**配置规则**页**声明规则名称**中键入此规则的显示名称**传入声明类型**在列表中，选择一种声明类型，然后选择其中一个以下选项，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **仅特定声明值传递**  
  
    -   **仅与特定电子邮件后缀值匹配的声明值直通**  
  
    -   **仅特定值开头的声明值直通**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  单击**完成**按钮。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>若要创建规则，以传递或筛选传入声明在 Windows Server 2012 R2

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS FSAD\\信任关系**，单击**声明提供方信任**或**信赖方信任**，然后单击在你想要创建此规则的列表中的特定信任。  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG)   
  
4.  在中**编辑声明规则**对话框中，以下选项卡中选择一个，具体取决于正在编辑的和的规则集的信任想要创建在中，此规则，然后单击**添加规则**启动规则向导这就是与该规则集关联：  
  
    -   **接受转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**传递或筛选传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  上**配置规则**页**声明规则名称**中键入此规则的显示名称**传入声明类型**在列表中，选择一种声明类型，然后选择其中一个以下选项，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **仅特定声明值传递**  
  
    -   **仅与特定电子邮件后缀值匹配的声明值直通**  
  
    -   **仅特定值开头的声明值直通**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  单击**完成**按钮。  
  
8.  在中**编辑声明规则**对话框中，单击**确定**以保存规则。  



  
## <a name="additional-references"></a>其他参考  
[配置声明规则](Configure-Claim-Rules.md)  
  
[何时使用传递或筛选声明规则](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
