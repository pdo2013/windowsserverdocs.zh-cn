---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: "附录 B 测试环境设置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: deb08b0663e5f349df7cce51ddabd4aae7f624c5
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-b-setting-up-the-test-environment"></a>附录 b：设置测试环境

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题概述的步骤，以创建动手实验中，以测试动态访问控制。 应继续遵循由于具有依赖项所执行的许多组件说明进行操作。  
  
## <a name="prerequisites"></a>先决条件  
**硬件和软件要求**  
  
适用于设置测试实验要求：  
  
-   运行 Windows Server 2008 R2 SP1 和 HYPER-V 主机服务器  
  
-   一份 Windows Server 2012 ISO  
  
-   一份 Windows 8 ISO  
  
-   Microsoft Office 2010  
  
-   运行 Microsoft Exchange Server 2003 或更高版本的服务器  
  
你需要版本以下虚拟机测试动态访问控制方案：  
  
-   DC1 （域控制器）  
  
-   DC2 （域控制器）  
  
-   文件 1 （文件服务器，并 Active Directory 权限管理服务）  
  
-   SRV1 （POP3 和 SMTP 服务器）  
  
-   客户端 1 （使用 Microsoft Outlook 客户端计算机）  
  
虚拟机密码应该如下所示：  
  
-   BUILTIN\Administrator:pass@word1  
  
-   Contoso\Administrator:pass@word1  
  
-   所有其他帐户：pass@word1  
  
## <a name="build-the-test-lab-virtual-machines"></a>版本实验虚拟机测试  
  
### <a name="install-the-hyper-v-role"></a>安装 HYPER-V 角色  
你需要在运行 Windows Server 2008 R2 SP1 的计算机上安装 HYPER-V 角色。  
  
##### <a name="to-install-the-hyper-v-role"></a>若要安装 HYPER-V 角色  
  
1.  单击**开始**，然后单击服务器管理器。  
  
2.  在角色摘要区域的主服务器管理器窗口中，单击**添加角色**。  
  
3.  在**选择服务器角色**页上，单击**HYPER-V**。  
  
4.  在**创建虚拟网络**页上，如果你想要虚拟机提供它们的网络连接，请单击一个或多个网络适配器。  
  
5.  在**确认安装选择**页上，单击**安装**。  
  
6.  必须重新启动计算机，以完成安装。 单击**关闭**以完成向导中，然后单击**是**重启计算机。  
  
7.  重新启动计算机后，请使用你用于安装角色相同帐户进行登录。 恢复配置向导将完成安装后，单击**关闭**完成向导。  
  
### <a name="create-an-internal-virtual-network"></a>创建了一个内部虚拟网络  
现在，您将创建称为 ID_AD_Network 内部虚拟网络。  
  
##### <a name="to-create-a-virtual-network"></a>若要创建虚拟网络  
  
1.  打开 HYPER-V 管理器。  
  
2.  从**操作**菜单上，单击**虚拟网络管理**。  
  
3.  下**创建虚拟网络**、 选择**内部**。  
  
4.  单击**添加**。 **新的虚拟网络**页面显示时。  
  
5.  键入**ID_AD_Network**作为新的网络的名称。 检查其他属性，并在必要时对其进行修改。  
  
6.  单击**确定**创建虚拟网络并关闭虚拟网络管理器，或单击**应用**创建虚拟网络并继续使用虚拟网络管理器。  
  
### <a name="BKMK_Build"></a>生成域控制器  
版本虚拟机要用作域控制器 (DC1)。 安装虚拟机使用 Windows Server 2012 ISO，并为手机命名 DC1。  
  
##### <a name="to-install-active-directory-domain-services"></a>若要安装 Active Directory 域服务  
  
1.  连接到 ID_AD_Network 虚拟机。 登录到 DC1 以管理员身份使用密码** pass@word1 **。  
  
2.  在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。  
  
3.  在**在开始之前**页上，单击**下一步**。  
  
4.  在**选择安装类型**页上，单击**角色基于或功能的安装**，然后单击**下一步**。  
  
5.  在**选择目标服务器**页上，单击**下一步**。  
  
6.  在**选择服务器角色**页上，单击**Active Directory 域服务**。 在**添加角色和功能向导**对话框中，单击**添加功能**，然后单击**下一步**。  
  
7.  在**选择功能**页上，单击**下一步**。  
  
8.  在**Active Directory 域服务**页上，检查信息，然后依次单击**下一步**。  
  
9. 在**确认安装选择**页上，单击**安装**。 在结果页面上的安装进度栏功能指示安装了角色。  
  
10. 在**结果**页上，检查是否已成功，安装，然后单击**关闭**。 在服务器管理器中，单击带感叹号在屏幕的右上角的警告图标旁边**管理**。 在任务列表中，单击**推广此域控制器服务器**链接。  
  
11. 在**部署配置**页上，单击**添加新的林**，键入根域的名称**contoso.com**，然后单击**下一步**。  
  
12. 在**域控制器选项**页面上，选择了域和森林功能级别作为 Windows Server 2012，指定 DSRM 密码** pass@word1 **，然后单击**下一步**。  
  
