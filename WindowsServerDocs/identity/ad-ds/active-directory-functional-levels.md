---
ms.assetid: f964d056-11bf-4d9b-b5ab-dceaad8bfbc3
title: Windows Server 2016 功能级别
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.custom: it-pro
ms.reviewer: maheshu
ms.technology: identity-adds
ms.openlocfilehash: 7f16d58eb6c5074c75f49ba7936c4d312a3dbda4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390981"
---
# <a name="forest-and-domain-functional-levels"></a>林和域功能级别

>适用于：Windows Server

功能级别决定可用的 Active Directory 域服务（AD DS）域或林功能。 它们还确定了可以在域或林中的域控制器上运行的 Windows Server 操作系统。 但是，功能级别不会影响可以在加入域或林的工作站和成员服务器上运行的操作系统。

部署 AD DS 时，请将域和林功能级别设置为环境可以支持的最高值。 这样，你就可以使用尽可能多的 AD DS 功能。 部署新林时，系统将提示你设置林功能级别，然后设置域功能级别。 您可以将域功能级别设置为高于林功能级别的值，但不能将域功能级别设置为低于林功能级别的值。

随着 Windows 2003 的生存期，Windows 2003 域控制器（Dc）需要更新为 Windows Server 2008、2008R2、2012、2012R2、2016或2019。 因此，任何运行 Windows Server 2003 的域控制器都应该从域中删除。

