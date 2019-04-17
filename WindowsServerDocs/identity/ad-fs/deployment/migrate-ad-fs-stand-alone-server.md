---
title: "迁移独立广告 FS 联盟服务器或单节点广告 FS 电场的日落"
description: "提供有关迁移独立或单节点广告的信息对 Windows Server 2012 FS 2.0 服务器"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10371349fe19be92fb997c9c28f19def0ecad7e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>迁移独立广告 FS 联盟服务器或单节点广告 FS 电场的日落  
本文将广告 FS 2.0 独立 server 迁移到 Windows Server 2012 上提供详细的信息。

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>迁移独立广告 FS 2.0 服务器

使用以下步骤迁移广告 FS 2.0 对 Windows Server 2012 的服务器。
  
1.  查看并执行中的步骤[准备迁移独立广告 FS 联盟服务器或单节点广告 FS 电场的日落](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md)。  
  
2.  你从 Windows Server 2008 R2 或 Windows Server 2008 于 Windows Server 2012 的服务器上执行操作系统的就地的升级。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，此服务器上的广告 FS 配置失去并且删除广告 FS 2.0 server 角色。 而是安装了 Windows Server 2012 广告 FS server 角色，但它未配置。 必须手动创建原始广告 FS 配置并还原剩余的广告 FS 设置，以完成联盟 server 迁移。  
  
3.  创建原始广告 FS 配置。 你可以通过以下方法之一创建原始广告 FS 配置：  
  
-   使用**广告 FS 联盟服务器配置向导**创建一个新联合身份验证的服务器。 有关详细信息，请参阅[联合身份验证的服务器场中创建第一个联盟服务器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)。  
  
当你完成向导中，使用你在准备，如下所示迁移广告 FS 联盟服务器时收集的信息：  
  
 |**联合身份验证的服务器配置向导输入的选项**|**使用以下值**| 
|-----|-----| 
|**SSL 证书**上**指定联合身份验证服务名称**页面|选择你准备广告 FS 联盟 server 迁移同时录制其主题名称和指纹的 SSL 证书。|  
|**服务帐户**和**密码**上**指定服务帐户**页面|输入准备广告 FS 联盟 server 迁移时记录服务帐户信息。 **注意：**如果选择了向导中的第二个页面上的独立联盟服务器，网络服务用作自动服务帐户。|  
  
> [!IMPORTANT] 
> 仅当你使用 Windows 内部数据库 (WID) 存储广告 FS 配置数据库独立联盟服务器或单节点广告 FS 电场的日落，你可以使用此方法。  
>
>  如果你使用 SQL Server 来存储广告 FS 配置数据库单节点广告 FS 电场的日落，必须使用 Windows PowerShell 联合身份验证的服务器上创建原始广告 FS 配置。  
  
-   使用 Windows PowerShell  
  
> [!IMPORTANT]
>  如果你使用 SQL Server 存储广告 FS 配置数据库独立联盟服务器或单节点广告 FS 电场的日落，则必须使用 Windows PowerShell。  
  
