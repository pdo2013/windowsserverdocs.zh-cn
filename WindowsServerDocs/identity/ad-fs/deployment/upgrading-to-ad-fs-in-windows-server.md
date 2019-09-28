---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Windows Server 2016 中的 AD FS 升级
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 169ee7d16fbac043c088c1e568600ee93e70d8a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359334"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>使用 WID 数据库升级到 Windows Server 2016 中的 AD FS


> [!NOTE]  
> 仅开始升级，并计划完成。 不建议在长时间内保持 AD FS 处于混合模式状态，因为在混合模式状态下 AD FS 会导致场出现问题。

## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>将 Windows Server 2012 R2 或 2016 AD FS 场升级到 Windows Server 2019
以下文档将介绍如何在使用 WID 数据库时将 AD FS 场升级到 Windows Server 2019 中的 AD FS。  

### <a name="ad-fs-farm-behavior-levels-fbl"></a>AD FS 场行为级别（FBL）  
在 Windows Server 2016 AD FS 中，引入了场行为级别（FBL）。 此设置决定了 AD FS 场可使用的功能。

下表按 Windows Server 版本列出了 FBL 值：

| Windows Server 版本  | FBL | AD FS 配置数据库名称 |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]  
> 升级 FBL 会创建新的 AD FS 配置数据库。  请参阅上表，了解每个 Windows Server AD FS 版本和 FBL 值的配置数据库的名称

### <a name="new-vs-upgraded-farms"></a>新的 vs 升级场
默认情况下，新 AD FS 场中的 FBL 与安装的第一个场节点的 Windows Server 版本的值相匹配。  

更高版本的 AD FS 服务器可以联接到 AD FS 2012 R2 或2016场，场将在与现有节点相同的 FBL 上运行。 如果在同一场中运行的多个 Windows Server 版本在最低版本的 FBL 值中运行，则您的场称为 "mixed"。 但是，在引发 FBL 之前，你将无法利用更高版本的功能。 使用混合场：  

-   管理员可以将新的 Windows Server 2019 联合服务器添加到现有 Windows Server 2012 R2 或2016场。 因此，场处于 "混合模式"，并以与原始场相同的场行为级别进行操作。 为确保服务器场的行为一致，不能配置或使用较新的 Windows Server AD FS 版本的功能。  

- 在可以引发 FBL 之前，管理员必须从场中删除以前的 Windows Server 版本的 AD FS 节点。  对于 WID 场，请注意，这需要将一个新的 Windows Server 2019 联合服务器 tp 升级为场中主节点的角色。

-   在场中的所有联合服务器都处于同一 Windows Server 版本后，可以引发 FBL。  因此，可以配置并使用任何新 AD FS Windows Server 2019 功能。

请注意，在混合场模式下，AD FS 场无法在 Windows Server 2019 AD FS 中引入的任何新功能或功能。 这意味着要尝试新功能的组织在引发 FBL 之前无法执行此操作。 因此，如果你的组织希望在 rasing FBL 之前测试新功能，则需要部署单独的场来实现此目的。  

的其余部分提供了有关将 Windows Server 2019 联合服务器添加到 Windows Server 2016 或 2012 R2 环境，然后将 FBL 提升为 Windows Server 2019 的步骤。 在下面的体系结构关系图中所述的测试环境中执行这些步骤。  

> [!NOTE]  
> 必须先删除所有 Windows Server 2016 或 2012 R2 节点，然后才能移到 Windows Server 2019 FBL 中的 AD FS。 你不能只是将 Windows Server 2016 或 2012 R2 OS 升级到 Windows Server 2019，并使其成为一个2019节点。 需要将其删除，并将其替换为新的2019节点。



##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>将 AD FS 场升级到 Windows Server 2019 场行为级别  

1.  使用服务器管理器在 Windows Server 2019 上安装 Active Directory 联合身份验证服务角色

2.  使用 AD FS 配置向导将新的 Windows Server 2019 服务器加入现有 AD FS 场。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  在 Windows Server 2019 联合服务器上，打开 AD FS 管理。 请注意，由于此联合服务器不是主服务器，因此管理功能不可用。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  在 Windows Server 2019 服务器上，打开提升的 PowerShell 命令窗口并运行以下 cmdlt： `Set-AdfsSyncProperties -Role PrimaryComputer`

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  在以前配置为主服务器 AD FS 服务器上，打开提升的 PowerShell 命令窗口并运行以下 cmdlt： `Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN} `

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  现在，在 Windows Server 2016 联合服务器上打开 AD FS 管理。 请注意，现在将显示所有管理员功能，因为主角色已传输到此服务器。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

7.  如果要将 AD FS 2012 R2 场升级到2016或2019，场升级要求 AD 架构至少为级别85。  若要使用 Windows Server 2016 安装介质升级架构，请打开命令提示符并导航到 support\adprep directory。 运行以下内容： `adprep /forestprep`

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

    完成运行后 `adprep/domainprep`
    >[!NOTE]
    >在运行下一步骤之前，请通过从 "设置" 运行 Windows 更新确保 Windows Server 处于最新状态。 继续这一过程，直到不需要进一步更新。
    >

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

8. 现在，在 Windows Server 2016 服务器上打开 PowerShell 并运行以下 cmdlt：
    >[!NOTE]
    > 在运行下一步骤之前，必须将所有 2012 R2 服务器从场中删除。

    `Invoke-AdfsFarmBehaviorLevelRaise`  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

9. 出现提示时，键入 Y。这将开始提升级别。 完成此过程后，您已成功地引发了 FBL。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

10. 现在，如果你开始 AD FS 管理，你将看到为之后 AD FS 版本添加了新功能

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

11. 同样，可以使用 PowerShell cmdlt： `Get-AdfsFarmInformation` 向你显示当前 FBL。  

    ![upgrade](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  

12. 若要将 WAP 服务器升级到最新级别，请在每个 Web 应用程序代理上，通过在提升的窗口中执行以下 PowerShell 命令来重新配置 WAP：  
    ```powershell
    $trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
    Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
    ```
    通过运行以下 Powershell commandlet，从群集中删除旧服务器，并仅保留运行最新服务器版本的 WAP 服务器。
    ```powershell
    Set-WebApplicationProxyConfiguration -ConnectedServersName WAPServerName1, WAPServerName2
    ```
    通过运行 Set-webapplicationproxyconfiguration commmandlet 检查 WAP 配置。 ConnectedServersName 将反映从上一个命令运行的服务器。
    ```powershell
    Get-WebApplicationProxyConfiguration
    ```
    若要升级 WAP 服务器的 ConfigurationVersion，请运行以下 Powershell 命令。
    ```powershell
    Set-WebApplicationProxyConfiguration -UpgradeConfigurationVersion
    ```
    这将完成 WAP 服务器的升级。