13. 在**DNS 选项**页上，单击**下一步**。  
  
14. 在**其他选项**页上，单击**下一步**。  
  
15. 在**路径**页面上，键入 Active Directory 的数据库、 日志文件和 SYSVOL 文件夹位置 （或接受默认位置），然后单击**下一步**。  
  
16. 在**回顾选项**页上，确认你的选择，然后单击**下一步**。  
  
17. 在**先决条件检查**页上，确认先决条件验证完成，然后单击**安装**。  
  
18. 在**结果**页、 验证是否成功域控制器，为配置了服务器，然后单击**关闭**。  
  
19. 重新启动以完成安装广告 DS 服务器。 （默认情况下，这会自动进行。）  
  
通过使用 Active Directory 管理中心创建以下的用户。  
  
##### <a name="create-users-and-groups-on-dc1"></a>创建 DC1 上的用户和组  
  
1.  Contoso.com 以管理员身份登录。 启动 Active Directory 管理中心。  
  
2.  创建以下安全组：  
  
    |组的名称|电子邮件地址|  
    |--------------|-----------------|  
    |FinanceAdmin|financeadmin@contoso.com|  
    |FinanceException|financeexception@contoso.com|  
  
3.  创建以下部门 (OU):  
  
    |组织单位名|计算机|  
    |-----------|-------------|  
    |FileServerOU|文件 1|  
  
