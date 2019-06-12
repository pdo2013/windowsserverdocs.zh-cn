---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: Deploy Implementing Retention of Information on File Servers (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0acfa6eec1d83c246c43ad32f7548ea771eb3c11
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445766"
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>Deploy Implementing Retention of Information on File Servers (Demonstration Steps)

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

通过使用文件分类基础结构和文件服务器资源管理器，你可以为文件夹设置保留期并将文件分类为合法保留。  
  
**本文档中**  
  
-   [必备条件](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [步骤 1：创建资源属性定义](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [步骤 2：配置通知](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [步骤 3：创建文件管理任务](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [步骤 4：手动分类文件](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="prerequisites"></a>先决条件  
本主题中的步骤假定你具有已针对文件过期通知进行配置的 SMTP 服务器。  
  
## <a name="BKMK_Step1"></a>步骤 1:创建资源属性定义  
在此步骤中，将启用“保留期”和“可发现性”资源属性，以便文件分类基础结构可以使用这些资源属性来标记已在网络共享文件夹上扫描的文件。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>创建资源属性定义  
  
1.  在域控制器上，以 Domain Admins 安全组成员的身份登录到服务器。  
  
2.  打开 Active Directory 管理中心。 在服务器管理器中，单击“工具”  ，然后单击“Active Directory 管理中心”  。  
  
3.  展开“动态访问控制”  ，然后单击“资源属性”  。  
  
4.  右键单击“保留期”  ，然后单击“启用”  。  
  
5.  右键单击“可发现性”  ，然后单击“启用”  。  
  
![解决方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>步骤 2:配置通知  
在此步骤中，将使用文件服务器资源管理器控制台来配置 SMTP 服务器、默认管理员电子邮件地址，以及从中发送报告的默认电子邮件地址。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>配置通知  
  
1.  以管理员安全组成员的身份登录到文件服务器。  
  
2.  在 Windows Powershell 命令提示符下，键入 **Update-FsrmClassificationPropertyDefinition**，然后按 ENTER。 这会将域控制器上创建的属性定义同步到文件服务器。  
  
3.  打开文件服务器资源管理器。 在服务器管理器中，单击“工具”  ，然后单击“文件服务器资源管理器”  。  
  
4.  右键单击“文件服务器资源管理器(本地)”  ，然后单击“配置选项”  。  
  
5.  在“电子邮件通知”  选项卡上，进行以下配置：  
  
    -   在“SMTP 服务器名称或 IP 地址”  框中，键入你网络上的 SMTP 服务器的名称。  
  
    -   在“默认管理员收件人”  框中，键入应收到通知的管理员的电子邮件地址。  
  
    -   在中**默认 '发件人"电子邮件地址**框中，键入应该用于发送通知的电子邮件地址。  
  
6.  单击 **“确定”** 。  
  
![解决方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>步骤 3:创建文件管理任务  
在此步骤中，我们使用文件服务器资源管理器控制台来创建文件管理任务，该任务将在每月最后一天运行，并使任何满足以下条件的文件过期：  
  
-   文件未归类为合法保留。  
  
-   文件归类为具有长期保留期。  
  
-   文件在最近 10 年内未修改。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>创建文件管理任务的步骤  
  
1.  以管理员安全组成员的身份登录到文件服务器。  
  
2.  打开文件服务器资源管理器。 在服务器管理器中，单击“工具”  ，然后单击“文件服务器资源管理器”  。  
  
3.  右键单击“文件管理任务”  ，然后单击“创建文件管理任务”  。  
  
4.  在“规则名称”  框中的“常规”  选项卡上，键入文件管理任务的名称，如保留任务的名称。  
  
5.  在“作用域”  选项卡上，单击“添加”  ，然后选择应该包括在该规则中的文件夹，如 D:\Finance Documents。  
  
6.  在“类型”  框中的“操作”  选项卡上，单击“文件过期”  。 在“过期目录”  框中，键入过期文件将移动到其中的本地文件服务器上文件夹的路径。 此文件夹应具有访问控制列表，该列表仅授予文件服务器管理员访问权限。  
  
7.  在“通知”  选项卡上，单击“添加”  。  
  
    -   选中“将电子邮件发送至下列管理员”  复选框。  
  
    -   选中“向用户发送一封带有受影响的文件的电子邮件”  复选框，然后单击“确定”  。  
  
8.  在“条件”  选项卡上，单击“添加”  并添加以下属性：  
  
    -   在“属性”  列表中，单击“可发现性”  。 在“运营商”  列表中，单击“不等于”  。 在“值”  列表中，单击“保留”  。  
  
    -   在“属性”  列表中，单击“保留期”  。 在“运营商”  列表中，单击“等于”  。 在“值”  列表中，单击“长期”  。  
  
9. 在“条件”  选项卡上，选中“自上次修改文件以来的天数”  复选框，并将值设置为 **3650**。  
  
10. 在“计划”  选项卡上，单击“每月”  选项，然后选中“最后一个”  复选框。  
  
11. 单击 **“确定”** 。  
  
![解决方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
```  
$fmjexpiration = New-FSRMFmjAction -Type 'Expiration' -ExpirationFolder folder  
$fmjNotificationAction = New-FsrmFmjNotificationAction -Type Email -MailTo "[FileOwner],[AdminEmail]"  
$fmjNotification = New-FsrmFMJNotification -Days 10 -Action @($fmjNotificationAction)  
$fmjCondition1 = New-FSRMFmjCondition -Property 'Discoverability_MS' -Condition 'NotEqual' -Value "Hold" 
$fmjCondition2 = New-FSRMFmjCondition -Property 'RetentionPeriod_MS' -Condition 'Equal' -Value "Long-term"  
$fmjCondition3 = New-FSRMFmjCondition -Property 'File.DateLastAccessed' -Condition 'Equal' -Value 3650  
$date = get-date  
$schedule = New-FsrmScheduledTask -Time $date -Monthly @(-1)    
$fmj1=New-FSRMFileManagementJob -Name "Retention Task" -Namespace @('D:\Finance Documents') -Action $fmjexpiration -Schedule $schedule -Notification @($fmjNotification) -Condition @( $fmjCondition1, $fmjCondition2, $fmjCondition3)  
```  
  
## <a name="BKMK_Step4"></a>步骤 4:手动分类文件  
在此步骤中，我们手动将文件分类为合法保留。 此文件的父文件夹将分类为具有长期的保留期。  
  
#### <a name="to-manually-classify-a-file"></a>手动分类文件  
  
1.  以管理员安全组成员的身份登录到文件服务器。  
  
2.  导航至已在步骤 3 中创建的文件管理任务作用域中配置的文件夹。  
  
3.  右键单击该文件夹，然后单击 **“属性”** 。  
  
4.  在“分类”  选项卡上，单击“保留期”  ，再单击“长期”  ，然后单击“确定”  。  
  
5.  右键单击该文件夹中的文件，然后单击“属性”  。  
  
6.  在“分类”  选项卡上，依次单击“可发现性  、“保留”  、“应用”  和“确定”  。  
  
7.  在文件服务器上，通过使用文件服务器资源管理器控制台来运行文件管理任务。 在文件管理任务完成后，检查该文件夹并确保该文件未移到过期目录。  
  
8.  右键单击该文件夹中的同一文件，然后单击“属性”  。  
  
9. 在“分类”  选项卡上，依次单击“可发现性  、“不适用”  、“应用”  和“确定”  。  
  
10. 在文件服务器上，通过使用文件服务器资源管理器控制台，再次运行该文件管理任务。 在文件管理任务完成后，检查该文件夹并确保该文件已移到过期目录。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [方案：实现文件服务器上信息的保留](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [规划文件服务器上的信息的保留期](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  

