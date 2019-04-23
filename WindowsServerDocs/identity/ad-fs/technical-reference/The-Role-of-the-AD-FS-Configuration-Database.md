---
ms.assetid: 68db7f26-d6e3-4e67-859b-80f352e6ab6a
title: AD FS 配置数据库的角色
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3372e1f051ba7f900753a4961d948ddabdef6f4d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867618"
---
>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

# <a name="the-role-of-the-ad-fs-configuration-database"></a>AD FS 配置数据库的角色
AD FS 配置数据库将表示 Active Directory 联合身份验证服务的单个实例的所有配置数据都存储\(AD FS\) \(即，联合身份验证服务\)。 AD FS 配置数据库定义联合身份验证服务识别伙伴、证书、属性存储、声明和有关这些相关联实体的各种数据所需的参数集。 可以在 Microsoft SQL Server® 数据库或 Windows 内部数据库中存储此配置数据\(WID\)随 Windows Server® 2008年、 Windows Server 2008 R2 和 Windows Server® 2012年的功能。  
  
> [!NOTE]  
> AD FS 配置数据库的全部内容可以存储在 WID 实例中或 SQL 数据库的实例中，但不能同时存储在这两者中。 这意味着你无法对 AD FS 配置数据库的同一个实例拥有某些使用 WID 的联合服务器和其他使用 SQL Server 数据库的服务器。  
  
你可以使用本主题中的以下信息以及 [AD FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489.aspx)中提供的内容了解有关选择 WID 或 SQL Server 存储 AD FS 配置数据库的优缺点：  
  
WID 使用关系型数据存储，并不具有其自己的管理用户界面\(UI\)。 相反，管理员可以修改 AD FS 配置数据库的内容，通过使用任一 AD FS 管理管理单元\-中 Fsconfig.exe 或 Windows PowerShell™ cmdlet。  
  
## <a name="using-wid-to-store-the-ad-fs-configuration-database"></a>使用 WID 存储 AD FS 配置数据库  
可以创建 AD FS 配置数据库使用 WID 作为存储通过使用 Fsconfig.exe 命令\-行工具或 AD FS 联合身份验证服务器配置向导。 当你使用这些工具中的任何一个时，可以选择以下任一选项来创建你的联合服务器拓扑结构。 上述的每个选项将 WID 用于存储 AD FS 配置数据库：  
  
-   创建独立\-单独的联合身份验证服务器  
  
-   在联合服务器场中创建第一个联合服务器  
  
-   将联合服务器添加到联合服务器场  
  