4.  创建指示的特性与以下的用户：  
  
    |用户|用户名|电子邮件地址|部门|组|国家/地区|  
    |--------|------------|-----------------|--------------|---------|-------------------|  
    |Myriam Delesalle|MDelesalle|MDelesalle@contoso.com|财经||我们|  
    |王华|MReid|MReid@contoso.com|财经|FinanceAdmin|我们|  
    |Esther Valle|EValle|EValle@contoso.com|操作|FinanceException|我们|  
    |Maira Wenzel|MWenzel|MWenzel@contoso.com|小时||我们|  
    |Jeff 低|JLow|JLow@contoso.com|小时||我们|  
    |Rms|rms|rms@contoso.com||||  
  
    有关创建组安全的详细信息，请参阅[创建新组](https://technet.microsoft.com/library/dd861305.aspx)Windows Server 网站上。  
  
##### <a name="to-create-a-group-policy-object"></a>若要创建组策略对象  
  
1.  将鼠标指针悬停将光标放在屏幕的右上角，然后单击搜索图标。 在搜索框中，键入**组策略管理**，然后单击**组策略管理**。  
  
2.  展开**森林： contoso.com**，然后展开**域**，导航到**contoso.com**，展开**(contoso.com)**，然后选择**FileServerOU**。 右键单击**此域中创建一个 GPO 并将其以下链接**
  
3.  键入描述性 GPO 名称，如**FlexibleAccessGPO**，然后单击**确定**。  
  
##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>若要为 contoso.com 启用动态访问控制  
  
1.  打开组策略管理控制台中，单击**contoso.com**，然后双击**域控制器**。  
  
2.  右键单击**默认域控制器策略**，然后选择**编辑**。  
  
3.  在组策略编辑器中管理窗口中，双击**计算机配置**，双击**策略**，双击**管理模板**，双击**系统**，然后双击**KDC**。  
  
4.  双击**KDC 支持索赔，复合身份验证、 Kerberos 程度**并选择下一步选项**启用**。 你需要启用此设置用于中央访问策略。  
  
5.  打开提升了权限的命令提示符下，并运行以下命令：  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_FS1"></a>文件服务器和广告 rms (文件 1) 版本  
  
1.  Windows Server 2012 ISO 从版本虚拟机文件 1 的名称。  
  
2.  连接到 ID_AD_Network 虚拟机。  
  
3.  虚拟机加入 contoso.com 域中，并为 contoso\administrator 使用密码然后登录到文件 1 ** pass@word1 **。  
  
#### <a name="install-file-services-resource-manager"></a>安装文件服务资源管理器  
  
###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>若要安装文件服务角色和文件服务器的资源管理器  
  
1.  在服务器管理器中，单击**添加角色和功能**。  
  
2.  在**在开始之前**页上，单击**下一步**。  
  
3.  在**选择安装类型**页上，单击**下一步**。  
  
4.  在**选择目标服务器**页上，单击**下一步**。  
  
5.  在**选择服务器角色**页面上，展开**文件和存储服务**，选择旁边的复选框**文件和 iSCSI 服务**，展开，然后选择**文件服务器资源管理器**。  
  
    在添加角色和功能向导中，单击**添加功能**，然后单击**下一步**。  
  
6.  在**选择功能**页上，单击**下一步**。  
  
7.  在**确认安装选择**页上，单击**安装**。  
  
8.  在**安装进度**页上，单击**关闭**。  
  
#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>安装文件服务器上的 Microsoft Office 筛选器包  
若要启用的不是默认提供的 Office 文件范围更广深刻的 Ifilter 的 Windows Server 2012 上，则应安装 Microsoft Office 筛选器包。  Windows Server 2012 不具有 Microsoft Office 文件默认情况下，安装任何 Ifilter 和文件分类基础结构使用 Ifilter 执行内容分析。  
  
若要下载和安装 Ifilter，请参阅[Microsoft Office 2010 筛选器包](https://go.microsoft.com/fwlink/?LinkID=234122)。  
  
#### <a name="configure-email-notifications-on-file1"></a>配置文件 1 上的电子邮件通知  
当您创建配额和文件屏幕时，你可以选择时磁盘限制即将或在用户尝试将已阻止的文件保存到用户发送电子邮件通知。 如果你想经常通知配额和文件屏蔽事件的某些管理员，可以将默认的一个或多个收件人配置为。 若要发送这些通知，你必须指定 SMTP 服务器以用于发送电子邮件。  
  
###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>若要配置文件服务器资源管理器中的电子邮件选项  
  
1.  打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击**开始**，类型**文件服务器资源管理器**，然后单击**文件服务器资源管理器**。  
  
2.  在文件服务器资源管理器界面中，右键单击**文件服务器资源管理器**，然后单击**配置选项**。 **文件服务器资源管理器选项**对话框中打开。  
  
3.  在**电子邮件通知**选项卡下 SMTP 服务器名称或 IP 地址，键入主名称或将转发 SMTP 服务器的 IP 地址发送电子邮件通知。  
  
4.  如果你想要定期通知的配额某些管理员或下文件筛选事件**默认管理员收件人**，键入每个电子邮件地址，如fileadmin@contoso.com。使用格式account@domain，并使用分号单独的多个帐户。  
  
#### <a name="create-groups-on-file1"></a>在文件 1 中创建组  
  
###### <a name="to-create-security-groups-on-file1"></a>若要创建安全分组文件 1  
  
1.  作为 contoso\administrator，使用密码登录到文件 1: ** pass@word1 **。  
  
2.  添加到 NT AUTHORITY\Authenticated 用户**WinRMRemoteWMIUsers__**组。  
  
#### <a name="create-files-and-folders-on-file1"></a>在文件 1 中创建的文件和文件夹  
  
1.  在文件 1 中创建新 NTFS 卷，然后创建以下文件夹： D:\Finance 文档。  
  
2.  指定的详细信息创建以下文件：  
  
    -   **财务 Memo.docx**： 添加一些财务相关的文档中的文本。 例如，业务规则哪些人可以访问财经文档关于已更改。 财经文档仅现在 FinanceExpert 组成员可以通过访问。 其他部门或组有权。 你需要在评估此之前环境中实现更改的影响。 确保本文档与每一页上页脚具有 CONTOSO 机密。  
  
    -   **到 Hire.docx 批准请求**： 创建收集申请人信息本文档中的窗体。 你必须在文档中的以下字段：**申请人名称、 身份证号、 职务、 提出薪金、 启动日期、 主管名称，部门**。 添加已窗体文档中的其他部分**主管签名，数字批准薪金，确认提供**，并**状态提供**。   
        请启用文档版权管理。  
  
    -   **Word Document1.docx**： 本文档中添加一些的测试内容。  
  
    -   **Word Document2.docx**： 添加测试本文的内容。  
  
    -   **Workbook1.xlsx**  
  
    -   **Workbook2.xlsx**  
  
    -   在桌面上称为常规表情创建一个文件夹。 创建名的文件夹下一个文本文档**RegEx SSN**。 在文件中，键入以下内容再保存并关闭该文件：   
        ^(?!000)([0-7]\d{2}|7([0-7]\d|7[012])) ([-]？)(?!00) \d\d\3 （？ ！0000) \d {4} $  
  
3.  共享文件夹 D:\Finance 文档作为财经文档，并使每个人都有读取和写入访问权限共享。  
  
> [!NOTE]  
> 中央访问策略未启用系统上的默认情况下，或者启动音量 c:。  
  
#### <a name="BKMK_CS1"></a>安装 Active Directory 权限管理服务  
添加 Active Directory 权限管理服务 (广告 RMS) 和所需的所有功能通过服务器管理器。 选择的默认值。  
  
###### <a name="to-install-active-directory-rights-management-services"></a>若要安装 Active Directory 权限管理服务  
  
1.  作为 CONTOSO\Administrator 或组成员的域管理员帐户登录文件 1。  
  
    > [!IMPORTANT]  
    > 若要安装广告 RMS 服务器角色安装程序 （在本例中为 CONTOSO\Administrator） 的帐户需要获得同时管理员计算机上本地组服务器广告 RMS 旨在安装在会员以及 Active Directory 中企业管理员组中的成员。  
  
2.  在服务器管理器中，单击**添加角色和功能**。 添加角色，功能向导将显示。  
  
3.  在**在开始之前**屏幕上，单击**下一步**。  
  
4.  在**选择安装类型**屏幕上，单击**角色/功能基于安装**，然后单击**下一步**。  
  
5.  在**选择服务器目标**屏幕上，单击**下一步**。  
  
6.  在**选择服务器角色**屏幕上，选中该框旁边**Active Directory 权限管理服务**，然后单击**下一步**。  
  
7.  在**添加所需的 Active Directory 权限管理服务的功能？**对话框中，单击**添加功能**。  
  
8.  在**选择服务器角色**屏幕上，单击**下一步**。  
  
9. 在**安装的选择功能**屏幕上，单击**下一步**。  
  
10. 在**Active Directory 权限管理服务**屏幕上，单击下一步。  
  
11. 在**选择角色服务**屏幕上，单击**下一步**。  
  
12. 在**Web 服务器角色 (IIS)**屏幕上，单击**下一步**。  
  
13. 在**选择角色服务**屏幕上，单击**下一步**。  
  
14. 在**确认安装选择**屏幕上，单击**安装**。  
  
15. 完成安装后，在**安装进度**屏幕上，单击**执行其他配置**。 显示广告 RMS 配置向导。  
  
16. 在**广告 RMS**屏幕上，单击**下一步**。  
  
17. 在**广告 RMS 群集**屏幕上，选择**创建新的广告 RMS 根群集**，然后单击**下一步**。  
  
18. 在**配置数据库**屏幕上，单击**使用该服务器上的 Windows 内部数据库**，然后单击**下一步**。  
  
    > [!NOTE]  
    > 使用 Windows 内部数据库建议测试环境只是因为它不支持广告 RMS 群集中的多个服务器。 生产部署应使用单独的数据库服务器。  
  
19. 在**服务帐户**屏幕上，在**域用户帐户**，单击**指定**，然后指定的用户名 (**contoso\rms**)，和密码 (**pass@word1**)，然后单击**确定**，然后单击**下一步**。  
  
20. 在**加密模式**屏幕上，单击**加密模式 2**。  
  
21. 在**群集键存储**屏幕上，单击**下一步**。  
  
22. 在**群集键密码**屏幕上，在**密码**和**确认密码**框、 键入** pass@word1 **，然后单击**下一步**。  
  
23. 在**群集网站**屏幕上，请确保**默认网站**选中，则，然后单击**下一步**。  
  
24. 在**群集地址**屏幕上，选择**使用加密的连接**选项，在**完全限定域名**框中，键入**FILE1.contoso.com**，然后单击**下一步**。  
  
25. 在**许可证书名称**屏幕上，接受默认名称 (**文件 1**) 在文本框中单击**下一步**。  
  
26. 在**SCP 注册**屏幕上，选择**注册 SCP**，然后单击**下一步**。  
  
27. 在**确认**屏幕上，单击**安装**。  
  
28. 在**结果**屏幕上，单击**关闭**，然后单击**关闭**上**安装进度**屏幕。 完成后，在注销然后作为 contoso\rms 使用提供的密码登录 (**pass@word1**)。  
  
29. 启动广告 RMS 主机，并导航到**权利策略模板**。  
  
    若要打开广告 RMS 控制台中，在服务器管理器中，单击**本地服务器**控制台树中，然后单击**工具**，然后单击**Active Directory 权限管理服务**。  
  
30. 单击**创建分配权利策略**模板位于右窗格中，单击**添加**，然后选择以下信息：  
  
    -   美国英语的语言：  
  
    -   仅管理员名称： Contoso 财经  
  
    -   描述： Contoso 财经管理员仅  
  
    单击**添加**，然后单击**下一步**。  
  
31. 在用户和权利部分下，单击**用户和权利**，单击**添加**，类型** financeadmin@contoso.com **，然后单击**确定**。  
  
32. 选择**完全控制**，并保留**授予直接与永远不会到期所有者 （作者） 完全控制**选定。  
  
33. 通过单击进行任何更改的其他选项卡，然后单击**完成**。 作为 CONTOSO\Administrator 登录。  
  
34. 浏览到文件夹中，C:\inetpub\wwwroot\\_wmcs\certification，选择该 ServerCertification.asmx 文件，并添加身份验证的用户已读取和写入文件权限。  
  
35. 打开 Windows PowerShell 和运行`Get-FsrmRmsTemplate`。 验证你可以看到您之前的步骤，在使用此命令此过程中创建的 RMS 模板。  
  
> [!IMPORTANT]  
> 如果你希望立即更改，以便你可以对它们进行测试你文件服务器，你需要执行以下操作：  
>   
> 1.  在文件服务器上，文件 1，打开提升了权限的命令提示符下，，并运行以下命令：  
>   
>     -   gpupdate /force。  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  在域控制器 (DC1) 上，复制 Active Directory。  
>   
>     有关步骤强制复制 Active Directory 的详细信息，请参阅[Active Directory 复制](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  
  
（可选），而不是使用添加角色和功能向导服务器管理器中，你可以使用 Windows PowerShell 来安装和配置按下面的过程中显示的广告 RMS 服务器角色。  
  
###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>安装和配置 Windows Server 2012 使用 Windows PowerShell 广告 RMS 群集  
  
1.  在为 CONTOSO\Administrator 登录，使用密码： ** pass@word1 **。  
  
    > [!IMPORTANT]  
    > 若要安装广告 RMS 服务器角色安装程序 （在本例中为 CONTOSO\Administrator） 的帐户需要获得同时管理员计算机上本地组服务器广告 RMS 旨在安装在会员以及 Active Directory 中企业管理员组中的成员。  
  
2.  在服务器桌面上，右键单击选择任务栏上的 Windows PowerShell 图标**以管理员身份运行**以打开具有管理员权限的 Windows PowerShell 提示。  
  
3.  若要使用服务器管理器 cmdlet 安装广告 RMS 服务器角色，键入：  
  
    ```  
    Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
    ```  
  
4.  创建代表广告 rms 你安装的 Windows PowerShell 驱动器。  
  
    例如，若要创建 Windows PowerShell 驱动器名为 RC 来安装和配置的第一个服务器广告 RMS 根群集中，键入：  
  
    ```  
    Import-Module ADRMS  
    New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
    ```  
  
5.  设置属性驱动器命名空间表示所需的配置设置的对象。  
  
    例如，若要将广告 RMS 服务帐户、 设置的 Windows PowerShell 命令提示符下，键入：  
  
    ```  
    $svcacct = Get-Credential  
    ```  
  
    当出现 Windows 安全对话框中时，键入广告 RMS service 帐户域用户名 CONTOSO\RMS 和分配的密码。  
  
    接下来，要广告 RMS 服务帐户为广告 RMS 群集设置，请键入以下命令：  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
    ```  
  
    接下来，若要设置广告 rms Windows PowerShell 命令提示符下，在使用 Windows 内部数据库中，键入：  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
    ```  
  
    接下来，安全地存储群集关键密码，在变量，在 Windows PowerShell 命令提示符下，键入：  
  
    ```  
    $password = Read-Host -AsSecureString -Prompt "Password:"  
    ```  
  
    键入群集的关键密码，然后按 ENTER 键。  
  
    接下来，若要将密码分配给您的广告 RMS 安装在 Windows PowerShell 命令提示符下，键入：  
  
    ```  
    Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
    ```  
  
    接下来，若要设置广告 RMS 群集地址，，在 Windows PowerShell 命令提示符下，键入：  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
    ```  
  
    接下来，若要将 SLC 名为您的广告 RMS 安装在 Windows PowerShell 命令提示符下，键入：  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
    ```  
  
    接下来，若要设置的广告 RMS 群集服务连接点 (SCP)，在 Windows PowerShell 命令提示符下，键入：  
  
    ```  
    Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
    ```  
  
6.  运行**安装 ADRMS** cmdlet。 除了安装广告 RMS 服务器角色和配置服务器，此 cmdlet 还会安装所需的广告 RMS，如有必要其他功能。  
  
    例如，若要切换到 RC 名为 Windows PowerShell 驱动器安装和配置广告 RMS，键入：  
  
    ```  
    Set-Location RC:\  
    Install-ADRMS -Path.  
    ```  
  
    键入"Y"当 cmdlet 提示你确认你想要开始安装。  
  
7.  登录到注销作为 CONTOSO\Administrator 和日志作为 CONTOSO\RMS 使用提供的密码 ("pass@word1")。  
  
    > [!IMPORTANT]  
    > 为了管理广告 rms 你使用管理服务器 （在本例中为 CONTOSO\RMS），并登录到该帐户将需要获得会员在这两个本地上广告 RMS 服务器计算机以及 Active Directory 中企业管理员组中的成员。  
  
8.  在服务器桌面上，右键单击选择任务栏上的 Windows PowerShell 图标**以管理员身份运行**以打开具有管理员权限的 Windows PowerShell 提示。  
  
9. 创建 Windows PowerShell 驱动器代表广告 rms 正在配置。  
  
    例如，若要创建一个名为 RC 配置广告 RMS 根群集的 Windows PowerShell 驱动器，键入：  
  
    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  
  
10. 若要为 Contoso 财经管理员创建新的权限模板和为其指定用户权限与完全控制你广告 RMS 安装中，在 Windows PowerShell 命令提示符下，键入：  
  
    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  
  
11. 若要验证，你可以看到新的权限模板 Contoso 财经管理员，在 Windows PowerShell 命令提示符下：  
  
    ```  
    Get-FsrmRmsTemplate  
    ```  
  
    检查此 cmdlet 以确认你上一步中创建的 RMS 模板输出不存在。  
  
### <a name="build-the-mail-server-srv1"></a>版本邮件服务器 (SRV1)  
SRV1 是 SMTP/POP3 邮件服务器。 你需要对其进行设置，以便你可以访问拒绝协助方案的一部分发送电子邮件通知。  
  
将 Microsoft Exchange Server 配置为在此计算机上。 有关详细信息，请参阅[如何安装 Exchange Server](https://go.microsoft.com/fwlink/?prd=12364)。  
  
### <a name="build-the-client-virtual-machine-client1"></a>版本客户端虚拟机 (客户端 1)  
  
##### <a name="to-build-the-client-virtual-machine"></a>若要版本客户端虚拟机  
  
1.  连接到 ID_AD_Network 的客户端 1。  
  
2.  安装 Microsoft Office 2010。  
  
3.  作为 Contoso\Administrator，登录，并使用以下信息将 Microsoft Outlook 配置。  
  
    -   你的姓名： 文件管理员  
  
    -   电子邮件地址：fileadmin@contoso.com  
  
    -   帐户类型： POP3  
  
    -   接收邮件服务器： SRV1 的静态 IP 地址  
  
    -   发送邮件服务器： SRV1 的静态 IP 地址  
  
    -   用户名：fileadmin@contoso.com  
  
    -   记住密码： 选择  
  
4.  创建的快捷方式在桌面上 contoso\administrator Outlook。  
  
5.  打开 Outlook，然后地址所有 '首次启动的消息。  
  
6.  删除已生成的所有测试消息。  
  
7.  在桌面上的所有用户指向 \\\FILE1\Finance 文档客户端虚拟机上创建新的快捷方式。  
  
8.  根据需要则重新启动。  
  
##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>启用客户虚拟机上的访问拒绝协助  
  
1.  打开注册表编辑器中，然后导航到**HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**。  
  
    -   设置**EnableShellExecuteFileStreamCheck**到**1**。  
  
    -   值： DWORD  
  
## <a name="BKMK_CF"></a>森林方案跨部署索赔的实验设置  
  
### <a name="BKMK_2.1"></a>构建 DC2 虚拟机  
  
-   Windows Server 2012 ISO 从版本虚拟机。  
  
-   作为 DC2 创建虚拟机的名称。  
  
-   连接到 ID_AD_Network 虚拟机。  
  
> [!IMPORTANT]  
> 加入域的虚拟机和部署跨林索赔类型需要虚拟机将无法解决的相关的域 Fqdn。 你可能需要手动来实现此目的在虚拟机上配置了 DNS 设置。 有关详细信息，请参阅[配置虚拟网络](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx)。  
>   
> 必须重新配置所有虚拟机图像 （服务器和客户），使用版本 4 (IPv4) 的静态 IP 地址和域名系统 (DNS) 客户端设置。 有关详细信息，请参阅[配置的静态 IP 地址 DNS 客户端](https://go.microsoft.com/fwlink/?LinkId=150952)。  
  
### <a name="BKMK_2.2"></a>设置新的林称为 adatum.com  
  
##### <a name="to-install-active-directory-domain-services"></a>若要安装 Active Directory 域服务  
  
1.  连接到 ID_AD_Network 虚拟机。 登录到 DC2 以管理员身份使用密码** Pass@word1 **。  
  
2.  在服务器管理器中，单击**管理**，然后单击**添加角色和功能**。  
  
3.  在**在开始之前**页上，单击**下一步**。  
  
4.  在**选择安装类型**页上，单击**角色基于或功能的安装**，然后单击**下一步**。  
  
5.  在**选择目标服务器**页上，单击**从服务器池选择服务器**，单击你想要安装 Active Directory 域服务 (广告 DS)，然后单击的服务器名称**下一步**。  
  
6.  在**选择服务器角色**页上，单击**Active Directory 域服务**。 在**添加角色和功能向导**对话框中，单击**添加功能**，然后单击**下一步**。  
  
7.  在**选择功能**页上，单击**下一步**。  
  
8.  在**广告 DS**页上，检查信息，然后依次单击**下一步**。  
  
9. 在**确认**页上，单击**安装**。 在结果页面上的安装进度栏功能指示安装了角色。  
  
10. 在**结果**页上，确认安装成功，然后单击有叹号上屏幕的右上角的警告图标旁边**管理**。 在任务列表中，单击**推广此域控制器服务器**链接。  
  
    > [!IMPORTANT]  
    > 如果在此时关闭安装向导中，而不是单击**推广此域控制器服务器**，你可以通过单击继续广告 DS 安装**任务**服务器管理器中。  
  
11. 在**部署配置**页上，单击**添加新的林**，键入根域的名称**adatum.com**，然后单击**下一步**。  
  
12. 在**域控制器选项**页面上，选择了域和森林功能级别作为 Windows Server 2012，指定 DSRM 密码** pass@word1 **，然后单击**下一步**。  
  
13. 在**DNS 选项**页上，单击**下一步**。  
  
14. 在**其他选项**页上，单击**下一步**。  
  
15. 在**路径**页面上，键入 Active Directory 的数据库、 日志文件和 SYSVOL 文件夹位置 （或接受默认位置），然后单击**下一步**。  
  
16. 在**回顾选项**页上，确认你的选择，然后单击**下一步**。  
  
17. 在**先决条件检查**页上，确认先决条件验证完成，然后单击**安装**。  
  
18. 在**结果**页、 验证是否成功域控制器，为配置了服务器，然后单击**关闭**。  
  
19. 重新启动以完成安装广告 DS 服务器。 （默认情况下，这会自动进行。）  
  
> [!IMPORTANT]  
> 若要确保网络配置无误，你有两个林在设置后，必须执行以下操作：  
>   
> -   作为 adatum\administrator 登录到 adatum.com。 打开 Command Prompt 窗口中，键入**nslookup contoso.com**，然后按 ENTER。  
> -   作为 contoso\administrator 登录到 contoso.com。 打开 Command Prompt 窗口中，键入**nslookup adatum.com**，然后按 ENTER。  
>   
> 如果不会出错执行这些命令，森林可以与彼此通信。 有关 nslookup 错误的详细信息，请参阅主题中的疑难解答节[使用 NSlookup.exe](https://support.microsoft.com/kb/200525)  
  
### <a name="BKMK_2.22"></a>将 contoso.com 设置为到 adatum.com 信任森林  
在此步骤中，您创建 Adatum Corporation 站点和 Contoso，Ltd.站点信任关系。  
  
##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>若要设置 Contoso 作为信任林中 Adatum 到  
  
1.  以管理员身份登录到 DC2。 在**开始**屏幕上，键入 domain.msc。  
  
2.  控制台树中 adatum.com，右键单击，然后单击属性。  
  
3.  在**信任**选项卡上，单击**新信任**，然后单击**下一步**。  
  
4.  在**信任名称**页上，键入**contoso.com**，在域名系统 (DNS) 命名思郡，然后单击**下一步**。  
  
5.  在**信任类型**页上，单击**森林信任**，然后单击**下一步**。  
  
6.  在**信任方向**页上，单击**双向**。  
  
7.  上**信任边**页上，单击**这两个域和指定的域**，然后单击**下一步**。  
  
8.  继续按照向导中的说明进行操作。  
  
### <a name="BKMK_2.4"></a>创建 Adatum 森林中的其他用户  
使用密码创建用户 Jeff 低** pass@word1 **，并将值公司特性分配**Adatum**。  
  
##### <a name="to-create-a-user-with-the-company-attribute"></a>若要创建用户公司属性  
  
1.  打开提升了权限的命令提示符下，在 Windows PowerShell 和粘贴以下代码：  
  
    ```  
    New-ADUser `  
    -SamAccountName jlow `  
    -Name "Jeff Low" `  
    -UserPrincipalName jlow@adatum.com `  
    -AccountPassword (ConvertTo-SecureString `  
    -AsPlainText "pass@word1" -Force) `  
    -Enabled $true `  
    -PasswordNeverExpires $true `  
    -Path 'CN=Users,DC=adatum,DC=com' `  
    -Company Adatum`  
  
    ```  
  
### <a name="BKMK_2.5"></a>创建 adataum.com 公司索赔类型  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>使用 Windows PowerShell 创建索赔类型  
  
1.  以管理员身份登录到 adatum.com。  
  
2.  在 Windows PowerShell 中打开提升了权限的命令提示符下，键入以下代码：  
  
    ```  
    New-ADClaimType `  
    -AppliesToClasses:@('user') `  
    -Description:"Company" `  
    -DisplayName:"Company" `  
    -ID:"ad://ext/Company:ContosoAdatum" `  
    -IsSingleValued:$true `  
    -Server:"adatum.com" `  
    -SourceAttribute:Company `  
    -SuggestedValues:@((New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Contoso", "Contoso", "")), (New-Object Microsoft.ActiveDirectory.Management.ADSuggestedValueEntry("Adatum", "Adatum", ""))) `  
  
    ```  
  
### <a name="BKMK_2.55"></a>启用上 contoso.com 公司资源属性  
  
##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>若要启用 contoso.com 的公司资源属性  
  
1.  以管理员身份登录到 contoso.com。  
  
2.  在服务器管理器中，单击**工具**，然后单击**Active Directory 管理中心**。  
  
3.  在左侧窗格中的 Active Directory 管理中心，单击**树视图**。 在左侧窗格中，单击**动态访问控制**，然后双击**资源属性**。  
  
4.  选择**公司**从**资源属性**列表中，右键单击并选择**属性**。 在**建议值**部分中，单击**添加**添加建议的值： Contoso 和 Adatum，然后单击**确定**两次。  
  
5.  选择**公司**从**资源属性**列表中，右键单击并选择**启用**。  
  
### <a name="BKMK_2.6"></a>启用 adatum.com 上的动态访问控制  
  
##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>若要为 adatum.com 启用动态访问控制  
  
1.  以管理员身份登录到 adatum.com。  
  
2.  打开组策略管理控制台中，单击**adatum.com**，然后双击**域控制器**。  
  
3.  右键单击**默认域控制器策略**，然后选择**编辑**。  
  
4.  在组策略编辑器中管理窗口中，双击**计算机配置**，双击**策略**，双击**管理模板**，双击**系统**，然后双击**KDC**。  
  
5.  双击**KDC 支持索赔，复合身份验证、 Kerberos 程度**并选择下一步选项**启用**。 你需要启用此设置用于中央访问策略。  
  
6.  打开提升了权限的命令提示符下，并运行以下命令：  
  
    ```  
    gpupdate /force  
    ```  
  
### <a name="BKMK_2.8"></a>创建 contoso.com 公司索赔类型  
  
##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>使用 Windows PowerShell 创建索赔类型  
  
1.  以管理员身份登录到 contoso.com。  
  
2.  打开提升了权限的命令提示符下，Windows PowerShell 中然后键入以下代码：  
  
    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  
  
    ```  
  
### <a name="BKMK_2.9"></a>创建中央使用规则  
  
##### <a name="to-create-a-central-access-rule"></a>若要创建中心访问规则  
  
1.  在左侧窗格中的 Active Directory 管理中心，单击**树视图**。 在左侧窗格中，单击**动态访问控制**，然后单击**中央访问规则**。  
  
2.  右键单击**中央访问规则**，单击**新建**，然后**中央访问规则**。  
  
3.  在**名称**字段中，键入**AdatumEmployeeAccessRule**。  
  
4.  在**权限**部分中，选择**为当前权限使用下列权限**选项，请单击**编辑**，然后单击**添加**。 单击**选择主体**链接，请键入**验证用户**，然后单击**确定**。  
  
5.  在**权限条目权限**对话框中，单击**添加条件**，然后输入以下条件: [**用户**] [**公司**] [**等于**] [**值**] [**Adatum**]。 应权限**修改、 阅读和执行、 读取、 写入**。  
  
6.  单击**确定**。  
  
7.  单击**确定**完成并返回到 Active Directory 管理中心的三倍。  
  
    ![解决方案指南](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)Windows PowerShell 等效命令 ***  
  
    以下 Windows PowerShell cmdlet 执行相同的功能前面的步骤。 输入上一行，每个 cmdlet，即使它们可能会显示换跨以下几个行由于格式化约束。  
  
    ```  
    New-ADCentralAccessRule `  
    -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
    -Name:"AdatumEmployeeAccessRule" `  
    -ProposedAcl:$null `  
    -ProtectedFromAccidentalDeletion:$true `  
    -Server:"contoso.com" `  
    ```  
  
### <a name="BKMK_2.10"></a>创建中心访问策略  
  
##### <a name="to-create-a-central-access-policy"></a>若要创建中心访问策略  
  
1.  以管理员身份登录到 contoso.com。  
  
2.  在 Windows PowerShell 中打开提升了权限的命令提示符下，然后将其粘贴以下代码：  
  
    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  
  
### <a name="BKMK_2.11"></a>在发布新的策略通过组策略  
  
##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>若要通过组策略文件服务器跨应用的中央访问策略  
  
1.  在**开始**屏幕上，键入**管理工具**，并在**搜索**栏中，单击**设置**。 在**设置**结果中，单击**管理工具**。 打开从组策略管理控制台**管理工具**文件夹。  
  
    > [!TIP]  
    > 如果**显示管理工具**禁用设置、 管理工具文件夹及其内容将不会显示在**设置**结果。  
  
2.  右键单击 contoso.com 域中，单击**此域中创建一个 GPO 并将其以下链接**  
  
3.  键入描述性 GPO 名称，如**AdatumAccessGPO**，然后单击**确定**。  
  
##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>将中心访问策略应用到文件服务器通过组策略  
  
1.  在**开始**屏幕上，键入**组策略管理**中**搜索**框。 打开**组策略管理**管理工具文件夹中。  
  
    > [!TIP]  
    > 如果**显示管理工具**禁用设置、 管理工具文件夹及其内容不会出现在设置结果。  
  
2.  导航到和选择 Contoso，如下所示： 组策略 Management\Forest: contoso.com\Domains\contoso.com。  
  
3.  右键单击**AdatumAccessGPO**策略，然后选择**编辑**。  
  
4.  在组策略管理编辑器中，单击**计算机配置**，展开**策略**，展开**Windows 设置**，然后单击**安全设置**。  
  
5.  展开**文件系统**，右键单击**中央访问策略**，然后单击**管理中央访问策略**。  
  
6.  在**中央访问策略配置**对话框中，单击**添加**，选择**仅 Adatum 访问策略**，然后单击**确定**。  
  
7.  关闭组策略管理编辑器。 您现在已经组策略中添加中心访问策略。  
  
### <a name="BKMK_2.12"></a>文件服务器上创建收益文件夹  
在文件 1 上, 创建新的 NTFS 音量，并创建以下文件夹： D:\Earnings。  
  
> [!NOTE]  
> 中央访问策略未启用系统上的默认情况下，或者启动音量 c:。  
  
### <a name="BKMK_2.13"></a>设置分类和收益文件夹上应用的中央访问策略  
  
##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>若要指定文件服务器上的中心访问策略  
  
1.  在 HYPER-V 管理器中连接到服务器文件 1。 使用 Contoso\Administrator，使用密码登录到服务器** pass@word1 **。  
  
2.  打开提升了权限的命令提示符下，键入： **gpupdate /force**。 这将确保组策略更改将会在服务器上的生效。  
  
3.  你还需要恢复 Active Directory 从全球的资源属性。 打开 Windows PowerShell、 键入`Update-FSRMClassificationpropertyDefinition`，然后按 ENTER。 关闭 Windows PowerShell。  
  
4.  打开 Windows 资源管理器，并导航到 D:\EARNINGS。 右键单击**收益**文件夹，然后单击**属性**。  
  
5.  单击**分类**选项卡，选择**公司**，然后选择**Adatum**中**值**字段。  
  
6.  单击**更改**、 选择**仅 Adatum 访问策略**从下拉菜单中，然后单击**应用**。  
  
7.  单击**安全**选项卡上，单击**高级**，然后单击**中央策略**选项卡。你应该看到**AdatumEmployeeAccessRule**列出。 可展开项目以查看所有 Active Directory 中创建规则时设置的权限。  
  
8.  单击**确定**返回到 Windows 资源管理器。  
  


