---
title: 开启遵守适用于 Windows Server 2016 的一般数据保护条例 (GDPR) 之旅
description: 通过本文了解 GDPR 是什么以及 Microsoft 提供的产品如何帮助你向实现合规性的目标迈进。
keywords: 隐私, GDPR
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.technology:
- techgroup-security
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
ms.openlocfilehash: be9509de0291924bb95733f995b447230bb75214
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870128"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>对于 Windows Server 从开始，一般数据保护条例 (GDPR) 之旅 

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本文提供有关 GDPR 的信息，包括它是什么以及 Microsoft 提供的产品如何帮助你实现合规性。

## <a name="introduction"></a>简介
欧洲隐私法将于 2018 年 5 月 25 日生效，它为隐私权利、安全性和合规性树立了新的全球标准。

一般数据保护条例（简称为 GDPR）从根本上讲是关于保护和实现个人隐私权利的法律。 GDPR 制定了严格的全球隐私要求，旨在监督你在尊重个人选择的同时管理和保护个人数据的方式 — 无论在何处发送、处理或存储数据。

Microsoft 和我们的客户正在为实现 GDPR 的隐私目标而努力。 在 Microsoft，我们坚信隐私是一项基本权利，并且 GDPR 是为阐释和实现个人隐私权利迈出的重要一步。 但是，我们还知道 GDPR 将要求世界各地的组织做出重大改变。

在[借助 Microsoft 云实现 GDPR 合规性](https://blogs.microsoft.com/on-the-issues/2017/02/15/get-gdpr-compliant-with-the-microsoft-cloud/#hv52B68OZTwhUj2c.99)博客（由我们的首席隐私官 [Brendon Lynch](https://blogs.microsoft.com/on-the-issues/author/brendonlynch/)发表）和[通过对一般数据保护条例的合同承诺赢得信任](https://blogs.microsoft.com/on-the-issues/2017/04/17/earning-trust-contractual-commitments-general-data-protection-regulation/#6QbqoGWXCLavGM63.99)博客（由 [Rich Sauer](https://blogs.microsoft.com/on-the-issues/author/rsauer/) - Microsoft 公司副总裁兼副总法律顾问发表）中，我们概述了我们对 GDPR 的承诺以及如何为客户提供支持。

虽然你的 GDPR 合规性之旅似乎充满了挑战，但我们会竭尽所能地为你提供帮助。 有关 GDPR 的具体信息、我们的承诺以及如何开启你的旅程，请访问 [Microsoft 信任中心的 GDPR 部分](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr)。

## <a name="gdpr-and-its-implications"></a>GDPR 及其含义
GDPR 是一项复杂法规，可能要求你对收集、使用和管理个人数据的方式进行重大更改。 Microsoft 在帮助客户遵守复杂法规方面拥有很长的历史，当你为遵守 GDPR 做准备时，我们将伴你一路同行。

GDPR 为向欧盟 (EU) 民众提供商品和服务或者负责收集和分析欧盟居民的相关数据的组织制定了规则，而无论这些企业位于何处。 GDPR 的关键要素如下所示：

- **增强的个人隐私权。** 通过确保欧盟居民有权访问其个人数据、纠正该数据的不准确之处、擦除该数据、反对处理其个人数据以及移动数据，加强对欧盟居民的数据保护。

- **用于保护个人数据的增加的关税。** 加强负责处理个人数据的组织的责任制，提高责任透明度，并确保遵守相关法规。

- **必需的个人数据违规报告。** 掌握个人数据的组织必须向监管机构报告对个人权利和自由造成风险的个人数据违规行为，而不得拖延，并且必须在知道违规行为的 72 小时内进行报告（如果可行）。

正如你所料，GDPR 可能会对你的业务产生重大影响，可能要求你更新隐私政策，实施和强化数据保护控制和违规通知程序，部署高度透明的政策，并加大在 IT 和培训方面的投资。 Microsoft Windows 10 可以帮助你有效且高效地满足某些需求。

## <a name="personal-and-sensitive-data"></a>个人数据和敏感数据
作为为遵守 GDPR 所做努力的一部分，你需要了解该法规如何定义个人数据和敏感数据以及这些定义与贵组织保留的数据有何关联。 基于你将能够发现其中创建该数据，处理，了解管理和存储。

GDPR 将个人数据定义为已确定身份或可确定身份的自然人的任何相关信息。 这可以包括直接身份（例如你的法定名称）和间接身份（例如，明确指出你是数据所指对象的特定信息）。 GDPR 还明确指出个人数据的概念包括在线标识符（如 IP 地址、移动设备 ID）和位置数据。

GDPR 引入遗传数据（如个人基因序列）和生物识别数据的具体定义。 遗传数据和生物识别数据以及其他子类别的个人数据（揭示种族或民族血统、政治观点、宗教或哲学信仰或工会会员资格的个人数据：有关健康的数据；或有关个人性生活或性取向的数据）均被 GDPR 视为敏感个人数据。 需要加强对敏感个人数据的保护，一般要求个人明确同意在何处处理这些数据。

### <a name="examples-of-info-relating-to-an-identified-or-identifiable-natural-person-data-subject"></a>已确定身份或可确定身份的自然人的相关信息示例（数据主体）
此列表中提供以下几种将通过 GDPR 进行监管的信息示例。 此列表并不详尽。

-   名称

-   标识号（例如 SSN)

