---
ms.assetid: 0039fbbb-b981-4526-a550-f3456ff27635
title: "创建规则转换传入的声明"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3745a0ab9d313223c611e58864dd6b4d747f0624
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="create-a-rule-to-send-an-ad-fs-1x-compatible-claim"></a>创建发送广告 FS 1.x 兼容索赔规则

>适用于：Windows Server 2016，Windows Server 2012 R2


在某些情况下使用 Active Directory 联合身份验证服务 \(AD FS\) 处理索赔，将收到联盟运行的服务器广告 FS 1.0 \ (Windows Server 2003 R2\) 或广告 F 1.1 \ （Windows Server 2008 或 Windows Server 2008 R2\），必须执行以下操作：  
  
-   创建规则将其发送名称 ID 的索赔类型格式 UPN、 电子邮件，或常见的名称。  
  
-   发送的所有其他索赔必须具有以下类型的索赔之一：  
  
    -   广告 FS 1。*x*电子邮件地址  
  
    -   广告 FS 1。*x* UPN  
  
    -   常见的名称  
  
    -   组  
  
    -   任何其他 https://schemas.xmlsoap.org/claims/，如 https://schemas.xmlsoap.org/claims/EmployeeID 开头的索赔类型  
  
根据你的组织的需求，使用以下过程中的一个创建广告 FS 1。*x*兼容 NameID 索赔：  
  
-   创建该问题的广告 FS 1.x 名称 ID 索赔使用规则**穿透或传入声称规则模板筛选**  
  
-   创建该问题的广告 FS 1.x 名称 ID 索赔使用规则**转换传入声称规则模板**。 你可以在你希望更改为新的声明类型，将适用于广告 FS 1 的现有的索赔类型的情况下使用此规则模板。 *x*索赔。  
  
> [!NOTE]  
> 此规则按预期方式工作，确保确认信赖方信任或创建此规则索赔提供商信任已配置为使用**广告 FS 1.0 和 1.1 个人资料**。 




## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建规则处理广告 FS 1。*x*名称 ID 声称穿透使用或筛选依赖方信任在 Windows Server 2016 上的传入声称规则模板 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**穿透或筛选器传入的声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**、 选择**名称 ID**列表中。  
  
8.  在**传入名称 ID 格式**、 选择一种以下广告 FS 1。*x*\-compatible 声称从列表中的格式：  
  
    -   **UPN**  
  
    -   **E\ 邮件**  
  
    -   **常见的名称**  
  
9. 选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **只有特定声称值通过**  
  
    -   **通过与特定电子邮件职务值匹配的索赔值**  
  
    -   **通过将启动带有特定值的索赔值**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. 单击**完成**，然后单击**确定**保存规则。  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建规则处理广告 FS 1。*x*名称 ID 声称穿透使用或筛选上在 Windows Server 2016 信任索赔提供商传入声称规则模板 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**穿透或筛选器传入的声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule4.PNG)    

6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**、 选择**名称 ID**列表中。  
  
8.  在**传入名称 ID 格式**、 选择一种以下广告 FS 1。*x*\-compatible 声称从列表中的格式：  
  
    -   **UPN**  
  
    -   **E\ 邮件**  
  
    -   **常见的名称**  
  
9. 选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **只有特定声称值通过**  
  
    -   **通过与特定电子邮件职务值匹配的索赔值**  
  
    -   **通过将启动带有特定值的索赔值**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs3.PNG)   

10. 单击**完成**，然后单击**确定**保存规则。  

  

## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-relying-party-trust-in-windows-server-2016"></a>若要创建规则转换依赖方信任在 Windows Server 2016 上传入的声明 

1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**信赖方信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule9.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔颁发策略**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule10.PNG)   
  
4.  在**编辑索赔颁发策略**对话框下**颁发转换规则**单击**添加规则**启动规则向导。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule11.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**转换传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**，选择你想要在列表中转换的传入声明的类型。  
  
8.  在**传出声称类型**、 选择**名称 ID**列表中。  
  
9. 在**传出名称 ID 格式**、 选择一种以下广告 FS 1。*x*\-compatible 声称从列表中的格式：  
  
    -   **UPN**  
  
    -   **E\ 邮件**  
  
    -   **常见的名称**  
  
10. 选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **使用不同的传出声明值替换传入的索赔值**  
  
    -   **使用新的 e\ 邮件职务替换传入 e\ 邮件职务声明**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. 单击**完成**，然后单击**确定**保存规则。  

  


## <a name="to-create-a-rule-to-transform-an-incoming-claim-on-a-claims-provider-trust-in-windows-server-2016"></a>若要创建规则转换上在 Windows Server 2016 信任索赔提供商传入声明 
  
1.  在服务器管理器中，单击**工具**，然后选择**广告 FS 管理**。  
  
2.  控制台树中，在下**广告 FS**，单击**索赔提供商信任**。 
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule1.PNG)  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule2.PNG)   
  
