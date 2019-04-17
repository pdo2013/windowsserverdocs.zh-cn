---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: "部署加密的 Office 文件（演示步骤）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 529000c60a80ee33fc2aa7d09370d8ac1e06311c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>部署加密的 Office 文件（演示步骤）

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

Contoso 财经部门具有大量存储他们文档中的文件服务器。 这些文档可常规的文档或它们可能会产生高业务影响 (HBI)。 例如，包含机密信息的任何文档被视为，contoso 具有高企业影响。 Contoso 想要确保它们的文档具有最少的保护，并且他们 HBI 文档限于适当的用户。 若要完成此操作，Contoso 探索使用文件分类的基础结构 (FCI) 和适用于 Windows Server 2012 的广告 RMS。 通过 FCI Contoso 将所有他们文件服务器，根据的内容上的文档进行分类，然后使用广告 RMS 应用的相应权限策略。  
  
在此情况下，你将执行以下步骤：  
  
|任务|描述|  
|--------|---------------|  
|[启用资源属性](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|启用**影响**和**个人身份信息**资源属性。|  
|[创建分类规则](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|创建分类以下规则：**HBI 分类规则**和**PII 分类规则**。|  
|[文件管理任务用于自动保护文档与广告 RMS](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|创建文件的管理任务自动用于广告 RMS 保护文档与高的个人身份信息 (PII)。 只有 FinanceAdmin 组成员都将有权访问包含高 PII 的文档。|  
|[查看结果](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|检查文档分类，观察如何更改文档中的内容时的更改。 同时验证文档广告 rms 获取受保护。|  
|[验证广告 RMS 保护](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|验证广告 RMS 设有文档。|  
|||  
  
## <a name="BKMK_1.1"></a>第 1 步：启用资源属性  
  
#### <a name="to-enable-resource-properties"></a>若要启用资源属性  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_DC1。 通过使用 Contoso\Administrator 使用密码登录到 server **pass@word1**。  
  
2.  打开 Active Directory 管理中心，然后单击**树视图**。  
  
3.  展开**动态访问控制**，然后选择**资源属性**。  
  
4.  向下滚动到**影响**中的属性**显示名称**列。 右键单击**影响**，然后单击**启用**。  
  
5.  向下滚动到**个人身份信息**中的属性**显示名称**列。 右键单击**个人身份信息**，然后单击**启用**。  
  
6.  将发布在资源属性**全球资源列表**，在左侧窗格中，单击**资源属性列出**，然后双击**全球属性资源列表**。  
  
7.  单击**添加**，然后向下滚动到和单击**影响**以将其添加到列表。 执行相同的操作**个人身份信息**。 单击**确定**两次才能完成。  
  
![解决方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>第 2 步：创建分类规则  
这一步中介绍了如何创建**高影响**分类规则。 此规则将搜索内容的文档，然后找到"Contoso 机密"字符串，如果它将分类具有高业务影响本文档。 此分类将覆盖低业务影响任何以前分配的分类。  
  
你还将创建**高 PII**规则。 此规则搜索内容的文档，并找到身份证号，如果进行分类为具有高 PII 文档。  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>若要创建高影响分类规则  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_FILE1。 通过使用 Contoso\Administrator 使用密码登录到 server **pass@word1**。  
  
2.  你需要恢复 Active Directory 从全球的资源属性。 打开 Windows PowerShell 和类型：`Update-FSRMClassificationPropertyDefinition`，然后按 ENTER。 关闭 Windows PowerShell。  
  
3.  打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击**开始**，类型**文件服务器资源管理器**，然后单击**文件服务器资源管理器**。  
  
4.  在左侧窗格中的文件服务器资源管理器中，展开**分类管理**，然后选择**分类规则**。  
  
5.  在**操作**窗格中，单击**配置分类计划**。 上**自动分类**选项卡上，选择**启用固定的日程安排**，选择**星期几**，然后选择**允许连续分类为新文件**复选框。 单击**确定**。  
  
6.  在**操作**窗格中，单击**创建分类规则**。 这将打开**创建分类规则**对话框。  
  
7.  在**规则名称**框中，键入**高度商业影响**。  
  
8.  在**描述**框中，键入**确定是否基于是否存在"Contoso 机密"字符串很高的业务影响文档 **  
  
9. 上**范围**选项卡上，单击**设置文件夹管理属性**，选择**文件夹的使用**，单击**添加**，然后单击**浏览**、浏览到作为路径 D:\Finance 文档、单击**确定**，然后选择一个名为属性值**组文件**，然后单击**关闭**。 一旦设置管理属性，在**规则范围**选项卡上选择**组文件**。  
  
10. 单击**分类**选项卡。  下**选择一种方法以分配到文件的属性**、选择**内容分类**从下拉列表。  
  
11. 下**选择要给其指定为的文件的属性**、选择**影响**从下拉列表。  
  
12. 下**指定值**、选择**高**从下拉列表。  
  
13. 单击**配置**下**参数**。  在**分类参数**对话框中，在**Expression 类型**列表中，选择**字符串**。 在**Expression**框中，键入：**Contoso 机密**，然后单击**确定**。  
  
14. 单击**评估类型**选项卡。  单击**重新评估现有属性值**，单击**覆盖**现有值，然后单击**确定**才能完成。  
  
![解决方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>若要创建高 PII 分类规则  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_FILE1。 通过使用 Contoso\Administrator 使用密码登录到 server **pass@word1**。  
  
2.  在桌面上，打开名为文件夹**常规表情**，然后打开名为文本文档**RegEx SSN**。 突出显示并复制以下常规 expression 字符串：**^（？！000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]？)(?!00) \d\d\3（？！0000) \d {4}$**。 将后面在使用此字符串此步骤以便使其保持打开入剪贴板。  
  
3.  打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击**开始**，类型**文件服务器资源管理器**，然后单击**文件服务器资源管理器**。  
  
4.  在左侧窗格中的文件服务器资源管理器中，展开**分类管理**，然后选择**分类规则**。  
  
5.  在**操作**窗格中，单击**配置分类计划**。 上**自动分类**选项卡上，选择**启用固定的日程安排**，选择**星期几**，然后选择**允许连续分类为新文件**复选框。 单击确定。  
  
6.  在**规则名称**框中，键入**高 PII**。 在**描述**框中，键入**如果文档具有较高，确定 PII 基于身份证号显示。**  
  
7.  单击**范围**选项卡上，选择**组文件**复选框。  
  
8.  单击**分类**选项卡。  下**选择一种方法以分配到文件的属性**、选择**内容分类**从下拉列表。  
  
9. 下**选择要给其指定为的文件的属性**、选择**个人身份信息**从下拉列表。  
  
10. 下**指定值**、选择**高**从下拉列表。  
  
11. 单击**配置**下**参数**。   
    在**分类参数**窗口，请在**Expression 类型**列表中，选择**常规 Expression**。 在**Expression**框中，从剪贴板粘贴文本你：**^（？！000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]？)(?!00) \d\d\3（？！0000) \d {4}$**，然后单击**确定**。  
  
    > [!NOTE]  
    > 此 expression 将允许无效身份证号。 这使我们可以使用虚构身份证号，在演示。  
  
12. 单击**评估类型**选项卡。  选择**重新评估现有属性值**，**覆盖**现有值，然后单击**确定**才能完成。  
  
![解决方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
现在，你应该拥有两个分类规则：  
  
-   高与众不同之处  
  
-   高 PII  
  
## <a name="BKMK_3"></a>第 3 步：使用文件管理任务自动保护文档与广告 RMS  
现在，你已创建规则自动分类基于内容的文档下, 一步是创建的文件管理任务广告 RMS 用于自动保护根据其分类某些文档。 在此步骤中，您将创建自动任何文档与高 PII 进行保护的文件管理任务。 只有 FinanceAdmin 组成员都将有权访问包含高 PII 的文档。  
  
#### <a name="to-protect-documents-with-ad-rms"></a>为了保护文档与广告 RMS  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_FILE1。 通过使用 Contoso\Administrator 使用密码登录到 server **pass@word1**。  
  
2.  打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击**开始**，类型**文件服务器资源管理器**，然后单击**文件服务器资源管理器**。  
  
3.  在左侧窗格中，选择**文件管理任务**。 在**操作**窗格中选择**创建的文件管理任务**。  
  
4.  在**任务名称：**字段中，键入**高 PII**。 在**描述**字段中，键入**高 PII 文档自动 RMS 保护**。  
  
5.  单击**范围**选项卡上，选择**组文件**复选框。  
  
6.  单击**操作**选项卡。 下**类型**、选择**RMS 加密**。 单击**浏览**来选择模板，然后选择**仅上 Contoso 财经管理员**模板。  
  
7.  单击**条件**选项卡，然后单击**添加**。 下**属性**、选择**个人身份信息**。 下**运营商**、选择**相等**。 下**值**、选择**高**。 单击**确定**。  
  
8.  单击**计划**选项卡。 在**计划**部分中，单击**每周**，然后选择**星期日**。 运行一次一周的任务将确保可以捕获任何可能因服务中断或其他中断事件错过的文档。  
  
9. 在**连续操作**部分中，选择**运行任务连续打开新文件**，然后单击**确定**。 现在，你应该拥有一个名为高 PII 的文件管理任务。  
  
![解决方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>第 4 步：查看结果  
它是时间，来看看你的新自动分类和广告 RMS 保护规则中操作。 在这一步中，您将检查分类的文档，并观察如何更改文档中的内容时的更改。  
  
#### <a name="to-view-the-results"></a>若要查看结果  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_FILE1。 通过使用 Contoso\Administrator 使用密码登录到 server **pass@word1**。  
  
2.  在 Windows 资源管理器，导航到 D:\Finance 文档。  
  
3.  右键单击财经备注文档，然后单击**属性**。单击**分类**选项卡，然后通知的影响属性当前具有没有值。 单击**取消**。  
  
4.  右键单击**到雇佣文档批准请求**，然后选择**属性**。  
  
5.  单击**分类**选项卡，然后将注意到，**个人身份信息属性**当前没有任何值。 单击**取消**。  
  
6.  切换到客户端 1。 关闭任何已登录的用户登录，然后再登录作为 Contoso\MReid 使用密码**pass@word1**。  
  
7.  从桌面上，打开**财经文档**共享的文件夹。  
  
8.  打开**财经备注**文档。 附近的文档底部，你将看到字词**机密**。 修改读取：**Contoso 机密**。 保存的文档，并将其关闭。  
  
9. 打开**到雇佣批准请求**文档。 在**身份证 #:**部分中，键入：777-77-7777。 保存的文档，并将其关闭。  
  
    > [!NOTE]  
    > 你可能需要等待 30 秒分类的发生。  
  
10. 切换回 ID_AD_FILE1。 在 Windows 资源管理器，导航到 D:\Finance 文档。  
  
11. 右键单击该财经备注文档，然后单击**属性**。 单击**分类**选项卡。 请注意，**影响**属性现已设置为**高**。 单击**取消**。  
  
12. 右键单击雇佣文档，然后单击可用于批准请求**属性**。  
  
13. . 单击**分类**选项卡。 请注意，**个人身份信息**属性现已设置为**高**。 单击**取消**。  
  
## <a name="BKMK_5"></a>第 5 步：验证广告 RMS 与保护  
  
#### <a name="to-verify-that-the-document-is-protected"></a>若要验证文档受保护  
  
1.  切换回 ID_AD_CLIENT1。  
  
2.  打开**到雇佣批准请求**文档。  
  
3.  单击**确定**允许连接到你的广告 rms 文档。  
  
4.  你现在可以看到该文档受广告 RMS 由于它含有身份证号。  
  

