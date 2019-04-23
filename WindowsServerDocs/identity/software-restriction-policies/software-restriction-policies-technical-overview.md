---
title: 软件限制策略技术概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7013b0-0efd-40fd-bd6d-75128adbd0b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: d007d55ced9c6a18581eaedb4edb66db9eeccab9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830848"
---
# <a name="software-restriction-policies-technical-overview"></a>软件限制策略技术概述

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本主题介绍了软件限制策略，何时以及如何使用功能，哪些更改了在过去的版本，并提供指向其他资源，以帮助您创建和部署软件限制策略从 Windows 开始Server 2008 和 Windows Vista。

## <a name="introduction"></a>简介
软件限制策略为管理员提供使用组策略驱动机制以确定软件并控制其本地计算机上运行的能力。 这些策略可用于保护运行 Microsoft Windows 操作系统 （Windows Server 2003 和 Windows XP Professional 的开头） 针对已知冲突的计算机，并保护计算机免受安全威胁，如恶意病毒和特洛伊木马程序。 你还可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 软件限制策略与 Microsoft Active Directory 和组策略集成。 你也可以在独立计算机上创建软件限制策略。

软件限制策略是信任策略，也是管理员设置的规则，旨在限制未完全受信任的脚本和其他代码的运行。 软件限制策略扩展到本地组策略编辑器提供一个用户界面可以通过其管理限制的应用程序使用的设置在本地计算机上或在域内。

## <a name="procedures"></a>过程

