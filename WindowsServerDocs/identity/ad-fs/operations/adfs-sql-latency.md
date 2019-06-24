---
title: 微调 SQL 和解决与 AD FS 的延迟问题
description: 本文档介绍如何优化 SQL 与 AD FS。
author: billmath
ms.author: billmath
manager: daveba
ms.date: 06/20/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb699a1f92013f5657d2fbb48b203f96a5e5a5ba
ms.sourcegitcommit: 6b6c3601fb7493ab145ccff02db26d7123df9a3d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67322860"
---
# <a name="fine-tuning-sql-and-addressing-latency-issues-with-ad-fs"></a>微调 SQL 和解决与 AD FS 的延迟问题
中的更新[AD FS 2016](https://support.microsoft.com/help/4503294/windows-10-update-kb4503294)我们引入了以下改进以减少跨数据库的延迟。 有关 AD FS 2019 将来的更新将包括这些改进。

## <a name="in-memory-cache-update-in-background-thread"></a>在后台线程中的内存中缓存更新 
在之前始终打开可用性 (AoA) 部署中，延迟存在的任何"读取"操作因为主节点可能位于不同的数据中心。 两个不同数据中心之间的调用会导致延迟。  

AD FS 的最新更新，通过后台线程来刷新 AD FS 配置缓存，用于设置刷新时间段设置添加目标是减少延迟。 在请求线程，大大减少了数据库查找所花费的时间，如数据库缓存更新将移到后台线程。  

当`backgroundCacheRefreshEnabled`设置为 true，AD FS 将启用后台线程来运行缓存更新。 从缓存中提取数据的频率可以自定义为时间值通过设置`cacheRefreshIntervalSecs`。 默认值设置为 300 秒时`backgroundCacheRefreshEnabled`设置为 true。 一组值的持续时间之后, AD FS 开始刷新缓存的和旧的缓存数据更新时，将继续使用。  

>[!NOTE]
> 将刷新缓存的数据之外的`cacheRefreshIntervalSecs`如果 ADFS 接收来自 SQL 表明，在数据库中发生了更改的通知，则值。 此通知会触发缓存刷新。 

### <a name="recommendations-for-setting-the-cache-refresh"></a>有关设置缓存刷新建议 
缓存刷新的默认值是**5 分钟内**。 建议将其设置为**1 小时**降低由 AD FS 不必要的数据刷新，因为如果未发生任何 SQL 更改刷新缓存数据。  

AD FS 注册一个回调以 SQL 更改且 ADFS 在更改后收到通知。 此方法中，通过 ADFS 接收每个新更改从 SQL 一旦它发生。 

出现网络问题，这会导致 AD FS 缺少 SQL 通知中，AD FS 将刷新缓存指定的时间间隔时刷新值。 如果 AD FS 和 SQL 之间可能是任何连接问题，建议设置为低于 1 小时的缓存刷新值。  

### <a name="configuration-instructions"></a>配置说明 
配置文件支持多个缓存项。 下面列出可以所有配置基于组织的需求。 

下面的示例启用后台缓存刷新，并设置为 1800 秒或 30 分钟的缓存刷新周期。 这必须在 ADFS 中的每个节点上完成，必须随后重启 ADFS 服务。 所做的更改不影响其他节点，并在所有节点中进行更改之前测试的第一个节点。 

  1. 导航到 AD FS 配置文件，在"Microsoft.IdentityServer.Service"部分下，添加以下条目：  
  
  - `backgroundCacheRefreshEnabled`  -指定是否已启用后台缓存功能。 "true/false"值。
  - `cacheRefreshIntervalSecs` -在数秒内 ADFS 刷新缓存的值。 如果没有 SQL 中的任何更改，AD FS 将刷新缓存。 AD FS 将会收到 SQL 通知并刷新缓存。  
 
 >[!NOTE]
 > 配置文件中的所有项都是区分大小写。  
 &lt;cache cacheRefreshIntervalSecs="1800" backgroundCacheRefreshEnabled="true" /&gt; 
 
其他支持的可配置值： 

   - **maxRelyingPartyEntries** -最大数字的信赖方的项的 AD FS 将保留在内存中。 OAuth 应用程序权限缓存还使用此值。 如果有多个应用程序权限比 RPs，如果所有将存储在内存中，此值应为应用程序权限数。 默认值为 1000年。
   - **maxIdentityProviderEntries** -此声明的最大数目是 AD FS 将保留在内存中的提供程序条目。 默认值为 200。 
   - **maxClientEntries** -这是 AD FS 将保留在内存中的 OAuth 客户端条目最大数目。 默认值为 500。 
   - **maxClaimDescriptorEntries** -最大的 AD FS 将保留在内存中的声明描述符条目数。 默认值为 500。 
   - **maxNullEntries** -这用作负缓存。 如果未找到 AD FS 将查找数据库中的条目，AD FS 添加负缓存中。 这是该缓存的最大大小。 每种类型的对象的负缓存，它不是单个缓存的所有对象。 默认值为 50,0000。 

## <a name="multiple-artifact-db-support-across-datacenters"></a>多个项目 DB 支持跨数据中心 
对于以前配置的多个数据中心中，AD FS 具有仅支持单个项目数据库，在检索调用期间导致跨 center 数据中心延迟。  

若要减少跨数据中心的延迟，AD FS 管理员可以现在部署多个项目 DB 实例，然后修改 AD FS 节点的配置文件以指向不同项目 DB 实例。 可以在配置文件，允许每个节点项目 DB 中提供项目数据库连接字符串。 如果不存在的配置文件中的连接字符串，该节点将回退到以前的设计，以使用配置数据库中存在的项目数据库。  
使用此配置也支持混合环境。  

### <a name="requirements"></a>要求： 
在设置多个项目数据库支持之前, 的所有节点上运行更新和更新二进制文件，因为通过此功能发生了多节点的调用。 
  1. 生成部署脚本创建项目数据库：若要部署多个项目 DB 实例，管理员将需要生成的项目 DB SQL 部署脚本。 此更新后，现有的一部分`Export-AdfsDeploymentSQLScript`cmdlet 已更新，以根据需要采用参数指定用于 AD FS 生成的一个 SQL 部署脚本的数据库。 
 
 例如，若要为只是项目 DB 生成部署脚本，请指定`-DatabaseType`参数，并传入的值"项目"。 可选`-DatabaseType`参数指定的 AD FS 数据库类型，可以设置为：所有 （默认值），项目或配置。 如果没有`-DatabaseType`指定参数，该脚本将配置项目和配置脚本。  

   ```PowerShell
   PS C:\> Export-AdfsDeploymentSQLScript -DestinationFolder <script folder where scripts will be created> -ServiceAccountName <domain\serviceaccount> -DatabaseType "Artifact" 
   ```
在生成的脚本应以创建所需的数据库，并为 AD FS 服务帐户、 SQL SA 权限到这些数据库的 SQL 计算机上运行。

 2. 创建使用部署脚本项目数据库。 将新生成的 CreateDB.sql 和 SetPermissions.sql 部署脚本复制到 SQL server 计算机，并执行它们，以创建本地项目数据库。 
 
 3. 修改配置文件以添加项目 DB 连接。 
 导航到 AD FS 节点的配置文件中，然后在"Microsoft.IdentityServer.Service"部分下，将入口点添加到新配置 ArtifactDB。 

 >[!NOTE] 
 > artifactStore 和 connectionString 是区分大小写的值。 请确保正确配置它们。 &lt;artifactStore connectionString="Data Source=.\SQLInstance;Integrated Security=True;Initial Catalog=AdfsArtifactStore" /&gt; 
>
>使用与匹配你的 sql 连接的数据源值。



 4. 重新启动 AD FS 服务，更改才能生效。 
 
 >[!NOTE] 
 > 建议不要使用 SQL 复制或项目数据库之间的同步。 建议是将每个数据中心的一个项目数据库设置。 
 
### <a name="cross-datacenter-failover-and-database-recovery"></a>跨数据中心故障转移和数据库恢复  
建议在主项目数据库所在的数据中心上创建故障转移项目数据库。 如果发生故障转移，将不会增加延迟。 不建议跨数据中心的故障转移项目数据库。 下面详细介绍了如何调用 OAuth、 SAML、 ESL，以及令牌重播检测函数和多个项目数据库。 
 - **OAuth 和 SAML** 

   对于 OAuth 和 SAML 项目请求，该节点将项目中创建项目 DB 存在配置文件中。 如果配置文件不包含项目数据库连接，它将使用常见项目数据库。 当提取项目的下一个请求将转到另一个节点时，另一个节点将对要从项目 DB 提取项目的第一个节点进行 rest API。 这是必需的因为不同节点可能具有不同项目数据库，节点不知道该如何。 如果第 1 个节点已关闭，项目解析将失败。 由于此设计中，复制不同数据中心内的项目 DB 不需要。 如果整个数据中心停机，最有可能创建该项目的节点还下，这意味着不能再解析该项目。  

 - **Extranet 锁定** 

    在配置文件中为所引用的项目数据库将用于 Extranet 锁定数据。 但是，对于 ESL 功能中，AD FS 选择 master 将数据写入项目 DB 中。 所有节点都发出 REST API 调用到主节点来获取和设置每个用户的最新信息。 如果多个项目 DB 的使用中，管理员必须选择一个主节点为每个项目数据库或数据中心。 

    若要选择一个节点来 ESL 主服务器，请导航到 ADFS 节点的配置文件中，然后在"Microsoft.IdentityServer.Service"部分下，添加以下代码：       
    
    在主机上添加以下条目。 请注意所有三个键都是区分大小写。 

    &lt;useractivityfarmrole masterFQDN = [的所选的主 FQDN] isMaster ="true"/&gt;
    
    在其他节点上添加以下条目：

   &lt;useractivityfarmrole masterFQDN = [的所选的主 FQDN] isMaster ="false"/&gt;
 
    >[!NOTE] 
    >由于多个项目数据库未同步数据，因此 ESL 值将不会同步项目数据库之间。
    用户可能可以命中不同的数据中心的请求，因此使 ExtranetLockoutThreshold 依赖于项目数据库，ExtranetLockoutThreshold 的数目 * 项目数据库数。 
 
  - **令牌重放检测** 
    
    令牌重放检测数据始终从中央项目数据库调用。 AD FS 将令牌保存从声明提供方信任，确保不能重播相同的令牌。 如果攻击者尝试重播相同的令牌，AD FS 验证令牌是否存在于项目数据库。 如果存在该令牌，则将拒绝该请求。 使用中央项目数据库的安全，因为不复制项目 DB 数据，攻击者可以将请求发送到另一个数据中心并重播令牌。 仅对中央项目数据库用作创建其他的只读副本的 ArtifactDB 不会阻止在此方案中，跨数据中心延迟。    
 
 