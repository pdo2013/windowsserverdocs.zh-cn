---
title: "准备迁移到在 Windows Server 2012 R2 上广告 FS 广告 FS 2.0 联盟服务器"
description: "在准备好将广告 FS server 迁移到 Windows Server 2012 R2 上提供的信息。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4d0ff53b9118db1dd6ba5af94b3e627bf1597e0c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/12/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>准备迁移到在 Windows Server 2012 R2 上广告 FS 广告 FS 2.0 联盟服务器

本文介绍了如何将广告 FS 2.0 或 Windows Server 2012 联合身份验证的服务器场迁移到了 Windows Server 2012 R2 广告 FS 场。  可以与用作基础数据库 WID 或 SQL Server 的广告 FS 场结合使用的步骤。  
  
-   [迁移进程轮廓](prepare-migrate-ad-fs-server-r2.md#migrate-process-outline)  
  
-   [在 Windows Server 2012 R2 的新广告 FS 功能](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [在 Windows Server 2012 R2 的广告 FS 要求](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [提高 Windows PowerShell 限制](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [其他迁移任务和注意事项](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>迁移进程轮廓  
 若要完成迁移的广告 FS 联盟服务器场对 Windows Server 2012 R2，必须完成以下任务：  
  
1.  导出，请记录，并备份你的现有广告 FS 场中以下配置数据。 有关详细说明了如何才能完成这些任务中，请参阅[迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)。  
  
在 Windows Server 2012 R2 安装光盘 \support\adfs 文件夹这些脚本与迁移以下设置：  
  
-   索赔提供商信任，自定义声明规则上 Active Directory 索赔提供商信任除外。 有关详细信息，请参阅[迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)。  
  
-   依赖方信任。  
  
-   广告 FS 内部生成、自签名令牌签名和令牌解密证书。  
  
必须手动迁移以下任一自定义设置：  
  
 -   服务设置：  
  
     -   非默认令牌签名和令牌解密证书已通过企业或公共证书颁发机构颁发。  
  
     -   由广告 FS SSL 服务器的身份验证证书。  
  
     -   广告 FS 使用的服务通信证书（默认情况下，是同一个证书作为 SSL 证书。  
  
      -   对于任何联合身份验证服务属性，如 AutoCertificateRollover 或 SSO 生存期非默认值。  
  
      -   非默认广告 FS 端点设置和索赔说明。  
  
-   自定义声称上 Active Directory 索赔提供商信任规则。  
  
    -   广告 FS 登录页面自定义设置  
  
有关详细信息，请参阅[迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)。  
  
2.  创建 Windows Server 2012 R2 联合身份验证的服务器场。  
  
3.  导入此新的 Windows Server 2012 R2 广告 FS 农场里的原始配置数据。  
  
4.  将配置并自定义广告 FS 登录页面。  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 的新广告 FS 功能  
 以下广告 FS 功能更改在 Windows Server 2012 R2 影响迁移从 AD 未 2.0 广告 FS 在 Windows Server 2012:  
  
**IIS 相关性**  
   - 在 Windows Server 2012 R2 的广告金融服务自托管，和不需要 IIS 安装。 请确保你注意以下进行此更改后：  
   -   通过 Windows PowerShell 必须现在执行 SSL 联合身份验证的服务器和您的广告 FS 电场的日落中的代理计算机的证书管理。  
  
**更改与广告 FS 登录页面的设置和自定义**  
-   在 Windows Server 2012 R2 的广告 FS，有几个用于改善登录体验管理员和用户的更改。 现在已删除了广告 FS 以前版本中存在 IIS 托管网页。 自行位于广告 FS 的广告 FS 登录网页外观和可以现在自定义定制用户体验。 更改包括：  
    -   自定义广告 FS 登录体验，其中包括自定义的公司名称、徽标、图中，并登录描述。  
    -   自定义的错误消息。  
    -   自定义 ADFS 家庭领域发现体验，其中包括：  
        -   配置使用某些电子邮件后缀你的身份提供商。  
        -   配置依赖每身份提供商列表方。  
        -   绕过家庭领域发现的 intranet。  
        -   创建自定义 web 主题。  
  
配置广告 FS 登录页面的外观的详细说明，请参阅[自定义广告 FS 登录页面](../operations/AD-FS-Customization-in-Windows-Server-2016.md)。  
  
如果你有网页自定义你想要迁移到 Windows Server 2012 R2 的现有广告 FS 场中时，你可以重新创建作为 Windows Server 2012 R2 在使用新的自定义项功能迁移过程的一部分。  
  
-   **其他更改**  
  
    -   广告 FS 在 Windows Server 2012 R2 基于对 Windows 身份基础 (WIF) 3.5，不 WIF 4.5。 因此，在 Windows Server 2012 R2 的广告 FS 中不支持（例如，Kerberos 索赔和动态访问控制）WIF 4.5 某些特定功能。  
  
    -   端口 443; 上运行 Windows Server 2012 R2 中的设备注册服务 (DRS)端口 49443 上运行 ClientTLS 用户证书身份验证  
  
        -   活动、非浏览器的客户端使用证书传输模式身份验证的专为难编码指向端口 443 代码更改才能继续使用端口 49443 用户证书身份验证。  
  
        -   无源应用程序无需进行更改因为广告 FS 重定向到正确的用户证书身份验证的端口。  
  
        -   客户端和代理服务器之间防火墙端口必须启用端口 49443 通信通过用户证书身份验证。  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 的广告 FS 要求  
 为了成功为 Windows Server 2012 R2 迁移你广告 FS 电场的日落，必须满足以下要求：  
  
 有关广告 FS，必须到某个域加入你想要将联盟每台计算机。  
  
 为广告 FS 为功能运行 Windows Server 2012 R2 上，您的域的 Active Directory 必须运行以下任一操作：  
  
-   Windows Server 2012R2  
  
-   Windows Server 2012  
  
-   Windows Server 2008 R2  
  
-   Windows Server 2008  
  
 如果你打算作为 service 帐户一组托管服务帐户 (gMSA) 用于广告 FS，你必须至少一个域控制器在 Windows Server 2012 或 Windows Server 2012 R2 的操作系统运行的你环境中。  
  
 如果你打算作为广告 FS 部署的一部分，用于广告工作区加入部署设备注册服务 (DRS)，广告 DS 方案将需要更新到 Windows Server 2012 R2 级别。 有三种方法更新的模式：  
  
1.  现有的 Active Directory 树林中运行 adprep /forestprep \support\adprep 文件夹中的 Windows Server 2012 R2 操作系统 DVD 运行 Windows Server 2008 或更高版本的任何 64 位服务器上。 在此情况下，没有其他的域控制器需要安装，并且没有现有域控制器需要升级。  
  
若要运行 adprep/forestprep，你必须的成员架构管理员组、企业版管理员组中，并且域管理员组承载方案主机的域。  
  
2.  现有的 Active Directory 树林中安装运行 Windows Server 2012 R2 域控制器。 在此情况下，adprep /forestprep 域控制器安装过程中会自动运行。  
  
域控制器安装期间，你可能需要才能运行 adprep 指定其他凭据 /forestprep。  
  
3.  通过运行 Windows Server 2012 R2 的服务器上安装广告 DS 创建新的 Active Directory 森林。 在此情况下，adprep /forestprep 副本不一定要运行，因为所有必要容器和支持 DRS 到的对象最初创建方案。  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 的广告 fs SQL Server 支持  
 如果你想要创建广告 FS 电场的日落，使用 SQL Server 存储配置数据，您可以使用 SQL Server 2008 和较新版本，包括 SQL Server 2012。  
  
##  <a name="increasing-your-windows-powershell-limits"></a>提高 Windows PowerShell 限制  
 如果你有多 1000 年声明提供商的信任和依赖方信任中广告 FS 电场的日落，或如果你在尝试运行广告 FS 迁移导出月导入工具时看到下列错误，你必须提高你的 Windows PowerShell 限制：  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 由于 Windows PowerShell 会话限制默认内存太低，将出现此错误。 在 Windows PowerShell 2.0、会话默认内存是 150 MB。 在 Windows PowerShell 3.0，会话默认内存是 1024 MB。 你可以采用验证 Windows PowerShell 远程会话内存限制使用以下命令：`Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`。 你可以通过运行以下命令增加限制：`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`。  
  
## <a name="other-migration-tasks-and-considerations"></a>其他迁移任务和注意事项  
 成功为 Windows Server 2012 R2 迁移你广告 FS 电场的日落，以确保你会注意以下：  
  
-   在 Windows Server 2012 R2 安装光盘 \support\adfs 文件夹迁移脚本要求，保持同一联盟服务器电场的日落名称和将其迁移到 Windows Server 2012 R2 时，在你的旧版广告 FS 场中使用的服务的帐户的身份名称。  
  
-   如果你想要迁移 SQL Server 广告 FS 电场的日落，请记下迁移过程需要创建一个新的 SQL 数据库实例，必须向其中导入的原始配置数据。  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [迁移广告 FS 联盟服务器](migrate-ad-fs-fed-server-r2.md)   
 [迁移广告 FS 联合身份验证的服务器代理](migrate-fed-server-proxy-r2.md)   
 [验证广告 FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)