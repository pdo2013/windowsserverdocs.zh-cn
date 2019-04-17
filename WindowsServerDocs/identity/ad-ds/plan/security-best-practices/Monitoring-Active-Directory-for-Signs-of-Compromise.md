---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: "危害的迹象监视 Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 096f2fa58b9aae53a06bf26c2107eb4cee3aa5d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>危害的迹象监视 Active Directory

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

*法律数字五：Eternal 警戒是安全的价格。* - [10 变的法律的安全管理](https://technet.microsoft.com/library/cc722488.aspx)  
  
监视系统事件日志稳定将任何安全 Active Directory 设计的一个重要组成部分。 许多计算机安全危害可能会发现提前在事件如果受害者制定相应事件日志监视和警告。 独立报告多长时间有支持此结束。 例如，[2009 Verizon 数据违反报告](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf)状态：  
  
"活动的监视和日志分析明显 ineffectiveness 仍然是 enigma 某种程度上。 检测有机会有;调查已注意到的受害者 66%有足够的可用来发现它们已在分析此类资源更认真违反他们日志中证据。"  
  
此缺乏监视活动事件日志许多公司安全 defense 计划中保持一致的弱点。 [2012 Verizon 数据违反报告](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf)，即使屏幕的违规 85%花几周时间才会注意到，84%的受害者的迹象违反他们事件日志中找到。  
  
## <a name="windows-audit-policy"></a>Windows 审核策略  
以下是链接到 Microsoft 官方企业支持博客。 这些博客的内容将提供建议、指南和有关审核，建议将帮助你增强您的 Active Directory 的基础结构的安全并设计审核策略时都有价值的资源。  
  
-   [全球对象访问审核是幻](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx)-介绍称为高级审核策略配置已添加到 Windows 7 和 Windows Server 2008 R2 可让你设置的你想要轻松地审核并不能够脚本和 auditpol.exe 同时处理的数据类型的控件机制。  
  
-   [引入 Windows 2008 审核变化](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx)-引入了审核在 Windows Server 2008 中所做的更改。  
  
-   [冷却审核 Vista 到 2008 年中的技巧](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx)-解释了的 Windows Vista 和 Windows Server 2008，可用于解决问题，或者看到您的环境中发生的有趣审核功能。  
  
-   [审核在 Windows Server 2008 和 Windows Vista 的一站式商店](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx)-包含的功能和 Windows Server 2008 和 Windows Vista 中包含的信息审核编译。  
  
下面的链接提供关于 Windows 审核在 Windows 8 和 Windows Server 2012、改进的信息以及有关广告 DS 信息审核在 Windows Server 2008。  
  
-   [在安全审核新增](https://technet.microsoft.com/library/hh849638.aspx)-提供新的安全审核功能在 Windows 8 和 Windows Server 2012 的概述。  
  
-   [广告 DS 审核 Step-by-Step 指南](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx)-介绍了新的 Active Directory 域服务 (广告 DS) 审核功能在 Windows Server 2008。 它还提供了实现此新功能的过程。  
  
### <a name="windows-audit-categories"></a>Windows 审核类别  
Windows Vista 和 Windows Server 2008 之前, Windows 了仅九事件日志审核策略类别：  
  
-   帐户的登录活动  
  
-   管理帐户  
  
-   目录服务访问  
  
-   登录活动  
  
-   对象访问  
  
-   更改策略  
  
-   使用权限  
  
-   进程的跟踪  
  
-   系统事件  
  
这些九传统审核类别构成审核策略。 每个审核策略类别可以启用的成功、失败，或成功，以及发生故障事件。 在下一步部分包含他们的说明。  
  
#### <a name="audit-policy-category-descriptions"></a>审核策略类别说明  
审核策略类别启用以下事件日志消息类型。  
  
##### <a name="audit-account-logon-events"></a>审核帐户的登录活动  
报告安全主体（例如，用户、的电脑或服务帐户）是登录到或注销从一台计算机使用另一台计算机验证该帐户的每个实例。 域控制器上验证域安全主体帐户后，将生成帐户的登录活动。 身份验证的本地计算机上的本地用户将生成一种登录活动，将在本地安全日志记录。 不帐户注销记录事件。  
  
此类别业务正常期间生成大量"噪点"因为 Windows 不断有帐户登录到和注销本地和远程计算机。 仍，任何安全套餐应包括成功和此审核类别失败。  
  
##### <a name="audit-account-management"></a>审核帐户管理  
此审核设置用于确定是否跟踪用户和组的管理。 例如，用户和组应跟踪时创建、更改或已删除; 用户或计算机帐户、安全性组中或 distribution 组用户或计算机的帐户被重命名、禁用，或启用;或更改用户或计算机的密码。 用户或组所添加或删除其他组中，可生成一个事件。  
  
##### <a name="audit-directory-service-access"></a>审核目录服务访问  

此策略设置确定是否审核安全主要访问 Active Directory 对象的具有自己指定的系统访问控制列表 (SACL)。 一般情况下，仅应在该域控制器上启用此类别。 当启用，此设置生成大量"噪点。"  
  
##### <a name="audit-logon-events"></a>审核登录活动  
登录事件时主体本地安全验证本地计算机上生成。 登录事件记录发生在本地计算机上域登录。 不会生成帐户注销事件。 启用时，登录事件将生成大量"噪点，"，但它们应默认情况下启用，在任何安全审核套餐。  
  
##### <a name="audit-object-access"></a>审核对象访问  
对象访问可生成事件（如打开、读取、重命名、已删除或已关闭）访问与启用审核随后定义的对象时。 主要的审核类别启用后，必须单独的对象将审核启用定义管理员。 审核启用，因此启用此类别会通常开始生成事件之前管理员定义任何附带许多 Windows 系统对象。  
  
此类别是非常"恼人"，并且会产生针对每个对象访问 5 到 10 事件。 将很难管理员新手对象审核获得有用的信息。 它只能在需要时启用。  
  
##### <a name="auditing-policy-change"></a>审核策略更改  
此策略设置用于确定是否审核用户版权分配策略、Windows 防火墙策略、信任策略或更改审核策略更改的每个事件。 应在所有计算机上启用此类别。 这将生成很少噪点。  
  
##### <a name="audit-privilege-use"></a>审核特权使用  

有多种的用户的权利和 Windows（例如，登录作为一批作业和作为操作系统的一部分）中的权限。 此策略设置用于确定是否审核通过正确的用户或特权在执行每个安全主体实例。 启用此类别大量"噪点，"，但结果很有帮助跟踪安全主要帐户使用的提升了权限的权限。  
  
##### <a name="audit-process-tracking"></a>跟踪审核进程  
此策略设置用于确定是否审核详细的过程跟踪事件例如计划激活、过程退出、句柄复制和访问间接对象的信息。 它是用于跟踪恶意用户以及他们使用的程序很有用。  
  
启用跟踪审核进程生成大量的事件，通常是设置为**无审核**。 但是，此设置可以提供在事件开始的进程的详细日志响应很大的作用以及它们在启动的时间。 对于域控制器和其他单角色基础结构服务器，此类别可以安全地打开所有的时间。 一个角色服务器不会产生其关税正常期间跟踪交通多过程。 因此，他们可以启用捕获未经授权的事件，如果发生。  
  
##### <a name="system-events-audit"></a>系统事件审核  

系统事件是几乎通用捕获全部类别，注册影响的计算机、其系统安全性或的安全日志的各种事件。 它包括计算机关机和重启、电源失败，系统时间更改、身份验证程序包初始化、审核日志 clearings、模拟问题和的其他常规事件主机的事件。 一般情况下，启用此审核类别生成大量"噪点，"，但它生成足够非常有用的事件，很难有史以来建议不能启用它。  
  
#### <a name="advanced-audit-policies"></a>高级的审核策略  
Windows Vista 和 Windows Server 2008 从开始，Microsoft 改进了事件日志类别选项可通过创建子类别中的每个主要审核类别下的方式。 子类别中允许审核不是通过使用主要类别否则可能更精确。 通过使用子类别中，你可以启用部分特定主类别中，并跳生成事件具有无需使用你的。 每个审核策略子可以启用的成功、失败，或成功，以及发生故障事件。  
  
若要列表所有可用审核子类别中，查看在组策略对象、的高级审核策略容器，或在运行 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8、Windows 7 或 Windows Vista 的任何计算机上的命令提示符中键入以下命令：  
  
**auditpol//list /subcategory: \ ***  
  
若要获取一台运行 Windows Server 2012、Windows Server 2008 R2 或 Windows 2008 计算机上的当前配置审核子类别列表，请键入以下命令：  
  
**auditpol//get /category: \ ***  
  
下面的屏幕截图显示 auditpol.exe 列出当前审核策略的示例。  
  
![监视广告](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> 组策略不而无法 auditpol.exe 始终准确报告所有启用审核策略的状态。 请参阅[获取有效的审核策略，在 Windows 7 和 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)的更多详细信息。  
  
每个主要类别具有多个子类别。 下面是列表类别，其子类别中，以及他们函数的说明。  
  
### <a name="auditing-subcategories-descriptions"></a>审核子类别中的说明  
审核策略子类别中启用了事件日志以下消息类型：  
  
#### <a name="account-logon"></a>帐户登录  
  
##### <a name="credential-validation"></a>凭据验证  
此子类别报告凭据提交用户帐户登录请求以验证测试的结果。 这些事件发生都凭据权威的计算机上。 对于域帐户，域控制器是权威，而对于本地帐户，在本地计算机是权威。  
  
域环境中的帐户登录事件大多数登录对于域帐户权威的域控制器的安全日志。 但是，这些事件上可能会出现在组织中的其他计算机使用的本地帐户登录。  
  
##### <a name="kerberos-service-ticket-operations"></a>Kerberos 服务票证操作  
此子类别报告 Kerberos 票证请求的是域帐户权威的域控制器上的进程所生成的事件。  
  
##### <a name="kerberos-authentication-service"></a>Kerberos 身份验证服务  
此子类别报告 Kerberos 身份验证服务所生成的事件。 这些事件发生都凭据权威的计算机上。  
  
##### <a name="other-account-logon-events"></a>其他帐户登录活动  
此子类别报告中并非与凭据验证或 Kerberos 门票时都凭据提交用于用户帐户登录请求的响应发生的事件。 这些事件发生都凭据权威的计算机上。 对于域帐户，域控制器是权威，而对于本地帐户，在本地计算机是权威。  
  
域环境中大多数帐户登录事件登录对于域帐户权威的域控制器的安全日志。 但是，这些事件上可能会出现在组织中的其他计算机使用的本地帐户登录。 示例可以包括以下信息：  
  
-   远程桌面服务会话断开连接  
  
-   新的远程桌面服务会话  
  
-   锁定和解锁工作站  
  
-   调用了一个屏幕保护程序  
  
-   消除一个屏幕保护程序  
  
-   检测到 Kerberos 重播攻击，在其中两次收到的 Kerberos 请求使用相同的信息  
  
-   无线网络授予用户或计算机的帐户的访问权限  
  
-   访问有线 802.1 x 授予用户或计算机的帐户中的网络  
  
#### <a name="account-management"></a>管理帐户  
  
##### <a name="user-account-management"></a>用户帐户管理  
此子类别报告的每个事件的用户帐户管理，如的用户帐户已创建、更改或已删除;重命名、禁用或者已启用; 的用户帐户或设置或更改密码。 如果启用此审核策略设置，则管理员可以检测到恶意、意外，和授权的用户帐户创建跟踪事件。  
  
##### <a name="computer-account-management"></a>计算机帐户管理  
此子类别报告的每个事件的计算机帐户管理，如计算机帐户创建、更改、删除、重命名、禁用，或时启用。  
  
##### <a name="security-group-management"></a>安全组管理  
此子类别报告安全组管理，如或时创建、更改或删除安全组成员时添加或删除安全组中的每个事件。 如果启用此审核策略设置，则管理员可以检测到恶意、意外，和授权的安全组帐户创建跟踪事件。  
  
##### <a name="distribution-group-management"></a>Distribution 组管理  
此子类别报告 distribution 组管理，如或时创建、更改或删除 distribution 组成员时添加或删除 distribution 组的每个事件。 如果启用此审核策略设置，则管理员可以检测到恶意、意外，和授权组帐户创建跟踪事件。  
  
##### <a name="application-group-management"></a>应用程序组管理  
此子类别一台电脑，例如当创建、更改或删除的应用程序组或时添加或删除的应用程序组成员报告应用程序组管理每个的事件。 如果启用此审核策略设置，则管理员可以检测到恶意、意外，和授权的应用程序组帐户创建跟踪事件。  
  
##### <a name="other-account-management-events"></a>其他帐户管理事件  
此子类别报告其他帐户管理事件。  
  
#### <a name="detailed-process-tracking"></a>详细的进程的跟踪  
  
##### <a name="process-creation"></a>创建进程  
此子类别报告的创建过程，并且用户或已创建的程序的名称。  
  
##### <a name="process-termination"></a>进程终止  
此子类别报告进程终止时。  
  
##### <a name="dpapi-activity"></a>DPAPI 活动  
此子类别报告加密，或解密连接到的数据保护应用程序编程接口 (DPAPI)。 DPAPI 用于保护机密信息，如存储的密码和信息。  
  
##### <a name="rpc-events"></a>RPC 事件  
此子类别报告远程过程调用 (RPC) 连接事件。  
  
#### <a name="directory-service-access"></a>目录服务访问  
  
##### <a name="directory-service-access"></a>目录服务访问  
访问广告 DS 对象时，此子类别报告。 仅对象配置 Sacl 导致访问它们的方式 SACL 条目匹配时，会生成，并且仅审核事件。 这些事件经过类似于在较早版本的 Windows Server 目录服务访问事件。 此子类别仅适用于域控制器。  
  
##### <a name="directory-service-changes"></a>目录服务更改  
此子类别中广告 DS 的对象报告更改。 报告的更改的类型是创建、修改、移动和删除恢复对象执行的操作。 目录服务更改审核，根据需要，指示所做的更改的对象更改的属性旧和新值。 仅具有 Sacl 对象导致审核事件会生成，并且仅当与其 SACL 条目匹配的方式访问它们。 某些对象和属性不会导致审核事件上的对象类架构中的设置因生成。 此子类别仅适用于域控制器。  
  
##### <a name="directory-service-replication"></a>目录服务复制  
两个域控制器之间复制开始和结束时，此子类别报告。  
  
##### <a name="detailed-directory-service-replication"></a>详细的目录服务复制  
此子类别报告域控制器之间复制的信息的详细的信息。 这些事件可能会在音量非常高。  
  
#### <a name="logonlogoff"></a>登录/注销  
  
##### <a name="logon"></a>登录  
当用户尝试登录到系统将报告此子类别。 这些事件发生访问计算机上。 对于交互式登录时，这些事件代发生登录到的计算机上。 如果网络登录花了一个值得访问的共享，这些事件生成承载访问的资源计算机上。 如果此设置配置为**无审核**，它是难以实现或无法确定的用户已访问过或尝试访问组织计算机。  
  
##### <a name="network-policy-server"></a>网络策略服务器  
此子类别报告 RADIUS (IAS) 和网络的访问权限保护 (NAP) 的用户访问请求所生成的事件。 可以将这些请求**授予**，**拒绝**，**放弃**，**隔离**，**锁屏**，并**解锁**。 审核此设置会导致中等或高量 NPS 和 IAS 服务器记录。  
  
##### <a name="ipsec-main-mode"></a>IPsec 主要模式  
此子类别主要模式协商期间报告 Internet 密钥 Exchange (IKE) 协议和验证 Internet 协议 (AuthIP) 的结果。  
  
##### <a name="ipsec-extended-mode"></a>IPsec 外延模式  
此子类别外延模式协商期间报告 AuthIP 的结果。  
  
##### <a name="other-logonlogoff-events"></a>其他登录/注销事件  
其他登录和注销相关的事件，例如远程桌面服务会话断开连接并重新连接时，使用 RunAs 运行不同的帐户下的进程和锁定和解锁工作站，报告此子类别。  
  
##### <a name="logoff"></a>注销  
当系统用户注销后，此子类别报告。 这些事件发生访问计算机上。 对于交互式登录时，这些事件代发生登录到的计算机上。 如果网络登录花了一个值得访问的共享，这些事件生成承载访问的资源计算机上。 如果此设置配置为**无审核**，它是难以实现或无法确定的用户已访问过或尝试访问组织计算机。  
  
##### <a name="account-lockout"></a>帐户锁定  
用户帐户在多次登录尝试失败的结果锁定时，将报告此子类别。  
  
##### <a name="ipsec-quick-mode"></a>快速 IPsec 模式  
此子类别期间快速模式协商报告 IKE 协议和 AuthIP 的结果。  
  
##### <a name="special-logon"></a>特殊的登录  
当使用特殊登录时，此子类别报告。 特殊的登录是管理员等效权限中并用于提升到较高的进程登录。  
  
#### <a name="policy-change"></a>更改策略  
  
##### <a name="audit-policy-change"></a>更改审核策略  
此子类别报告中包括 SACL 更改审核策略的更改。  
  
##### <a name="authentication-policy-change"></a>身份验证更改策略  
此子类别报告身份验证的策略中的更改。  
  
##### <a name="authorization-policy-change"></a>授权策略更改  
此子类别报告中包括权限 (DACL) 更改授权策略的更改。  
  
##### <a name="mpssvc-rule-level-policy-change"></a>更改 mpssvc 规则级别策略  
此子类别报告中 Microsoft 保护服务 (MPSSVC.exe) 所使用的策略规则的更改。 该服务使用通过 Windows 防火墙。  
  
##### <a name="filtering-platform-policy-change"></a>筛选平台策略更改  
此子类别报告添加和删除 WFP，包括启动 filters 从的对象。 这些事件可能会在音量非常高。  
  
##### <a name="other-policy-change-events"></a>其他策略更改事件  
此子类别报告安全策略更改，如配置加密的提供商的受信任的平台模块 (TPM) 的其他的类型。  
  
#### <a name="privilege-use"></a>使用权限  
  
##### <a name="sensitive-privilege-use"></a>敏感特权使用  
当用户帐户时，此子类别报告或服务使用敏感权限。 敏感权限包含以下的用户版权：充当操作系统的一部分、备份文件和目录、创建标记对象、调试程序、启用受信任委派、生成安全审核、模拟客户端后身份验证、加载并卸载设备驱动程序、管理审核的计算机和用户帐户和安全日志、修改固件环境值、替换过程级别令牌还原的文件和目录，并获得的文件或其他对象所有权。 审核此子将创建有大量的事件。  
  
##### <a name="nonsensitive-privilege-use"></a>Nonsensitive 特权使用  
当用户帐户时，此子类别报告或服务使用 nonsensitive 特权。 Nonsensitive 特权包含以下的用户版权：访问为受信任的调用方的凭据管理器、访问该计算机的网络、添加域工作站、调整进程的内存配额、允许本地登录、允许远程桌面服务通过登录、跳过遍历检查、更改系统时间、创建页面文件、创建全球对象、创建永久共享的对象、创建符号链接拒绝此计算机的网络的访问、拒绝作为批量作业登录、拒绝服务作为登录、本地拒绝登录、拒绝远程桌面服务通过登录、强制关机远程系统从、提高进程工作集、计划优先级提高、锁定在内存中的页面、作为批量作业登录、登录作为一项服务、修改对象标签执行音量维护任务，个人资料单个进程配置系统性能、删除拓展坞的计算机、关机系统，和同步目录服务的数据。 审核此子将创建量很大的事件。  
  
##### <a name="other-privilege-use-events"></a>其他特权使用事件  
当前未使用该安全策略设置。  
  
#### <a name="object-access"></a>对象访问  
  
##### <a name="file-system"></a>系统文件  
此子类别报告时，文件系统对象进行访问。 仅具有 Sacl 的文件系统对象导致匹配他们 SACL 条目的方式进行访问时，会生成，并且仅审核事件。 自行，此设置不会导致审核的所有事件。 确定是否审核访问已指定的系统访问控制列表 (SACL) 时，文件系统对象的用户事件有效地启用审核才会生效。  
  
如果设置审核对象访问配置为**成功**，审核项生成每次用户成功访问具有特定的对象。 如果此策略设置配置为**失败**，用户无法在尝试访问具有特定的对象每次生成审核条目。  
  
##### <a name="registry"></a>注册表  
访问注册表对象时，将报告此子类别。 仅注册表对象与 Sacl 导致匹配他们 SACL 条目的方式进行访问时，会生成，并且仅审核事件。 自行，此设置不会导致审核的所有事件。  
  
##### <a name="kernel-object"></a>内核对象  
访问如进程和 mutex 内核对象时，将报告此子类别。 只有内核对象与 Sacl 导致匹配他们 SACL 条目的方式进行访问时，会生成，并且仅审核事件。 通常内核对象将仅授予 Sacl 如果启用了 AuditBaseObjects 或 AuditBaseDirectories 审核选项。  
  
##### <a name="sam"></a>三千  
访问本地安全帐户管理器（三千）身份验证数据库对象时，将报告此子类别。  
  
##### <a name="certification-services"></a>证书服务  
执行认证服务操作时，此子类别报告。  
  
##### <a name="application-generated"></a>生成的应用程序  
应用程序通过使用审核应用程序编程接口 (Api) Windows 生成审核事件尝试时，将报告此子类别。  
  
##### <a name="handle-manipulation"></a>句柄操作  
此子类别报告时，打开或关闭的对象句柄。 仅具有 Sacl 对象导致会生成，并且仅当尝试句柄操作匹配 SACL 条目这些事件。 句柄事件，才会生成对象类型的操作（如系统文件或注册表）中启用，相应的对象访问子类别。  
  
##### <a name="file-share"></a>文件共享  
此子类别报告访问文件共享时。 自行，此设置不会导致审核的所有事件。 确定是否审核访问指定的系统访问控制列表 (SACL)、文件共享对象的用户事件有效地启用审核才会生效。  
  
##### <a name="filtering-platform-packet-drop"></a>筛选平台数据包放  
此子类别报告丢数据包时通过 Windows 筛选平台 (WFP)。 这些事件可能会在音量非常高。  
  
##### <a name="filtering-platform-connection"></a>筛选平台连接  
允许或阻止 WFP 连接时，将报告此子类别。 这些事件可能很高的音量。  
  
##### <a name="other-object-access-events"></a>其他对象访问事件  
此子类别报告其他对象访问相关事件任务计划程序作业和 COM + 对象等。  
  
#### <a name="system"></a>系统  
  
##### <a name="security-state-change"></a>更改安全状态  
此子类别中的系统，例如当安全子系统启动和停止的安全状态报告更改。  
  
##### <a name="security-system-extension"></a>安全系统扩展  
此子类别安全子系统报告身份验证程序包如扩展代码的加载。  
  
##### <a name="system-integrity"></a>系统完整性  
有关安全子系统完整性违反报告此子类别。  
  
IPsec 驱动程序  
  
此子类别 Internet 协议安全驱动程序的活动报告。  
  
##### <a name="other-system-events"></a>其他系统事件  
有关其他系统事件的报告此子类别。  
  
有关子描述的详细信息，请参阅[Microsoft 安全合规性管理器工具](https://technet.microsoft.com/library/cc677002.aspx)。  
  
每一个组织应的以前覆盖的类别和子类别中查看，并启用所这最适合他们的环境。 始终应之前生产环境中部署测试审核策略的更改。  
  
### <a name="configuring-windows-audit-policy"></a>配置 Windows 审核策略  
可以使用组策略、auditpol.exe、Api 或注册表编辑设置 Windows 审核策略。 对于大多数公司配置审核策略推荐的方法是组策略或 auditpol.exe。 设置系统审核策略需要管理员级别的帐户权限或委派相应的权限。  
  
> [!NOTE]  
> **管理审核和安全日志**特权必须为安全原则（管理员其默认具有）允许对象访问文件、Active Directory 对象和注册表项等个别资源的选项的审核修改。  
  
#### <a name="setting-windows-audit-policy-by-using-group-policy"></a>使用组策略设置 Windows 审核策略  
若要设置审核策略使用组策略、配置相应审核类别下找到**计算机配置 windows \ 设置本地审核策略**（请参阅下面的屏幕截图，例如本地组策略编辑器 (gpedit.msc) 从）。 可以的启用了每个审核策略类别**成功**，**失败**，或**成功**和故障事件。  
  
![监视广告](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
高级的审核策略可以通过使用 Active Directory 或本地组策略设置。 若要设置高级审核策略、配置相应子类别中位于**计算机配置 windows \ Settings\Advanced 审核策略**（请参阅下面的屏幕截图，例如本地组策略编辑器 (gpedit.msc) 从）。 每个审核策略子可以的启用**成功**，**失败**，或**成功**和**失败**事件。  
  
![监视广告](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
#### <a name="setting-windows-audit-policy-using-auditpolexe"></a>设置 Windows 审核策略使用 Auditpol.exe  
在 Windows Server 2008 和 Windows Vista 引入了 Auditpol.exe（适用于设置 Windows 审核策略）。 最初，仅 auditpol.exe 可用于设置高级审核策略，但在 Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008、Windows 8 和 Windows 7，可以使用组策略。  
  
Auditpol.exe 是命令行实用程序。 语法如下所示：  
  
**auditpol//set / < 类别 |子类别 >:<audit category> / < 成功 | 失败：> / < 启用 | 禁用 >**  
  
Auditpol.exe 语法示例：  
  
**auditpol//set /subcategory:"用户帐户管理"/success：启用 /failure：启用**  
  
**auditpol//set /subcategory:"登录"/success：启用 /failure：启用**  
  
**auditpol//set /subcategory:"IPSEC 主要模式"/failure：启用**  
  
> [!NOTE]  
> Auditpol.exe 本地设置高级审核策略。 如果本地策略冲突 Active Directory 或本地组策略、组策略设置通常通过 auditpol.exe 设置为准。 当多个组或当地策略冲突存在时，只有一个策略将为准（，，将其替换）。 不会合并审核策略。  
  
#### <a name="scripting-auditpol"></a>脚本 Auditpol  
Microsoft 提供[示例脚本](https://support.microsoft.com/kb/921469)适用于想要设置高级审核策略，而不是在每个 auditpol.exe 命令手动键入使用脚本的管理员。  
  
**注意**组策略不始终准确报告的所有启用审核策略状态而 auditpol.exe 的功能。 请参阅[获取有效的审核策略，在 Windows 7 和 Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx)的更多详细信息。  
  
#### <a name="other-auditpol-commands"></a>其他 Auditpol 命令  
若要保存和还原一个本地审核策略，并查看其他审核相关的命令，则可以使用 Auditpol.exe。 以下是另**auditpol**命令。  
  
**auditpol/清除**-使用若要清除并重置本地审核策略  
  
**auditpol /backup /file:<filename>** -使用当前的本地审核策略备份到二进制文件  
  
**auditpol /restore /file:<filename>** -使用以前保存的审核策略文件导入到本地审核策略  
  
**auditpol / < 获取/设置 > /option:<CrashOnAuditFail> / < 启用/禁用 >** -如果已启用此审核策略设置，它会导致系统立即停止 (停止使用：C0000244 {失败审核} 消息) 不能因任何原因记录安全审核。 通常，一个事件无法正常工作时在安全审核日志已满，并且保留方法指定的安全日志记录**不会覆盖事件**或**覆盖通过天的事件**。 通常仅启用了该需要更高版本保障的安全日志记录的环境。 如果启用，则管理员必须密切关注安全日志大小并旋转根据需要日志。 它还可以设置使用组策略通过修改安全选项**审核：关机系统如果无法登录安全审核立即**(默认 = 禁用)。  
  
**Auditpol / < 获取/设置 > /option:<AuditBaseObjects> / < 启用/禁用 >** -此审核策略设置用于确定是否审核全球系统对象的访问。 如果启用了该策略，它会导致系统对象，如 mutex、事件、信号和 DOS 设备使用默认系统访问控制列表 (SACL) 创建。 大多数管理员考虑审核全球系统对象是太"恼人"，并他们只能将启用它，如果怀疑有恶意攻击。 仅命名的对象指定 SACL。 如果还启用审核访问审核策略（的对象内核对象审核子），被审核访问这些系统对象。 配置时该安全设置，请更改才会生效，直到重新启动 Windows。 该策略还可以设置使用组策略通过修改全球系统对象的安全选项审核访问的权限 (默认 = 禁用)。  
  
**auditpol / < 获取/设置 > /option:<AuditBaseDirectories> / < 启用/禁用 >** -此审核策略设置指定是要创建时提供 Sacl 命名的内核（如 mutex 和信号）的对象。 AuditBaseDirectories 影响容器对象时 AuditBaseObjects 影响无法包含其他对象的对象。  
  
**Auditpol / < 获取/设置 > /option:<FullPrivilegeAuditing> / < 启用/禁用 >** -  
  
此审核策略设置指定客户端将生成一个时的事件，还是分配给用户安全令牌这些权限的详细信息：AssignPrimaryTokenPrivilege、AuditPrivilege、BackupPrivilege、CreateTokenPrivilege、DebugPrivilege、EnableDelegationPrivilege、ImpersonatePrivilege、LoadDriverPrivilege、RestorePrivilege、SecurityPrivilege、SystemEnvironmentPrivilege、TakeOwnershipPrivilege 和 TcbPrivilege。 如果未启用该选项 (默认 = 禁用)，将不会记录 BackupPrivilege 和 RestorePrivilege 的权限。 启用该选项可使备份操作期间的安全日志非常恼人（有时数百种一秒的事件）。 该策略还可以设置使用组策略通过修改安全选项**审核：审核备份和还原权限使用**。  
  
> [!NOTE]  
> 下面提供的一些信息来自 Microsoft[审核选项类型](https://msdn.microsoft.com/library/dd973862(prot.20).aspx)和 Microsoft SCM 工具。  
  
### <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>履行传统审核或高级审核  
在 Windows Server 2012、Windows Server 2008 R2、Windows Server 2008、Windows 8、Windows 7 和 Windows Vista 管理员可以选择要启用九传统类别或使用子类别。 它是二进制必须每个 Windows 系统中进行选择。 启用了主要的类别，或者 subcategoriesit 不能同时。  
  
若要阻止旧的传统类别策略覆盖审核策略子类别中，你必须启用**强制审核策略子设置 (Windows Vista 或更高版本) 覆盖审核策略类别设置**策略设置位于**计算机配置 windows \ 设置本地安全选项**。  
  
我们建议子类别中处于启用状态并配置而不是九个主要的类别。 这要求组策略设置启用（以允许子类别中覆盖审核类别）以及配置支持审核的策略不同子类别。  
  
可以使用几种方法，包括组策略和命令行计划，auditpol.exe 配置审核子类别。  
  
有关 Windows 审核的详细信息，请参阅下面的文章：  
  
-   [高级安全审核在 Windows 7 和 Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
-   [审核和在 Windows Server 2008 合规性](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
-   [如何使用组策略配置详细的安全审核设置基于 Windows Vista 和 Windows Server 2008 基于计算机在域 Windows Server 2008、Windows Server 2003 某个域中，或 Windows 2000 域中](https://support.microsoft.com/kb/921469)  
  
-   [高级安全审核策略 Step-by-Step 指南](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
  


