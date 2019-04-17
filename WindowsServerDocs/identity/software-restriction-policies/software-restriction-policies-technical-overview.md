---
title: "软件限制策略 Technical 概述"
description: "Windows Server 安全"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="software-restriction-policies-technical-overview"></a>软件限制策略 Technical 概述

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本主题介绍软件限制策略，哪些更改何时以及如何使用此功能，已在过去的版本实现，并提供其他资源来帮助你创建和部署 Windows Server 2008 和 Windows Vista 的开头软件限制策略链接。

## <a name="introduction"></a>简介
软件限制策略提供了一组策略驱动种机制识别软件，并控制要在本地计算机上运行的管理员。 可以使用这些策略保护针对已知的冲突运行 Microsoft Windows 操作系统 （开头与 Windows Server 2003 和 Windows XP 专业版） 的计算机并保护的计算机免受恶意的病毒和特洛伊木马程序等安全威胁。 你还可以使用软件限制策略若要创建受限高度对于你在其中允许仅明确识别运行的应用程序的计算机，配置。 软件限制策略集成与 Microsoft Active Directory 和组策略中。 你还可以单独的计算机上创建限制策略的软件。

软件限制策略是信任策略，即法规管理员设置以限制脚本和其他不完全受信任的运行的代码。 在本地计算机上或域中的所有，软件限制策略扩展到本地组策略编辑器中提供了可通过这些管理限制的应用程序使用的设置的单个用户界面。

## <a name="procedures"></a>步骤

