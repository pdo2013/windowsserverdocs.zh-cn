---
title: 升级到与 SQL Server 的 Windows Server 2016 中的 AD FS
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 0a3db2a095d1a31f55bd1c8bfc5bf3c9f6bb65b8
ms.sourcegitcommit: ccc802338b163abdad2e53b55f39addcfea04603
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/05/2019
ms.locfileid: "66687394"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>升级到与 SQL Server 的 Windows Server 2016 中的 AD FS


> [!NOTE]  
> 仅在明确的时间段计划完成的与开始升级。 建议不要继续 AD FS 处于混合的模式状态的时间，长时间处于混合的模式状态的保留 AD FS 与场可能引起问题。


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>从 Windows Server 2012 R2 AD FS 场迁移到 Windows Server 2016 AD FS 场  
以下文档将描述如何将你的 AD FS Windows Server 2012 R2 场升级到 Windows Server 2016 中的 AD FS，为 AD FS 数据库使用 SQL Server 时。  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>升级到 Windows Server 2016 FBL 的 AD FS  
在 AD FS 的 Windows Server 2016 中新增的场行为级别功能 (FBL)。   此功能是整个服务器场，并确定可以使用 AD FS 场的功能。   默认情况下，Windows Server 2012 R2 AD FS 场中的 FBL 是在 Windows Server 2012 R2 FBL。  

Windows Server 2016 AD FS 服务器可以添加到 Windows Server 2012 R2 场，它将运行在 Windows Server 2012 R2 作为相同 FBL。  在这种方式运行的 Windows Server 2016 AD FS 服务器后，您的服务器场称为"mixed"。  但是，您将不能充分利用新的 Windows Server 2016 功能，直到 FBL 引发到 Windows Server 2016。  与混合的服务器场：  

-   管理员可以添加新的 Windows Server 2016 的现有 Windows Server 2012 R2 场的联合身份验证服务器。  因此，场处于"混合模式"，并运行 Windows Server 2012 R2 场行为级别。  若要确保整个场中一致的行为，不能配置 Windows Server 2016 的新增功能或将其在此模式下使用。  

-   已从混合的模式场中删除所有 Windows Server 2012 R2 联合身份验证服务器并在 WID 场的情况下的一个新的 Windows Serve 2016 联合身份验证服务器已升级到主节点的角色后，管理员然后可以提升从 Win FBL予以 Server 2012 R2 到 Windows Server 2016。  因此，任何新的 AD FS Windows Server 2016 功能可配置和使用。  

-   因此混合的场功能，AD FS Windows Server 2012 R2 升级到 Windows Server 2016 的组织将无需部署的整个新场的导出和导入配置数据。  相反，它们可以将 Windows Server 2016 节点添加到现有场，它处于联机状态时，仅会产生参与 FBL 引发的相对较短的停机时间。  

请注意，在混合的场模式下，AD FS 场不支持的任何新功能或 Windows Server 2016 中的 AD FS 中引入的功能。  这意味着想要试用新功能的组织不能执行此操作之前引发 FBL。  因此如果你的组织希望在测试之前祝愿 FBL 的新功能，您需要部署一个单独的场来执行此操作。  

其余部分是文档提供有关将 Windows Server 2016 联合身份验证服务器添加到 Windows Server 2012 R2 环境，然后提升到 Windows Server 2016 FBL 步骤。  在下面的体系结构关系图中所述的测试环境中执行这些步骤。  

> [!NOTE]  
> 可以将它们移动到 Windows Server 2016 FBL 中的 AD FS 之前，必须删除所有 Windows 2012 R2 节点。  只需不能升级到 Windows Server 2016 的 Windows Server 2012 R2 操作系统，并使其成为 2016年节点。  你将需要删除它，然后将其替换为新的 2016年节点。  

为以下体系结构关系图显示了用于验证和记录下面的步骤设置。

