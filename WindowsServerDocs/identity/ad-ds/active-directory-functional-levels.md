---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Windows Server 2016 功能级别
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: ea56c718394d145a36145d32e5769661a62efd56
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840998"
---
# <a name="forest-and-domain-functional-levels"></a>林和域功能级别

>适用于：Windows Server

功能级别确定可用的 Active Directory 域服务 (AD DS) 域或林功能。 它们还决定了可以在域或林中的域控制器运行的 Windows Server 操作系统。 但是，功能级别不会影响哪些操作系统可以在工作站运行和已加入域或林的成员服务器。

在部署 AD DS 时，设置域和林功能级别为你的环境可以支持的最大值。 这样一来，可以尽可能使用尽可能多的 AD DS 功能。 在部署新林时，系统会提示将林功能级别设置，然后设置域功能级别。 可以将域功能级别设置为一个值，高于林功能级别，但不能将域功能级别设置为低于林功能级别的值。

使用 Windows 2003 的生命周期结束时，Windows 2003 域控制器 (Dc) 需要更新到 Windows Server 2008，2008R2、 2012、 2016年或 2019年 2012R2。 因此，应从域中删除任何运行 Windows Server 2003 的域控制器。

在 Windows Server 2008 和更高版本的域功能级别，分布式文件服务 (DFS) 复制用于域控制器之间复制 SYSVOL 文件夹的内容。 如果在 Windows Server 2008 域功能级别或更高版本创建一个新的域，自动使用 DFS 复制来复制 SYSVOL。 如果在较低功能级别创建域，您将需要使用 FRS 的 SYSVOL 的 DFS 复制进行迁移。 有关迁移的步骤，你可以按照[TechNet 上的过程](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)也可以引用[简化组的 Cab 文件的存储团队博客上的步骤](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。

## <a name="windows-server-2019"></a>Windows Server 2019

没有任何新的林或此版本中添加的域功能级别。

若要添加 Windows Server 2019 域控制器的最低要求是 Windows Server 2008R2 功能级别。

## <a name="windows-server-2016"></a>Windows Server 2016

受支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Windows Server 2016 林功能级别的功能

* 在 Windows Server 2012R2 林功能级别，可用的功能及以下功能，都可用：
   * [使用 Microsoft Identity Manager (MIM) 特权的访问管理 (PAM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Windows Server 2016 域功能级别的功能

* 所有默认的 Active Directory 功能、 所有来自 Windows Server 2012R2 域功能级别，以及以下功能：
   * Dc 可以支持自动滚动的 NTLM 和其他基于密码的机密信息的用户帐户配置为需要 PKI 身份验证。 此配置也称为是"交互式登录需要智能卡"
   * Dc 可以支持允许网络 NTLM，如果某个用户是限于特定的已加入域的设备。
   * 已成功使用 PKInit 新鲜度扩展进行身份验证的 Kerberos 客户端将获得全新公共密钥标识的 SID。

    有关详细信息请参阅[What's New in Kerberos 身份验证](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication)和[什么是凭据保护中的新增功能](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

受支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012R2 林功能级别的功能

* 所有功能，可在 Windows Server 2012 林功能级别，而无需额外的功能。

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012R2 域功能级别的功能

* 所有默认的 Active Directory 功能、 所有来自 Windows Server 2012 的域功能级别，以及以下功能：
   * 受保护的用户的 DC 端保护。 受保护用户对 Windows Server 2012 R2 的域可以不再进行身份验证：
      * 使用 NTLM 身份验证进行身份验证
      * 在 Kerberos 预身份验证中使用 DES 或 RC4 密码套件
      * 使用不受约束或约束委派进行委派
      * 超出初始 4 小时生存期后续订用户票证 (Tgt)
   * 身份验证策略
      * 基于新林的 Active Directory 策略可应用于 Windows Server 2012 R2 域来控制哪些主机中的帐户的帐户可以从登录和身份验证的访问控制条件应用到运行方式帐户的服务。
   * 身份验证策略接收器
      * 基于新林的 Active Directory 对象，该对象可以创建用户、 托管的服务和计算机帐户要用来进行身份验证策略或身份验证隔离帐户分类之间的关系。

## <a name="windows-server-2012"></a>Windows Server 2012

受支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Windows Server 2012 林功能级别的功能

* 所有功能，可在 Windows Server 2008 R2 林功能级别，而无需额外的功能。

### <a name="windows-server-2012-domain-functional-level-features"></a>Windows Server 2012 域功能级别的功能

* 所有默认的 Active Directory 功能、 所有来自 Windows Server 2008R2 域功能级别，以及以下功能：
   * KDC 支持声明、 复合身份验证和 Kerberos 保护的 KDC 管理模板策略具有需要 Windows Server 2012 域功能级别的两个设置 （始终提供声明和使未保护身份验证请求失败）。 有关详细信息，请参阅[What's New in Kerberos 身份验证](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

受支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008R2 林功能级别的功能

* Windows Server 2003 林功能级别上可用的所有功能，以及下列功能：
   * Active Directory 回收站，提供在运行 AD DS 时还原整个已删除对象的功能。

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008R2 域功能级别的功能

* 所有默认的 Active Directory 功能、 所有来自 Windows Server 2008 域功能级别，以及以下功能：
   * 身份验证机制保证，将对域用户进行身份验证所用的登录方法类型（智能卡或用户名/密码）相关信息封装在每个用户的 Kerberos 令牌中。 在网络环境中部署的联合身份标识管理基础结构，如 Active Directory 联合身份验证服务 (AD FS) 启用此功能在令牌中的信息然后每当用户尝试访问任何都会提取已开发以确定授权基于用户的登录方法的声明感知应用程序。
   * 自动的名称或 DNS 主机名的计算机帐户更改时，托管服务帐户的上下文在特定计算机上运行的服务的 SPN 管理。 有关托管服务帐户的详细信息，请参阅[服务帐户循序渐进指南](https://go.microsoft.com/fwlink/?LinkId=180401)。

## <a name="windows-server-2008"></a>Windows Server 2008

受支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Windows Server 2008 林功能级别的功能

* 可用的所有 Windows Server 2003 林功能级别上可用的功能，但任何其他功能都。 

### <a name="windows-server-2008-domain-functional-level-features"></a>Windows Server 2008 域功能级别的功能

* 所有默认的 AD DS 功能，所有来自 Windows Server 2003 域功能级别的功能，并提供了以下功能：
   * 分布式的文件系统 (DFS) 复制支持的 Windows Server 2003 系统卷 (SYSVOL)
      * DFS 复制支持提供 SYSVOL 内容的更稳健更详细的复制。
        [!NOTE]>
        >从 Windows Server 2012 R2 开始，文件复制服务 (FRS) 已弃用。 在至少运行的域控制器创建一个新域为 Windows Server 2008 域功能级别或更高版本，必须设置 Windows Server 2012 R2。

   * 基于域的 DFS 命名空间在 Windows Server 2008 模式，其中包括对基于访问的枚举以及增强的可伸缩性的支持中运行。 在 Windows Server 2008 模式下基于域的命名空间还要求要使用 Windows Server 2003 林功能级别的林中。 有关详细信息，请参阅[选择 Namespace 类型](https://go.microsoft.com/fwlink/?LinkId=180400)。
   * Kerberos 协议的高级的加密标准 （AES 128 和 AES 256） 支持。 为了使使用 AES 要颁发的 Tgt，域功能级别必须是 Windows Server 2008 或更高版本和域密码需要更改。 
      * 有关详细信息，请参阅[Kerberos 增强功能](https://technet.microsoft.com/library/cc749438(ws.10).aspx)。
        [!NOTE]>
        >身份验证错误后可能会发生在域控制器上域功能级别被提升到 Windows Server 2008 或更高版本的域控制器如果具有已复制 dfl 上更改，但不是刷新 krbtgt 密码。 在这种情况下，重新启动域控制器上的 KDC 服务将触发新的 krbtgt 密码的内存中刷新并解决相关身份验证错误。

   * [上次交互式登录](https://go.microsoft.com/fwlink/?LinkId=180387)信息将显示以下信息：
      * 在已加入域的 Windows Server 2008 server 或 Windows Vista 工作站的失败的登录尝试总次数
      * 失败的登录尝试后成功登录到 Windows Server 2008 server 或 Windows Vista 工作站的总数
      * 在 Windows Server 2008 或 Windows Vista 工作站的最后一个失败的登录尝试的时间
      * 在 Windows Server 2008 server 或 Windows Vista 工作站尝试的上一次成功登录时间
   * 严格的密码策略使您可以指定域中的密码和帐户锁定策略的用户和全局安全组。 有关详细信息，请参阅[细化密码和帐户锁定策略配置的循序渐进指南](https://go.microsoft.com/fwlink/?LinkID=91477)。
   * 个人虚拟桌面
      * 若要使用的 Active Directory 用户和计算机中的用户帐户属性对话框中的个人虚拟桌面选项卡提供的附加的功能，你的 AD DS 架构必须扩展适用于 Windows Server 2008 R2 (架构对象版本 = 47)。 有关详细信息，请参阅[部署个人虚拟桌面通过使用 RemoteApp 和桌面连接循序渐进指南](https://go.microsoft.com/fwlink/?LinkId=183552)。

## <a name="windows-server-2003"></a>Windows Server 2003

受支持的域控制器操作系统：

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Windows Server 2003 林功能级别的功能

* 所有默认的 AD DS 功能，以及以下功能都可用：
   * 林信任
   * 域重命名
   * 链接值复制
      - 链接值复制使您可以更改组成员身份以存储和复制为各个成员而不是复制整个成员身份作为单个单元的值。 存储和复制的各个成员的值使用较少网络带宽和较少的处理器周期在复制期间，并阻止您在添加或删除多个成员，同时在不同的域控制器时丢失更新。
   * 部署只读域控制器 (RODC) 的能力
   * 改进了知识一致性检查 (KCC) 算法和可伸缩性
      - 站点间拓扑生成器 (ISTG) 使用改进的算法，缩放以支持具有更多的站点不是 AD DS 可以支持在 Windows 2000 林功能级别。 改进的 ISTG 选择算法是一种选择在 Windows 2000 林功能级别 ISTG 无侵入性机制。
   * 创建名为动态辅助类的实例的功能**dynamicObject**域目录分区中
   * 要转换的功能**inetOrgPerson**对象实例转换**用户**对象实例，并完成相反的方向的转换
   * 创建新组类型以支持基于角色的授权的实例的功能。 
      - 这些类型称为应用程序基本组和 LDAP 查询组。
   * 在架构中停用并重新定义属性和类别。 可以重复使用以下属性： ldapDisplayName、 schemaIdGuid、 OID，和 mapiID。
   * 基于域的 DFS 命名空间在 Windows Server 2008 模式，其中包括对基于访问的枚举以及增强的可伸缩性的支持中运行。 有关详细信息，请参阅[选择 Namespace 类型](https://go.microsoft.com/fwlink/?LinkId=180400)。

### <a name="windows-server-2003-domain-functional-level-features"></a>Windows Server 2003 域功能级别的功能

* 所有默认的 AD DS 功能，可在 Windows 2000 本机模式域功能级别的所有功能及以下功能有：
   * 域管理工具 Netdom.exe，这样就可以为您重命名域控制器
   * 登录时间戳更新
      * 使用用户或计算机的上次登录时间来更新 **lastLogonTimestamp** 属性。 可以在域内复制该属性。
   * 若要设置的功能**userPassword**特性为有效密码，可以在**inetOrgPerson**和用户对象
   * 能够将重定向用户和计算机容器
      * 默认情况下，两个已知容器提供用于容纳计算机和用户帐户，即 cn = Computers，<domain root>和 cn = Users，<domain root>。 此功能，这些帐户的一个新的、 众所周知的位置的定义。
   * 功能用于授权管理器将其授权策略存储在 AD DS
   * 约束的委派
      * 约束的委派，使应用程序可以基于 Kerberos 的身份验证通过充分利用用户凭据的安全委派。
      * 你可以限制仅委派到特定的目标服务。
   * 选择性身份验证
      * 选择性身份验证成为可能，以指定的用户和组从受信任的林中他们有权对信任林中资源服务进行身份验证。

## <a name="windows-2000"></a>Windows 2000

受支持的域控制器操作系统：

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Windows 2000 本机林功能级别的功能

* 所有默认的 AD DS 功能都可用。

### <a name="windows-2000-native-domain-functional-level-features"></a>Windows 2000 本机模式域功能级别的功能

* 所有默认的 AD DS 功能和以下目录功能都可包括：
   * 分发和安全组的通用组。
   * 组嵌套
   * 组转换，它允许安全和通讯组之间的转换
   * 安全标识符 (SID) 历史记录

## <a name="next-steps"></a>后续步骤

* [提升域功能级别](https://technet.microsoft.com/library/cc753104.aspx)  
* [提升林功能级别](https://technet.microsoft.com/library/cc730985.aspx)
