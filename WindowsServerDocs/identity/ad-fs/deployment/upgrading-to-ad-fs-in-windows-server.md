---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: Windows Server 2016 中的 AD FS 升级
description: ''
author: billmath
manager: femila
ms.date: 04/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 39c735e9dde0fd60c7eb9ccfe0af890bdc5a5950
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838318"
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>使用 WID 数据库升级到 Windows Server 2016 中的 AD FS

>适用于：Windows Server 2019、Windows Server 2016


## <a name="upgrading-a-windows-server-2012-r2-or-2016-ad-fs-farm-to-windows-server-2019"></a>Windows Server 2012 R2 或 2016 AD FS 场升级到 Windows Server 2019 
以下文档将描述如何升级到 Windows Server 2019 中的 AD FS 的 AD FS 场，使用 WID 数据库时。  

### <a name="ad-fs-farm-behavior-levels-fbl"></a>AD FS 场行为级别 (FBL)  
在 Windows Server 2016 的 AD FS 中，引入了场行为级别 (FBL)。 这是将确定可以使用功能在 AD FS 场的场级设置。 

下表列出了由 Windows Server 版本的 FBL 值：
| Windows Server 版本  | FBL | AD FS 配置数据库名称 |
| ------------- | ------------- | ------------- |
| 2012 R2  | 1  | AdfsConfiguration |
| 2016  | 3  | AdfsConfigurationV3 |
| 2019  | 4  | AdfsConfigurationV4 |

> [!NOTE]  
> 升级 FBL 创建新的 AD FS 配置数据库。  请参阅上表以获取每个 Windows Server AD FS 版本和 FBL 值的配置数据库的名称

### <a name="new-vs-upgraded-farms"></a>新安装和升级场
默认情况下，新的 AD FS 场中 FBL 与安装的第一个场节点的 Windows Server 版本的值匹配。  

更高版本的 AD FS 服务器可以加入 AD FS 2012 R2 或 2016年场，并且场将现有节点作为同一个 FBL 速度运行。 当有多个操作系统的最低版本为 FBL 值在同一场中的 Windows Server 版本时，您的服务器场称为"mixed"。 但是，您将不能充分利用更高版本的功能，直到引发 FBL。 与混合的服务器场：  

-   管理员可以将新的 Windows Server 2019 联合身份验证服务器添加到现有的 Windows Server 2012 R2 或 2016年场。 因此，场处于"混合模式"，在与原始场相同的场行为级别进行操作。 若要确保整个场中一致的行为，不能配置或使用较新的 Windows Server AD FS 版本的功能。  

- 可以引发 FBL 之前，管理员必须从场中删除 Windows Server 早期版本的 AD FS 节点。  对于 WID 场，请注意，这需要一个新的 Windows Server 2019 联合身份验证服务器 tp 提升到主节点场中的角色。

-   一旦所有联合服务器场中的保持相同的 Windows Server 版本，则会引发 FBL。  因此，任何新的 AD FS Windows Server 2019 功能可配置和使用。

请注意，在混合的场模式下，AD FS 场不支持的任何新功能或 Windows Server 2019 中的 AD FS 中引入的功能。 这意味着想要试用新功能的组织不能执行此操作之前引发 FBL。 因此如果你的组织希望在测试之前祝愿 FBL 的新功能，您需要部署一个单独的场来执行此操作。  

其余部分是文档提供有关将 Windows Server 2019 联合身份验证服务器添加到 Windows Server 2016 或 2012 R2 环境，然后提升到 Windows Server 2019 FBL 步骤。 在下面的体系结构关系图中所述的测试环境中执行这些步骤。  

> [!NOTE]  
> 可以将它们移动到 Windows Server 2019 FBL 中的 AD FS 之前，必须删除所有 Windows Server 2016 或 2012 R2 节点。 只需不能将 Windows Server 2016 或 2012 R2 操作系统升级到 Windows Server 2019，并使其成为 2019年节点。 你将需要删除它，然后将其替换为新的 2019年节点。



##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2019-farm-behavior-level"></a>若要将 AD FS 场升级到 Windows Server 2019 场行为级别  

1.  使用服务器管理器，在 Windows Server 2019 上安装 Active Directory 联合身份验证服务角色 

2.  使用 AD FS 配置向导，将新的 Windows Server 2019 服务器加入到现有的 AD FS 场。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  在 Windows Server 2019 联合身份验证服务器上，打开 AD FS 管理。 请注意，管理功能将无效，因为此联合身份验证服务器不是主服务器。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  在 Windows Server 2019 服务器上，打开提升的 PowerShell 命令窗口并运行以下 cmdlt: `Set-AdfsSyncProperties -Role PrimaryComputer`

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  在以前配置为主要 AD FS 服务器上打开提升的 PowerShell 命令窗口并运行以下 cmdlt: `Set-AdfsSyncProperties -Role SecondaryComputer -PrimaryComputerName {FQDN} `

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  在每个 Web 应用程序代理服务器上，重新配置通过在提升的窗口中执行以下 PowerShell 命令 WAP:  
```powershell
$trustcred = Get-Credential -Message "Enter Domain Administrator credentials"
Install-WebApplicationProxy -CertificateThumbprint {SSLCert} -fsname fsname -FederationServiceTrustCredential $trustcred  
```

7.  现在在 Windows Server 2016 联合身份验证服务器上打开 AD FS 管理。 请注意，现在所有管理功能会显示，因为主角色已转移到此服务器。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

8.  如果您要升级到 2016年或 2019 AD FS 2012 R2 场，场升级要求 AD 架构是至少为级别 85。  若要升级的架构、 使用 Windows Server 2016 安装媒体，打开命令提示符并导航到 support\adprep 目录。 运行以下命令：  `adprep /forestprep`

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

    该操作完成后运行 `adprep/domainprep`
    >[!NOTE]
    >在运行下一步之前, 请确保 Windows Server 是最新的设置从运行 Windows Update。 继续这一过程，直到不需要进一步更新。 
    > 
    
    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

9. 现在在 Windows Server 2016 服务器上打开 PowerShell 并运行以下 cmdlt:
    >[!NOTE]
    > 在运行下一步之前，必须从场删除所有 2012 R2 服务器。
 
    `Invoke-AdfsFarmBehaviorLevelRaise`  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

10. 出现提示时，请键入 Y。这将开始提升级别。 完成此操作已成功提升 FBL。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

11. 现在，如果您转到 AD FS 管理，你将看到更高版本的 AD FS 版本，已添加新功能 

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

13. 同样，可以使用 PowerShell cmdlt:`Get-AdfsFarmInformation`以显示当前 FBL。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  
    

