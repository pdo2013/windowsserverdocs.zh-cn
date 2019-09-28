---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: AD FS 配置数据库的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 22047a93ab67d3f21b3e2318fcce497feab8f996
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385580"
---
# <a name="the-role-of-the-ad-fs-configuration-database"></a>AD FS 配置数据库的角色
AD FS 配置数据库存储所有表示单个实例的配置数据 Active Directory 联合身份验证服务 \(AD FS @ @no__t no__t 2that 是，联合身份验证服务 @ no__t。 AD FS 配置数据库定义联合身份验证服务识别伙伴、证书、属性存储、声明和有关这些相关联实体的各种数据所需的参数集。 你可以将此配置数据存储在 Windows Server®2008、Windows Server 2008 R2 和 Windows server®2012附带的 Microsoft SQL Server®数据库或 Windows 内部数据库 \) 功能中。  
  
> [!NOTE]  
> AD FS 配置数据库的全部内容可以存储在 WID 实例中或 SQL 数据库的实例中，但不能同时存储在这两者中。 这意味着你无法对 AD FS 配置数据库的同一个实例拥有某些使用 WID 的联合服务器和其他使用 SQL Server 数据库的服务器。  
  
你可以使用本主题中的以下信息以及 [AD FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)中提供的内容了解有关选择 WID 或 SQL Server 存储 AD FS 配置数据库的优缺点：  
  
