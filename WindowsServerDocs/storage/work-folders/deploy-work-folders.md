---
ms.assetid: d2429185-9720-4a04-ad94-e89a9350cdba
title: 部署工作文件夹
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dongill
ms.author: jgerend
ms.date: 6/24/2017
description: 如何部署工作文件夹（包括安装服务器角色）、创建同步共享和创建 DNS 记录。
ms.openlocfilehash: 45b25befcde328e38f694b64fa7536a2b5c7f232
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867024"
---
# <a name="deploying-work-folders"></a>部署工作文件夹

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows 10，Windows 8.1，Windows 7

本主题介绍部署工作文件夹需要执行的步骤。 所述内容假设你已阅读[规划工作文件夹部署](plan-work-folders.md)。  
  
 若要部署工作文件夹（部署过程可能涉及到多个服务器和技术），请执行以下步骤。  
  
> [!TIP]
>  最简单的工作文件夹部署是不支持通过 Internet 同步的单一文件服务器（通常称为同步服务器），这种部署对于测试实验室而言可能非常有用，也可以用作已加入域的客户端计算机的同步解决方案。 若要创建简单部署，至少需要执行以下步骤： 
>  -   步骤 1：获取 SSL 证书  
>  -   步骤 2：创建 DNS 记录 
>  -   步骤 3：在文件服务器上安装工作文件夹  
>  -   步骤 4：在同步服务器上绑定 SSL 证书
>  -   步骤 5：创建工作文件夹的安全组  
>  -   步骤 7：为用户数据创建同步共享  
  
## <a name="step-1-obtain-ssl-certificates"></a>步骤 1：获取 SSL 证书  
 工作文件夹使用 HTTPS 以在工作文件夹客户端和工作文件夹服务器之间安全同步文件。 工作文件夹使用的 SSL 证书的要求如下：  
  
- 该证书必须由受信任的证书颁发机构颁发。 对于大多数工作文件夹实施方案，建议使用公众信任的 CA，因为证书将由未加入域的、基于 Internet 的设备使用。  
  
- 该证书必须有效。  
  
- 该证书的私钥必须可导出（因为需要在多个服务器上安装该证书）。  
  
- 证书使用者名称必须包含用于通过 Internet 发现工作文件夹服务时使用的公用工作文件夹 URL - 它必须采用格式 `workfolders.` *<domain_name>* 。  
  
