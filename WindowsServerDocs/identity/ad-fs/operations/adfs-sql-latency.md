---
title: 利用 AD FS 微调 SQL 和解决延迟问题
description: 本文档介绍如何通过 AD FS 微调 SQL。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 29c8e8ba52f62a335ab136756e759b6114ecfb20
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865608"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>利用 AD FS 微调 SQL 和解决延迟问题
在[AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294)的更新中，我们引入了以下改进以减少跨数据库延迟。 即将推出的 AD FS 2019 更新将包括这些改进。

## <a name="in-memory-cache-update-in-background-thread"></a>后台线程中的内存中缓存更新 
在之前的 Always on 可用性（AoA）部署中，由于主节点可以位于不同的数据中心，因此存在任何 "读取" 操作的延迟。 两个不同数据中心之间的调用导致延迟。  

在 AD FS 的最新更新中，通过添加后台线程来刷新 AD FS 配置缓存，并设置设置刷新时间段的设置，使延迟时间降低。 在请求线程中，数据库查找所花费的时间明显减少，因为数据库缓存更新会移到后台线程中。  

`backgroundCacheRefreshEnabled`当设置为 true 时，AD FS 将允许后台线程运行缓存更新。 通过设置`cacheRefreshIntervalSecs`，可以将数据从缓存中提取的频率自定义为时间值。 如果`backgroundCacheRefreshEnabled`设置为 true，则默认值设置为300秒。 在设置的值持续时间后，AD FS 开始刷新它的缓存，而更新正在进行时，将继续使用旧的缓存数据。  

>[!NOTE]
> 如果 ADFS 从 SQL 接收到指示数据库中发生`cacheRefreshIntervalSecs`了更改的通知，则将在值之外刷新缓存的数据。 此通知将触发缓存刷新。 

### <a name="recommendations-for-setting-the-cache-refresh"></a>有关设置缓存刷新的建议 
缓存刷新的默认值为**5 分钟**。 建议将其设置为**1 小时**，以减少不必要的数据刷新，因为这会 AD FS，因为在发生任何 SQL 更改时将刷新缓存数据。  

AD FS 为 SQL 更改注册回调，并且在发生更改时，ADFS 会收到通知。 通过此方法，ADFS 就会在 SQL 每次发生更改时立即收到。 

如果出现网络问题，导致 AD FS 缺少 SQL 通知，AD FS 将按缓存刷新值指定的时间间隔进行刷新。 如果 AD FS 与 SQL 之间有任何连接问题，建议将缓存刷新值设置为低于1小时。  

### <a name="configuration-instructions"></a>配置说明 
配置文件支持多个缓存条目。 下面列出的各项都可以根据组织的需要进行配置。 

下面的示例启用后台缓存刷新，并将缓存刷新周期设置为1800秒，即30分钟。 必须在每个 ADFS 节点上完成此操作，并且必须在之后重新启动 ADFS 服务。 更改不会影响其他节点，并会在更改所有节点之前测试第一个节点。 

  1. 导航到 AD FS 配置文件，在 "IdentityServer" 部分下，添加以下条目：  
  
  - `backgroundCacheRefreshEnabled`-指定是否启用后台缓存功能。 "true/false" 值。
  - `cacheRefreshIntervalSecs`-时间值（以秒为单位），ADFS 将刷新缓存。 如果 SQL 中有任何更改，AD FS 会刷新缓存。 AD FS 将接收 SQL 通知并刷新缓存。  
 
 >[!NOTE]
 > 配置文件中的所有项都区分大小写。  
 &lt;cache cacheRefreshIntervalSecs = "1800" backgroundCacheRefreshEnabled = "true"/&gt; 
 
其他支持的可配置值： 

   - **maxRelyingPartyEntries** -AD FS 将保留在内存中的信赖方条目的最大数量。 OAuth 应用程序权限缓存也使用此值。 如果有比 RPs 更多的应用程序权限，并且所有这些权限都将存储在内存中，则此值应为应用程序权限的数目。 默认值为1000。
   - **maxIdentityProviderEntries** -这是 AD FS 将保留在内存中的声明提供程序条目的最大数量。 默认值为200。 
   - **maxClientEntries** -这是 AD FS 将保留在内存中的 OAuth 客户端条目的最大数量。 默认值为 500。 
   - **maxClaimDescriptorEntries** -AD FS 将保留在内存中的声明描述符条目的最大数目。 默认值为 500。 
   - **maxNullEntries** -用于作为否定缓存。 如果 AD FS 查找数据库中的条目，但找不到该条目，则 AD FS 会添加负缓存。 这是该缓存的最大大小。 对于每种类型的对象都有负缓存，它不是所有对象的单个缓存。 默认值为50，0000。 

## <a name="multiple-artifact-db-support-across-datacenters"></a>跨数据中心的多个项目 DB 支持 
对于之前的多个数据中心的配置，AD FS 仅支持单个项目数据库，导致检索调用期间跨中心数据中心的延迟。  

