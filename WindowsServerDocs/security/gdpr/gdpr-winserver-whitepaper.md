---
title: 开启遵守适用于 Windows Server 2016 的一般数据保护条例 (GDPR) 之旅
description: 通过本文了解 GDPR 是什么以及 Microsoft 提供的产品如何帮助你向实现合规性的目标迈进。
ms.technology: techgroup-security
ms.topic: article
ms.date: 09/25/2017
ms.author: nirb
author: nirb-ms
ms.openlocfilehash: 506cd5cb44d93c9d7d221917505f76a2c5625baa
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870550"
---
# <a name="beginning-your-general-data-protection-regulation-gdpr-journey-for-windows-server"></a>开始适用于 Windows Server 的一般数据保护条例（GDPR）旅程 

>适用于：Windows Server（半年频道）、Windows Server 2016

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

- **增强的个人隐私权利。** 通过确保欧盟居民有权访问其个人数据、纠正该数据的不准确之处、擦除该数据、反对处理其个人数据以及移动数据，加强对欧盟居民的数据保护。

- **增强了保护个人数据的责任。** 加强负责处理个人数据的组织的责任制，提高责任透明度，并确保遵守相关法规。

- **必需的个人数据违规报告。** 掌握个人数据的组织必须向监管机构报告对个人权利和自由造成风险的个人数据违规行为，而不得拖延，并且必须在知道违规行为的 72 小时内进行报告（如果可行）。

正如你所料，GDPR 可能会对你的业务产生重大影响，可能要求你更新隐私政策，实施和强化数据保护控制和违规通知程序，部署高度透明的政策，并加大在 IT 和培训方面的投资。 Microsoft Windows 10 可以帮助你有效且高效地满足某些需求。

## <a name="personal-and-sensitive-data"></a>个人数据和敏感数据
作为为遵守 GDPR 所做努力的一部分，你需要了解该法规如何定义个人数据和敏感数据以及这些定义与贵组织保留的数据有何关联。 基于这一了解，你将能够发现创建、处理、管理和存储数据的位置。

GDPR 将个人数据定义为已确定身份或可确定身份的自然人的任何相关信息。 这可以包括直接身份（例如你的法定名称）和间接身份（例如，明确指出你是数据所指对象的特定信息）。 GDPR 还明确指出个人数据的概念包括在线标识符（如 IP 地址、移动设备 ID）和位置数据。

GDPR 介绍遗传数据（如个人的基因序列）和生物识别数据的特定定义。 遗传数据和生物数据以及其他子类别的个人数据（个人数据泄露种族歧视或种族、政治观点、宗教或哲学信仰，或商业联合成员身份：有关运行状况的数据; 或有关人员的性爱生活或色情方向）在 GDPR 下被视为敏感的个人数据。 敏感的个人数据是增强的保护功能，通常需要个人同意才能处理这些数据。

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

-   **查询.** 识别你拥有的个人数据及其所在的位置。 

-   **管理.** 管理使用和访问个人数据的方式。

-   **确保.** 制定安全控制措施来防止、检测和响应安全漏洞和数据泄露。  

-   **报表.** 处理数据请求、报告数据泄漏和保留所需的文档。

    ![关于 4 个主要 GDPR 步骤如何协同工作的图示](../media/GDPR-Windows-Server-Overview/gdpr-steps-diagram.png)

