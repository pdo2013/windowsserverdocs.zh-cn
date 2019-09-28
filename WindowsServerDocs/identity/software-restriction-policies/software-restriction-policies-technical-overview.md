---
title: 软件限制策略技术概述
description: Windows Server 安全
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 293239c9f746f939b06d45d6e8c1a50b59e2bc43
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407134"
---
# <a name="software-restriction-policies-technical-overview"></a>软件限制策略技术概述

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主题介绍了软件限制策略、何时以及如何使用该功能、在过去版本中实现了哪些更改，并提供了指向其他资源的链接，以帮助你创建和部署从 Windows 开始的软件限制策略服务器2008和 Windows Vista。

## <a name="introduction"></a>介绍
软件限制策略为管理员提供了组策略驱动机制来识别软件并控制其在本地计算机上的运行能力。 这些策略可用于保护运行 Microsoft Windows 操作系统（从 Windows Server 2003 和 Windows XP Professional 开始）的计算机免受已知冲突，并保护计算机免受安全威胁（如恶意病毒）特洛伊木马程序。 你还可以使用软件限制策略创建计算机的高度受限配置，从而仅允许运行专门标识的应用程序。 软件限制策略与 Microsoft Active Directory 和组策略集成。 你也可以在独立计算机上创建软件限制策略。

软件限制策略是信任策略，也是管理员设置的规则，旨在限制未完全受信任的脚本和其他代码的运行。 本地组策略编辑器的软件限制策略扩展提供了一个用户界面，通过该界面，可以在本地计算机或整个域中管理用于限制应用程序使用的设置。

## <a name="procedures"></a>过程

