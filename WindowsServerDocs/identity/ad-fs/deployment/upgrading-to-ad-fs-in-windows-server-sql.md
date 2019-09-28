---
title: 升级到 Windows Server 2016 中的 AD FS SQL Server
description: ''
author: billmath
manager: mtillman
ms.date: 04/11/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: dd843724faf1c7a8101def84091484a5e7f7900f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408239"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>升级到 Windows Server 2016 中的 AD FS SQL Server


> [!NOTE]  
> 仅开始升级，并计划完成。 不建议在长时间内保持 AD FS 处于混合模式状态，因为在混合模式状态下 AD FS 会导致场出现问题。


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>从 Windows Server 2012 R2 AD FS 场迁移到 Windows Server 2016 AD FS 场  
以下文档将介绍如何在将 SQL Server 用于 AD FS 数据库时将 AD FS Windows Server 2012 R2 场升级到 Windows Server 2016 中的 AD FS。  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>将 AD FS 升级到 Windows Server 2016 FBL  
Windows Server 2016 AD FS 中的新增功能是场行为级别功能（FBL）。   此功能是场范围内的，并决定 AD FS 场可使用的功能。   默认情况下，Windows Server 2012 R2 AD FS 场中的 FBL 位于 Windows Server 2012 R2 FBL。  

可以将 Windows server 2016 AD FS Server 添加到 Windows Server 2012 R2 场中，它将在与 Windows Server 2012 R2 相同的 FBL 上运行。  如果你有 Windows Server 2016 AD FS 服务器以这种方式运行，则你的场会被称为 "混合"。  但是，在将 FBL 提升到 Windows Server 2016 之前，你将无法利用新的 Windows Server 2016 功能。  使用混合场：  

-   管理员可以向现有 Windows Server 2012 R2 场中添加新的 Windows Server 2016 联合服务器。  因此，场处于 "混合模式"，并运行 Windows Server 2012 R2 场行为级别。  若要确保整个场中的行为一致，不能在此模式下配置或使用新的 Windows Server 2016 功能。  

-   在所有 Windows Server 2012 R2 联合服务器都从混合模式场中删除后，如果是 WID 场，则会将一个新的 Windows 服务2016联合服务器升级为主节点的角色，然后，管理员可以从 Win 引发 FBLdows Server 2012 R2 到 Windows Server 2016。  因此，可以配置并使用任何新 AD FS Windows Server 2016 功能。  

-   作为混合场功能的结果，希望升级到 Windows Server 2016 的 Windows Server 2012 R2 组织 AD FS 不需要部署全新的场，而是导出和导入配置数据。  相反，他们可以在 Windows Server 2016 节点处于联机状态时将其添加到现有场，而只会产生 FBL 引发的相对短暂的停机时间。  

请注意，在混合场模式下，AD FS 场无法在 Windows Server 2016 AD FS 中引入的任何新功能或功能。  这意味着要尝试新功能的组织在引发 FBL 之前无法执行此操作。  因此，如果你的组织希望在 rasing FBL 之前测试新功能，则需要部署单独的场来实现此目的。  

的其余部分提供了有关将 Windows Server 2016 联合服务器添加到 Windows Server 2012 R2 环境，然后将 FBL 提升为 Windows Server 2016 的步骤。  在下面的体系结构关系图中所述的测试环境中执行这些步骤。  

> [!NOTE]  
> 在 Windows Server 2016 FBL 中移动到 AD FS 之前，必须删除所有 Windows 2012 R2 节点。  你不能只是将 Windows Server 2012 R2 OS 升级到 Windows Server 2016，并使其成为2016节点。  需要将其删除，并将其替换为新的2016节点。  

下面的体系结构关系图显示了用于验证和记录以下步骤的设置。

![体系结构](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png)


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>将 Windows 2016 AD FS 服务器联接到 AD FS 场

1.  使用服务器管理器在 Windows Server 2016 上安装 Active Directory 联合身份验证服务角色  