以下是如何使用 Windows PowerShell 单节点 SQL Server 场中联盟服务器上创建原始广告 FS 配置的示例。  打开 Windows PowerShell 模块，并运行以下命令：`$fscredential = Get-Credential`。 输入姓名和时迁移准备 SQL server 电场的日落记录服务帐户的密码。 然后，运行以下命令：`C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`位置`Data Source`策略官方商城连接字符串值，以下文件中的数据来源值：`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。  
  
4.  还原剩余广告 FS 服务设置和信任与您的关系。 这是在此期间你可以使用导出你的文件和您准备广告 FS 迁移时收集的值手动步骤。 详细说明，请参阅还原剩余广告 FS 场配置。  
  
> [!NOTE]
>  仅限这一步中所需正在独立联盟服务器或单个节点 WID 场迁移。  如果联合服务器用作配置官方商城的 SQL Server 数据库，在数据库中保留的服务设置和信任关系。  
  
5.  请更新你的广告 FS 网页。 这是手动步骤。 如果时迁移准备备份你的自定义广告 FS 网页，使用你的备份数据覆盖默认广告 FS 网页中的默认情况下创建的**%systemdrive%\inetpub\adfs\ls**目录广告 FS 配置 Windows Server 2012 上的结果。  
  
6.  还原任何剩余广告 FS 自定义设置，如自定义特性官方商城。  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>恢复剩余广告 FS 电场的日落配置  
  
-   如下所示还原到一个节点 WID 电场的日落或独立联合身份验证服务的以下广告 FS 服务设置：  
  
    -   在广告 FS 管理控制台中，选择**服务**单击**编辑联合身份验证服务…**. 检查的值准备迁移时导出到 properties.txt 文件值针对每个验证联合身份验证服务设置：  
  
    
|**联合身份验证服务属性名称 Get-ADFSProperties 由报告**|**广告 FS 管理控制台在联合身份验证服务属性名称**|  
|-----|-----|
|显示名称|联合身份验证服务显示名称|  
|主机|联合身份验证服务名称|  
|标识符|联合身份验证服务的标识符|  
  
-   在广告 FS 管理控制台中，选择**证书**。 检查每个对时准备迁移到 certificates.txt 文件导出这些值，以验证服务通信，令牌解密，和令牌签名证书。  
  
若要更改外部证书从默认自签名证书的标记解密或令牌签名证书，必须先禁用默认启用自动证书翻转功能。  若要执行此操作，你可以使用下面的 Windows PowerShell 命令：`PSH: Set-ADFSProperties –AutoCertificateRollover $false`。  
  
-   在广告 FS 管理控制台中，选择**端点**。 检查启用的广告 FS 端点根据启用广告 FS 端点准备广告 FS 迁移时导出到一个文件的列表。  
  
-   在广告 FS 管理控制台中，选择**索赔说明**。 检查广告 FS 索赔说明根据列表索赔描述准备广告 FS 迁移时导出到一个文件的列表。 添加任何包含在你的文件，但不是包括在广告 FS 默认列表中的自定义声明说明。  请注意到文件中 ClaimType 都地图中管理主机的索赔标识符。  添加索赔描述的详细信息，请参阅[添加声称描述](../operations/add-a-claim-description.md)。 详细信息，请参阅中准备广告迁移到"步骤 1-导出服务设置"部分 FS 2.0 联合身份验证的服务器。  
  
-   在广告 FS 管理控制台中，选择**索赔提供商信任**。 你必须重新创建手动使用每个索赔提供商信任**添加索赔提供商信任向导**。  使用索赔导出，并且在准备广告 FS 迁移的同时录制的提供商信任的列表。 因为这是默认广告 FS 配置的一部分"Active Directory"索赔提供商信任，可以忽略索赔提供商的信任标识符"广告颁发机构"文件中。  但是，检查你可能已添加到之前迁移的 Active Directory 信任任何自定义声明规则。 有关详细信息创建声称信任提供商，请参阅[创建索赔提供商信任使用联盟元数据](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata)或[索赔提供商信任手动创建](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually)。  
  
-   在广告 FS 管理控制台中，**选择信赖方信任**。 你必须重新创建手动使用每个依赖方信任**添加依赖方信任向导**。 使用信赖方信任导出，并且在准备广告 FS 迁移的同时录制的列表。 创建信赖方信任的详细信息，请参阅[创建依赖方信任使用联盟元数据](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata)或[依赖方信任手动创建](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually)。 

## <a name="next-steps"></a>后续步骤
 [准备迁移广告 FS 2.0 联盟服务器](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移广告 FS 2.0 联盟服务器代理服务器](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [迁移广告 FS 2.0 联盟服务器](migrate-the-ad-fs-fed-server.md)   
 [迁移广告 FS 2.0 联盟服务器代理服务器](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移广告 FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)




-   
-    