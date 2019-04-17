---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: "部署访问拒绝协助（演示步骤）"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5201441ba884fe4658b917919e60c7d20530341b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>部署访问拒绝协助（演示步骤）

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍如何配置访问拒绝协助，并验证工作正常。  
  
**本文档**  
  
-   [第 1 步：配置访问拒绝协助](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [第 2 步：配置的电子邮件通知设置](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [第 3 步：验证正确配置访问拒绝协助](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> 本主题包含示例 Windows PowerShell cmdlet，你可以使用自动所述的过程的一部分。 有关详细信息，请参阅[使用 Cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_1"></a>第 1 步：配置访问拒绝协助  
你可以使用组策略，配置在某个域中的访问拒绝协助或您可以配置协助单独每个文件服务器上通过使用控制台文件服务器资源管理器。 你还可以更改文件服务器上的特定共享文件夹的访问拒绝消息。  
  
可以配置为域访问拒绝协助使用组策略，如下所示：  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>若要使用组策略配置访问拒绝协助  
  
1.  打开组策略管理。 在服务器管理器中，单击**工具**，然后单击**组策略管理**。  
  
2.  右键单击相应的组策略，然后单击**编辑**。  
  
3.  单击**计算机配置**，单击**策略**，单击**管理模板**，单击**系统**，然后单击**Access-Denied 协助**。  
  
4.  右键单击**拒绝访问错误自定义邮件**，然后单击**编辑**。  
  
5.  选择**启用**选项。  
  
6.  配置以下选项：  
  
    1.  在**拒绝访问向用户显示以下消息**框中，键入一条消息，用户将看到他们时拒绝对文件或文件夹的访问权限。  
  
        将插入自定义的文本的消息，你可以添加宏。 探秘包括：  
  
        -   **[原始文件路径]**的用户所访问的原始文件路径。  
  
        -   **[原始文件路径文件夹]**的父文件夹的用户所访问的原始文件路径。  
  
        -   **[管理员电子邮件]**管理员电子邮件收件人列表。  
  
        -   **[数据所有者电子邮件]**数据所有者电子邮件收件人列表。  
  
    2.  选择**使用户提供协助**复选框。  
  
    3.  保留剩余默认设置。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName AllowEmailRequests -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName GenerateLog -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeDeviceClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName IncludeUserClaims -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutAdminOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName PutDataOwnerOnTo -Type DWORD -value 1  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName ErrorMessage -Type MultiString -value "Type the text that the user will see in the error message dialog box."  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\Software\Policies\Microsoft\Windows\ADR\AccessDenied" -ValueName Enabled -Type DWORD -value 1 
  
```  
  
或者，你可以访问拒绝协助单独每个文件服务器上使用配置文件服务器资源管理器主机。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>若要使用文件服务器资源管理器配置访问拒绝协助  
  
1.  打开文件服务器资源管理器。 在服务器管理器中，单击**工具**，然后单击**文件服务器资源管理器**。  
  
2.  右键单击**文件服务器资源管理器（本地）**，然后单击**配置选项**。  
  
3.  单击**Access-Denied 协助**选项卡。  
  
4.  选择**启用访问拒绝协助**复选框。  
  
5.  在**拒绝到文件或文件夹的访问向用户显示以下消息**框中，键入一条消息，用户将看到他们时拒绝对文件或文件夹的访问权限。  
  
    将插入自定义的文本的消息，你可以添加宏。 探秘包括：  
  
    -   **[原始文件路径]**的用户所访问的原始文件路径。  
  
    -   **[原始文件路径文件夹]**的父文件夹的用户所访问的原始文件路径。  
  
    -   **[管理员电子邮件]**管理员电子邮件收件人列表。  
  
    -   **[数据所有者电子邮件]**数据所有者电子邮件收件人列表。  
  
6.  单击**配置电子邮件请求**、选择**使用户提供协助**复选框，然后依次单击**确定**。  
  
7.  单击**预览**如果你想要查看给用户外观的错误消息。  
  
8.  单击**确定**。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
配置访问拒绝协助后，你必须启用它的所有文件类型使用组策略。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>若要为所有文件类型，请使用组策略配置访问拒绝协助  
  
1.  打开组策略管理。 在服务器管理器中，单击**工具**，然后单击**组策略管理**。  
  
2.  右键单击相应的组策略，然后单击**编辑**。  
  
3.  单击**计算机配置**，单击**策略**，单击**管理模板**，单击**系统**，然后单击**Access-Denied 协助**。  
  
4.  右键单击**的所有文件类型，使客户端上的访问拒绝协助**，然后单击**编辑**。  
  
5.  单击**启用**，然后单击**确定**。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
你还可以通过使用控制台文件服务器资源管理器文件服务器上指定单独的访问拒绝邮件每个共享文件夹。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>若要指定的共享文件夹的单独访问拒绝消息使用文件服务器资源管理器  
  
1.  打开文件服务器资源管理器。 在服务器管理器中，单击**工具**，然后单击**文件服务器资源管理器**。  
  
2.  展开**文件服务器资源管理器（本地）**，然后单击**分类管理**。  
  
3.  右键单击**分类属性**，然后单击**设置文件夹管理属性**。  
  
4.  在**属性**框中，单击**Access-Denied 协助消息**，然后单击**添加**。  
  
5.  单击**浏览**，然后选择应有自定义拒绝访问消息该文件夹。  
  
6.  在**值**框中，键入应时，他们无法访问该文件夹中的资源向用户显示消息。  
  
    将插入自定义的文本的消息，你可以添加宏。 探秘包括：  
  
    -   **[原始文件路径]**的用户所访问的原始文件路径。  
  
    -   **[原始文件路径文件夹]**的父文件夹的用户所访问的原始文件路径。  
  
    -   **[管理员电子邮件]**管理员电子邮件收件人列表。  
  
    -   **[数据所有者电子邮件]**数据所有者电子邮件收件人列表。  
  
7.  单击**确定**，然后单击**关闭**。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>第 2 步：配置的电子邮件通知设置  
将访问拒绝协助消息发送每个文件服务器上，你必须配置的电子邮件通知设置。  
  
[执行此步骤，使用 Windows PowerShell](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  打开文件服务器资源管理器。 在服务器管理器中，单击**工具**，然后单击**文件服务器资源管理器**。  
  
2.  右键单击**文件服务器资源管理器（本地）**，然后单击**配置选项**。  
  
3.  单击**电子邮件通知**选项卡。  
  
4.  配置以下设置：  
  
    -   在**SMTP 服务器名称或 IP 地址**框中，键入你的组织中的 IP 地址 SMTP 服务器的名称。  
  
    -   在**默认管理员收件人**和**默认开始电子邮件地址**框中，键入文件服务器管理员的电子邮件地址。  
  
5.  单击**发送电子邮件测试**以确保电子邮件通知是否已正确配置。  
  
6.  单击**确定**。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>第 3 步：验证正确配置访问拒绝协助  
你可以验证访问拒绝协助通过让运行的 Windows 8 尝试访问共享它们有权访问共享或中的文件的用户已正确配置。 当访问拒绝消息出现时，用户应该可以看到**请求协助**按钮。 单击请求协助按钮后，用户可以指定的访问权限的原因，然后将电子邮件发送给文件夹所有者或文件服务器管理员。 电子邮件到达并包含相应的详细信息，可以为你验证文件夹所有者或文件服务器管理员联系。  
  
> [!IMPORTANT]  
> 如果你想要验证访问拒绝协助通过让运行的 Windows Server 2012 的用户，你必须文件共享连接之前安装桌面体验。  
  
## <a name="BKMK_Links"></a>请参阅  
  
-   [方案：访问拒绝协助](Scenario--Access-Denied-Assistance.md)  
  
-   [套餐以访问拒绝获取帮助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  

