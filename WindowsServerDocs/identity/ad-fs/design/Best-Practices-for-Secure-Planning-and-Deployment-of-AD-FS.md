---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: "安全规划和部署广告 FS 的最佳实践"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ed8c36d4bec455879ffd00ad40b72fd5e90484ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>安全规划和部署广告 FS 的最佳实践

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题提供最佳做法信息，以帮助你在规划和时设计 Active Directory 联合身份验证服务 (广告 FS) 部署评估安全。 本主题介绍了查看和评估影响广告 FS 您使用的整体安全的注意事项的起点。 本主题中的信息是为了补充并延长你现有的安全规划和其他设计的最佳实践。  
  
## <a name="core-security-best-practices-for-ad-fs"></a>广告 FS 的核心安全最佳做法  
以下 core 最佳做法是常见所有广告 FS 安装到你希望如何改进或扩展设计或部署安全性：  
  
-   **使用安全配置向导适用于联合身份验证的服务器和联盟服务器代理计算机的广告特定于 FS 安全的最佳实践**  
  
    安全配置向导 (SCW) 是在所有 Windows Server 2008、 Windows Server 2008 R2 和 Windows Server 2012 的计算机附带预安装的工具。 可用于应用，可帮助的最佳实践减少服务器，基于你安装了 server 角色攻击 surface 的安全。  
  
    当你安装广告 FS 时，安装程序创建扩展文件，可用于与 SCW 创建将适用于特定广告 FS server 角色 （联合身份验证的服务器或联合身份验证的服务器代理），在设置期间选择的安全策略角色。  
  
    安装每个角色扩展文件表示的角色和子每台计算机配置为其的类型。 以下角色扩展文件安装在 C:WindowsADFSScw 目录：  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml （此文件才会显示你已将计算机配置以联合身份验证的服务器代理角色。）  
  
    若要将在 SCW 广告 FS 角色扩展的应用，请完成订单中的以下步骤操作：  
  
    1.  安装广告 FS，然后选择该计算机的相应服务器角色。 有关详细信息，请参阅[安装联合身份验证服务代理角色服务](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md)广告 FS 部署指南中。  
  
    2.  注册使用 Scwcmd 命令行工具相应角色扩展文件。 请参阅下表有关角色配置你的计算机中使用此工具的详细信息。  
  
    3.  验证命令已成功完成通过检查位于 WindowssecurityMsscwLogs 目录 SCWRegister_log.xml 文件。  
  
    在每个联盟服务器或服务器联合身份验证的代理计算机你希望应用广告基于 FS – SCW 安全策略，必须执行以下所有步骤。  
  
    下表介绍了如何注册相应 SCW 角色扩展，根据你选择在装有广告 FS 的计算机的广告 FS server 角色。  
  
    |广告 FS 服务器角色|使用广告 FS 配置数据库|在命令提示符中键入以下命令：|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |独立联合身份验证的服务器|Windows 内部数据库|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |加入农场里的联合身份验证的服务器|Windows 内部数据库|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |加入农场里的联合身份验证的服务器|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |联合身份验证的服务器代理服务器|不适用|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    你可以使用与广告 FS 数据库有关详细信息，请参阅[广告 FS 配置数据库中的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
-   **在安全处于非常重要的考虑因素，例如，当使用柜台的情况下使用令牌重播检测。**  
    标记重播检测是的以确保检测到任何尝试重播对联合身份验证服务令牌请求时，并请求被丢弃广告 FS 一项功能。 默认情况下，启用了令牌重播检测。 它通过确保永远不会多次使用同一标记适用于 WS 联盟被动配置文件和安全声明标记语言 (SAML) WebSSO 配置文件。  
  
    联合身份验证服务启动时，它将开始生成的缓存的通过它来完成任何令牌请求。 随着时间的推移后续令牌请求添加到缓存，能够检测到任何尝试重播令牌请求多次联合身份验证服务用于提高。 如果你禁用令牌重播检测并以后选择要重新启用，请记住联合身份验证服务用于一段时间，可能已使用之前，直到重播缓存已经允许足够时间重新生成它的内容将仍接受标记。 有关详细信息，请参阅[广告 FS 配置数据库中的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
-   **使用令牌加密，，尤其是在你使用的支持 SAML 项目分辨率。**  
  
    强烈建议加密的标记，以增加安全性，并保护潜在可能对您的广告 FS 部署尝试的男子在中间 (MITM) 攻击。 使用使用加密可能对轻微影响整个但一般情况下，它应该不会通常会注意到，在许多部署权益更高的安全性不超过服务器性能在任何费用。  
  
    若要启用令牌加密，第一组添加了对你依赖的方信任加密证书。 您可以配置加密证书，无论是在创建依赖方信任或更高版本。 若要将加密证书稍后添加到现有信赖方信任，可以设置使用证书上**加密**信任属性时使用广告 FS 内的选项卡。 若要指定的现有信任使用广告 FS cmdlet 证书，请使用的 EncryptionCertificate 参数**组 ClaimsProviderTrust**或**组 RelyingPartyTrust** cmdlet。 若要设置为联合身份验证服务用于解密标记的证书，使用**组 ADFSCertificate** cmdlet 指定和"`Token-Encryption`"为*CertificateType*参数。 启用和禁用特定信赖方的加密信任可以通过使用完成*EncryptClaims*参数**组 RelyingPartyTrust** cmdlet。  
  
-   **使用扩展的验证保护**  
  
    若要帮助保护你的部署，可以设置，并扩展的保护用于与广告 FS 身份验证功能。 此设置指定外延支持的联合服务器身份验证的保护程度。  
  
    身份验证外延的保护有助于防止男子在中间 (MITM) 攻击攻击拦截客户凭据，并将它们转发给服务器。 此类攻击防御可通过信道有约束力的标记 (CBT) 以将需要、 允许，或不需要通过服务器时它建立与客户通信。  
  
    若要启用扩展的保护功能，请使用**ExtendedProtectionTokenCheck**上的参数**组 ADFSProperties** cmdlet。 此设置和的值提供的安全级别可能值如下表所述。  
  
    |参数值|安全级别|保护设置|  
    |-------------------|------------------|----------------------|  
    |需要|服务器是完全强化。|扩展的保护强制执行，并一直要求。|  
    |允许|服务器是强化部分。|扩展的保护强制修补已系统涉及为支持它。|  
    |无|服务器很容易。|不能强制扩展的保护。|  
  
-   **如果你使用的日志记录并跟踪，请确保任何敏感信息的隐私。**  
  
    广告 FS 不会默认情况下，公开或跟踪直接作为联合身份验证服务或正常操作的一部分的个人身份信息 (PII)。 当广告 FS 中启用了事件日志记录调试跟踪日志记录时，但是，具体取决于你配置某些索赔的索赔策略类型和其关联的值可能包含 PII 可能会在广告 FS 事件或跟踪日志记录。  
  
    因此，强烈建议强制访问控制广告 FS 配置和其日志文件。 如果你不希望此类信息为相互可见，应禁用 loggin，或在你日志筛选出任何 PII 或敏感数据，然后与其他人共享它们。  
  
    以下使用技巧可以有助于防止无意中在公开日志文件的内容：  
  
    -   确保广告 FS 事件日志和跟踪日志文件受限制只需要访问这些那些受信任管理员访问的访问权限控制列表 (ACL)。  
  
    -   复制或存档使用文件扩展名或，可使用 Web 请求轻松地处理路径日志文件。 例如，.xml 文件扩展名不安全的选择。 你可以检查 Internet 信息服务 (IIS) 管理指南，查看可以提供的扩展的列表。  
  
    -   如果修订本日志文件路径，请务必指定绝对日志文件的位置，这应该 Web 主机虚拟根 (vroot) 公共目录若要防止它正在通过外部方使用 Web 浏览器中访问之外的路径。  

-   **广告 FS 联网锁定保护**  
    
    在遇到攻击形式的身份验证 invalid(bad) 密码，通过 Web 应用程序代理服务器随使用的请求，广告 FS 联网锁定可以让你从广告 FS 帐户锁定保护你的用户。 除了从广告 FS 保护你的用户帐户的锁定，广告 FS 联网锁定也可防止猜测攻击暴力密码。  有关详细信息，请参阅[广告 FS 联网锁定保护](../../ad-fs/operations/Configure-AD-FS-Extranet-Lockout-Protection.md)。  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>SQL Server – 特定安全广告 fs 的最佳实践  
用于管理数据广告 FS 设计和部署在这些数据库技术时会特定于 Microsoft SQL Server® 或内部数据库 Windows (WID) 的使用以下安全的最佳实践。  
  
> [!NOTE]  
> 这些建议旨在扩展，但无法替换 SQL Server 产品的安全指南。 有关规划安全的 SQL Server 安装的详细信息，请参阅[安全安全的 SQL 安装注意事项](https://go.microsoft.com/fwlink/?LinkID=139831)(https://go.microsoft.com/fwlink/?LinkID=139831)。  
  
-   **始终防火墙物理在安全网络环境中部署 SQL Server。**  
  
    SQL Server 安装应永远不会公开直接到 Internet。 应该能够访问您的 SQL server 安装支持广告 FS 仅在您的数据中心中的计算机。 有关详细信息，请参阅[安全的最佳实践清单](https://go.microsoft.com/fwlink/?LinkID=189229)(https://go.microsoft.com/fwlink/?LinkID=189229)。  
  
-   **运行而不是通过使用内置的默认系统 service 帐户帐户服务的 SQL Server。**  
  
    默认情况下，SQL Server 通常安装并配置为使用一个受支持的内置系统帐户时，如本地系统或网络服务的帐户。 以增强安全性的 SQL Server 安装广告 fs，只要可能使用单独的服务帐户来访问你的 SQL Server 服务并启用 Kerberos 身份验证通过 Active Directory 部署中注册安全此帐户的主要名称 (SPN)。 这使客户端和服务器间相互身份验证。 而无需 SPN 注册的单独的服务帐户、 SQL Server 将使用基于 NTLM 适用于 Windows 的身份验证，其中只客户端进行身份验证。  
  
-   **最小化 SQL Server surface 区域。**  
  
    启用仅这些所需的 SQL Server 端点。 默认情况下，SQL Server 提供无法删除单个内置 TCP 端点。 广告 fs 应启用此 Kerberos 身份验证的 TCP 端点。 若要查看当前的 TCP 端点，若要查看其他用户定义 TCP 端口是否将添加到 SQL 安装，你可以使用"选择 * 从 sys.tcp_endpoints"查询事务处理 SQL (T SQL) 会话中的隐私声明。 SQL Server 端点配置的详细信息，请参阅[如何：多 TCP 端口配置倾听数据库引擎](https://go.microsoft.com/fwlink/?LinkID=189231)(https://go.microsoft.com/fwlink/?LinkID=189231)。  
  
-   **避免使用 SQL 基于身份验证。**  
  
    若要避免在你的网络传输清晰的文本的形式的密码或将密码存储在配置设置中使用 Windows 身份验证仅 SQL Server 安装。 身份验证的传统方式 SQL Server 身份验证。 存储结构化查询语言 (SQL) 的登录凭据 （SQL 用户名和密码） 使用时不建议 SQL Server 身份验证。 有关详细信息，请参阅[身份验证模式](https://go.microsoft.com/fwlink/?LinkID=189232)(https://go.microsoft.com/fwlink/?LinkID=189232)。  
  
-   **仔细评估 SQL 安装中的其他频道安全需求。**  
  
    即使使用 Kerberos 身份验证生效，SQL Server 安全支持提供程序接口 (SSPI) 不提供信道级的安全性。 但是，对于在其服务器牢固位于防火墙保护的网络的安装，加密 SQL 通信可能不需要。  
  
    尽管加密是非常有用的工具，以帮助确保安全，但它不应该将视为的所有数据或连接。 当您决定是否要实现加密时，请考虑用户访问数据的方式。 如果用户通过公共网络访问数据，可能需要数据加密增加安全性。 但是，如果 SQL FS 广告的数据的所有访问都涉及安全 intranet 配置，加密可能不需要。 任何使用加密应还包括密码、 键和证书的维护策略。  
  
    如果没有任何 SQL 数据可能会看到或篡改通过你的网络，使用 Internet 协议安全） 或安全套接字层 (SSL) 来帮助保护你的 SQL 连接的关注。 但是，这可能会 SQL Server 性能，这可能会影响或在某些情况下广告 FS 性能受到限制负面影响。 例如，用于令牌发行的关键特性查找从 SQL 基于属性官方商城时，可能会降低令牌发放广告 FS 性能。 你可以更好地消除 SQL 篡改通过强大的外围安全配置时遇到的威胁。 例如，用于保护你的 SQL Server 安装更好的解决方案是确保它是不能访问 Internet 的用户而计算机和，它将保持只能由用户或计算机 datacenter 环境中的访问。  
  
    有关详细信息，请参阅[加密连接到 SQL Server](https://go.microsoft.com/fwlink/?LinkID=189234)或[SQL Server 加密](https://go.microsoft.com/fwlink/?LinkID=189233)。  
  
-   **通过使用存储的过程广告 FS 的 SQL 存储的数据由执行基于 SQL 的查找配置牢固设计的访问。**  
  
    为了提供更好的服务和数据隔离，则可以创建存储的所有特性应用商店查找命令的过程。 你可以创建数据库角色然后的授予权限运行存储的过程。 这一数据库角色指定广告 FS Windows 服务服务的身份。 广告 FS Windows service 应无法运行任何其他 SQL 隐私声明，用于特性查找相应存储过程以外的操作。 锁定访问 SQL Server 数据库这种方式可减少特权提升攻击的风险。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 指导广告 FS 设计](AD-FS-Design-Guide-in-Windows-Server-2012.md)
