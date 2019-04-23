---
title: 准备将 AD FS 2.0 联合服务器迁移到 Windows Server 2012 R2 上的 AD FS
description: 提供有关准备迁移到 Windows Server 2012 R2 AD FS 服务器信息。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4d0ff53b9118db1dd6ba5af94b3e627bf1597e0c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889018"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>准备将 AD FS 2.0 联合服务器迁移到 Windows Server 2012 R2 上的 AD FS

本文档介绍如何将 AD FS 2.0 或 Windows Server 2012 的联合服务器场迁移到 Windows Server 2012 R2 AD FS 场。  AD FS 场使用 WID 或 SQL Server 作为基础数据库，可以使用的步骤。  
  
-   [迁移过程概述](prepare-migrate-ad-fs-server-r2.md#migrate-process-outline)  
  
-   [Windows Server 2012 R2 中的新 AD FS 功能](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Windows Server 2012 R2 中 AD FS 要求](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [提高 Windows PowerShell 限制](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [其他迁移任务和注意事项](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>迁移过程概述  
 若要完成将 AD FS 联合服务器场迁移到 Windows Server 2012 R2 的过程，必须完成以下任务：  
  
1.  导出、记录并备份现有 AD FS 场中的以下配置数据。 有关如何完成这些任务的详细说明，请参阅[迁移 AD FS 联合服务器](migrate-ad-fs-fed-server-r2.md)。  
  
在 Windows Server 2012 R2 安装 cd 的 \support\adfs 文件夹中的脚本迁移以下设置：  
  
-   声明提供程序信任，针对 Active Directory 声明提供程序信任的自定义声明规则除外。 有关详细信息，请参阅[迁移 AD FS 联合服务器](migrate-ad-fs-fed-server-r2.md)。  
  
-   信赖方信任。  
  
-   AD FS 内部生成的自签名令牌签名和令牌解密证书。  
  
必须手动迁移以下所有自定义设置：  
  
 -   服务设置：  
  
     -   企业或公共证书颁发机构颁发的非默认令牌签名和令牌解密证书。  
  
     -   AD FS 使用的 SSL 服务器身份验证证书。  
  
     -   AD FS 使用的服务通信证书（默认情况下，此证书与 SSL 证书相同）。  
  
      -   任何联合身份验证服务属性的非默认值，例如 AutoCertificateRollover 或 SSO 生存期。  
  
      -   非默认 AD FS 终结点设置和声明说明。  
  
-   针对 Active Directory 声明提供程序信任的自定义声明规则。  
  
    -   AD FS 登录页自定义  
  
有关详细信息，请参阅[迁移 AD FS 联合服务器](migrate-ad-fs-fed-server-r2.md)。  
  
2.  创建 Windows Server 2012 R2 联合服务器场。  
  
3.  将原始配置数据导入这个新的 Windows Server 2012 R2 AD FS 场。  
  
4.  配置并自定义 AD FS 登录页。  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中新增的 AD FS 功能  
 以下 AD FS 功能更改在 Windows Server 2012 R2 影响迁移从 AD FS 2.0 或 Windows Server 2012 中的 AD FS:  
  
**IIS 依赖关系**  
   - Windows Server 2012 R2 中的 AD FS 是自我托管的，不需要安装 IIS。 由于做出了这项更改，请确保注意以下事项：  
   -   现在，必须通过 Windows PowerShell 对 AD FS 场中的联合服务器和代理计算机执行 SSL 证书管理。  
  
**对 AD FS 登录页的设置和自定义项的更改**  
-   在 Windows Server 2012 R2 中 AD FS 中，有几处更改旨在改进管理员和用户的登录体验。 上一版本的 AD FS 中存在的 IIS 托管网页现在已被删除。 AD FS 登录网页的外观自我托管在 AD FS 中，现在可以对它进行自定义以符合用户体验。 更改包括：  
    -   自定义 AD FS 登录体验，包括自定义公司名称、徽标、插图和登录说明。  
    -   自定义错误消息。  
    -   自定义 ADFS 主领域发现体验，包括：  
        -   将标识提供程序配置为使用特定的电子邮件后缀。  
        -   按照信赖方配置标识提供程序列表。  
        -   对于 Intranet 绕过主领域发现。  
        -   创建自定义 Web 主题。  
  
有关配置 AD FS 登录页外观的详细说明，请参阅 [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md)。  
  
如果您想要迁移到 Windows Server 2012 R2 的现有 AD FS 场中有自定义 web 页，可以重新创建它们在 Windows Server 2012 R2 中使用新的自定义功能的迁移过程的一部分。  
  
-   **其他更改**  
  
    -   Windows Server 2012 R2 中的 AD FS 基于在 Windows Identity Foundation (WIF) 3.5 中，不是 WIF 4.5。 因此，WIF 4.5 （例如，Kerberos 声明和动态访问控制） 的某些特定功能不支持在 Windows Server 2012 R2 中的 AD FS 中。  
  
    -   端口 443; 上运行 Windows Server 2012 R2 中的设备注册服务 (DRS)用户证书身份验证的 clienttls 在端口 49443 上  
  
        -   对于使用证书传输模式身份验证的、已专门硬编码为指向端口 443 的主动非浏览器客户端，需要先更改代码，然后才能在端口 49443 上继续使用用户证书身份验证。  
  
        -   对于被动应用程序，则无需做出任何更改，因为 AD FS 将会针对用户证书身份验证重定向到正确的端口。  
  
        -   客户端与代理之间的防火墙端口必须允许端口 49443 的流量通过，以便进行用户证书身份验证。  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中的 AD FS 要求  
 为了成功将 AD FS 场迁移到 Windows Server 2012 R2，必须满足以下要求：  
  
 AD fs 正常，你想要将联合身份验证的每台计算机必须加入到域。  
  
 对于 AD FS Windows Server 2012 R2 上运行到函数中，你的 Active Directory 域必须运行以下任一：  
  
-   Windows Server 2012 R2  
  
-   Windows Server 2012  
  
-   Windows Server 2008 R2  
  
-   Windows Server 2008  
  
 如果打算使用组托管服务帐户 (gMSA) 作为服务帐户为 AD FS，你必须至少一个域控制器环境中的 Windows Server 2012 或 Windows Server 2012 R2 操作系统上运行。  
  
 如果你打算针对 AD 工作区加入 AD FS 部署的一部分部署设备注册服务 (DRS)，将 AD DS 架构必须更新到 Windows Server 2012 R2 级别。 可通过三种方法更新该架构：  
  
1.  在现有 Active Directory 林中，任何运行 Windows Server 2008 或更高版本的 64 位服务器上运行 Windows Server 2012 R2 操作系统 DVD 的 \support\adprep 文件夹中的 adprep /forestprep。 在此情况下，无需安装其他域控制器，并且无需升级现有的域控制器。  
  
若要运行 adprep/forestprep，必须是托管架构主机的域的 Schema Admins 组、Enterprise Admins 组和 Domain Admins 组的成员。  
  
2.  在现有 Active Directory 林中，安装运行 Windows Server 2012 R2 的域控制器。 在此情况下，安装域控制器的过程中会自动运行 adprep /forestprep。  
  
在安装域控制器期间，你可能需要指定其他凭据以运行 adprep /forestprep。  
  
3.  通过在运行 Windows Server 2012 R2 的服务器上安装 AD DS 中创建新的 Active Directory 林。 在此情况下，不需要运行 adprep /forestprep，因为最初就会创建包含所有必需容器和对象的架构以支持 DRS。  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>对 Windows Server 2012 R2 中 AD FS 的 SQL Server 支持  
 如果需要创建 AD FS 场并使用 SQL Server 来存储配置数据，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。  
  
##  <a name="increasing-your-windows-powershell-limits"></a>提高 Windows PowerShell 限制  
 如果 AD FS 场中具有 1000 个以上的声明提供程序信任和信赖方信任，或者如果你在尝试运行 AD FS 迁移导出/导入工具时看到以下错误，则必须提高 Windows PowerShell 限制：  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 之所以引发此错误，是因为 Windows PowerShell 会话默认内存限制太低。 在 Windows PowerShell 2.0 中，会话默认内存为 150MB。 在 Windows PowerShell 3.0 中，会话默认内存为 1024MB。 可以使用以下命令来验证 Windows PowerShell 远程会话内存限制： `Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`。 可以通过运行以下命令来提高限制：`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`。  
  
## <a name="other-migration-tasks-and-considerations"></a>其他迁移任务和注意事项  
 若要成功地将 AD FS 场迁移到 Windows Server 2012 R2，请确保了解以下要点：  
  
-   在 Windows Server 2012 R2 安装 cd 的 \support\adfs 文件夹中的迁移脚本要求保留的相同的联合身份验证服务器场名称并将其迁移到 Windows 时，在旧版 AD FS 场中使用的服务帐户标识名称Server 2012 R2。  
  
-   如果需要迁移 SQL Server AD FS 场，请注意，迁移过程包括创建一个新的 SQL 数据库实例，到时必须将原始配置数据导入到该实例。  
  
## <a name="next-steps"></a>后续步骤
 [将 Active Directory 联合身份验证服务角色服务迁移到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [迁移 AD FS 联合身份验证服务器](migrate-ad-fs-fed-server-r2.md)   
 [迁移 AD FS 联合服务器代理](migrate-fed-server-proxy-r2.md)   
 [验证 AD FS 迁移到 Windows Server 2012 R2](verify-ad-fs-migration.md)