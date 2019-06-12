---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: 创建规则以转换传入声明
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bda071be6668710361205643125fc8ad44246012
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453021"
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>创建一个规则以发送 AD FS 1.x 兼容声明

在使用 Active Directory 联合身份验证服务的情况下\(AD FS\)为将由运行 AD FS 1.0 的联合身份验证服务器接收的问题声明\(Windows Server 2003 R2\)或 AD FS 1.1 \(Windows Server 2008 或 Windows Server 2008 R2\)，必须执行以下操作：  
  
-   创建一个将发送具有格式 UPN、 电子邮件或公用名的名称 ID 声明类型的规则。  
  
-   发送的其他所有声明必须都具有以下声明类型之一：  
  
    -   AD FS 1。*x*电子邮件地址  
  
    -   AD FS 1。*x* UPN  
  
    -   公用名  
  
    -   Group  
  
    -   开头的任何其他声明类型 https://schemas.xmlsoap.org/claims/，如 https://schemas.xmlsoap.org/claims/EmployeeID  
  
具体取决于你组织的需要，使用以下过程之一来创建与 AD FS 1。*x*兼容的 NameID 声明：  
  
-   创建此规则应用于问题与 AD FS 1.x 名称 ID 声明使用**传递或筛选传入声明规则模板**  
  
-   创建此规则应用于问题与 AD FS 1.x 名称 ID 声明使用**转换传入声明规则模板**。 在想要将现有的声明类型更改为新的声明类型，将使用 AD FS 1 的情况下，可以使用此规则模板。 *x*声明。  
  
> [!NOTE]  
> 对于此规则中按预期方式工作，请确保是否信赖方信任或要在其中创建此规则的声明提供方信任已配置为使用**AD FS 1.0 和 1.1 配置文件**。 

## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建一个规则以发出与 AD FS 1。*x*名称 ID 声明使用传递或筛选传入声明规则模板上了信赖方信任在 Windows Server 2016 

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在中**编辑声明颁发策略**对话框中的**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**传递或筛选传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择**名称 ID**列表中。  
  
8.  在中**传入名称 ID 格式**中，选择一个以下 AD FS 1。*x*\-兼容声明来自列表的格式：  
  
    -   **UPN**  
  
    -   **E\-邮件**  
  
    -   **公用名**  
  
9. 选择以下选项之一，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **仅特定声明值传递**  
  
    -   **仅与特定电子邮件后缀值匹配的声明值直通**  
  
    -   **仅特定值开头的声明值直通**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. 单击**完成**，然后单击**确定**以保存规则。  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建一个规则以发出与 AD FS 1。*x*名称 ID 声明使用传递或筛选传入声明规则模板在 Windows Server 2016 中的声明提供方信任 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**声明提供方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在中**编辑声明规则**对话框中的**接受转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**传递或筛选传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择**名称 ID**列表中。  
  
8.  在中**传入名称 ID 格式**中，选择一个以下 AD FS 1。*x*\-兼容声明来自列表的格式：  
  
    -   **UPN**  
  
    -   **E\-邮件**  
  
    -   **公用名**  
  
9. 选择以下选项之一，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **仅特定声明值传递**  
  
    -   **仅与特定电子邮件后缀值匹配的声明值直通**  
  
    -   **仅特定值开头的声明值直通**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)

10. 单击**完成**，然后单击**确定**以保存规则。  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建一个规则以转换传入声明上了信赖方信任在 Windows Server 2016 

1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)
  
4.  在中**编辑声明颁发策略**对话框中的**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**转换传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)

6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择你想要转换的列表中的传入声明的类型。  
  
8.  在中**传出声明类型**，选择**名称 ID**列表中。  
  
9. 在中**传出名称 ID 格式**中，选择一个以下 AD FS 1。*x*\-兼容声明来自列表的格式：  
  
    -   **UPN**  
  
    -   **E\-邮件**  
  
    -   **公用名**  
  
10. 选择以下选项之一，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **传入声明值替换为不同的传出声明值**  
  
    -   **替换为传入电子\-邮件后缀声明与新的电子\-邮件后缀**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)

