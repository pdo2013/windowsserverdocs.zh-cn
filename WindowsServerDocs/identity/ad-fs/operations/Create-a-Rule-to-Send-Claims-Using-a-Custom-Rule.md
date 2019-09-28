---
ms.assetid: 38eb3726-e97b-484e-9926-67e8a046b0c5
title: 创建规则以使用自定义规则发送声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8cd3e6d0073061710bd9ee76958891e036688472
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407605"
---
# <a name="create-a-rule-to-send-claims-using-a-custom-rule"></a>创建规则以使用自定义规则发送声明


通过使用 Active Directory 联合身份验证服务（AD FS）中的 "**使用自定义规则发送声明**" 模板，可以创建自定义声明规则，以便在标准规则模板不满足组织要求的情况下使用。 自定义声明规则以声明规则语言编写，然后必须复制到 "**自定义规则**" 文本框中，然后才能将其用于规则集。 有关构造高级规则的语法的信息，请参阅[声明规则语言的角色](../../ad-fs/technical-reference/The-Role-of-the-Claim-Rule-Language.md)。  
  
你可以使用以下过程通过 "AD FS 管理" snap @ no__t-0in 创建声明规则。  
  
在本地计算机上， **Administrators**中的成员身份或同等身份是完成此过程的最低要求。  可在[本地默认组和域默认组](https://go.microsoft.com/fwlink/?LinkId=83477)中查看有关使用适合的帐户和组成员身份的详细信息。



## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>创建规则以在 Windows Server 2016 中的信赖方信任上传递或筛选传入声明 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**信赖方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明颁发策略**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 "**编辑声明颁发策略**" 对话框中的 "**颁发转换规则**" 下，单击 "**添加规则**" 以启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称。 在 "**自定义规则**" 下，键入或粘贴要用于此规则的声明规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  单击 **“完成”** 。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。   
  
## <a name="to-create-a-rule-to-pass-through-or-filter-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>创建规则以在 Windows Server 2016 中通过声明提供方信任传递或筛选传入声明 
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**声明提供方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 "**编辑声明规则**" 对话框中的 "**接受转换规则**" 下，单击 "**添加规则**" 以启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom3.PNG)   
  
6.  在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称。 在 "**自定义规则**" 下，键入或粘贴要用于此规则的声明规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom4.PNG)     

7.  单击 **“完成”** 。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。   

















   
  
## <a name="to-create-a-rule-to-send-claims-by-using-a-custom-claim-in-windows-server-2012-r2"></a>使用 Windows Server 2012 R2 中的自定义声明创建用于发送声明的规则 
  
1.  在服务器管理器中，单击 "**工具**"，然后单击 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS\\信任关系**" 下，单击 "**声明提供方信任**或**信赖方信任**"，然后在要创建此规则的列表中单击特定信任。  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  在 "**编辑声明规则**" 对话框中，选择下列选项卡，其中一个选项卡依赖于你正在编辑的信任，以及你要在哪个规则集中创建此规则，然后单击 "**添加规则**" 以启动与该规则集关联的规则向导:  
  
    -   **接受转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**使用自定义规则发送声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom1.PNG)   
  
6.  在 "**配置规则**" 页上的 "**声明规则名称**" 下，键入此规则的显示名称。 在 "**自定义规则**" 下，键入或粘贴要用于此规则的声明规则语言语法。  
![创建规则](media/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule/custom2.PNG)     

7.  单击 **“完成”** 。  
  
8.  在 "**编辑声明规则**" 对话框中，单击 **"确定"** 保存规则。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供方信任创建声明规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
