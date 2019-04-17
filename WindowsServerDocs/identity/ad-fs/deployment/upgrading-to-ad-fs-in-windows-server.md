---
ms.assetid: 7671e0c9-faf0-40de-808a-62f54645f891
title: "升级到 Windows Server 2016 中的广告 FS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ce07398a2d624a1e9b004cd35eb9228d59dc2b5b
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/20/2018
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-using-a-wid-database"></a>升级到 Windows Server 2016 使用 WID 数据库广告 FS

>适用于：Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>从 Windows Server 2012 R2 广告 FS 场移至 Windows Server 2016 广告 FS 电场的日落  
下面的文档将介绍如何升级到 Windows Server 2016 中的广告 FS 的广告 FS Windows Server 2012 R2 电场的日落，当你使用的 WID 数据库。  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>升级到 Windows Server 2016 FBL 广告 FS  
中的新增适用于 Windows Server 2016 的广告 FS 是场行为级别功能 (FBL)。   此功能宽电场的日落，并且确定广告 FS 电场的日落可以使用的功能。   默认情况下，Windows Server 2012 R2 广告 FS 场中的 FBL 是在 Windows Server 2012 R2 FBL。  

Windows Server 2016 广告 FS 服务器可以将添加到 Windows Server 2012 R2 电场的日落，它将作为 Windows Server 2012 R2 相同 FBL 以运行。  当你有 Windows Server 2016 广告 FS 服务器操作系统以这种方式时，你电场的日落称为"混合"。  但是，你将不能充分利用新的 Windows Server 2016 功能，直到 FBL 引发至 Windows Server 2016。  与混合电场的日落：  

-   管理员可以新的、 添加到现有 Windows Server 2012 R2 场 Windows Server 2016 联盟服务器。  因此，电场的日落处于"混合模式"，并运行 Windows Server 2012 R2 电场的日落行为级别。  以确保整个场一致的行为，无法配置 Windows Server 2016 的新增功能或将其用于在此模式中。  

-   所有 Windows Server 2012 R2 联合身份验证的服务器都已从混合的模式电场的日落，并在 WID 电场的日落的情况下其中一个新的 Windows Server 2016 联盟服务器已升级到主节点角色，一旦管理员可以然后提升对 Windows Server 2016 FBL 从 Windows Server 2012 R2。  因此，然后进行配置并且用于任何新的广告 FS Windows Server 2016 功能。  

-   因此的混合的场功能，广告 FS Windows Server 2012 R2 想要升级到 Windows Server 2016 的组织不必部署全新电场的日落，导出和导入配置数据。  他们可以联机时，将 Windows Server 2016 节点添加到现有场而只会产生相对简短的停机参与 FBL 提升的。  

请注意，在混合的场模式时，广告 FS 电场的日落不支持的新功能或 Windows Server 2016 中的广告 FS 中引入的功能。  这意味着想要试用的新功能的组织无法执行此操作之前 FBL 引发。  因此如果你的组织希望测试祝愿 FBL 之前的新功能，你将需要部署单独场执行此操作。  

其余内容将文档提供对 Windows Server 2012 R2 环境中添加的 Windows Server 2016 联合身份验证的服务器，然后引发了对 Windows Server 2016 FBL 的步骤。  按下体系结构图中所述的测试环境中执行了以下步骤。  

> [!NOTE]  
> 你可以将它们移到在 Windows Server 2016 FBL 广告 FS 之前，你必须要删除的所有 Windows 2012 R2 节点。  你只需无法升级到 Windows Server 2016 的 Windows Server 2012 R2 操作系统，并使其成为 2016年节点。  你将需要将其删除，并将其替换为新的 2016年节点。
>
> 升级 FBL 使用 SQL 存储广告 FS 配置时会创建新管理数据库"AdfsConfigurationV3"。

##### <a name="to-upgrade-your-ad-fs-farm-to-windows-server-2016-farm-behavior-level"></a>若要升级到 Windows Server 2016 电场的日落行为级别你广告 FS 电场的日落  

1.  在 Windows Server 2016 上使用服务器管理器安装 Active Directory 联合身份验证服务角色  

2.  使用该广告 FS 配置向导，加入现有广告 FS 场的新的 Windows Server 2016 服务器。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_1.png)  

3.  在 Windows Server 2016 联盟服务器上，打开广告 FS 管理。    请注意，执行任何操作按显示此联盟服务器不可主服务器。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_3.png)  

4.  加入完成后，Windows Server 2016 服务器上，打开 PowerShell，并运行以下 cmdlt： 集 AdfsSyncProperties-角色 PrimaryComputer  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_4.png)  

5.  在原始广告 FS Windows Server 2012 R2 服务器上，打开 PowerShell，并运行以下 cmdlt： 集 AdfsSyncProperties-角色 SecondaryComputer PrimaryComputerName {FQDN}  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_5.png)  

6.  在你的 Web 应用程序代理打开 PowerShell 和运行 followoing cmdlt： 安装 WebApplicationProxy CertificateThumbprint {SSLCert}-fsname fsname-TrustCred $trustcred  

7.  立即在 Windows Server 2016 联合身份验证的服务器上打开广告 FS 管理。  请注意，现在的所有节点显示因为的主要角色已转移到此服务器。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_6.png)  

8.  与 Windows Server 2016 的安装媒体，打开 command prompt 下，并导航到 support\adprep 目录。  运行以下命令： adprep /forestprep 成功。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_7.png)  

9. 后，完成运行的 adprep 域  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_8.png)  

10. 立即在 Windows Server 2016 服务器打开 PowerShell 并运行以下 cmdlt： 调用 AdfsFarmBehaviorLevelRaise  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_9.png)  

11. 出现提示时，键入 Y。这将开始提升级别。  此操作完成后，你已成功引发 FBL。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_10.png)  

> [!NOTE]  
> 如果您的广告 FS 服务器配置为使用 SQL，现在已名称"AdfsConfiguraionV3"创建新 manamgement 数据库。 

12. 现在，如果你转到广告 FS 管理，你将看到已添加广告 fs Windows Server 2016 中的新节点  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_12.png)  

13. 同样，你可以使用 PowerShell cmdlt： 获取-AdfsFarmInformation 来向你显示当前 FBL。  

    ![升级](media/Upgrading-to-AD-FS-in-Windows-Server-2016/ADFS_Mixed_13.png)  