-   [管理软件限制策略](administer-software-restriction-policies.md)

    -   [确定允许-拒绝列表和应用程序清单的软件限制策略](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [使用软件限制策略规则](work-with-software-restriction-policies-rules.md)

    -   [使用软件限制策略来帮助保护计算机免受电子邮件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [软件限制策略疑难解答](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>软件限制策略使用方案
业务用户使用电子邮件、 即时消息和对等应用程序进行协作。 随着这些协作的增加，尤其是在企业计算和 Internet 使用，所以执行来自恶意代码，例如，蠕虫、 病毒和恶意用户或攻击者威胁的威胁。

用户可能会收到以多种形式，范围从本机 Windows 可执行文件 （.exe 文件） 到 （例如.doc 文件） 的文档中的宏的恶意代码 （例如.vbs 文件） 的脚本。 恶意用户或攻击者通常使用社会工程方法来获取用户运行包含病毒和蠕虫的代码。 （社会工程是欺骗用户暴露用户的密码或某种形式的安全信息的术语。）如果已激活此类代码，它可以生成在网络上的拒绝服务攻击、 向 Internet 发送敏感或私有数据、 使计算机的安全性面临风险，或破坏硬盘驱动器上的内容。

IT 组织和用户必须能够确定哪种软件可以安全地运行和它不是。 使用较大数字和恶意代码可以采取的窗体，这将成为一个困难的任务。

若要帮助保护其网络计算机免受恶意代码和未知或不受支持的软件，组织可以实现作为其整体安全策略的一部分的软件限制策略。

管理员可以将软件限制策略用于以下任务：

-   定义什么是受信任代码

-   设计灵活的组策略来规范脚本、可执行文件和 ActiveX 控件

操作系统和符合软件限制策略的应用程序（如脚本操作应用程序）将强制实施软件限制策略。

特别地，管理员可以将软件限制策略用于以下目的：

-   指定可在客户端计算机上运行的软件 （可执行文件）

-   防止用户在共享计算机上运行特定程序

-   指定谁可以向客户端计算机添加受信任的发行者

-   设置 （指定策略影响的所有用户还是客户端计算机上的用户的子集） 的软件限制策略的作用域

-   防止可执行文件在本地计算机、组织单位 (OU)、网站或域中运行。 这适用于未使用软件限制策略解决恶意用户的潜在问题的情况。

## <a name="BKMK_Diffs_Changes"></a>差异和功能更改
在 Windows Server 2012 的 SRP 和 Windows 8 中的功能有任何更改。

**支持的版本**

软件限制策略可以只上配置和应用于计算机至少运行 Windows Server 2003，包括 Windows Server 2012，并且至少包括 Windows 8 的 Windows XP。

> [!NOTE]
> 某些版本的 Windows 客户端操作系统开始使用 Windows Vista 没有软件限制策略。 不由组策略管理的域中的计算机可能不会收到分布式的策略。

**比较软件限制策略和 AppLocker 中的应用程序控制功能**

下表比较的特性和功能的软件限制策略 (SRP) 功能和 AppLocker。

|应用程序控制功能|SRP|AppLocker|
|----------------|----|-------|
|范围|SRP 策略可以应用于从 Windows XP 和 Windows Server 2003 开始的所有 Windows 操作系统。|AppLocker 策略仅适用于 Windows Server 2008 R2、 Windows Server 2012、 Windows 7 和 Windows 8。|
|创建策略|SRP 策略通过组策略以进行维护和只有 GPO 的管理员可以更新 SRP 策略。 在本地计算机上的管理员可以修改本地 GPO 中定义的 SRP 策略。|AppLocker 策略通过组策略以进行维护和只有 GPO 的管理员可以更新策略。 在本地计算机上的管理员可以修改本地 GPO 中定义的 AppLocker 策略。<br /><br />AppLocker 允许自定义错误消息，从而将用户定向到某一网页寻求帮助。|
|策略维护|必须通过使用本地安全策略管理单元中 （如果策略已本地创建） 或通过组策略管理控制台 (GPMC) 更新 SRP 策略。|可以通过使用本地安全策略管理单元 （如果策略已本地创建），或 GPMC 中或 Windows PowerShell AppLocker cmdlet 更新 AppLocker 策略。|
|策略应用程序|SRP 策略是通过组策略分发的。|通过组策略分发 AppLocker 策略。|
|强制模式|SRP 适用于"拒绝列表模式"管理员可以在其中创建规则的管理员不想将允许该企业中，而该文件的其余部分运行默认情况下允许的文件。<br /><br />此外可以配置的"允许列表模式"中的 SRP，默认情况下所有文件被都阻止，管理员需要创建允许他们想要允许的文件的规则。|AppLocker 通过中的"允许列表模式"允许这些文件仅运行允许对其进行那里是匹配的规则的默认工作原理。|
|可以控制的文件类型|SRP 可以控制以下文件类型：<br /><br />可执行文件<br />-Dll<br />-脚本<br />-Windows 安装程序<br /><br />SRP 不能单独控制每个文件类型。 所有 SRP 规则都都在单个规则集合。|AppLocker 可以控制以下文件类型：<br /><br />可执行文件<br />-Dll<br />-脚本<br />-Windows 安装程序<br />打包应用和安装程序 （Windows Server 2012 和 Windows 8）<br /><br />AppLocker 的五个文件类型的每个维护单独的规则集合。|
|指定的文件类型|SRP 支持可扩展的被认为是可执行文件的文件类型的列表。 管理员可以添加的文件应被视为可执行文件的扩展名。|AppLocker 不支持此操作。 AppLocker 目前支持以下文件扩展名：<br /><br />可执行文件 （.exe、.com）<br />-Dll （.ocx、.dll）<br />-脚本 （.vbs、.js、.ps1、.cmd、.bat）<br />-Windows 安装程序 （.msi、.mst、.msp）<br />-打包应用安装程序 (.appx)|
|规则类型|SRP 支持四种类型的规则：<br /><br />哈希<br />-   Path<br />-签名<br />-Internet 区域|AppLocker 支持通过三种类型的规则：<br /><br />哈希<br />-   Path<br />-   Publisher|
|编辑的哈希值|SRP 允许管理员提供自定义的哈希值。|AppLocker 计算的哈希值本身。 在内部它使用验证码 SHA1 哈希可移植可执行文件 （Exe 和 Dll） 和 Windows 安装程序和 rest 的 SHA1 平面文件哈希值。|
|支持不同的安全级别|具有 SRP 管理员可以指定可以运行应用程序的权限。 因此，管理员可以配置此类规则，记事本始终运行具有受限权限和永远不会使用管理权限。<br /><br />SRP 在 Windows Vista 及更早版本支持多个安全级别。 在 Windows 7 上该列表被限制为仅使用两个级别：不允许使用和不受限制的 （基本用户转换为不允许）。|AppLocker 不支持安全级别。|
|管理打包应用和打包应用安装程序|无法|.appx 是 AppLocker 可以管理的有效的文件类型。|
|目标规则，以便用户或用户组|SRP 规则适用于特定计算机上的所有用户。|AppLocker 规则可以针对特定用户或一组用户。|
|对规则例外的支持|SRP 不支持规则例外|AppLocker 规则可以允许管理员创建规则，如"允许的所有内容从 Windows 除 Regedit.exe"的异常。|
|对审核模式的支持|SRP 不支持审核模式。 测试 SRP 策略的唯一方法是设置测试环境和运行几个试验。|AppLocker 支持通过审核模式，这样管理员就能在实际生产环境中测试其策略的效果，而不会影响用户体验。 结果感到满意后，可以开始实施该策略。|
|对导出和导入策略的支持|SRP 不支持策略导入/导出。|AppLocker 支持通过导入和导出的策略。 这允许你创建的示例计算机上，AppLocker 策略测试然后导出该策略并导入回所需的 GPO。|
|规则强制|在内部，SRP 规则强制过程在用户模式这是不太安全。|在内部，这是比在用户模式下强制它们更安全在内核模式中强制 Exe 和 Dll 的 AppLocker 规则。|

## <a name="system-requirements"></a>系统要求
软件限制策略可以只上配置和应用于计算机至少运行 Windows Server 2003，并且至少 Windows XP。 需要使用组策略分发包含软件限制策略的组策略对象。

## <a name="software-restriction-policies-components-and-architecture"></a>软件限制策略组件和体系结构
软件限制策略提供的操作系统和应用程序符合软件限制策略来限制运行时执行的软件程序的机制。

在高级别中，软件限制策略包含以下组件：

-   软件限制策略 API。 应用程序编程接口 (Api) 用于创建和配置构成软件限制策略的规则。 还有一些软件限制策略用于查询、 处理、 Api 和强制实施软件限制策略。

-   软件限制策略管理工具。 这包括**软件限制策略**的扩展**本地组策略对象编辑器**中管理单元，它们是由管理员用于创建和编辑软件限制策略。

-   应用程序和操作系统 Api 的一组软件限制策略 Api 提供的软件限制策略在运行时强制执行调用的。

-   Active Directory 和组策略。 软件限制策略依赖于用于传播的软件限制策略在 Active Directory 中适当的客户端，以及指定作用域和筛选这些策略应用于相应的组策略基础结构目标计算机。

-   验证码和 WinVerify 信任的 Api 用于处理已签名的可执行文件。

-   事件查看器。 使用软件限制策略将事件记录到事件查看器日志的函数。

-   生成设置的策略 (RSoP)，这可以帮助将应用于客户端的有效策略的诊断。

详细了解 SRP 体系结构如何 SRP 管理规则、 进程和交互，请参阅[软件限制策略的工作原理](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)Windows Server 2003 技术库中。

## <a name="BKMK_Best_Practices"></a>最佳做法

### <a name="do-not-modify-the-default-domain-policy"></a>不要修改默认域策略。

-   如果你不要编辑默认域策略，始终必须重新应用默认域策略，如果出现问题与您的自定义的域策略的选项。

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>创建单独的组策略对象以软件限制策略。

-   如果创建软件限制策略的一个单独组策略对象 (GPO)，则可以禁用在紧急情况下的软件限制策略，但不能禁用您的域策略的其余部分。

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>如果遇到问题的应用的策略设置，重新启动在安全模式下的 Windows。

-   在安全模式下启动 Windows 时不适用于软件限制策略。 如果意外锁定工作站与软件限制策略，在安全模式下重新启动计算机，以本地管理员身份登录，修改策略，运行**gpupdate**，重新启动计算机，然后正常登录。

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>定义不允许的默认设置时要格外小心。

-   定义默认设置为时**不允许**，所有软件不允许都使用除已显式允许的软件。 你想要打开任何文件必须具有一个软件限制策略规则，允许其打开。

-   若要保护的管理员将自己锁定系统，超出时的默认安全级别设置为**不允许**、 四个注册表路径规则将自动创建。 可以删除或修改这些注册表路径规则;但是，这不是建议在一起。

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>为了获得最佳安全性，与软件限制策略一起使用访问控制列表。

-   用户可能会尝试规避软件限制策略，通过重命名或移动不允许的文件或覆盖不受限制的文件。 因此，建议使用访问控制列表 (Acl) 以拒绝用户执行这些任务所需的访问权限。

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>在测试环境中全面测试新的策略设置之前将策略设置应用于你的域。

-   新的策略设置可能不同于最初预期的行为。 测试，可以减少通过网络部署策略设置时遇到问题的可能性。

-   您可以设置测试域中，独立于你组织的域，用于测试新的策略设置。 此外可以通过创建测试 GPO 并将其链接到测试组织单位测试的策略设置。 时已全面测试的测试用户的策略设置，可以将测试 GPO 链接到你的域。

-   未设置程序或文件复制到**不允许**没有经过测试，请参阅效果可能是。 限制对某些文件可能会严重影响您的计算机或网络的操作。

-   输入不正确的信息或者键入了错误可能导致不会按预期方式执行策略设置。 将其应用之前测试新的策略设置可避免意外的行为。

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>筛选基于安全组的成员身份的用户策略设置。

-   你可以指定用户或组你不想要通过清除应用的策略设置**应用组策略**并**读取**复选框，位于**安全**GPO 的属性对话框的选项卡。

-   当读取权限被拒绝时，计算机不被下载的策略设置。 因此，较少带宽都会使用下载不必要的策略设置，从而使网络更快地运行。 若要拒绝读取权限，请选择**拒绝**有关**读取**复选框，位于**安全**的 GPO 的属性对话框的选项卡。

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>不链接至另一个域或站点中的 GPO。

-   将链接到另一个域或站点中的 GPO 可能会导致性能不佳。

## <a name="additional-resources"></a>其他资源

|内容类型|参考|
|--------|-------|
|**规划**|[软件限制策略技术参考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**操作**|[管理软件限制策略](administer-software-restriction-policies.md)|
|**疑难解答**|[软件限制策略故障排除 (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**安全性**|[威胁和对策的软件限制策略 (2008)](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[威胁和对策的软件限制策略 (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**工具和设置**|[软件限制策略工具和设置 (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**社区资源**|[软件限制策略的应用程序锁定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