对于每个步骤，我们已经概述了各种 Microsoft 解决方案中的示例工具、资源以及功能，它们可用来帮助你满足该步骤的要求。 虽然本文不是一种全面的 "操作方法" 指南，但我们提供了一些链接，可用于查找更多详细信息，并在[Microsoft 信任中心的 GDPR 部分](https://www.microsoft.com/en-us/trustcenter/privacy/gdpr)获得详细信息。

## <a name="windows-server-security-and-privacy"></a>Windows Server 安全和隐私
GDPR 要求你实施适当的技术和组织安全措施来保护个人数据和处理系统。 在 GDPR 的上下文中，物理和虚拟服务器环境可能正在处理个人数据和敏感数据。 处理可以表示任何操作或操作集，如数据收集、存储和检索。

您能否满足这一要求并实施适当的技术安全措施，必须反映在当今日益恶意的 IT 环境中面临的威胁。 当今安全威胁领域是一种 tenacious 的威胁。 过去几年，恶意攻击者主要专注于通过其攻击获得社群认同，或因暂时使系统离线而获得的兴奋感。 从那时起，攻击者的动机已转移到了金钱上，包括将设备和数据 hostage，直到所有者支付了要求的 ransom。

现代攻击日益集中于大规模的知识产权盗窃；可能导致财务损失的目标系统性能降低；甚至会威胁全世界的个人、企业和国家/地区利益的网络恐怖主义。 此类攻击者通常是训练有素的个人和安全专家，其中一些人受雇于国家/地区，这些国家/地区拥有大额预算和似乎无限的人力资源。 这样的威胁需要可以解决这一挑战的方法。

这些威胁不仅威胁到你全面掌控所拥有的任何个人数据或敏感数据的能力，而且对你的整体业务也构成了重大风险。 考虑来自 McKinsey、Ponemon 研究所、Verizon 和 Microsoft 的最新数据：

- GDPR 预计报告的数据泄露的平均成本是 350 万美元。

- 其中 63％ 的泄露涉及 GDPR 希望你解决的弱密码或密码被盗问题。

- 每天创建和传播超过 300,000 个新的恶意软件样本，这让你解决数据保护问题的任务变得更具挑战性。

正如最近的勒索软件攻击所见，一旦被称为 Internet 的黑色灾难，攻击者就会遭受更大的目标，这些目标可以承受花钱，并可能产生灾难性后果。 GDPR 包括一些惩罚，使你的系统（包括台式机和便携式计算机）确实包含了个人和敏感的数据丰富目标。

以下两个关键原则指导并继续指导 Windows 的开发：

- **安全性。** 我们代表我们的客户的软件和服务存储的数据应受到保护，不会受到损害，只能以适当的方式使用或修改。 安全模式应该便于开发人员了解并构建在其应用程序中。

- **保密.** 用户应该控制其数据的使用方式。 用户应清楚地说明信息的使用策略。 如果用户接收信息以充分利用其时间，则应该控制用户的情况。 用户可轻松指定其信息的适当使用，包括控制其发送的电子邮件的使用。

Microsoft 在 Microsoft 的 CEO，Satya Nadella 最近指出了这些原则，变 

> "_随着世界不断变化和业务需求的发展，某些因素是一致的：客户对安全和隐私的需求。_ "

当你努力遵从 GDPR 时，了解你的物理服务器和虚拟服务器的角色，以便在创建、访问、处理、存储和管理可能在 GDPR 下作为个人和潜在敏感数据的数据的角色非常重要。 Windows Server 提供了一些功能，这些功能将帮助你满足 GDPR 要求，以便实施适当的技术和组织安全措施来保护个人数据。

Windows Server 2016 的安全状况不是一个插件;这是一种体系结构原则。 而且，可以在四个主体中充分了解这一点：

- **确保.** 预防措施的持续关注和创新;阻止已知的攻击和已知的恶意软件。

- **察觉.** 全面的监视工具，可帮助你更快地发现异常并响应攻击。

- **答复.** 领先的响应和恢复技术，以及深入的咨询专业知识。

- **解决.** 隔离操作系统组件和数据机密，限制管理员权限，并严格测量主机的运行状况。

借助 Windows Server，你能够保护、检测和防御可能导致数据泄露的攻击类型，从而大大提高了性能。 鉴于 GDPR 对违规通知有着严格要求，确保台式机和便携式计算机系统得到良好保护将降低你所面临的可能导致成本高昂的违规分析和通知的风险。

在下面的部分中，你将了解 Windows Server 如何提供适用于 GDPR 合规性旅程的 "保护" 阶段中的水平的功能。 这些功能分为三个保护方案：

- **保护您的凭据并限制管理员权限。** Windows Server 2016 可帮助实现这些更改，以帮助防止系统被用作进一步入侵的启动点。

- **保护操作系统以运行你的应用程序和基础结构。** Windows Server 2016 提供保护层，有助于阻止外部攻击者运行恶意软件或利用漏洞。

- **安全虚拟化。** Windows Server 2016 使用受防护的虚拟机和受保护的构造实现安全虚拟化。 这可以帮助你在构造中的受信任主机上加密和运行虚拟机，从而更好地保护它们免受恶意攻击。

下面更详细地讨论了这些功能，其中详细介绍了对特定 GDPR 的要求，这些功能是基于高级设备保护构建的，有助于维护操作系统和数据的完整性和安全性。

GDPR 中的密钥预配是按设计和默认方式进行数据保护的，并且帮助你满足此项设置的能力是 Windows 10 中的功能，例如 BitLocker 设备加密。 BitLocker 使用受信任的平台模块（TPM）技术，该技术提供基于硬件的安全性相关的功能。 此加密处理器芯片包括多个物理安全机制，使其无法篡改，恶意软件无法篡改 TPM 的安全功能。

该芯片包含多个物理安全机制以使其防篡改，并且恶意软件无法篡改 TPM 的安全功能。 使用 TPM 技术的一些主要优点是你可以实现以下目的：

-   生成、存储和限制使用加密密钥。

-   使用 tpm 技术进行平台设备身份验证，方法是使用 TPM 的唯一 RSA 密钥，该密钥刻录到其自身中。

-   通过采用并存储安全措施可帮助确保平台的完整性。

与你的无数据泄露操作相关的其他高级设备保护包括 Windows 受信任启动，它通过确保在系统防御之前恶意软件无法启动来帮助维护系统的完整性。

## <a name="windows-server-supporting-your-gdpr-compliance-journey"></a>Windows Server:支持 GDPR 合规性旅程
Windows Server 中的主要功能可帮助你有效有效地实施 GDPR 要求的安全和隐私机制。 虽然使用这些功能不能保证你的符合性，但它们将支持你的工作。

服务器操作系统在组织的基础结构中处于战略层，使了新的机会，旨在防范可能盗取数据和中断业务的攻击。 GDPR 的关键方面（如设计、数据保护和访问控制）需要在服务器级别的 IT 基础结构中进行寻址。

Windows Server 2016 帮助保护标识、操作系统和虚拟化层的安全，帮助阻止用于获取对系统违法访问权限的常见攻击媒介：盗窃的凭据、恶意软件和安全的虚拟化结构。 除了降低业务风险以外，Windows Server 2016 中内置的安全组件还有助于满足关键政府和行业安全法规的合规性要求。 

使用这些标识、操作系统和虚拟化保护，你可以更好地保护在任何云中作为 VM 运行 Windows Server 的数据中心，并限制攻击者泄露凭据的能力，启动恶意软件，并在你的网桥. 同样，在部署为 Hyper-v 主机时，Windows Server 2016 通过受防护的虚拟机和分布式防火墙功能为你的虚拟化环境提供安全保障。 对于 Windows Server 2016，服务器操作系统将成为你的数据中心安全的活动参与者。

### <a name="protect-your-credentials-and-limit-administrator-privileges"></a>保护凭据并限制管理员权限 
控制对个人数据和处理该数据的系统的访问，是包含 GDPR 的区域，该区域具有特定的要求，包括管理员的访问权限。 特权标识是具有提升权限的任何帐户，如域管理员、企业管理员、本地管理员甚至超级用户组的成员的用户帐户。 此类标识还可以包括已直接获得权限的帐户，如执行备份、关闭系统或本地安全策略控制台的 "用户权限分配" 节点中列出的其他权限。

作为一般的访问控制原则和 GDPR，你需要保护这些特权身份，使其免受潜在攻击者的损害。 首先，请务必了解标识的安全程度;然后，你可以计划防止攻击者获取对这些特权标识的访问权限。

#### <a name="how-do-privileged-identities-get-compromised"></a>特权标识如何受到侵害？
当组织没有保护它们的指导原则时，特权身份可能会受到威胁。 下面是一些示例：

- **比所需的权限更多。** 最常见的问题之一是，用户具有的权限超过了执行其作业功能所需的权限。 例如，管理 DNS 的用户可能是 AD 管理员。 大多数情况下，这是为了避免需要配置不同的管理级别。 但是，如果此类帐户已泄露，则攻击者会自动获得提升的权限。

- **已通过提升的权限不断登录。** 另一个常见问题是具有提升权限的用户可以不受限制地使用它。 这对于使用特权帐户登录到台式计算机、保持登录，并使用特权帐户浏览 web 和使用电子邮件（典型的工作作业功能）的 IT 专业人员非常常见。 特权帐户的持续时间不受限制会使该帐户更容易受到攻击，并会导致帐户泄露的可能性。

- **社会工程研究。** 大多数凭据威胁首先通过研究组织，然后通过社交工程进行。 例如，攻击者可能会执行电子邮件仿冒攻击，以泄露有权访问组织网络的合法帐户（但不一定是提升的帐户）。 然后，攻击者使用这些有效帐户在您的网络上执行其他调查，并确定可以执行管理任务的特权帐户。 

- **利用具有提升权限的帐户。** 即使使用网络中的普通非提升用户帐户，攻击者也可以使用提升的权限访问帐户。 执行此操作的一种更常见的方法是使用传递哈希或传递令牌攻击。 有关哈希传递和其他凭据盗窃技术的详细信息，请参阅[传递哈希（PtH）页](https://technet.microsoft.com/dn785092.aspx)上的资源。

当然，攻击者可以使用其他方法来确定和破坏特权标识（每日创建新的方法）。 因此，请务必确定用户使用最少特权帐户登录，以减少攻击者获取特权标识的能力。 以下部分概述了 Windows Server 可在其中缓解这些风险的功能。

#### <a name="just-in-time-admin-jit-and-just-enough-admin-jea"></a>实时管理员（JIT）和足够的管理员（JEA）
尽管防范传递哈希攻击或传递票证攻击非常重要，但其他方法仍然可以盗取管理员凭据，包括社会工程、不满的员工和暴力破解。 因此，除了尽可能隔离凭据以外，还需要一种方法来限制管理员级别权限的覆盖范围，以防它们受到威胁。

如今，过多的管理员帐户太多，即使只有一个责任区域。 例如，DNS 管理员（需要一组非常窄的权限来管理 DNS 服务器）通常被授予域管理员级别的权限。 此外，因为这些凭据是为 perpetuity 授予的，所以对它们的使用时长没有限制。

每个具有不必要的域管理员级别权限的帐户都会使攻击者试图泄露凭据。 若要最大程度地减少攻击面，只需提供管理员执行该作业所需的特定权限集，而仅提供完成此操作所需的时间窗口。

使用足够的管理和实时管理，管理员可以请求所需的确切时间窗口中所需的特定权限。 例如，对于 DNS 管理员，使用 PowerShell 启用足够的管理功能，可以创建一组可用于 DNS 管理的有限命令。

如果 DNS 管理员需要对其中一台服务器进行更新，她会请求使用 Microsoft Identity Manager 2016 管理 DNS 的权限。 请求工作流可包含一个审批过程，如双重身份验证，这种身份验证可以调用管理员的移动电话来确认其身份，然后再授予请求的权限。 一旦获得授权，这些 DNS 权限就会为特定时间范围内的 DNS 提供对 PowerShell 角色的访问权限。

如果 DNS 管理员凭据被盗，请设想这种情况。 首先，由于凭据没有附加的管理员特权，攻击者将无法访问 DNS 服务器或任何其他系统，从而进行任何更改。 如果攻击者尝试请求 DNS 服务器的权限，则第二重身份验证会要求用户确认其身份。 由于攻击者不可能具有 DNS 管理员的移动电话，因此身份验证会失败。 这会将攻击者锁定在系统之外，并提醒 IT 组织凭据可能会泄露。

此外，许多组织使用免费的[本地管理员密码解决方案（LAPS）](http://aka.ms/laps)作为其服务器和客户端系统的简单但功能强大的 JIT 管理机制。 LAPS 功能提供了对加入域的计算机的本地帐户密码的管理。 密码存储在 Active Directory （AD）中并受和访问控制列表（ACL）保护，因此只有符合条件的用户才能读取它或请求其重置。

如[Windows 凭据盗窃缓解指南](https://www.microsoft.com/en-us/download/confirmation.aspx?id=54095)中所述， 

> "_罪犯使用各种工具和技术来执行凭据被盗和重复使用，恶意攻击者发现实现其目标更容易。凭据盗窃通常依赖于操作实践或用户凭据的公开，因此有效的缓解措施要求使用一种全面的方法来解决人员、流程和技术。此外，这些攻击依赖于攻击者在损害系统后盗取凭据以扩展或保留访问权限，因此，组织必须通过实施策略来使攻击者在网络受到威胁。_ "

Windows Server 的一项重要设计注意事项是减轻凭据被盗的情况，特别是派生凭据。 Credential Guard 通过在旨在帮助消除基于硬件的隔离攻击的 Windows 中实施重大体系结构更改，大大提高了对派生的凭据被盗和重复使用的安全性，而不是简单地尝试防御它们。

虽然使用 Windows Defender Credential Guard、NTLM 和 Kerberos 派生凭据通过基于虚拟化的安全进行保护，但会阻止在许多目标攻击中使用的凭据盗窃攻击方法和工具。 在带有管理权限的操作系统中运行的恶意软件无法提取受基于虚拟化的安全性保护的密钥。 Windows Defender Credential Guard 是一项强大的缓解措施，持续威胁攻击可能会转移到新的攻击方法，还应结合使用设备防护，如下面所述，以及其他安全策略和体系结构。

#### <a name="windows-defender-credential-guard"></a>Windows Defender Credential Guard
Windows Defender Credential Guard 使用基于虚拟化的安全性来隔离凭据信息，防止无法截获密码哈希或 Kerberos 票证。 它使用全新的隔离本地安全机构（LSA）进程，这在操作系统的其他部分不可访问。 独立 LSA 使用的所有二进制文件都使用在受保护的环境中启动之前验证的证书进行签名，从而使传递哈希类型攻击完全无效。

Windows Defender Credential Guard 使用：

- 基于虚拟化的安全性（必需）。 还需要：

    - 64 位 CPU

    - CPU 虚拟化扩展和扩展页表

    - Windows 虚拟机监控程序

- 安全启动（必需）

- TPM 2.0，离散或固件（首选 - 提供硬件绑定）

可以通过保护 Windows Server 2016 上的凭据和凭据派生，使用 Windows Defender 凭据防护来帮助保护特权标识。 有关 Windows Defender Credential Guard 要求的详细信息，请参阅[通过 Windows Defender Credential Guard 保护派生的域凭据](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。

#### <a name="windows-defender-remote-credential-guard"></a>Windows Defender 远程 Credential Guard
Windows Server 2016 和 Windows 10 周年更新上的 windows Defender 远程 Credential Guard 还有助于保护具有远程桌面连接的用户的凭据。 以前，使用远程桌面服务的任何人都必须登录到其本地计算机，然后在执行与目标计算机的远程连接时需要再次登录。 此第二个登录会将凭据传递到目标计算机，并将其公开给传递哈希或传递票证攻击。

利用 Windows Defender 远程 Credential Guard，Windows Server 2016 实现了远程桌面会话的单一登录，因而不需要重新输入用户名和密码。 而是利用已用于登录到本地计算机的凭据。 若要使用 Windows Defender 远程 Credential Guard，远程桌面客户端和服务器必须满足以下要求：

- 必须加入到 Active Directory 域中，并且必须位于同一个域或具有信任关系的域中。

- 必须使用 Kerberos 身份验证。

- 至少必须运行 Windows 10 版本1607或 Windows Server 2016。  

- 远程桌面经典 Windows 应用程序是必需的。 远程桌面通用 Windows 平台应用不支持 Windows Defender 远程 Credential Guard。

您可以通过使用远程桌面服务器上的注册表设置并组策略或远程桌面客户端上的远程桌面连接参数来启用 Windows Defender 远程 Credential Guard。 有关启用 Windows Defender 远程 Credential Guard 的详细信息，请参阅[通过 Windows Defender 远程 Credential Guard 保护远程桌面凭据](https://docs.microsoft.com/windows/access-protection/remote-credential-guard)。 与 Windows Defender Credential Guard 一样，你可以使用 Windows Defender 远程凭据防护来帮助保护 Windows Server 2016 上的特权标识。

### <a name="secure-the-operating-system-to-run-your-apps-and-infrastructure"></a>保护操作系统以运行你的应用程序和基础结构
防止网络威胁还需要查找和阻止恶意软件和攻击，这些攻击会通过破坏基础结构的标准操作实践来获得控制。 如果攻击者可以使操作系统或应用程序以非预先确定的方式运行，则可能会使用该系统来执行恶意操作。 Windows Server 2016 提供阻止外部攻击者运行恶意软件或利用漏洞的保护层。 在保护基础结构和应用程序的过程中，操作系统通过向管理员通知指示系统已被破坏的活动的活动角色。

#### <a name="windows-defender-device-guard"></a>Windows Defender Device Guard
Windows Server 2016 包含 Windows Defender 设备防护，以确保只有受信任的软件可以在服务器上运行。 使用基于虚拟化的安全性，可以根据组织的策略限制可在系统上运行的二进制文件。 如果除指定的二进制文件以外的任何内容尝试运行，Windows Server 2016 会阻止它并记录失败的尝试，以便管理员可以看到发生了潜在的破坏。 入侵通知是 GDPR 合规性要求的重要组成部分。

Windows Defender Device Guard 还与 PowerShell 集成，因此你可以授权可以在你的系统上运行的脚本。 在早期版本的 Windows Server 中，管理员只需删除代码文件中的策略，就可以绕过代码完整性强制执行。 使用 Windows Server 2016，你可以配置由组织签名的策略，以便只有有权访问对策略进行签名的证书的人员才能更改策略。

#### <a name="control-flow-guard"></a>控制流保护 
Windows Server 2016 还包括针对某些内存损坏攻击类的内置保护。 修补服务器非常重要，但始终有可能为尚未识别的漏洞开发恶意软件。 利用这些漏洞的一些最常见方法是向正在运行的程序提供异常或极端数据。 例如，攻击者可以通过提供对程序的更多输入来利用缓冲区溢出漏洞，并使程序保留响应的区域溢出。 这可能会损坏可能包含函数指针的相邻内存。

当程序通过此函数调用时，它可以跳转到攻击者指定的意外位置。 这些攻击也称为定向编程（JOP）攻击。 控制流防护通过对可执行的应用程序代码施加严格限制（特别是间接调用说明）来防止 JOP 攻击。 它添加了轻型安全检查来识别应用程序中作为间接调用的有效目标的函数集。 当应用程序运行时，它会验证这些间接调用目标是否有效。

如果控制流防护检查在运行时失败，Windows Server 2016 会立即终止程序，从而中断试图间接调用无效地址的任何攻击。 控制流防护为设备保护提供了一个重要的附加保护层。 如果已列出了白名单的应用程序，则设备保护可以运行该应用程序，因为 Device Guard 筛选会发现该应用程序已签名并被视为受信任。

但由于控制流防护可以确定应用程序是否是以非预先确定的、不可行的顺序执行的，因此攻击会失败，从而阻止已损坏的应用程序运行。 同时，这些保护使攻击者难以将恶意软件注入到 Windows Server 2016 上运行的软件中。

鼓励开发人员构建将在其中处理个人数据的应用程序，以便在应用程序中启用控制流防护（CFG）。 此功能在 Microsoft Visual Studio 2015 中提供，并在 Windows 的 "CFG 感知" 版本（Windows 10 的桌面和服务器的 x86 和 x64 版本以及 Windows 8.1 更新（KB3000850））上运行。 无需为代码的每个部分启用 CFG，因为启用了 CFG 并且启用了非 CFG 的代码即可。 但未能为所有代码启用 CFG 会打开保护中的空白。 而且，启用了 CFG 的代码在 "CFG 感知" 的 Windows 版本上运行正常，因此与它们完全兼容。

#### <a name="windows-defender-antivirus"></a>Windows Defender 防病毒
Windows Server 2016 包含 Windows Defender 的业界领先的活动检测功能，用于阻止已知的恶意软件。 Windows Defender 防病毒（AV）与 Windows Defender Device Guard 和控制流防护一起使用，以防止在你的服务器上安装任何种类的恶意代码。 默认情况下，此功能处于打开状态-管理员无需采取任何措施即可开始工作。 Windows Defender AV 还进行了优化，以支持 Windows Server 2016 中的各种服务器角色。 过去，攻击者使用了 shell （如 PowerShell）来启动恶意的二进制代码。 在 Windows Server 2016 中，PowerShell 现在与 Windows Defender AV 集成，以便在启动代码之前扫描恶意软件。

Windows Defender AV 是一种内置的反恶意软件解决方案，它为台式计算机、便携式计算机和服务器提供安全性和反恶意软件管理。 Windows Defender AV 已明显提高，因为它是在 Windows 8 中引入的。 Windows Server 中的 windows Defender 防病毒使用多种方法来改进反恶意软件：

- **云提供的保护**可在几秒钟内帮助检测和阻止新的恶意软件，即使这种恶意软件之前从未出现过。

- **丰富的本地上下文**改进了恶意软件的标识方式。 Windows Server 不仅通知 Windows Defender AV 内容（如文件和进程），而且还通知内容来自何处、存储位置以及更多内容。 

- **广泛的全局传感器**有助于保持 WINDOWS Defender AV 最新，甚至能了解最新的恶意软件。 这可以通过以下两种方式实现：从终结点收集丰富的本地上下文数据，以及集中分析该数据。

- **篡改校对**有助于保护 WINDOWS Defender AV 本身免遭恶意软件攻击。 例如，Windows Defender AV 使用受保护的进程，这会阻止不受信任的进程尝试使用 Windows Defender AV 组件、其注册表项等进行篡改。

- **企业级功能**为 IT 专业人员提供了使 WINDOWS Defender AV 成为企业级反恶意软件解决方案所需的工具和配置选项。

#### <a name="enhanced-security-auditing"></a>增强的安全性审核 
Windows Server 2016 主动提醒管理员使用增强的安全审核功能，从而提供更详细的信息，可用于更详细的攻击检测和取证分析。 它在一个位置记录控制流防护、Windows Defender 设备防护和其他安全功能的事件，使管理员能够更轻松地确定哪些系统可能有风险。

新的事件类别包括：

- **审核组成员身份。** 允许你在用户的登录令牌中审核组成员身份信息。 在创建登录会话的 PC 上枚举或查询组成员身份时，将生成事件。 
 
- **审核 PnP 活动。** 允许你在即插即用检测到外部设备（可能包含恶意软件）时进行审核。 PnP 事件可用于跟踪系统硬件中的更改。 事件中包含硬件供应商 Id 列表。

Windows Server 2016 可以轻松地与安全事件事件管理（SIEM）系统（如 Microsoft Operations Management Suite （OMS））集成，这种系统可以将信息合并到智能报表中以发现潜在的安全漏洞。 增强审核提供的信息深度使安全团队能够更快、更高效地识别和响应潜在的破坏。

### <a name="secure-virtualization"></a>安全虚拟化
如今，企业可以通过从 SQL Server 到 SharePoint 到 Active Directory 域控制器来虚拟化所有内容。 虚拟机（Vm）只是让你更轻松地部署、管理、服务和自动化你的基础结构。 但当涉及到安全性时，受到威胁的虚拟化结构就成为了一个难以防御的新攻击向量–到目前为止。 从 GDPR 的角度来看，应考虑保护 Vm，就像保护物理服务器（包括使用 VM TPM 技术）一样。

Windows Server 2016 从根本上改变了企业可如何保护虚拟化的方式，包括多种技术，使你能够创建仅在自己的构造上运行的虚拟机;帮助保护它们在其上运行的存储、网络和主机设备。

#### <a name="shielded-virtual-machines"></a>受防护的虚拟机
使虚拟机更易于迁移、备份和复制的操作也是如此，使其更易于修改和复制。 虚拟机只是一个文件，因此它在网络、存储、备份或其他地方不受保护。 另一个问题是构造管理员–是指存储管理员还是网络管理员–可以访问所有虚拟机。

结构上的受攻击管理员可轻松地在虚拟机上产生泄露的数据。 攻击者必须做的就是使用泄露的凭据将他们喜欢的任何 VM 文件复制到 USB 驱动器，并将其从组织中退出，其中可以从任何其他系统访问这些 VM 文件。 例如，如果这些被盗 Vm 中的任何一个是 Active Directory 域控制器，攻击者可能会轻松查看内容，并使用随时可用的暴力破解方法破解 Active Directory 数据库中的密码，最终给予他们访问权限到基础结构中的其他所有内容。

Windows Server 2016 引入了防护的虚拟机（受防护的 Vm），以帮助防止出现这种情况。 受防护的 Vm 包括虚拟 TPM 设备，该设备使组织能够将 BitLocker 加密应用到虚拟机，并确保它们只能在受信任的主机上运行，以帮助防止损坏的存储、网络和主机管理员。 受防护的 Vm 是使用第2代 Vm 创建的，后者支持统一可扩展固件接口（UEFI）固件和虚拟 TPM。

#### <a name="host-guardian-service"></a>主机保护者服务
与受防护的 Vm 一起，主机保护者服务是创建安全虚拟化结构的基本组件。 它的任务是证明 Hyper-v 主机的运行状况，使受防护的 VM 可以启动或迁移到该主机。 它保留受防护的 Vm 的密钥，而不会释放它们，直到安全运行状况得以保证。 可以通过两种方式来要求 Hyper-v 主机证明主机保护者服务。

第一种也是最安全的证明，是硬件信任的证明。 此解决方案要求在具有 TPM 2.0 芯片和 UEFI 2.3.1 的主机上运行受防护的 Vm。 需要此硬件来提供主机保护者服务所需的经过测量的引导和操作系统内核完整性信息，以确保 Hyper-v 主机未被篡改。

IT 组织可以使用管理员信任的证明，如果你的组织中未使用 TPM 2.0 硬件，则这可能是必需的。 此证明模型易于部署，因为主机仅放入安全组，主机保护者服务配置为允许受防护的 Vm 在作为安全组成员的主机上运行。 利用此方法，没有复杂的度量值来确保主机未被篡改。 但是，你可以避免未加密的 Vm 遍历 USB 驱动器上的门，或者 VM 将在未经授权的主机上运行。 这是因为 VM 文件不会在指定组中的任何其他计算机上运行。 如果你还没有 TPM 2.0 硬件，则可以从管理受信任的证明开始，并在你的硬件升级时切换到硬件可信的证明。

#### <a name="virtual-machine-trusted-platform-module"></a>虚拟机受信任的平台模块
Windows Server 2016 支持适用于虚拟机的 TPM，使你能够在虚拟机中支持高级安全技术，如 BitLocker®驱动器加密。 你可以使用 Hyper-v 管理器或 VMTPM Windows PowerShell cmdlet 在任何第2代 Hyper-v 虚拟机上启用 TPM 支持。

你可以使用存储在主机上或存储在主机保护者服务中的本地加密密钥来保护虚拟 TPM （vTPM）。 因此，虽然主机保护者服务需要更多的基础结构，但它还提供更多的保护。

#### <a name="distributed-network-firewall-using-software-defined-networking"></a>使用软件定义的网络的分布式网络防火墙
改善虚拟化环境中的保护的一种方法是将网络分段，使 Vm 只能与运行所需的特定系统进行通信。 例如，如果你的应用程序不需要连接到 Internet，则可以将其分区，从而将这些系统作为外部攻击者的目标。 Windows Server 2016 中的软件定义网络（SDN）包含一个分布式网络防火墙，该防火墙允许您动态创建安全策略，从而保护应用程序免受来自网络内部或外部的攻击。 此分布式网络防火墙通过使你能够将你的应用程序隔离到网络中，将层添加到了你的安全性。 策略可以应用于虚拟网络基础结构中的任何位置，隔离 VM 到 VM 的流量、VM 到主机流量或 VM 到 Internet 的流量（必要时），这些流量适用于可能已泄露或以编程方式跨的各个系统多个子网。 使用 Windows Server 2016 软件定义的网络功能，还可以将传入流量路由或镜像到非 Microsoft 虚拟设备。 例如，你可以选择通过 Barracuda 虚拟设备发送所有电子邮件流量，以进行其他垃圾邮件筛选保护。 这使你可以轻松地在本地或云中的其他安全性中分层。

### <a name="other-gdpr-considerations-for-servers"></a>服务器的其他 GDPR 注意事项
GDPR 包括对泄露通知的明确要求，其中的个人数据泄露是指违反了严重_或非法破坏、丢失、更改、未经授权泄露或访问个人数据的安全漏洞传输、存储或以其他方式处理。_ "  很明显，如果无法在第一次检测到违规行为，则无法在72小时内开始继续满足严格的 GDPR 通知要求。

如 Windows 安全中心白皮书中所述， [破坏后：处理高级威胁](http://wincom.blob.core.windows.net/documents/Post_Breach_Dealing_with_Advanced_Threats_Whitepaper.pdf)

> "_违反了预侵害性，入侵后假设已发生违规情况-充当网络流量记录器和犯罪场景调查人员（CSI）。破坏后，为安全团队提供识别、调查和响应攻击所需的信息和工具集，否则将保持未检测到的和在该雷达图下面的内容。_ "

在本部分中，我们将介绍 Windows Server 如何帮助你满足 GDPR 违规通知义务。 这从了解可供 Microsoft 收集和分析的基础威胁数据开始，并通过 Windows Defender 高级威胁防护（ATP）通过 Windows Defender 高级威胁防护（ATP）使数据对你至关重要。

#### <a name="insightful-security-diagnostic-data"></a>见解深入的安全诊断数据
近数十年来，Microsoft 一直在将威胁转变为有用的智能，有助于巩固其平台和保护客户。 如今，随着云计算带来的巨大优势，我们正在寻找新的方法来利用威胁情报驱动的丰富分析引擎保护我们的客户。

通过应用自动化和手动流程、机器学习和人类专家的组合，我们可以创建一个能够自学并实时进行演变的 Intelligent Security Graph，从而减少用来检测和响应我们产品中的新事件的集体协作时间。

![Microsoft 智能安全性图](../media/GDPR-Windows-Server-Overview/gdpr-intelligent-security-graph.png)

Microsoft 的威胁情报范围（字面上）为数十亿个数据点：35000000000每月、每个企业和使用者之间扫描1000000000的、访问200多个云服务14000000000和每日执行的身份验证的消息。 所有这些数据都将由 Microsoft 代表你进行收集，以创建 Intelligent Security Graph，从而帮助你以动态方式保护前门，使其保持安全，并满足 GDPR 的要求。

#### <a name="detecting-attacks-and-forensic-investigation"></a>检测攻击和取证调查
即使是最好的端点防御最终也可能会被攻破，因为网络攻击变得越来越复杂和有针对性。 可以使用两种功能来帮助进行潜在的破坏性检测-Windows Defender 高级威胁防护（ATP）和 Microsoft 高级威胁分析（ATA）。

Windows Defender 高级威胁防护 (ATP) 可帮助你检测、调查和应对网络上的高级攻击和数据泄露。 GDPR 的数据泄露类型要求你通过技术安全措施进行保护，以确保个人数据和处理系统的持续机密性、完整性和可用性。

Windows Defender ATP 的主要优点如下：

- **检测无法检测的。** 从1000000000计算机和所有 Microsoft 服务中的多个计算机和信号构建的传感器深入到操作系统内核、Windows 安全专家和独特的光纤。

- **内置，而不是外加。** 无代理，具有高性能和最小影响，具有云动力;轻松管理，无需部署。 

- **用于 Windows 安全性的单一窗格。** 浏览6个月的丰富计算机时间线，从 Windows Defender ATP、Windows Defender 防病毒和 Windows Defender 设备防护统一安全事件。

- **Microsoft graph 的强大功能。** 利用 Microsoft 智能安全图形将检测和浏览功能与 Office 365 ATP 订阅集成，以跟踪反馈和响应攻击。

有关详细信息，请参阅[Windows DEFENDER ATP 创意者更新预览版的新增功能](https://blogs.microsoft.com/microsoftsecure/2017/03/13/whats-new-in-the-windows-defender-atp-creators-update-preview/)。

ATA 是一项本地产品，可帮助检测组织中的身份泄露。 ATA 可以捕获和分析用于身份验证、授权和信息收集协议（例如 Kerberos、DNS、RPC、NTLM 和其他协议）的网络流量。 ATA 使用此数据生成有关网络上的用户和其他实体的行为配置文件，以便能够检测异常和已知攻击模式。 下表列出了 ATA 检测到的攻击类型。


|攻击类型 |描述 |
|---------|---------|
|恶意攻击 |通过从已知的攻击类型列表中查找攻击来检测这些攻击，其中包括：<ul><li>传递票证（PtT）</li><li>传递哈希（PtH）</li><li>超传递哈希</li><li>伪造 PAC （MS14-068）</li><li>黄金票证</li><li>恶意复制</li><li>侦察</li><li>暴力破解</li><li>远程执行</li></ul>有关可检测到的恶意攻击及其说明的完整列表，请参阅[ATA 可以检测哪些可疑活动？](https://docs.microsoft.com/advanced-threat-analytics/understand-explore/ata-threats)。|
|异常行为 |这些攻击通过使用行为分析来检测，并使用机器学习识别有疑问的活动，包括：<ul><li>异常登录</li><li>未知威胁</li><li>密码共享</li><li>横向移动</li></ul>|
|安全问题和风险 |通过查看当前的网络和系统配置来检测这些攻击，其中包括：<ul><li>信任中断</li><li>弱协议</li><li>已知协议漏洞</li></ul>|

你可以使用 ATA 帮助检测攻击者尝试破坏特权标识。 有关部署 ATA 的详细信息，请参阅[高级威胁分析文档](https://docs.microsoft.com/advanced-threat-analytics/)中的计划、设计和部署主题。

## <a name="related-content-for-associated-windows-server-2016-solutions"></a>关联 Windows Server 2016 解决方案的相关内容

- **Windows Defender 防病毒：** https://www.youtube.com/watch?v=P1aNEy09NaI 和 https://docs.microsoft.com/windows/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10

- **Windows Defender 高级威胁防护：** https://www.youtube.com/watch?v=qxeGa3pxIwg 和 https://docs.microsoft.com/windows/threat-protection/windows-defender-atp/configure-server-endpoints-windows-defender-advanced-threat-protection

- **Windows Defender Device Guard：** https://www.youtube.com/watch?v=F-pTkesjkhI 和 https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide

- **Windows Defender Credential Guard：** https://www.youtube.com/watch?v=F-pTkesjkhI 和 https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard

- **控制流防护：** https://msdn.microsoft.com/library/windows/desktop/mt637065(v=vs.85).aspx

- **安全性和保证：** https://docs.microsoft.com/windows-server/security/security-and-assurance

## <a name="disclaimer"></a>免责声明
本文是截至发布日期 Microsoft 对 GDPR 的评论。 我们花费了大量时间来 GDPR，并希望我们非常熟悉其意图和含义。 但是 GDPR 的应用高度具体，我们对 GDPR 的解释并非面面俱到。

因此，本文仅供参考，不应作为法律意见或确定 GDPR 在贵组织内的适用程度的依据。 我们鼓励你与具有法律资质的专业人士合作，共同探讨 GDPR、它在贵组织内的适用程序以及确保合规性的最佳做法。

MICROSOFT 对本文中的信息不做任何保证（明示的、暗示的或法定的）。 本文按“原样”提供。 本文中表达的信息和视图（包括 URL 和其他 Internet 网站参考）如有更改，恕不另行通知。

本文未向你提供针对任何 Microsoft 产品的任何知识产权的任何法律权限。  你可以复制本文并仅将其用于内部参考用途。  

发布日期 2017 年 9 月<br>
版本 1.0<br>
© 2017 Microsoft。 保留所有权利。