- 使用者可选名称 (SAN) 必须在该证书中存在，其中列出了使用中的每个同步服务器的服务器名称。

  工作文件夹证书管理[博客](https://blogs.technet.microsoft.com/filecab/2013/08/09/work-folders-certificate-management/)提供了有关借助工作文件夹使用证书的其他信息。
  
## <a name="step-2-create-dns-records"></a>步骤 2：创建 DNS 记录  
 若要允许用户通过 Internet 同步，必须在公用 DNS 中创建一个主机 (A) 记录，使 Internet 客户端能够解析你的工作文件夹 URL。 此 DNS 记录应解析为反向代理服务器的外部接口。  
  
 在内部网络上，在名为 workfolders 的 DNS 中创建 CNAME 记录，该 DNS 解析为工作文件夹服务器的 FDQN。 工作文件夹客户端使用自动发现时，用于发现工作文件夹服务器的 URL 是 https：\//workfolders.domain.com。 如果计划使用自动发现，则 DNS 中必须存在 workfolders CNAME 记录。  
  
## <a name="step-3-install-work-folders-on-file-servers"></a>步骤 3：在文件服务器上安装工作文件夹  
 可以使用服务器管理器或 Windows PowerShell，于本地或者通过网络以远程方式在已加入域的服务器上安装工作文件夹。 如果你要通过网络配置多个同步服务器，则这种做法将十分有效。  
  
若要在服务器管理器中部署角色，请执行以下操作：  
  
1.  启动“添加角色和功能向导”。  
  
2.  在“选择安装类型” 页面上，选择“基于角色或基于功能的部署”。  
  
3.  在“选择目标服务器”页面上，选择你要在其上安装工作文件夹的服务器 。  
  
4.  在“选择服务器角色”页面上，依次展开“文件和存储服务”及“文件和 iSCSI 服务”，然后选择“工作文件夹”。  
  
5.  当系统询问你是否想要安装“IIS 可承载 Web 核心”时，请单击“确定”以安装工作文件夹所需最小版本的 Internet 信息服务 (IIS)。  
  
6.  单击“下一步”，直到完成向导操作。

若要使用 Windows PowerShell 部署该角色，请使用以下 cmdlet：
  
```powershell  
Add-WindowsFeature FS-SyncShareService  
```

## <a name="step-4-binding-the-ssl-certificate-on-the-sync-servers"></a>步骤 4：在同步服务器上绑定 SSL 证书
 工作文件夹将会安装 IIS 可承载 Web 核心，这是一个 IIS 组件，根据设计，使用它无需完整安装 IIS，就能启用 Web 服务。 安装 IIS 可承载 Web 核心后，应该将服务器的 SSL 证书绑定到文件服务器上的默认网站。 但是，IIS 可承载 Web 核心不会安装 IIS 管理控制台。

 有两个选项可用于将证书绑定到默认 Web 接口。 若要使用其中的任一选项，你必须事先将证书的私钥安装到计算机的个人存储中。

- 在装有 IIS 管理控制台的服务器上使用该控制台。 从控制台内部连接到你要管理的文件服务器，然后选择该服务器的默认网站。 默认网站将显示为已禁用，但是，你仍可以编辑站点的绑定，并选择要绑定到该网站的证书。

- 使用 netsh 命令将证书绑定到默认网站 https 接口。 命令如下：

    ```
    netsh http add sslcert ipport=<IP address>:443 certhash=<Cert thumbprint> appid={CE66697B-3AA0-49D1-BDBD-A25C8359FD5D} certstorename=MY
    ```

## <a name="step-5-create-security-groups-for-work-folders"></a>步骤 5：创建工作文件夹的安全组
 在创建同步共享之前，域管理员组或企业管理员组的成员需要在 Active Directory 域服务 (AD DS) 中为工作文件夹创建一些安全组（可能还需要按步骤 6 中所述委派某种控制）。 下面是所需的组：

- 为每个同步共享创建一个组，用于指定允许哪些用户与该同步共享同步

- 为所有工作文件夹管理员创建一个组，使他们能够编辑每个用户对象上的某个属性，该属性可将用户链接到正确的同步服务器（如果你要使用多个同步服务器）

  组应该遵守标准的命名约定，并且只应该用于工作文件夹，以免与其他安全要求造成潜在的冲突。

  若要创建适当的安全组，请执行以下过程多次 – 针对每个同步共享执行一次，还有一次是为了有选择性地为文件服务器管理员创建组。

#### <a name="to-create-security-groups-for-work-folders"></a>为工作文件夹创建安全组的步骤

1. 在装有 Active Directory 管理中心的 Windows Server 2012 R2 或 Windows Server 2016 计算机上打开服务器管理器。

2.  在“工具”菜单中单击“Active Directory 管理中心”。 此时将出现 Active Directory 管理中心。

3.  右键单击你想要创建新组的容器（例如相应域或 OU 的用户容器），依次单击“新建”和“组”。

4.  在“创建组”窗口的“组”部分，指定以下设置：

    -   在“组名”中，键入安全组的名称，例如：**HR 同步共享用户**或**工作文件夹管理员**。  
  
    -   在“组作用域”中单击“安全”，然后单击“全局”。  
  
5.  在“成员”部分中单击“添加”。 此时将出现“选择用户、联系人、计算机、服务帐户或组”对话框。  
  
6.  键入你要向其授予对特定同步共享的访问权限的用户或组的名称（如果要创建一个组用于控制对同步共享的访问），或者键入工作文件夹管理员的名称（如果要配置用户帐户以自动发现相应的同步服务器），单击**确定**，然后再次单击**确定**。

若要使用 Windows PowerShell 创建安全组，请使用以下 cmdlet：

```powershell  
$GroupName = "Work Folders Administrators"  
$DC = "DC1.contoso.com"  
$ADGroupPath = "CN=Users,DC=contoso,DC=com"  
$Members = "CN=Maya Bender,CN=Users,DC=contoso,DC=com","CN=Irwin Hume,CN=Users,DC=contoso,DC=com"  
  
New-ADGroup -GroupCategory:"Security" -GroupScope:"Global" -Name:$GroupName -Path:$ADGroupPath -SamAccountName:$GroupName -Server:$DC  
Set-ADGroup -Add:@{'Member'=$Members} -Identity:$GroupName -Server:$DC
```

## <a name="step-6-optionally-delegate-user-attribute-control-to-work-folders-administrators"></a>步骤 6：根据需要，向工作文件夹管理员委派用户属性控制  
 如果你正在部署多个同步服务器，并想要自动将用户定向到正确的同步服务器，则需要在 AD DS 中更新每个用户帐户的某个属性。 但是，这通常需要请求域管理员组或企业管理员组的成员更新属性，因此，如果你经常要添加用户或者在同步服务器之间移动用户，则很快就会感到厌烦。  
  
 出于此原因，域管理员组或企业管理员组的成员可能需要根据以下过程中所述，将修改用户对象的 msDS-SyncServerURL 属性的能力委派给你在步骤 5 中创建的工作文件夹管理员组。  
  
#### <a name="delegate-the-ability-to-edit-the-msds-syncserverurl-property-on-user-objects-in-ad-ds"></a>在 AD DS 中委派编辑用户对象的 msDS-SyncServerURL 属性的能力  
  
1.  在装有 Active Directory 用户和计算机的 Windows Server 2012 R2 或 Windows Server 2016 计算机上打开服务器管理器。  
  
2.  在“工具”菜单上，单击“Active Directory 用户和计算机”。 此时将出现 Active Directory 用户和计算机。  
  
3.  右键单击其下的所有用户对象已在工作文件夹中存在的 OU（如果用户存储在多个 OU 或域中，请右键单击所有用户公用的容器），然后单击“委派控制...”。 此时将出现控制委派向导。  
  
4.  在**用户或组**页上，单击**添加...** ， ，然后指定你为工作文件夹管理员创建的组（如 **工作文件夹管理员**）。  
  
5.  在“要委派的任务” 页面上，单击“创建要委派的自定义任务”。  
  
6.  在“Active Directory 对象类型”页面上，单击“仅限以下文件夹中的对象”，然后选中“用户对象”复选框。  
  
7.  在“权限”页面上，清除“常规”复选框，选中“特定于属性”复选框，然后选中“读取 msDS-SyncServerUrl”复选框和“写入 msDS-SyncServerUrl”复选框。

若要使用 Windows PowerShell 委派编辑用户对象的 msDS-SyncServerURL 属性的能力，请使用以下包含 DsAcls 命令的示例脚本。
  
```powershell  
$GroupName = "Contoso\Work Folders Administrators"  
$ADGroupPath = "CN=Users,dc=contoso,dc=com"  
  
DsAcls $ADGroupPath /I:S /G ""$GroupName":RPWP;msDS-SyncServerUrl;user"  
```  
  
> [!NOTE]
>  在包含大量用户的域中，委派操作可能需要较长时间才能运行完毕。  
  
## <a name="step-7-create-sync-shares-for-user-data"></a>步骤 7：为用户数据创建同步共享  
 现在，你已准备好在同步服务器上指定一个文件夹用于存储用户的文件。 此文件夹称为同步共享，你可以使用以下过程来创建。  
  
1. 如果你尚未准备好一个有足够可用空间（可以存储同步共享及其包含的用户文件）的 NTFS 卷，请创建一个新卷并使用 Windows NT 文件系统将其格式化。  
  
2. 在服务器管理器中，依次单击“文件和存储服务”和“工作文件夹”。  
  
3. 详细信息窗格的顶部将显示所有现有同步共享的列表。 若要创建新的同步共享，请在“任务”菜单中选择“新建同步共享...”。 此时将出现新建同步共享向导。  
  
4. 在“选择服务器和路径”页面上，指定该同步共享要存储到的位置 。 如果你已经为此用户数据创建了一个文件共享，则可以选择该共享。 你也可以创建一个新文件夹。  
  
   > [!NOTE]
   >  默认情况下，无法通过文件共享直接访问同步共享（除非你选择了现有的文件共享）。 如果你想要通过文件共享使同步共享可以访问，请使用服务器管理器的“共享” 磁贴或 [New-SmbShare](https://technet.microsoft.com/library/jj635722.aspx) cmdlet 创建文件共享，最好还启用了基于访问权限的枚举。  
  
5. 在“指定用户文件夹的结构”页面上，选择用户文件夹在同步共享中的命名约定。 可以使用两个选项：  
  
   - **用户别名**用于创建不包含域名的用户文件夹。 如果使用的文件共享已在文件夹重定向或其他用户数据解决方案中使用，请选择此命名约定。 （可选）你可以选中“仅同步以下子文件夹”复选框，以便仅同步特定的子文件夹（例如“文档”文件夹） 。  
  
   - <strong>用户 alias@domain</strong> 用于创建包含域名的用户文件夹。 如果你未使用已在文件夹重定向或其他用户数据解决方案中使用的文件共享，请选择此命名约定，以防在共享的多个用户具有相同的别名（当用户属于不同的域时，可能会发生这种情况）时出现文件夹命名冲突。  
  
6. 在“输入同步共享名称”页面上，指定该同步共享的名称和描述。 此信息不会在网络中播发，但会显示在服务器管理器和 Windows Powershell 中，以帮助将同步共享彼此区分开来。  
  
7. 在“向组授予同步访问权限”页面上，指定你创建的、列出了允许使用此同步共享的用户的组 。  
  
   > [!IMPORTANT]
   >  若要提高性能和安全性，请向组而不是单个用户授予访问权限，并且应尽量具体，避免向“经过身份验证的用户”和“域用户”等常规组授予访问权限。 向包含大量用户的组授予访问权限会增加工作文件夹查询 AD DS 所花费的时间。 如果有大量的用户，请创建多个同步共享以帮助分散负载。  
  
8. 在“指定设备策略”页面上，指定是否在客户端电脑和设备上请求任何安全限制 。 可以单独选择两个设备策略：  
  
   -   **加密工作文件夹** 请求在客户端电脑和设备上加密工作文件夹  
  
   -   **自动锁定屏幕，并要求输入密码** 请求客户端电脑和设备在 15 分钟后自动锁定其屏幕，要求输入包含六个或更多字符的密码来解锁屏幕，并在 10 次重试失败后激活设备锁定模式。  
  
       > [!IMPORTANT]
       >  若要强制执行 Windows 7 电脑和域加入电脑上非管理员的密码策略，请将组策略密码策略用于计算机域，并将这些域从工作文件夹密码策略中排除。 创建同步共享后，你可以使用 [Set-Syncshare -PasswordAutoExcludeDomain](https://technet.microsoft.com/library/dn296649\(v=wps.630\).aspx) cmdlet 排除域。 有关设置组策略密码策略的信息，请参阅[密码策略](https://technet.microsoft.com/library/hh994572(v=ws.11).aspx)。  
  
9. 复查所做的选择，然后完成向导操作以创建同步共享。

通过 [New-SyncShare](https://technet.microsoft.com/library/dn296635.aspx) cmdlet，你可以使用 Windows PowerShell 创建同步共享。 下面是此方法的一个示例：  
  
```powershell  
New-SyncShare "HR Sync Share" K:\Share-1 –User "HR Sync Share Users"  
```  
  
以上示例将创建一个名为 *Share01* 的新同步共享，其路径为 *K:\Share-1*，其访问权限已授予名为 *HR Sync Share Users* 的组  
  
> [!TIP]
>  创建同步共享后，可以使用文件服务器资源管理器功能来管理共享中的数据。 例如，可以使用服务器管理器中“工作文件夹”页面内的“配额”磁贴来设置用户文件夹的配额 。 还可以使用[文件屏蔽管理](https://technet.microsoft.com/library/cc732074.aspx)来控制工作文件夹要同步的文件类型，或者，使用[动态访问控制](https://technet.microsoft.com/windows-server-docs/identity/solution-guides/dynamic-access-control--scenario-overview)中所述的方案来完成更复杂的文件分类任务。  
  
## <a name="step-8-optionally-specify-a-tech-support-email-address"></a>步骤 8：（可选）指定技术支持电子邮件地址   
 在文件服务器上安装工作文件夹之后，你可能希望为服务器指定一个管理联系人电子邮件地址。 若要添加电子邮件地址，请执行以下过程：  
  
#### <a name="specifying-an-administrative-contact-email"></a>指定管理联系人电子邮件 
  
1.  在服务器管理器中，依次单击“文件和存储服务”和“服务器”。  
  
2.  右键单击同步服务器，然后单击“工作文件夹设置”。 此时将出现“工作文件夹设置”窗口。  
  
3.  在导航窗格中单击“支持电子邮件”，然后键入用户在通过电子邮件请求工作文件夹相关帮助时要使用的一个或多个电子邮件地址。 完成后单击**确定**。  
  
     工作文件夹用户可以在“工作文件夹控制面板”项中单击一个链接，将包含客户端电脑诊断信息的电子邮件发送到此处指定的地址。  
  
## <a name="step-9-optionally-set-up-server-automatic-discovery"></a>步骤 9：根据需要，设置服务器自动发现  
 如果你在环境中托管了多个同步服务器，则应该通过在 AD DS 中填充用户帐户的 **msDS-SyncServerURL** 属性，来配置服务器自动发现。  
  
>[!NOTE]
>不应为通过 Web 应用程序代理或 Azure AD 应用程序代理等反向代理解决方案访问工作文件夹的远程用户定义 Active Directory 中的 msDS-SyncServerURL 属性。 如果定义了 Msds-syncserverurl 属性，则工作文件夹客户端将尝试访问无法通过反向代理解决方案访问的内部 URL。 使用 Web 应用程序代理或 Azure AD 应用程序代理时，需要为每个工作文件夹服务器创建唯一的代理应用程序。 有关更多详细信息[，请参阅使用 AD FS 和 Web 应用程序代理部署工作文件夹：](deploy-work-folders-adfs-overview.md) [使用 Azure AD 应用程序代理概述或部署工作文件夹](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)。


 在执行此操作之前，必须先安装 Windows Server 2012 R2 域控制器，或使用 `Adprep /forestprep` 和 `Adprep /domainprep` 命令更新林和域的架构。 有关如何安全地运行这些命令的信息，请参阅 [运行 Adprep](https://technet.microsoft.com/library/dd464018.aspx)。  
  
 你也许还想要根据步骤 5 和步骤 6 中所述为文件服务器管理员创建安全组，并向他们委派修改此特定用户属性的权限。 如果不执行这些步骤，则你需要请求域管理员组或企业管理员组的成员为每个用户配置自动发现。  
  
#### <a name="to-specify-the-sync-server-for-users"></a>为用户指定同步服务器的步骤  
  
1.  在装有 Active Directory 管理工具的计算机上打开服务器管理器。  
  
2.  在“工具”菜单中单击“Active Directory 管理中心”。 此时将出现 Active Directory 管理中心。  
  
3.  导航到相应域中的“用户” 容器，右键单击想要分配到同步共享的用户，然后单击“属性”。  
  
4.  在导航窗格中单击“扩展”。  
  
5.  单击“属性编辑器” 选项卡，选择“msDS-SyncServerUrl” ，然后单击“编辑”。 此时将出现“多值字符串编辑器”对话框。  
  
6.  在“要添加的值”框中，键入要与此用户同步的同步服务器的 URL，然后依次单击“添加”、“确定”、“确定”。  
  
    > [!NOTE]
    >  同步服务器 URL 无非是 `https://` 或 `http://`（取决于你是否需要使用安全连接）后接同步服务器的完全限定域名。 例如， **https：\//sync1.contoso.com**。

若要填充多个用户的该属性，请使用 Active Directory PowerShell。 下面是填充步骤 5 中所述的 *HR Sync Share Users* 组的所有成员的该属性的示例。
  
```powershell  
$SyncServerURL = "https://sync1.contoso.com"  
$GroupName = "HR Sync Share Users"  
  
Get-ADGroupMember -Identity $GroupName |  
Set-ADUser –Add @{"msDS-SyncServerURL"=$SyncServerURL}  
  
```  
  
## <a name="step-10-optionally-configure-web-application-proxy-azure-ad-application-proxy-or-another-reverse-proxy"></a>步骤 10：（可选）配置 Web 应用程序代理、Azure AD 应用程序代理或其他反向代理  

若要使远程用户能够访问文件，需要通过反向代理发布工作文件夹服务器，以便在 Internet 上从外部使用工作文件夹。 可以使用 Web 应用程序代理、Azure Active Directory 应用程序代理或其他反向代理解决方案。  
  
若要使用 AD FS 和 Web 应用程序代理配置工作文件夹访问权限，请参阅[使用 AD FS 和 Web 应用程序代理 (WAP) 部署工作文件夹](deploy-work-folders-adfs-overview.md)。 有关 Web 应用程序代理的背景信息，请参阅 [Windows Server 2016 中的 Web 应用程序代理](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/web-application-proxy-windows-server)。 有关使用 Web 应用程序代理在 Internet 上发布应用程序（如工作文件夹）的详细信息，请参阅[使用 AD FS 预身份验证发布应用程序](https://technet.microsoft.com/windows-server-docs/identity/web-application-proxy/publishing-applications-using-ad-fs-preauthentication)。
 
若要使用 Azure Active Directory 应用程序代理配置工作文件夹访问权限，请参阅[使用 Azure Active Directory 应用程序代理启用对工作文件夹的远程访问](https://blogs.technet.microsoft.com/filecab/?p=7945) 
  
## <a name="step-11-optionally-use-group-policy-to-configure-domain-joined-pcs"></a>步骤 11：（可选）使用组策略配置已加入域的电脑  

如果你要向大量已加入域的电脑部署工作文件夹，可以使用组策略来完成以下客户端电脑配置任务：  
  
- 指定要与哪些同步服务器用户同步  
  
- 使用默认设置强制自动设置工作文件夹（在执行此操作之前，请回顾[设计工作文件夹实施方案](plan-work-folders.md)中有关组策略的内容）  
  
  若要控制这些设置，请为工作文件夹创建一个新的组策略对象 (GPO)，然后相应地配置以下组策略设置：  
  
- 在“用户配置”\“策略”\“管理模板”\“Windows 组件”\“工作文件夹”中配置“指定工作文件夹设置”策略设置  
  
- Computer Configuration\Policies\Administrative Templates\Windows Components\WorkFolders 中的“强制为所有用户自动设置”策略设置  
  
> [!NOTE]
>  仅当从运行组策略管理的 Windows 8.1、Windows Server 2012 R2 或更高版本的计算机编辑组策略时，这些策略设置才可用。 早期操作系统中的组策略管理版本未提供此项设置。 这些策略设置确实适用于安装了 [适用于 Windows 7 的工作文件夹](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) 应用的 Windows 7 电脑。  
  
##  <a name="BKMK_LINKS"></a>另请参阅  
 有关其他相关信息，请参阅以下资源。  
  
|内容类型|参考资料|  
|------------------|----------------|  
|**大致**|-   [工作文件夹](work-folders-overview.md)|  
|**规划**|-   [设计工作文件夹实现](plan-work-folders.md)|
|**部署**|-   [使用 AD FS 和 Web 应用程序代理（WAP）部署工作文件夹](deploy-work-folders-adfs-overview.md)<br />-   [工作文件夹测试实验室部署](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx)（博客文章）<br />-   [工作文件夹服务器 Url 的新用户属性](http://blogs.technet.com/b/filecab/archive/2013/10/09/a-new-user-attribute-for-work-folders-server-url.aspx)（博客文章）|  
|**技术参考**|-   [交互式登录：计算机帐户锁定阈值](https://technet.microsoft.com/library/jj966264(v=ws.11).aspx)<br />-   [同步共享 Cmdlet](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)|