为了减少跨数据中心的延迟，AD FS 管理员现在可以部署多个项目 DB 实例，然后将 AD FS 节点的配置文件修改为指向不同的项目 DB 实例。 可以在配置文件中提供项目数据库连接字符串，以允许每个节点的项目数据库。 如果连接字符串不在配置文件中，则该节点将回退到以前的设计，以使用配置数据库中存在的项目数据库。  
此配置还支持混合环境。  

### <a name="requirements"></a>要求： 
在设置多个项目数据库支持之前，请在所有节点上运行更新并更新二进制文件，因为多节点调用通过此功能进行。 
  1. 生成部署脚本以创建项目数据库：若要部署多个项目 DB 实例，管理员需要生成项目 DB 的 SQL 部署脚本。 作为此更新的一部分，已更新`Export-AdfsDeploymentSQLScript`现有 cmdlet，以根据需要使用参数，该参数指定要为其生成 SQL 部署脚本的 AD FS 数据库。 
 
 例如，若要只为项目 DB 生成部署脚本，请指定`-DatabaseType`参数，并传入值 "项目"。 可选`-DatabaseType`参数指定 AD FS 数据库类型，并且可以设置为：All （默认值）、项目或配置。 如果未`-DatabaseType`指定参数，则脚本将配置项目和配置脚本。  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
生成的脚本应在 SQL 计算机上运行，以创建所需的数据库，并向 AD FS 服务帐户授予对这些数据库的 SQL SA 权限。

 2. 使用部署脚本创建项目 DB。 将新生成的 CreateDB 和 SetPermissions 部署脚本复制到 SQL server 计算机上，并执行这些脚本以创建本地项目数据库。 
 
 3. 修改配置文件以添加 "项目 DB 连接"。 
 导航到 AD FS 节点的配置文件，然后在 "IdentityServer" 部分下，将入口点添加到新配置的 ArtifactDB。 

 >[!NOTE] 
 > artifactStore 和 connectionString 为区分大小写的值。 确保正确配置这些设置。 &lt;artifactStore connectionString = "Data Source = .\SQLInstance; 集成 Security = True; 初始目录 = AdfsArtifactStore"/&gt; 
>
>使用与 sql 连接匹配的数据源值。



 4. 重新启动 AD FS 服务，使更改生效。 
 
 >[!NOTE] 
 > 不建议在项目数据库之间使用 SQL 复制或同步。 建议为每个数据中心设置一个项目数据库。 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>跨数据中心故障转移和数据库恢复  
建议在主项目数据库所在的数据中心内创建故障转移项目数据库。 如果发生故障转移，则延迟不会增加。 不建议跨数据中心的故障转移项目数据库。 下面详细说明了如何对多个项目数据库调用 OAuth、SAML、ESL 和令牌重播检测函数。 
 - **OAuth 和 SAML** 

   对于 OAuth 和 SAML 项目请求，节点将在配置文件中存在的项目数据库中创建项目。 如果配置文件不包含项目数据库连接，它将使用 common 项目 DB。 当下一次请求提取项目时，另一个节点会将 rest API 设置为第1个节点，以从项目数据库中获取项目。 这是必需的，因为不同的节点可能具有不同的项目数据库，而且节点并不知道。 如果第一个节点关闭，则项目解析将会失败。 由于此设计，不需要在不同的数据中心之间复制项目数据库。 如果整个数据中心关闭，则很可能是创建该项目的节点也会关闭，这意味着不能再解析该项目。  

 - **Extranet 锁定** 

    配置文件中引用的项目数据库将用于 Extranet 锁定数据。 但是，对于 ESL 功能，AD FS 选择在项目数据库中写入数据的主数据库。 所有节点都向主节点发出 REST API 调用，以获取并设置每个用户的最新信息。 如果正在使用多个项目 DB，则管理员必须为每个项目 DB 或数据中心选择一个主节点。 

    若要选择一个节点作为 ESL 主机，请导航到 ADFS 节点的配置文件，并在 "IdentityServer" 部分下添加以下内容：       
    
    在 master 添加以下条目。 请注意，所有三个键都区分大小写。 

    &lt;useractivityfarmrole masterFQDN = [所选主副本的 FQDN] isMaster = "true"/&gt;
    
    在其他节点上添加以下条目：

   &lt;useractivityfarmrole masterFQDN = [所选主副本的 FQDN] isMaster = "false"/&gt;
 
    >[!NOTE] 
    >由于多个项目数据库不同步数据，ESL 值将不会在项目数据库之间进行同步。
    用户可能会遇到请求的其他数据中心，因此，ExtranetLockoutThreshold 依赖于项目数据库的数量，ExtranetLockoutThreshold * 项目数据库数。 
 
  - **令牌重播检测** 
    
    始终从中央项目数据库调用令牌重播检测数据。 AD FS 保存来自声明提供程序信任的标记，确保不能重播同一标记。 如果攻击者尝试重播同一令牌，AD FS 将验证该令牌是否存在于项目数据库中。 如果该令牌存在，则将拒绝该请求。 中心项目数据库用于安全，因为不复制项目数据库数据，攻击者可能会将请求发送到另一个数据中心并重播令牌。 在这种情况下，创建 ArtifactDB 的其他只读副本将不会阻止跨数据中心的延迟，因为只使用了中心项目数据库。    
 
 