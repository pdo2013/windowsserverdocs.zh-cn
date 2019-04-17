---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: "Windows Server 2016 功能级别"
description: 
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: a39955cf088ce7ce8bef20c70b83d49c6d508497
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="forest-and-domain-functional-levels"></a>森林和域功能级别

>适用于： Windows Server

Windows 2003 的生命周期结束时，Windows 2003 域控制器 (Dc) 需要进行更新到 Windows Server 2008、 2012年或 2016年。 因此，应从域中删除任何运行 Windows Server 2003 的域控制器。 到应该至少提升域和森林功能级别 Windows Server 2008 以防止被添加到环境中运行较早版本的 Windows Server 的域控制器。

我们建议客户在此更新其域功能级别 (DFL) 和森林功能级别 (FFL)、 由于 2003 DFL 和 FFL 弃用 Windows Server 2016 中，并且不会再将在将来支持的版本。

对于需要额外时间来评估移动他们 DFL 和 FFL 2003 从的客户，2003 DFL 和 FFL 会继续受具有 Windows 10 和 Windows Server 2016 提供域和森林中的所有域控制器都都可以在 Windows Server 2008、 2008R2、 2012、 2012R2，或 2016年。

在 Windows Server 2008 和更高版本的域功能级别，分布式文件服务 (DFS) 复制用于域控制器之间复制 SYSVOL 文件夹的内容。 如果你创建一个新域，Windows Server 2008 域级别正常工作或更高版本，已自动使用 DFS 复制复制 SYSVOL。 如果你在较低的功能级别创建域，你将需要使用的 SYSVOL DFS 复制到 FRS 迁移。 对于迁移步骤中，您可以按照[TechNet 上的过程](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)，或者你可以参考[优化的步骤存储团队文件柜博客上的一套](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。

Windows Server 2003 域和森林功能级别继续受支持，但组织应提升与 Windows Server 2008 （或如果可能，更高版本） 功能级别以确保 SYSVOL 复制兼容性，并在将来的支持。 此外，有许多其他权益和功能可在更高版本的功能级别更高版本。 请参阅以下详细信息资源：

## <a name="windows-server-2016"></a>Windows Server 2016

受支持的域控制器操作系统：

* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Windows Server 2016 森林功能级别功能

* 所有功能，可在 Windows Server 2012R2 森林功能级别，以及以下功能，都可：
    * [访问权限的管理 (PAM) 使用 Microsoft 身份管理器 (MIM)](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Windows Server 2016 域功能级别功能

* 默认 Active Directory 的所有功能、 所有功能从 Windows Server 2012R2 域功能级别，以及以下功能：
    * Dc 可支持推出公钥仅用户的 NTLM 机密信息。 
    * 用户限于特定加入域的设备时，域控制器可支持允许网络 NTLM。 
    * 成功进行身份验证的 PKInit 新鲜扩展的 Kerberos 客户端将获取最新公开键标识 SID。

    有关详细信息，请参阅[Kerberos 身份验证的新增](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication)和[凭据保护中的新增功能](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

受支持的域控制器操作系统：

* Windows Server 2016
* Windows Server 2012R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012R2 森林功能级别功能

* 可在 Windows Server 2012 的功能的所有林正常工作，而无需额外的功能。

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012R2 域功能级别功能

* 默认 Active Directory 的所有功能、 所有功能从 Windows Server 2012 域功能的级别，以及以下功能：
    * 对于受保护的用户 DC 侧边提供保护。 保护向域不再可以 Windows Server 2012 R2 的用户身份：
        * 通过 NTLM 身份验证身份验证
        * 使用 DES 或 RC4 密码套件中 Kerberos 预先身份验证
        * 使用不受约束或受限制委派委派
        * 续订初始 4 小时整个使用期限内之外的用户门票 (Tgt)
    * 身份验证策略
        * 新林基于 Active Directory 策略，这可用于 Windows Server 2012 R2 域来控制哪些主机中的帐户的帐户可以登录从和适用于运行的为某个帐户的服务的访问控制条件身份验证。
    * 身份验证策略思
        * 基于新林 Active Directory 对象，可以创建用户、托管的服务和计算机之间关系，这帐户要用于分类帐户身份验证的策略或身份验证隔离。

## <a name="windows-server-2012"></a>Windows Server 2012

受支持的域控制器操作系统：

* Windows Server 2016
* Windows Server 2012R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Windows Server 2012 森林功能级别功能

* 可在 Windows Server 2008 R2 的功能的所有林正常工作，而无需额外的功能。

### <a name="windows-server-2012-domain-functional-level-features"></a>Windows Server 2012 域功能级别功能

* 默认 Active Directory 的所有功能、 所有功能从 Windows Server 2008R2 域功能级别，以及以下功能：
    * KDC 支持索赔、 复合身份验证、 和程度 KDC 管理模板策略 Kerberos 有两个设置 （始终提供索赔和失败 unarmored 身份验证请求） 需要 Windows Server 2012 域功能级别。 有关详细信息，请参阅[什么是验证 Kerberos 中的新增功能](https://technet.microsoft.com/en-us/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

受支持的域控制器操作系统：

* Windows Server 2016
* Windows Server 2012R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008R2 森林功能级别功能

* 全部可用功能，在 Windows Server 2003 森林功能级别，以及以下功能：
    * Active Directory回收站，该法案还原已删除的完整地的对象广告 DS 正在运行时的能力。

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008R2 域功能级别功能

* 默认 Active Directory 的所有功能、 所有功能从 Windows Server 2008 域功能的级别，以及以下功能：
    * 身份验证机制保障、 打包用于验证在每个用户 Kerberos 令牌域用户身份登录方法 （智能卡或用户名/密码） 的类型的信息。 已部署联合的身份管理基础结构，如 Active Directory 联合身份验证服务 (AD FS) 网络环境中启用此功能用户尝试访问已开发确定授权根据用户登录方法任何声明感知应用时，然后会提取中标记的信息。
    * 自动 SPN 管理时姓名或 DNS 主机更改机器帐户的名称下托管服务帐户上下文特定计算机上运行的服务。 有关托管服务帐户的详细信息，请参阅[服务帐户 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkId=180401)。

## <a name="windows-server-2008"></a>Windows Server 2008

受支持的域控制器操作系统：

* Windows Server 2016
* Windows Server 2012R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Windows Server 2008 林级别正常工作的功能

* 所有在 Windows Server 2003 森林功能级别有可用的功能，但没有其他功能都可用。 

### <a name="windows-server-2008-domain-functional-level-features"></a>Windows Server 2008 域功能级别功能

* 所有的默认广告 DS 功能的所有功能从 Windows Server 2003 域功能的级别，以及以下功能：
    * 分布式的文件系统 (DFS) 复制支持的 Windows Server 2003 系统卷 (SYSVOL)
        * DFS 复制支持提供更强大、更详细复制 SYSVOL 内容。
        [!NOTE]>
        >自 Windows Server 2012 R2 起，不再文件复制服务 (FRS)。 运行至少域控制器创建一个新域为 Windows Server 2008 域功能级别或更高版本的 Windows Server 2012 R2 必须设置。

    * 基于域 DFS 命名空间运行 Windows Server 2008 模式，包括在支持访问基于枚举和增加可扩展性。 将命名空间域基于 Windows Server 2008 型还需要使用 Windows Server 2003 森林功能级别森林。 有关详细信息，请参阅[选择 Namespace 类型](https://go.microsoft.com/fwlink/?LinkId=180400)。
    * 高级的 Kerberos 协议的加密标准（AES 128 和 AES 256）支持。 为了使 Tgt AES 发出，域功能级别必须是 Windows Server 2008 或更高版本，并域密码需要进行更改。 
        * 有关详细信息，请参阅[Kerberos 增强](https://technet.microsoft.com/library/cc749438(ws.10).aspx)。
        [!NOTE]>
        >身份验证错误，可能会发生该域控制器上之后域功能提升到 Windows Server 2008 或更高版本如果域控制器已复制 DFL 更改，但不是刷新 krbtgt 密码。 在此情况下，重启该域控制器上的 KDC 服务将触发新 krbtgt 密码的内存中刷新并解决相关身份验证错误。

    * [最后一交互式登录](https://go.microsoft.com/fwlink/?LinkId=180387)信息将显示以下信息：
        * 总在已加入域的 Windows Server 2008 服务器或 Windows Vista 工作站的失败的登录数
        * 登录尝试失败后成功登录到 Windows Server 2008 服务器或 Windows Vista 工作站总数量
        * 在 Windows Server 2008 或 Windows Vista 工作站尝试的最后一个失败的登录的时间
        * 在 Windows Server 2008 服务器或 Windows Vista 工作站尝试成功上次登录的时间
    * 精细的密码策略，使您可以在某个域中指定的用户和组全局安全的密码和帐户锁定策略。 有关详细信息，请参阅[Fine-Grained 密码和帐户锁定策略配置的分步指南](https://go.microsoft.com/fwlink/?LinkID=91477)。
    * 个人虚拟桌面
        * 若要使用提供个人的虚拟桌面选项卡 Active Directory 用户和计算机中的用户帐户属性对话框中的新增的功能，广告 DS 方案必须延长适用于 Windows Server 2008 R2 (方案对象版本 = 47)。 有关详细信息，请参阅[部署个人虚拟桌面使用 RemoteApp 和桌面连接通过 Step-by-Step 指南](https://go.microsoft.com/fwlink/?LinkId=183552)。

## <a name="windows-server-2003"></a>Windows Server 2003

受支持的域控制器操作系统：

* Windows Server 2012R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Windows Server 2003 林级别正常工作的功能

* 默认广告 DS 功能，以及以下功能，均适用：
    * 森林信任
    * 域重命名
    * 链接值复制
        - 链接值复制可供你更改组成员来存储和个别成员复制的所有成员作为一个整体而复制值。 存储和复制个别成员的值使用较少的网络带宽和较少的处理器周期复制期间，，并且会阻止你添加或删除在不同的域控制器同时的多个成员时，会丢失更新。
    * 部署仅阅读域控制器 (RODC) 的能力
    * 改进了知识一致性检查 (KCC) 算法的支持，并可扩展性
        - 站点间拓扑 generator (ISTG) 使用缩放以使用更多的站点森林广告 DS 可以支持在 Windows 2000 森林功能级别支持的改进的算法。 改进了的 ISTG 选举算法是用于选择级别的 Windows 2000 森林功能 ISTG 不太干扰机制。
    * 创建的名为动态辅助类实例的能力**dynamicObject**域目录分区中
    * 能够转换**inetOrgPerson**到的对象实例**用户**对象实例，以完成以相反方向转换
    * 创建的新组类型，以支持角色基于授权的情况下的能力。 
        - 这些类型称为应用程序的基本组和 LDAP 查询组。
    * 停用并重新的特性并类架构中的定义。 可重复使用以下属性：ldapDisplayName，schemaIdGuid，OID，并 mapiID。
    * 基于域 DFS 命名空间运行 Windows Server 2008 模式，包括在支持访问基于枚举和增加可扩展性。 有关详细信息，请参阅[选择 Namespace 类型](https://go.microsoft.com/fwlink/?LinkId=180400)。

### <a name="windows-server-2003-domain-functional-level-features"></a>Windows Server 2003 域正常工作级别功能

* 默认广告 DS 的所有功能、Windows 2000 原始域级别正常工作，有可用的所有功能和以下功能现在提供：
    * 域管理工具，Netdom.exe，从而使其更有可能重命名域控制器
    * 登录的时间戳更新
        * **LastLogonTimestamp**特性用户或计算机的上次登录的更新。 此属性复制域中。
    * 若要设置的能力**用户密码**特性作为生效密码**inetOrgPerson**和用户对象
    * 能够用户和计算机重定向容器
        * 默认情况下，将两个普遍容器提供容纳计算机和用户帐户，即 cn = 计算机，<domain root>和 cn = 用户，<domain root>。 此功能允许这些帐户的新的已知位置的清晰度。
    * 功能用于授权管理器中广告 DS 存储其授权策略
    * 约束的委派
        * 约束的委派使利用用户凭据安全委派通过 Kerberos 基于身份验证的应用程序。
        * 您可以限制委派仅特定目的地服务。
    * 选择性身份验证
        * 选择性身份验证变得更为使您可以指定的用户和组从受信任的森林有权通过信任森林中的资源服务器的身份验证。

## <a name="windows-2000"></a>Windows 2000

受支持的域控制器操作系统：

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Windows 2000 本机森林功能级别功能

* 所有默认广告 DS 功能都可用。

### <a name="windows-2000-native-domain-functional-level-features"></a>Windows 2000 原始域功能级别功能

* 默认广告 DS 功能和的以下目录功能均可用包括：
    * 通用组 distribution 和安全性的组。
    * 组嵌套
    * 这使安全和 distribution 组之间转换的组转换
    * 安全标识符历史记录

## <a name="next-steps"></a>后续步骤

* [提升域功能级别](https://technet.microsoft.com/library/cc753104.aspx)  
* [提升森林功能级别](https://technet.microsoft.com/library/cc730985.aspx)