-   位置数据（如家庭地址）

-   在线标识符（如电子邮件地址、屏幕名称、IP 地址、设备 ID）

-   匿名数据（如使用密钥来识别个人）

-   遗传数据（如来自个人的生物标本）

-   生物识别数据（如指纹、面部识别）

## <a name="getting-started-on-the-journey-towards-gdpr-compliance"></a>开启实现 GDPR 合规性的旅程
考虑到遵守 GDPR 法规的程度，我们强烈建议你不要等到强制执行才开始准备。 你现在应审查你的隐私和数据管理做法。 我们建议你通过关注以下四个关键步骤来开启实现 GDPR 合规性的旅程：

-   **发现。** 识别你拥有的个人数据及其所在的位置。 

-   **管理。** 管理使用和访问个人数据的方式。

-   **保护。** 制定安全控制措施来防止、检测和响应安全漏洞和数据泄露。  

-   **报表。** 处理数据请求、报告数据泄漏和保留所需的文档。

    ![关于 4 个主要 GDPR 步骤如何协同工作的图示](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

对于每个步骤，我们已经概述了各种 Microsoft 解决方案中的示例工具、资源以及功能，它们可用来帮助你满足该步骤的要求。 尽管本文并不全面的"操作方法"指南，我们提供了您若要了解更多详细信息的链接和详细信息现已推出[GDPR 部分 Microsoft 信任中心](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr)。

## <a name="windows-server-security-and-privacy"></a>Windows Server 安全和隐私
GDPR 要求您实现适当的技术和组织安全措施来保护个人数据和处理系统。 在 GDPR 的上下文中，物理和虚拟服务器环境可能的处理私人和敏感数据。 处理可以表示任何操作或组的操作，如数据收集、 存储和检索。

您能够为满足此要求，并且可以实现适当的技术安全措施必须反映在当今日益恶意 IT 环境中面临的威胁。 如今的安全威胁环境是具有攻击性且顽固的威胁之一。 过去几年，恶意攻击者主要专注于通过其攻击获得社群认同，或因暂时使系统离线而获得的兴奋感。 从那时起，攻击者的动机已经转向获取金钱，包括将设备和数据作为抵押，直到所有者支付要求的赎金。

现代攻击日益集中于大规模的知识产权盗窃；可能导致财务损失的目标系统性能降低；甚至会威胁全世界的个人、企业和国家/地区利益的网络恐怖主义。 此类攻击者通常是训练有素的个人和安全专家，其中一些人受雇于国家/地区，这些国家/地区拥有大额预算和似乎无限的人力资源。 这样的威胁需要可以解决这一挑战的方法。

这些威胁不仅威胁到你全面掌控所拥有的任何个人数据或敏感数据的能力，而且对你的整体业务也构成了重大风险。 请考虑从 Institute McKinsey，Ponemon、 Verizon、 和 Microsoft 最新数据：

- GDPR 预计报告的数据泄露的平均成本是 350 万美元。

- 其中 63％ 的泄露涉及 GDPR 希望你解决的弱密码或密码被盗问题。

- 每天创建和传播超过 300,000 个新的恶意软件样本，这让你解决数据保护问题的任务变得更具挑战性。

最新的勒索软件攻击，一次调用 Internet，黑色困扰常见于攻击者将后可以承受付款详细信息，与可能的灾难性后果的更大目标。 GDPR 包含使您的系统，包括台式计算机和便携式计算机，包含私人和敏感数据确实丰富目标的损失。

两个关键原则有指导，并继续引导的 Windows 开发：

- **安全性。** 我们的软件和服务代表我们的客户所存储的数据应受到危害和使用或仅以适当的方式修改。 安全模型应便于开发人员了解和构建到其应用程序。

- **隐私。** 用户应如何使用其数据的控件中。 信息使用的策略应明确告知用户。 用户应控制时，如果它们接收信息来充分利用其时间。 它应该是便于用户能够指定他们发送其信息包括控制电子邮件使用的正确使用。

Microsoft 一直坚定针对最近由 Microsoft 的首席执行官 Satya Nadella 记下这些原则 

> "_随着世界上不断变化和业务需求的发展，是一致的一些事项： 对安全和隐私的客户的需求。_"

在遵守 GDPR，了解角色中创建、 访问、 处理、 存储和管理可能会被视为个人数据的物理和虚拟服务器和潜在的敏感数据在 GDPR 下都很重要。 Windows Server 提供了功能，可帮助你符合 GDPR 要求才能实现适当的技术和组织安全措施来保护个人数据。

Bolt 上; 不是 Windows Server 2016 的安全状况它是一个体系结构原则。 而且，在四个主体可以最好地理解：

- **保护。** 正在进行的焦点和预防措施; 上的创新阻止已知的攻击和已知的恶意软件。

- **检测到。** 全面监视工具来帮助您发现异常情况、 响应更快的攻击。

- **响应。** 领先的响应和恢复技术和咨询性专业知识的深度。

- **隔离。** 隔离操作系统组件和数据密钥、 限制管理员权限，以及严格测量主机运行状况。

使用 Windows Server，大大提高您的能力来保护、 检测和抵御攻击可能会导致数据泄漏的类型。 鉴于 GDPR 对违规通知有着严格要求，确保台式机和便携式计算机系统得到良好保护将降低你所面临的可能导致成本高昂的违规分析和通知的风险。

在后面的部分，您将看到 Windows Server 如何提供直接满足 gdpr 符合性的"保护"阶段中的功能。 这些功能分为三个保护方案：

- **保护你的凭据，并限制管理员权限。** Windows Server 2016 可帮助实现这些更改，以帮助防止您的系统用作进一步入侵的启动点。

- **保护操作系统以运行应用程序和基础结构。** Windows Server 2016 提供保护，这有助于阻止运行恶意软件或利用漏洞的外部攻击者的层。

- **安全虚拟化。** Windows Server 2016 可以实现安全虚拟化，使用受防护的虚拟机和受保护的构造。 这有助于进行加密，并在你的结构，更好地保护它们免受恶意攻击的受信任主机上运行你的虚拟机。

对特定 GDPR 要求，引用与下面的更详细地讨论这些功能都基于可帮助保持完整性和安全性的操作系统和数据的高级的设备保护。

GDPR 中的密钥预配是设计并且默认情况下，数据保护，帮助您能以满足此设置是 BitLocker 设备加密等的 Windows 10 中的功能。 BitLocker 使用受信任的平台模块 (TPM) 技术，它提供基于硬件的、 与安全相关的函数。 此加密处理器芯片包括多个物理安全机制，以使其篡改，并且恶意软件无法篡改 TPM 的安全功能。

该芯片包含多个物理安全机制以使其防篡改，并且恶意软件无法篡改 TPM 的安全功能。 使用 TPM 技术的一些主要优点是你可以实现以下目的：

-   生成、存储和限制使用加密密钥。

-   使用刻录到自身的 TPM 的唯一 RSA 密钥，可以将 TPM 技术用于平台设备身份验证。

-   通过采用并存储安全措施可帮助确保平台的完整性。

与你的无数据泄露操作相关的其他高级设备保护包括 Windows 受信任启动，它通过确保在系统防御之前恶意软件无法启动来帮助维护系统的完整性。

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server:支持 gdpr 符合性
Windows Server 内的主要功能可以帮助您有效地实现 GDPR 符合性要求的安全和隐私机制。 而使用这些功能不会保证您的符合性，它们将支持你的工作以执行此操作。

服务器操作系统位于组织的基础结构，使新的机会，若要创建的保护层免受攻击可能窃取数据和中断你的业务中的战略层。 通过设计、 数据保护和访问控制的隐私如 GDPR 的关键方面，必须先解决你在服务器级别的 IT 基础结构。

Windows Server 2016 致力于帮助保护标识、 操作系统和虚拟化层，有助于阻止使用非法访问您的系统的常见攻击平台： 被盗凭据、 恶意软件，以及遭到入侵的虚拟化结构。 除了降低业务风险，安全组件内置到 Windows Server 2016 帮助满足合规性要求的重要政府和行业安全法规。 

这些标识、 操作系统和虚拟化的保护功能，您可以更好地保护你的数据中心运行 Windows Server 作为任何云中的 VM 和限制攻击者能够破坏凭据、 启动恶意软件，并保持在未检测到了你网络。 同样，部署为 HYPER-V 主机时，Windows Server 2016 提供了受防护的虚拟机和分布式的防火墙功能通过在虚拟化环境的安全保障。 使用 Windows Server 2016 server 操作系统变得数据中心安全性的积极参与者。

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>保护你的凭据，并限制管理员权限 
控制对个人数据的访问和处理这些数据的系统是符合 GDPR 具有特定要求，包括由管理员访问权限的区域。 特权的标识是任何具有提升权限，例如是域管理员、 企业管理员、 本地管理员或甚至 Power Users 组的成员的用户帐户的帐户。 此类标识还可以包含已被授予的权限直接，如执行备份，关闭系统或本地安全策略控制台中的用户权限分配节点中列出的其他权限的帐户。

作为常规访问控制原则和行符合 GDPR，你需要防止潜在的攻击者造成的泄露这些特权的标识。 首先，务必要了解如何标识已泄露时然后您可以计划以防止攻击者获得对这些特权标识访问权限。

#### <a name="how-do-privileged-identities-get-compromised"></a>如何特权的标识遭到了破坏？
当组织没有准则，以对其进行保护时，获取泄漏特权的标识。 下面是一些示例：

- **非必要的更多权限。** 最常见的问题之一是，用户具有多是执行其工作职责所必需的权限。 例如，管理 DNS 的用户可能是 AD 管理员。 大多数情况下，这样做是为了避免需要在要配置不同的管理级别。 但是，如果此类帐户受到攻击，攻击者会自动具有提升的权限。

- **不断地登录所使用提升的权限。** 另一个常见问题是，使用提升的权限的用户可以使用它无时间限制。 这是很常见的 IT 专业人员登录到使用特权的帐户的桌面计算机、 保持登录，并使用特权的帐户来浏览 web 和使用电子邮件 (典型的 IT 工作工作职能)。 特权帐户的无限制的持续时间使帐户更易受攻击并提高帐户会遭到破坏的几率。

- **社会工程研究。** 大多数凭据威胁首先研究组织，然后通过社交工程进行。 例如，攻击者可能会执行的电子邮件网页仿冒攻击到泄露合法帐户 （但不是一定是提升权限的帐户） 有权访问组织的网络。 若要在网络上执行额外的研究并确定可以执行管理任务的特权的帐户，攻击者然后使用这些有效帐户。 

- **利用使用提升的权限的帐户。** 即使使用网络中的普通的、 非提升的用户帐户，攻击者可以访问帐户使用提升的权限。 这样做的更常用的方法之一因此是通过传递哈希或传递令牌攻击。 传递的哈希和其他凭据盗窃技术的详细信息，请参阅[传递的哈希 (PtH) 页](https://technet.microsoft.com/dn785092.aspx)。

当然有其他方法的攻击者可以使用标识和损坏特权的标识 （使用每日创建的新方法）。 因此，重要建立用户使用最低特权帐户，以减少攻击者能够访问特权标识登录的做法是。 以下各节概述了 Windows Server，可以降低这些风险的功能。

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>在实时 (JIT) 管理和 Just Enough Admin (JEA)
虽然对传递哈希或传递票证攻击中保护是重要的是，仍可以通过其他方式，包括社会工程、 不满的员工和暴力破解被盗管理员凭据。 因此，除了隔离尽可能多的凭据，您还需要一种方法来限制管理员级别特权的覆盖范围，如果它们遭到破坏。

今天，过多的管理员帐户是责任的过度特权，即使它们具有只有一个区域中。 例如，DNS 管理员，用户需要一组非常有限的权限来管理 DNS 服务器，通常被授予域管理员级别权限。 此外，这些凭据为永久免费授予的因为没有任何限制在多长时间使用。

每个帐户使用不必要的域管理员级别权限会增加攻击者设法侵入凭据所造成的危害。 为了尽量减少对攻击面，你想要提供只有特定组的权限的管理员需要完成这项工作 – 和仅对完成其所需的时间窗口。

使用 Just Enough Administration 和中实时管理，管理员可以请求所需的确切所需的时间窗口的特定特权。 DNS 管理员，例如，使用 PowerShell 启用 Just Enough Administration 可以创建一组有限的命令所提供的 DNS 管理。

如果 DNS 管理员需要对她服务器之一进行更新，她将请求 DNS 使用 Microsoft Identity Manager 2016 的管理访问权限。 请求工作流可以包含如双因素身份验证，可以调用要授予所请求的权限之前确认其身份的管理员的移动电话的批准过程。 一旦获得授权，这些 DNS 权限提供访问权限的 PowerShell 角色 DNS 的特定时间跨度。

如果 DNS 管理员凭据被盗，请设想以下场景。 首先，因为凭据已附加到它们没有管理员权限，攻击者将无法访问 DNS 服务器 – 或任何其他系统 – 若要进行任何更改。 如果攻击者尝试请求对 DNS 服务器的权限，第二重身份验证会要求他们确认其身份。 因为它不可能使攻击者 DNS 管理员的移动电话，身份验证将失败。 这将锁定从系统中，攻击者并发出警报的 IT 组织的凭据可能会泄露。

此外，许多组织使用免费[本地管理员密码解决方案 (LAPS)](http://aka.ms/laps)作为其服务器和客户端系统的简单而又强大 JIT 管理机制。 LAPS 功能提供了管理加入域的计算机的本地帐户密码。 密码存储在 Active Directory (AD) 和受访问控制列表 (ACL)，因此仅有资格的用户可以读取它，或请求其重置。

如中所述[Windows 凭据被盗缓解指南](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095)， 

> "_工具和技术罪犯使用来执行凭据被盗和重复使用攻击改进、 恶意攻击者发现，在具有更轻松地实现其目标。凭据被盗通常依赖于操作实践或用户凭据公开，因此有效的缓解措施需要解决了人员、 流程和技术的整体方法。此外，这些攻击取决于攻击者破坏系统展开或一直持续访问权限，因此，组织必须通过实现策略，阻止攻击者自由地移动中未检测到快速包含违规后窃取的凭据受攻击的网络。_"

Windows Server 的一个重要的设计考虑因素缓解凭据被盗，特别是，派生的凭据。 Credential Guard 通过在设计用于帮助消除基于硬件的隔离攻击的 Windows 中实现显著的体系结构更改而不只尝试提供显著改进了的安全性，以防止派生的凭据被盗和重复使用对其进行保护。

使用 Windows Defender Credential Guard、 NTLM 和 Kerberos 使用基于虚拟化的安全性来保护派生的凭据，凭据被盗攻击技术和工具中使用时被阻止多个有针对性的攻击。 在带有管理权限的操作系统中运行的恶意软件无法提取受基于虚拟化的安全性保护的密钥。 虽然 Windows Defender Credential Guard 是功能强大的缓解措施，持久性威胁攻击的将可能转变为新的攻击技术，你还应涵盖 Device Guard，如下所述，以及其他安全策略和体系结构。

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard 使用基于虚拟化的安全性来隔离凭据信息不被他人截获阻止密码哈希或 Kerberos 票证。 它使用一个全新的独立本地安全机构 (LSA) 进程，它不能访问操作系统的其余部分。 使用独立的 LSA 的所有二进制文件使用在启动中受保护的环境、 进行传递哈希类型攻击完全无效之前验证的证书进行签名。

使用 Windows Defender Credential Guard:

- 基于虚拟化的安全性 （必需）。 其他要求：

    - 64 位 CPU

    - CPU 虚拟化扩展和扩展的页表

    - Windows 虚拟机监控程序

- 安全启动（必需）

- TPM 2.0，离散或固件（首选 - 提供硬件绑定）

可以使用 Windows Defender Credential Guard 以帮助保护特权的标识来保护凭据和 Windows Server 2016 上的凭据派生对象。 Windows Defender Credential Guard 要求的详细信息，请参阅[保护派生的域凭据与 Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender 远程 Credential Guard
Windows Defender 远程 Credential Guard，在 Windows Server 2016 和 Windows 10 周年更新上还有助于保护使用远程桌面连接的用户凭据。 以前，使用远程桌面服务的任何人都必须登录到其本地计算机上，则需要登录再次在执行到其目标计算机的远程连接。 此第二个登录名会将凭据传递到目标计算机，使它们暴露于传递哈希或传递票证攻击。

使用 Windows Defender 远程 Credential Guard，Windows Server 2016 实现单一登录的远程桌面会话，无需重新输入用户名和密码。 相反，它利用您已使用登录到本地计算机上的凭据。 若要使用 Windows Defender 远程 Credential Guard，远程桌面客户端和服务器必须满足以下要求：

- 必须加入 Active Directory 域并且位于同一个域或域具有信任关系。

- 必须使用 Kerberos 身份验证。

- 必须至少运行 Windows 10 版本 1607年或 Windows Server 2016。  

- 远程桌面经典 Windows 应用是必需的。 远程桌面的通用 Windows 平台应用不支持 Windows Defender 远程 Credential Guard。

可以通过使用远程桌面服务器和组策略或远程桌面客户端上的远程桌面连接参数上的注册表设置来启用 Windows Defender 远程 Credential Guard。 启用 Windows Defender 远程 Credential Guard 的详细信息，请参阅[与 Windows Defender 远程 Credential Guard 保护远程桌面凭据](https://docs.microsoft.com/windows/access-protection/remote-credential-guard)。 为与 Windows Defender Credential Guard，你可以使用 Windows Defender 远程 Credential Guard 以帮助保护 Windows Server 2016 上的特权的标识。

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>保护操作系统以运行应用程序和基础结构
防止网络威胁也需要查找和阻止恶意软件和攻击来获得控制权会破坏您的基础结构的标准操作实践。 如果攻击者可以获取操作系统或应用程序以非预先确定的、 非可行的方式运行，他们可能正在使用该系统来执行恶意操作。 Windows Server 2016 提供了阻止运行恶意软件或利用漏洞的外部攻击者的保护层。 操作系统将采取积极的警报指示系统已破坏的活动的管理员保护基础结构和应用程序。

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 包括 Windows Defender Device Guard，以确保仅受信任的软件，可以在服务器上运行。 使用基于虚拟化的安全性，它可以限制二进制文件可以根据组织的策略在系统上运行。 指定二进制文件之外的任何内容尝试运行，如果 Windows Server 2016 会阻止该请求并记录尝试失败，以便管理员可以看到已发送潜在违规情况。 违反通知是针对 GDPR 符合性要求的关键部分。

Windows Defender Device Guard 还集成使用 PowerShell，以便您可以授权哪些脚本可以在系统上运行。 在早期版本的 Windows Server 中，管理员可以绕过代码完整性强制只需从代码文件中删除策略。 使用 Windows Server 2016 时，可以配置你的组织的签名，以便仅具有签名策略的证书的访问权限的用户可以更改的策略的策略。

#### <a name="control-flow-guard"></a>控制流保护 
Windows Server 2016 还包括针对一些类的内存损坏攻击的内置保护。 修补你的服务器是重要的是，但始终就可能无法为尚未确定的漏洞开发该恶意软件。 一些利用这些漏洞的最常见方法是提供到正在运行的程序或极端寻常的图表数据。 例如，攻击者可以通过提供程序不是预期并超过保留的程序来保存响应的区域的多个输入利用缓冲区溢出漏洞。 这可能会损坏可能保存函数指针的相邻内存。

当程序调用此函数通过时，它可以然后跳转到指定的攻击者的意外位置。 这些攻击也称为是面向跳转的编程 (JOP) 攻击。 控制流防护可防止 JOP 攻击通过将严格限制放置在哪些应用程序代码可以执行 – 尤其是间接调用的说明。 它会添加轻量的安全检查来识别应用程序中是有效目标间接调用的函数集。 运行程序时，它验证这些间接调用目标有效。

如果在运行时，控制流防护检查失败，Windows Server 2016 立即终止程序，尝试间接调用了无效的地址任何攻击的重大。 控制流防护到 Device Guard 提供重要的附加保护层。 如果已泄露的允许列表的应用程序，它将能够运行 Device Guard 通过取消选中因为屏蔽会看到该应用程序已签名，并被视为 Device Guard 信任。

但控制流防护可以识别是否非预先确定的、 非可行的顺序执行应用程序，因为攻击会失败，防止遭到入侵的应用程序无法运行。 在一起，这些保护措施使攻击者将恶意软件注入到 Windows Server 2016 上运行的软件非常困难。

建议在其应用程序中启用控制流防护 (CFG) 的开发人员构建应用程序处理个人数据。 此功能现已推出 Microsoft Visual Studio 2015，并在"CFG 感知"Windows 版本上运行-x86 和 x64 版本中针对桌面和服务器的 Windows 10 和 Windows 8.1 更新 (KB3000850)。 您无需启用 CFG 的代码中，每个部分，因为启用 CFG 混合和非启用 CFG 代码将正常执行。 但是，无法用于所有代码启用 CFG 可以打开保护中的缺口。 此外，CFG"CFG 感知"的 Windows 版本上正常运行启用代码正常运行，因此与它们完全兼容。

#### <a name="windows-defender-antivirus"></a>Windows Defender 防病毒
Windows Server 2016 包括行业领先的活动检测功能的 Windows Defender 阻止已知恶意软件。 Windows Defender 防病毒软件 (AV) 一起使用 Windows Defender Device Guard 和控制流防护，以防止在服务器上安装的任何类型的恶意代码。 默认开启 – 管理员不需要执行任何操作即可开始工作。 Windows Defender AV 还优化为在 Windows Server 2016 中支持各种服务器角色。 在过去，攻击者使用 PowerShell 等的 shell 启动恶意的二进制代码。 在 Windows Server 2016 中，PowerShell 现在与集成，Windows Defender AV 启动代码前扫描恶意软件。

Windows Defender AV 是一种内置的反恶意软件解决方案，为台式机、 便携式计算机和服务器提供安全性和反恶意软件的管理。 由于它在 Windows 8 中引入已显著改进 Windows Defender AV。 Windows Server 中的 Windows Defender 防病毒使用了多种方法来提高反恶意软件：

- **云提供的保护**可在几秒钟内帮助检测和阻止新的恶意软件，即使这种恶意软件之前从未出现过。

- **丰富的本地上下文**改进了恶意软件的标识方式。 Windows Server 通知 Windows Defender AV 不仅有关内容，如文件和进程，但也内容原来所在的位置，其中已存储，和的详细信息。 

- **大量全局传感器**使 Windows Defender AV 当前和甚至是最新的恶意软件的注意。 这可以通过以下两种方式实现：从终结点收集丰富的本地上下文数据，以及集中分析该数据。

- **篡改校对**可帮助保护 Windows Defender AV 本身免受恶意软件攻击。 例如，Windows Defender AV 使用受保护的进程，以防止不受信任的进程将尝试进行篡改 Windows Defender AV 组件，其注册表项，依次类推。

- **企业级功能**为 IT 专业人员提供的工具和使 Windows Defender AV 企业级的反恶意软件解决方案所需的配置选项。

#### <a name="enhanced-security-auditing"></a>增强的安全审核 
Windows Server 2016 主动发出警报管理员可以使用增强的安全审核提供更多详细的信息，可用于更快的攻击检测和取证分析潜在的违反尝试。 它从控制流防护、 Windows Defender Device Guard 和在一个位置，其他安全功能，使管理员能够确定系统可能会面临风险更轻松地记录事件。

新的事件类别包括：

- **审核组的成员身份。** 可以审核用户的登录标记中的组成员身份信息。 组成员身份是枚举或创建登录会话时所在的 PC 进行查询时，会生成事件。 
 
- **审核即插即用的活动。** 可以审核时插检测到外部设备 – 这可能包含恶意软件。 即插即用事件可以用于跟踪系统硬件中的更改。 事件中包含的硬件供应商 Id 列表。

Windows Server 2016 集成，轻松地与安全事件的事件管理 (SIEM) 系统，如 Microsoft Operations Management Suite (OMS)，它可以合并到潜在的违规事件上的智能报表的信息。 所提供的增强的审核信息的深度，安全团队可以标识和更快速地响应潜在的违规事件。

### <a name="secure-virtualization"></a>安全的虚拟化
现在，企业虚拟化它们可以从 SQL Server 到 SharePoint 到 Active Directory 域控制器的所有内容。 虚拟机 (Vm) 只需轻松地部署、 管理服务，并自动执行您的基础结构。 谈到安全性，受攻击的虚拟化结构已变得很难防范 – 一个新的攻击向量，但到目前为止。 从 GDPR 的角度看，您应考虑将保护包括 VM TPM 技术使用的物理服务器保护的 Vm。

Windows Server 2016 从根本上改变了企业如何通过包括允许你创建虚拟机将仅在你自己的结构; 上运行的多个技术来保护虚拟化，帮助防止存储、 网络和其运行的主机设备。

#### <a name="shielded-virtual-machines"></a>受防护的虚拟机
相同的操作，使虚拟机可以十分轻松地迁移，备份和复制，还使其易于修改和复制。 虚拟机是只是一个文件，因此不受保护网络中存储、 备份或其他位置上。 另一个问题是 – 无论它们是存储管理员或网络管理员 – 结构管理员有权访问的所有虚拟机。

在结构上的受攻击的管理员可以轻松地导致受攻击的数据在虚拟机。 攻击者必须执行的就是使用已泄露的凭据复制到 USB 驱动器上他们喜欢的任何 VM 文件，并引导其在组织外部，可以从任何其他系统访问这些 VM 的文件。 如果这些被盗的虚拟任何的机一个 Active Directory 域控制器，例如，攻击者可以轻松地查看内容和使用随时可用的暴力破解技术来破解密码在 Active Directory 数据库中，最终授予他们访问权限所有基础结构内的其他内容。

Windows Server 2016 引入了类似于刚刚介绍的受防护的虚拟机 (受防护的 Vm)，以帮助防止方案。 受防护的 Vm 包括虚拟 TPM 设备，从而使组织可以将 BitLocker 加密应用到虚拟机，并确保它们仅在受信任的主机，以防止泄露的存储、 网络和主机管理员对上运行。 使用第 2 代 Vm，也不能支持统一可扩展固件接口 (UEFI) 固件具有虚拟 TPM 创建受防护的 Vm。

#### <a name="host-guardian-service"></a>主机保护者服务
受防护的 Vm，以及主机保护者服务是用于创建安全虚拟化结构的基本组件。 其工作是之前它将允许启动，或将迁移到该主机的受防护的 VM 的 HYPER-V 主机的运行状况证明。 它为受防护的 Vm 保留密钥并不会释放它们，直到有保证的安全运行状况。 有两种方法需要 HYPER-V 主机，以证明主机保护者服务。

第一个和最安全的则是受信任的硬件证明。 此解决方案要求在具有 TPM 2.0 芯片和 UEFI 2.3.1 的主机上运行受防护的 Vm。 此硬件需要提供标准的引导，以确保 HYPER-V 主机由主机保护者服务所需的操作系统内核完整性信息未被篡改。

IT 组织还可以使用受信任的管理员证明中，这可能不在你的组织中使用 TPM 2.0 硬件是否理想的选择。 此证明模型很容易部署，因为主机只需放置到一个安全组和主机保护者服务配置为允许受防护的 Vm 的安全组的成员主机上运行。 使用此方法，没有任何复杂的度量，以确保主机尚未被篡改。 但是，消除可能的未加密 Vm 的每个步骤的门 USB 驱动器或虚拟机将在未经授权的主机上运行。 这是因为 VM 文件中所指定的组以外的任何计算机上不会运行。 如果你还没有 TPM 2.0 硬件，可以使用受信任的管理员证明启动并升级您的硬件时切换到受信任的硬件证明。

#### <a name="virtual-machine-trusted-platform-module"></a>虚拟机受信任的平台模块
Windows Server 2016 支持 TPM 的虚拟机，以便您可以在虚拟机中支持高级的安全技术，如 BitLocker® 驱动器加密。 可以通过使用 Hyper-v 管理器或启用 VMTPM Windows PowerShell cmdlet 来启用任何生成 2 的 HYPER-V 虚拟机上的 TPM 支持。

可以使用本地加密密钥存储在主机上或存储在主机保护者服务来保护虚拟 TPM (vTPM)。 因此，虽然主机保护者服务需要更多基础结构，它还提供更多保护。

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>使用软件定义网络的分布式的网络防火墙
提高虚拟化环境中的保护一种方法是，Vm 就可以仅与函数所需的特定系统的方式对网络进行分割。 例如，如果你的应用程序不需要与 Internet 连接，可以分区，其关闭，消除这些系统作为从外部攻击者的目标。 软件定义的网络 (SDN) Windows Server 2016 中包括分布式的网络防火墙，您可以动态创建可从来自内部或外部网络的攻击保护您的应用程序的安全策略。 此分布式的网络防火墙将层添加到你的安全，通过使您能够隔离网络中的应用程序。 在虚拟网络基础结构，如有必要 – 隔离 VM 到 VM 的流量、 VM 主机流量或 VM 到 Internet 流量可以任意位置应用策略的可能已泄露的单个系统或以编程方式跨多个子网。 Windows Server 2016 软件定义网络功能还使您能够路由或镜像到非 Microsoft 虚拟设备的传入流量。 例如，你可以选择通过其他垃圾邮件筛选保护 Barracuda 虚拟设备发送所有电子邮件流量。 因此，您可以轻松地层中其他安全在本地或云中。

### <a name="other-gdpr-considerations-for-servers"></a>有关服务器的其他 GDPR 注意事项
GDPR 包含显式要求违规通知，其中的个人数据泄露事件表示，"_出现导致意外的或非法破坏、 丢失、 更改、 未经授权的泄漏或访问、 个人的安全违规数据传输、 存储或处理。_"  显然，您不能开始向前移动以满足严格的 GDPR 通知要求在 72 小时内，如果不能在第一时间检测安全违规。

Windows 安全中心白皮书中所述[Post 违规：面对的高级威胁](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> "_违规后假定已发生违规 – 充当网络流量记录器和犯罪场景调查者 (CSI) 与预违规行为不同。违规后提供安全团队的信息和工具集需要以标识，请调查并做出受到攻击，否则将保持未检测到和雷达图下方。_"

在本部分中，我们将介绍 Windows Server 可以如何帮助你满足 GDPR 违反通知义务。 这首先需要了解基础的威胁数据供 Microsoft 收集和分析你的权益的方式，通过 Windows Defender 高级威胁防护 (ATP)，该数据可以是很重要，则。

#### <a name="insightful-security-diagnostic-data"></a>见解深入的安全诊断数据
近二十，Microsoft 已被转化为威胁有用智能，可帮助保护其平台并保护客户。 如今，随着云计算带来的巨大优势，我们正在寻找新的方法来利用威胁情报驱动的丰富分析引擎保护我们的客户。

通过应用自动化和手动流程、机器学习和人类专家的组合，我们可以创建一个能够自学并实时进行演变的 Intelligent Security Graph，从而减少用来检测和响应我们产品中的新事件的集体协作时间。

![Microsoft 智能安全图](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

按原义，Microsoft 的威胁智能范围跨越数十亿个数据点：35 亿条消息每月扫描跨企业和使用者访问 200 多个云服务和每日执行的 14 亿次身份验证的段的 1 亿个客户。 所有这些数据是由 Microsoft 创建，帮助你保持安全、 保持工作效率并满足 GDPR 要求以动态方式保护你第一道防线 Intelligent Security Graph 一起抽取代表你。

#### <a name="detecting-attacks-and-forensic-investigation"></a>检测攻击和取证调查
即使是最好的端点防御最终也可能会被攻破，因为网络攻击变得越来越复杂和有针对性。 两个功能可用于帮助进行潜在漏洞检测-Windows Defender 高级威胁防护 (ATP) 和 Microsoft 高级威胁分析 (ATA)。

Windows Defender 高级威胁防护 (ATP) 可帮助你检测、调查和应对网络上的高级攻击和数据泄露。 类型的数据泄露 GDPR 要求可防止通过技术安全措施，以确保持续的保密性、 完整性和个人数据和处理系统的可用性。

在 Windows Defender ATP 的主要优点如下所示：

- **检测到无法检测到。** 深入了解操作系统内核、 Windows 安全专家和唯一制作超过 1 亿个计算机和信号生成跨所有 Microsoft 服务的传感器。

- **内置的而非外加。** 无代理，使用高性能和影响最小，由云支持;不具有部署的易于管理。 

- **单一管理平台的 Windows 安全。** 探索 6 个月的功能丰富的计算机的时间线，从 Windows Defender ATP、 Windows Defender 防病毒软件和 Windows Defender Device Guard 统一的安全事件。

- **Microsoft graph 的强大功能。** 利用 Microsoft Intelligence Security Graph 与 Office 365 ATP 订阅来返回跟踪和响应攻击将检测和浏览进行集成。

在 [Windows Defender ATP 创意者更新预览版的新增功能](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/)中阅读详细信息。

ATA 是一种本地产品，可帮助在组织中检测标识入侵。 ATA 可以捕获和分析网络流量进行身份验证、 授权和信息收集协议 （例如 Kerberos、 DNS、 RPC、 NTLM 和其他协议）。 ATA 使用此数据在网络上生成有关用户和其他实体的行为配置文件，以便它可以检测异常和已知的攻击模式。 下表列出了 ATA 检测到的攻击类型。


|攻击类型 |描述 |
|---------|---------|
|恶意攻击 |这些攻击会检测到从已知的攻击类型，包括列表查找攻击：<ul><li>传递票证 (PtT)</li><li>传递的哈希 (PtH)</li><li>超传递哈希</li><li>伪造的 PAC (MS14-068)</li><li>黄金票证</li><li>恶意复制</li><li>侦察</li><li>暴力破解</li><li>远程执行</li></ul>可以检测到的恶意攻击的完整列表和及其说明，请参阅[什么可以 ATA 检测可疑活动？](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats)。|
|异常行为 |这些攻击检测到的使用行为分析和使用机器学习来识别可疑活动，包括：<ul><li>异常登录</li><li>未知的威胁</li><li>密码共享</li><li>横向移动</li></ul>|
|安全问题和风险 |通过查看当前网络和系统配置，检测到这些攻击包括：<ul><li>信任破坏</li><li>弱协议</li><li>已知的协议漏洞</li></ul>|

ATA 可用于帮助检测攻击者尝试破坏特权的标识。 部署 ATA 的详细信息，请参阅中的计划、 设计和部署主题[高级威胁分析文档](https://docs.microsoft.com/advanced-threat-analytics/)。

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>关联的 Windows Server 2016 解决方案的相关的内容

- **Windows Defender 防病毒：** https://www.youtube.com/watch?v=P1aNEy09NaI和 https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender 高级威胁防护：** https://www.youtube.com/watch?v=qxeGa3pxIwg和 https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI和 https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard:** https://www.youtube.com/watch?v=F-pTkesjkhI和 https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **控制流防护：** https://msdn.microsoft.com/en-us/library/windows/desktop/mt637065(v=vs.85).aspx

- **安全和保障：** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>免责声明
本文是截至发布日期 Microsoft 对 GDPR 的评论。 我们花了很多时间来研究 GDPR，并且我们一直在思考它的意图和意义。 但是 GDPR 的应用高度具体，我们对 GDPR 的解释并非面面俱到。

因此，本文仅供参考，不应作为法律意见或确定 GDPR 在贵组织内的适用程度的依据。 我们鼓励你与具有法律资质的专业人士合作，共同探讨 GDPR、它在贵组织内的适用程序以及确保合规性的最佳做法。

MICROSOFT 对本文中的信息不做任何保证（明示的、暗示的或法定的）。 本文按“原样”提供。 本文中表达的信息和视图（包括 URL 和其他 Internet 网站参考）如有更改，恕不另行通知。

本文未向你提供针对任何 Microsoft 产品的任何知识产权的任何法律权限。  你可以复制本文并仅将其用于内部参考用途。  

发布日期 2017 年 9 月<br>
版本 1.0<br>
© 2017 Microsoft。 保留所有权利。


