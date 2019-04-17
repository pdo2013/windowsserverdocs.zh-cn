---
title: "升级到 Windows Server 2016 的 SQL Server 中广告 FS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 70f279bf-aea1-4f4f-9ab3-e9157233e267
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 034d4f1f8d81cf105ba94bff34b180555702acde
ms.sourcegitcommit: 33c1f4965cd2eed7d384a096cfa5c883467b16a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2017
---
# <a name="upgrading-to-ad-fs-in-windows-server-2016-with-sql-server"></a>升级到 Windows Server 2016 的 SQL Server 中广告 FS

>适用于：Windows Server 2016


## <a name="moving-from-a-windows-server-2012-r2-ad-fs-farm-to-a-windows-server-2016-ad-fs-farm"></a>从 Windows Server 2012 R2 广告 FS 场移至 Windows Server 2016 广告 FS 电场的日落  
下面的文档将介绍如何升级到 Windows Server 2016 中的广告 FS 的广告 FS Windows Server 2012 R2 电场的日落时你正在使用 SQL Server 广告 FS 数据库。  

### <a name="upgrading-ad-fs-to-windows-server-2016-fbl"></a>升级到 Windows Server 2016 FBL 广告 FS  
中的新增适用于 Windows Server 2016 的广告 FS 是场行为级别功能 (FBL)。   此功能宽电场的日落，并且确定广告 FS 电场的日落可以使用的功能。   默认情况下，Windows Server 2012 R2 广告 FS 场中的 FBL 是在 Windows Server 2012 R2 FBL。  

Windows Server 2016 广告 FS 服务器可以将添加到 Windows Server 2012 R2 电场的日落，它将作为 Windows Server 2012 R2 相同 FBL 以运行。  当你有 Windows Server 2016 广告 FS 服务器操作系统以这种方式时，你电场的日落称为"混合"。  但是，你将不能充分利用新的 Windows Server 2016 功能，直到 FBL 引发至 Windows Server 2016。  与混合电场的日落：  

-   管理员可以新的、 添加到现有 Windows Server 2012 R2 场 Windows Server 2016 联盟服务器。  因此，电场的日落处于"混合模式"，并运行 Windows Server 2012 R2 电场的日落行为级别。  以确保整个场一致的行为，无法配置 Windows Server 2016 的新增功能或将其用于在此模式中。  

-   所有 Windows Server 2012 R2 联合身份验证的服务器都已从混合的模式电场的日落，并在 WID 电场的日落的情况下其中一个新的 Windows 提供 2016年联盟服务器已升级到主节点角色，一旦管理员可以然后提升对 Windows Server 2016 FBL 从 Windows Server 2012 R2。  因此，然后进行配置并且用于任何新的广告 FS Windows Server 2016 功能。  

-   因此的混合的场功能，广告 FS Windows Server 2012 R2 想要升级到 Windows Server 2016 的组织不必部署全新电场的日落，导出和导入配置数据。  他们可以联机时，将 Windows Server 2016 节点添加到现有场而只会产生相对简短的停机参与 FBL 提升的。  

请注意，在混合的场模式时，广告 FS 电场的日落不支持的新功能或 Windows Server 2016 中的广告 FS 中引入的功能。  这意味着想要试用的新功能的组织无法执行此操作之前 FBL 引发。  因此如果你的组织希望测试祝愿 FBL 之前的新功能，你将需要部署单独场执行此操作。  

其余内容将文档提供对 Windows Server 2012 R2 环境中添加的 Windows Server 2016 联合身份验证的服务器，然后引发了对 Windows Server 2016 FBL 的步骤。  按下体系结构图中所述的测试环境中执行了以下步骤。  

> [!NOTE]  
> 你可以将它们移到在 Windows Server 2016 FBL 广告 FS 之前，你必须要删除的所有 Windows 2012 R2 节点。  你只需无法升级到 Windows Server 2016 的 Windows Server 2012 R2 操作系统，并使其成为 2016年节点。  你将需要将其删除，并将其替换为新的 2016年节点。  

下面的体系结构图中显示用于验证和录制以下步骤设置。

![建筑](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/arch.png) 


#### <a name="join-the-windows-2016-ad-fs-server-to-the-ad-fs-farm"></a>Windows 2016 广告 FS 服务器加入广告 FS 电场的日落

1.  在 Windows Server 2016 上使用服务器管理器安装 Active Directory 联合身份验证服务角色  

2.  使用该广告 FS 配置向导，加入现有广告 FS 场的新的 Windows Server 2016 服务器。  在**欢迎**屏幕单击**下一步**。
 ![加入电场的日落](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure1.png)  
3.  在**连接到 Active Directory 域服务**屏幕上，s**指定管理员帐户**权限，以执行联合身份验证服务配置，然后单击**下一步**。
4.  在**指定电场的日落**屏幕，输入的 SQL server 实例名称，然后单击**下一步**。
![加入电场的日落](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure3.png)
5.  在**指定 SSL 证书**屏幕、 指定证书，然后单击**下一步**。
![加入电场的日落](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/configure4.png)
6.  在**指定的服务帐户**屏幕、 指定的服务的帐户，然后单击**下一步**。 
7.  在**查看选项**屏幕、 查看的选项，然后单击**下一步**。 
8.  在**先决条件检查**屏幕上，确保所有必需预检查已通过单击**配置**。
9.  在**结果**屏幕，确保服务器已成功配置，然后单击**关闭**。
 
   
#### <a name="remove-the-windows-server-2012-r2-ad-fs-server"></a>Windows Server 2012 R2 广告 FS 服务器中删除

>[!NOTE]
>不需要将主要使用组 AdfsSyncProperties 广告 FS 服务器设置-使用 SQL 作为数据库时的角色。  这是因为的所有节点将视为主要此配置中。

1.  Windows Server 2012 R2 广告 FS 服务器上中服务器管理器中使用**删除角色和功能**下**管理**。 
![删除 server](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove1.png)
2.  在**在开始之前**屏幕上，单击**下一步**。
3.  在**选择服务器**屏幕上，单击**下一步**。
4.  在**服务器角色**屏幕上，旁边取消选中**Active Directory 联合身份验证服务**单击**下一步**。
![删除 server](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/remove2.png)
5.  在**功能**屏幕上，单击**下一步**。
6.  在**确认**屏幕上，单击**删除**。
7.  一旦此操作完成，重新启动的服务器。
     
#### <a name="raise-the-farm-behavior-level-fbl"></a>提升电场的日落行为级别 (FBL)
在这一步之前，你需要确保 forestprep 和域已运行 Active Directory 环境和 Active Directory 具有的 Windows Server 2016 的方案。  本文档开始使用 Windows 2016 域控制器，不需要运行这些，因为它们正在运行时广告已安装。

1. 立即在 Windows Server 2016 服务器上打开 PowerShell，并运行以下命令：**$cred = 获取凭据**输入的词和。
2. 输入 SQL Server 具有管理员权限的凭据。
3. 现在在 PowerShell 中，输入以下命令：**调用 AdfsFarmBehaviorLevelRaise-凭据 $cred**
2. 出现提示时，键入**Y**。这将开始提升级别。  此操作完成后，你已成功引发 FBL。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish1.png)
3. 现在，如果你转到广告 FS 管理，你将看到已添加广告 fs Windows Server 2016 中的新节点  
4. 同样，你可以使用 PowerShell cmdlt： 获取-AdfsFarmInformation 来向你显示当前 FBL。  
![完成更新](media/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL/finish2.png)
