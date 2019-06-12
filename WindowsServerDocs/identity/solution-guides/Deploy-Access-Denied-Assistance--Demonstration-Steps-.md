---
ms.assetid: b035e9f8-517f-432a-8dfb-40bfc215bee5
title: Deploy Access-Denied Assistance (Demonstration Steps)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 441dad92611e1a4a1135bd15bbcdfd05f38c1be3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445827"
---
# <a name="deploy-access-denied-assistance-demonstration-steps"></a>Deploy Access-Denied Assistance (Demonstration Steps)

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍如何配置“拒绝访问”协助，并验证它是否可以正常工作。  
  
**本文档中**  
  
-   [步骤 1：配置拒绝访问协助](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_1)  
  
-   [步骤 2：配置电子邮件通知设置](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_2)  
  
-   [步骤 3：验证已正确配置拒绝访问协助](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md#BKMK_3)  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_1"></a>步骤 1:配置“拒绝访问”协助  
你可以通过使用组策略在域中配置“拒绝访问”协助，或者可以通过使用文件服务器资源管理器控制台，在每个文件服务器上单独配置该协助。 还可以为文件服务器上的特定共享文件夹更改拒绝访问的消息。  
  
可以通过使用组策略为域配置“拒绝访问”协助，如下所示：  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1)  
  
#### <a name="to-configure-access-denied-assistance-by-using-group-policy"></a>使用组策略配置“拒绝访问”协助  
  
1.  打开“组策略管理”。 在“服务器管理器”中，单击 **“工具”** ，然后单击 **“组策略管理”** 。  
  
2.  右键单击相应的组策略，然后单击“编辑”  。  
  
3.  依次单击“计算机配置”  、“策略”  、“管理模板”  、“系统”  和“‘拒绝访问’协助”  。  
  
4.  右键单击“自定义‘拒绝访问’错误的消息”  ，然后单击“编辑”  。  
  
5.  选择“启用”  选项。  
  
6.  配置以下选项：  
  
    1.  在“向拒绝访问的用户显示以下消息”  框中，键入用户在被拒绝访问文件或文件夹时看到的消息。  
  
        可以将宏添加到将插入自定义文本的消息。 这些宏包括：  
  
        -   **[原始文件路径]** 用户已访问的原始文件路径。  
  
        -   **[原始文件路径文件夹]** 用户已访问的原始文件路径的父文件夹。  
  
        -   **[管理员电子邮件]** 管理员电子邮件收件人列表。  
  
        -   **[数据所有者电子邮件]** 数据所有者电子邮件收件人列表。  
  
    2.  选中“允许用户请求协助”  复选框。  
  
    3.  保留其余默认设置。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
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
  
或者，你可以使用文件服务器资源管理器控制台，在每个文件服务器上单独配置“拒绝访问”协助。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1a)  
  
#### <a name="to-configure-access-denied-assistance-by-using-file-server-resource-manager"></a>使用文件服务器资源管理器配置“拒绝访问”协助  
  
1.  打开文件服务器资源管理器。 在服务器管理器中，单击“工具”  ，然后单击“文件服务器资源管理器”  。  
  
2.  右键单击“文件服务器资源管理器(本地)”  ，然后单击“配置选项”  。  
  
3.  单击“‘拒绝访问’协助”  选项卡。  
  
4.  选中“启用‘拒绝访问’协助”  复选框。  
  
5.  在“向拒绝访问文件夹或文件的用户显示以下消息”  框中，键入用户在被拒绝访问文件或文件夹时看到的消息。  
  
    可以将宏添加到将插入自定义文本的消息。 这些宏包括：  
  
    -   **[原始文件路径]** 用户已访问的原始文件路径。  
  
    -   **[原始文件路径文件夹]** 用户已访问的原始文件路径的父文件夹。  
  
    -   **[管理员电子邮件]** 管理员电子邮件收件人列表。  
  
    -   **[数据所有者电子邮件]** 数据所有者电子邮件收件人列表。  
  
6.  单击“配置电子邮件请求”  ，选中“允许用户请求协助”  复选框，然后单击“确定”  。  
  
7.  如果希望查看呈现给用户的错误消息，请单击“预览”  。  
  
8.  单击 **“确定”** 。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。
  
```  
Set-FSRMAdrSetting -Event "AccessDenied" -DisplayMessage "Type the text that the user will see in the error message dialog box." -Enabled:$true -AllowRequests:$true  
```  
  
配置完“拒绝访问”协助后，你必须通过使用组策略对所有文件类型启用它。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1c)  
  
#### <a name="to-configure-access-denied-assistance-for-all-file-types-by-using-group-policy"></a>使用组策略对所有文件类型配置“拒绝访问”协助  
  
1.  打开“组策略管理”。 在“服务器管理器”中，单击 **“工具”** ，然后单击 **“组策略管理”** 。  
  
