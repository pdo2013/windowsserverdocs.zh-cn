---
title: 使用 AD FS 和 Web 应用程序代理部署工作文件夹 - 步骤 1，设置 AD FS
ms.prod: windows-server
ms.technology: storage-work-folders
ms.topic: article
manager: klaasl
ms.author: jeffpatt
author: JeffPatt24
ms.date: 10/18/2018
ms.assetid: 938cdda2-f17e-4964-9218-f5868fd96735
ms.openlocfilehash: 0920d091d6e8b5f3db9bf945a966fdd577918179
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365795"
---
# <a name="deploy-work-folders-with-ad-fs-and-web-application-proxy-step-1-set-up-ad-fs"></a>使用 AD FS 和 Web 应用程序代理部署工作文件夹：步骤1、设置 AD FS

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍了使用 Active Directory 联合身份验证服务 (AD FS) 和 Web 应用程序代理部署工作文件夹的第一步。 你可以在这些主题中查找这一过程的其他步骤：  
  
-   使用 AD FS 和 Web 应用程序代理 @no__t 0Deploy 工作文件夹：叙述](deploy-work-folders-adfs-overview.md)  
  
-   使用 AD FS 和 Web 应用程序代理 @no__t 0Deploy 工作文件夹：步骤 2 AD FS 配置后工作 @ no__t-0  
  
-   使用 AD FS 和 Web 应用程序代理 @no__t 0Deploy 工作文件夹：步骤3，设置工作文件夹 @ no__t-0  
  
-   使用 AD FS 和 Web 应用程序代理 @no__t 0Deploy 工作文件夹：步骤4，设置 Web 应用程序代理 @ no__t-0  
  
-   使用 AD FS 和 Web 应用程序代理 @no__t 0Deploy 工作文件夹：步骤5，设置客户端 @ no__t-0  
  