-   [管理软件限制策略](administer-software-restriction-policies.md)

    -   [确定软件限制策略允许拒绝列表和应用程序目录获利](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

    -   [适用于软件限制策略规则](work-with-software-restriction-policies-rules.md)

    -   [使用限制策略的软件来帮助保护你的计算机免受电子邮件病毒](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

-   [疑难解答软件限制策略](troubleshoot-software-restriction-policies.md)

## <a name="software-restriction-policy-usage-scenarios"></a>软件限制策略使用情况
商业用户通过使用电子邮件、 即时消息和到等应用程序进行协作。 随着这些协作增加，尤其是通过使用业务计算，对 internet 如此免受恶意的代码，如蠕虫、 病毒和恶意用户或威胁攻击的威胁。

用户可能会收到处于范围从原始的 Windows 可执行文件 （.exe 文件）、 到风光 （如.doc 文件）、 文档中的许多表单恶意代码到脚本 （如.vbs 文件）。 恶意用户或攻击者经常使用的社交工程方法，使用户运行包含病毒和蠕虫代码。 （社交工程为欺骗人脉暴露他们的密码或一些形式的安全信息的术语。）如果此类代码已激活，它可以生成拒绝服务网络上的攻击、 敏感或个人数据发送到 Internet、 危及的计算机的安全性，或损坏硬盘驱动器上的内容。

IT 部门和用户都必须是可以确定哪些软件安全地运行，并且它不是。 大量和恶意代码可以采取的表单，这将成为困难的任务。

为了帮助保护他们网络的计算机免受恶意代码和未知的或不受支持的软件，组织可以作为其整体的安全策略实现限制策略的软件。

管理员可以使用下面的任务软件限制策略：

-   什么是受信任的代码定义

-   脚本、 可执行文件和 ActiveX 控件，控制设计灵活的组策略

由操作系统和遵守限制策略的软件 （如脚本应用程序） 的应用程序强制软件限制的策略。

特别是，管理员可以使用软件限制策略为以下目标：

-   指定客户端计算机上运行的软件 （可执行文件），可以

-   阻止用户共享计算机上运行特定程序

-   指定人可以向客户端计算机添加受信任的发布者

-   设置 （指定策略影响所有用户或的客户端计算机上的用户子集） 软件限制策略范围

-   在本地计算机、 部门 (OU)、 网站或域运行阻止可执行文件。 当你未与恶意用户使用软件对地址潜在问题的限制策略，这将是适用于情况。

## <a name="BKMK_Diffs_Changes"></a>差异和功能发生更改
在 SRP 适用于 Windows Server 2012 和 Windows 8 的功能有任何更改。

**受支持的版本**

软件限制策略可以仅上配置并应用于至少运行计算机 Windows Server 2003，包括 Windows Server 2012 和至少 Windows XP 中，包括 Windows 8。

> [!NOTE]
> Windows Vista 与 Windows 客户端操作系统开头的某些版本不具有软件限制的策略。 不在某个域中管理由组策略的计算机可能无法接收分布式的策略。

**比较软件限制策略和 AppLocker 中的应用程序控件函数**

下表比较功能和软件限制策略 (SRP) 功能和 AppLocker 的功能。

|应用程序控件函数|SRP|AppLocker|
|----------------|----|-------|
|范围|可以对 Windows XP 和 Windows Server 2003 开始所有 Windows 操作系统应用 SRP 策略。|AppLocker 策略仅适用于 Windows Server 2008 R2、 Windows Server 2012、 Windows 7 和 Windows 8。|
|创建策略|通过组策略由 SRP 策略，并且仅在 gpo 管理员可以更新 SRP 策略。 在本地计算机上的管理员可以修改本地 GPO 中定义的 SRP 策略。|通过组策略由 AppLocker 策略，并且仅 GPO 的管理员可以更新的策略。 在本地计算机上的管理员可以修改本地 GPO 中定义的 AppLocker 策略。<br /><br />AppLocker 允许自定义的错误消息，以将定向到网页寻求帮助的用户。|
|策略的维护|通过使用本地安全策略管理单元 （如果策略创建本地） 或组策略管理控制台 (GPMC)，必须先更新 SRP 策略。|可以通过使用本地安全策略管理单元 （如果策略创建本地），或 GPMC 或 Windows PowerShell AppLocker cmdlet 更新 AppLocker 策略。|
|策略应用程序|通过组策略分布 SRP 策略。|通过组策略分布 AppLocker 策略。|
|强制模式|SRP 的工作方式"拒绝列表模式"管理员可以在其中创建规则他们不想允许此企业中，而其余部分的文件允许运行默认情况下的文件。<br /><br />此外可以在"允许列表模式"配置 SRP 以便默认阻止所有文件和管理员需要创建允许规则，他们想要允许的文件。|通过在"允许列表模式"允许仅将这些文件运行为其那里是匹配允许规则默认有效 AppLocker。|
|可以控制的文件类型|SRP 可以控制下面的文件类型：<br /><br />-可执行文件<br />-Dll<br />-脚本<br />Windows 安装程序<br /><br />SRP 不能单独控制每个文件类型。 所有 SRP 规则都是一个规则集锦中。|AppLocker 可以控制下面的文件类型：<br /><br />-可执行文件<br />-Dll<br />-脚本<br />Windows 安装程序<br />-打包应用并安装程序 （Windows Server 2012 和 Windows 8）<br /><br />AppLocker 维护的五个文件类型的每个单独的规则集锦。|
|指定的文件类型|SRP 支持被认为是可执行文件类型可扩展列表。 管理员可以添加的文件应该可执行的扩展。|AppLocker 不支持此。 AppLocker 当前支持以下文件扩展名：<br /><br />-可执行文件 （.com.exe）<br />-Dll （.dll.ocx)<br />-脚本 （.vbs、.js、.ps1、.cmd、.bat）<br />Windows 安装程序 （.msi、.mst、.msp）<br />-打包应用安装程序 (.appx)|
|规则类型|SRP 支持四种类型的规则：<br /><br />-希<br />-路径<br />-签名<br />-Internet 区域|AppLocker 支持规则的三种类型：<br /><br />-希<br />-路径<br />-发布者|
|编辑希值|SRP 允许管理员提供希自定义的值。|AppLocker 计算哈希代码值，本身。 内部它便携可执行文件 （Exe 和 Dll） 和 Windows 安装程序和其他 SHA1 直文件哈希使用 SHA1 验证码希。|
|不同的安全级别的支持|与 SRP 管理员可以指定运行应用可以使用的应用的权限。 因此，管理员可以配置规则此类该记事本始终运行受限权限和永远不会使用管理权限。<br /><br />在 Windows Vista 上及更早版本的 SRP 支持多个安全级别。 在 Windows 7 上，该列表已限制为仅两个级别： 不允许和无限制 （基本用户可以转换为不允许）。|AppLocker 不支持安全级别。|
|管理打包应用和打包应用安装程序|无法|.appx 是 AppLocker 可以管理的有效的文件类型。|
|定位给用户或一组用户规则|SRP 规则适用于所有用户的特定的计算机上。|AppLocker 规则可以针对特定用户或组的用户。|
|支持规则例外|SRP 不支持规则例外|AppLocker 规则产生异常允许管理员创建规则，如"允许所有内容从 Windows 除 Regedit.exe"。|
|支持的审核模式|SRP 不支持审核模式。 测试 SRP 策略的唯一方法是设置测试环境和运行几次的实验。|AppLocker 支持审核模式，它允许管理员测试真实生产环境中的他们策略的影响，不会影响用户体验。 一旦你结果感到满意，你可以启动履行策略。|
|有关导出和导入策略的支持|SRP 不支持策略导入月导出。|AppLocker 支持导入和导出的策略。 这允许你在计算机上示例，创建 AppLocker 策略出进行测试，导出该策略，然后重新导入所需的 GPO。|
|实施规则|在内部，SRP 规则执法发生以用户模式它不太安全。|内部，AppLocker 规则 Exe 和 Dll 强制执行以内核模式是比强制执行方式以用户模式更安全。|

## <a name="system-requirements"></a>系统要求
软件限制策略可以仅上配置并应用于至少运行计算机 Windows Server 2003，并且至少 Windows XP。 组策略要求以分散组策略对象包含软件限制策略。

## <a name="software-restriction-policies-components-and-architecture"></a>限制策略的软件组件和体系结构
软件限制策略提供的操作系统和应用程序软件限制策略符合限制软件程序运行时执行的机制。

在较高级别软件限制策略包含以下组件：

-   限制策略 API 的软件。 应用程序编程接口 (Api) 用于创建和配置构成软件限制策略规则。 还有一些软件限制策略查询、 处理，Api 和实施软件限制的策略。

-   软件限制策略管理工具。 这包括**软件限制策略**的扩展**本地组策略编辑器**在贴靠，用于创建和编辑软件限制策略哪些管理员。

-   一组 Api 操作系统和应用程序的呼叫软件限制策略提供强制软件限制策略，在运行时的 Api。

-   Active Directory 和组策略。 软件限制策略取决于传播软件限制策略从 Active Directory 给相应的客户端，以及范围和筛选这些策略应用到适当的目标计算机的组策略基础结构。

-   验证码，WinVerify 信任 Api 用于处理符号可执行文件。

-   事件查看器。 使用的软件限制策略日志事件事件查看器日志到的功能。

-   结果设置的策略 (RSoP)，这可以帮助有效将应用于客户端的策略的诊断。

有关详细信息 SRP 建筑如何 SRP 管理规则进程，交互，请参阅[软件限制策略的工作原理](https://technet.microsoft.com/library/cc786941(v=WS.10).aspx)Windows Server 2003 Technical 库中。

## <a name="BKMK_Best_Practices"></a>最佳做法

### <a name="do-not-modify-the-default-domain-policy"></a>不要修改默认域策略。

-   不要编辑默认域策略，如果你始终可以选择重新应用默认值域策略，如果发生错误与您的自定义的域策略。

### <a name="create-a-separate-group-policy-object-for-software-restriction-policies"></a>创建独立组策略对象限制策略的软件。

-   创建一个单独组策略对象 (的 GPO) 软件限制策略，如果你可以禁用域策略的其余部分不禁用软件限制策略，在紧急情况。

### <a name="if-you-experience-problems-with-applied-policy-settings-restart-windows-in-safe-mode"></a>如果你遇到问题的应用的策略设置，重新在安全模式下启动 Windows。

-   软件限制策略不适用于在安全模式下启动 Windows。 如果你不小心锁定使用软件限制策略工作站，重启计算机，在安全模式下、 本地管理员身份登录、 修改策略、 运行**gpupdate**、 重启计算机，并且通常登录。

### <a name="use-caution-when-defining-a-default-setting-of-disallowed"></a>定义不允许的默认设置时要小心。

-   当你定义默认设置为**不允许**，所有软件，则不都允许除外已明确允许的软件。 你想要打开的任何文件必须具有策略规则软件限制，使其打开。

-   若要防止管理员时的默认安全级别设置为将自行系统，利用**不允许**、 四个会自动创建注册表路径规则。 你可以删除或修改这些注册表路径规则;但是，这不是建议。

### <a name="for-best-security-use-access-control-lists-in-conjunction-with-software-restriction-policies"></a>为了获得最佳的安全性，软件限制策略结合使用控件的访问列表。

-   用户可能会试图避开软件限制策略重命名或移动禁止的文件或通过覆盖无限制的文件。 因此，建议你使用访问 acl) 来拒绝需执行这些任务的访问权限的用户。

### <a name="test-new-policy-settings-thoroughly-in-test-environments-before-applying-the-policy-settings-to-your-domain"></a>在测试环境全面测试新的策略设置应用到你的域的策略设置之前。

-   新的策略设置可能会与最初预期的不同操作。 测试会降低有机会使你的网络跨部署策略设置时遇到问题。

-   你可以设置测试域，不同于你的组织域，在其中来测试新的策略设置。 你还可以通过创建 GPO 一个测试，并将其链接到测试单位测试的策略设置。 全面测试用户使用的策略设置后，你可以将测试 GPO 链接到你的域。

-   不要将设置为的文件的程序或**不允许**测试查看效果可能的情况下。 某些文件限制非常可能会影响你的计算机或网络的操作。

-   输入错误的信息或键入了错误，可能会导致不执行按预期某个策略设置。 应用之前测试新的策略设置可以防止意外的行为。

### <a name="filter-user-policy-settings-based-on-membership-in-security-groups"></a>筛选基于安全组成员的用户策略设置。

-   你可以指定用户或组你不希望某个策略设置以通过清除应用**应用组策略**和**阅读**复选框，位于**安全**gpo 属性对话框中的选项卡。

-   朗读权限被拒绝，当计算机不下载的策略设置。 因此，较少使用带宽下载不必要的策略设置，这可以使网络更快地函数。 若要拒绝阅读权限，请选择**拒绝**为**阅读**复选框，位于**安全**gpo 属性对话框中的选项卡。

### <a name="do-not-link-to-a-gpo-in-another-domain-or-site"></a>没有链接到另一台域或网站中 GPO。

-   链接到另一台域或网站中 GPO 可能导致性能不佳。

## <a name="additional-resources"></a>更多资源

|键入的内容|引用|
|--------|-------|
|**计划**|[软件限制策略技术参考](https://technet.microsoft.com/library/cc728085(v=WS.10).aspx)|
|**操作**|[管理软件限制策略](administer-software-restriction-policies.md)|
|**故障排除**|[软件限制策略疑难解答 (2003)](https://technet.microsoft.com/library/cc737011(v=WS.10).aspx)|
|**安全**|[威胁和软件限制的对策策略 2008 （-）](https://technet.microsoft.com/library/dd349795(v=WS.10).aspx)<br /><br />[威胁和软件限制的对策策略 (2008 R2)](https://technet.microsoft.com/library/hh125926(v=WS.10).aspx)|
|**工具和设置**|[软件限制策略工具和设置 (2003)](https://technet.microsoft.com/library/cc782454(v=WS.10).aspx)|
|**社区资源**|[使用软件限制策略应用程序锁定](https://technet.microsoft.com/magazine/2008.06.srp.aspx?pr=blog)|


