---
ms.assetid: 846c3680-b321-47da-8302-18472be42421
title: 跨林部署声明（示范步骤）
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 3bab7a396061ecae8a187cc6986d6d026a9e4b32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830348"
---
# <a name="deploy-claims-across-forests-demonstration-steps"></a>跨林部署声明（示范步骤）

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在本主题中，我们将介绍一个说明如何配置信任和受信任林之间的声明转换的基本方案。 您将学习如何创建和链接到信任林和受信任的林信任的声明转换策略对象。 然后，您将验证方案。  
  
## <a name="scenario-overview"></a>方案概述  
Adatum Corporation 提供到 Contoso，Ltd.的金融服务每个季度，Adatum 会计师复制其帐户电子表格到位于在 Contoso，Ltd.的文件服务器上的文件夹没有设置从 Contoso 到 Adatum 双向信任关系。 Contoso，Ltd.想要保护的共享，以便只有 Adatum 员工可以访问远程共享。  
  
在此方案中：  
  
1.  [设置的先决条件和测试环境](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_1.1)  
  
2.  [在受信任的林中 (Adatum) 上设置了声明转换](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_3)  
  
3.  [声明转换在信任林 (Contoso) 中进行设置](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_4)  
  
4.  [验证方案](Deploy-Claims-Across-Forests--Demonstration-Steps-.md#BKMK_5)  
  
## <a name="BKMK_1.1"></a>设置的先决条件和测试环境  
测试配置涉及到两个林的设置：Adatum Corporation 和 Contoso，Ltd，并让 Contoso 和 Adatum 之间的双向信任。 "adatum.com"受信任的林，"contoso.com"是信任林。  
  
声明转换方案演示转换到信任林中声明的声明中受信任的林。 若要执行此操作，需要设置称为 adatum.com 的新林并填充具有公司值为 Adatum 的测试用户的林。 然后需要设置 adatum.com contoso.com 之间的双向信任。  
  
> [!IMPORTANT]  
> Contoso 和 Adatum 林中时，必须确保这两个根域位于 Windows Server 2012 域功能级别的声明转换来工作。  
  
需要设置以下项作为实验室。 中详细介绍了这些步骤[附录 b:设置测试环境](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
需要执行以下步骤，以这种情况下将实验室：  
  
1.  [设置为受信任林 Contoso Adatum](Appendix-B--Setting-Up-the-Test-Environment.md)  
  
2.  [在 Contoso 上创建的公司声明类型](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.8)  
  
3.  [启用 Contoso 上的公司资源属性](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.55)  
  
4.  [创建中心访问规则](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.9)  
  
5.  [创建中心访问策略](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.10)  
  
6.  [发布新策略通过组策略](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.11)  
  
7.  [文件服务器上创建 Earnings 文件夹](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.12)  
  
8.  [设置分类并将中心访问策略应用于新的文件夹](Appendix-B--Setting-Up-the-Test-Environment.md#BKMK_2.13)  
  
使用以下信息来完成此方案：  
  
|Objects|详细信息|  
|-----------|-----------|  
|用户|名为 Jeff Low Contoso|  
|在 Adatum 和 Contoso 的用户声明|ID: ad://ext/Company:ContosoAdatum,<br /><br />源属性： 公司<br /><br />建议的值：Contoso，Adatum**重要：** 在 Contoso 和 Adatum 是相同的声明转换工作必须设置上公司的 ID 声明类型。|  
|在 Contoso 的中心访问规则|AdatumEmployeeAccessRule|  
|在 Contoso 的中心访问策略|Adatum 唯一的访问策略|  
|在 Adatum 和 Contoso 的声明转换策略|DenyAllExcept Company|  
|在 Contoso 文件文件夹|D:\EARNINGS|  
  
## <a name="BKMK_3"></a>在受信任的林中 (Adatum) 上设置了声明转换  
在此步骤中你创建 Adatum 拒绝除公司的所有声明传递到 Contoso 中的转换策略。  
  
提供了 Windows PowerShell 的 Active Directory 模块**DenyAllExcept**参数，在转换策略中删除指定的声明以外的所有内容。  
  
若要设置声明转换，您需要创建声明转换策略，并将其链接到受信任及信任林之间。  
  
### <a name="BKMK_2.2"></a>在 Adatum 中创建声明转换策略  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-claims-except-company"></a>若要创建 Adatum 拒绝除公司的所有声明转换策略  
  
1.  登录到域控制器，以管理员身份使用密码的 adatum.com **pass@word1**。  
  
2.  在 Windows PowerShell 中，打开提升的命令提示符并键入以下命令：  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except Company"`  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"adatum.com" `  
  
    ```  
  
### <a name="BKMK_2.3"></a>在 Adatum 的信任域对象上设置声明转换链接  
在此步骤中，您应用的新创建的声明转换策略 Adatum 的信任域对象上为 Contoso。  
  
##### <a name="to-apply-the-claims-transformation-policy"></a>要应用的声明转换策略  
  
1.  登录到域控制器，以管理员身份使用密码的 adatum.com **pass@word1**。  
  
2.  在 Windows PowerShell 中，打开提升的命令提示符并键入以下命令：  
  
    ```  
  
      Set-ADClaimTransformLink `  
    -Identity:"contoso.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    '"TrustRole:Trusted `  
  
    ```  
  
## <a name="BKMK_4"></a>声明转换在信任林 (Contoso) 中进行设置  
在此步骤中创建 Contoso 拒绝除公司。 的所有声明 （信任林） 中的声明转换策略 您需要创建声明转换策略，并将其链接到此目录林信任。  
  
### <a name="BKMK_4.1"></a>在 Contoso 中创建声明转换策略  
  
##### <a name="to-create-a-transformation-policy-adatum-to-deny-all-except-company"></a>若要创建转换策略 Adatum 拒绝除公司  
  
1.  登录到域控制器，以管理员身份使用密码的 contoso.com **pass@word1**。  
  
2.  在 Windows PowerShell 中打开提升的命令提示符并键入以下命令：  
  
    ```  
    New-ADClaimTransformPolicy `  
    -Description:"Claims transformation policy to deny all claims except company" `  
    -Name:"DenyAllClaimsExceptCompanyPolicy" `  
    -DenyAllExcept:company `  
    -Server:"contoso.com" `  
  
    ```  
  
### <a name="BKMK_4.2"></a>Contoso 的信任域对象上设置声明转换链接  
在此步骤中，您将应用新创建 Adatum 以允许"公司"的 contoso.com 信任域对象上的声明转换策略传递到 contoso.com。 信任域对象命名为 adatum.com。  
  
##### <a name="to-set-the-claims-transformation-policy"></a>若要设置声明转换策略  
  
1.  登录到域控制器，以管理员身份使用密码的 contoso.com **pass@word1**。  
  
2.  在 Windows PowerShell 中打开提升的命令提示符并键入以下命令：  
  
    ```  
  
      Set-ADClaimTransformLink   
    -Identity:"adatum.com" `  
    -Policy:"DenyAllClaimsExceptCompanyPolicy" `  
    -TrustRole:Trusting `  
  
    ```  
  
## <a name="BKMK_5"></a>验证方案  
在此步骤中您尝试访问已设置文件服务器 FILE1 以验证该用户有权访问共享文件夹的 D:\EARNINGS 文件夹。  
  
#### <a name="to-ensure-that-the-adatum-user-can-access-the-shared-folder"></a>若要确保 Adatum 用户可以访问共享的文件夹  
  
1.  登录到客户端计算机，作为密码使用名为 Jeff Low 的 CLIENT1 **pass@word1**。  
  
2.  浏览到文件夹\\\FILE1.contoso.com\Earnings。  
  
3.  名为 Jeff Low 应能够访问的文件夹。  
  
## <a name="additional-scenarios-for-claims-transformation-policies"></a>声明转换策略的其他方案  
以下是声明转换中的其他常见用例的列表。  
  
|应用场景|策略|  
|------------|----------|  
|允许来自 Adatum 转到 Contoso Adatum 的所有声明|Code - <br />New-ADClaimTransformPolicy \`<br /> 说明:"声明转换策略，以允许所有声明" \`<br />-Name:"AllowAllClaimsPolicy" \`<br />-AllowAll \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsPolicy" \`<br />-TrustRole： 信任 \`<br />-Server:"contoso.com" `|  
|拒绝来自 Adatum 转到 Contoso Adatum 的所有声明|Code - <br />New-ADClaimTransformPolicy \`<br />说明:"声明转换策略来拒绝所有声明" \`<br />-Name:"DenyAllClaimsPolicy" \`<br /> -DenyAll \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br />-Identity:"adatum.com" \`<br />-Policy:"DenyAllClaimsPolicy" \`<br />-TrustRole： 信任 \`<br />-Server:"contoso.com"`|  
|允许来自除"公司"和"部门"若要转到 Contoso Adatum Adatum 的所有声明|代码 <br />- New-ADClaimTransformationPolicy \`<br />说明:"声明转换策略，以允许除公司和部门的所有声明" \`<br /> -Name:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br />-AllowAllExcept:company,department \`<br />-Server:"contoso.com" \`<br />Set-ADClaimTransformLink \`<br /> -Identity:"adatum.com" \`<br />-Policy:"AllowAllClaimsExceptCompanyAndDepartmentPolicy" \`<br /> -TrustRole： 信任 \`<br />-Server:"contoso.com" `|  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   有关可用于声明转换的所有 Windows PowerShell cmdlet 的列表，请参阅[Active Directory PowerShell Cmdlet 参考](https://go.microsoft.com/fwlink/?LinkId=243150)。  
  
-   对于涉及导出和导入 DAC 两个林之间的配置信息的高级任务，使用[动态访问控制 PowerShell 参考](https://go.microsoft.com/fwlink/?LinkId=243150)  
  
-   [跨林部署声明](Deploy-Claims-Across-Forests.md)  
  
-   [声明转换规则语言](Claims-Transformation-Rules-Language.md)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  