> [!NOTE]
>   本部分中所述的说明适用于 Windows Server 2019 或 Windows Server 2016 环境。 如果你使用的是 Windows Server 2012 R2，请遵循 [Windows Server 2012 R2 说明](https://technet.microsoft.com/library/dn747208(v=ws.11).aspx)。

若要设置 AD FS 以用于工作文件夹，请使用以下过程。  
  
## <a name="pre-installment-work"></a>预安装工作  
如果要将使用这些说明设置的测试环境转换为生产环境，你可能希望在开始之前完成以下两处操作：  
  
-   设置 Active Directory 域管理员帐户以用于运行 AD FS 服务。
  
-   获取用于服务器验证的安全套接字层 (SSL) 使用者可选名称 (SAN) 证书。 对于测试示例，你将使用自签名的证书，但对于生产，应使用公共可信证书。  
  
获得这些项目可能需要一些时间，具体取决于公司策略，因此在开始创建测试环境前开始项目请求过程是非常有用的。  
  
你可以从多家商业证书颁发机构 (CA) 购买此证书。 你可以在[知识库文章 931125](https://support.microsoft.com/kb/931125) 中找到 Microsoft 信任的 CA 列表。 另一个方法是从公司的企业 CA 获取证书。  
  
对于测试环境，你将使用由提供的脚本之一创建的自签名证书。  
  
> [!NOTE]  
> AD FS 不支持下一代加密技术 (CNG) 证书，这意味着你无法通过使用 Windows PowerShell cmdlet New-SelfSignedCertificate 创建自签名证书。 但是，你可以使用[使用 AD FS 和 Web 应用程序代理部署工作文件夹](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap)博客文章中的 makecert.ps1 脚本。 此脚本会创建适用于 AD FS 的自签名证书并提示创建证书所需的 SAN 名称。  
  
接下来，请执行以下部分中所述的预安装工作。  
  
### <a name="create-an-ad-fs-self-signed-certificate"></a>创建 AD FS 自签名证书  
若要创建 AD FS 自签名证书，请执行以下步骤：  
  
1.  下载[使用 AD FS 和 Web 应用程序代理部署工作文件夹](https://blogs.technet.microsoft.com/filecab/2014/03/03/deploying-work-folders-with-ad-fs-and-web-application-proxy-wap)博客文章中提供的脚本，然后将文件 makecert.ps1 复制到 AD FS 计算机。  
  
2.  使用管理员权限打开 Windows PowerShell 窗口。  
  
3.  将执行策略设置为无限制：  
  
    ```powershell  
    Set-ExecutionPolicy –ExecutionPolicy Unrestricted   
    ```  
  
4.  更改为从中复制脚本的目录。  
  
5.  执行 makecert 脚本：  
  
    ```powershell  
    .\makecert.ps1  
    ```  
  
6.  当提示更改主题证书时，请为主题输入新值。 在此示例中，值为 **blueadfs.contoso.com**。  
  
7.  当提示输入 SAN 名称时，按 Y，然后一次输入一个 SAN 名称。  
  
    对于此示例，键入 **blueadfs.contoso.com**，按 Enter，然后键入 **2016-adfs.contoso.com**，再按 Enter，最后键入 **enterpriseregistration.contoso.com** 并按 Enter。  
  
    当所有 SAN 名称输入完毕后，请在空行上按 Enter。  
  
8.  当提示将证书安装至受信任的根证书机构存储时，按 Y。  
  
AD FS 证书必须为使用下列值的 SAN 证书：  

-   AD FS service name.domain

-   enterpriseregistration.domain

-   AD FS server name.domain

在测试示例中，这些值为：  
  
-   **blueadfs.contoso.com**
  
-   **enterpriseregistration.contoso.com**
  
-   **2016-adfs.contoso.com**
  
Workplace Join 需要 enterpriseregistration SAN。  
  
### <a name="set-the-server-ip-address"></a>设置服务器的 IP 地址  
将服务器的 IP 地址更改为静态 IP 地址。 对于测试示例，请使用 IP 类 A，它是 192.168.0.160/子网掩码：255.255.0.0/默认网关：192.168.0.1/首选 DNS：192.168.0.150 （域控制器 @ no__t 的 IP 地址。  
  
## <a name="install-the-ad-fs-role-service"></a>安装 AD FS 角色服务  
按照下列步骤安装 AD FS：  
  
1.  登录到计划安装 AD FS 的物理或者虚拟机，打开**服务器管理器**，并启动“添加角色和功能向导”。  
  
2.  在**服务器角色**页面上，选择 **Active Directory 联合身份验证服务**角色，然后单击**下一步**。  
  
3.  在 **Active Directory 联合身份验证服务 (AD FS)** 页面上，你将看到一条消息，这条消息指出无法在同一台计算机上将 Web 应用程序代理角色作为 AD FS 安装。 单击“下一步”。  
  
4.  在“确认”页上，单击**安装**。  
  
若要通过 Windows PowerShell 完成 AD FS 的等效安装，请使用以下命令：  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation –IncludeManagementTools  
```  
  
## <a name="configure-ad-fs"></a>配置 AD FS  
接下来，通过使用服务器管理器或 Windows PowerShell 配置 AD FS。  
  
### <a name="configure-ad-fs-by-using-server-manager"></a>使用服务器管理器配置 AD FS  
若要使用服务器管理器配置 AD FS，请执行以下步骤：  
  
1.  打开服务器管理器。  
  
2.  在服务器管理器窗口顶部，单击**通知**标志，然后单击**在此服务器上配置联合身份验证服务**。  
  
3.  启动“Active Directory 联合身份验证服务配置向导”。 在**连接到 AD DS** 页面上，输入你想要用作 AD FS 帐户的域管理员帐户，然后单击**下一步**。  
  
4.  在**指定服务属性**页面上，输入用于 AD FS 通信的 SSL 证书使用者名称。 在测试示例中，此为 **blueadfs.contoso.com**。  
  
5.  输入联合身份验证服务名称。 在测试示例中，此为 **blueadfs.contoso.com**。 单击“下一步”。  
  
    > [!NOTE]  
    > 联合身份验证服务名称不得使用环境中现有服务器的名称。 一旦使用现有服务器的名称，AD FS 安装将失败，并且必须重启。  
  
6.  在**指定服务帐户**页面上，输入你想要用于托管服务帐户的名称。 对于测试示例，请选择**创建组托管服务帐户**，并在**帐户名称**中输入 **ADFSService**。 单击“下一步”。  
  
7.  在**指定配置数据库**页面上，选择**使用 Windows 内部数据库在此服务器上创建数据库**，然后单击**下一步**。  
  
8.  **查看选项**页面会显示所选选项的概述。 单击“下一步”。  
  
9. **先决条件检查**页面会指示所有先决条件是否都成功通过检查。 如果没有任何问题，请单击**配置**。  
  
    > [!NOTE]  
    > 如果你使用 AD FS 服务器或其他任何现有计算机的名称作为联合身份验证服务名称，则将显示一条错误消息。 你必须重新安装，并选择除现有计算机名称以外的名称。  
  
10. 配置成功完成后，**结果**页面确认 AD FS 已配置成功。  
  
### <a name="configure-ad-fs-by-using-powershell"></a>使用 PowerShell 配置 AD FS  
若要通过 Windows PowerShell 完成 AD FS 的等效配置，请使用以下命令。  
  
若要安装 AD FS：  
  
```powershell  
Add-WindowsFeature RSAT-AD-Tools  
Add-WindowsFeature ADFS-Federation -IncludeManagementTools   
```  
  
若要创建托管服务帐户：  
  
```powershell  
New-ADServiceAccount "ADFSService"-Server 2016-DC.contoso.com -Path "CN=Managed Service Accounts,DC=Contoso,DC=COM" -DNSHostName 2016-ADFS.contoso.com -ServicePrincipalNames HTTP/2016-ADFS,HTTP/2016-ADFS.contoso.com  
```  
  
配置 AD FS 后，你必须使用在上一步中创建的托管服务帐户和在预配置步骤中创建的证书设置 AD FS 场。  
  
若要设置 AD FS 场：  
  
```powershell  
$cert = Get-ChildItem CERT:\LocalMachine\My |where {$_.Subject -match blueadfs.contoso.com} | sort $_.NotAfter -Descending | select -first 1    
$thumbprint = $cert.Thumbprint  
Install-ADFSFarm -CertificateThumbprint $thumbprint -FederationServiceDisplayName "Contoso Corporation" –FederationServiceName blueadfs.contoso.com -GroupServiceAccountIdentifier contoso\ADFSService$ -OverwriteConfiguration -ErrorAction Stop  
```  
  
下一步：使用 AD FS 和 Web 应用程序代理 @no__t 0Deploy 工作文件夹：步骤 2 AD FS 配置后工作 @ no__t-0  
  
## <a name="see-also"></a>请参阅  
[工作文件夹概述](Work-Folders-Overview.md)  
  