-   [管理软件限制策略](administer-software-restriction-policies.md)

    -   [确定软件限制策略的允许-拒绝列表和应用程序清单](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [使用软件限制策略规则](work-with-software-restriction-policies-rules.md)

    -   [使用软件限制策略来帮助保护你的计算机免受电子邮件病毒侵害](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [软件限制策略疑难解答](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>软件限制策略使用方案
业务用户使用电子邮件、即时消息和对等应用程序进行协作。 随着这些协作的提高，尤其是在业务计算中使用 Internet，因此会受到恶意代码（如蠕虫、病毒和恶意用户或攻击者威胁）的威胁。

用户可能会从本机 Windows 可执行文件（.exe 文件）等多种形式接收恶意代码，并将其发送到文档中的宏（例如 .doc 文件）。 恶意用户或攻击者通常会使用社交工程方法来使用户运行包含病毒和蠕虫的代码。 （社交工程是引诱用户泄露其密码或某种形式的安全信息的一项术语。）如果激活了此类代码，它可以在网络上生成拒绝服务攻击，将敏感或私有数据发送到 Internet，将计算机的安全性置于风险之中，或损坏硬盘驱动器的内容。

IT 组织和用户必须能够确定哪些软件可以安全运行，哪些软件不能运行。 由于恶意代码可以使用大量和窗体，因此这会成为一件很困难的任务。

为了帮助保护其网络计算机免受恶意代码和未知或不受支持的软件的安全，组织可以将软件限制策略作为总体安全策略的一部分来实现。

管理员可以将软件限制策略用于以下任务：

-   定义什么是受信任代码

-   设计灵活的组策略来规范脚本、可执行文件和 ActiveX 控件

操作系统和符合软件限制策略的应用程序（如脚本操作应用程序）将强制实施软件限制策略。

特别地，管理员可以将软件限制策略用于以下目的：

-   指定可在客户端计算机上运行的软件（可执行文件）

-   防止用户在共享计算机上运行特定程序

-   指定可以将受信任的发布者添加到客户端计算机的人员

-   设置软件限制策略的范围（指定策略是影响客户端计算机上的所有用户还是用户子集）

-   防止可执行文件在本地计算机、组织单位 (OU)、网站或域中运行。 这适用于未使用软件限制策略解决恶意用户的潜在问题的情况。

## <a name="BKMK_Diffs_Changes"></a>功能差异和更改
对于 Windows Server 2012 和 Windows 8，SRP 中没有功能更改。

**支持的版本**

软件限制策略只能在至少运行 Windows Server 2003 的计算机上配置和应用，包括 windows Server 2012 和至少 Windows XP，包括 Windows 8。

> [!NOTE]
> 从 Windows Vista 开始，某些版本的 Windows 客户端操作系统没有软件限制策略。 组策略中未在域中管理的计算机可能收不到分布式策略。

**比较软件限制策略和 AppLocker 中的应用程序控制功能**

下表比较了软件限制策略（SRP）功能和 AppLocker 的特性和功能。

|应用程序控制功能|SRP|AppLocker|
|----------------|----|-------|
|范围|SRP 策略可以应用于从 Windows XP 和 Windows Server 2003 开始的所有 Windows 操作系统。|AppLocker 策略仅适用于 Windows Server 2008 R2、Windows Server 2012、Windows 7 和 Windows 8。|
|策略创建|SRP 策略通过组策略维护，只有 GPO 管理员才能更新 SRP 策略。 本地计算机上的管理员可以修改本地 GPO 中定义的 SRP 策略。|AppLocker 策略通过组策略维护，只有 GPO 管理员才能更新策略。 本地计算机上的管理员可以修改本地 GPO 中定义的 AppLocker 策略。<br /><br />AppLocker 允许自定义错误消息，从而将用户定向到某一网页寻求帮助。|
|策略维护|必须使用本地安全策略管理单元（如果本地创建策略）或组策略管理控制台（GPMC）更新 SRP 策略。|可以使用本地安全策略管理单元（如果本地创建策略）、GPMC 或 Windows PowerShell AppLocker cmdlet 来更新 AppLocker 策略。|
|策略应用程序|SRP 策略通过组策略分配。|AppLocker 策略通过组策略分配。|
|强制模式|SRP 在 "拒绝列表模式" 下工作，管理员可在此创建不想在此企业中允许的文件规则，而默认情况下允许运行文件的其余部分。<br /><br />SRP 还可以在 "允许列表模式" 中进行配置，这样，默认情况下，所有文件都将被阻止，管理员需要为要允许的文件创建允许规则。|默认情况下，AppLocker 在 "允许列表模式" 下运行，其中仅允许运行具有匹配允许规则的文件。|
|可控制的文件类型|SRP 可以控制以下文件类型：<br /><br />-可执行文件<br />-Dll<br />-脚本<br />-Windows 安装程序<br /><br />SRP 不能单独控制每个文件类型。 所有 SRP 规则都位于单个规则集合中。|AppLocker 可以控制以下文件类型：<br /><br />-可执行文件<br />-Dll<br />-脚本<br />-Windows 安装程序<br />-打包应用和安装程序（Windows Server 2012 和 Windows 8）<br /><br />AppLocker 维护每种文件类型的单独规则集合。|
|指定的文件类型|SRP 支持被视为可执行文件类型的可扩展列表。 管理员可以为应视为可执行文件的文件添加扩展。|AppLocker 不支持此功能。 AppLocker 当前支持以下文件扩展名：<br /><br />-可执行文件（.exe，.com）<br />-Dll （.ocx，.dll）<br />-Scripts （.vbs，.js，. ps1，.cmd，.bat）<br />-Windows 安装程序（.msi、.mst、.msp）<br />-打包的应用安装程序（.appx）|
|规则类型|SRP 支持四种类型的规则：<br /><br />-哈希<br />-路径<br />-签名<br />-Internet 区域|AppLocker 支持三种类型的规则：<br /><br />-哈希<br />-路径<br />-发布服务器|
|编辑哈希值|SRP 允许管理员提供自定义哈希值。|AppLocker 计算哈希值本身。 在内部，它对可移植的可执行文件（Exe 和 Dll）和 Windows 安装程序使用 SHA1 Authenticode 哈希，并为 rest 使用 SHA1 平面文件哈希。|
|支持不同的安全级别|使用 SRP administrators 可以指定应用可用于运行的权限。 因此，管理员可以配置一个规则，使记事本始终以受限权限运行，而永远不使用管理权限。<br /><br />Windows Vista 和更早版本上的 SRP 支持多个安全级别。 在 Windows 7 上，此列表仅限于两个级别：不允许和不受限制（基本用户转换为不允许）。|AppLocker 不支持安全级别。|
|管理打包应用和打包应用安装程序|不|.appx 是 AppLocker 可以管理的有效文件类型。|
|将规则定向到用户或用户组|SRP 规则适用于特定计算机上的所有用户。|AppLocker 规则可以针对特定用户或用户组。|
|规则异常支持|SRP 不支持规则异常|AppLocker 规则可以有例外，这些例外使管理员可以创建规则，例如 "允许来自 Windows 的所有内容（Regedit.exe 除外）"。|
|支持审核模式|SRP 不支持审核模式。 测试 SRP 策略的唯一方法是设置测试环境并运行几个试验。|AppLocker 支持审核模式，这使管理员能够在实际生产环境中测试策略的影响，而不会影响用户体验。 对结果感到满意后，即可开始实施策略。|
|支持导出和导入策略|SRP 不支持策略导入/导出。|AppLocker 支持导入和导出策略。 这允许你在示例计算机上创建 AppLocker 策略，测试该策略，然后导出该策略并将其导入到所需的 GPO。|
|规则强制|在内部，SRP 规则强制发生在不太安全的用户模式下。|在内部，Exe 和 Dll 的 AppLocker 规则在内核模式下强制实施，这比在用户模式下执行它们更安全。|

## <a name="system-requirements"></a>系统要求
只能在至少运行 Windows Server 2003 和 Windows XP 的计算机上配置和应用软件限制策略。 需要组策略才能分发包含软件限制策略组策略对象。

## <a name="software-restriction-policies-components-and-architecture"></a>软件限制策略组件和体系结构
软件限制策略为操作系统和符合软件限制策略的应用程序提供了一种机制，以限制软件程序的运行时执行。

在较高级别，软件限制策略包含以下组件：

-   软件限制策略 API。 应用程序编程接口（Api）用于创建和配置构成软件限制策略的规则。 还存在用于查询、处理和强制实施软件限制策略的软件限制策略 Api。

-   软件限制策略管理工具。 这包括**本地组策略对象编辑器**管理单元的**软件限制策略**扩展，管理员使用此扩展来创建和编辑软件限制策略。

-   一组操作系统 Api 和应用程序，它们调用软件限制策略 Api，以便在运行时强制实施软件限制策略。

-   Active Directory 和组策略。 软件限制策略取决于组策略基础结构，用于将软件限制策略从 Active Directory 传播到适当的客户端，并将这些策略的应用范围和筛选到适当的目标计算机。

-   Authenticode 和 WinVerify 信任 Api，它们用于处理签名的可执行文件。

-   事件查看器。 软件限制策略使用的函数将事件记录到事件查看器日志中。

-   策略的结果集（RSoP），有助于诊断将应用于客户端的有效策略。

有关 SRP 体系结构的详细信息，SRP 如何管理规则、进程和交互，请参阅 Windows Server 2003 技术库中的[软件限制策略的工作方式](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)。

## <a name="BKMK_Best_Practices"></a>最佳做法

### <a name="do-not-modify-the-default-domain-policy"></a>不要修改默认域策略。

-   如果你未编辑默认域策略，你始终可以选择重新应用默认域策略（如果你的自定义域策略出现问题）。

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>为软件限制策略创建一个单独的组策略对象。

-   如果为软件限制策略创建了单独的组策略对象（GPO），则可以在紧急情况下禁用软件限制策略，而不会禁用域策略的其余部分。

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>如果在应用策略设置时遇到问题，请在安全模式下重新启动 Windows。

-   在安全模式下启动 Windows 时，软件限制策略不适用。 如果你意外地使用软件限制策略锁定工作站，请在安全模式下重新启动计算机，以本地管理员身份登录，修改策略，运行**gpupdate**，重新启动计算机，然后以正常方式登录。

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>定义 "不允许" 的默认设置时请小心。

-   如果将默认设置定义为 "不**允许**"，则除已显式允许的软件之外，禁止所有软件。 要打开的任何文件都必须有一个允许它打开的软件限制策略规则。

-   若要防止管理员将自己锁定在系统之外，当默认安全级别设置为 "不**允许**" 时，系统会自动创建四个注册表路径规则。 可以删除或修改这些注册表路径规则;但是，不建议这样做。

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>为了获得最佳安全性，请将访问控制列表与软件限制策略结合使用。

-   用户可能会尝试通过重命名或移动禁止的文件或覆盖不受限制的文件来规避软件限制策略。 因此，建议使用访问控制列表（Acl）来拒绝用户执行这些任务所必需的访问权限。

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>在将策略设置应用于域之前，在测试环境中全面测试新的策略设置。

-   新策略设置的行为可能与最初预期的行为不同。 测试降低了在网络上部署策略设置时遇到问题的可能性。

-   你可以设置一个与组织域分离的测试域，用于测试新策略设置。 你还可以通过创建测试 GPO 并将其链接到测试组织单位来测试策略设置。 使用测试用户对策略设置进行了全面测试后，可以将测试 GPO 链接到域。

-   不要在不测试的情况下将程序或文件设置为 "不**允许**"，以查看其效果。 对某些文件的限制可能会严重影响您的计算机或网络的运行。

-   如果输入的信息不正确或键入错误，可能会导致策略设置不会按预期执行。 在应用新策略设置之前对其进行测试可能会导致意外的行为。

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>根据安全组中的成员身份筛选用户策略设置。

-   可以通过清除 GPO 的 "属性" 对话框的 "**安全**" 选项卡上的 "**应用组策略**和**读取**" 复选框，指定你不希望应用策略设置的用户或组。

-   当 "读取" 权限被拒绝时，计算机不会下载该策略设置。 因此，下载不必要的策略设置会使网络更快地运行，从而减少带宽消耗。 若要拒绝读取权限，请选择 "拒绝 **" 复选框**，该复选框位于 GPO 的 "属性" 对话框的 "**安全**" 选项卡上。

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>不要链接到另一个域或站点中的 GPO。

-   在另一个域或站点中链接到 GPO 可能会导致性能不佳。

## <a name="additional-resources"></a>其他资源

|内容类型|参考资料|
|--------|-------|
|**规划**|[软件限制策略技术参考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**操作**|[管理软件限制策略](administer-software-restriction-policies.md)|
|**疑难解答**|[软件限制策略故障排除（2003）](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**安全性**|[软件限制策略的威胁和对策（2008）](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[软件限制策略的威胁和对策（2008 R2）](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**工具和设置**|[软件限制策略工具和设置（2003）](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**社区资源**|[软件限制策略的应用程序锁定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


