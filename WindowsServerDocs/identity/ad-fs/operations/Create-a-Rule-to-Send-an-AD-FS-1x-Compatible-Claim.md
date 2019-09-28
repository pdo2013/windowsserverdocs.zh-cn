---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: 创建规则以转换传入声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d4e3a59ff9154a948143cef32103cae9f1f2d235
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407593"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>创建规则以发送 AD FS 1.x 兼容声明

如果你使用的是 Active Directory 联合身份验证服务 @no__t 0AD FS @ no__t-1）发出声明，这些声明将由运行 AD FS 1.0 \(Windows Server 2003 R2 @ no__t-3 或 AD FS 1.1 \(Windows Server 2008 或 Windows 的联合服务器接收服务器 2008 R2 @ no__t，必须执行以下操作：  
  
-   创建一个规则，该规则将使用 UPN、电子邮件或公用名格式发送名称 ID 声明类型。  
  
-   发送的其他所有声明都必须具有以下声明类型之一：  
  
    -   AD FS 1。*x*电子邮件地址  
  
    -   AD FS 1。*x* UPN  
  
    -   公用名  
  
    -   Group  
  
    -   任何其他声明类型，以 https://schemas.xmlsoap.org/claims/ 开头，如 https://schemas.xmlsoap.org/claims/EmployeeID  
  
根据组织的需要，使用以下过程之一创建 AD FS 1。*x*兼容的 NameID 声明：  
  
-   使用 "**传递或筛选传入声明" 规则模板**创建此规则以发出 AD FS 1.X 名称 ID 声明  
  
-   创建此规则以使用 "**转换传入声明" 规则模板**发出 AD FS 1.X 名称 ID 声明。 如果要将现有声明类型更改为将与 AD FS 1 一起使用的新声明类型，则可以使用此规则模板。 *x*声明。  
  
> [!NOTE]  
> 要使此规则按预期运行，请确保已将创建此规则的信赖方信任或声明提供方信任配置为使用**AD FS 1.0 和1.1 配置文件**。 

## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>创建规则以发出 AD FS 1。*x*名称 ID 声明使用在 Windows Server 2016 中的依赖方信任上传递或筛选传入声明规则模板 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**信赖方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明颁发策略**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在 "**编辑声明颁发策略**" 对话框中的 "**颁发转换规则**" 下，单击 "**添加规则**" 以启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "通过"**或 "筛选传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在 "**配置规则**" 页上，键入声明规则名称。  
  
7.  在 "**传入声明类型**" 中，选择列表中的 "**名称 ID** "。  
  
8.  在 "**传入名称 ID 格式**" 中，选择以下 AD FS 1 之一。*x*@no__t 从列表中2compatible 声明格式：  
  
    -   **UPN**  
  
    -   **E @ no__t-1Mail**  
  
    -   **公用名**  
  
9. 根据组织的需要，选择下列选项之一：  
  
    -   **传递所有声明值**  
  
    -   **仅传递特定的声明值**  
  
    -   **仅传递与特定电子邮件后缀值匹配的声明值**  
  
    -   **仅传递以特定值开头的声明值**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. 单击 "**完成**"，然后单击 **"确定"** 保存规则。  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>创建规则以发出 AD FS 1。*x*名称 ID 声明，在 Windows Server 2016 中的声明提供方信任上使用 "传递" 或 "筛选传入声明" 规则模板 
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**声明提供方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 "**编辑声明规则**" 对话框中的 "**接受转换规则**" 下，单击 "**添加规则**" 以启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，选择 "通过"**或 "筛选传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在 "**配置规则**" 页上，键入声明规则名称。  
  
7.  在 "**传入声明类型**" 中，选择列表中的 "**名称 ID** "。  
  
8.  在 "**传入名称 ID 格式**" 中，选择以下 AD FS 1 之一。*x*@no__t 从列表中2compatible 声明格式：  
  
    -   **UPN**  
  
    -   **E @ no__t-1Mail**  
  
    -   **公用名**  
  
9. 根据组织的需要，选择下列选项之一：  
  
    -   **传递所有声明值**  
  
    -   **仅传递特定的声明值**  
  
    -   **仅传递与特定电子邮件后缀值匹配的声明值**  
  
    -   **仅传递以特定值开头的声明值**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. 单击 "**完成**"，然后单击 **"确定"** 保存规则。  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>在 Windows Server 2016 中创建规则以转换信赖方信任上的传入声明 

1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**信赖方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明颁发策略**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)
  
4.  在 "**编辑声明颁发策略**" 对话框中的 "**颁发转换规则**" 下，单击 "**添加规则**" 以启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**转换传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  在 "**配置规则**" 页上，键入声明规则名称。  
  
7.  在 "**传入声明类型**" 中，选择要在列表中转换的传入声明的类型。  
  
8.  在 "**传出声明类型**" 中，选择列表中的 "**名称 ID** "。  
  
9. 在 "**传出名称 ID 格式**" 中，选择以下 AD FS 1 之一。*x*@no__t 从列表中2compatible 声明格式：  
  
    -   **UPN**  
  
    -   **E @ no__t-1Mail**  
  
    -   **公用名**  
  
10. 根据组织的需要，选择下列选项之一：  
  
    -   **传递所有声明值**  
  
    -   **将传入声明值替换为不同的传出声明值**  
  
    -   **将传入的 e @ no__t-1mail 后缀声明替换为新的 e @ no__t-2mail 后缀**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. 单击 "**完成**"，然后单击 **"确定"** 保存规则。  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>创建规则以在 Windows Server 2016 中的声明提供方信任上转换传入声明 
  
1.  在服务器管理器中，单击 "**工具**"，然后选择 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS**下，单击"**声明提供方信任**"。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右键\-单击所选的信任，然后单击 "**编辑声明规则**"。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在 "**编辑声明规则**" 对话框中的 "**接受转换规则**" 下，单击 "**添加规则**" 以启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**转换传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在 "**配置规则**" 页上，键入声明规则名称。  
  
7.  在 "**传入声明类型**" 中，选择要在列表中转换的传入声明的类型。  
  
8.  在 "**传出声明类型**" 中，选择列表中的 "**名称 ID** "。  
  
9. 在 "**传出名称 ID 格式**" 中，选择以下 AD FS 1 之一。*x*@no__t 从列表中2compatible 声明格式：  
  
    -   **UPN**  
  
    -   **E @ no__t-1Mail**  
  
    -   **公用名**  
  
10. 根据组织的需要，选择下列选项之一：  
  
    -   **传递所有声明值**  
  
    -   **将传入声明值替换为不同的传出声明值**  
  
    -   **将传入的 e @ no__t-1mail 后缀声明替换为新的 e @ no__t-2mail 后缀**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. 单击 "**完成**"，然后单击 **"确定"** 保存规则。  













  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>创建规则以发出 AD FS 1。在 Windows Server 2012 R2 上使用 "传递" 或 "筛选传入声明" 规则模板的*x*名称 ID 声明
  
1.  在服务器管理器中，单击 "**工具**"，然后单击 " **AD FS 管理**"。  
  
2.  在控制台树中的 " **AD FS\\信任关系**" 下，单击 "**声明提供方信任**或**信赖方信任**"，然后在要创建此规则的列表中单击特定信任。  
  
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
  
6.  在 "**配置规则**" 页上，键入声明规则名称。  
  
7.  在 "**传入声明类型**" 中，选择列表中的 "**名称 ID** "。  
  
8.  在 "**传入名称 ID 格式**" 中，选择以下 AD FS 1 之一。*x*@no__t 从列表中2compatible 声明格式：  
  
    -   **UPN**  
  
    -   **E @ no__t-1Mail**  
  
    -   **公用名**  
  
9. 根据组织的需要，选择下列选项之一：  
  
    -   **传递所有声明值**  
  
    -   **仅传递特定的声明值**  
  
    -   **仅传递与特定电子邮件后缀值匹配的声明值**  
  
    -   **仅传递以特定值开头的声明值**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)

