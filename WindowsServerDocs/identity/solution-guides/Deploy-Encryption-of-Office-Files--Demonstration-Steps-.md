---
ms.assetid: 2c76e81a-c2eb-439f-a89f-7d3d70790244
title: Deploy Encryption of Office Files (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 529000c60a80ee33fc2aa7d09370d8ac1e06311c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850228"
---
# <a name="deploy-encryption-of-office-files-demonstration-steps"></a>Deploy Encryption of Office Files (Demonstration Steps)

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Contoso 的财务部门具有多个存储其文档的文件服务器。 这些文档可能是通用文档，也可能是具有高业务影响 (HBI) 的文档。 例如，Contoso 将包含机密信息的所有文档都视为具有高业务影响。 Contoso 希望确保其所有文档都有最小程度的保护，并且将其 HBI 文档限制为仅相应人员才可访问。 若要实现此目的，Contoso 将尝试使用文件分类基础结构 (FCI) 和 Windows Server 2012 中可用的 AD RMS。 通过使用 FCI，Contoso 可根据文档内容对其文件服务器上的所有文档进行分类，然后使用 AD RMS 来应用相应的权限策略。  
  
在此方案中，将执行以下步骤：  
  
|任务|描述|  
|--------|---------------|  
|[启用资源属性](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_1.1)|启用“影响”  和“个人身份信息”  资源属性。|  
|[创建分类规则](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_2)|创建以下分类规则：“HBI 分类规则”和“PII 分类规则”。|  
|[使用文件管理任务来通过 AD RMS 自动保护文档](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_3)|创建文件管理任务，该任务自动使用 AD RMS 来保护具有高个人身份信息 (PII) 的文档。 只有 FinanceAdmin 组的成员才有权访问包含高 PII 的文档。|  
|[查看结果](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_4)|检查文档的分类，并在更改文档内容时观察它们如何更改。 还要验证文档如何通过 AD RMS 获取保护。|  
|[验证 AD RMS 保护](Deploy-Encryption-of-Office-Files--Demonstration-Steps-.md#BKMK_5)|验证是否已使用 AD RMS 保护文档。|  
|||  
  
## <a name="BKMK_1.1"></a>步骤 1:启用资源属性  
  
#### <a name="to-enable-resource-properties"></a>启用资源属性  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_DC1。 通过使用 Contoso\Administrator 和密码登录到服务器**pass@word1**。  
  
2.  打开 Active Directory 管理中心，然后单击“树视图”。  
  
3.  展开“动态访问控制” ，并选择“资源属性” 。  
  
4.  在“显示名称”列中，向下滚动到“影响”属性。 右键单击“影响”，然后单击“启用”。  
  
5.  在“显示名称”列中，向下滚动到“个人身份信息”属性。 右键单击“个人身份信息”，然后单击“启用”。  
  
6.  若要发布“全局资源列表”中的资源属性，请在左窗格中单击“资源属性列表”，然后双击“全局资源属性列表”。  
  
7.  单击“添加”，然后向下滚动到“影响”并单击它以将其添加到该列表。 对于“个人身份信息” ，请执行相同的步骤。 单击“确定”  两次以完成操作。  
  
![解决方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=Impact_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com"  
Set-ADResourceProperty -Enabled:$true -Identity:"CN=PII_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com" 
```  
  
## <a name="BKMK_2"></a>步骤 2:创建分类规则  
此步骤介绍如何创建“高影响”  分类规则。 此规则将搜索的文档的内容，如果找到字符串"Contoso 机密"，则它将此文档归类为具有高业务影响。 此分类将覆盖之前分配的低业务影响的分类。  
  
你还将创建“高 PII”规则。 此规则将搜索文档的内容，并且如果找到身份证号，则它将此文档归类为具有高 PII 的文档。  
  
#### <a name="to-create-the-high-impact-classification-rule"></a>创建高影响分类规则  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_FILE1。 通过使用 Contoso\Administrator 和密码登录到服务器**pass@word1**。  
  
2.  你需要通过 Active Directory 刷新全局资源属性。 在 Windows PowerShell 上，键入 `Update-FSRMClassificationPropertyDefinition`，然后按 ENTER。 关闭 Windows PowerShell。  
  
3.  打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击“开始” ，然后键入“文件服务器资源管理器” ，最后单击“文件服务器资源管理器” 。  
  
4.  在“文件服务器资源管理器”的左窗格中，展开“分类管理” ，然后选择“分类规则” 。  
  
5.  在“操作”  窗格中，单击“配置分类计划” 。 在“自动分类”选项卡上，选择“启用固定日程安排”，选择“星期几”，然后选中“允许对新文件进行连续分类”复选框。 单击 **“确定”**。  
  
6.  在“操作”窗格中，单击“创建分类规则”。 此操作将打开“创建分类规则”对话框。  
  
7.  在“规则名称”框中，键入**高业务影响**。  
  
8.  在中**描述**框中，键入**确定文档是否具有基于字符串"Contoso 机密"的状态显示为高业务影响**  
  
9. 在“作用域”  选项卡上，单击“设置文件夹管理属性” ，选择“文件夹用途” ，单击“添加” ，再单击“浏览” 以浏览到作为路径的 D:\Finance Documents，单击“确定” ，然后选择名为“组文件”  的属性值，最后单击“关闭” 。 在设置管理属性之后，在“规则作用域”  选项卡上选择“组文件” 。  
  
10. 单击“分类”  选项卡。在“选择用于将属性分配给文件的方法” 下，从下拉列表中选择“内容分类器”  。  
  
11. 在“选择要分配给文件的属性”下，从下拉列表中选择“影响”。  
  
12. 在“指定一个值” 下，从下拉列表中选择“高”  。  
  
13. 在“参数”  下单击“配置” 。  在“分类参数”  对话框中，在“表达式类型”  列表中选择“字符串” 。 在“表达式”框中，键入：Contoso 机密，然后单击“确定”。  
  
14. 单击“评估类型”  选项卡。依次单击“重新评估现有的属性值” 、“覆盖现有值” 和“确定”  来完成操作。  
  
![解决方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Update-FSRMClassificationPropertyDefinition  
$date = Get-Date  
$AutomaticClassificationScheduledTask = New-FsrmScheduledTask -Time $date -Weekly @(3, 2, 4, 5,1,6,0) -RunDuration 0;  
Set-FsrmClassification -Continuous -schedule $AutomaticClassificationScheduledTask  
New-FSRMClassificationRule -Name "High Business Impact" -Property "Impact_MS" -Description "Determines if the document has a high business impact based on the presence of the string 'Contoso Confidential'" -PropertyValue "3000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("StringEx=Min=1;Expr=Contoso Confidential") -ReevaluateProperty Overwrite  
```  
  
#### <a name="to-create-the-high-pii-classification-rule"></a>创建高 PII 分类规则  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_FILE1。 通过使用 Contoso\Administrator 和密码登录到服务器**pass@word1**。  
  
2.  在桌面上，打开名为“正则表达式”的文件夹，然后打开名为“RegEx-SSN”的文本文档。 突出显示并复制以下正则表达式字符串： **^ （？ ！000) ([0-7] \d{2}| 7([0-7]\d|7[012 ([-]？)(?!00) \d\d\3 （？ ！0000) \d{4}$**。 在本步骤的后面部分会使用此字符串，因此请将它保留在你的剪贴板上。  
  
3.  打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击“开始” ，然后键入“文件服务器资源管理器” ，最后单击“文件服务器资源管理器” 。  
  
4.  在“文件服务器资源管理器”的左窗格中，展开“分类管理” ，然后选择“分类规则” 。  
  
5.  在“操作”  窗格中，单击“配置分类计划” 。 在“自动分类”选项卡上，选择“启用固定日程安排”，选择“星期几”，然后选中“允许对新文件进行连续分类”复选框。 单击“确定”。  
  
6.  在“规则名称”  框中，键入 **高 PII**。 在“描述”  框中，键入 **根据是否存在身份证号来确定文档是否为高 PII 的文档。**  
  
7.  单击“作用域”  选项卡，然后选中“组文件”  复选框。  
  
8.  单击“分类”  选项卡。在“选择用于将属性分配给文件的方法” 下，从下拉列表中选择“内容分类器”  。  
  
9. 在“选择要分配给文件的属性”下，从下拉列表中选择“个人身份信息”。  
  
10. 在“指定一个值” 下，从下拉列表中选择“高”  。  
  
11. 在“参数”  下单击“配置” 。   
    在中**分类参数**窗口，请在**表达式类型**列表中，选择**正则表达式**。 在中**表达式**框中，从剪贴板粘贴以下文本： **^ （？ ！000) ([0-7] \d{2}| 7([0-7]\d|7[012 ([-]？)(?!00) \d\d\3 （？ ！0000) \d{4}$**，然后单击**确定**。  
  
    > [!NOTE]  
    > 此表达式将允许无效的身份证号。 这使得我们可以在演示中使用虚构的身份证号。  
  
12. 单击“评估类型”  选项卡。依次选择“重新评估现有的属性值” 和“覆盖现有值” ，然后单击“确定”  来完成操作。  
  
![解决方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
New-FSRMClassificationRule -Name "High PII" -Description "Determines if the document has a high PII based on the presence of a Social Security Number." -Property "PII_MS" -PropertyValue "5000" -Namespace @("D:\Finance Documents") -ClassificationMechanism "Content Classifier" -Parameters @("RegularExpressionEx=Min=1;Expr=^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$") -ReevaluateProperty Overwrite  
```  
  
你现在应具有两个分类规则：  
  
-   高业务影响  
  
-   高 PII  
  
## <a name="BKMK_3"></a>步骤 3:使用文件管理任务来通过 AD RMS 自动保护文档  
现在，已创建规则，以自动对基于内容的文档进行分类下, 一步是创建使用 AD RMS 自动保护某些文档基于分类的文件管理任务。 在此步骤中，你将创建可自动保护任何高 PII 文档的文件管理任务。 只有 FinanceAdmin 组的成员才有权访问包含高 PII 的文档。  
  
#### <a name="to-protect-documents-with-ad-rms"></a>使用 AD RMS 保护文档  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_FILE1。 通过使用 Contoso\Administrator 和密码登录到服务器**pass@word1**。  
  
2.  打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击“开始” ，然后键入“文件服务器资源管理器” ，最后单击“文件服务器资源管理器” 。  
  
3.  在左窗格中，选择“文件管理任务”。 在“操作”  窗格中，选择“创建文件管理任务” 。  
  
4.  在“任务名称:”  字段中，键入 **高 PII**。 在“描述”字段中，键入**高 PII 文档的自动 RMS 保护**。  
  
5.  单击“作用域”  选项卡，然后选中“组文件”  复选框。  
  
6.  单击“操作”  选项卡。在“类型” 下，选择“RMS 加密” 。 单击“浏览”以选择模板，然后选择“仅限 Contoso 财务管理员”模板。  
  
7.  单击“条件”选项卡，然后单击“添加”。 在“属性”下，选择“个人身份信息”。 在“运营商”下，选择“等于”。 在“值”下，选择“高”。 单击 **“确定”**。  
  
8.  单击“计划”  选项卡。在“计划”部分中，单击“每周”，然后选择“周日”。 通过每周运行一次该任务，可确保你捕获由于服务中断或其他破坏性事件而错过的任何文档。  
  
9. 在“连续运行”  部分中，选择“持续在新文件上运行任务” ，然后单击“确定” 。 现在，应该具有一个名为“高 PII”的文件管理任务。  
  
![解决方案指南](media/Deploy-Encryption-of-Office-Files--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 * * *  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
$fmjRmsEncryption = New-FSRMFmjAction -Type 'Rms' -RmsTemplate 'Contoso Finance Admin Only'  
$fmjCondition1 = New-FSRMFmjCondition -Property 'PII_MS' -Condition 'Equal' -Value '5000'  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Weekly @('Sunday')    
$fmj1=New-FSRMFileManagementJob -Name "High PII" -Description "Automatic RMS protection for high PII documents" -Namespace @('D:\Finance Documents') -Action $fmjRmsEncryption -Schedule $schedule -Continuous -Condition @($fmjCondition1)  
```  
  
## <a name="BKMK_4"></a>步骤 4:查看结果  
就可以看看新自动分类和 AD RMS 保护规则中操作。 在本步骤中，你将检查文档的分类，并在更改文档内容时观察它们如何更改。  
  
#### <a name="to-view-the-results"></a>查看结果  
  
1.  在 Hyper-V 管理器中，连接到服务器 ID_AD_FILE1。 通过使用 Contoso\Administrator 和密码登录到服务器**pass@word1**。  
  
2.  在 Windows 资源管理器中，导航到 D:\Finance Documents。  
  
3.  右键单击“财务备注”文档，单击“属性” 。单击“分类”  选项卡，请注意，“影响”属性当前没有任何值。 单击“取消” 。  
  
4.  右键单击“‘请求批准聘用’文档” ，然后选择“属性” 。  
  
5.  单击“分类”选项卡，请注意，“个人身份信息”属性当前当前没有任何值。 单击“取消” 。  
  
6.  切换到 CLIENT1。 注销的任何用户都已登录，登录，然后以 contoso\mreid 身份登录的密码**pass@word1**。  
  
7.  从桌面上，打开“财务文档”  共享文件夹。  
  
8.  打开“财务备注”文档。 在接近文档底部处，会看到 **机密**一词。 将其修改为：**Contoso 机密**。 保存该文档并将其关闭。  
  
9. 打开“请求批准聘用”文档。 在“身份证号:”部分中，键入：777-77-7777。 保存该文档并将其关闭。  
  
    > [!NOTE]  
    > 可能需要等待 30 秒才会进行分类。  
  
10. 重新切换到 ID_AD_FILE1。 在 Windows 资源管理器中，导航到 D:\Finance Documents。  
  
11. 右键单击“财务备注”文档，然后单击“属性” 。 单击“分类”  选项卡。请注意，现已将“影响”属性设置为“高”。 单击“取消” 。  
  
12. 右键单击“请求批准聘用”文档，然后单击“属性” 。  
  
13. . 单击“分类”  选项卡。请注意，现已将“个人身份信息”属性设置为“高”。 单击“取消” 。  
  
## <a name="BKMK_5"></a>步骤 5:验证使用 AD RMS 的保护  
  
#### <a name="to-verify-that-the-document-is-protected"></a>验证文档是否受保护  
  
1.  重新切换到 ID_AD_CLIENT1。  
  
2.  打开“请求批准聘用”文档。  
  
3.  单击“确定”  以允许该文档连接到 AD RMS 服务器。  
  
4.  你现在可以看到该文档由 AD RMS 保护，因为它包含身份证号。  
  