2.  使用 AD FS 配置向导将新的 Windows Server 2016 服务器加入现有 AD FS 场。  在**欢迎**屏幕上，单击 "**下一步**"。
 ![Join farm @ no__t-1  
3.  在 "**连接到 Active Directory 域服务**" 屏幕上，p) 具有执行联合身份验证服务配置权限的**管理员帐户**，然后单击 "**下一步**"。
4.  在 "**指定场**" 屏幕上，输入 SQL server 和实例的名称，然后单击 "**下一步**"。
![Join farm @ no__t-1
5.  在 "**指定 SSL 证书**" 屏幕上，指定证书，然后单击 "**下一步**"。
![Join farm @ no__t-1
6.  在 "**指定服务帐户**" 屏幕上，指定服务帐户，然后单击 "**下一步**"。
7.  在 "**查看选项**" 屏幕上，查看选项，然后单击 "**下一步**"。
8.  在 "**先决条件检查**" 屏幕上，确保所有先决条件检查已通过，然后单击 "**配置**"。
9.  在**结果**屏幕上，确保已成功配置服务器，并单击 "**关闭**"。


#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>删除 Windows Server 2012 R2 AD FS 服务器

>[!NOTE]
>使用 SQL 作为数据库时，无需使用 AdfsSyncProperties-Role 设置主 AD FS 服务器。  这是因为在此配置中所有节点均被视为主要节点。

1.  在 Windows Server 2012 R2 上 AD FS Server 上服务器管理器使用 "**管理**" 下的 "**删除角色和功能**"。
![Remove server @ no__t-1
2.  在“开始之前”屏幕上，单击“下一步”。
3.  在**服务器选择**屏幕上，单击 "**下一步**"。
4.  在 "**服务器角色**" 屏幕上，删除**Active Directory 联合身份验证服务**旁边的复选标记，然后单击 "**下一步**"。
![Remove server @ no__t-1
5.  在**功能**屏幕上，单击 "**下一步**"。
6.  在**确认**屏幕上，单击 "**删除**"。
7.  完成此过程后，请重新启动服务器。

#### <a name="raise-the-farm-behavior-level-fbl"></a>提升场行为级别（FBL）
在执行此步骤之前，你需要确保已在 Active Directory 环境中运行 forestprep 和域准备操作，并且 Active Directory 具有 Windows Server 2016 架构。  此文档是使用 Windows 2016 域控制器开始的，不需要运行这些文档，因为在安装 AD 时运行这些文档。

>[!NOTE]
>在开始下面的过程之前，请通过从 "设置" 运行 Windows 更新确保 Windows Server 2016 是最新的。  继续这一过程，直到不需要进一步更新。

1. 现在，在 Windows Server 2016 服务器上打开 PowerShell 并运行以下内容： **$cred = Get-Credential**并按 enter。
2. 输入对 SQL Server 拥有管理员权限的凭据。
3. 现在，在 PowerShell 中输入以下内容：**AdfsFarmBehaviorLevelRaise-Credential $cred**
2. 出现提示时，键入**Y**。这将开始提升级别。  完成此过程后，您已成功地引发了 FBL。  
@no__t 0Finish Update @ no__t-1
3. 现在，如果你前往 AD FS 管理，你将看到已添加的新节点，用于 Windows Server 2016 中的 AD FS  
4. 同样，可以使用 PowerShell cmdlt：AdfsFarmInformation 显示当前 FBL。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)

#### <a name="upgrade-the-configuration-version-of-existing-wap-servers"></a>升级现有 WAP 服务器的配置版本
1. 在每个 Web 应用程序代理上，通过在提升的窗口中执行以下 PowerShell 命令来重新配置 WAP：  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
2. 通过运行以下 Powershell commandlet，从群集中删除旧服务器，并仅保留运行最新服务器版本的 WAP 服务器。
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
3. 通过运行 Set-webapplicationproxyconfiguration commmandlet 检查 WAP 配置。 ConnectedServersName 将反映从上一个命令运行的服务器。
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
4. 若要升级 WAP 服务器的 ConfigurationVersion，请运行以下 Powershell 命令。
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
5. 验证 ConfigurationVersion 是否已通过 Set-webapplicationproxyconfiguration Powershell 命令升级。
