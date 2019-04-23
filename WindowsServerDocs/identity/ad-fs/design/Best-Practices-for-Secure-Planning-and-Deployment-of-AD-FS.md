---
ms.assetid: 963a3d37-d5f1-4153-b8d5-2537038863cb
title: AD FS 安全规划和部署的最佳做法
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 67b122353ca9dff3a4df6cbfac56b16bed52b539
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848078"
---
# <a name="best-practices-for-secure-planning-and-deployment-of-ad-fs"></a>AD FS 安全规划和部署的最佳做法

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题提供了最佳实践信息，以便帮助您规划和设计您的 Active Directory 联合身份验证服务 (AD FS) 部署时评估安全。 本主题是检查和评估会影响你的 AD FS 使用的总体安全性的注意事项的起始点。 本主题中的信息是为了补充并扩展现有安全规划和其他设计的最佳实践。  
  
## <a name="core-security-best-practices-for-ad-fs"></a>AD FS 的核心安全最佳实践  
下面的核心最佳实践是普遍适用于所有 AD FS 安装你想要改进或扩展设计或部署的安全性：  
  
-   **使用安全配置向导对联合身份验证服务器和联合服务器代理计算机应用特定于 AD FS 的安全最佳方案**  
  
    安全配置向导 (SCW) 是预装在所有 Windows Server 2008、 Windows Server 2008 R2 和 Windows Server 2012 计算机的工具。 基于你安装的服务器角色，你可以使用该向导来应用安全最佳实践，从而帮助减少服务器攻击面。  
  
    在安装 AD FS 时，安装程序将创建角色扩展文件，你可将其与 SCW 一起使用来创建安全策略，该策略将应用到在安装期间选择的特定 AD FS 服务器角色（联合服务器或联合服务器代理）。  
  
    每个已安装的角色扩展文件表示每台计算机配置的角色和子角色的类型。 以下角色扩展文件安装在 C:WindowsADFSScw 目录中：  
  
    -   Farm.xml  
  
    -   SQLFarm.xml  
  
    -   StandAlone.xml  
  
    -   Proxy.xml（此文件仅在将计算机配置为联合服务器代理角色时存在。)  
  
    若要应用 SCW 中的 AD FS 角色扩展，请按顺序完成以下步骤：  
  
    1.  安装 AD FS，并为该计算机选择相应的服务器角色。 有关详细信息，请参阅[安装联合身份验证服务代理角色服务](../../ad-fs/deployment/Install-the-Federation-Service-Proxy-Role-Service.md)AD FS 部署指南中。  
  
    2.  使用 Scwcmd 命令行工具注册相应的角色扩展文件。 请参阅下表获取有关在你的计算机配置的角色中使用此工具的详细信息。  
  
    3.  验证命令成功完成，可以通过检查 SCWRegister_log.xml 文件，位于 WindowssecurityMsscwLogs 目录中。  
  
    你必须在你希望对其应用 AD FS（基于 SCW 安全策略）的每个联合服务器或联合服务器代理计算机上执行所有这些步骤。  
  
    下表介绍了如何基于你在安装 AD FS 的计算机上选择的 AD FS 服务器角色，注册相应的 SCW 角色扩展。  
  
    |AD FS 服务器角色|使用的 AD FS 配置数据库|在命令提示符下键入以下命令：|  
    |---------------------|-------------------------------------|---------------------------------------------------|  
    |独立联合服务器|Windows 内部数据库|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwStandAlone.xml"`|  
    |加入场的联合服务器|Windows 内部数据库|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwFarm.xml"`|  
    |加入场的联合服务器|SQL Server|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwSQLFarm.xml"`|  
    |联合服务器代理|不可用|`scwcmd register /kbname:ADFS2Standalone /kbfile:"WindowsADFSscwProxy.xml"`|  
  
    有关可以用于 AD FS 的数据库的详细信息，请参阅 [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
-   **在其中安全是非常重要的问题，例如使用展台时的情况下使用令牌重放检测。**  
    令牌重放检测是一项功能的 AD FS 的可确保检测到任何重播令牌请求的联合身份验证服务所做的尝试，并请求将被丢弃。 默认情况下启用了令牌重放检测。 它通过确保永远不会多次使用同一个令牌，而同时适用于 WS 联合身份验证被动配置文件和安全声明标记语言 (SAML) WebSSO 配置文件。  
  
    当联合身份验证服务启动时，它开始构建任何完成的令牌请求的缓存。 针对联合身份验证服务，随着时间推移，并随着后续的令牌请求添加到缓存中，检测任何重播令牌请求的多次尝试的能力将会提升。 如果禁用令牌重放检测并在以后选择将它再次启用，请记住联合身份验证服务将仍然会在一个时间段内接受以前可能已使用的令牌，直到允许重播缓存有足够的时间来重建其内容。 有关详细信息，请参阅 [AD FS 配置数据库的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)。  
  
-   **使用令牌加密，尤其是当你使用支持 SAML 项目解析。**  
  
    强烈建议使用令牌加密以提高安全性并针对潜在拦截 (MITM) 攻击的保护可能会尝试对 AD FS 部署。 使用加密可能会对整体稍有影响，但通常情况下，并不会经常注意到这种影响，并且在许多部署中，更高的安全性优势会超过服务器性能提升所需的任何成本。  
  
    若要启用令牌加密，请首先向你的信赖方信任添加加密证书。 你可以在创建信赖方信任时配置加密证书，也可以在以后添加。 若要将加密证书添加到现有信赖方信任的更高版本，您可以设置使用的证书**加密**在信任属性，同时使用 AD FS 管理单元中的选项卡。 若要指定现有的信任使用 AD FS cmdlet 的证书，使用的 EncryptionCertificate 参数**集 ClaimsProviderTrust**或**Set-relyingpartytrust** cmdlet。 若要设置联合身份验证服务解密令牌时要使用的证书，请使用**Set-adfscertificate** cmdlet 并指定"`Token-Encryption`"的*CertificateType*参数。 为特定的信赖方信任启用和禁用加密可以通过使用 *Set-RelyingPartyTrust* cmdlet 的 **EncryptClaims** 参数来实现。  
  
-   **利用身份验证扩展的保护**  
  
    若要帮助保护你的部署，可以设置并使用与 AD FS 身份验证功能的扩展的保护。 此设置指定的联合身份验证服务器支持的身份验证的扩展保护级别。  
  
    身份验证的扩展保护有助于保护计算机免受中间人 (MITM) 攻击，其中攻击者会截获客户端凭据，并将它们转发到服务器。 可以通过通道绑定令牌 (CBT) 实现针对此类攻击的保护，当服务器建立与客户端的通信时，服务器可要求、允许或不要求该令牌。  
  
    若要启用扩展保护功能，请使用 **Set-ADFSProperties** cmdlet 上的 **ExtendedProtectionTokenCheck** 参数。 下表描述了用于此设置的可能值，以及这些值提供的安全级别。  
  
    |参数值|安全级别|保护设置|  
    |-------------------|------------------|----------------------|  
    |要求|服务器完全强化。|强制执行并且始终要求扩展保护。|  
    |允许|服务器部分强化。|在所涉及的系统已修补为支持扩展保护的情况下，强制执行扩展保护。|  
    |无|服务器易受攻击。|不强制执行扩展保护。|  
  
-   **如果使用日志记录和跟踪，请确保任何敏感信息的隐私。**  
  
    AD FS 不，默认情况下公开或作为联合身份验证服务或正常操作的一部分直接跟踪个人可识别信息 (PII)。 当在 AD FS 中启用了事件日志记录和调试跟踪日志记录时，但是，具体取决于配置某些声明的声明策略类型和其关联的值可能包含可能在 AD FS 事件或跟踪日志中记录的 PII。  
  
    因此，强烈建议强制执行上的 AD FS 配置和其日志文件的访问控制。 如果不希望这种类型的信息始终可见，应在与他人共享之前禁用登录，或筛选出日志中的任何 PII 或敏感数据。  
  
    以下提示可以帮助你防止无意中泄露日志文件的内容：  
  
    -   请确保 AD FS 事件日志和跟踪日志文件受访问控制列表 (ACL) 限制为仅需要对其进行访问这些受信任管理员的访问。  
  
    -   不要使用可通过 Web 请求轻松处理的文件扩展名或路径复制或存档日志文件。 例如，.xml 文件扩展名不是安全的选择。 你可以查阅 Internet 信息服务 (IIS) 管理指南，以查看可使用的扩展名列表。  
  
    -   如果要修改日志文件的路径，请确保为日志文件的位置指定绝对路径，该路径应该在 Web 主机虚拟根 (vroot) 公用目录以外，以防止使用 Web 浏览器的外部方访问它。  

-   **AD FS Extranet 软锁定和 AD FS Extranet 的智能锁定保护**  
    
    发生在使用 Web 应用程序代理通过 invalid(bad) 密码的身份验证请求的窗体中的攻击，可使用 AD FS extranet 锁定保护你的用户从 AD FS 帐户锁定。 除了从 AD FS 保护你的用户帐户锁定，AD FS extranet 锁定还有助于防止暴力密码猜测攻击。  
    
    适用于 Windows Server 2012 R2 上的 AD FS 的 Extranet 软锁定，请参阅[AD FS Extranet 软锁定保护](../../ad-fs/operations/Configure-AD-FS-Extranet-Soft-Lockout-Protection.md)。  

     适用于 Windows Server 2016 上的 AD FS 的 Extranet 智能锁定，请参阅[AD FS Extranet 智能锁定保护](../../ad-fs/operations/Configure-AD-FS-Extranet-Smart-Lockout-Protection.md)。  
  
## <a name="sql-serverspecific-security-best-practices-for-ad-fs"></a>AD FS 的特定于 SQL 服务器的安全最佳实践  
当这些数据库技术用于管理 AD FS 设计和部署中的数据时，下列最佳安全方案是特定于使用 Microsoft SQL Server® 或 Windows 内部数据库 (WID)。  
  
> [!NOTE]  
> 这些建议旨在扩展但不能替代 SQL Server 产品安全指南。 有关规划安全 SQL Server 安装的详细信息，请参阅[安全 SQL 安装的安全注意事项](https://go.microsoft.com/fwlink/?LinkID=139831)(https://go.microsoft.com/fwlink/?LinkID=139831)。  
  
-   **始终在物理上安全网络环境中防火墙后面部署 SQL Server。**  
  
    SQL Server 安装应永远不直接公开到 Internet 上。 数据中心中的计算机应能够访问 SQL server 安装，支持的 AD FS。 有关详细信息，请参阅[安全最佳实践清单](https://go.microsoft.com/fwlink/?LinkID=189229)(https://go.microsoft.com/fwlink/?LinkID=189229)。  
  
-   **而不是使用内置的默认系统服务帐户为服务帐户下运行 SQL Server。**  
  
    默认情况下，经常将 SQL Server 安装和配置为使用一个受支持的内置系统帐户，例如 LocalSystem 或 NetworkService 帐户。 若要增强 AD FS 的 SQL Server 安装安全性，尽可能使用一个单独的任何位置访问你的 SQL Server 服务的服务帐户和通过注册此帐户中的安全主体名称 (SPN) 来启用 Kerberos 身份验证应用Active Directory 部署。 这样可以使客户端和服务器之间进行相互身份验证。 SQL Server 将使用基于 Windows 的 NTLM 身份验证，而无需单独的服务帐户的 SPN 注册，在 NTLM 身份验证中只有客户端进行身份验证。  
  
-   **最小化 SQL Server 的图面区域。**  
  
    只启用那些必需的 SQL Server 终结点。 默认情况下，SQL Server 提供了一个不能删除的内置 TCP 终结点。 对于 AD FS，则应启用 Kerberos 身份验证此 TCP 终结点。 若要查看当前的 TCP 终结点，以查看其他用户定义的 TCP 端口是否已添加到 SQL 安装，可以在 Transact-SQL (T-SQL) 会话中使用“SELECT * FROM sys.tcp_endpoints”查询语句。 有关 SQL Server 终结点配置的详细信息，请参阅[How To:将数据库引擎配置为侦听多个 TCP 端口](https://go.microsoft.com/fwlink/?LinkID=189231)(https://go.microsoft.com/fwlink/?LinkID=189231)。  
  
-   **避免使用基于 SQL 的身份验证。**  
  
    若要避免通过网络以明文形式传输密码或将密码存储在配置设置中，请仅对你的 SQL Server 安装使用 Windows 身份验证。 SQL Server 身份验证是一种传统的身份验证模式。 当你使用 SQL Server 身份验证时，不建议存储结构化查询语言 (SQL) 登录凭据（SQL 用户名和密码）。 有关详细信息，请参阅[身份验证模式](https://go.microsoft.com/fwlink/?LinkID=189232)(https://go.microsoft.com/fwlink/?LinkID=189232)。  
  
-   **仔细评估对 SQL 安装中的其他通道安全需求。**  
  
    实际上，即使使用 Kerberos 身份验证，但 SQL Server 安全支持提供程序接口 (SSPI) 并不提供通道级别安全。 但是，对于服务器安全地位于防火墙保护的网络中的安装，可能并不需要加密 SQL 通信。  
  
    虽然加密是有助于确保安全性的有价值的工具，但不应该将它用于所有数据或连接。 在决定是否要执行加密时，请考虑用户访问数据的方式。 如果用户通过公用网络访问数据，可能会要求数据加密来提高安全性。 但是，如果所有的 AD fs 的 SQL 数据访问涉及到安全的 intranet 配置，加密可能不需要。 任何加密的使用还应包括密码、密钥和证书的维护策略。  
  
    如果需要考虑任何 SQL 数据可能会通过网络被看到或被篡改，则使用 Internet 协议安全 (IPsec) 或安全套接字层 (SSL) 来帮助确保 SQL 连接的安全。 但是，这可能会产生负面影响 SQL Server 性能，这可能会影响或限制在某些情况下 AD FS 性能。 例如，在令牌颁发中的 AD FS 性能可能会降低时来自基于 SQL 的属性存储的属性查找对令牌颁发至关重要。 你可以通过强大的外围安全配置更好地消除 SQL 篡改威胁。 例如，用于保护 SQL Server 安装的更好的解决方案是确保它将始终无法访问 Internet 用户和计算机，并且始终只能由数据中心环境中的用户或计算机对它进行访问。  
  
    有关详细信息，请参阅[加密与 SQL Server 的连接](https://go.microsoft.com/fwlink/?LinkID=189234)或[SQL Server 加密](https://go.microsoft.com/fwlink/?LinkID=189233)。  
  
-   **通过使用存储的过程来通过 AD FS 的 SQL 存储的数据执行所有基于 SQL 的查找配置安全设计的访问。**  
  
    若要提供更好的服务和数据隔离，可以创建所有属性存储查找命令的存储过程。 你可以创建数据库角色，然后向其授予运行存储过程的权限。 AD FS Windows 服务的服务标识分配到此数据库角色。 AD FS Windows 服务应无法运行任何其他 SQL 语句，用于属性查找的相应存储过程之外。 以这种方式锁定对 SQL Server 数据库的访问会降低特权提升攻击的风险。  
  
## <a name="see-also"></a>请参阅
[在 Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
