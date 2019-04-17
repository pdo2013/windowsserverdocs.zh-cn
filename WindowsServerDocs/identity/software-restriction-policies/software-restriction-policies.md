---
title: "软件限制策略"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies"></a>软件限制策略

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

此主题以供 IT 专业人员描述软件限制策略 (SRP)，Windows Server 2012 和 Windows 8 中，并提供有关 Windows Server 2003 SRP 开头 technical 信息的链接。

有关过程和疑难解答提示，请参阅[管理软件限制策略](administer-software-restriction-policies.md)和[疑难解答软件限制策略](troubleshoot-software-restriction-policies.md)。

## <a name="BKMK_OVER"></a>软件限制策略说明
软件限制策略 (SRP) 是基于组策略的功能，以标识某个域中，在计算机上运行的软件程序，并控制这些程序能够运行。 软件限制策略是 Microsoft security 和管理战略，以帮助企业增加可靠性、 完整性和可管理性其计算机的一部分。

你还可以使用软件限制策略若要创建受限高度对于你在其中允许仅明确识别运行的应用程序的计算机，配置。 软件限制策略集成与 Microsoft Active Directory 和组策略中。 你还可以单独的计算机上创建限制策略的软件。 软件限制策略是信任策略，即法规管理员设置以限制脚本和其他不完全受信任的运行的代码。

你可以向 Microsoft 管理控制台 (MMC) 定义本地组策略编辑器中或在本地安全策略贴靠该软件限制策略扩展通过这些策略。

有关 SRP 的详细信息，请参阅[软件限制策略 Technical 概述](software-restriction-policies-technical-overview.md)。

## <a name="BKMK_APP"></a>实际应用
管理员可以使用下面的任务软件限制策略：

-   什么是受信任的代码定义

-   脚本、 可执行文件和 ActiveX 控件，控制设计灵活的组策略

由操作系统和遵守限制策略的软件 （如脚本应用程序） 的应用程序强制软件限制的策略。

特别是，管理员可以使用软件限制策略为以下目标：

-   指定客户端上运行的软件 （可执行文件），可以

-   阻止用户共享计算机上运行特定程序

-   指定谁可以向客户端添加了受信任的发布者

-   设置 （指定策略影响所有用户或的客户端上的用户子集） 软件限制策略范围

-   在本地计算机、 部门 (OU)、 网站或域运行阻止可执行文件。 当你未与恶意用户使用软件对地址潜在问题的限制策略，这将是适用于情况。

## <a name="BKMK_NEW"></a>新的和更改功能
没有为限制策略的软件功能更改。

## <a name="BKMK_DEP"></a>已删除或不建议使用功能
没有已删除或过时限制策略的软件功能。

## <a name="BKMK_SOFT"></a>软件要求
可通过 MMC 访问软件限制策略扩展到本地组策略编辑器中。

要创建和维护本地计算机上的软件限制策略需要以下功能：

-   本地组策略编辑器

-   Windows 安装程序

-   验证码和 WinVerifyTrust

如果你设计要求域部署这些策略，除了上面列出，是必需的以下功能：

-   Active Directory 域服务

-   组策略

## <a name="BKMK_INSTALL"></a>服务器管理器信息
软件限制策略是一个扩展的本地组策略编辑器，而无法安装通过服务器管理器、 添加角色和功能。

## <a name="BKMK_LINKS"></a>请参阅
下表提供理解和使用 SRP 相关的资源的链接。

|键入的内容|引用|
|--------|-------|
|**产品评估**|[使用软件限制策略应用程序锁定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|
|**计划**|[软件限制策略 Technical 概述](software-restriction-policies-technical-overview.md)(Windows Server 2012)<br /><br />[软件限制策略技术参考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)(Windows Server 2003)|
|**部署**|没有可用的资源。|
|**操作**|[管理软件限制策略](administer-software-restriction-policies.md)(Windows Server 2012)<br /><br />[软件限制策略产品帮助](https://technet.microsoft.com/library/cc779607(v=WS.10).aspx)(Windows Server 2003)|
|**故障排除**|[疑难解答软件限制策略](troubleshoot-software-restriction-policies.md)(Windows Server 2012)<br /><br />[故障排除软件限制策略](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)(Windows Server 2003)|
|**安全**|[威胁和软件限制的对策策略](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)(Windows Server 2008)<br /><br />[威胁和软件限制的对策策略](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)(Windows Server 2008 R2)|
|**工具和设置**|[软件限制策略工具和设置](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)(Windows Server 2003)|
|**社区资源**|[使用软件限制策略应用程序锁定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|



