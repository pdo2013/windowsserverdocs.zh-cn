---
ms.assetid: 01988844-df02-4952-8535-c87aefd8a38a
title: "部署自动文件分类（演示步骤）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1c5c0fa221e0d7375216426f838ba37bee852984
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-automatic-file-classification-demonstration-steps"></a>部署自动文件分类（演示步骤）

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍如何启用资源属性 Active Directory 中的、文件服务器，在创建分类规则，然后向值分配文件服务器上的文件的资源属性。 例如，创建分类以下规则：  
  
-   内容分类规则搜索一组文件字符串 ' Contoso 机密。 如果文件中找到字符串，则影响的资源属性设置为在文件高。  
  
-   搜索一套常规 expression 相匹配身份证号至少 10 倍一文件中的文件内容分类规则。 如果找到模式，该文件已将其划分为时遇到的个人身份的信息和的个人身份信息资源属性设置为高。  
  
**本文档**  
  
-   [第 1 步：创建资源属性定义](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [第 2 步：创建字符串内容分类规则](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step2)  
  
-   [第 3 步：创建常规 expression 内容分类规则](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [第 4 步：验证，归文件](Deploy-Automatic-File-Classification--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> 本主题包含示例 Windows PowerShell cmdlet，你可以使用自动所述的过程的一部分。 有关详细信息，请参阅[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_Step1"></a>第 1 步：创建资源属性定义  
启用影响和个人身份信息的资源属性，以便文件分类基础结构可以使用这些资源属性标记在网络共享文件夹扫描的文件。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>若要创建资源属性定义  
  
1.  在上域控制器，登录到服务器的域管理员安全组成员。  
  
2.  打开 Active Directory 管理中心。 在服务器管理器中，单击**工具**，然后单击**Active Directory 管理中心**。  
  
3.  展开**动态访问控制**，然后单击**资源属性**。  
  
4.  右键单击**影响**，然后单击**启用**。  
  
5.  右键单击**个人身份信息**，然后单击**启用**。  
  
![解决方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'   
Set-ADResourceProperty '"Enabled:$true '"Identity:'CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>第 2 步：创建字符串内容分类规则  
字符串内容分类规则扫描特定字符串文件。 如果找到字符串，则可以配置资源属性的价值。 在此示例中，我们将扫描在网络文件夹上共享的每个文件，并查找字符串 ' Contoso 机密。 如果找到字符串，并属于关联的文件时遇到高与众不同之处。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-create-a-string-content-classification-rule"></a>若要创建字符串内容分类规则  
  
1.  组成员的管理员安全登录到文件服务器上。  
  
2.  Windows PowerShell 命令提示符下，键入**更新 FsrmClassificationPropertyDefinition**，然后按 ENTER。 这将同步到文件服务器的域控制器上创建的属性定义。  
  
3.  打开文件服务器资源管理器。 在服务器管理器中，单击**工具**，然后单击**文件服务器资源管理器**。  
  
4.  展开**分类管理**，右键单击**分类规则**，然后单击**配置分类计划**。  
  
5.  选择**启用固定的日程安排**复选框、选择**允许为新文件连续分类**复选框、选择要运行分类，然后单击某一天**确定**。  
  
6.  右键单击**分类规则**，然后单击**创建分类规则**。  
  
7.  在**常规**选项卡上，在**规则名称**框中，键入规则名称，如**Contoso 机密**。  
  
8.  在**范围**选项卡上，单击**添加**，然后选择应该包含该规则，如 D:\Finance 文档中的文件夹。  
  
    > [!NOTE]  
    > 你还可以选择范围动态命名空间。 有关分类规则动态名称空间的详细信息，请参阅[什么是 Windows Server 2012 \[redirected\ 中的新文件服务器资源管理器中]](assetId:///d53c603e-6217-4b98-8508-e8e492d16083)。  
  
9. 在**分类**选项卡上，将配置以下：  
  
    -   在**选择一种方法以分配到文件的属性**框中，请确保**内容分类**选择。  
  
    -   在**选择要给其指定为的文件的属性**框中，单击**影响**。  
  
    -   在**指定值**框中，单击**高**。  
  
10. 下**参数**标题，单击**配置**。  
  
11. 在**Expression 类型**列中，选择**字符串**。  
  
12. 在**Expression**列中，键入**Contoso 机密**，然后单击**确定**。  
  
13. 在**评估类型**选项卡上，选择**重新评估现有属性值**复选框、单击**覆盖现有值**，然后单击**确定**。  
  
![解决方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;$AutomaticClassificationScheduledTask  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name 'Contoso Confidential' -Property "Impact_MS" -PropertyValue "3000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step3"></a>第 3 步：创建常规 expression 内容分类规则  
常规 expression 分类规则扫描匹配常规 expression 模式的文件。 如果找到匹配常规 expression 字符串，则可以配置资源属性的价值。 在此示例中，我们将扫描在网络文件夹上共享的每个文件，然后查找匹配身份证号 (XXX XX XXXX) 模式的字符串。 如果找到模式，并属于关联的文件时遇到的个人身份的信息。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-regular-expression-content-classification-rule"></a>若要创建常规 expression 内容分类规则  
  
1.  组成员的管理员安全登录到文件服务器。  
  
2.  Windows PowerShell 命令提示符下，键入**更新 FsrmClassificationPropertyDefinition**，然后按 ENTER。 这将同步到文件服务器的域控制器创建的属性定义。  
  
3.  打开文件服务器资源管理器。 在服务器管理器中，单击**工具**，然后单击**文件服务器资源管理器**。  
  
4.  右键单击**分类规则**，然后单击**创建分类规则**。  
  
5.  在**常规**选项卡上，在**规则名称**框中，键入分类规则，如 PII 规则的名称。  
  
6.  在**范围**选项卡上，单击**添加**，然后选择应该包含该规则，如 D:\Finance 文档中的文件夹。  
  
7.  在**分类**选项卡上，将配置以下：  
  
    -   在**选择一种方法以分配到文件的属性**框中，请确保**内容分类**选择。  
  
    -   在**选择要给其指定为的文件的属性**框中，单击**个人身份信息**。  
  
    -   在**指定值**框中，单击**高**。  
  
8.  下**参数**标题，单击**配置**。  
  
9. 在**Expression 类型**列中，选择**常规 expression**。  
  
10. 在**Expression**列中，键入**^（？！000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]？)(?!00) \d\d\3（？！0000) \d {4} $**  
  
11. 在**至少发生**列中，键入**10**，然后单击**确定**。  
  
12. 在**评估类型**选项卡上，选择**重新评估现有属性值**复选框、单击**覆盖现有值**，然后单击**确定**。  
  
![解决方案指南](media/Deploy-Automatic-File-Classification--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
New-FSRMClassificationRule -Name "PII Rule" -Property "PII_MS" -PropertyValue "5000" -Namespace @('D:\Finance Documents') -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=10;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
## <a name="BKMK_Step4"></a>第 4 步：验证，正确归文件  
你可以采用验证文件正确分为通过查看已创建分类规则中指定的文件夹中文件的属性。  
  
#### <a name="to-verify-that-the-files-are-classified-correctly"></a>若要验证，正确归文件  
  
1.  文件服务器，在使用文件服务器资源管理器中运行分类规则。  
  
    1.  单击**分类管理**，右键单击**分类规则**，然后单击**运行分类的所有立即规则**。  
  
    2.  单击**等待分类以完成**选项，然后依次单击**确定**。  
  
    3.  关闭自动分类报告。  
  
    4.  你可以通过使用 Windows PowerShell 使用以下命令：**开始 FSRMClassification '"RunDuration 0-确认：$false**  
  
2.  导航到分类规则，如 D:\Finance 文档中指定的文件夹。  
  
3.  右键单击该文件夹中的文件，然后单击**属性**。  
  
4.  单击**分类**选项卡，然后确认该文件被归正确。  
  
## <a name="BKMK_Links"></a>请参阅  
  
-   [通过使用分类的方案：获得深入地了解你的数据](Scenario--Get-Insight-into-Your-Data-by-Using-Classification.md)  
  
-   [自动文件分类的套餐](assetId:///e3c3bb4b-3034-42b7-b391-8ef5f5851955)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  

