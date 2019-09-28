---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: 跨林部署声明（示范步骤）
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 6045c144a0da399e8279c781273235942316e538
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407122"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>跨林部署声明（示范步骤）

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在本主题中，我们将介绍一种基本方案，说明如何在信任林和受信任林之间配置声明转换。 你将了解如何创建声明转换策略对象并将其链接到信任林和受信任林的信任。 然后，将验证方案。  

## <a name="scenario-overview"></a>方案概述  
Adatum 公司向 Contoso，公司提供金融服务。每个季度，Adatum 会计将其帐户电子表格复制到位于 Contoso，有限公司中的文件服务器上的一个文件夹中。从 Contoso 到 Adatum 有双向信任设置。 Contoso，有限公司需要保护共享，以便只有 Adatum 员工才能访问远程共享。  

在此方案中：  

1.  [设置先决条件和测试环境](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  

2.  [在受信任林（Adatum）上设置声明转换](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  

3.  [在信任林中设置声明转换（Contoso）](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  

4.  [验证方案](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  

## <a name="BKMK_1.1"></a>设置先决条件和测试环境  
测试配置涉及设置两个林：在 Contoso 和 Adatum 之间具有双向信任的 Adatum 公司和 Contoso。 "adatum.com" 是受信任的林，而 "contoso.com" 是信任林。  

声明转换方案演示如何将受信任林中的声明转换为信任林中的声明。 为此，需要设置一个名为 adatum.com 的新林，并使用公司值为 "Adatum" 的测试用户填充该林。 然后，必须设置 contoso.com 和 adatum.com 之间的双向信任。  

> [!IMPORTANT]  
> 设置 Contoso 和 Adatum 林时，必须确保两个根域都处于 Windows Server 2012 域功能级别，这样声明转换才能工作。  

需要为实验室设置以下各项。 @No__t-0Appendix B 中详细说明了这些过程：设置测试环境](Appendix-B--Setting-Up-the-Test-Environment.md)  

需要执行以下过程来为此方案设置实验室：  

1.  [将 Adatum 设置为受信任的林到 Contoso](Appendix-B--Setting-Up-the-Test-Environment.md)  

2.  [在 Contoso 上创建 "公司" 声明类型](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  

3.  [启用 Contoso 上的 "公司" 资源属性](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  

4.  [创建中心访问规则](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  

5.  [创建中心访问策略](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  

6.  [通过组策略发布新策略](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  

7.  [在文件服务器上创建收益文件夹](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  

8.  [对新文件夹设置分类并应用中心访问策略](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  

使用以下信息来完成此方案：  

|Objects|详细信息|  
|-----------|-----------|  
|用户|张颖，Contoso|  
|用于 Adatum 和 Contoso 的用户声明|ID： ad://ext/Company:ContosoAdatum、<br /><br />源属性：公司<br /><br />建议的值：Contoso，Adatum**非常重要：** 您必须将 Contoso 和 Adatum 上 "公司" 声明类型的 ID 设置为相同的，声明转换才能工作。|  
|Contoso 上的中心访问规则|AdatumEmployeeAccessRule|  
|Contoso 上的中心访问策略|仅限 Adatum 访问策略|  
|Adatum 和 Contoso 上的声明转换策略|DenyAllExcept 公司|  
|Contoso 上的文件文件夹|D:\EARNINGS|  

## <a name="BKMK_3"></a>在受信任林（Adatum）上设置声明转换  
在此步骤中，你将在 Adatum 中创建转换策略，拒绝除 "Company" 之外的所有声明传递给 Contoso。  

Windows PowerShell 的 Active Directory 模块提供了**DenyAllExcept**参数，该参数删除转换策略中除指定声明之外的所有内容。  

若要设置声明转换，需要创建一个声明转换策略，并将其链接到受信任的林和信任的林。  

### <a name="BKMK_2.2"></a>在 Adatum 中创建声明转换策略  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>创建转换策略 Adatum 以拒绝除 "公司" 之外的所有声明  

1. 以管理员身份登录到域控制器，adatum.com 密码<strong>pass@word1</strong>。  

2. 在 Windows PowerShell 中打开提升的命令提示符，然后键入以下命令：  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except Company"`  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"adatum.com" `  

   ```  

### <a name="BKMK_2.3"></a>在 Adatum 的信任域对象上设置声明转换链接  
在此步骤中，将对 Contoso 的 Adatum 信任域对象应用新创建的声明转换策略。  

##### <a name="to-apply-the-claims-transformation-policy"></a>应用声明转换策略  

1. 以管理员身份登录到域控制器，adatum.com 密码<strong>pass@word1</strong>。  

2. 在 Windows PowerShell 中打开提升的命令提示符，然后键入以下命令：  

   ```  

     Set-ADClaimTransformLink `  
   -Identity:"contoso.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   '"TrustRole:Trusted `  

   ```  

## <a name="BKMK_4"></a>在信任林中设置声明转换（Contoso）  
在此步骤中，将在 Contoso （信任林）中创建声明转换策略，拒绝除 "公司" 之外的所有声明。 需要创建声明转换策略，并将其链接到林信任。  

### <a name="BKMK_4.1"></a>在 Contoso 中创建声明转换策略  

##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>创建转换策略 Adatum 以拒绝除 "公司" 之外的所有  

1. 以管理员身份登录到域控制器，contoso.com 密码<strong>pass@word1</strong>。  

2. 在 Windows PowerShell 中打开提升的命令提示符，然后键入以下命令：  

   ```  
   New-ADClaimTransformPolicy `  
   -Description:"Claims transformation policy to deny all claims except company" `  
   -Name:"DenyAllClaimsExceptCompanyPolicy" `  
   -DenyAllExcept:company `  
   -Server:"contoso.com" `  

   ```  

### <a name="BKMK_4.2"></a>在 Contoso 的信任域对象上设置声明转换链接  
在此步骤中，将对 Adatum 应用 contoso.com 信任域对象上新创建的声明转换策略，以允许 "公司" 传递到 contoso.com。 信任域对象命名为 adatum.com。  

##### <a name="to-set-the-claims-transformation-policy"></a>设置声明转换策略  

1. 以管理员身份登录到域控制器，contoso.com 密码<strong>pass@word1</strong>。  

2. 在 Windows PowerShell 中打开提升的命令提示符，然后键入以下命令：  

   ```  

     Set-ADClaimTransformLink   
   -Identity:"adatum.com" `  
   -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
   -TrustRole:Trusting `  

   ```  

## <a name="BKMK_5"></a>验证方案  
在此步骤中，尝试访问在文件服务器 FILE1 上设置的 D:\EARNINGS 文件夹，以验证用户是否有权访问共享文件夹。  

#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>确保 Adatum 用户可以访问共享文件夹  

1. 登录到客户端计算机，CLIENT1 为 Jeff Low， <strong>@no__t 为-1</strong>。  

2. 浏览到文件夹 \\ \ com\Earnings。  

3. Jeff Low 应能访问该文件夹。  

## <a name="additional-scenarios-for-claims-transformation-policies"></a>声明转换策略的其他方案  
下面是声明转换中的其他常见情况的列表。  


|                                                 应用场景                                                 |                                                                                                                                                                                                                                           策略                                                                                                                                                                                                                                            |
|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                  允许来自 Adatum 的所有声明进入 Contoso Adatum                  |                                                          编写 <br />ADClaimTransformPolicy \`<br /> -Description： "允许所有声明的声明转换策略" \`<br />-Name： "AllowAllClaimsPolicy" \`<br />-AllowAll \`<br />-Server:"contoso" \`<br />ADClaimTransformLink \`<br />-Identity:"adatum" \`<br />-Policy： "AllowAllClaimsPolicy" \`<br />-TrustRole：信任 \`<br />-Server:"contoso" \`                                                          |
|                  拒绝来自 Adatum 的所有声明，使其进入 Contoso Adatum                   |                                                            编写 <br />ADClaimTransformPolicy \`<br />-Description： "声明转换策略以拒绝所有声明" \`<br />-Name： "DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso" \`<br />ADClaimTransformLink \`<br />-Identity:"adatum" \`<br />-Policy： "DenyAllClaimsPolicy" \`<br />-TrustRole：信任 \`<br />-Server:"contoso" \`                                                             |
| 允许来自 Adatum 的所有声明（"公司" 和 "部门" 除外）进入 Contoso Adatum | 代码 <br />-New-ADClaimTransformationPolicy \`<br />-Description： "声明转换策略以允许除公司和部门之外的所有声明" \`<br /> -Name： "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept： company，部门 \`<br />-Server:"contoso" \`<br />ADClaimTransformLink \`<br /> -Identity:"adatum" \`<br />-Policy： "AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole：信任 \`<br />-Server:"contoso" \` |

## <a name="BKMK_Links"></a>另请参阅  

-   有关可用于声明转换的所有 Windows PowerShell cmdlet 的列表，请参阅[Active Directory PowerShell Cmdlet 参考](https://go.microsoft.com/fwlink/?LinkId=243150)。  

-   对于涉及在两个林之间导出和导入 DAC 配置信息的高级任务，请使用[动态访问控制 PowerShell 参考](https://go.microsoft.com/fwlink/?LinkId=243150)  

-   [跨林部署声明](Deploy-Claims-Across-Forests.md)  

-   [声明转换规则语言](Claims-Transformation-Rules-Language.md)  

-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  