![体系结构](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>将 Windows 2016 AD FS 服务器联接到 AD FS 场

1.  Windows Server 2016 上使用服务器管理器安装 Active Directory 联合身份验证服务角色  

2.  使用 AD FS 配置向导，将新的 Windows Server 2016 服务器加入到现有的 AD FS 场。  上**欢迎**屏幕单击**下一步**。
 ![加入场](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  上**连接到 Active Directory 域服务**屏幕，s**指定的管理员帐户**有权执行联合身份验证服务配置，并单击**下一步**.
4.  上**指定场**屏幕上，输入 SQL 服务器和实例的名称，然后单击**下一步**。
![加入场](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  上**指定 SSL 证书**屏幕上，指定的证书，单击**下一步**。
![加入场](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  上**指定服务帐户**屏幕上，指定服务帐户，然后单击**下一步**。
7.  上**查看选项**屏幕上，查看选项并单击**下一步**。
8.  上**系统必备组件检查**屏幕上，确保所有先决条件检查已通过，单击**配置**。
9.  上**结果**屏幕上，确保已成功配置该服务器，然后单击**关闭**。


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>删除 Windows Server 2012 R2 AD FS 服务器

>[!NOTE]
>不需要设置主 AD FS 服务器使用集 AdfsSyncProperties-角色时，使用 SQL 作为数据库。  这是因为所有节点被视为主要在这种配置。

1.  在服务器管理器使用 Windows Server 2012 R2 AD FS 服务器上**删除角色和功能**下**管理**。
![删除服务器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  在“开始之前”  屏幕上，单击“下一步”  。
3.  上**服务器选择**屏幕上，单击**下一步**。
4.  上**服务器角色**屏幕上，清除该复选框旁边**Active Directory 联合身份验证服务**然后单击**下一步**。
![删除服务器](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  上**功能**屏幕上，单击**下一步**。
6.  上**确认**屏幕上，单击**删除**。
7.  完成此操作，重新启动服务器。

#### <a name="raise-the-farm-behavior-level-fbl"></a>提升场行为级别 (FBL)
在此步骤之前，您需要确保 forestprep 和 domainprep 已运行 Active Directory 环境和 Active Directory 具有 Windows Server 2016 的架构。  本文档开始使用 Windows 2016 域控制器并不需要运行这些，因为它们已运行 AD 安装时。

>[!NOTE]
>开始以下过程之前，确保 Windows Server 2016 是最新的设置从运行 Windows 更新。  继续这一过程，直到不需要进一步更新。

1. 现在在 Windows Server 2016 服务器上打开 PowerShell 并运行以下命令： **$cred = Get-credential**并按 enter。
2. 输入 SQL 服务器具有管理员权限的凭据。
3. 现在在 PowerShell 中，输入以下信息：**Invoke-AdfsFarmBehaviorLevelRaise -Credential $cred**
2. 出现提示时，键入**Y**。这将开始提升级别。  完成此操作已成功提升 FBL。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. 现在，如果转到 AD FS 管理，你将看到已添加适用于 Windows Server 2016 中的 AD FS 的新节点  
4. 同样，可以使用 PowerShell 脚本：Get AdfsFarmInformation 以显示当前 FBL。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>升级现有的 WAP 服务器的配置版本
1. 在每个 Web 应用程序代理服务器上，重新配置通过在提升的窗口中执行以下 PowerShell 命令 WAP:  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. 从群集中删除旧服务器并保留仅的 WAP 服务器运行最新的服务器版本中，已重新配置更高版本，通过运行以下 Powershell commandlet。
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. 运行 Get WebApplicationProxyConfiguration commmandlet 检查 WAP 配置。 ConnectedServersName 将反映从以前的命令运行的服务器。
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. 若要升级的 WAP 服务器 ConfigurationVersion，运行以下 Powershell 命令。
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. 验证已使用 Get WebApplicationProxyConfiguration Powershell 命令升级 ConfigurationVersion。
