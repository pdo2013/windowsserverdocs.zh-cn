---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: Deploy Automatic File Classification (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 77fb8cc6e13cb82e4d07808c3ae77757a4b2de79
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826778"
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>Deploy Automatic File Classification (Demonstration Steps)

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍如何在 Active Directory 中启用资源属性，在文件服务器上创建分类规则，然后将值分配给文件服务器上文件的资源属性。 在此示例中，将创建以下分类规则：  
  
-   适用于字符串 Contoso 机密。 的文件组中搜索内容的分类规则 如果在某个文件中找到该字符串，则在该文件上将“影响”资源属性设置为“高”。  
  
-   可在一组文件中搜索正则表达式的内容分类规则，该正则表达式在文件中至少匹配 10 次身份证号。 如果找到该模式，该文件归类为具有个人身份信息和个人身份信息资源属性设置为高。  
  
**本文档中**  
  
-   [步骤 1：创建资源属性定义](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [步骤 2：创建字符串内容的分类规则](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [步骤 3：创建正则表达式内容分类规则](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [步骤 4：验证已分类文件](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_Step1"></a>步骤 1:创建资源属性定义  
启用“影响”和“个人身份信息”资源属性，以便文件分类基础结构可以使用这些资源属性来标记已在网络共享文件夹上扫描的文件。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>创建资源属性定义  
  
1.  在域控制器上，以 Domain Admins 安全组成员的身份登录到服务器。  
  
2.  打开 Active Directory 管理中心。 在服务器管理器中，单击“工具”，然后单击“Active Directory 管理中心”。  
  
3.  展开“动态访问控制”，然后单击“资源属性”。  
  
4.  右键单击“影响”，然后单击“启用”。  
  
5.  右键单击“个人身份信息”，然后单击“启用”。  
  
![解决方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)****Windows PowerShell 等效命令****  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>步骤 2:创建字符串内容的分类规则  
字符串内容的分类规则将扫描某个文件以查找特定字符串。 如果找到该字符串，则可以配置资源属性的值。 在此示例中，我们将扫描网络共享文件夹上的每个文件并查找字符串 Contoso 机密。 如果找到该字符串，则将关联的文件都归类为具有高业务影响。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>创建字符串内容的分类规则  
  
1.  以管理员安全组成员的身份登录到文件服务器。  
  
2.  在 Windows Powershell 命令提示符下，键入 **Update-FsrmClassificationPropertyDefinition**，然后按 ENTER。 这会将域控制器上创建的属性定义同步到文件服务器。  
  
3.  打开文件服务器资源管理器。 在服务器管理器中，单击“工具”，然后单击“文件服务器资源管理器”。  
  
4.  展开“分类管理” ，右键单击“分类规则” ，然后单击“配置分类计划” 。  
  
5.  依次选中“启用固定计划”复选框和“允许对新文件进行连续分类”复选框，再选择一周中的某一天来运行分类，然后单击“确定”。  
  
6.  右键单击“分类规则” ，然后单击“创建分类规则” 。  
  
7.  在“规则名称”框中的“常规”选项卡上，键入规则名称，如 **Contoso Confidential**。  
  
8.  在“作用域”  选项卡上，单击“添加” ，然后选择应该包括在该规则中的文件夹，如 D:\Finance Documents。  
  
    > [!NOTE]  
    > 还可以选择用于作用域的动态命名空间。 有关用于分类规则的动态命名空间的详细信息，请参阅[What's New 中文件服务器资源管理器在 Windows Server 2012\[重定向\]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083)。  
  
9. 在“分类”选项卡上，进行以下配置：  
  
    -   在“选择用于将属性分配给文件的方法”框中，请确保“内容分类器”处于选中状态。  
  
    -   在“选择要分配给文件的属性”  框中，单击“影响” 。  
  
    -   在“指定一个值”框中，单击“高”。  
  
10. 在“参数”标题下，单击“配置”。  
  
11. 在“表达式类型”列中，选择“字符串”。  
  
12. 在“表达式”  列中，键入 **Contoso Confidential**，然后单击“确定” 。  
  
13. 在“评估类型”选项卡上，选中“重新评估现有属性值”复选框，再单击“覆盖现有值”，然后单击“确定”。  
  
![解决方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)****Windows PowerShell 等效命令****  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>步骤 3:创建正则表达式内容的分类规则  
正则表达式分类规则将扫描某个文件以查找与正则表达式匹配的模式。 若找到与该正则表达式匹配的字符串，则可以配置资源属性的值。 在此示例中，我们将扫描网络共享文件夹上的每个文件，并查找与身份证号的模式 (XXX XX XXXX) 匹配的字符串。 如果找到该模式，则将关联的文件归类为具有个人身份信息。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>创建正则表达式内容分类规则  
  
1.  以管理员安全组成员的身份登录到文件服务器。  
  
2.  在 Windows Powershell 命令提示符下，键入 **Update-FsrmClassificationPropertyDefinition**，然后按 ENTER。 这会将域控制器上创建的属性定义同步到文件服务器。  
  
3.  打开文件服务器资源管理器。 在服务器管理器中，单击“工具”，然后单击“文件服务器资源管理器”。  
  
4.  右键单击“分类规则” ，然后单击“创建分类规则” 。  
  
5.  在“规则名称”框中的“常规”选项卡上，键入分类规则的名称，如 PII 规则。  
  
6.  在“作用域”选项卡上，单击“添加”，然后选择应该包括在该规则中的文件夹，如 D:\Finance Documents。  
  
7.  在“分类”选项卡上，进行以下配置：  
  
    -   在“选择用于将属性分配给文件的方法”框中，请确保“内容分类器”处于选中状态。  
  
    -   在“选择要分配给文件的属性”  框中，单击“个人身份信息” 。  
  
    -   在“指定一个值”框中，单击“高”。  
  
8.  在“参数”标题下，单击“配置”。  
  
9. 在“表达式类型”  列中，选择“正则表达式” 。  
  
10. 在中**表达式**列中，键入 **^ （？ ！000) ([0-7] \d{2}| 7([0-7]\d|7[012 ([-]？)(?!00) \d\d\3 （？ ！0000) \d{4}$**  
  
11. 在“最少出现次数”列中，键入 **10**，然后单击“确定”。  
  
12. 在“评估类型”选项卡上，选中“重新评估现有属性值”复选框，再单击“覆盖现有值”，然后单击“确定”。  
  
![解决方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)****Windows PowerShell 等效命令****  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>步骤 4:验证文件是否已正确分类  
通过查看分类规则中指定的文件夹中已创建的文件的属性，可以验证文件是否已正确分类。  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>验证文件是否已正确分类  
  
1.  在文件服务器上，通过使用文件服务器资源管理器来运行分类规则。  
  
    1.  单击“分类管理”，右键单击“分类规则”，然后单击“立即使用所有规则运行分类”。  
  
    2.  单击“等待分类完成”选项，然后单击“确定”。  
  
    3.  关闭“自动分类”报告。  
  
    4.  通过使用以下命令借助 Windows PowerShell 可执行此操作：**Start-FSRMClassification '"RunDuration 0 -Confirm:$false**  
  
2.  导航至分类规则中已指定的文件夹，如 D:\Finance Documents。  
  
3.  右键单击该文件夹中的文件，然后单击“属性”。  
  
4.  单击“分类”  选项卡，并验证文件是否已正确分类。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [方案：深入了解你的数据使用分类](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [自动文件分类规划](https://docs.microsoft.com/previous-versions/orphan-topics/ws.11/jj574209(v%3dws.11))  

  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  

