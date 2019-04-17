---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: "森林（演示步骤）跨部署索赔"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>森林（演示步骤）跨部署索赔

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在本主题中，我们将在本文说明如何进行配置信任受信任的林之间的索赔转换的基本方案。 您将学会如何创建和链接到信任林和受信任的森林信任索赔转换策略对象。 然后，你将验证项 scenario。  
  
## <a name="scenario-overview"></a>方案概述  
Adatum Corporation 到 Contoso，Ltd.提供金融服务每个季度 Adatum 会计到文件服务器内 Contoso，Ltd.上的文件夹复制其帐户电子表格的地方没有双向信任到 Adatum 从 Contoso 设置。 Ltd.Contoso，要保护共享，以便仅 Adatum 员工可以访问该远程共享。  
  
在此情况下：  
  
1.  [先决条件并测试环境设置](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [在受信任的森林 (Adatum) 上设置了索赔转换](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [设置在信任的森林 (Contoso) 的索赔转换](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [验证项 scenario](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>先决条件并测试环境设置  
测试配置涉及林两个设置：Adatum Corporation 和 Contoso、Ltd，有双向信任 Contoso 和 Adatum 之间。 "adatum.com"受信任的森林并且"contoso.com"信任森林。  
  
索赔转换方案演示转换到信任森林中的索赔受信任的树林中索赔。 若要执行此操作，你需要设置新的林称为 adatum.com 填充提供测试用户公司值为 Adatum 森林。 然后，你需要设置双向信任 contoso.com 和 adatum.com 之间。  
  
> [!IMPORTANT]  
> 当设置 Contoso 和 Adatum 森林，你必须确保这两个根域在 Windows Server 2012 域功能级别索赔转换工作。  
  
你需要设置下面的实验。 以下过程介绍中的详述[附录 b：设置了测试环境](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
你需要实现以下设置这种情况下此实验的过程：  
  
1.  [设置为受信任的森林到 Contoso Adatum](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [在 Contoso 上创建的公司索赔类型](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [启用 Contoso 上的公司资源属性](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [创建中央使用规则](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [创建中心访问策略](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [在发布新的策略通过组策略](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [文件服务器上创建收益文件夹](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [设置分类和新文件夹应用的中央访问策略](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
使用以下信息才能完成此项 scenario:  
  
|对象|详细信息|  
|-----------|-----------|  
|用户|Jeff Low、Contoso|  
|Adatum 和 Contoso 用户声明|ID: 广告: / / 文本/公司：ContosoAdatum，<br /><br />源属性：公司<br /><br />建议的值：Contoso、Adatum**重要提示：** Contoso 和上 Adatum 索赔转换相同工作，必须在索赔键入设置公司上的 ID。|  
|上 Contoso 中心访问规则|AdatumEmployeeAccessRule|  
|上 Contoso 中心访问策略|Adatum 仅访问策略|  
|Adatum 和 Contoso 索赔转换策略|DenyAllExcept 公司|  
|Contoso 上的文件文件夹|D:\EARNINGS|  
  
## <a name="BKMK_3"></a>在受信任的森林 (Adatum) 上设置了索赔转换  
在这一步中，您将创建 Adatum 来拒绝除公司的所有声明将传递到 Contoso 转换策略。  
  
提供了 Active Directory 的 Windows PowerShell 模块**DenyAllExcept**参数，下降指定的索赔除转换策略中。  
  
若要设置索赔转换时，你需要创建索赔转换策略，并将其链接受信任并信任林之间。  
  
### <a name="BKMK_2.2"></a>在 Adatum 中创建索赔转换策略  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>若要创建转换策略 Adatum 来拒绝公司除的所有声明  
  
1.  了域控制器，adatum.com 以管理员身份使用密码登录**pass@word1**。  
  
2.  在 Windows PowerShell、打开提升了权限的命令提示符下，键入以下命令：  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>设置 Adatum 的信任域对象索赔转换链接  
在此步骤，你申请新创建的索赔转换策略 Adatum 的信任域对象上 Contoso。  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>若要将索赔转换策略应用  
  
1.  了域控制器，adatum.com 以管理员身份使用密码登录**pass@word1**。  
  
2.  在 Windows PowerShell、打开提升了权限的命令提示符下，键入以下命令：  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>设置在信任的森林 (Contoso) 的索赔转换  
在此步骤创建索赔转换策略中 Contoso（信任森林）来拒绝所有索赔除公司。 您需要创建索赔转换策略，并将其链接到森林信任。  
  
### <a name="BKMK_4.1"></a>创建 Contoso 索赔转换策略  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>若要创建转换策略 Adatum 来拒绝除外公司  
  
1.  了域控制器，contoso.com 以管理员身份使用密码登录**pass@word1**。  
  
2.  在 Windows PowerShell 中打开提升了权限的命令提示符下，键入以下命令：  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>设置 Contoso 信任域对象索赔转换链接  
在此步骤中，您将应用新建到 contoso.com 通过索赔转换策略 Adatum 允许"公司"contoso.com 信任域对象上的。信任域对象 adatum.com 命名。  
  
##### <a name="to-set-the-claims-transformation-policy"></a>若要设置索赔转换策略  
  
1.  了域控制器，contoso.com 以管理员身份使用密码登录**pass@word1**。  
  
2.  在 Windows PowerShell 中打开提升了权限的命令提示符下，键入以下命令：  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>验证项 scenario  
在此步骤尝试访问已设置文件服务器文件 1 验证则用户可以访问权限共享文件夹 D:\EARNINGS 文件夹。  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>若要确保 Adatum 用户都可以访问共享的文件夹  
  
1.  客户端计算机，客户端 1 作为 Jeff 低使用密码登录**pass@word1**。  
  
2.  浏览到文件夹 \\\FILE1.contoso.com\Earnings。  
  
3.  Jeff 低应该能够访问该文件夹。  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>其他索赔转换策略的场景  
以下是其他的常见情况索赔转换中的列表。  
  
|方案|策略|  
|------------|----------|  
|允许来自 Adatum 转到 Contoso Adatum 的所有声明|代码- <br />New-ADClaimTransformPolicy \<br /> 描述:"索赔转换策略允许所有索赔"\<br />-名称:"AllowAllClaimsPolicy"\<br />-AllowAll \<br />服务器:"contoso.com"\<br />Set-ADClaimTransformLink \<br />-的身份:"adatum.com"\<br />-策略:"AllowAllClaimsPolicy"\<br />-TrustRole：信任 \<br />服务器:"contoso.com"'|  
|拒绝来自 Adatum 转到 Contoso Adatum 的所有声明|代码- <br />New-ADClaimTransformPolicy \<br />描述:"索赔转换策略来拒绝所有索赔"\<br />-名称:"DenyAllClaimsPolicy"\<br /> -DenyAll \<br />服务器:"contoso.com"\<br />Set-ADClaimTransformLink \<br />-的身份:"adatum.com"\<br />-策略:"DenyAllClaimsPolicy"\<br />-TrustRole：信任 \<br />服务器:"contoso.com"'|  
|允许来自"公司"和"商业部"，以转到 Contoso Adatum 除外 Adatum 的所有声明|代码 <br />-New-ADClaimTransformationPolicy \<br />描述:"索赔转换策略允许所有索赔除公司和部门"\<br /> -名称:"AllowAllClaimsExceptCompanyAndDepartmentPolicy"\<br />-AllowAllExcept：公司、部门 \<br />服务器:"contoso.com"\<br />Set-ADClaimTransformLink \<br /> -的身份:"adatum.com"\<br />-策略:"AllowAllClaimsExceptCompanyAndDepartmentPolicy"\<br /> -TrustRole：信任 \<br />服务器:"contoso.com"'|  
  
## <a name="BKMK_Links"></a>请参阅  
  
-   适用于索赔转换的所有 Windows PowerShell cmdlet 的列表，请参阅[Active Directory PowerShell Cmdlet 参考](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
-   对于涉及导出和导入的两个林之间 DAC 配置信息的高级任务，使用[动态访问控制 PowerShell 参考](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [森林跨部署索赔](Deploy-Claims-Across-Forests.md)  
  
-   [索赔转换规则语言](Claims-Transformation-Rules-Language.md)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  

