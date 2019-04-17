---
title: "使用 AD FS 和 Web 应用程序代理部署工作文件夹 - 步骤 2，AD FS 配置后工作"
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 4/5/2017
ms.assetid: 0a48852e-48cc-4047-ae58-99f11c273942
ms.openlocfilehash: 2a07f31e3040f63edfb8c73d454b6301aad46583
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-2-ad-fs-post-configuration-work"></a>使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 2，AD FS 后期配置工作

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍使用 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理部署工作文件夹的第二个步骤。 你可以在这些主题中找到此过程中的其他步骤：  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：概述](deploy-work-folders-adfs-overview.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 1，设置 AD FS](deploy-work-folders-adfs-step1.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 3，设置工作文件夹](deploy-work-folders-adfs-step3.md)  
  
-   [使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 4，设置 Web 应用程序代理](deploy-work-folders-adfs-step4.md)  
  
-   [
使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 5，设置客户端](deploy-work-folders-adfs-step5.md)  
  
> [!NOTE]
>   本节中包含的说明适用于 Windows Server 2016 环境。 如果你使用的是 Windows Server 2012 R2，请遵循 [Windows Server 2012 R2 说明](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。

在步骤 1 中，你安装并配置了 AD FS。 现在，你需要对 AD FS 执行以下配置后步骤。  
  
## <a name="configure-dns-entries"></a>配置 DNS 条目  
你必须为 AD FS 创建两个 DNS 条目。 这些条目就是你在创建使用者备用名称 (SAN) 证书时在预安装步骤中使用的两个条目。  
  
DNS 条目的格式如下：  
  
-   AD FS service name.domain  
  
-   enterpriseregistration.domain  
  
-   AD FS server name.domain   （DNS 条目应该已经存在。 例如，2016-ADFS.contoso.com）
  
在测试示例中，值为：  
  
-   **blueadfs.contoso.com**  
  
-   **enterpriseregistration.contoso.com**  
  
## <a name="create-the-a-and-cname-records-for-ad-fs"></a>为 AD FS 创建 A 和 CNAME 记录  
要为 AD FS 创建 A 和 CNAME 记录，请遵循下列步骤：  
  
1.  在域控制器上，打开 DNS 管理器。  
  
2.  展开“正向查找区域”文件夹，右键单击你的域，然后选择**新建主机 (A)**。  
  
3.  **新建主机**窗口随即打开。 在**名称**字段中，输入 AD FS 服务名称的别名。 在测验示例中，别名为 **blueadfs**。  
  
    别名必须与用于 AD FS 的证书中的主题相同。 例如，如果主题是 adfs.contoso.com，则此处输入的别名将为 **adfs**。  
  
    > [!IMPORTANT]  
    > 使用 Windows Server 用户界面 (UI) 而不是 Windows PowerShell 设置 AD FS 时，必须为 AD FS 创建 A 记录而不是 CNAME 记录。 原因是通过 UI 创建的服务主体名称 (SPN) 仅包含用于将 AD FS 服务设置为主机的别名。  
    >   
4.  在 **IP 地址**中，输入 AD FS 服务器上的 IP 地址。 在测试示例中，这是 **192.168.0.160**。 单击**添加主机**。  
  
5.  在“正向查找区域”文件夹中，再次右键单击你的域，然后选择**新别名 (CNAME)**。  
  
6.  在**新资源记录**窗口中添加别名 **enterpriseregistration** 并输入 AD FS 服务器的 FQDN。 此别名用于 Device Join，必须称为 **enterpriseregistration**。
  
7.  单击**确定**。  
  
要通过 Windows PowerShell 完成相同的步骤，请使用以下命令。 该命令必须在域控制器上执行。  
  
```powershell  
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name blueadfs -A -IPv4Address 192.168.0.160   
Add-DnsServerResourceRecord  -ZoneName "contoso.com" -Name enterpriseregistration -CName  -HostNameAlias 2016-ADFS.contoso.com   
```  
  
## <a name="set-up-the-ad-fs-relying-party-trust-for-work-folders"></a>为工作文件夹设置 AD FS 信赖方信任  
即使工作文件夹尚未设置，也可以设置和配置工作文件夹的信赖方信任。 必须设置信赖方信任，以使工作文件夹能够使用 AD FS。 因为你正在设置 AD FS，因此，现在是执行这个步骤的好时机。  
  
若要设置信赖方信任：  
  
1.  打开**服务器管理器**，在**工具**菜单上，选择 **AD FS 管理**。  
  
2.  在右侧窗格的**操作**下，单击**添加信赖方信任**。  
  
3.  在**欢迎**页上，选择**声明感知**，然后单击**开始**。  
  
4.  在**选择数据源**页上，选择**手动输入信赖方数据**，然后单击**下一步**。  
  
5.  在**显示名称**字段中，输入 **WorkFolders**，然后单击**下一步**。  
  
6.  在**配置证书**页上，单击**下一步**。 令牌加密证书是可选的，并且不需要测试配置。  
  
7.  在**配置 URL** 页上，单击**下一步**。  
  
8. 在**配置标识符**页上，添加以下标识符：**https://windows-server-work-folders/V1**。 此标识符是工作文件夹使用的硬编码值，并在与 AD FS 通信时由工作文件夹服务发送。 单击**下一步**。  
  
9. 在“选择访问控制策略”页上，选择**允许所有人**，然后单击**下一步**。  
  
10. 在**准备好添加信任**页上，单击**下一步**。  
  
11. 配置完成后，向导的最后一页会指示配置成功。 选中用于编辑声明规则的复选框，然后单击**关闭**。  
  
12. 在 AD FS 管理单元中，选择 **WorkFolders** 信赖方信任，然后单击“操作”下的**编辑声明颁发策略**。

13. **编辑 WorkFolders 的声明颁发策略**窗口随即打开。 单击**添加规则**。  
  
14. 在**声明规则模板**下拉列表中，选择**以声明方式发送 LDAP 特性**，然后单击**下一步**。  
  
15. 在**配置声明规则**页上，在**声明规则名称**字段中，输入 **WorkFolders**。  
  
16. 在**属性存储**下拉列表中，选择 **Active Directory**。  
  
17. 在映射表中，输入以下值：  
  
    -   用户主体名称：UPN  
  
    -   显示名称：名称  
  
    -   姓氏：姓氏  
  
    -   名字：名字  
  
18. 单击**完成**。 你将在“颁发转换规则”选项卡上看到 WorkFolders 规则，然后单击**确定**。  
  
### <a name="set-relying-part-trust-options"></a>设置信赖方信任选项  
在为 AD FS 设置信赖方信任之后，必须在 Windows PowerShell 中运行五个命令来完成配置。 这些命令设置工作文件夹与 AD FS 成功通信所需的选项，无法通过 UI 进行设置。 这些选项有：  
  
-   支持使用 JSON Web 令牌 (JWT)  
  
-   禁用加密声明  
  
-   启用自动更新  
  
-   为所有设备设置颁发 Oauth 刷新令牌。  

-   将客户端访问权限授予信赖方信任

若要设置这些选项，请使用以下命令：  
  
```powershell  
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -EnableJWT $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -Encryptclaims $false   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -AutoupdateEnabled $true   
Set-ADFSRelyingPartyTrust -TargetIdentifier "https://windows-server-work-folders/V1" -IssueOAuthRefreshTokensTo AllDevices
Grant-AdfsApplicationPermission -ServerRoleIdentifier "https://Windows-Server-Work-Folders/V1" -AllowAllRegisteredClients  
```  
  
## <a name="enable-workplace-join"></a>启用 Workplace Join  
启用 Workplace Join 是可选的，但是当你希望用户能够使用其个人设备访问工作区资源时，这可能十分有用。  
  
要为 Workplace Join 启用设备注册，必须运行以下 Windows PowerShell 命令，这些命令将配置设备注册并设置全局身份验证策略：  
  
```powershell  
Initialize-ADDeviceRegistration -ServiceAccountName <your AD FS service account>
    Example: Initialize-ADDeviceRegistration -ServiceAccountName contoso\adfsservice$
Set-ADFSGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true   
```  
  
## <a name="export-the-ad-fs-certificate"></a>导出 AD FS 证书  
接下来，导出自签名证书，以便它可以安装在测试环境中的以下机器上：  
  
-   用于工作文件夹的服务器  
  
-   用于 Web 应用程序代理的服务器  
  
-   加入域的 Windows 客户端  
  
-   未加入域的 Windows 客户端  
  
要导出证书，请按照下列步骤操作：  
  
1.  单击**开始**，然后单击**运行**。  
  
2.  键入 **MMC**。  
  
3.  在**文件**菜单上，单击**添加/删除管理单元**。  
  
4.  在**可用的管理单元**列表中，单击**证书**，然后单击**添加**。 证书管理单元向导启动。  
  
5.  选择**计算机帐户**，然后单击**下一步**。  
  
6.  选择**本地计算机：（运行此控制台的计算机）**，然后单击**完成**。  
  
7.  单击 **OK**。  
  
8.  展开文件夹**控制台根节点\证书\(本地计算机)\个人\证书**。  
  
9.  右键单击 **AD FS 证书**，单击**所有任务**，然后单击**导出...**。  
  
10. 此时将打开“证书导出向导”。 选择**是，导出私钥**。  
  
11. 在**导出文件格式**页上，选择默认选项，然后单击**下一步**。  
  
12. 为证书创建密码。 这是以后在将证书导入其他设备时使用的密码。 单击**下一步**。  
  
13. 输入证书的位置和名称，然后单击**完成**。  
  
将在后面的部署过程中介绍证书的安装。  
  
## <a name="manage-the-private-key-setting"></a>管理私钥设置  
你必须授予 AD FS 服务帐户权限才能访问新证书的私钥。 当更换已过期的通信证书时，你需要再次授予此权限。 若要授予权限，请遵循下列步骤：  
  
1.  单击**开始**，然后单击**运行**。  
  
2.  键入 **MMC**。  
  
3.  在**文件**菜单上，单击**添加/删除管理单元**。  
  
4.  在**可用的管理单元**列表中，单击**证书**，然后单击**添加**。 证书管理单元向导启动。  
  
5.  选择**计算机帐户**，然后单击**下一步**。  
  
6.  选择**本地计算机：（运行此控制台的计算机）**，然后单击**完成**。  
  
7.  单击 **OK**。  
  
8.  展开文件夹**控制台根节点\证书\(本地计算机)\个人\证书**。  
  
9.  右键单击 **AD FS 证书**，单击**所有任务**，然后单击**管理私钥**。  
  
10. 在**权限**窗口中，单击**添加**。  
  
11. 在**对象类型**上，选择**服务帐户**，然后单击**确定**。  
  
12. 键入运行 AD FS 的帐户的名称。 在测验示例中，名称为 ADFSService。 单击**确定**。  
  
13. 在**权限**窗口中，至少将读取权限给予帐户，然后单击**确定**。  
  
如果没有管理私钥的选项，则可能需要运行以下命令： `certutil -repairstore my *`  
  
## <a name="verify-that-ad-fs-is-operational"></a>验证 AD FS 是否可运行  
要验证 AD FS 是否可运行，请打开浏览器窗口，然后转到 https://blueadfs.contoso.com/federationmetadata/2007-06/federationmetadata.xml 
  
浏览器窗口将显示联合服务器元数据，而不进行任何格式化。 如果你可以看到数据没有任何 SSL 错误或警告，则你的联合服务器可以运行。  
  
下一步：[使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤 3，设置工作文件夹](deploy-work-folders-adfs-step3.md)  
  
## <a name="see-also"></a>另请参阅  
[工作文件夹概述](Work-Folders-Overview.md)  
  

