---
title: 用户访问日志记录入门
desctription: Describes the User Access Logging feature and how to start using it.
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: manage-user-access-logging
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c395b8b-3b35-4042-b9cc-07e438f86d50
author: brentfor
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1706756b52777f5dd3bf1db59fb2ed087ca8648
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866254"
---
# <a name="get-started-with-user-access-logging"></a>用户访问日志记录入门

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

用户访问日志记录（UAL）是 Windows Server 中的一项功能，在本地服务器上按角色和产品聚合客户端使用数据。 它可帮助 Windows server 管理员量化来自本地服务器上的角色和服务的客户端计算机的请求。  
  
默认情况下，UAL 已安装并处于启用状态，并几乎实时收集数据。 无需任何管理员配置，但是可以禁用或启用 UAL。 有关详细信息，请参阅 [Manage User Access Logging](Manage-User-Access-Logging.md)。 用户访问日志记录服务按照角色和产品将客户端使用数据聚合到本地数据库文件中。  之后，IT 管理员可以使用 Windows Management Instrumentation (WMI) 或 Windows PowerShell cmdlet，按服务器角色（或软件产品）、用户、设备、本地服务器和日期来检索数量和实例。  
  
> [!NOTE]  
> UAL 支持 [Microsoft 评估与计划工具包](https://go.microsoft.com/fwlink/?LinkID=111000)。  
  
## <a name="BKMK_APP"></a>实用应用程序  
UAL 聚合登录到本地数据库的唯一客户端设备和用户请求事件。 然后，这些记录可用于（通过服务器管理员执行的查询）按服务器角色、用户、设备、本地服务器和日期检索数目和实例。  此外，UAL 已扩展为允许非 Microsoft 软件开发人员检测由 Windows Server 聚合的 UAL 事件。  
  
UAL 可以执行以下任务：  
  
-   为本地物理或虚拟服务器量化客户端用户请求。  
  
-   为本地物理服务器或虚拟服务器上安装的软件产品量化客户端用户请求。  
  
-   检索运行 Hyper-V 的本地服务器上的数据，以识别 Hyper-V 虚拟计算机上需求较高和较低的时段。  
  
-   从多个远程服务器中检索 UAL 数据。  
  
此外，软件开发人员还可以检测 UAL 事件，然后使用 WMI 和 Windows PowerShell 界面来聚合和检索这些事件。  
  
UAL 可以支持以下服务器角色和服务：  
  
-   Active Directory 证书服务 (AD CS)  
  
-   Active Directory 权限管理服务 (AD RMS)  
  
-   BranchCache  
  
-   域名系统 (DNS)  
  
    > [!NOTE]  
    > UAL 每隔 24 小时收集一次 DNS 数据，并且存在一个适用于此方案的单独的 UAL cmdlet。  
  
-   动态主机配置协议 (DHCP)  
  
-   传真服务器  
  
-   文件服务  
  
-   文件传输协议 (FTP) 服务器  
  
-   Hyper-V  
  
    > [!NOTE]  
    > UAL 每隔 24 小时收集一次 Hyper-V 数据，并且存在一个适用于此方案的单独的 UAL cmdlet。  
  
-   Web 服务器 (IIS)  
  
    > [!WARNING]  
    > 若要将 UAL 与 IIS 配合使用，必须使用 iisual.exe。 有关详细信息，请参阅 [使用 IIS 用户访问日志记录分析客户端使用数据](http://www.iis.net/learn/manage/configuring-security/analyzing-client-usage-data-with-iis-user-access-logging)。  
  
-   Microsoft 消息队列 (MSMQ) 服务  
  
-   网络策略和访问服务  
  
-   打印和文档服务  
  
-   路由和远程访问服务 (RRAS)  
  
-   Windows 部署服务 (WDS)  
  
-   Windows Server Update Services (WSUS)  
  
> [!IMPORTANT]  
> 不建议在直接连接到 Internet 的服务器（如可访问 Internet 的地址空间中的 Web 服务器），或在极高性能是服务器主要功能的应用场景下（如在 HPC 工作负荷环境中）使用 UAL。 UAL 主要用于小型、中型和企业内部网方案，其中应有高容量，但并不像为定期提供面向 Internet 的流量的部署那么高。  
  
## <a name="BKMK_NEW"></a>重要功能  
下表描述 UAL 的关键功能及其可能值。  
  
|功能|ReplTest1|  
|-----------------|---------|  
|以几乎实时的方式收集和聚合客户端请求事件数据。|最多可以保存三年的数据。 **重要说明：** 管理员需要强制遵守组织的隐私策略和本地法规收集的数据和数据保留期。|  
|通过使用 WMI 或 Windows PowerShell 界面来查询 UAL，以检索本地或远程服务器上的客户端请求数据。|UAL 启用持续的用法数据的单一视图。 服务器和企业管理员可以检索此数据并与业务管理员进行协调，以优化其批量软件许可证的使用。|  
|默认情况下处于启用状态。|服务器管理员不必配置或通过其他方式设置此功能，即可使用所有核心功能并使其正常运行。|  
  
## <a name="data-logged-with-ual"></a>通过 UAL 记录的数据  
如下用户相关数据由 UAL 记录。  
  
|Data|描述|  
|--------|---------------|  
|**UserName**|随附来自已安装角色和产品的 UAL 条目的客户端上的用户名（如适用）。|  
|**ActivityCount**|特定用户已访问某个角色或服务的次数。|  
|**FirstSeen**|用户首次访问某个角色或服务的日期和时间。|  
|**LastSeen**|用户最近一次访问某个角色或服务的日期和时间。|  
|**ProductName**|提供 UAL 数据的软件父产品的名称（如 Windows）。|  
|**Roleguid 关联**|UAL 分配或登记的 GUID，它表示服务器角色或已安装的产品。|  
|**RoleName**|提供 UAL 数据的角色、组件或子产品的名称。 这还与 ProductName 和 RoleGUID 关联。|  
|**TenantIdentifier**|随附 UAL 数据的已安装角色或产品的租户客户端的唯一 GUID（如适用）。|  
  
如下设备相关数据由 UAL 记录。  
  
|Data|描述|  
|--------|---------------|  
|**地址**|用于访问角色或服务的客户端设备的 IP 地址。|  
|**ActivityCount**|特定设备已访问角色或服务的次数。|  
|**FirstSeen**|IP 地址首次用于访问某个角色或服务时的日期和时间。|  
|**LastSeen**|IP 地址最近一次用于访问某个角色或服务时的日期和时间。|  
|**ProductName**|提供 UAL 数据的软件父产品的名称（如 Windows）。|  
|**Roleguid 关联**|由 UAL 分配或登记的 GUID，它表示服务器角色或已安装的产品。|  
|**RoleName**|提供 UAL 数据的角色、组件或子产品的名称。 这还与 ProductName 和 RoleGUID 关联。|  
|**TenantIdentifier**|随附 UAL 数据的已安装角色或产品的租户客户端的唯一 GUID（如适用）。|  
  
## <a name="BKMK_SOFT"></a>软件要求  
在 Windows Server 2012 之后，可以在运行 Windows Server 版本的任何计算机上使用 UAL。  
  
## <a name="see-also"></a>请参阅  
MSDN 上的[用户访问日志记录](https://msdn.microsoft.com/library/windows/desktop/hh437528(v=vs.85).aspx) 。  
[管理用户访问日志记录](Manage-User-Access-Logging.md)  
  