如果选择独立\-单独的选项，WID 用于存储 AD FS 配置数据库的单个实例。 不能跨多个联合服务器共享此实例。 它仅适用于测试实验室环境。 详细了解独立\-单独使用联合身份验证服务器选项或如何设置其中一个，请参阅[独立联合身份验证服务器使用 WID](https://technet.microsoft.com/library/gg982486.aspx)或[创建独立联合身份验证服务器](https://technet.microsoft.com/library/ee913579.aspx)。  
  
如果你在联合服务器场选项中选择第一个联合服务器，则 WID 配置的可伸缩性允许稍后将其他联合服务器添加到服务器场。 有关部署 WID 场或如何设置其中一个服务器的详细信息，请参阅[使用 WID 的联合服务器场](https://technet.microsoft.com/library/gg982492.aspx)或[在联合服务器场中创建第一个联合服务器](https://technet.microsoft.com/library/dd807070.aspx)  
  
如果你选择“添加联合服务器”选项，则 WID 配置为按设置的间隔将配置数据库的更改复制到新的联合服务器。 有关将联合服务器添加到 WID 场中的详细信息，请参阅[使用 WID 的联合服务器场](https://technet.microsoft.com/library/gg982492.aspx)或[将联合服务器添加到联合服务器场](https://technet.microsoft.com/library/ee913575.aspx)。  
  
> [!NOTE]  
> 部署联合服务器场使用 WID 时，AD FS 的某些功能可能不可用。 若要在配置服务器场时访问完整的功能集，请考虑改用 Microsoft SQL Server 以存储 AD FS 配置数据库。 有关详细信息，请参阅 [AD FS 部署拓扑注意事项](https://technet.microsoft.com/library/gg982489(v=ws.11).aspx)。  
  
### <a name="how-a-wid-federation-server-farm-works"></a>WID 联合服务器场的工作原理  
本部分介绍一些重要概念，这些概念描述 WID 联合服务器场如何在主联合服务器和辅助联合服务器之间复制数据。 .  
  
#### <a name="primary-federation-server"></a>主联合服务器  
主联合服务器是运行 Windows Server 2008、 Windows Server 2008 R2 或 Windows Server® 2012 的已配置联合身份验证服务器角色与 AD FS 联合身份验证服务器配置向导中，并且具有 AD FS 的读/写副本的计算机配置数据库。 在使用 AD FS 联合身份验证服务器配置向导并选择选项以创建新的联合身份验证服务并使该计算机成为服务器场中的第一个联合身份验证服务器时，将始终创建主联合服务器。 所有在此服务器场中的其他联合服务器，也称为辅助联合服务器，必须将主联合服务器所做的更改同步到存储在本地的 AD FS 配置数据库的副本。  
  
#### <a name="secondary-federation-servers"></a>辅助联合服务器  
辅助联合服务器存储从主联合服务器的 AD FS 配置数据库的副本，但这些副本读取\-仅。 辅助联合服务器通过定期轮询来连接到场中的主联合服务器并与之同步数据，以检查数据是否已更改。 辅助联合服务器存在操作加载时将主联合服务器提供容错能力\-平衡整个网络环境的不同站点中进行的访问请求。  
  
> [!NOTE]  
> 如果主联合服务器崩溃，并且处于脱机状态，则所有辅助联合服务器将继续按常规处理请求。 但是，无法对联合身份验证服务进行任何新的更改，直到主联合服务器重新恢复联机。 此外你可以通过使用 Windows PowerShell 指定辅助联合服务器成为主联合服务器。 有关详细信息，请参阅[借助 Windows PowerShell 的 AD FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
#### <a name="how-the-adfs-configuration-database-is-synchronized"></a>AD FS 配置数据库的同步方式  
由于 AD FS 配置数据库所扮演的重要角色，它可用于在网络中的所有联合服务器上提供容错和负载\-均衡功能，在处理请求时\(时网络负载\-均衡器用于\)。 但是，为了使辅助联合服务器执行该功能，必须同步存储在主联合服务器上的 AD FS 配置数据库。  
  
当你将联合服务器添加到场中时，新计算机将成为连接到主联合服务器的辅助联合服务器，以复制 AD FS 配置数据库的副本。 从这一刻起，新的联合服务器继续从主联合服务器上定期提取更新，如下图中所示。  
  
![AD FS 角色](media/adfs2_WID.png)  
  
每个辅助联合服务器每五分钟向主联合服务器轮询更改。 您可以调整此默认设置五个\-分钟值，或通过使用 Windows PowerShell cmdlet 随时强制进行即时同步。 有关如何执行此操作的详细信息，请参阅[借助 Windows PowerShell 的 AD FS 管理](https://go.microsoft.com/fwlink/?LinkID=179634)。  
  
WID 同步过程还支持用于中间更改的更高效传输的增量传输。 增量传输过程需要在网络上大大降低流量，这样传输将完成得更快。  
  
> [!NOTE]  
> 支持从 WID 到 SQL Server 实例的 AD FS 配置数据库的迁移。 有关如何执行此操作的详细信息，请参阅[AD FS:将你的 AD FS 配置数据库迁移到 SQL Server](https://go.microsoft.com/fwlink/?LinkId=192232) TechNet Wiki 站点上。  
  
## <a name="using-sql-server-to-store-the-ad-fs-configuration-database"></a>使用 SQL Server 存储 AD FS 配置数据库  
可以创建使用 Fsconfig.exe 命令作为存储使用单个 SQL Server 数据库实例的 AD FS 配置数据库\-行工具。 使用 SQL Server 数据库作为 AD FS 配置数据库，通过 WID 提供以下优势：  
  
-   管理员可以利用 SQL Server 的高可用性功能  
  
-   它为高流量提供额外的性能增加。  
  
-   它提供的功能支持的 SAML 项目解析和 SAML/WS\-联合身份验证令牌重放检测\(如下所述\)。  
  
当 AD FS 配置数据库存储在 SQL 数据库实例中时，术语“主联合服务器”不适用，因为所有联合服务器可以平等地读取和写入到 AD FS 配置数据库，该库使用相同群集的 SQL Server 实例，如下图所示。  
  
![AD FS 角色](media/adfs2_SQL.png)  
  
可以使用 SQL Server 配置两个或多个服务器以确保 AD FS 进行为传入客户端请求提供服务高度可用的服务器群集一起工作。 高可用性提供规模\-外扩展体系结构在其中可以通过添加其他服务器提高服务器容量。 通过自动群集故障转移缓解单点故障。  
  
可以通过使用网络负载实现高可用性\-平衡和故障转移 SQL 群集技术所提供的服务。 有关如何配置 SQL Server 的高可用性的详细信息，请参阅[高可用性解决方案概述](https://go.microsoft.com/fwlink/?LinkId=179853)。  
  
### <a name="saml-artifact-resolution"></a>SAML 项目解析  
安全断言标记语言\(SAML\)项目解析是基于部分描述了信赖方如何直接从声明提供方检索令牌的 SAML 2.0 协议的终结点。 在解析过程的第一个阶段，浏览器客户端与资源联合服务器联系，并为其提供一个项目。 在第二个阶段，资源联合服务器将项目发送到 SAML 项目终结点 URL，为了解析该项目消息，该 URL 托管于帐户伙伴组织中的某个位置。 在最后阶段，帐户联合服务器代表浏览器客户端将令牌颁发给联合服务器。  
  
> [!NOTE]  
> 如果您是帐户伙伴组织中的管理员，请确保分配或绑定 SSL 证书，链接到 Windows 根证书程序成员的根证书，在 IIS 中的联合被动 Web 站点\( <ComputerName>\\站点\\Default Web Site\\adfs\\ls\)场中的所有帐户联合身份验证服务器上。 若要避免资源联合服务器通过手动将 SSL 证书添加到本地计算机受信任人证书存储，或无法解析你的组织中发布的项目，这样做是非常重要的。  
  
### <a name="samlws---federation-token-replay-detection"></a>SAML/WS-联合身份验证令牌重放检测  
术语*令牌重放*是指在帐户伙伴组织中的浏览器客户端尝试将其从帐户联合服务器收到的同一个令牌进行多次发送，以对资源联合服务器进行身份验证的操作。  在用户单击他们浏览器的“返回”按钮时，会执行此操作以尝试重新提交身份验证页面。  
  
AD FS 提供称为*令牌重放检测*的功能，通过此功能，可检测使用相同令牌的多个令牌请求，然后丢弃这些令牌。 启用此功能后，令牌重放检测可保护的身份验证请求中这两个 WS 完整性\-联合身份验证被动配置文件和通过确保永远不会多次使用相同的令牌的 SAML WebSSO 配置文件。 应在对安全有很高要求的情况下启用此功能，例如使用展台时。  
  
在展台示例中，用户可以从所有网站注销，并且以后恶意用户可以尝试使用浏览器历史记录，以便重新提交前一个用户加载的联合身份验证页面。 此功能通过存储有关每个由帐户伙伴组织成功完成的身份验证的其他信息，以便检测后续令牌的重放，并防止来自已成功验证的多个身份验证尝试，从而缓解这一问题。  
  

