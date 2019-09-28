---
ms.assetid: 82918181-525d-4e93-af96-957dac6aedb6
title: 附录 B 设置测试环境
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af045545826269630af9327480cda59093d219df
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407149"
---
# <a name="appendix-b-setting-up-the-test-environment"></a>附录 B：设置测试环境

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题概述了构建用于测试动态访问控制的动手实验室的步骤。 应按顺序遵循说明进行操作，因为其中存在许多具有相关性的组件。  

## <a name="prerequisites"></a>先决条件  
**硬件和软件要求**  

设置测试实验室的要求：  

-   运行带有 SP1 和 Hyper-V 的 Windows Server 2008 R2 的主机服务器  

-   Windows Server 2012 ISO 的副本  

-   Windows 8 ISO 的副本  

-   Microsoft Office 2010  

-   运行 Microsoft Exchange Server 2003 或更高版本的服务器  

你需要构建以下虚拟机来测试动态访问控制方案：  

-   DC1（域控制器）  

-   DC2（域控制器）  

-   FILE1（文件服务器和 Active Directory Rights Management Services）  

-   SRV1（POP3 和 SMTP 服务器）  

-   CLIENT1（安装了 Microsoft Outlook 的客户端计算机）  

虚拟机的密码应如下所示：  

-   BUILTIN\Administrator： pass@word1  

-   Contoso\Administrator： pass@word1  

-   所有其他帐户： pass@word1  

## <a name="build-the-test-lab-virtual-machines"></a>构建测试实验室中的虚拟机  

### <a name="install-the-hyper-v-role"></a>安装 Hyper-V 角色  
需要在运行带有 SP1 的 Windows Server 2008 R2 的计算机上安装 Hyper-V 角色。  

##### <a name="to-install-the-hyper-v-role"></a>安装 Hyper-V 角色  

1.  单击“开始”，然后再单击“服务器管理器”。  

2.  在服务器管理器主窗口的“角色摘要”区域中，单击“添加角色”。  

3.  在“选择服务器角色”页上，单击“Hyper-V”。  

4.  在“创建虚拟网络” 页上，单击一个或多个网络适配器（如果希望虚拟机可以使用其网络连接）。  

5.  在“确认安装选择”页上，单击“安装”。  

6.  必须重新启动计算机才能完成此安装。 单击“关闭”以完成向导，然后单击“是”重新启动计算机。  

7.  重新启动计算机后，请使用用于安装角色的同一帐户进行登录。 在恢复配置向导完成安装后，单击“关闭”以完成该向导。  

### <a name="create-an-internal-virtual-network"></a>创建内部虚拟网络  
现在将创建名为 ID_AD_Network 的内部虚拟网络。  

##### <a name="to-create-a-virtual-network"></a>创建虚拟网络  

1.  打开 Hyper-V 管理器。  

2.  在“操作”菜单上，单击“虚拟网络管理器”。  

3.  在“创建虚拟网络”下，选择“内部”。  

4.  单击**添加**。 将出现“新建虚拟网络” 页。  

5.  键入 **ID_AD_Network** 作为新网络的名称。 查看其他属性并对其进行修改（如果需要）。  

6.  单击“确定” 创建虚拟网络并关闭虚拟网络管理器，或单击“应用” 创建虚拟网络并继续使用虚拟网络管理器。  

### <a name="BKMK_Build"></a>构建域控制器  
构建要用作域控制器 (DC1) 的虚拟机。 使用 Windows Server 2012 ISO 安装虚拟机，并将其命名为 DC1。  

##### <a name="to-install-active-directory-domain-services"></a>安装 Active Directory 域服务  

1. 将虚拟机连接到 ID_AD_Network。 以管理员身份登录到 DC1，并<strong>pass@word1</strong>密码。  

2. 在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。  

3. 在“开始之前” 页上，单击“下一步”。  

4. 在“选择安装类型” 页上，单击“基于角色或基于功能的安装”，然后单击“下一步”。  

5. 在“选择目标服务器”页上，单击“下一步”。  

6. 在“选择服务器角色”页上，单击“Active Directory 域服务”。 在 **“添加角色和功能向导”** 对话框中，单击 **“添加功能”** ，然后单击 **“下一步”** 。  

7. 在“选择功能”页上，单击“下一步”。  

8. 在“Active Directory 域服务” 页上，查看信息，再单击“下一步”。  

9. 在 **“确认安装选择”** 页上，单击 **“安装”** 。 “结果”页上的功能安装进度条指示正在安装角色。  

10. 在“结果” 页上，确认安装已成功，然后单击“关闭”。 在服务器管理器中，单击屏幕右上角带有叹号的警告图标，该图标位于“管理”旁。 在任务列表中，单击“将此服务器升级为域控制器”链接。  

11. 在“部署配置”页上，单击“添加新林”，再键入根域名 **contoso.com**，然后单击“下一步”。  

12. 在 "**域控制器选项**" 页上，选择 "域和林功能级别" 作为 "Windows Server 2012"，指定 DSRM 密码<strong>pass@word1</strong>，然后单击 "**下一步**"。  

13. 在“DNS 选项”页上，单击“下一步”。  

14. 在“其他选项” 页上，单击“下一步”。  

15. 在“路径”页上，键入 Active Directory 数据库、日志文件和 SYSVOL 文件夹的位置（或接受默认位置），然后单击“下一步”。  