2.  右键单击相应的组策略，然后单击“编辑”  。  
  
3.  依次单击“计算机配置”  、“策略”  、“管理模板”  、“系统”  和“‘拒绝访问’协助”  。  
  
4.  右键单击“在客户端上对所有文件类型启用‘拒绝访问’协助”  ，然后单击“编辑”  。  
  
5.  单击 **“启用”** ，然后单击 **“确定”** 。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。 
  
```  
Set-GPRegistryValue -Name "Name of GPO" -key "HKLM\SOFTWARE\Policies\Microsoft\Windows\Explore" -ValueName EnableShellExecuteFileStreamCheck -Type DWORD -value 1  
  
```  
  
还可以通过使用文件服务器资源管理器控制台，为文件服务器上的每个共享文件夹指定单独的拒绝访问消息。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep1b)  
  
#### <a name="to-specify-a-separate-access-denied-message-for-a-shared-folder-by-using-file-server-resource-manager"></a>使用文件服务器资源管理器为共享文件夹指定单独的拒绝访问消息  
  
1.  打开文件服务器资源管理器。 在服务器管理器中，单击“工具”  ，然后单击“文件服务器资源管理器”  。  
  
2.  展开“文件服务器资源管理器(本地)”  ，然后单击“分类管理”  。  
  
3.  右键单击“分类属性”  ，然后单击“设置文件夹管理属性”  。  
  
4.  在“属性”  框中，单击“‘拒绝访问’协助消息”  ，然后单击“添加”  。  
  
5.  单击“浏览”  ，然后选择应具有自定义拒绝访问消息的文件夹。  
  
6.  在“值”  框中，键入在用户无法访问文件夹中的资源时应呈现给他们的消息。  
  
    可以将宏添加到将插入自定义文本的消息。 这些宏包括：  
  
    -   **[原始文件路径]** 用户已访问的原始文件路径。  
  
    -   **[原始文件路径文件夹]** 用户已访问的原始文件路径的父文件夹。  
  
    -   **[管理员电子邮件]** 管理员电子邮件收件人列表。  
  
    -   **[数据所有者电子邮件]** 数据所有者电子邮件收件人列表。  
  
7.  单击“确定”  ，然后单击“关闭”  。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。 
  
```  
Set-FSRMMgmtProperty -Namespace "folder path" -Name "AccessDeniedMessage_MS" -Value "Type the text that the user will see in the error message dialog box."  
```  
  
## <a name="BKMK_2"></a>步骤 2:配置电子邮件通知设置  
必须在每个可发送“拒绝访问”协助消息的文件服务器上配置电子邮件通知设置。  
  
[使用 Windows PowerShell 执行此步骤](assetId:///4a96cdaf-0081-4824-aab8-f0d51be501ac#BKMK_PSstep2)  
  
1.  打开文件服务器资源管理器。 在服务器管理器中，单击“工具”  ，然后单击“文件服务器资源管理器”  。  
  
2.  右键单击“文件服务器资源管理器(本地)”  ，然后单击“配置选项”  。  
  
3.  单击“电子邮件通知”  选项卡。  
  
4.  配置下列设置：  
  
    -   在“SMTP 服务器名称或 IP 地址”  框中，键入组织中 SMTP 服务器的 IP 地址的名称。  
  
    -   在中**默认管理员收件人**并**默认 '发件人电子邮件地址**框中，键入文件服务器管理员的电子邮件地址。  
  
5.  单击“发送测试电子邮件”  以确保正确配置电子邮件通知。  
  
6.  单击 **“确定”** 。  
  
![解决方案指南](media/Deploy-Access-Denied-Assistance--Demonstration-Steps-/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。
  
```  
set-FSRMSetting -SMTPServer "server1" -AdminEmailAddress "fileadmin@contoso.com" -FromEmailAddress "fileadmin@contoso.com"  
```  
  
## <a name="BKMK_3"></a>步骤 3:验证是否已正确配置“拒绝访问”协助  
你可以验证拒绝访问协助配置正确，通过让正在运行 Windows 8 尝试访问的共享或中的文件共享，他们没有访问的用户。 当出现拒绝访问的消息时，用户应该可以看到“请求协助”  按钮。 单击“请求协助”按钮后，用户可以指定访问的原因，然后可以向文件夹所有者或文件服务器管理员发送一封电子邮件。 文件夹所有者或文件服务器管理员可以为你验证该电子邮件是否已送达以及是否包含相应的详细信息。  
  
> [!IMPORTANT]  
> 如果你想要验证通过让正在运行 Windows Server 2012 的用户拒绝访问协助，你必须连接到文件共享之前安装桌面体验。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [方案：“拒绝访问”协助](Scenario--Access-Denied-Assistance.md)  
  
-   [拒绝访问协助的规划](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  