11. 单击**完成**，然后单击**确定**以保存规则。  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建一个规则以转换传入声明上 Windows Server 2016 中的声明提供方信任 
  
1.  在服务器管理器中，单击**工具**，然后选择**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS**，单击**声明提供方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在中**编辑声明规则**对话框中的**接受转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  上**选择规则模板**页面上，在**声明规则模板**，选择**转换传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择你想要转换的列表中的传入声明的类型。  
  
8.  在中**传出声明类型**，选择**名称 ID**列表中。  
  
9. 在中**传出名称 ID 格式**中，选择一个以下 AD FS 1。*x*\-兼容声明来自列表的格式：  
  
    -   **UPN**  
  
    -   **E\-邮件**  
  
    -   **公用名**  
  
10. 选择以下选项之一，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **传入声明值替换为不同的传出声明值**  
  
    -   **替换为传入电子\-邮件后缀声明与新的电子\-邮件后缀**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. 单击**完成**，然后单击**确定**以保存规则。  













  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>若要创建一个规则以发出与 AD FS 1。*x*名称 ID 声明使用传递或筛选传入声明规则模板在 Windows Server 2012 R2
  
1.  在服务器管理器中，单击**工具**，然后单击**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS\\信任关系**，单击**声明提供方信任**或**信赖方信任**，然后单击特定你想要创建此规则的列表中的信任。  
  
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
  
6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择**名称 ID**列表中。  
  
8.  在中**传入名称 ID 格式**中，选择一个以下 AD FS 1。*x*\-兼容声明来自列表的格式：  
  
    -   **UPN**  
  
    -   **E\-邮件**  
  
    -   **公用名**  
  
9. 选择以下选项之一，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **仅特定声明值传递**  
  
    -   **仅与特定电子邮件后缀值匹配的声明值直通**  
  
    -   **仅特定值开头的声明值直通**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)

10. 单击**完成**，然后单击**确定**以保存规则。  

  
## <a name="to-create-a-rule-to-issue-an-adfs1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>若要创建一个规则以发出与 AD FS 1。*x*在 Windows Server 2012 R2 中使用转换传入声明规则模板的名称 ID 声明  
  
1.  在服务器管理器中，单击**工具**，然后单击**AD FS 管理**。  
  
2.  在控制台树中下, **AD FS\\信任关系**，单击**声明提供方信任**或**信赖方信任**，然后单击特定你想要创建此规则的列表中的信任。  
  
3.  右\-单击选定的信任，然后单击**编辑声明规则**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  在中**编辑声明规则**以下选项卡，具体取决于你正在编辑和中的规则集的信任对话框中，选择一个想要创建此规则，然后单击**添加规则**启动规则与该规则集关联的向导：  
  
    -   **接受转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  上**选择规则模板**页面上，在**声明规则模板**，选择**转换传入声明**从列表中，然后单击**下一步**.  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   
  
6.  上**配置规则**页上，键入声明规则名称。  
  
7.  在中**传入声明类型**，选择你想要转换的列表中的传入声明的类型。  
  
8.  在中**传出声明类型**，选择**名称 ID**列表中。  
  
9. 在中**传出名称 ID 格式**中，选择一个以下 AD FS 1。*x*\-兼容声明来自列表的格式：  
  
    -   **UPN**  
  
    -   **E\-邮件**  
  
    -   **公用名**  
  
10. 选择以下选项之一，具体取决于你的组织的需求：  
  
    -   **传递所有声明值**  
  
    -   **传入声明值替换为不同的传出声明值**  
  
    -   **替换为传入电子\-邮件后缀声明与新的电子\-邮件后缀**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. 单击**完成**，然后单击**确定**以保存规则。  

## <a name="additional-references"></a>其他参考 
[配置声明规则](Configure-Claim-Rules.md)  
 
[清单：为信赖方信任创建声明规则](https://technet.microsoft.com/library/ee913578.aspx)  

[清单：为声明提供方信任创建声明规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权声明规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[声明的角色](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[声明规则的角色](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