在 Windows Server 2008 和更高版本的域功能级别，使用分布式文件服务（DFS）复制来复制域控制器之间的 SYSVOL 文件夹内容。 如果在 Windows Server 2008 域功能级别或更高级别创建新域，则会自动使用 DFS 复制来复制 SYSVOL。 如果在较低的功能级别创建了域，则需要从使用 FRS 迁移到 SYSVOL 的 DFS 复制。 对于迁移步骤，你可以按照[TechNet 上的过程](https://technet.microsoft.com/library/dd640019(v=WS.10).aspx)进行操作，也可以参考[存储团队文件 Cabinet 博客上的一组简化的步骤](http://blogs.technet.com/b/filecab/archive/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol.aspx)。

## <a name="windows-server-2019"></a>Windows Server 2019

此版本中未添加新的林或域功能级别。

添加 Windows Server 2019 域控制器的最低要求是 Windows Server 2008 功能级别。 域还必须使用 DFS 作为引擎来复制 SYSVOL。

## <a name="windows-server-2016"></a>Windows Server 2016

支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016

### <a name="windows-server-2016-forest-functional-level-features"></a>Windows Server 2016 林功能级别功能

* Windows Server 2012R2 林功能级别提供的所有功能以及以下功能可用：
   * [使用 Microsoft Identity Manager （MIM）的特权访问管理（PAM）](https://docs.microsoft.com/windows-server/identity/whats-new-active-directory-domain-services#a-namebkmkpamaprivileged-access-management)

### <a name="windows-server-2016-domain-functional-level-features"></a>Windows Server 2016 域功能级别功能

* 所有默认 Active Directory 功能，Windows Server 2012R2 域功能级别中的所有功能，以及以下功能：
   * Dc 可以支持在配置为需要 PKI 身份验证的用户帐户上自动滚动 NTLM 和其他基于密码的机密。 此配置也称为 "交互式登录需要智能卡"
   * 当用户仅限于特定的已加入域的设备时，Dc 可支持允许网络 NTLM。
   * Kerberos 客户端成功地通过 PKInit 新鲜度扩展进行身份验证时，将获得新的公钥标识 SID。

    有关详细信息，请参阅[Kerberos 身份验证中的新增功能](https://docs.microsoft.com/windows-server/security/kerberos/whats-new-in-kerberos-authentication)和[凭据保护的新增](https://docs.microsoft.com/windows-server/security/credentials-protection-and-management/whats-new-in-credential-protection)功能

## <a name="windows-server-2012r2"></a>Windows Server 2012R2

支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2

### <a name="windows-server-2012r2-forest-functional-level-features"></a>Windows Server 2012R2 林功能级别功能

* Windows Server 2012 林功能级别提供的所有功能，但没有其他功能。

### <a name="windows-server-2012r2-domain-functional-level-features"></a>Windows Server 2012R2 域功能级别功能

* 所有默认 Active Directory 功能、Windows Server 2012 域功能级别中的所有功能，以及以下功能：
   * 受保护用户的 DC 端保护。 对 Windows Server 2012 R2 域进行身份验证的受保护用户将无法再执行以下操作：
      * 通过 NTLM 身份验证进行身份验证
      * 在 Kerberos 预身份验证中使用 DES 或 RC4 密码套件
      * 通过不受约束或约束委派进行委派
      * 续订超过初始4小时生存期的用户票证（Tgt）
   * 身份验证策略
      * 可应用于 Windows Server 2012 R2 域中的帐户的新的基于林的 Active Directory 策略来控制帐户可以登录的主机，并将身份验证的访问控制条件应用到作为帐户运行的服务。
   * 身份验证策略接收器
      * 新的基于林的 Active Directory 对象，它可以在用户、托管服务和计算机之间创建关系，并使用这些帐户来分类用于身份验证策略或用于身份验证的帐户。

## <a name="windows-server-2012"></a>Windows Server 2012

支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012

### <a name="windows-server-2012-forest-functional-level-features"></a>Windows Server 2012 林功能级别功能

* Windows Server 2008 R2 林功能级别上可用的所有功能，但没有其他功能。

### <a name="windows-server-2012-domain-functional-level-features"></a>Windows Server 2012 域功能级别功能

* 所有默认 Active Directory 功能，Windows Server 2008R2 域功能级别中的所有功能，以及以下功能：
   * "KDC 支持声明、复合身份验证和 Kerberos 保护" KDC 管理模板策略具有两个设置（"始终提供声明" 和 "未保护身份验证请求失败"），需要 Windows Server 2012 域功能级别。 有关详细信息，请参阅[Kerberos 身份验证中的新增功能](https://technet.microsoft.com/library/hh831747.aspx)

## <a name="windows-server-2008r2"></a>Windows Server 2008R2

支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2

### <a name="windows-server-2008r2-forest-functional-level-features"></a>Windows Server 2008R2 林功能级别功能

* Windows Server 2003 林功能级别上可用的所有功能，以及下列功能：
   * Active Directory 回收站，提供在运行 AD DS 时还原整个已删除对象的功能。

### <a name="windows-server-2008r2-domain-functional-level-features"></a>Windows Server 2008R2 域功能级别功能

* 所有默认 Active Directory 功能、Windows Server 2008 域功能级别中的所有功能，以及以下功能：
   * 身份验证机制保证，其中打包了有关用于对每个用户的 Kerberos 令牌中的域用户进行身份验证的登录方法（智能卡或用户名/密码）类型的信息。 如果在部署了联合身份管理基础结构的网络环境中启用了此功能（如 Active Directory 联合身份验证服务（AD FS），则每当用户尝试访问任何已开发的声明感知应用程序，用于根据用户的登录方法确定授权。
   * 计算机帐户的名称或 DNS 主机名更改时，在特定计算机上运行的服务的自动 SPN 管理。 有关托管服务帐户的详细信息，请参阅[服务帐户分步指南](https://go.microsoft.com/fwlink/?LinkId=180401)。

## <a name="windows-server-2008"></a>Windows Server 2008

支持的域控制器操作系统：

* Windows Server 2019
* Windows Server 2016
* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008

### <a name="windows-server-2008-forest-functional-level-features"></a>Windows Server 2008 林功能级别功能

* Windows Server 2003 林功能级别提供的所有功能，但没有其他功能可用。 

### <a name="windows-server-2008-domain-functional-level-features"></a>Windows Server 2008 域功能级别功能

* 所有默认 AD DS 功能，Windows Server 2003 域功能级别中的所有功能都可用：
  * Windows Server 2003 系统卷（SYSVOL）分布式文件系统（DFS）复制支持
    * DFS 复制支持提供更强大和更详细的 SYSVOL 内容复制。

      > [!NOTE]
      > 从 Windows Server 2012 R2 开始，文件复制服务（FRS）已弃用。 在至少运行 Windows Server 2012 R2 的域控制器上创建的新域必须设置为 Windows Server 2008 域功能级别或更高版本。

  * 在 Windows Server 2008 模式下运行的基于域的 DFS 命名空间，其中包括对基于访问权限的枚举的支持和增加的可伸缩性。 Windows Server 2008 模式下的基于域的命名空间还要求林使用 Windows Server 2003 林功能级别。 有关详细信息，请参阅[选择命名空间类型](https://go.microsoft.com/fwlink/?LinkId=180400)。
  * 高级加密标准（AES 128 和 AES 256）对 Kerberos 协议的支持。 为了使用 AES 颁发 Tgt，域功能级别必须是 Windows Server 2008 或更高版本，并且需要更改域密码。 
    * 有关详细信息，请参阅[Kerberos 增强功能](https://technet.microsoft.com/library/cc749438(ws.10).aspx)。

      > [!NOTE]
      >在域功能级别提升到 Windows Server 2008 或更高版本（如果域控制器已复制 DFL 更改但尚未刷新 krbtgt 密码）后，域控制器上可能会发生身份验证错误。 在这种情况下，域控制器上的 KDC 服务重新启动将触发新的 krbtgt 密码的内存中刷新，并解决相关的身份验证错误。

  * [上次交互式登录](https://go.microsoft.com/fwlink/?LinkId=180387)信息显示以下信息：
     * 已加入域的 Windows Server 2008 服务器或 Windows Vista 工作站上登录尝试失败的总次数
     * 成功登录到 Windows Server 2008 服务器或 Windows Vista 工作站后的失败登录尝试总数
     * Windows Server 2008 或 Windows Vista 工作站上一次登录尝试失败的时间
     * 在 Windows Server 2008 服务器或 Windows Vista 工作站上最后一次成功登录尝试的时间
  * 细化密码策略使你可以为域中的用户和全局安全组指定密码和帐户锁定策略。 有关详细信息，请参阅[细化密码和帐户锁定策略配置的循序渐进指南](https://go.microsoft.com/fwlink/?LinkID=91477)。
  * 个人虚拟机
     * 若要在 Active Directory 用户和计算机的 "用户帐户属性" 对话框中使用 "个人虚拟桌面" 选项卡提供的附加功能，必须为 Windows Server 2008 R2 扩展 AD DS 架构（架构对象版本 = 47）。 有关详细信息，请参阅[使用 RemoteApp 和桌面连接分步指南部署个人虚拟机](https://go.microsoft.com/fwlink/?LinkId=183552)。

## <a name="windows-server-2003"></a>Windows Server 2003

支持的域控制器操作系统：

* Windows Server 2012 R2
* Windows Server 2012
* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003

### <a name="windows-server-2003-forest-functional-level-features"></a>Windows Server 2003 林功能级别功能

* 所有默认 AD DS 功能以及以下功能都可用：
   * 林信任
   * 域重命名
   * 链接值复制
      - 通过链接值复制，您可以更改组成员身份以存储和复制单个成员的值，而不是将整个成员身份作为一个单元复制。 在复制过程中，存储和复制单个成员的值会占用较少的网络带宽和更少的处理器周期，并可防止在不同域控制器中同时添加或删除多个成员时丢失更新。
   * 部署只读域控制器（RODC）的功能
   * 提高了知识一致性检查器（KCC）的算法和可伸缩性
      - 站点间拓扑生成器（ISTG）使用改进的算法，该算法可缩放以支持具有更多站点的林，AD DS 而不能支持在 Windows 2000 林功能级别上支持的站点。 改进后的 ISTG 选举算法是一种不太侵入的机制，用于选择 Windows 2000 林功能级别的 ISTG。
   * 在域目录分区中创建名为**dynamicObject**的动态辅助类的实例的功能
   * 能够将**inetOrgPerson**对象实例转换为**用户**对象实例，并以相反方向完成转换
   * 创建新组类型的实例以支持基于角色的授权的功能。 
      - 这些类型称为应用程序基本组和 LDAP 查询组。
   * 在架构中停用并重新定义属性和类别。 可重复使用以下属性： ldapDisplayName、schemaIdGuid、OID 和 mapiID。
   * 在 Windows Server 2008 模式下运行的基于域的 DFS 命名空间，其中包括对基于访问权限的枚举的支持和增加的可伸缩性。 有关详细信息，请参阅[选择命名空间类型](https://go.microsoft.com/fwlink/?LinkId=180400)。

### <a name="windows-server-2003-domain-functional-level-features"></a>Windows Server 2003 域功能级别功能

* 所有默认 AD DS 功能，Windows 2000 本机域功能级别提供的所有功能都可用：
   * 域管理工具，Netdom .exe，使你能够重命名域控制器
   * 登录时间戳更新
      * 使用用户或计算机的上次登录时间来更新 **lastLogonTimestamp** 属性。 可以在域内复制该属性。
   * 将**userPassword**属性设置为**inetOrgPerson**和 user 对象的有效密码的功能
   * 重定向用户和计算机容器的功能
      * 默认情况下，提供了两个已知的容器，用于容纳计算机和用户帐户，即 cn =<domain root> computer 和 cn =<domain root>Users。 此功能允许为这些帐户定义新的已知位置。
   * 授权管理器可以将其授权策略存储在 AD DS
   * 约束的委派
      * 约束委派使得应用程序可以通过基于 Kerberos 的身份验证来利用用户凭据的安全委派。
      * 只能将委派限制为特定的目标服务。
   * 选择性身份验证
      * 选择性身份验证使你可以从受信任林指定允许对信任林中资源服务器进行身份验证的用户和组。

## <a name="windows-2000"></a>Windows 2000

支持的域控制器操作系统：

* Windows Server 2008 R2
* Windows Server 2008
* Windows Server 2003
* Windows 2000

### <a name="windows-2000-native-forest-functional-level-features"></a>Windows 2000 本机林功能级别功能

* 所有默认 AD DS 功能都可用。

### <a name="windows-2000-native-domain-functional-level-features"></a>Windows 2000 本机域功能级别功能

* 所有默认 AD DS 功能和以下目录功能都可用，包括：
   * 分发组和安全组的通用组。
   * 组嵌套
   * 组转换，允许在安全组和通讯组之间进行转换
   * 安全标识符（SID）历史记录

## <a name="next-steps"></a>后续步骤

* [提升域功能级别](https://technet.microsoft.com/library/cc753104.aspx)  
* [提升林功能级别](https://technet.microsoft.com/library/cc730985.aspx)
