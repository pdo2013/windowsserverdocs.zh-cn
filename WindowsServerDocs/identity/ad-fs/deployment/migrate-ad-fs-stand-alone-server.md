---
title: 迁移独立 AD FS 联合服务器或单节点 AD FS 场
description: 提供有关将独立或单节点 AD FS 2.0 服务器迁移到 Windows Server 2012 的信息
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b8029d67a9f21e5189322692b8f1316306542c96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359380"
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>迁移独立 AD FS 联合服务器或单节点 AD FS 场  
本文档详细介绍了如何将 AD FS 2.0 独立服务器迁移到 Windows Server 2012。

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>迁移独立 AD FS 2.0 服务器

使用以下过程将 AD FS 2.0 服务器迁移到 Windows Server 2012。
  
1.  查看并执行准备中的过程[以迁移独立 AD FS 联合服务器或单节点 AD FS 场](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md)。  
  
2.  将服务器上的操作系统从 Windows Server 2008 R2 或 Windows Server 2008 就地升级到 Windows Server 2012。 有关详细信息，请参阅[安装 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作为操作系统升级的结果，在此服务器上的 AD FS 配置将丢失并且 AD FS 2.0 的服务器角色会被删除。 改为安装 Windows Server 2012 AD FS Server 角色，但未对其进行配置。 你必须手动创建原始 AD FS 配置并还原剩余的 AD FS 设置，以完成联合服务器迁移。  
  
3. 创建原始 AD FS 配置。 你可以通过使用下列方法之一来创建原始 AD FS 配置：  
  
-   使用“AD FS 联合服务器配置向导” 创建新的联合服务器。 有关详细信息，请参阅[在联合服务器场中创建第一个联合服务器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)。  
  
当你完成该向导时，使用在准备迁移 AD FS 联合服务器时收集的信息，如下所示：  
  
 |**联合服务器配置向导输入选项**|**使用以下值**| 
|-----|-----| 
|“指定联合身份验证服务名称”页面上的“SSL 证书”|选择你在准备 AD FS 联合服务器迁移时记录的其使用者名称和指纹的 SSL 证书。|  
|“指定服务帐户”页面上的“服务帐户”和“密码”|输入你在准备 AD FS 联合服务器迁移时所记录的服务帐户信息。 **注意：** 如果你在向导的第二页上选择独立联合服务器，会自动将网络服务用作服务帐户。|  
  
> [!IMPORTANT] 
> 仅当你使用 Windows 内部数据库 (WID) 为独立联合服务器或单节点 AD FS 场存储 AD FS 配置数据库时，可以采用此方法。  
>
>  如果你使用 SQL Server 为单节点 AD FS 场存储 AD FS 配置数据库，则必须使用 Windows PowerShell 在联合服务器上创建原始 AD FS 配置。  
  
-   使用 Windows PowerShell  
  
> [!IMPORTANT]
>  如果你使用 SQL Server 为独立联合服务器或单节点 AD FS 场存储 AD FS 配置数据库，则必须使用 Windows PowerShell。  
  
以下是如何使用 Windows PowerShell 在单节点 SQL Server 场中的联合服务器上创建原始 AD FS 配置的示例。  打开 Windows PowerShell 模块，并运行以下命令： `$fscredential = Get-Credential`。 输入你在为迁移准备 SQL Server 场时所记录的服务帐户的名称和密码。 然后运行以下命令： `C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"` ，其中 `Data Source` 是以下文件 `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`的策略存储连接字符串值中的数据源值。  
  
4. 还原剩余的 AD FS 服务设置和信任关系。 这是一个手动步骤，你可以在此期间使用导出的文件和在准备 AD FS 迁移时收集的值。 有关详细说明，请参阅还原剩余 AD FS 场配置。  
  
> [!NOTE]
>  只有在迁移独立联合服务器或单节点 WID 场的情况下才需要执行此步骤。  如果联合服务器使用 SQL Server 数据库作为配置存储，则在数据库中保留服务设置和信任关系。  
  
5. 更新你的 AD FS 网页。 这是一个手动步骤。 如果你在准备迁移时备份你的自定义 AD FS 网页，则使用你的备份数据来覆盖默认情况下在**位于%systemdrive%\inetpub\adfs\ls**目录中作为 AD FS 的结果创建的默认 AD FS 网页Windows Server 2012 上的配置。  
  
6. 还原任何剩余 AD FS 自定义项，如自定义属性存储。  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>还原剩余的 AD FS 场配置  
  
-   请将以下 AD FS 服务设置还原到单节点 WID 场或独立联合身份验证服务，如下所示：  
  
    -   在 AD FS 管理控制台中，选择“服务” ，然后单击“编辑联合身份验证服务...”。 在准备迁移时，通过针对你导出到 properties.txt 文件中的值来检查每个值，从而验证联合身份验证服务设置：  
  
    
|**Set-adfsproperties 报告的联合身份验证服务属性名称**|**AD FS 管理控制台中联合身份验证服务属性名称**|  
|-----|-----|
|DisplayName|联合身份验证服务显示名称|  
|HostName|联合身份验证服务名称|  
|标识符|联合身份验证服务标识符|  
  
-   在 AD FS 管理控制台中选择“证书”。 通过检查每个在准备迁移时导出到 certificates.txt 文件中的值，验证服务通信、令牌解密和令牌签名证书。  
  
若要将令牌解密或令牌签名证书从默认的自签名证书更改为外部证书，必须先禁用按默认启用的自动证书滚动功能。  为此，可以使用以下 Windows PowerShell 命令： `PSH: Set-ADFSProperties –AutoCertificateRollover $false`。  
  
-   在 AD FS 管理控制台中选择“终结点”。 根据你在准备 AD FS 迁移时导出到文件中的已启用 AD FS 终结点列表检查已启用的 AD FS 终结点。  
  
-   在 AD FS 管理控制台中选择“声明说明”。 针对你在准备 AD FS 迁移时导出到文件中的声明说明列表，检查 AD FS 声明说明的列表。 在 AD FS 中添加包含在文件中，但未包含在默认列表中的所有自定义声明说明。  请注意，管理控制台中的 Claim 标识符将映射到文件中的 ClaimType。  有关添加声明说明的详细信息，请参阅[添加声明说明](../operations/add-a-claim-description.md)。 有关详细信息，请参阅准备迁移 AD FS 2.0 联合服务器中的“步骤 1 - 导出服务设置”部分。  
  
-   在 AD FS 管理控制台中，选择“声明提供方信任”。 你必须使用“添加声明提供方信任向导”手动重新创建每个声明提供方信任。  使用你在准备 AD FS 迁移时已导出并已记录的声明提供方信任列表。 你可以忽略文件中附带标识符“AD 颁发机构”的声明提供方信任，因为这是作为默认 AD FS 配置的一部分的“Active Directory”声明提供方信任。  但是，在迁移之前检查你可能已添加到 Active Directory 信任的任何自定义声明规则。 有关创建声明提供方信任的详细信息，请参阅[使用联合元数据创建声明提供方信任](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata)或[手动创建声明提供方信任](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually)。  
  
-   在 AD FS 管理控制台中，选择“信赖方信任”。 你必须使用“添加信赖方信任向导”手动重新创建每个信赖方信任。 使用你在准备 AD FS 迁移时已导出并已记录的信赖方信任列表。 有关创建信赖方信任的详细信息，请参阅[使用联合元数据创建信赖方信任](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata)或[手动创建信赖方信任](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually)。 

## <a name="next-steps"></a>后续步骤
 [准备将 AD FS 2.0 联合服务器迁移](prepare-to-migrate-ad-fs-fed-server.md)   
 [准备迁移 AD FS 2.0 联合服务器代理](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [将 AD FS 2.0 联合服务器迁移](migrate-the-ad-fs-fed-server.md)   
 [迁移 AD FS 2.0 联合服务器代理](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [迁移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)




-   
-    