16. 在“查看选项”页上，确认你的选择，然后单击“下一步”。  

17. 在“先决条件检查”页上，确认先决条件验证已完成，然后单击“安装”。  

18. 在“结果” 页上，验证是否已将服务器成功配置为域控制器，然后单击“关闭”。  

19. 重新启动服务器以完成 AD DS 安装。 （默认情况下，会自动执行此操作。）  

使用 Active Directory 管理中心创建以下用户。  

##### <a name="create-users-and-groups-on-dc1"></a>在 DC1 上创建用户和组  

1. 以管理员身份登录到 contoso.com。 启动 Active Directory 管理中心。  

2. 创建以下安全组：  


   |    组名    |        电子邮件地址         |
   |------------------|------------------------------|
   |   FinanceAdmin   |   financeadmin@contoso.com   |
   | FinanceException | financeexception@contoso.com |


3. 创建以下组织单位 (OU)：  


   |   OU 名称    | 计算机 |
   |--------------|-----------|
   | FileServerOU |   FILE1   |


4. 使用指示的属性创建以下用户：  


   |       “用户”       |  Username  |     电子邮件地址      | 部门 |      Group       | 国家/地区 |
   |------------------|------------|------------------------|------------|------------------|----------------|
   | Myriam Delesalle | MDelesalle | MDelesalle@contoso.com |  财务组   |                  |       US       |
   |    Miles Reid    |   MReid    |   MReid@contoso.com    |  财务组   |   FinanceAdmin   |       US       |
   |   Esther Valle   |   EValle   |   EValle@contoso.com   | 操作 | FinanceException |       US       |
   |   Maira Wenzel   |  MWenzel   |  MWenzel@contoso.com   |     HR     |                  |       US       |
   |     Jeff Low     |    JLow    |    JLow@contoso.com    |     HR     |                  |       US       |
   |    RMS Server    |    rms     |    rms@contoso.com     |            |                  |                |

   有关创建安全组的详细信息，请参阅 Windows Server 网站上的 [创建新组](https://technet.microsoft.com/library/dd861305.aspx) 。  

##### <a name="to-create-a-group-policy-object"></a>创建组策略对象  

1.  将鼠标光标悬停在屏幕的右上角，然后单击搜索图标。 在搜索框中，键入“组策略管理”，然后单击“组策略管理”。  

2.  展开 **Forest: contoso.com**，然后展开“域”，导航至 **contoso.com**，展开 **(contoso.com)** ，最后选择 **FileServerOU**。 右键单击 "**在此域中创建 GPO 并在此处链接**"

3.  键入 GPO 的描述性名称（例如 **FlexibleAccessGPO**），然后单击“确定”。  

##### <a name="to-enable-dynamic-access-control-for-contosocom"></a>为 contoso.com 启用动态访问控制  

1.  打开组策略管理控制台，单击“contoso.com”，然后双击“域控制器”。  

2.  右键单击“默认域控制器策略”，然后选择“编辑”。  

3.  在“组策略管理编辑器”窗口中，依次双击“计算机配置”、“策略”、“管理模板”、“系统”和“KDC”。  

4.  双击“KDC 支持声明、复合身份验证和 Kerberos 保护” ，然后选择“启用”旁的选项。 你需要启用此设置来使用中心访问策略。  

5.  打开提升的命令提示符并运行以下命令：  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_FS1"></a>构建文件服务器和 AD RMS 服务器（FILE1）  

1. 使用 Windows Server 2012 ISO 构建名为 FILE1 的虚拟机。  

2. 将虚拟机连接到 ID_AD_Network。  

3. 将虚拟机加入到 contoso.com 域，然后使用密码<strong>pass@word1</strong>以 contoso\administrator 的身份登录到 FILE1。  

#### <a name="install-file-services-resource-manager"></a>安装文件服务资源管理器  

###### <a name="to-install-the-file-services-role-and-the-file-server-resource-manager"></a>安装文件服务角色和文件服务器资源管理器  

1.  在服务器管理器中，单击“添加角色和功能”。  

2.  在“开始之前” 页上，单击“下一步”。  

3.  在“选择安装类型”页上，单击“下一步”。  

4.  在“选择目标服务器”页上，单击“下一步”。  

5.  在“选择服务器角色”页上，展开“文件和存储服务”，再选择“文件和 iSCSI 服务”旁的复选框，展开该项，然后选择“文件服务器资源管理器”。  

    在添加角色和功能向导中，单击“添加功能”，然后单击“下一步”。  

6.  在“选择功能”页上，单击“下一步”。  

7.  在 **“确认安装选择”** 页上，单击 **“安装”** 。  

8.  在“安装进度”页上，单击“关闭”。  

#### <a name="install-the-microsoft-office-filter-packs-on-the-file-server"></a>在文件服务器上安装 Microsoft Office 过滤包  
你应在 Windows Server 2012 上安装 Microsoft Office 筛选器包，以便为比默认提供的更多的 Office 文件启用 Ifilter。  默认情况下，Windows Server 2012 没有 Ifilter 安装 Microsoft Office 文件，并且文件分类基础结构使用 Ifilter 执行内容分析。  

若要下载并安装 IFilter，请参阅 [Microsoft Office 2010 过滤包](https://go.microsoft.com/fwlink/?LinkID=234122)。  

#### <a name="configure-email-notifications-on-file1"></a>在 FILE1 上配置电子邮件通知  
在创建配额和文件屏蔽时，可以选择在接近配额限制时或者在用户尝试保存已阻止的文件后向用户发送电子邮件通知。 如果希望向特定管理员例行通知配额和文件屏蔽事件，则可以配置一个或多个默认的收件人。 若要发送这些通知，你必须指定用于转发电子邮件的 SMTP 服务器。  

###### <a name="to-configure-email-options-in-file-server-resource-manager"></a>在文件服务器资源管理器中配置电子邮件选项  

1. 打开文件服务器资源管理器。 若要打开文件服务器资源管理器，请单击“开始”，然后键入“文件服务器资源管理器”，最后单击“文件服务器资源管理器”。  

2. 在文件服务器资源管理器界面中，右键单击“文件服务器资源管理器”，然后单击“配置选项”。 此时将打开“文件服务器资源管理器选项” 对话框。  

3. 在“电子邮件通知” 选项卡上的 SMTP 服务器名称或 IP 地址下，键入将转发电子邮件通知的 SMTP 服务器的主机名或 IP 地址。  

4. 如果要定期通知特定管理员的配额或文件屏蔽事件，请在 "**默认管理员收件人**" 下键入每个电子邮件地址，例如 fileadmin@contoso.com。 使用格式 account@domain，并使用分号分隔多个帐户。  

#### <a name="create-groups-on-file1"></a>在 FILE1 上创建组  

###### <a name="to-create-security-groups-on-file1"></a>在 FILE1 上创建安全组  

1. 以 contoso\administrator 的身份登录到 FILE1，密码为： <strong>pass@word1</strong>。  

2. 向“WinRMRemoteWMIUsers__” 组添加 NT AUTHORITY\Authenticated 用户。  

#### <a name="create-files-and-folders-on-file1"></a>在 FILE1 上创建文件和文件夹  

1.  在 FILE1 上创建新的 NTFS 卷，然后创建以下文件夹：D:\Finance Documents。  

2.  根据指定的详细信息创建以下文件：  

    -   **财务备忘.docx**：在该文档中添加一些与财务相关的文本。 例如，"关于可访问财务文档的人员的业务规则已更改。 现在，只有 FinanceExpert 组的成员才能访问财务文档。 任何其他部门或组都不具有访问权限。 在环境中实现此更改之前，你需要评估它的影响。 请确保此文档每一页的页脚上都具有“CONTOSO 机密”。  

    -   **请求批准聘用.docx**：在此文档中创建一个用于收集申请人信息的表单。 此文档中必须具有以下字段：**申请人姓名、身份证号、职务、建议薪酬、开始日期、主管姓名、部门**。 在文档中添加附加部分，该部分具有用于**主管签名、已批准薪酬、聘约确认**以及**聘约状态**内容的表单。   
        启用该文档的权限管理。  

    -   **Word Document1.docx**：向此文档添加一些测试内容。  

    -   **Word Document2.docx**：向此文档添加测试内容。  

    -   **Workbook1 .xlsx**  

    -   **Workbook2 .xlsx**  

    -   在桌面上创建称为“正则表达式”的文件夹。 在称为“RegEx-SSN”的文件夹下创建一个文本文档。 在该文件中键入以下内容，然后保存并关闭该文件：   
        ^(?!000)([0-7]\d{2}|7([0-7]\d|7[012]))([ -]?)(?!00)\d\d\3(?!0000)\d{4}$  

3.  将文件夹 D:\Finance Documents 作为财务文档共享，并允许每个人具有该共享的读写访问权限。  

> [!NOTE]  
> 默认情况下，不会在系统卷 C: 或引导卷 C: 上启用中心访问策略。  

#### <a name="BKMK_CS1"></a>安装 Active Directory Rights Management Services  
通过服务器管理器添加 Active Directory Rights Management Services (AD RMS) 和所有需要的功能。 选择所有默认值。  

###### <a name="to-install-active-directory-rights-management-services"></a>安装 Active Directory Rights Management Services  

1. 以 CONTOSO\Administrator 的身份或 Domain Admins 组的成员身份登录到 FILE1。  

   > [!IMPORTANT]  
   > 若要安装 AD RMS 服务器角色，安装程序帐户（在此案例中为 CONTOSO\Administrator）必须既是要安装 AD RMS 的服务器计算机上本地 Administrators 组的成员，又是 Active Directory 中 Enterprise Admins 组的成员。  

2. 在服务器管理器中，单击“添加角色和功能”。 将出现“添加角色和功能向导”。  

3. 在“开始之前”屏幕上，单击“下一步”。  

4. 在“选择安装类型” 屏幕上，单击“基于角色/功能的安装”，然后单击“下一步”。  

5. 在“选择服务器目标”屏幕上，单击“下一步”。  

6. 在“选择服务器角色”屏幕上，选中“Active Directory Rights Management Services”旁的框，然后单击“下一步”。  

7. 在“是否添加 Active Directory Rights Management Services 所需的功能？”对话框中，单击“添加功能”。  

8. 在“选择服务器角色”屏幕上，单击“下一步”。  

9. 在“选择要安装的功能” 屏幕上，单击“下一步”。  

10. 在“Active Directory Rights Management Services”屏幕上，单击“下一步”。  

11. 在“选择角色服务” 屏幕上，单击“下一步”。  

12. 在 **“Web 服务器角色 (IIS)”** 屏幕上，单击 **“下一步”** 。  

13. 在“选择角色服务” 屏幕上，单击“下一步”。  

14. 在“确认安装选择” 屏幕上，单击“安装”。  

15. 完成安装后，在“安装进度” 屏幕上，单击“执行其他配置”。 将显示 AD RMS 配置向导。  

16. 在“AD RMS” 屏幕上，单击“下一步”。  

17. 在“AD RMS 群集” 屏幕上，选择“创建新的 AD RMS 根群集” ，然后单击“下一步”。  

18. 在“配置数据库”屏幕上，单击“在此服务器上使用 Windows 内部数据库”，然后单击“下一步”。  

    > [!NOTE]  
    > 建议仅将 Windows 内部数据库用于测试环境，因为它在 AD RMS 群集中不支持多个服务器。 生产部署应使用单独的数据库服务器。  

19. 在 "**服务帐户**" 屏幕上的 "**域用户帐户**" 中，单击 "**指定**"，然后指定用户名（**contoso\rms**）和密码（<strong>pass@word1</strong>），然后单击 "**下一步** **"** 。  

20. 在“加密模式”屏幕上，单击“加密模式 2”。  

21. 在“群集密钥存储”屏幕上，单击“下一步”。  

22. 在 "**群集密钥密码**" 屏幕上的 "**密码**" 和 "**确认密码**" 框中，键入<strong>pass@word1</strong>，然后单击 "**下一步**"。  

23. 在“群集网站” 屏幕上，请确保已选中“默认网站” ，然后单击“下一步”。  

24. 在“群集地址”屏幕上，选择“使用未加密的连接”选项，在“完全限定的域名”框中，键入“FILE1.contoso.com”，然后单击“下一步”。  

25. 在“许可方证书名称”屏幕上，接受文本框中的默认名称 (**FILE1**)，然后单击“下一步。  

26. 在“SCP 注册” 屏幕上，选择“立即注册 SCP”，然后单击“下一步。  

27. 在“确认”屏幕上，单击“安装”。  

28. 在“结果” 屏幕上，单击“关闭”，然后在“安装进度” 屏幕上，单击“关闭” 。 完成后，请使用提供的密码注销并以 contoso\rms 的身份登录（<strong>pass@word1</strong>）。  

29. 启动 AD RMS 控制台，然后导航至“权限策略模板”。  

    若要打开 AD RMS 控制台，请在服务器管理器中，单击控制台树中的“本地服务器”，再单击“工具”，然后单击“Active Directory Rights Management Services”。  

30. 单击位于右窗格的“创建分布式权限策略模板” ，再单击“添加”，然后选择以下信息：  

    -   语言：美国英语  

    -   名称：仅限 Contoso 财务管理员  

    -   说明：仅限 Contoso 财务管理员  

    单击 **“添加”** ，然后单击 **“下一步”** 。  

31. 在 "用户和权限" 部分下，依次单击 "**用户和权限**"、"**添加**"、" <strong>@no__t"-3</strong>，然后单击 **"确定"** 。  

32. 选择“完全控制”，并保留“授予所有者(作者)不会过期的完全控制权限” 的选中状态。  

33. 单击余下选项卡，而无需进行任何更改，然后单击“完成”。 以 CONTOSO\Administrator 身份登录。  

34. 浏览到文件夹 C:\inetpub\wwwroot @ no__t-0_wmcs\certification，选择 Servercertification.asmx 文件，并添加经过身份验证的用户对文件具有读取和写入权限。  

35. 打开 Windows PowerShell 并运行 `Get-FsrmRmsTemplate`。 验证是否可以通过此命令在此过程看到你在之前步骤中创建的 RMS 模板。  

> [!IMPORTANT]  
> 若需要立即更改文件服务器以便对其进行测试，则需要执行以下操作：  
>   
> 1.  在文件服务器 FILE1 上，打开提升的命令提示符，然后运行以下命令：  
>   
>     -   gpupdate /force。  
>     -   NLTEST /SC_RESET:contoso.com  
> 2.  在域控制器 (DC1) 上，复制 Active Directory。  
>   
>     有关强制复制 Active Directory 的步骤的详细信息，请参阅 [Active Directory 复制](https://technet.microsoft.com/library/cc794809(WS.10).aspx)  

也可以选择使用 Windows PowerShell 来安装并配置 AD RMS 服务器角色，而不是使用服务器管理器中的添加角色和功能向导，如以下步骤所示。  

###### <a name="to-install-and-configure-an-ad-rms-cluster-in-windows-server-2012-using-windows-powershell"></a>使用 Windows PowerShell 在 Windows Server 2012 中安装并配置 AD RMS 群集  

1. 以 CONTOSO\Administrator 的身份登录，密码为： <strong>pass@word1</strong>。  

   > [!IMPORTANT]  
   > 若要安装 AD RMS 服务器角色，安装程序帐户（在此案例中为 CONTOSO\Administrator）必须既是要安装 AD RMS 的服务器计算机上本地 Administrators 组的成员，又是 Active Directory 中 Enterprise Admins 组的成员。  

2. 在服务器桌面上，在任务栏上右键单击 Windows PowerShell 图标，然后选择“以管理员身份运行” ，以使用管理权限打开 Windows PowerShell 提示符。  

3. 若要使用服务器管理器 cmdlet 安装 AD RMS 服务器角色，请键入：  

   ```  
   Add-WindowsFeature ADRMS '"IncludeAllSubFeature '"IncludeManagementTools  
   ```  

4. 创建 Windows PowerShell 驱动器来表示你要安装的 AD RMS 服务器。  

   例如，若要创建一个名为 RC 的 Windows PowerShell 驱动器来安装和配置 AD RMS 根群集中的第一个服务器，请键入：  

   ```  
   Import-Module ADRMS  
   New-PSDrive -PSProvider ADRMSInstall -Name RC -Root RootCluster  
   ```  

5. 在驱动器命名空间中的对象上设置属性，来表示所需的配置设置。  

   例如，若要设置 AD RMS 服务帐户，请在 Windows PowerShell 命令提示符下键入：  

   ```  
   $svcacct = Get-Credential  
   ```  

   在出现“Windows 安全性”对话框后，键入 AD RMS 服务帐户域用户名 CONTOSO\RMS 和分配的密码。  

   接下来，若要为 AD RMS 群集设置指定 AD RMS 服务帐户，请键入以下内容：  

   ```  
   Set-ItemProperty -Path RC:\ -Name ServiceAccount -Value $svcacct  
   ```  

   然后，若要设置 AD RMS 服务器以使用 Windows 内部数据库，请在 Windows PowerShell 命令提示符下键入：  

   ```  
   Set-ItemProperty -Path RC:\ClusterDatabase -Name UseWindowsInternalDatabase -Value $true  
   ```  

   接着，若要将群集密钥密码安全地存储在变量中，请在 Windows PowerShell 命令提示符下键入：  

   ```  
   $password = Read-Host -AsSecureString -Prompt "Password:"  
   ```  

   键入该群集密钥密码，然后按 ENTER 键。  

   接下来，若要将密码分配给 AD RMS 安装程序，请在 Windows PowerShell 命令提示符下键入：  

   ```  
   Set-ItemProperty -Path RC:\ClusterKey -Name CentrallyManagedPassword -Value $password  
   ```  

   然后，若要设置 AD RMS 群集地址，请在 Windows PowerShell 命令提示符下键入：  

   ```  
   Set-ItemProperty -Path RC:\ -Name ClusterURL -Value "http://file1.contoso.com:80"  
   ```  

   接着，若要为 AD RMS 安装分配 SLC 名称，请在 Windows PowerShell 命令提示符下键入：  

   ```  
   Set-ItemProperty -Path RC:\ -Name SLCName -Value "FILE1"  
   ```  

   接下来，若要为 AD RMS 群集设置服务连接点 (SCP)，请在 Windows PowerShell 命令提示符下键入：  

   ```  
   Set-ItemProperty -Path RC:\ -Name RegisterSCP -Value $true  
   ```  

6. 运行 **Install-ADRMS** cmdlet。 除安装 AD RMS 服务器角色和配置服务器之外，此 cmdlet 还可安装 AD RMS 所需的其他功能（如果需要）。  

   例如，若要更改为名为 RC 的 Windows PowerShell 驱动器，并且需要安装和配置 AD RMS，请键入：  

   ```  
   Set-Location RC:\  
   Install-ADRMS -Path.  
   ```  

   当 cmdlet 提示你确认是否要开始安装时，请键入“Y”。  

7. 以 CONTOSO\Administrator 的身份注销，并使用提供的密码（"pass@word1"）作为 CONTOSO\RMS 登录。  

   > [!IMPORTANT]  
   > 若要管理 AD RMS 服务器角色，你所登录的用于管理服务器的帐户（在此案例中为 CONTOSO\RMS）必须既是 AD RMS 服务器计算机上本地 Administrators 组的成员，又是 Active Directory 中 Enterprise Admins 组的成员。  

8. 在服务器桌面上，在任务栏上右键单击 Windows PowerShell 图标，然后选择“以管理员身份运行” ，以使用管理权限打开 Windows PowerShell 提示符。  

9. 创建 Windows PowerShell 驱动器来表示你要配置的 AD RMS 服务器。  

    例如，若要创建名为 RC 的 Windows PowerShell 驱动器来配置 AD RMS 根群集，请键入：  

    ```  
    Import-Module ADRMSAdmin `  
    New-PSDrive -PSProvider ADRMSAdmin -Name RC -Root http://localhost -Force -Scope Global  
    ```  

10. 若要在 AD RMS 安装中为 Contoso 财务管理员创建新的权限模板，并为其分配具有完全控制的用户权限，请在 Windows PowerShell 命令提示符下键入：  

    ```  
    New-Item -Path RC:\RightsPolicyTemplate '"LocaleName en-us -DisplayName "Contoso Finance Admin Only" -Description "Contoso Finance Admin Only" -UserGroup financeadmin@contoso.com  -Right ('FullControl')  
    ```  

11. 若要验证你是否可以查看面向 Contoso 财务管理员的新权限模板，请在 Windows PowerShell 命令提示符下键入：  

    ```  
    Get-FsrmRmsTemplate  
    ```  

    查看此 cmdlet 的输出以确认是否存在之前步骤中创建的 RMS 模板。  

### <a name="build-the-mail-server-srv1"></a>构建邮件服务器 (SRV1)  
SRV1 是 SMTP/POP3 邮件服务器。 你需要设置它，以便将发送电子邮件通知作为“拒绝访问”协助方案的一部分。  

在此计算机上配置 Microsoft Exchange Server。 有关详细信息，请参阅 [如何安装 WSUS 3.0 服务器](https://go.microsoft.com/fwlink/?prd=12364)。  

### <a name="build-the-client-virtual-machine-client1"></a>构建客户端虚拟机 (CLIENT1)  

##### <a name="to-build-the-client-virtual-machine"></a>构建客户端虚拟机  

1. 将 CLIENT1 连接到 ID_AD_Network。  

2. 安装 Microsoft Office 2010。  

3. 以 contoso\administrator 的身份登录，并使用以下信息来配置 Microsoft Outlook。  

   - 你的名称：文件管理员  

   - 电子邮件地址： fileadmin@contoso.com  

   - 帐户类型：POP3  

   - 传入邮件服务器：SRV1 的静态 IP 地址  

   - 传出邮件服务器：SRV1 的静态 IP 地址  

   - 用户名：fileadmin@contoso.com  

   - 记住密码：选择  

4. 在 contoso\administrator 桌面上创建 Outlook 的快捷方式。  

5. 打开 Outlook 并处理所有 "首次启动" 的消息。  

6. 删除已生成的所有测试邮件。  

7. 为客户端虚拟机上的所有用户创建新的短暂切削，使其指向 \\ \ FILE1\Finance 的文档。  

8. 根据需要重新启动。  

##### <a name="enable-access-denied-assistance-on-the-client-virtual-machine"></a>在客户端虚拟机上启用“拒绝访问”协助  

1.  打开注册表编辑器，然后导航至 **HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\Explorer**。  

    -   将“EnableShellExecuteFileStreamCheck”设置为 **1**。  

    -   值：DWORD  

## <a name="BKMK_CF"></a>用于跨林部署声明方案的实验室设置  

### <a name="BKMK_2.1"></a>为 DC2 构建虚拟机  

-   从 Windows Server 2012 ISO 构建虚拟机。  

-   创建名为 DC2 的虚拟机。  

-   将虚拟机连接到 ID_AD_Network。  

> [!IMPORTANT]  
> 若要将虚拟机加入到域并跨林部署声明类型，则需要这些虚拟机能够解析相关域的 FQDN。 为完成此操作，你可能需要手动在虚拟机上配置 DNS 设置。 有关详细信息，请参阅 [配置服务器核心服务器](https://technet.microsoft.com/library/cc732470%28v=ws.10%29.aspx)。  
>   
> 必须对所有虚拟机映像（服务器和客户端）进行重新配置，以使用静态 IP 版本 4 (IPv4) 地址和域名系统 (DNS) 客户端设置。 有关详细信息，请参阅 [为静态 IP 地址配置 DNS 客户端](https://go.microsoft.com/fwlink/?LinkId=150952)。  

### <a name="BKMK_2.2"></a>设置名为 adatum.com 的新林  

##### <a name="to-install-active-directory-domain-services"></a>安装 Active Directory 域服务  

1. 将虚拟机连接到 ID_AD_Network。 以管理员身份登录到 DC2，并<strong>Pass@word1</strong>密码。  

2. 在“服务器管理器”中，单击“管理”，然后单击“添加角色和功能”。  

3. 在“开始之前” 页上，单击“下一步”。  

4. 在“选择安装类型” 页上，单击“基于角色或基于功能的安装”，然后单击“下一步”。  

5. 在“选择目标服务器”页上，单击“从服务器池中选择服务器”，并单击要安装 Active Directory 域服务 (AD DS) 的服务器的名称，然后单击“下一步”。  

6. 在“选择服务器角色” 页上，单击“Active Directory 域服务”。 在 **“添加角色和功能向导”** 对话框中，单击 **“添加功能”** ，然后单击 **“下一步”** 。  

7. 在“选择功能”页上，单击“下一步”。  

8. 在“AD DS”页上，查看信息，然后单击“下一步”。  

9. 在“确认”页上，单击“安装”。 “结果”页上的功能安装进度条指示正在安装角色。  

10. 在“结果” 页上，验证安装是否已成功，然后单击屏幕右上角带有叹号的警告图标，该图标位于“管理”旁。 在任务列表中，单击“将此服务器升级为域控制器”链接。  

    > [!IMPORTANT]  
    > 若此时关闭安装向导，而非单击“将此服务器升级为域控制器”，则你可以在服务器管理器中通过单击“任务”来继续安装 AD DS。  

11. 在“部署配置” 页上，单击“添加新林”，键入根域名 **adatum.com**，然后单击“下一步”。  

12. 在 "**域控制器选项**" 页上，选择 "域和林功能级别" 作为 "Windows Server 2012"，指定 DSRM 密码<strong>pass@word1</strong>，然后单击 "**下一步**"。  

13. 在“DNS 选项”页上，单击“下一步”。  

14. 在“其他选项” 页上，单击“下一步”。  

15. 在“路径”页上，键入 Active Directory 数据库、日志文件和 SYSVOL 文件夹的位置（或接受默认位置），然后单击“下一步”。  

16. 在“查看选项”页上，确认你的选择，然后单击“下一步”。  

17. 在“先决条件检查”页上，确认先决条件验证已完成，然后单击“安装”。  

18. 在“结果” 页上，验证是否已将服务器成功配置为域控制器，然后单击“关闭”。  

19. 重新启动服务器以完成 AD DS 安装。 （默认情况下，会自动执行此操作。）  

> [!IMPORTANT]  
> 若要确保网络配置正确，则在设置两个林之后，必须执行以下操作：  
>   
> -   以 adatum\administrator 的身份登录到 adatum.com。 打开“命令提示符”窗口，键入 **nslookup contoso.com**，然后按 ENTER。  
> -   以 contoso\administrator 的身份登录到 contoso.com。 打开“命令提示符”窗口，键入 **nslookup adatum.com**，然后按 ENTER。  
>   
> 如果这些命令能够正确执行，则这两个林可相互通信。 有关 nslookup 错误的详细信息，请参阅 [使用 NSlookup.exe](https://support.microsoft.com/kb/200525)主题中的疑难解答部分  

### <a name="BKMK_2.22"></a>将 contoso.com 设置为信任林到 adatum.com  
在此步骤中，你创建 Adatum Corporation 站点和 Contoso, Ltd. 站点之间的信任关系。  

##### <a name="to-set-contoso-as-a-trusting-forest-to-adatum"></a>将 Contoso 设置为 Adatum 的信任林  

1.  以管理员的身份登录到 DC2。 在“开始” 屏幕上，键入 domain.msc。  

2.  在控制台树中，右键单击 adatum.com，然后单击“属性”。  

3.  在 **“信任”** 选项卡上，单击 **“新建信任”** ，然后单击 **“下一步”** 。  

4.  在“信任名称”页上，在域名系统 (DNS) 的名称字段中键入 **contoso.com**，然后单击“下一步”。  

5.  在“信任类型”页上，单击“林信任”，然后单击“下一步”。  

6.  在“信任方向” 页上，单击“双向”。  

7.  在“信任方” 页上，单击“此域和指定域，然后单击“下一步”。  

8.  继续按照向导中的说明执行操作。  

### <a name="BKMK_2.4"></a>在 Adatum 林中创建其他用户  
用密码<strong>@no__t</strong>为 Jeff Low 创建用户，并分配值为**Adatum**的公司属性。  

##### <a name="to-create-a-user-with-the-company-attribute"></a>创建带有公司属性的用户  

1.  在 Windows PowerShell 中打开提升的命令提示符，然后粘贴以下代码：  

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

### <a name="BKMK_2.5"></a>在 adataum.com 上创建公司声明类型  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>使用 Windows PowerShell 创建声明类型  

1.  以管理员的身份登录到 adatum.com。  

2.  在 Windows PowerShell 中打开提升的命令提示符，然后键入以下代码：  

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

### <a name="BKMK_2.55"></a>启用 contoso.com 上的公司资源属性  

##### <a name="to-enable-the-company-resource-property-on-contosocom"></a>启用 contoso.com 上的公司资源属性  

1.  以管理员身份登录到 contoso.com。  

2.  在服务器管理器中，单击“工具”，然后单击“Active Directory 管理中心”。  

3.  在 Active Directory 管理中心的左窗格中，单击“树视图”。 在左窗格中，单击“动态访问控制”，然后双击“资源属性”。  

4.  从“资源属性” 列表中选择“公司” ，然后右键单击并选择“属性”。 在“建议的值”部分，单击“添加”以添加建议的值：Contoso 和 Adatum，然后单击“确定”两次。  

5.  从“资源属性”列表中选择“公司”，然后右键单击并选择“启用”。  

### <a name="BKMK_2.6"></a>对 adatum.com 启用动态访问控制  

##### <a name="to-enable-dynamic-access-control-for-adatumcom"></a>对 adatum.com 启用动态访问控制  

1.  以管理员的身份登录到 adatum.com。  

2.  打开组策略管理控制台，请单击“adatum.com”，然后双击“域控制器”。  

3.  右键单击“默认域控制器策略”，然后选择“编辑”。  

4.  在“组策略管理编辑器”窗口中，依次双击“计算机配置”、“策略”、“管理模板”、“系统”和“KDC”。  

5.  双击“KDC 支持声明、复合身份验证和 Kerberos 保护” ，然后选择“启用”旁的选项。 你需要启用此设置来使用中心访问策略。  

6.  打开提升的命令提示符并运行以下命令：  

    ```  
    gpupdate /force  
    ```  

### <a name="BKMK_2.8"></a>在 contoso.com 上创建公司声明类型  

##### <a name="to-create-a-claim-type-by-using-windows-powershell"></a>使用 Windows PowerShell 创建声明类型  

1.  以管理员身份登录到 contoso.com。  

2.  在 Windows PowerShell 中，打开提升的命令提示符，然后键入以下代码：  

    ```  
    New-ADClaimType '"SourceTransformPolicy `  
    '"DisplayName 'Company' `  
    '"ID 'ad://ext/Company:ContosoAdatum' `  
    '"IsSingleValued $true `  
    '"ValueType 'string' `  

    ```  

### <a name="BKMK_2.9"></a>创建中心访问规则  

##### <a name="to-create-a-central-access-rule"></a>创建中心访问规则  

1. 在 Active Directory 管理中心的左窗格中，单击“树视图”。 在左窗格中，单击“动态访问控制”，然后单击“中心访问规则”。  

2. 右键单击“中心访问规则”，单击“新建”，然后单击“中心访问规则”。  

3. 在“名称”字段中，键入 **AdatumEmployeeAccessRule**。  

4. 在“权限”部分中，选择“使用以下权限作为当前权限”选项，单击“编辑”，然后单击“添加”。 单击“选择主体” 链接，键入“经过身份验证的用户”，然后单击“确定”。  

5. 在“权限的权限条目”对话框中，单击“添加条件”，然后输入以下条件：[**User**] [**Company**] [**Equals**] [**Value**] [**Adatum**]。 权限应为“修改、读取并执行、读取、写入”。  

6. 单击 **“确定”** 。  

7. 单击“确定” 三次完成操作，然后返回到 Active Directory 管理中心。  

   @no__t 0solution 指南](media/Appendix-B--Setting-Up-the-Test-Environment/PowerShellLogoSmall.gif)***<em>Windows PowerShell 等效命令</em>***  

   下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  

   ```  
   New-ADCentralAccessRule `  
   -CurrentAcl:"O:SYG:SYD:AR(A;;FA;;;OW)(A;;FA;;;BA)(A;;FA;;;SY)(XA;;0x1301bf;;;AU;(@USER.ad://ext/Company:ContosoAdatum == `"Adatum`"))" `  
   -Name:"AdatumEmployeeAccessRule" `  
   -ProposedAcl:$null `  
   -ProtectedFromAccidentalDeletion:$true `  
   -Server:"contoso.com" `  
   ```  

### <a name="BKMK_2.10"></a>创建中心访问策略  

##### <a name="to-create-a-central-access-policy"></a>创建中心访问策略  

1.  以管理员身份登录到 contoso.com。  

2.  在 Windows PowerShell 中，打开提升的命令提示符，然后粘贴以下代码：  

    ```  
    New-ADCentralAccessPolicy "Adatum Only Access Policy"   
    Add-ADCentralAccessPolicyMember "Adatum Only Access Policy" `  
    -Member "AdatumEmployeeAccessRule" `  
    ```  

### <a name="BKMK_2.11"></a>通过组策略发布新策略  

##### <a name="to-apply-the-central-access-policy-across-file-servers-through-group-policy"></a>通过组策略跨文件服务器应用中心访问策略  

1.  在 “开始”屏幕上，键入“管理工具”，然后在 “搜索”栏中，单击“设置”。 在 “设置”结果中，单击“管理工具”。 从“管理工具” 文件夹打开组策略管理控制台。  

    > [!TIP]  
    > 如果 “显示管理工具”设置处于禁用状态，则“管理工具”文件夹及其内容将不会出现在 “设置”结果中。  

2.  右键单击 contoso.com 域，单击 "**在此域中创建 GPO 并在此处链接**"  

3.  键入 GPO 的描述性名称（例如 **AdatumAccessGPO**），然后单击“确定”。  

##### <a name="to-apply-the-central-access-policy-to-the-file-server-through-group-policy"></a>通过组策略将中心访问策略应用于文件服务器  

1.  在“开始”屏幕上，在“搜索”框中键入“组策略管理”。 从“管理工具”文件夹打开“组策略管理”。  

    > [!TIP]  
    > 如果 “显示管理工具”设置处于禁用状态，则“管理工具”文件夹及其内容将不会出现在“设置”结果中。  

2.  导航至 Contoso 并将其选中，如下所示：Group Policy Management\Forest：contoso.com\Domains\contoso.com。  

3.  右键单击“AdatumAccessGPO”策略，并选择“编辑”。  

4.  在组策略管理编辑器中，单击“计算机配置”，再依次展开“策略”和“Windows 设置”，然后单击“安全设置”。  

5.  展开“文件系统”，右键单击“中心访问策略”，然后单击“管理中心访问策略”。  

6.  在“中心访问策略配置” 对话框中，单击“添加”，选择“仅限 Adatum 访问策略”，然后单击“确定”。  

7.  关闭“组策略管理编辑器”。 现在，你已经将中心访问策略添加到组策略。  

### <a name="BKMK_2.12"></a>在文件服务器上创建收益文件夹  
在 FILE1 上创建新的 NTFS 卷，并创建以下文件夹：D:\Earnings。  

> [!NOTE]  
> 默认情况下，不会在系统卷 C: 或引导卷 C: 上启用中心访问策略。  

### <a name="BKMK_2.13"></a>设置分类并对 "收益" 文件夹应用中心访问策略  

##### <a name="to-assign-the-central-access-policy-on-the-file-server"></a>在文件服务器上分配中心访问策略  

1. 在 Hyper-V 管理器中，连接到服务器 FILE1。 使用 Contoso\Administrator 登录到服务器，密码<strong>@no__t 为-1</strong>。  

2. 打开提升的命令提示符并键入： **gpupdate /force**。 这样可以确保组策略的更改将在你的服务器上生效。  

3. 还需要通过 Active Directory 刷新全局资源属性。 在 Windows PowerShell 上，键入 `Update-FSRMClassificationpropertyDefinition`，然后按 ENTER。 关闭 Windows PowerShell。  

4. 打开 Windows 资源管理器，然后导航到 D:\EARNINGS。 右键单击“Earnings” 文件夹，然后单击“属性”。  

5. 单击“分类” 选项卡。选择“公司” ，然后在“值” 字段中选择“Adatum” 。  

6. 单击“更改”，从下拉菜单中选择“仅限 Adatum 访问策略” ，然后单击“应用”。  

7. 依次单击“安全”选项卡、“高级”和“中心策略”选项卡。你应看到列出的“AdatumEmployeeAccessRule” 。 你可以展开项以查看你在 Active Directory 中创建规则时设置的所有权限。  

8. 单击“确定”返回到 Windows 资源管理器。  



