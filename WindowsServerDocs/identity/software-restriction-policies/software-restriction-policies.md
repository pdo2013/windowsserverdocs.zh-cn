---
title: 软件限制策略
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c0befad-07c3-4262-b418-372d01850305
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: ab44013b947d33adc12c54b527415bf16c46a4c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875818"
---
# <a name="software-restriction-policies"></a>软件限制策略

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

面向 IT 专业人员的本主题介绍在 Windows Server 2012 和 Windows 8 中的软件限制策略 (SRP)，并提供了有关从开始 Windows Server 2003 的 SRP 的技术信息的链接。

过程和故障排除提示，请参阅[管理软件限制策略](administer-software-restriction-policies.md)并[解决软件限制策略](troubleshoot-software-restriction-policies.md)。

## <a name="BKMK_OVER"></a>软件限制策略描述
软件限制策略 (SRP) 是基于组策略的功能，用于标识在域中的计算机上运行的软件程序，以及控制这些程序的运行能力。 软件限制策略属于 Microsoft 安全和管理战略，旨在帮助企业提高计算机的可靠性、完整性和可管理性。

你还可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 软件限制策略与 Microsoft Active Directory 和组策略集成。 你也可以在独立计算机上创建软件限制策略。 软件限制策略是信任策略，也是管理员设置的规则，旨在限制未完全受信任的脚本和其他代码的运行。

通过本地组策略编辑器或 Microsoft Management Console (MMC) 的本地安全策略管理单元的软件限制策略扩展，可以定义策略。

有关 SRP 的详细信息，请参阅 [Software Restriction Policies Technical Overview](software-restriction-policies-technical-overview.md)。

## <a name="BKMK_APP"></a>实际应用程序
管理员可以将软件限制策略用于以下任务：

-   定义什么是受信任代码

-   设计灵活的组策略来规范脚本、可执行文件和 ActiveX 控件

操作系统和符合软件限制策略的应用程序（如脚本操作应用程序）将强制实施软件限制策略。

特别地，管理员可以将软件限制策略用于以下目的：

-   指定可在客户端上运行的软件（可执行文件）

-   防止用户在共享计算机上运行特定程序

-   指定可以将受信任发布程序添加到客户端的人员

-   设置软件限制策略的范围（指定策略是影响客户端上的所有用户还是影响某个用户子集）

-   防止可执行文件在本地计算机、组织单位 (OU)、网站或域中运行。 这适用于未使用软件限制策略解决恶意用户的潜在问题的情况。

## <a name="BKMK_NEW"></a>新的和更改功能
软件限制策略无功能更改。

## <a name="BKMK_DEP"></a>已删除或弃用的功能
软件限制策略无删除或弃用的功能。

## <a name="BKMK_SOFT"></a>软件要求
可以通过 MMC 访问本地组策略编辑器的软件限制策略扩展。

需要以下功能，才能在本地计算机上创建和维护软件限制策略：

-   本地组策略编辑器

-   Windows Installer

-   Authenticode 和 WinVerifyTrust

如果设计调用这些策略的域部署，则除了上述列表外，还需要以下功能：

-   Active Directory 域服务

-   组策略

## <a name="BKMK_INSTALL"></a>服务器管理器信息
软件限制策略是本地组策略编辑器的扩展，因此未通过“服务器管理器”、“添加角色和功能”安装。

## <a name="BKMK_LINKS"></a>另请参阅
下表提供了有关了解和使用 SRP 的相关资源的链接。

|内容类型|参考|
|--------|-------|
|**产品评估**|[软件限制策略的应用程序锁定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**规划**|[软件限制策略技术概述](software-restriction-policies-technical-overview.md)(Windows Server 2012)<br /><br />[软件限制策略技术参考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx) (Windows Server 2003)|
|**部署**|无资源可用。|
|**操作**|[管理软件限制策略](administer-software-restriction-policies.md)(Windows Server 2012)<br /><br />[软件限制策略产品帮助](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx) (Windows Server 2003)|
|**疑难解答**|[软件限制策略疑难解答](troubleshoot-software-restriction-policies.md)(Windows Server 2012)<br /><br />[解决软件限制策略问题](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx) (Windows  Server 2003)|
|**安全性**|[软件限制策略的威胁和对策](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx) (Windows  Server 2008)<br /><br />[软件限制策略的威胁和对策](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx) (Windows Server 2008 R2)|
|**工具和设置**|[软件限制策略工具和设置](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx) (Windows Server 2003)|
|**社区资源**|[软件限制策略的应用程序锁定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