4.  在**编辑索赔规则**对话框下**验收转换规则**单击**添加规则**启动规则向导。
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule3.PNG)    

5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**转换传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform3.PNG)      

6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**，选择你想要在列表中转换的传入声明的类型。  
  
8.  在**传出声称类型**、 选择**名称 ID**列表中。  
  
9. 在**传出名称 ID 格式**、 选择一种以下广告 FS 1。*x*\-compatible 声称从列表中的格式：  
  
    -   **UPN**  
  
    -   **E\ 邮件**  
  
    -   **常见的名称**  
  
10. 选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **使用不同的传出声明值替换传入的索赔值**  
  
    -   **使用新的 e\ 邮件职务替换传入 e\ 邮件职务声明**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs4.PNG)    

11. 单击**完成**，然后单击**确定**保存规则。  













  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-pass-through-or-filter-an-incoming-claim-rule-template-on-windows-server-2012-r2"></a>若要创建规则处理广告 FS 1。*x*名称 ID 声称穿透使用或筛选器将规则模板传入声称 Windows Server 2012 R2 上
  
1.  在服务器管理器中，单击**工具**，然后单击**广告 FS 管理**。  
  
2.  控制台树中下,**广告 FS\\Trust 关系**，单击以下任何一**索赔提供商信任**或**信赖方信任**，然后单击你想要创建该规则列表中的特定信任。  
  
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
  
6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**、 选择**名称 ID**列表中。  
  
8.  在**传入名称 ID 格式**、 选择一种以下广告 FS 1。*x*\-compatible 声称从列表中的格式：  
  
    -   **UPN**  
  
    -   **E\ 邮件**  
  
    -   **常见的名称**  
  
9. 选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **只有特定声称值通过**  
  
    -   **通过与特定电子邮件职务值匹配的索赔值**  
  
    -   **通过将启动带有特定值的索赔值**  
![创建规则](media/\Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs1.PNG)   

10. 单击**完成**，然后单击**确定**保存规则。  

  
## <a name="to-create-a-rule-to-issue-an-ad-fs-1x-name-id-claim-using-the-transform-an-incoming-claim-rule-template-in-windows-server-2012-r2"></a>若要创建规则处理广告 FS 1。*x*名称 ID 索赔转换传入声称规则模板使用在 Windows Server 2012 R2  
  
1.  在服务器管理器中，单击**工具**，然后单击**广告 FS 管理**。  
  
2.  控制台树中下,**广告 FS\\Trust 关系**，单击以下任何一**索赔提供商信任**或**信赖方信任**，然后单击你想要创建该规则列表中的特定信任。  
  
3.  Right\ 单击所选信任，然后单击**编辑索赔规则**。  
![创建规则](media/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim/claimrule6.PNG) 
  
4.  在**编辑索赔规则**对话框中，选择一个以下选项卡，这取决于你正在编辑，并将你的规则中设置信任想要创建此规则，，然后单击**添加规则**启动程序与该规则集规则向导：  
  
    -   **验收转换规则**  
  
    -   **颁发转换规则**  
  
    -   **颁发授权规则**  
  
    -   **委派授权规则**  
![创建规则](media/Create-a-Rule-to-Permit-All-Users/permitall5.PNG)
  
5.  在**选择规则模板**页面上下,**索赔规则模板**、 选择**转换传入声明**从列表中，然后单击**下一步**。  
![创建规则](media/Create-a-Rule-to-Transform-an-Incoming-Claim/transform1.PNG)   
  
6.  在**配置规则**页上，键入声明规则名称。  
  
7.  在**传入声称类型**，选择你想要在列表中转换的传入声明的类型。  
  
8.  在**传出声称类型**、 选择**名称 ID**列表中。  
  
9. 在**传出名称 ID 格式**、 选择一种以下广告 FS 1。*x*\-compatible 声称从列表中的格式：  
  
    -   **UPN**  
  
    -   **E\ 邮件**  
  
    -   **常见的名称**  
  
10. 选择以下选项，具体取决于你的组织的需求之一：  
  
    -   **通过所有索赔值**  
  
    -   **使用不同的传出声明值替换传入的索赔值**  
  
    -   **使用新的 e\ 邮件职务替换传入 e\ 邮件职务声明**  
![创建规则](media/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim/adfs2.PNG)    

11. 单击**完成**，然后单击**确定**保存规则。  

## <a name="additional-references"></a>其他参考 
[配置索赔规则](Configure-Claim-Rules.md)  
 
[清单：创建适用于信赖的方信任索赔规则](https://technet.microsoft.com/library/ee913578.aspx)  

[信任清单：创建索赔提供商的索赔规则](https://technet.microsoft.com/library/ee913564.aspx)  
  
[何时使用授权索赔规则](../../ad-fs/technical-reference/When-to-Use-an-Authorization-Claim-Rule.md)  

[索赔的作用](../../ad-fs/technical-reference/The-Role-of-Claims.md)  
  
[角色的索赔规则](../../ad-fs/technical-reference/The-Role-of-Claim-Rules.md) 
