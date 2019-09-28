---
ms.assetid: 6127963f-71b2-4d8f-8b53-7c525bf06521
title: 创建传递或筛选传入声明的规则
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 145558e620188c4311d79d2a9ba4ed7aaf7b13a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358139"
---
# <a name="create-a-rule-to-pass-through-or-filter-an-incoming-claim"></a>创建传递或筛选传入声明的规则

使用 "传递或筛选传入声明" 规则模板 Active Directory 联合身份验证服务 \(AD FS @ no__t-1 中，可以通过所选声明类型传递所有传入声明。 你还可以使用所选声明类型筛选传入声明的值。 例如，你可以使用此规则模板创建一个规则，该规则将发送所有传入组声明。 你还可以使用此规则仅发送以 @fabrikam 结尾的用户主体名称 \(UPN @ no__t 声明。  
  
你可以使用以下过程通过 AD FS 管理 "管理单元\-来创建声明规则。  
  
本地计算机上的 **Administrators** 中的成员身份或等效身份是完成这些过程所需的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。   

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>创建规则以在 Windows Server 2016 中的信赖方信任上传递或筛选传入声明 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**信赖方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明颁发策略**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 "**编辑声明颁发策略**" 对话框中的 "**颁发转换规则**" 下，单击 "**添加规则**" 以启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "通过"**或 "筛选传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在 "**声明规则名称**" 下的 "**配置规则**" 页上，键入此规则的显示名称，在 "**传入声明类型**" 中选择列表中的声明类型，然后根据组织的需要选择以下选项之一：  
  
    -   **传递所有声明值**  
  
    -   **仅传递特定的声明值**  
  
    -   **仅传递与特定电子邮件后缀值匹配的声明值**  
  
    -   **仅传递以特定值开头的声明值**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  单击 "**完成**" 按钮。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>创建规则以在 Windows Server 2016 中通过声明提供方信任传递或筛选传入声明 
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**声明提供方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 "**编辑声明规则**" 对话框中的 "**接受转换规则**" 下，单击 "**添加规则**" 以启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "通过"**或 "筛选传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在 "**声明规则名称**" 下的 "**配置规则**" 页上，键入此规则的显示名称，在 "**传入声明类型**" 中选择列表中的声明类型，然后根据组织的需要选择以下选项之一：  
  
    -   **传递所有声明值**  
  
    -   **仅传递特定的声明值**  
  
    -   **仅传递与特定电子邮件后缀值匹配的声明值**  
  
    -   **仅传递以特定值开头的声明值**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule5.PNG)    

7.  单击 "**完成**" 按钮。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。  

## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-in-windows-server-2012-r2"></a>创建规则以在 Windows Server 2012 R2 中通过或筛选传入声明

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

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "通过"**或 "筛选传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule7.PNG)    

6.  在 "**声明规则名称**" 下的 "**配置规则**" 页上，键入此规则的显示名称，在 "**传入声明类型**" 中选择列表中的声明类型，然后根据组织的需要选择以下选项之一：  
  
    -   **传递所有声明值**  
  
    -   **仅传递特定的声明值**  
  
    -   **仅传递与特定电子邮件后缀值匹配的声明值**  
  
    -   **仅传递以特定值开头的声明值**  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule8.PNG)    

7.  单击 "**完成**" 按钮。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。  



  
## <a name="additional-references"></a>其他参考  
[配置声明规则](Configure-Claim-Rules.md)  
  
[何时使用传递或筛选声明规则](../../ad-fs/technical-reference/When-to-Use-a-Pass-Through-or-Filter-Claim-Rule.md)  
  
[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md)  
  