10. 单击 "**完成**"，然后单击 **"确定"** 保存规则。  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>创建规则以发出 AD FS 1。*x*名称 ID 声明使用 Windows Server 2012 R2 中的 "转换传入声明" 规则模板  
  
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
  
5.  在 "**选择规则模板**" 页上的 "**声明规则模板**" 下，从列表中选择 "**转换传入声明**"，然后单击 "**下一步**"。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   
  
6.  在 "**配置规则**" 页上，键入声明规则名称。  
  
7.  在 "**传入声明类型**" 中，选择要在列表中转换的传入声明的类型。  
  
8.  在 "**传出声明类型**" 中，选择列表中的 "**名称 ID** "。  
  
9. 在 "**传出名称 ID 格式**" 中，选择以下 AD FS 1 之一。*x*@no__t 从列表中2compatible 声明格式：  
  
    -   **UPN**  
  
    -   **E @ no__t-1Mail**  
  
    -   **公用名**  
  
10. 根据组织的需要，选择下列选项之一：  
  
    -   **传递所有声明值**  
  
    -   **将传入声明值替换为不同的传出声明值**  
  
    -   **将传入的 e @ no__t-1mail 后缀声明替换为新的 e @ no__t-2mail 后缀**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. 单击 "**完成**"，然后单击 **"确定"** 保存规则。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供方信任创建声明规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