WID 使用关系数据存储，并且没有其自己的管理用户界面 \(UI @ no__t-1。 相反，管理员可以通过使用 AD FS 管理 snap @ no__t-0in、Fsconfig.exe 或 Windows PowerShell™ cmdlet 来修改 AD FS 配置数据库的内容。  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>使用 WID 存储 AD FS 配置数据库  
可以使用 Fsconfig.exe 命令 @ no__t-0line 工具或 AD FS 联合服务器配置向导，将 WID 作为存储来创建 AD FS 配置数据库。 当你使用这些工具中的任何一个时，可以选择以下任一选项来创建你的联合服务器拓扑结构。 上述的每个选项将 WID 用于存储 AD FS 配置数据库：  
  
-   创建备用 @ no__t-0alone 联合服务器  
  
-   在联合服务器场中创建第一个联合服务器  
  
-   将联合服务器添加到联合服务器场  
  
如果选择 "no__t-0alone" 选项，则会使用 WID 存储 AD FS 配置数据库的单个实例。 不能跨多个联合服务器共享此实例。 它仅适用于测试实验室环境。 有关 "备用 @ no__t-0alone 联合服务器" 选项或如何设置一个选项的详细信息，请参阅[使用 WID 的独立联合服务器](https://technet.microsoft.com/library/gg982486.aspx)或[创建独立的联合服务器](https://technet.microsoft.com/library/ee913579.aspx)。  
  
如果你在联合服务器场选项中选择第一个联合服务器，则 WID 配置的可伸缩性允许稍后将其他联合服务器添加到服务器场。 有关部署 WID 场或如何设置其中一个服务器的详细信息，请参阅[使用 WID 的联合服务器场](https://technet.microsoft.com/library/gg982492.aspx)或[在联合服务器场中创建第一个联合服务器](https://technet.microsoft.com/library/dd807070.aspx)  
  
如果你选择“添加联合服务器”选项，则 WID 配置为按设置的间隔将配置数据库的更改复制到新的联合服务器。 有关将联合服务器添加到 WID 场中的详细信息，请参阅[使用 WID 的联合服务器场](https://technet.microsoft.com/library/gg982492.aspx)或[将联合服务器添加到联合服务器场](https://technet.microsoft.com/library/ee913575.aspx)。  
  
> [!NOTE]  
> 当你使用 WID 部署联合服务器场时，AD FS 的某些功能可能不可用。 若要在配置服务器场时访问完整的功能集，请考虑改用 Microsoft SQL Server 以存储 AD FS 配置数据库。 有关详细信息，请参阅 [AD FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx)。  
  
### <a name="how-a-wid-federation-server-farm-works"></a>WID 联合服务器场的工作原理  
本部分介绍一些重要概念，这些概念描述 WID 联合服务器场如何在主联合服务器和辅助联合服务器之间复制数据。 .  
  
#### <a name="primary-federation-server"></a>主联合服务器  
主联合服务器是运行 Windows Server 2008、Windows Server 2008 R2 或 Windows Server®2012的计算机，该计算机已使用 AD FS 联合服务器配置向导在联合服务器角色中进行配置，并且具有 AD FS 的读/写副本。配置数据库。 当你使用 AD FS 联合服务器配置向导并选择创建新联合身份验证服务的选项并使该计算机成为服务器场中的第一台联合服务器时，将始终创建主联合服务器。 所有在此服务器场中的其他联合服务器，也称为辅助联合服务器，必须将主联合服务器所做的更改同步到存储在本地的 AD FS 配置数据库的副本。  
  
#### <a name="secondary-federation-servers"></a>辅助联合服务器  
"辅助联合服务器" 从主联合服务器存储 AD FS 配置数据库的副本，但这些副本是 read @ no__t-0only。 辅助联合服务器通过定期轮询来连接到场中的主联合服务器并与之同步数据，以检查数据是否已更改。 辅助联合服务器存在，可为主联合服务器提供容错，同时可用于在整个网络环境中的不同站点中发出 @ no__t-0balance 访问请求。  
  
> [!NOTE]  
> 如果主联合服务器崩溃，并且处于脱机状态，则所有辅助联合服务器将继续按常规处理请求。 但是，无法对联合身份验证服务进行任何新的更改，直到主联合服务器重新恢复联机。 此外你可以通过使用 Windows PowerShell 指定辅助联合服务器成为主联合服务器。 有关详细信息，请参阅[借助 Windows PowerShell 的 AD FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>AD FS 配置数据库的同步方式  
由于 AD FS 配置数据库扮演着重要的角色，因此它在网络中的所有联合服务器上可用，以便在处理请求 @no__t 1when 网络负载 @ no_ 时提供容错和 load @ no__t-0balancing 功能_t-2balancers 用于 @ no__t。 但是，为了使辅助联合服务器执行该功能，必须同步存储在主联合服务器上的 AD FS 配置数据库。  
  
当你将联合服务器添加到场中时，新计算机将成为连接到主联合服务器的辅助联合服务器，以复制 AD FS 配置数据库的副本。 从这一刻起，新的联合服务器继续从主联合服务器上定期提取更新，如下图中所示。  
  
![AD FS 角色](media/adfs2_WID.png)  
  
每个辅助联合服务器每五分钟向主联合服务器轮询更改。 可以使用 Windows PowerShell cmdlet 调整此默认值 5 @ no__t-0minute 值或强制立即同步。 有关如何执行此操作的详细信息，请参阅[借助 Windows PowerShell 的 AD FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
WID 同步过程还支持用于中间更改的更高效传输的增量传输。 增量传输过程需要在网络上大大降低流量，这样传输将完成得更快。  
  
> [!NOTE]  
> 支持从 WID 到 SQL Server 实例的 AD FS 配置数据库的迁移。 有关如何执行此操作的详细信息，请参阅 [AD FS：将 AD FS 配置数据库迁移到 TechNet Wiki 网站上 SQL Server @ no__t-0。  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>使用 SQL Server 存储 AD FS 配置数据库  
您可以使用 Fsconfig.exe command @ no__t-0line 工具将单个 SQL Server 数据库实例用作存储区来创建 AD FS 配置数据库。 使用 SQL Server 数据库作为 AD FS 配置数据库，通过 WID 提供以下优势：  
  
-   管理员可以利用 SQL Server 的高可用性功能  
  
-   它为高流量提供额外的性能增加。  
  
-   它提供 SAML 项目解析和 SAML/WS @ no__t-0Federation 令牌重播检测 \(described 低于 @ no__t-2 的功能支持。  
  
当 AD FS 配置数据库存储在 SQL 数据库实例中时，术语“主联合服务器”不适用，因为所有联合服务器可以平等地读取和写入到 AD FS 配置数据库，该库使用相同群集的 SQL Server 实例，如下图所示。  
  
![AD FS 角色](media/adfs2_SQL.png)  
  
你可以使用 SQL Server 将两个或多个服务器配置为作为服务器群集一起工作，以确保将 AD FS 高度提供给传入的客户端请求。 高可用性提供一个可扩展的 @ no__t-0out 体系结构，在该体系结构中，可以通过添加更多服务器来增加服务器容量。 通过自动群集故障转移缓解单点故障。  
  
可以通过使用 SQL 群集技术所提供的网络负载 @ no__t-0balancing 和故障转移服务来实现高可用性。 有关如何配置 SQL Server 以实现高可用性的详细信息，请参阅[高可用性解决方案概述](https://go.microsoft.com/fwlink/?LinkId=179853)。  
  
### <a name="saml-artifact-resolution"></a>SAML 项目解析  
安全断言标记语言 @no__t 0SAML @ no__t）项目解析是基于 SAML 2.0 协议部分的终结点，该终结点描述了依赖方如何直接从声明提供程序检索令牌。 在解析过程的第一个阶段，浏览器客户端与资源联合服务器联系，并为其提供一个项目。 在第二个阶段，资源联合服务器将项目发送到 SAML 项目终结点 URL，为了解析该项目消息，该 URL 托管于帐户伙伴组织中的某个位置。 在最后阶段，帐户联合服务器代表浏览器客户端将令牌颁发给联合服务器。  
  
> [!NOTE]  
> 如果你是帐户伙伴组织中的管理员，请确保将 SSL 证书分配或绑定到 IIS 中的联合被动网站的 SSL 证书（该证书链接到 Windows 根证书程序成员的根证书） \( @ no__t-1 @ no__2Sites @ no__t-3Default Web Site @ no__t-4adfs @ no__t-5ls @ no__t （在服务器场中的所有帐户联合服务器上）。 若要避免资源联合服务器通过手动将 SSL 证书添加到本地计算机受信任人证书存储，或无法解析你的组织中发布的项目，这样做是非常重要的。  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML/WS 联合身份验证令牌重放检测  
术语*令牌重放*是指在帐户伙伴组织中的浏览器客户端尝试将其从帐户联合服务器收到的同一个令牌进行多次发送，以对资源联合服务器进行身份验证的操作。  在用户单击他们浏览器的“返回”按钮时，会执行此操作以尝试重新提交身份验证页面。  
  
AD FS 提供称为*令牌重放检测*的功能，通过此功能，可检测使用相同令牌的多个令牌请求，然后丢弃这些令牌。 启用此功能后，令牌重播检测通过确保从未多次使用同一个令牌，来保护 WS @ no__t-0Federation 被动配置文件和 SAML WebSSO 配置文件中的身份验证请求的完整性。 应在对安全有很高要求的情况下启用此功能，例如使用展台时。  
  
在展台示例中，用户可以从所有网站注销，并且以后恶意用户可以尝试使用浏览器历史记录，以便重新提交前一个用户加载的联合身份验证页面。 此功能通过存储有关每个由帐户伙伴组织成功完成的身份验证的其他信息，以便检测后续令牌的重放，并防止来自已成功验证的多个身份验证尝试，从而缓解这一问题。  
  

