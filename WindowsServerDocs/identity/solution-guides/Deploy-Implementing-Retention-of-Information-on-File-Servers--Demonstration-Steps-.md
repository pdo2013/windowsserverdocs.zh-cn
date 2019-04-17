---
ms.assetid: ee008835-7d3b-4977-adcb-7084c40e5918
title: "部署文件服务器（演示步骤）上实现保留的信息"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e0f79dd72190888340144bc5c109ee31fa301937
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-implementing-retention-of-information-on-file-servers-demonstration-steps"></a>部署文件服务器（演示步骤）上实现保留的信息

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以保留期个文件夹的设置，并通过文件分类基础结构和文件服务器资源管理器使文件上按法律。  
  
**本文档**  
  
-   [先决条件](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Prereqs)  
  
-   [第 1 步：创建资源属性定义](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step1)  
  
-   [第 2 步：配置通知](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step2)  
  
-   [第 3 步：创建文件的管理任务](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_Step3)  
  
-   [第 4 步：手动将文件](Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-.md#BKMK_Step4)  
  
> [!NOTE]  
> 本主题包含示例 Windows PowerShell cmdlet，你可以使用自动所述的过程的一部分。 有关详细信息，请参阅[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="prerequisites"></a>先决条件  
本主题中的步骤操作假定已 SMTP 服务器配置文件过期通知。  
  
## <a name="BKMK_Step1"></a>第 1 步：创建资源属性定义  
在此步骤中，我们将启用保留期以及可发现性资源属性，以便文件分类基础结构可以使用这些资源属性标记扫描在网络共享文件夹的文件。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-create-resource-property-definitions"></a>若要创建资源属性定义  
  
1.  在上域控制器，登录到服务器的域管理员安全组成员。  
  
2.  打开 Active Directory 管理中心。 在服务器管理器中，单击**工具**，然后单击**Active Directory 管理中心**。  
  
3.  展开**动态访问控制**，然后单击**资源属性**。  
  
4.  右键单击**保留期间**，然后单击**启用**。  
  
5.  右键单击**可发现性**，然后单击**启用**。  
  
![解决方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=RetentionPeriod_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
Set-ADResourceProperty -Enabled:$true -Identity:'CN=Discoverability_MS,CN=Resource Properties,CN=Claims Configuration,CN=Services,CN=Configuration,DC=contoso,DC=com'  
```  
  
## <a name="BKMK_Step2"></a>第 2 步：配置通知  
在此步骤中，我们使用文件服务器资源管理器控制台配置 SMTP server、默认管理员电子邮件地址，以及从发送报告的默认电子邮件地址。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
#### <a name="to-configure-notifications"></a>若要配置通知  
  
1.  组成员的管理员安全登录到文件服务器。  
  
2.  Windows PowerShell 命令提示符下，键入**更新 FsrmClassificationPropertyDefinition**，然后按 ENTER。 这将同步到文件服务器的域控制器创建的属性定义。  
  
3.  打开文件服务器资源管理器。 在服务器管理器中，单击**工具**，然后单击**文件服务器资源管理器**。  
  
4.  右键单击**文件服务器资源管理器（本地）**，然后单击**配置选项**。  
  
5.  在**电子邮件通知**选项卡上，将配置以下：  
  
    -   在**SMTP 服务器名称或 IP 地址**框中，键入你的网络上 SMTP 服务器的名称。  
  
    -   在**默认管理员收件人**框中，键入应会收到通知的管理员的电子邮件地址。  
  
    -   在**默认"从"电子邮件地址**框中，键入应用于发送通知电子邮件地址。  
  
6.  单击**确定**。  
  
![解决方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
Set-FsrmSetting -SmtpServer IP address of SMTP server -FromEmailAddress "FromEmailAddress" -AdminEmailAddress "AdministratorEmailAddress"  
```  
  
## <a name="BKMK_Step3"></a>第 3 步：创建文件的管理任务  
在此步骤中，我们使用文件服务器资源管理器主机创建文件的管理任务，将在每个月的最后一天上运行并使其过期以下条件的任何文件：  
  
-   该文件不将其划分为正在法律暂停。  
  
-   有一个长期保留期属于该文件。  
  
-   该文件不已修改在过去 10 年。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep3)  
  
#### <a name="to-create-a-file-management-task"></a>若要创建文件的管理任务  
  
1.  组成员的管理员安全登录到文件服务器。  
  
2.  打开文件服务器资源管理器。 在服务器管理器中，单击**工具**，然后单击**文件服务器资源管理器**。  
  
3.  右键单击**文件管理任务**，然后单击**创建的文件管理任务**。  
  
4.  在**常规**选项卡上，在**任务名称**框中，键入的文件管理任务，如保留任务的名称。  
  
5.  在**范围**选项卡上，单击**添加**，然后选择应该包含该规则，如 D:\Finance 文档中的文件夹。  
  
6.  在**操作**选项卡上，在**类型**框中，单击**文件到期**。 在**过期目录**框中，键入将在移动过期的文件的本地文件服务器上文件夹的路径。 此文件夹应有授予访问权限的仅文件服务器管理员访问控制列表。  
  
7.  在**通知**选项卡上，单击**添加**。  
  
    -   选择**发送电子邮件与以下管理员**复选框。  
  
    -   选择**受影响文件的电子邮件发送给用户**复选框，然后依次单击**确定**。  
  
8.  在**条件**选项卡上，单击**添加**，并添加以下属性：  
  
    -   在**属性**列表中，单击**可发现性**。 在**运营商**列表中，单击**等于**。 在**值**列表中，单击**按**。  
  
    -   在**属性**列表中，单击**保留期间**。 在**运营商**列表中，单击**相等**。 在**值**列表中，单击**长期**。  
  
9. 在**条件**选项卡上，选择**天自上次修改文件**复选框，并将值设置为**3650**。  
  
10. 在**计划**选项卡上，单击**每月**选项，然后依次选择**最后一个**复选框。  
  
11. 单击**确定**。  
  
![解决方案指南](media/Deploy-Implementing-Retention-of-Information-on-File-Servers--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
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
  
## <a name="BKMK_Step4"></a>第 4 步：手动将文件  
在此步骤中，我们手动分类要法律按的文件。 此文件的父文件夹会归长期保留期间使用。  
  
#### <a name="to-manually-classify-a-file"></a>若要手动分类文件  
  
1.  组成员的管理员安全登录到文件服务器。  
  
2.  导航到了配置文件的管理任务第 3 步中创建的范围内的文件夹。  
  
3.  右键单击该文件夹，，然后单击**属性**。  
  
4.  在**分类**选项卡上，单击**保留期间**，单击**长期**，然后单击**确定**。  
  
5.  右键单击该文件夹中的文件，然后单击**属性**。  
  
6.  在**分类**选项卡上，单击**可发现性**，单击**按**，单击**应用**，然后单击**确定**。  
  
7.  在文件服务器上，通过使用控制台文件服务器资源管理器中运行文件的管理任务。 文件管理任务完成后，检查的文件夹，并确保未文件移到到期目录。  
  
8.  右键单击该文件夹内的同一个文件，然后单击**属性**。  
  
9. 在**分类**选项卡上，单击**可发现性**，单击**不适**，单击**应用**，然后单击**确定**。  
  
10. 在文件服务器上，再次运行文件的管理任务，通过使用控制台文件服务器资源管理器。 文件管理任务完成后，检查的文件夹，并确保该文件已移动到到期目录。  
  
## <a name="BKMK_Links"></a>请参阅  
  
-   [文件服务器上的方案：实现保留的信息](Scenario--Implement-Retention-of-Information-on-File-Servers.md)  
  
-   [文件服务器上的信息保留套餐](assetId:///edf13190-7077-455a-ac01-f534064a9e0c)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  

