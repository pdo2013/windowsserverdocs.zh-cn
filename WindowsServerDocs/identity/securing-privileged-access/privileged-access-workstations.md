---
title: 为什么特权访问工作站可以帮助保护你的组织
description: PAW 如何提高组织的安全状况
ms.prod: windows-server
ms.topic: article
ms.assetid: 93589778-3907-4410-8ed5-e7b6db406513
ms.date: 03/13/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: mas
ms.openlocfilehash: fb91ca583fd71a7fbe38369606d2dcc4a816d8aa
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935009"
---
# <a name="privileged-access-workstations"></a>特权访问工作站

>适用于：Windows Server

特权访问工作站 (PAW) 为免受 Internet 攻击和威胁媒介的敏感任务提供专用操作系统。 从日常使用的工作站和设备中分离这些敏感任务和帐户可提供非常强大的保护措施，防止网络钓鱼攻击、应用程序和操作系统漏洞、各种模拟攻击和凭据盗窃攻击（如击键）日志记录、[哈希传递](https://aka.ms/pth)和票证传递。

## <a name="what-is-a-privileged-access-workstation"></a>什么是特权访问工作站？

简单来说，PAW 是为敏感帐户和任务提供高安全性而设计的强化且锁定的工作站。  建议使用 PAW 管理标识系统、云服务、私有云构造及敏感业务功能。

> [!NOTE]
> PAW 体系结构不需要将帐户 1:1 映射到工作站，虽然这是一种常见配置。 PAW 可创建由一个或多个帐户使用的受信任的工作站环境。

为了提供最高的安全性，Paw 应始终运行最新和安全的操作系统：Microsoft 强烈建议使用 Windows 10 企业版，其中包括其他版本中未提供的其他一些安全功能（特别是[Credential guard](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)和[Device guard](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)）。

> [!NOTE]
> 没有 Windows 10 企业版访问权限的组织可以使用 Windows 10 专业版，其中包括 PAW 的许多关键基础技术，包括受信任的引导、BitLocker 和远程桌面。  教育客户可以使用 Windows 10 教育版。  Windows 10 家庭版不应用于 PAW。
>
> 对于 Windows 10 不同版本的比较矩阵，请参阅[此文章](https://www.microsoft.com/en-us/WindowsForBusiness/Compare)。

PAW 安全控件侧重于降低严重影响和危害的可能性较高的风险。 这包括缓解对环境的攻击，以及在一段时间内可降低 PAW 控制有效性的风险：

* **Internet 攻击** - 大多数攻击直接或间接地源于 Internet 源，并使用 Internet 进行渗透及命令和控制 (C2)。 将 PAW 与开放的 Internet 隔离是确保 PAW 不会被入侵的关键因素。
* **可用性风险** - 如果 PAW 因太难而无法在日常任务中使用，管理员将积极创建变通方法，以使他们的作业变得简单。 通常情况下，这些变通方法会使管理工作站和帐户面临重大安全风险，因此，安全地使 PAW 用户参与并授权其来缓解这些可用性问题很重要。 这可以通过以下方式完成：侦听其反馈、安装执行其作业所需的工具和脚本，并确保所有管理人员都知道为什么需要使用 PAW，什么是 PAW，以及如何正确成功地使用它。
* **环境风险** - 由于环境中的很多其他计算机和帐户直接或间接地暴露在 Internet 风险中，必须保护 PAW 免受生产环境中被入侵的资产攻击。 这要求最大程度地减少对 Paw 具有访问权限的管理工具和帐户的使用，以保护和监视这些专用工作站。
* **供应链篡改** - 虽然不可能清除硬件和软件供应链中所有可能的篡改风险，但是执行一些关键操作可以缓解攻击者现已可用的关键攻击媒介。 这些操作包括验证所有安装介质的完整性（[清洁源原则](https://aka.ms/cleansource)），以及使用受信任且信誉好的供应商提供的硬件和软件。
* **物理攻击** - PAW 可以物理方式移动，并可用于物理安全设施的外部，因此，必须保护它们免受利用未经授权的计算机物理访问权限的攻击。

> [!NOTE]
> PAW 不会保护已通过 Active Directory 林获得管理权限的环境的免受攻击者攻击。
> 因为现有许多已实现的 Active Directory 域服务在凭据被盗的风险中已运行数年，组织应假定存在漏洞，并考虑其可能具有未检测到的域或企业管理员凭据被入侵的可能性。 怀疑域被入侵的组织应考虑使用专业事件响应服务。
>
> 有关响应和恢复指南的详细信息，请参阅[缓解哈希传递和其他凭据被盗](https://aka.ms/pth)第 2 版中的“对可疑活动进行响应”和“从漏洞中恢复”部分。
>
> 有关详细信息，请访问 [Microsoft 事件响应和恢复服务](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)页。

### <a name="paw-hardware-profiles"></a>PAW 硬件配置文件

管理人员也是标准用户，他们还需要 PAW 以及标准用户工作站来检查电子邮件、浏览 web 和访问公司业务线应用程序。  确保管理员的工作效率和安全是任何 PAW 部署成功的基本条件。  用户会放弃严重限制工作效率的安全解决方案，而青睐于可提升工作效率的解决方案（即使它是通过不安全的方式来提升工作效率）。

为平衡安全需要与工作效率需要，Microsoft 建议使用其中一个 PAW 硬件配置文件：

* **专用硬件**-用于用户任务的专用设备与管理任务。
* **同时使用** - 利用操作系统或演示虚拟化，可以同时运行用户任务和管理任务的单个设备。

组织可以仅使用一个配置文件，也可以同时使用两个配置文件。 硬件配置文件之间没有互操作性问题，组织可以灵活地将硬件配置文件与具有特定需要和情况的指定管理员相匹配。

> [!NOTE]
> 重要的是，在所有这些情况下，管理人员颁发的是一个与指定管理帐户不同的标准用户帐户。 管理帐户应仅在 PAW 管理操作系统上使用。

此表从操作易用性、工作效率和安全性的角度总结了每个硬件配置文件的相对优点和缺点。  两种硬件方法均可提供强大的安全功能，使管理帐户免于凭据被盗及重复使用。

|**方案**|**优点**|**缺点**|
|--------|---------|-----------|
|专用硬件|- 强信号的任务敏感度<br />- 最强安全隔离|- 其他安全空间<br />- 其他权重（适用于远程工作）<br />- 硬件成本|
|同时使用|- 更低的硬件成本<br />- 单一设备体验|- 共享单一键盘/鼠标会导致疏忽性错误/风险|

本指南包含针对专用硬件方法的 PAW 配置的详细说明。 如果你有同时使用硬件配置文件的需求，可以在本指南的基础上自行调整操作说明，或雇用类似于 Microsoft 的专业服务组织帮助实施。

#### <a name="dedicated-hardware"></a>专用硬件

在此方案中，PAW 用于完全独立于处理日常活动（例如电子邮件、文档编辑及开发工作）的 PC 的管理。 所有管理工具和应用程序均安装在 PAW 上，所有生产力应用程序均安装在标准用户工作站上。 本指南中的分步说明基于此硬件配置文件。

#### <a name="simultaneous-use---adding-a-local-user-vm"></a>同时使用-添加本地用户 VM

在此同时使用方案中，一台 PC 同时用于管理任务和日常活动（例如电子邮件、文档编辑及开发工作）。 在此配置中，用户操作系统在断开连接时仍可用（可编辑文档并处理本地缓存的电子邮件），但需要可以适应此断开连接的状态的硬件和支持进程。

![显示在同时使用方案中，一台电脑同时用于管理任务和日常活动（例如电子邮件、文档编辑及开发工作）的图](../media/privileged-access-workstations/PAWFig10.JPG)

物理硬件在本地运行两个操作系统：

* **管理员操作系统** - 物理主机在 PAW 主机上运行 Windows 10 来执行管理任务
* **用户操作系统** - Windows 10 客户端 Hyper-V 虚拟机来宾运行企业映像

使用 Windows 10 Hyper-v，来宾虚拟机（也是运行 Windows 10）可以拥有丰富的用户体验，包括声音、视频和 Internet 通信应用程序（例如 Skype for Business）。

在此配置中，不需要管理权限的日常工作是在用户操作系统虚拟机中完成的，该虚拟机具有常规企业 Windows 10 映像，并且不受应用于 PAW 主机的限制的约束。 所有管理工作都在管理员操作系统上完成。

若要对此进行配置，按照本指南中对 PAW 主机的说明进行操作，添加客户端 Hyper-V 功能、创建用户虚拟机，然后在用户虚拟机上安装 Windows 10 企业映像。

请参阅文章[客户端 Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/index)，获取有关此功能的详细信息。 请注意，来宾虚拟机中的操作系统需要按 [Microsoft 产品许可](https://www.microsoft.com/en-us/Licensing/product-licensing/products.aspx)进行授权，我们还在[此处](https://download.microsoft.com/download/9/8/D/98D6A56C-4D79-40F4-8462-DA3ECBA2DC2C/Licensing_Windows_Desktop_OS_for_Virtual_Machines.pdf)进行了说明。

#### <a name="simultaneous-use---adding-remoteapp-rdp-or-a-vdi"></a>同时使用-添加 RemoteApp、RDP 或 VDI

在此同时使用方案中，一台 PC 同时用于管理任务和日常活动（例如电子邮件、文档编辑及开发工作）。 在此配置中，用户操作系统将集中部署和管理（在云中或在数据中心中），但在断开连接时不可用。

![显示在同时使用方案中，一台电脑同时用于管理任务和日常活动（例如电子邮件、文档编辑及开发工作）的图](../media/privileged-access-workstations/PAWFig11.JPG)

物理硬件在本地运行单个 PAW 操作系统来执行管理任务，并为用户应用程序（例如电子邮件、文档编辑及业务线应用程序）联系 Microsoft 或第三方远程桌面服务。

在此配置中，不需要管理权限的日常工作在不受应用于 PAW 主机的限制约束的远程操作系统和应用程序中完成。 所有管理工作都在管理员操作系统上完成。

要对此进行配置，请按照本指南中对 PAW 主机的说明进行操作，允许到远程桌面服务的网络连接，然后将快捷方式添加至 PAW 用户桌面，以便访问应用程序。 远程桌面服务可通过多种方式进行托管，包括：

* 现有远程桌面或 VDI 服务（在本地或在云中）
* 在本地或在云中安装的新服务
* 使用预配置的 Office 365 模板或你所有的安装映像的 Azure RemoteApp

有关 Azure RemoteApp 的详细信息，请访问[此页面](https://www.remoteapp.windowsazure.com)。

## <a name="how-microsoft-is-using-administrative-workstations"></a>Microsoft 如何使用管理工作站

Microsoft 同时在内部系统和客户系统中使用 PAW 体系结构方法。 Microsoft 在内部使用管理工作站，其中包括 Microsoft IT 基础结构、Microsoft 云结构基础结构开发和运营以及其他高价值资产的管理。

本指南的直接依据是我们的网络安全专业服务团队部署的特权访问工作站 (PAW) 参考体系结构，能够保护客户免受网络安全攻击。 管理工作站还是为域管理任务、增强的安全管理环境 (ESAE) 管理林参考体系结构提供最强保护的关键元素。

有关 ESAE 管理林的详细信息，请参阅[保护特权访问参考资料](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach)中的*ESAE 管理林设计方法*部分。

## <a name="architecture-overview"></a>体系结构概述

下图描绘了管理（高度敏感任务）的单独“通道”，该“通道”通过维护单独的专用管理帐户和工作站创建。

![显示管理（高度敏感任务）的单独“通道”的图，该“通道”通过维护单独的专用管理帐户和工作站创建](../media/privileged-access-workstations/PAWFig1.JPG)

此体系结构方法在 Windows 10 [凭据保护](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)和[设备保护](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)功能的基础上构建，但为敏感帐户和任务提供的保护远不止这两项保护功能。

此方法适用于对高值资产具有访问权限的帐户：

* **管理权限**-PAW 为 IT 管理角色和任务带来高影响，提高安全性。 这种体系结构可以应用于许多类型的系统管理，包括 Active Directory 域和林、Microsoft Azure Active Directory 租户、Office 365 租户、流程控制网络 (PCN)、监管控制和数据采集 (SCADA) 系统、自动取款机 (ATM) 和销售点 (PoS) 设备。
* **高敏感度信息工作者**-在 PAW 中使用的方法还可为高度敏感的信息工作者任务和人员提供保护，例如涉及预公告合并和收购活动的人员、预发布的财务报表、组织的社交媒体状态、执行通信、unpatented 的商业机密、敏感研究或其他专有数据或敏感数据。 本指南不深入讨论这些信息工作者方案的配置，也不在技术说明中包含此种情况。

    > [!NOTE]
    > Microsoft IT 使用 PAW （内部称为“安全管理工作站”或 SAW）来管理对 Microsoft 内部的内部高值系统的安全访问权限。 在下方的“Microsoft 如何使用管理工作站”部分，本指南还包括了针对 PAW 使用方法的其他信息。 有关此高值资产环境的详细信息，请参阅文章[使用安全管理工作站保护高值资产](https://msdn.microsoft.com/library/mt186538.aspx)。

本文档将介绍为什么推荐这种做法来保护高影响力特权帐户、保护管理权限的这些 PAW 解决方案是怎样的，以及如何为域和云服务管理快速部署 PAW 解决方案。

本文档提供了用于实现数种 PAW 配置的详细指导，包括详细的操作说明，使你得以保护常用的高影响力帐户：

* [**阶段 1-为 Active Directory 管理员立即部署**](#phase-1-immediate-deployment-for-active-directory-administrators)，这提供了一个可在本地域和林管理角色上保护的 PAW
* [**阶段 2-将 PAW 扩展到所有管理员**](#phase-2-extend-paw-to-all-administrators)这使得云服务（如 Office 365 和 Azure、企业服务器、企业应用程序和工作站）的管理员可以进行保护
* [**阶段 3-高级 PAW 安全**](#phase-3-extend-and-enhance-protection)这讨论了 PAW 安全性的其他保护和注意事项

### <a name="why-dedicated-workstations"></a>为什么选择专用工作站？

组织当前的威胁环境是存在大量可对 Internet 暴露帐户和工作站构成持续安全入侵的复杂仿冒攻击及其他 Internet 攻击。

此威胁环境要求组织在设计对高价值资产（如管理帐户和敏感业务资产）的保护时采用 "假定违反" 安全状况。 需要保护这些高值资产免受直接 Interne t威胁，以及来自环境中其他工作站、服务器和设备的攻击。

![显示如果攻击者获取了使用敏感凭据的用户工作站的控制权限，托管资产会遭遇的风险的图](../media/privileged-access-workstations/PAWFig2.JPG)

此图描述了如果攻击者获取了使用敏感凭据的用户工作站的控制权限，托管资产会遭遇的风险。

获取操作系统权限的攻击者可以使用多种方式非法获取工作站上所有活动的访问权限，并可以模拟合法帐户。 多种已知和未知的攻击技术可以用于获得此级别的访问权限。 网络攻击不断增加的数量和复杂性使分离概念扩展到为敏感帐户完全分离客户端操作系统变得很有必要。 有关这些攻击类型的详细信息，请访问[传递哈希网站](https://www.microsoft.com/pth)，获取内容丰富的白皮书、视频等。

PAW 方法是管理人员使用单独的管理员和用户帐户时广为接受的建议做法的延伸。 该做法使用单独分配的、完全独立于用户的标准用户帐户的管理帐户。 PAW 在帐户分离做法的基础上构建，可为这些敏感帐户提供可信的工作站。

本 PAW 指南旨在帮助实现保护高值帐户（例如高权限 IT 管理员和高敏感度的商业帐户）的功能。 本指南可帮助你：

* 将凭据限制为仅暴露给受信任的主机
* 向管理员提供安全性较高的工作站，以便他们可以轻松地执行管理任务。

将敏感帐户限制为仅使用强化的 PAW 是对管理员频繁使用而且攻击者非常难以获取的这些帐户的直接保护。

### <a name="alternate-approaches"></a>备用方法

本部分包含与 PAW 相比，备选方法安全性的信息，以及如何如何在 PAW 体系结构中正确集成这些方法的信息。 在隔离实现时，所有这些方法都具有重大风险，但在某些情况下可以在 PAW 实现中添加值。

#### <a name="credential-guard-and-windows-hello-for-business"></a>Credential Guard 和 Windows Hello 企业版

在 Windows 10 中引入的[凭据保护](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)使用基于硬件和虚拟化的安全性，通过保护派生的凭据来缓解常见的凭据被盗攻击（例如哈希传递）。 受信任的平台模块（TPM）硬件也可以保护[Windows Hello 企业版](https://aka.ms/passport)所使用的凭据的私钥。

这些是功能强大的缓解措施，但即使凭据受到 Credential Guard 或 Windows Hello 企业版的保护，工作站仍可能易受到某些攻击。 攻击可以直接从被泄露的设备中包括滥用权限和凭据，在工作站上启用 Credential Guard 并滥用管理工具和弱应用程序配置之前，重复使用以前盗用的凭据。

本部分的 PAW 指南包括针对高敏感性帐户和任务使用多种技术。

#### <a name="administrative-vm"></a>管理虚拟机

管理虚拟机（管理员 VM）是一种专用操作系统，适用于在标准用户桌面上托管的管理任务。 虽然此方法在为管理任务提供专用操作系统方面与 PAW 类似，但它具有致命缺陷，即此管理虚拟机依赖于标准用户桌面才能实现其安全性。

下图描述了攻击者按照控件链找到用户工作站上的管理员虚拟机关注的目标对象，但难以在相反的配置上创建路径的功能。

PAW 体系结构不允许在用户工作站上托管管理员 VM，但具有标准企业映像的用户 VM 可以托管在管理 PAW 上，为人员提供适用于所有责任的一台 PC。

![PAW 体系结构关系图](../media/privileged-access-workstations/PAWFig9.JPG)

#### <a name="shielded-vm-based-paws"></a>基于防护 VM 的 Paw

管理 VM 模型的一种安全变体是使用[受防护的虚拟机](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md)将一个或多个管理 vm 与用户 VM 一起托管。
受防护的 Vm 旨在在可能不受信任的用户或代码可能在物理计算机的标准用户桌面上运行的环境中运行安全工作负荷。
受防护的 VM 有一个虚拟 TPM，它允许它加密其自己的静态数据，并禁用多个管理控制（如基本控制台访问、PowerShell Direct）以及调试 VM 的功能，以进一步将 VM 与标准用户桌面隔离，并其他虚拟机。
受防护的 VM 的密钥存储在受信任的密钥管理服务器上，这要求物理设备在发布密钥以启动 VM 之前证明其身份和运行状况。
这可确保受防护的 Vm 只能在目标设备上启动，且这些设备正在运行已知和受信任的软件配置。

由于受防护的 Vm 彼此隔离，而标准用户桌面是可接受的，因此可以在单个主机上运行多个受防护的 PAW Vm，即使这些管理 Vm 管理不同的层也是如此。

有关详细信息，请参阅下面的[使用受保护的构造部署 paw](#deploy-paws-using-a-guarded-fabric)部分。

#### <a name="jump-server"></a>跳转服务器

管理“跳转服务器”体系结构设置少量管理控制服务器，并限制员工使用这些服务器执行执行管理任务。 这通常取决于远程桌面服务、第三方演示虚拟化解决方案或虚拟桌面基础结构 (VDI) 技术。

此方法经常建议用于缓解风险管理，并且提供了一些安全保证，但跳转服务器方法本身容易遭受某些攻击，因为它违反了[“清洁源”原则](../securing-privileged-access/securing-privileged-access-reference-material.md#clean-source-principle)。 “清洁源”原则要求所有安全依赖关系与受保护的对象一样可信。

![显示简单的控制关系的图](../media/privileged-access-workstations/PAWFig3.JPG)

此图描述了简单的控制关系。 控制对象的任何使用者均是该对象的安全依赖关系。 如果攻击者可以控制目标对象（使用者）的安全依赖关系，那么他们可以控制该对象。

跳转服务器上的管理会话依赖于访问它的本地计算机的完整性。 如果此计算机是面临仿冒攻击和其他基于 Internet 的攻击媒介的用户工作站，则管理会话仍面临这些风险。

![显示攻击者如何按照已建立的控件链找到关注的目标对象的图](../media/privileged-access-workstations/PAWFig4.JPG)

上图描述了攻击者如何按照已建立的控件链找到关注的目标对象。

虽然某些高级的安全控件（例如多重身份验证）可以增加攻击者从用户工作站接管此管理会话的难度，但如果攻击者具有源计算机的管理访问权限（例如将非法命令注入合法会话、劫持合法进程等），则任何一项安全功能均无法充分抵御技术攻击。

本 PAW 指南中的默认配置在 PAW 上安装管理工具，如有需要，还可以添加跳转服务器体系架构。

![显示反转控制关系及从管理工作站访问用户应用如何使攻击者无法获取目标对象的路径的图](../media/privileged-access-workstations/PAWFig5.JPG)

此图显示反转控制关系及从管理工作站访问用户应用如何使攻击者无法获取目标对象的路径。 用户跳转服务器仍暴露于风险之中，因此，仍应为面向 Internet 的计算机应用正确的保护控件、检测控件和响应流程。

此配置要求管理员严格遵循操作实践，以确保他们不会意外地在其桌面上将管理员凭据输入到用户会话中。

![显示从 PAW 访问管理跳转服务器如何做到不为攻击者添加至管理资产的路径的图](../media/privileged-access-workstations/PAWFig6.JPG)

此图显示了从 PAW 访问管理跳转服务器如何做到不为攻击者添加至管理资产的路径。 在此情况下，带有 PAW 的跳转服务器允许你合并用于监控管理活动和用于分发管理应用程序与工具的位置数量。 这会增加一些设计复杂性，但如果你在 PAW 实现中使用大量帐户和工作站，这可以简化安全监控和软件更新。 需要使用与 PAW 类似的安全标准才能对跳转服务器进行构建和配置。

#### <a name="privilege-management-solutions"></a>特权管理解决方案

特权管理解决方案是按需提供对离散权限或特权帐户的临时访问权限的应用程序。 特权管理解决方案是完整策略的一个极具价值的组件，可为特权访问提供保护，并提供管理活动至关重要的可见性和责任性。

这些解决方案通常使用灵活的工作流来授予访问权限，其中许多方案具有其他安全功能（例如服务帐户密码管理及与管理跳转服务器集成）。 市场上能提供特权管理功能的解决方案有许多，其中之一就是 Microsoft 标识管理器 (MIM) 特权访问管理 (PAM)。

Microsoft 建议使用 PAW 访问特权管理解决方案。 应仅向 PAW 授予这些解决方案的访问权限。 Microsoft 不建议将这些解决方案用作 PAW 的替代方案，因为从可能被入侵的用户桌面使用这些解决方案访问特权违反了如下图中所示的[清洁源](https://aka.ms/cleansource)原则：

![显示 Microsoft 不建议将这些解决方案用作 PAW 的替代方案的原因的图，因为从可能被入侵的用户桌面使用这些解决方案访问特权违反了清洁源原则](../media/privileged-access-workstations/PAWFig7.JPG)

通过使 PAW 具备这些解决方案的访问权限，你可以同时获得 PAW 和特权管理解决方案的双重优势，具体如此图中所示：

![显示通过使 PAW 具备这些解决方案的访问权限，你可以同时获得 PAW 和特权管理解决方案的双重优势的图](../media/privileged-access-workstations/PAWFig8.JPG)

> [!NOTE]
> 这些系统应在它们所管理特权的最高层进行分类，并受到此安全级别或更高级别的保护。 这些系统通常被配置为管理第 0 层解决方案和第 0 层资产，因此，应在第 0 层进行分类。
> 有关层模型的详细信息，请参阅[https://aka.ms/tiermodel](https://aka.ms/tiermodel)有关第0层组的详细信息，请参阅[保护特权访问参考资料](../securing-privileged-access/securing-privileged-access-reference-material.md)中的第0层。

有关部署 Microsoft Identity Manager （MIM）特权访问管理（PAM）的详细信息，请参阅[https://aka.ms/mimpamdeploy](https://aka.ms/mimpamdeploy)

## <a name="paw-scenarios"></a>PAW 方案

本部分包含此 PAW 指南应当应用于哪个方案的说明。 在所有方案中，应培训管理员仅使用 PAW 为远程系统提供支持。 为鼓励成功、安全的使用，还应鼓励所有 PAW 用户提供反馈，以改进 PAW 体验，并且应仔细查看这种反馈，以便集成到 PAW 程序中。

在所有方案中，稍后阶段中的其他强化版本和本指南中的不同硬件配置文件可能用于满足角色的可用性或安全要求。

> [!NOTE]
> 本指南明确区分要求访问 internet 上的特定服务（例如 Azure 和 Office 365 管理门户）和所有主机和服务的 "开放 Internet"。

请参阅[层模型页](https://aka.ms/tiermodel)，获取有关层指定的详细信息。

|**方案**|**使用 PAW？**|**范围和安全注意事项**|
|---------|--------|---------------------|
|Active Directory 管理员 - 第 0 层|是|使用阶段 1 指南构建的 PAW 对此角色来说已足够。<br /><br />- 可以添加管理林来为此方案提供最强的保护。 有关 ESAE 管理林的详细信息，请参阅 [ESAE 管理林设计方法](../securing-privileged-access/securing-privileged-access-reference-material.md#esae-administrative-forest-design-approach)<br />- PAW 可用于管理多个域或多个林。<br />-如果域控制器托管在基础结构即服务（IaaS）或本地虚拟化解决方案上，则应为这些解决方案的管理员实现 Paw 的优先级。|
|Azure IaaS 和 PaaS 服务管理员 - 第 0 层或第 1 层（请参阅“范围和设计注意事项”）|是|使用阶段 2 中提供的指南构建的 PAW 对于此角色来说已足够。<br /><br />- PAW 应至少用于全局管理员和订阅计费管理员。 PAW 还应用于关键或敏感服务器的委派管理员。<br />-Paw 应该用于管理为云服务（例如[Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect/)和 ACTIVE DIRECTORY 联合身份验证服务（ADFS））提供目录同步和联合身份验证的操作系统和应用程序。<br />- 出站网络限制必须仅允许连接至使用阶段 2 中指南的已授权云服务。 PAW 不允许任何开放 Internet 接入。<br />-应在工作站上配置 Windows Defender Exploit Guard**说明：**   如果域控制器或其他第0层主机位于订阅中，则将订阅视为林的第0层。 如果没有第 0 层服务器托管在 Azure 中，则订阅将是第 1 层。|
|管理 Office 365 租户 <br />- 第 1 层|是|使用阶段 2 中提供的指南构建的 PAW 对于此角色来说已足够。<br /><br />- PAW 应至少用于订阅计费管理员、全局管理员、Exchange 管理员、SharePoint 管理员及用户管理管理员角色。 你还应认真考虑将 PAW 用于极为关键或敏感数据的委派管理员。<br />-应在工作站上配置 Windows Defender 攻击防护。<br />- 出站网络限制必须仅允许连接至使用阶段 2 中的指南的 Microsoft 服务。 PAW 不允许任何开放 Internet 接入。|
|其他 IaaS 或 PaaS 云服务管理员<br />- 第 0 层或第 1 层（请参阅“范围和设计注意事项”）|是|使用阶段 2 中提供的指南构建的 PAW 对于此角色来说已足够。<br /><br />- PAW 应该用于具有云托管虚拟机管理权限的任意角色，包括安装代理、导出硬盘文件，或访问包含操作系统信息、敏感数据或关键业务数据的硬盘驱动器的存储等功能。<br />- 出站网络限制必须仅允许连接至使用阶段 2 中的指南的 Microsoft 服务。 PAW 不允许任何开放 Internet 接入。<br />-应在工作站上配置 Windows Defender 攻击防护。 **注意：** 如果域控制器或其他第0层主机位于订阅中，则订阅为林的第0层。 如果没有第 0 层服务器托管在 Azure 中，则订阅将是第 1 层。|
|虚拟化管理员<br />- 第 0 层或第 1 层（请参阅“范围和设计注意事项”）|是|使用阶段 2 中提供的指南构建的 PAW 对于此角色来说已足够。<br /><br />- PAW 应该用于具有虚拟机管理权限的任意角色，包括安装代理、导出虚拟硬盘文件，或访问包含操作系统信息、敏感数据或关键业务数据的硬盘驱动器的存储等功能。 **注意：** 如果域控制器或其他第0层主机位于订阅中，则虚拟化系统（及其管理员）被视为林的第0层。 如果没有第 0 层服务器托管在虚拟化系统中，则订阅将是第 1 层。|
|服务器维护管理员<br />- 第 1 层|是|使用阶段 2 中提供的指南构建的 PAW 对于此角色来说已足够。<br /><br />- PAW 应该用于更新、修补及检修运行 Windows Server、Linux 和其他操作系统的企业服务器和应用程序的管理员。<br />- 可能需要为 PAW 添加专用管理工具，以便供更多管理员使用。|
|用户工作站管理员 <br />- 第 2 层|是|使用阶段 2 中提供的指南构建的 PAW 对于具有最终用户设备管理权限的角色（例如支持人员和桌端支持角色）来说已足够。<br /><br />- 可能需要在 PAW 上安装其他应用程序，以便启用票证管理及其他支持功能。<br />-应在工作站上配置 Windows Defender 攻击防护。<br />    可能需要为 PAW 添加专用管理工具，以便供更多管理员使用。|
|SQL、SharePoint 或业务线 (LOB) 管理员<br />- 第 1 层|是|使用阶段 2 指南构建的 PAW 对于此角色来说已足够。<br /><br />- 可能需要在 PAW 上安装其他管理工具，使管理员无需使用远程桌面连接服务器，即可管理应用程序。|
|管理社交媒体状态的用户|部分|使用阶段 2 中提供的指南构建的 PAW 可以用作为这些角色提供安全功能的起点。<br /><br />- 使用 Azure Active Directory (AAD) 保护和管理社交媒体帐户以共享、保护和跟踪社交媒体帐户的访问权限。<br />    有关此功能的详细信息，请参阅[此博客文章](http://blogs.technet.com/b/ad/archive/2015/02/20/azure-ad-automated-password-roll-over-for-facebook-twitter-and-linkedin-now-in-preview.aspx)。<br />- 出站网络限制必须允许连接到这些服务。 这可通过允许开放的 Internet 连接（否定许多 PAW 保证的更高安全风险）或仅允许服务所需的 DNS 地址（可能比较难于获取）完成。|
|标准用户|否|尽管许多强化步骤可为标准用户所用，但 PAW 设计为将帐户与多数用户完成工作职责所需的开放 Internet 接入隔离。|
|来宾 VDI/网亭|否|虽然许多强化步骤可为来宾用于网亭系统，PAW 体系结构旨在为高敏感度帐户而非较低敏感度帐户提供更高的安全性。|
|VIP 用户（总经理、研究人员等）|部分|使用阶段2中提供的指南构建的 PAW 可以用作提供这些角色安全性的起点。<br /><br />- 此方案与标准用户桌面类似，但通常有一个更小、更简单且已知的应用程序配置文件。 此方案通常需要发现和保护敏感数据、服务及应用程序（应用程序可能会安装在桌面上，也可能不会）。<br />- 这些角色通常要求高度安全性和极高的可用性，这就要求更改设计以满足用户偏好。|
|工业控制系统（例如 SCADA、PCN 和 DCS）|部分|使用阶段 2 中提供的指南构建的 PAW 可作为起点，为这些角色提供安全性，因为与大多数 ICS 控制台（包括 SCADA 和 PCN 等通用标准）不需要浏览开放的 Internet 及检查电子邮件。<br /><br />-用于控制物理机械的应用程序必须进行集成和测试，以确保其兼容性和受保护。|
|嵌入式操作系统|否|虽然 PAW 的许多强化步骤可用于嵌入式操作系统，但在此方案中，需要开发自定义解决方案进行强化。|

> [!NOTE]
> **组合方案**某些员工可能具有跨越多个方案的管理职责。
> 在这些情况下，要记住的关键规则是必须始终遵循层模型规则。 请参阅层模型页获取详细信息。

> [!NOTE]
> **缩放 PAW 程序** 随着 PAW 程序的缩放以包含更多管理员和角色，需要继续确保持续符合安全标准及可用性。 这就要求你更新 IT 支持结构或创建新的结构来应对 PAW 特定挑战（例如 PAW 载入进程、事件管理、配置管理及收集反馈以解决可用性挑战）。  举例来说，组织决定为管理员启用在家工作方案，这就势必会造成从桌面 PAW 到笔记本电脑 PAW 的转变，这种转变可能会使其他安全注意事项成为必然考虑因素。  另一个常见示例是为新管理员创建或更新培训，培训内容现在必须包括正确使用 PAW（包括为什么 PAW 的正确使用很重要以及什么是 PAW，什么不是 PAW）。  有关在缩放 PAW 程序时需要考虑的更多注意事项，请参阅本说明的阶段 2。

本指南包含针对如上所述方案的 PAW 配置的详细说明。 如果你对其他方案有需求，可以在本指南的基础上自行调整操作说明，或雇用类似于 Microsoft 的专业服务组织帮助实施。

有关将 Microsoft 服务用于为环境量身打造一款 PAW 的详细信息，请联系 Microsoft 代表，或访问[此页面](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)。

## <a name="paw-phased-implementation"></a>PAW 分阶段实现

因为 PAW 必须为管理提供安全、受信任的源，所以安全可信的构建过程至关重要。  本部分将提供详细的说明，你可以使用与 Microsoft IT 和 Microsoft 云工程和服务管理组织所使用的概念非常类似的一般原则和概念构建你自己的 PAW。

说明分为以下三个阶段，重点是快速实施最关键的缓解措施，然后为企业逐渐增加并扩展 PAW 的使用范围。

* [阶段 1-为 Active Directory 管理员立即部署](#phase-1-immediate-deployment-for-active-directory-administrators)
* [阶段 2-将 PAW 扩展至所有管理员](#phase-2-extend-paw-to-all-administrators)
* [阶段 3-高级 PAW 安全](#phase-3-extend-and-enhance-protection)

请务必注意，即使各阶段是作为同一个整体项目的一部分进行计划并实施，也应始终按顺序执行。

### <a name="phase-1-immediate-deployment-for-active-directory-administrators"></a>第 1 阶段：即时部署 Active Directory 管理员

用途：提供可保护本地域和林管理角色的 PAW。

内第0层管理员，包括 Enterprise Admins、Domain Admins （适用于所有域）以及其他权威标识系统的管理员。

阶段 1 重点介绍管理你的本地 Active Directory 域的管理员，他们是经常成为攻击者目标的关键角色。 无论 Active Directory 域控制器 (DC) 是托管于本地数据中心、Azure 基础设施即服务 (IaaS) 中还是托管给其他 IaaS 供应商，这些标识系统都可以有效地保护这些管理员。

在此阶段，你可以创建安全管理 Active Directory 组织单位 (OU) 体系结构，以托管特权访问工作站 (PAW)，并使工作站自行部署 PAW。  此体系结构还包括支持 PAW 所需的组策略和组。  你可以使用 [TechNet 库](https://aka.ms/pawmedia)中的 PowerShell 脚本创建结构的大部分。

这些脚本将创建以下 OU 和安全组：

* 组织单位 (OU)
   * 六个新的顶级 Ou：Admin组合第1层服务器：工作站用户帐户;和计算机隔离。  每个顶级 OU 将包含多个子 Ou。
* 组
   * 六个新的启用安全的全局组：第0层复制维护;第1层服务器维护;服务台操作员;工作站维护;PAW 用户;PAW 维护。

还会创建多个组策略对象：PAW 配置-计算机;PAW 配置-User;所需的 RestrictedAdmin-Computer;PAW 出站限制;限制工作站登录;限制服务器登录。

阶段 1 包括以下步骤：

#### <a name="complete-the-prerequisites"></a>完成先决条件

1. **确保所有管理员均使用单独的单个帐户进行管理活动和最终用户活动**（包括电子邮件、Internet 浏览、业务线应用程序和其他非管理活动）。  将独立于员工标准用户帐户的管理帐户分配给每个已授权的员工是 PAW 模型的基础，因为 PAW 自身仅允许某些帐户登录。

   > [!NOTE]
   > 每个管理员都应使用他或她自己的帐户进行管理。  不要共享管理帐户。

2. **最小化第 0 层特权管理员的人数**。  因为每个管理员都必须使用 PAW，减少管理员数量将降低支持管理员所需的 PAW 数量及相关成本。 管理员数量的减少也降低了这些权限和相关风险的暴露机率。 虽然管理员在一个位置可以共享 PAW，但单独物理位置中的管理员将需要单独的 PAW。
3. **获取来自受信任的供应商、满足所有技术要求的硬件**。 Microsoft 建议获取满足文章[使用凭据保护来保护域凭据](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)中技术要求的硬件。

   > [!NOTE]
   > 无需这些功能即可安装在硬件上的 PAW 可以提供重要的保护功能，但凭据保护和设备保护等高级安全功能将不可用。  凭据保护和设备保护在阶段 1 部署中不作要求，但强烈建议将其作为阶段 3 部署的一部分（高级强化）。
   >
   > 确保用于 PAW 中的硬件来自于其安全做法受组织信任的制造商或供应商。 这是一款用于供应链安全、符合清洁源原则的应用程序。
   >
   > 有关供应链安全的重要性的详细背景信息，请访问[本站点](https://www.microsoft.com/security/cybersecurity/)。

4. **获取并验证所需的 Windows 10 企业版和应用程序软件**。 获取 PAW 所需的软件，并使用[安装介质的清洁源](https://aka.ms/cleansource)中的指南进行验证。

   * Windows 10 企业版
   * 适用于 Windows 10 的 [远程服务器管理工具](https://www.microsoft.com/en-us/download/details.aspx?id=45520)
   * [Windows 10 安全基线](https://aka.ms/win10baselines)

      > [!NOTE]
      > Microsoft 在 MSDN 上为所有操作系统和应用程序发布了 MD5 哈希值，但并不是所有软件供应商都提供类似文档。  在这些情况下将需要其他策略。  有关验证软件的其他信息，请参阅[安装介质的清洁源](https://aka.ms/cleansource)。

5. **确保 Intranet 上有可用的 WSUS 服务器**。 在 Intranet 上需要有 WSUS 服务器，以便下载并安装 PAW 更新程序。 此 WSUS 服务器应配置为自动批准所有 Windows 10 安全更新程序，或管理人员应有责任和义务快速批准软件更新程序。

   > [!NOTE]
   > 有关详细信息，请参阅[批准更新](https://technet.microsoft.com/library/cc708458(v=ws.10).aspx)指南中的“自动批准安装更新”部分。

#### <a name="deploy-the-admin-ou-framework-to-host-the-paws"></a>部署管理员 OU 框架来托管 PAW

1. 从 [TechNet 库](https://aka.ms/PAWmedia)中下载 PAW 脚本库

   > [!NOTE]
   > 下载所有文件并将其保存到同一目录，并按下面指定的顺序运行这些文件。  Create-PAWGroup 依赖于通过 Create-PAWOU 创建的 OU 结构，Set-PAWOUDelegation 依赖于通过 Create-PAWGroup 创建的组。
   > 不要修改任何脚本或逗号分隔值 (CSV) 文件。

2. **运行 Create-PAWOUs.ps1 脚本**。  此脚本将在 Active Directory 中创建新的组织单位 (OU) 结构，并在新 OU 上酌情阻止 GPO 继承。
3. **运行 Create-PAWGroups.ps1 脚本**。  此脚本将在相应的 OU 中创建新的全局安全组。

   > [!NOTE]
   > 尽管此脚本将创建新的安全组，但它并不自动填充这些组。

4. **运行 Set-PAWOUDelegation.ps1 脚本**。  此脚本将新 OU 的权限分配至相应的组。

#### <a name="move-tier-0-accounts-to-the-admintier-0accounts-ou"></a>将第0层帐户移动到 Admin\Tier 0 \ 帐户 OU

将每个是域管理员、企业管理员第 0 层等效组（包括嵌套成员身份）成员的帐户移动到该 OU。 如果组织具有已添加到这些组的自己的组，则应该将自己的组移动到 Admin\Tier 0\Groups OU。

   > [!NOTE]
   > 有关哪些组是第 0 层的详细信息，请参阅[保护特权访问参考资料](../securing-privileged-access/securing-privileged-access-reference-material.md)中的“第 0 层等效性”。

#### <a name="add-the-appropriate-members-to-the-relevant-groups"></a>将相应成员添加到相关组

1. **PAW 用户** - 使用域或你在阶段 1 的第 1 步中识别的 Enterprise Admin 组添加第 0 层管理员。
2. **PAW 维护** - 添加至少一个用于 PAW 维护和故障排除任务的帐户。 PAW 维护帐户仅在极少数情况下使用。

   > [!NOTE]
   > 不将相同的用户帐户或组同时添加至 PAW 用户和 PAW 维护。  PAW 安全模型一定程度上基于以下假设：PAW 用户帐户具有托管系统或 PAW 自身的特权权限，但并不同时具备上述两项特权权限。
   >
   > * 这对于在阶段 1 构建好的管理做法和习惯非常重要。
   > * 这对于阶段 2 及之后阶段通过 PAW 阻止特权提升至关重要，因为 PAW 可以跨层。
   >
   > 理想情况下，为了强制执行职责分离原则，不会将多个层的工作分配给某个员工，但 Microsoft 认识到，许多组织的员工数量有限（或具有其他组织要求），可能无法充分进行此分离。 在这些情况下，同一员工可能会分配到两种角色，但不应对这些功能使用相同的帐户。

#### <a name="create-paw-configuration---computer-group-policy-object-gpo"></a>创建 "PAW 配置-计算机" 组策略对象（GPO）

在本部分中，将创建一个新的 "PAW 配置-计算机" GPO，该 GPO 为这些 Paw 提供特定保护，并将其链接到第0层设备 OU （第0层中的 "设备"）。

   > [!NOTE]
   > **不要将这些设置添加到默认域策略**。  这样做可能会影响整个 Active Directory 环境中的操作。  仅在此处描述的新创建的 GPO 中配置这些设置，并仅将它们应用于 PAW OU。

1. **PAW 维护访问** - 此设置将 PAW 上特定特权组的成员身份设置为一组特定的用户。 转到“*计算机配置\首选项\控制面板设置\本地用户和组*”，然后执行以下步骤：
   1. 单击“**新建**”，然后单击“**本地组**”
   2. 选择“**更新**”操作，然后选择“Administrators (内置)”（不要使用浏览按钮选择域组管理员）。
   3. 依次选择“**删除所有成员用户**”、“**删除所有成员组**”复选框
   4. 添加 PAW 维护 (pawmaint) 和管理员（同样不要使用浏览按钮选择管理员）。

      > [!NOTE]
      > 不要将 PAW 用户组添加到本地管理员组成员资格列表。  要确保 PAW 用户不能无意或故意修改 PAW 自身的安全设置，它们不应是本地管理员组的成员。
      >
      > 有关使用组策略首选项来修改组成员身份的详细信息，请参阅 TechNet 文章[配置本地组项目](https://technet.microsoft.com/library/cc732525.aspx)。

2. **限制本地组成员身份** - 此设置可确保工作站上本地管理员组的成员身份始终为空
   1. 转到“计算机配置\首选项\控制面板设置\本地用户和组”，然后执行以下步骤：
      1. 单击“**新建**”，然后单击“**本地组**”
      2. 选择“**更新**”操作，然后选择“Backup Operators (内置)”（不要使用浏览按钮选择域组 Backup Operators）。
      3. 依次选择“**删除所有成员用户**”、“**删除所有成员组**”复选框。
      4. 不要向组添加任何成员。  只需单击“**确定**”。  通过分配空列表，组策略将自动移除所有成员，并确保在每次刷新组策略时有一个空成员资格列表。
   2. 为以下其他组完成上面的步骤：
      * Cryptographic Operators
      * Hyper-V 管理员
      * Network Configuration Operators
      * Power Users
      * Remote Desktop Users
      * 复制器
   3. **PAW 登录限制**-此设置将限制可登录到 PAW 的帐户。 按照以下步骤可配置此设置：
      1. 转到“计算机配置\策略\Windows 设置\安全设置\本地策略\用户权限分配\允许本地登录”。
      2. 选择 "定义这些策略设置" 并添加 "PAW 用户" 和管理员（同样，不要使用 "浏览" 按钮选择管理员）。
   4. **阻止入站网络流量** - 此设置可确保任何未经请求的入站网络流量都无法到达 PAW。 按照以下步骤可配置此设置：
      1. 转到“计算机配置\策略\Windows 设置\安全设置\具有高级安全性的 Windows 防火墙\具有高级安全性的 Windows 防火墙”，然后按照以下步骤操作：
         1. 右键单击“具有高级安全性的 Windows 防火墙”，然后选择“**导入策略**”。
         2. 单击“**是**”接受此设置将替代任何现有的防火墙策略。
         3. 浏览到 PAWFirewall.wfw，然后选择“**打开**”。
         4. 单击 **“确定”** 。

            > [!NOTE]
            > 此时可以添加必须通过未经请求的流量连接 PAW 的地址或子网（例如安全扫描或管理软件）。
            > WFW 文件中的设置将为所有防火墙配置文件启用防火墙“阻止 - 默认值”模式、关闭规则合并启用记录被丢弃的和成功的数据包。 这些设置将阻止未经请求的流量，同时仍允许在从 PAW 启动的连接上进行双向通信，并阻止具有本地管理访问权限的用户创建会替代 GPO 设置的本地防火墙规则，并确保记录传入和传出 PAW 的流量。
            > **打开此防火墙将扩大 PAW 的攻击面，并增加安全风险。添加任何地址前，请参阅本指南中的“管理和操作 PAW”部分**。

   5. **为 WSUS 配置 Windows 更新** - 按照以下步骤更改设置，为 PAW 配置 Windows 更新：
      1. 请访问计算机 Configuration\Policies\Administrative \Windows 组件 \Windows 更新，并按照以下步骤操作：
         1. 启用“**配置自动更新策略**”。
         2. 选择选项“**4 - 自动下载并计划安装**”。
         3. 将选项“**计划安装日期**”更改为“**0 - 每天**”，并在你的组织首选项中，更改“**计划安装时间**”选项。
         4. 启用选项 "**指定 Intranet Microsoft 更新服务位置**策略"，并在两个选项中指定 ESAE WSUS 服务器的 URL。
   6. 按如下所示链接 "PAW 配置-计算机" GPO：

         |策略|链接位置|
         |-----|---------|
         |PAW 配置 - 计算机 |Admin\Tier 0\Devices|

#### <a name="create-paw-configuration---user-group-policy-object-gpo"></a>创建 "PAW 配置-用户" 组策略对象（GPO）

在本部分中，将创建一个新的 "PAW 配置-用户" GPO，该 GPO 为这些 Paw 提供特定保护，并链接到第0层帐户 OU （第0层中的 "帐户"）。

   > [!NOTE]
   > 不将这些设置添加到默认域策略

1. **阻止 Internet 浏览** - 若要阻止疏忽性 Internet 浏览，该操作将设置环回地址 (127.0.0.1) 的代理地址。
   1. 中转到 User 首选项 \windows Settings\Registry。 右键单击 "注册表"，选择 "**新建** > **注册表项**"，然后配置下列设置：
      1. 操作：替换
      2. 配置单元：HKEY_CURRENT_USER
      3. 密钥路径：Software\Microsoft\Windows\CurrentVersion\Internet 设置
      4. 值名称：ProxyEnable

         > [!NOTE]
         > 不要选中值名称左侧的“默认值”框。

      5. 值类型：REG_DWORD
      6. 值数据：1
         1. 单击“通用”选项卡，然后选择“**当不再应用项目时删除此项目**”。
         2. 在“通用”选项卡上选择“**项目级别目标**”，然后单击“**目标**”。
         3. 单击“**新建项目**”，然后选择“**安全组**”。
         4. 选择“…”按钮并浏览到 PAW 用户组。
         5. 单击“**新建项目**”，然后选择“**安全组**”。
         6. 选择“…”按钮并浏览到“**云服务管理员**”组。
         7. 单击“**云服务管理员**”项，然后单击“**项目选项**”。
         8. 选择“**不是**”。
         9. 单击目标窗口上的“**确定**”。
      7. 单击“**确定**”即可完成 ProxyServer 组策略设置
   2. 中转到 User 首选项 \windows Settings\Registry。 右键单击 "注册表"，选择 "**新建** > **注册表项**"，然后配置下列设置：

      * 操作：替换
      * 配置单元：HKEY_CURRENT_USER
      * 密钥路径：Software\Microsoft\Windows\CurrentVersion\Internet 设置
         * 值名称：ProxyServer

            > [!NOTE]
            > 不要选中值名称左侧的“默认值”框。

         * 值类型：REG_SZ
         * 值数据：127.0.0.1：80
            1. 单击“**通用**”选项卡，然后选择“**当不再应用项目时删除此项目**”。
            2. 在“**通用**”选项卡上选择“**项目级别目标**”，然后单击“**目标**”。
            3. 单击“**新建项目**”，然后选择安全组。
            4. 选择“…”按钮并添加 PAW 用户组。
            5. 单击“**新建项目**”，然后选择安全组。
            6. 选择“…”按钮并浏览到“**云服务管理员**”组。
            7. 单击“**云服务管理员**”项，然后单击“**项目选项**”。
            8. 选择“**不是**”。
            9. 单击目标窗口上的“**确定**”。

   3. 单击“**确定**”即可完成 ProxyServer 组策略设置。
2. 请参阅 User Configuration\Policies\Administrative \Windows 组件 \Internet Explorer，并启用以下选项。 这些设置将会阻止管理员手动替代代理设置。
   1. 启用“**禁用更改自动配置**”设置。
   2. 启用“**阻止更改代理设置**”。

#### <a name="restrict-administrators-from-logging-onto-lower-tier-hosts"></a>限制管理员登录到较低层主机

在本部分中，我们将配置组策略以防止有特权的管理帐户登录较低层主机。

1. 创建新的**限制工作站登录** GPO - 此设置将限制第 0 层和第 1 层管理员帐户登录到标准工作站。  此 GPO 应链接到 "工作站" 顶级 OU，并具有以下设置：
   * 在 "计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 用户权限分配登录登录为批处理作业" 中，选择 "**定义这些策略设置**" 并添加第0层和第1层组：
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     DOMAIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > 内置第0层组，有关更多详细信息，请参阅第0层。

         Other Delegated Groups

     > [!NOTE]
     > 任何具有有效第0层访问权限的自定义创建的组，有关详细信息，请参阅第0层。

         Tier 1 Admins

     > [!NOTE]
     > 此组是之前在阶段1中创建的。

   * 在 "计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 用户权限分配登录登录为服务" 中，选择 "**定义这些策略设置**" 并添加第0层和第1层组：
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     DOMAIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > 注意:内置第0层组，有关更多详细信息，请参阅第0层。

         Other Delegated Groups

     > [!NOTE]
     > 注意:任何具有有效第0层访问权限的自定义创建的组，有关详细信息，请参阅第0层。

         Tier 1 Admins

     > [!NOTE]
     > 注意:此组是之前在阶段1中创建的

2. 创建新的**限制服务器登录**GPO-此设置将限制第0层管理员帐户登录到第1层服务器。  此 GPO 应链接到 "第1层服务器" 顶级 OU，并具有以下设置：
   * 在 "计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 用户权限分配登录登录为批处理作业" 中，选择 "**定义这些策略设置**" 并添加第0层组：
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     DOMAIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > 内置第0层组，有关更多详细信息，请参阅第0层。

         Other Delegated Groups

     > [!NOTE]
     > 任何具有有效第0层访问权限的自定义创建的组，有关详细信息，请参阅第0层。

   * 在 "计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 用户权限分配登录登录为服务" 中，选择 "**定义这些策略设置**" 并添加第0层组：
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     DOMAIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > 内置第0层组，有关更多详细信息，请参阅第0层。

         Other Delegated Groups

     > [!NOTE]
     > 任何具有有效第0层访问权限的自定义创建的组，有关详细信息，请参阅第0层。

   * 在 "计算机配置 \windows 设置 \ 安全设置 \ 本地策略 \ 用户权限分配登录本地登录" 中，选择 "**定义这些策略设置**" 并添加第0层组：
     ```
     Enterprise Admins
     Domain Admins
     Schema Admins
     DOMAIN\Administrators
     Account Operators
     Backup Operators
     Print Operators
     Server Operators
     Domain Controllers
     Read-Only Domain Controllers
     Group Policy Creators Owners
     Cryptographic Operators
     ```

     > [!NOTE]
     > 注意:内置第0层组，有关更多详细信息，请参阅第0层。

         Other Delegated Groups

     > [!NOTE]
     > 注意:任何具有有效第0层访问权限的自定义创建的组，有关详细信息，请参阅第0层。

#### <a name="deploy-your-paws"></a>部署 PAW

   > [!NOTE]
   > 确保在操作系统构建过程中，PAW 与网络断开连接。

1. 使用先前获取的清洁源安装介质安装 Windows 10。

   > [!NOTE]
   > 可以使用 Microsoft 部署工具包 (MDT) 或其他自动映像部署系统来自动部署 PAW，但必须确保构建过程与 PAW 一样可信。 按惯例，攻击者会明确寻找企业映像和部署系统（包括 ISO、部署程序包等），因此，不应使用已有的部署系统或映像。
   >
   > 如果自动部署 PAW，你必须：
   >
   > * 使用经[安装介质的清洁源](https://aka.ms/cleansource)验证的安装介质构建系统。
   > * 在操作系统构建过程中，确保自动部署系统与网络断开连接。

2. 为本地 Administrator 帐户设置唯一的复杂密码。  不要使用在相应环境中任何其他帐户已用过的密码。

   > [!NOTE]
   > Microsoft 建议使用[本地管理员密码解决方案 (LAPS)](https://www.microsoft.com/en-us/download/details.aspx?id=46899) 来管理所有工作站（包括 PAW）的本地管理员密码。  如果你使用 LAPS，请确保仅授权 PAW 维护组为 PAW 读取 LAPS 管理的密码。

3. 使用清洁源安装介质为 Windows 10 安装远程服务器管理工具。
4. 配置 Windows Defender 攻击防护

   > [!NOTE]
   > 要遵循的配置指南

5. 将 PAW 连接到网络。  确保 PAW 可以至少连接到一个域控制器 (DC)。
6. 使用 PAW 维护组成员的帐户，从新创建的 PAW 运行以下 PowerShell 命令，以将其加入到相应的 OU 中的域中：

   `Add-Computer -DomainName Fabrikam -OUPath "OU=Devices,OU=Tier 0,OU=Admin,DC=fabrikam,DC=com"`

   酌情将对 *Fabrikam* 的引用替换为你的域名。  如果你的域名扩展到多个级别（例如 child.fabrikam.com），使用“DC=”标识符以域的完全限定的域名中出现的顺序添加其他名称。

   > [!NOTE]
   > 如果你已部署了 [ESAE 管理林](https://aka.ms/esae)（适用于阶段 1 中的第 0 层管理员）或 [Microsoft 标识管理器 (MIM) 特权访问管理 (PAM)](https://aka.ms/mimpamdeploy)（适用于阶段 2 中的第 1 层和第 2 层管理员），你应把 PAW 联接到此处环境中的域，而不是生产域中。

7. 在安装任何其他软件（包括管理工具、代理等）前，应用所有关键且重要的 Windows 更新。
8. 强制安装组策略应用程序。
   1. 打开提升的命令提示符，然后输入以下命令： `Gpupdate /force /sync`
   2. 重启计算机

9. 可有可无为 Active Directory 管理员安装其他所需的工具。 安装执行作业职责所需的任何其他工具或脚本。 将凭据添加到 PAW 之前，确保使用任意工具评估凭据在目标计算机上泄露的风险。 访问[此页面](https://aka.ms/logontypes)获取评估凭据暴露风险的管理工具和连接方法的详细信息。 确保使用[安装介质的清洁源](https://aka.ms/cleansource)中的指南获取全部安装介质。

   > [!NOTE]
   > 即使不会作为安全边界，为这些工具在中心位置使用跳转服务器也可以降低复杂性。

10. 可有可无下载并安装所需的远程访问软件。 如果管理员远程使用 PAW 进行管理使用，则使用远程访问解决方案供应商所提供的安全指南安装远程访问软件。 确保使用“安装介质的清洁源”中的指南获取全部安装介质。

    > [!NOTE]
    > 仔细考虑允许通过 PAW 远程访问所涉及的所有风险。  虽然移动 PAW 可使用多种重要方案（包括在家办公），远程访问软件可能会容易受到攻击，并被用于入侵 PAW。

11. 通过使用以下步骤，查看并确认所有相应设置均已就绪的方式，验证 PAW 系统的完整性：
    1. 确认仅将 PAW 特定的组策略应用于 PAW
       1. 打开提升的命令提示符，然后输入以下命令： `Gpresult /scope computer /r`
       2. 查看生成的列表，并确保出现的唯一组策略是你在上述步骤中创建的。
    2. 使用以下步骤确认没有其他用户帐户是 PAW 上的特权组成员：
       1. 打开“**编辑本地用户和组**”(lusrmgr.msc)，选择“**组**”，并确认只有本地管理员组的成员是本地 Administrator 帐户和 PAW 维护全局安全组。

          > [!NOTE]
          > PAW 用户组不应该是本地管理员组的成员。  成员只能是本地 Administrator 帐户和 PAW 维护全局安全组（PAW 用户也不应是该全局组的成员）。

       2. 此外，使用“**编辑本地用户和组**”，确保下列组没有任何成员：备份操作员加密操作员 Hyper-v 管理员网络配置操作员超级用户远程桌面用户复制程序

12. 可有可无如果你的组织使用安全信息和事件管理（SIEM）解决方案，请确保将 PAW[配置为使用 Windows 事件转发（WEF）将事件转发到系统](http://blogs.technet.com/b/jepayne/archive/2015/11/24/monitoring-what-matters-windows-event-forwarding-for-everyone-even-if-you-already-have-a-siem.aspx)，或将其注册到解决方案，以便 SIEM主动接收来自 PAW 的事件和信息。  此操作的详细信息将因 SIEM 解决方案不同而有所不同。

    > [!NOTE]
    > 如果 SIEM 需要作为系统运行的代理或 PAW 上的本地管理帐户，请确保通过与域控制器和标识系统同一级别的信任来管理 SIEM。

13. 可有可无如果你选择部署 LAPS 来管理 PAW 上的本地管理员帐户的密码，请验证是否已成功注册密码。

    * 使用具有读取 LAPS 管理的密码权限的帐户，打开“**Active Directory 用户和计算机**”(dsa.msc)。  确保已启用“高级功能”，然后右键单击相应的计算机对象。  选择“属性编辑器”选项卡，确认 msSVSadmPwd 的值使用有效密码填充。

### <a name="phase-2-extend-paw-to-all-administrators"></a>阶段 2:将 PAW 扩展至所有管理员

内对任务关键型应用程序和依赖项具有管理权限的所有用户。  这至少应包含应用程序服务器、运行状况和安全监视解决方案、虚拟化解决方案、存储系统及网络设备的管理员。

> [!NOTE]
> 本阶段中的说明假定阶段 1 步骤已全部执行。  在完成阶段1中的所有步骤之前，请勿开始阶段2。

确认已完成所有步骤后，请执行以下步骤来完成阶段 2:

#### <a name="recommended-enable-restrictedadmin-mode"></a>您启用**RestrictedAdmin**模式

在现有服务器和工作站上启用此功能，然后强制使用此功能。 此功能将要求目标服务器运行 Windows Server 2008 R2 或更高版本，并且目标工作站运行 Windows 7 或更高版本。

1. 按照[此页面](https://aka.ms/rdpra)中的说明，在服务器和工作站上启用 **RestrictedAdmin** 模式。

   > [!NOTE]
   > 在为面向 Internet 的服务器启用此功能前，你应考虑攻击者可利用以前被盗的密码哈希通过这些服务器进行身份验证的风险。

2. 创建“所需的 RestrictedAdmin - 计算机”组策略对象 (GPO)。 本部分将创建一个 GPO，为传出远程桌面连接强制使用 /RestrictedAdmin 转换，使目标系统上的帐户免受凭据被盗的风险
   * 转到“计算机配置\策略\管理模板\系统\凭据委派\限制向远程服务器委派凭据”，然后设置为“**已启用**”。
3. 使用以下策略选项，将所需的 **RestrictedAdmin** 计算机链接到相应的第 1 层和/或第 2 层设备：
   * PAW 配置 - 计算机
      * -> 链接位置：Admin\Tier 0\Devices（现有）
   * PAW 配置 - 用户
      * -> 链接位置：Admin\Tier 0 帐户
   * 所需的 RestrictedAdmin - 计算机
      * -> Admin\Tier1\Devices 或-> Admin\Tier2\Devices （两者都可选）

   > [!NOTE]
   > 这不是第 0 层系统所必需的，因为这些系统已可对在环境中的所有资产进行完全控制。

#### <a name="move-tier-1-objects-to-the-appropriate-ous"></a>将第1层对象移到适当的 Ou

1. 将第 1 层组移动到 Admin\Tier 1\Groups OU。 找到授予以下管理权限的所有组，然后将其移动到此 OU。
   * 多个服务器上的本地管理员
      * 对云服务的管理访问权限
      * 对企业应用程序的管理访问权限
2. 将第 1 层帐户移动到 Admin\Tier 1\Accounts OU。 将是第 1 层组成员（包括嵌套的成员身份）的全部帐户移动到此 OU。
3. 将相应成员添加到相关组
   * **第 1 层管理员** - 此组将包括被限制不能登录第 2 层主机的第 1 层管理员。 添加对服务器或 internet 服务具有管理特权的所有第1层管理组。

      > [!NOTE]
      > 如果管理人员有责任管理多个层上的资产，则需要在每层创建独立的管理员帐户。

4. 启用凭据保护功能以减少凭据被盗和重复使用的风险。  凭据保护是 Windows 10 的一项新功能，可限制应用程序访问凭据，阻止凭据被盗攻击（包括哈希传递）。  凭据保护对最终用户是完全透明的，需要的设置时间和精力最少。  有关凭据保护（包括部署步骤及硬件要求）的详细信息，请参阅文章[使用凭据保护来保护域凭据](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)。

   > [!NOTE]
   > 必须启用设备保护，才能配置和使用凭据保护功能。  但是，无需配置任何其他设备保护功能即可使用凭据保护功能。

5. 可有可无启用与云服务的连接。 此步骤允许管理具有相应安全保证的云服务（例如 Azure 和 Office 365）。 此步骤还要求 Microsoft Intune 管理 PAW。

   > [!NOTE]
   > 如果不需要云连接来管理云服务或不通过 Intune 进行管理，请跳过此步骤。

   * 这些步骤将限制仅通过已授权的云服务（但不是开放的 Internet）进行 Internet 通信，并向处理 Internet 内容的浏览器和其他应用程序添加保护功能。 这些用于管理的 PAW 永远不会用于执行标准用户任务，例如 Internet 通信和工作效率。
   * 要启用到 PAW 服务的连接，请执行以下步骤：

   1. 配置 PAW 以仅允许已授权的 Internet 目标。  扩展 PAW 部署以启用云管理时，从攻击者可以更轻松地攻击管理员的开放 Internet 筛选访问权限的同时，需要允许访问已授权的服务。

      1. 创建 "**云服务管理员**" 组，并向其添加需要访问 internet 上的云服务的所有帐户。
      2. 从 [TechNet 库](https://aka.ms/pawmedia)下载 PAW *proxy.pac* 文件，并发布在内部网站上。

         > [!NOTE]
         > 下载后需要更新 *proxy.pac* 文件，以确保文件为最新且完整。  
         > Microsoft 在 Office [支持中心](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US)发布所有最新的 Office 365 和 Azure URL。 这些说明假定你将使用 Internet Explorer（或 Microsoft Edge）管理 Office 365、Azure 和其他云服务。 Microsoft 建议为需要用于管理的任何第三方浏览器配置类似的限制。 PAW 上的 Web 浏览器仅用于管理云服务并且永远不用于常规的 Web 浏览。
         >
         > 你可能需要为其他 IaaS 提供商将其他有效的 Internet 目标添加到此列表，但不要将工作效率、娱乐、新闻或搜索站点添加到此列表。
         >
         > 你可能还需要调整 PAC 文件，以便使有效的代理地址可以使用这些地址。
         >
         > 你还可以使用 Web 代理限制来自 PAW 的访问，同时获得深度防护。 我们不建议在没有 PAC 文件的情况下自动使用此功能，因为该功能在连接到企业网络的情况下，仅限制 PAW 的访问权限。

      3. 配置 *proxy.pac* 文件后，更新 PAW 配置 - 用户 GPO。
         1. 中转到 User 首选项 \windows Settings\Registry。 右键单击 "注册表"，选择 "**新建** > **注册表项**"，然后配置下列设置：
            1. 操作：替换
            2. 配置单元：HKEY_ CURRENT_USER
            3. 密钥路径：Software\Microsoft\Windows\CurrentVersion\Internet 设置
            4. 值名称：AutoConfigUrl

               > [!NOTE]
               > 不要选择值名称左侧的“**默认值**”框。

            5. 值类型：REG_SZ
            6. 值数据：输入*代理 pac*文件的完整 URL，包括 http://和文件名（例如 http://proxy.fabrikam.com/proxy.pac ）。  URL 还可以是单标签 URL，例如 http://proxy/proxy.pac

               > [!NOTE]
               > PAC 文件还可托管于文件共享上，语法为 file://server.fabrikan.com/share/proxy.pac，但这需要允许 file:// 协议。 请参阅 "备注：本[了解 Web 代理配置](http://blogs.msdn.com/b/ieinternals/archive/2013/10/11/web-proxy-configuration-and-ie11-changes.aspx)博客的 File://-based Proxy 脚本 "部分，详细了解如何配置所需的注册表值。

            7. 单击“**通用**”选项卡，然后选择“**当不再应用项目时删除此项目**”。
            8. 在“**通用**”选项卡上选择“**项目级别目标**”，然后单击“**目标**”。
            9. 单击“**新建项目**”，然后选择“**安全组**”。
            10. 选择“…”按钮并浏览到“**云服务管理员**”组。
            11. 单击“**新建项目**”，然后选择“**安全组**”。
            12. 选择“...”按钮，然后浏览到“**PAW 用户**”组。
            13. 单击“**PAW 用户**”项，然后单击“**项目选项**”。
            14. 选择“**不是**”。
            15. 单击目标窗口上的“**确定**”。
            16. 单击“**确定**”以完成 **AutoConfigUrl** 组策略设置。
   2. 按照以下步骤，分别将 Windows 和云服务访问（如果需要）的 Windows 10 安全基线和云服务访问链接安全基线应用至 OU：
      1. 解压缩 Windows 10 安全基线 ZIP 文件的内容。
      2. 创建这些 GPO，[导入策略](https://technet.microsoft.com/library/cc753786.aspx)设置并按此表进行[链接](https://technet.microsoft.com/library/cc732979.aspx)。 将每个策略链接到每个位置，确保遵循此表中的顺序（表中较为靠后的条目应稍后应用且享有更高的优先级）：

         **政策**

         |||
         |-|-|
         |CM Windows 10 - 域安全性|N/A - 现在不链接|
         |SCM Windows 10 TH2 - 计算机|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Windows 10 TH2 - BitLocker|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Windows 10 - 凭据保护|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Internet Explorer - 计算机|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |PAW 配置 - 计算机|Admin\Tier 0\Devices（现有）|
         ||Admin\Tier 1\Devices（新链接）|
         ||Admin\Tier 2\Devices（新链接）|
         |所需的 RestrictedAdmin - 计算机|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Windows 10 - 用户|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |SCM Internet Explorer - 用户|Admin\Tier 0\Devices|
         ||Admin\Tier 1\Devices|
         ||Admin\Tier 2\Devices|
         |PAW 配置 - 用户|Admin\Tier 0\Devices（现有）|
         ||Admin\Tier 1\Devices（新链接）|
         ||Admin\Tier 2\Devices（新链接）|

         > [!NOTE]
         > “SCM Windows 10 - 域安全”GPO 可能链接到独立于 PAW 的域，但会影响整个域。

6. 可有可无为第1层管理员安装其他所需的工具。 安装执行作业职责所需的任何其他工具或脚本。 将凭据添加到 PAW 之前，确保使用任意工具评估凭据在目标计算机上泄露的风险。 有关评估凭据暴露风险的管理工具和连接方法的详细信息，请访问[此页面](https://aka.ms/logontypes)。 确保使用“安装介质的清洁源”中的指南获取全部安装介质
7. 标识并安全地获取管理所需的软件和应用程序。  这与阶段 1 执行的工作类似，但由于要保护的应用程序、服务和系统数量的增加，因而范围更广。

   > [!NOTE]
   > 请确保通过将这些新应用程序（包括 web 浏览器）加入到 Windows Defender 攻击防护提供的保护中来保护这些新应用程序。

   其他软件和应用程序示例包括：

      * Microsoft Azure PowerShell
      * Office 365 PowerShell（也称为 Microsoft Online Services 模块）
      * 基于 Microsoft 管理控制台的应用程序或服务管理软件
      * 专有（非基于 MMC）的应用程序或服务管理软件

         > [!NOTE]
         > 目前，许多应用程序通过 Web 浏览器全权托管，其中包括多项云服务。  这种做法在减少需要在 PAW 上安装的应用程序数量的同时，也引入了浏览器互操作性问题的风险。  可能需要将非 Microsoft Web 浏览器部署到特定 PAW 实例，以启用特定服务的管理。  如果部署了其他 Web 浏览器，请确保遵循所有清洁源原则，并按照供应商安全指南保护浏览器。

8. 可有可无下载并安装任何所需的管理代理。

   > [!NOTE]
   > 如果选择安装其他管理代理（监控、安全、配置管理等），确保管理系统与域控制器、标识系统的受信任级别一致变得至关重要。 有关其他指南，请参阅“管理和更新 PAW”。

9. 评估基础结构，以识别需要 PAW 提供的其他安全保护功能的系统。  确保明确了解哪些系统必须受到保护。  提出有关资源本身的关键问题，例如：

   * 必须进行管理的目标系统在哪里？  它们是被收集到一个物理位置还是连接到单个明确定义的子网？
   * 有多少个系统？
   * 这些系统是否依赖于其他系统（虚拟化、存储等）？如果是这样，如何对这些系统进行管理？  暴露于这些依赖关系的关键系统如何？与这些依赖关系相关的其他风险是什么？
   * 所管理服务的重要性如何？如果这些服务被入侵，预期损失是什么？

      > [!NOTE]
      > 在此评估中包括云服务，攻击者越来越多地针对不安全的云部署，因此，将这些服务按照本地关键任务应用程序的安全保护级别进行管理很重要。

        使用此评估方法识别需要其他保护功能的特定系统，然后将 PAW 程序扩展至这些系统的管理员。  大大受益于基于 PAW 管理的系统的常见示例包括 SQL Server（本地和 SQL Azure）、人力资源应用程序及财务软件。

        > [!NOTE]
        > 如果资源可通过 Windows 系统管理，那么也可以使用 PAW 管理，即使应用程序本身运行在 Windows 之外的操作系统或非 Microsoft 云平台上也是如此。  例如，Amazon Web 服务订阅的所有者应仅使用 PAW 管理该帐户。

10. 制定在组织中大规模部署 PAW 的请求和分发方法。  视阶段 2 中所选择要部署的 PAW 数量而定，可能需要自动执行此流程。

    * 考虑制定一个管理员用于获取 PAW 的正式请求和审批流程。  此流程有助于标准化部署过程，明确 PAW 设备责任，并帮助确定 PAW 部署过程中的差距。
    * 如前面所述，此部署解决方案应该与现有的自动化方法（可能已被入侵）分开，并且应遵循阶段 1 中列出的原则。

        > [!NOTE]
        > 管理资源的任何系统自身托管的级别应与信任级别相同或更高。

11. 进行检查并根据需要部署其他 PAW 硬件配置文件。  为阶段 1 部署所选择的硬件配置文件可能不适合所有管理员。  查看硬件配置文件，并根据需要选择其他 PAW 硬件配置文件来满足管理员需求。  例如，专用硬件配置文件（独立的 PAW 和日常使用工作站）可能并不适合经常出差的管理员，在这种情况下，可以选择为该管理员部署“同时使用”配置文件（带有用户虚拟机的 PAW）。
12. 考虑伴有扩展 PAW 部署的文化、操作、通信和培训需求。   管理模型的这种重大改变自然要求一定程度的管理改变，将这种改变构建到部署项目自身中很有必要。  至少考虑下列问题：

    * 通过何种方式将这些改变传达给高层领导，以确保获得他们的支持？  没有高层领导支持，任何项目都很有可能失败，或至少要努力筹措资金并获得广泛接受。
    * 如何为管理员记录新流程？  这些改变必须记录，并且不仅要传达给现有管理员（他们必须改变自己的习惯并以不同方式管理资源），也要传达给新管理员（内部提升或从外部组织聘用的管理员）。  文档必须能清晰完整地阐述威胁的重要性、PAW 保护管理员的角色及如何正确使用 PAW。

      > [!NOTE]
      > 这对于离职率较高的角色尤为重要，包括但不限于支持人员。

    * 如何确保遵循新流程？  尽管 PAW 模型包含多个技术控件来防止泄露特权凭据，但无法完全使用技术控件完全阻止所有可能的公开。  例如，虽然可以阻止管理员使用特权凭据成功登录到用户桌面，但简单的尝试登录行为会将凭据暴露给安装在相应用户桌面上的恶意软件。  因此，明确阐述 PAW 模型优势及不遵守新流程的风险非常重要。  这可以通过审核和警报实现，这样就可以快速检测到凭据泄露并予以解决。

### <a name="phase-3-extend-and-enhance-protection"></a>阶段 3:扩展和增强保护

内这些保护增强了在阶段1中构建的系统，增强了具有高级功能（包括多重身份验证和网络访问规则）的基本保护。

> [!NOTE]
> 在阶段 1 的步骤完成后，可随时执行此阶段步骤。  此阶段不受阶段 2 是否完成的影响，因而可在阶段 2 之前或之后执行，也可与阶段 2 同时执行。

按照以下步骤配置此阶段：

1. **为特权帐户启用多重身份验证**。  多重身份验证要求用户除凭据外，还提供物理令牌，因而可增强帐户安全性。  多重身份验证极好地补充了身份验证策略，但部署时不依赖于身份验证策略（类似地，身份验证策略也不需要多重身份验证）。  Microsoft 建议使用其中一种多重身份验证形式：

   * **智能卡**：智能卡是防篡改的便携式物理设备，在 Windows 登录过程中提供第二次验证。  通过要求每个人都拥有登录卡，可以减少被盗凭据被远程重复使用的风险。  有关 Windows 智能卡登录的详细信息，请参阅文章[智能卡概述](https://technet.microsoft.com/library/hh831433.aspx)。
   * **虚拟智能卡**：虚拟智能卡与物理智能卡具有相同的安全性优势，并且具有关联到特定硬件的优势。  有关部署和硬件要求的详细信息，请参阅文章1Get 虚拟智能[卡概述](https://technet.microsoft.com/library/dn593708.aspx)@no__t 和虚拟智能卡入门：演练指南 @ no__t。
   * **Windows Hello 企业版**:Windows Hello 企业版使用户能够对 Microsoft 帐户、Active Directory 帐户、Microsoft Azure Active Directory （Azure AD）帐户或支持 Fast ID Online （FIDO）身份验证的非 Microsoft 服务进行身份验证。 在 Windows Hello 企业版注册期间进行初始双重验证后，将在用户的设备上设置 Windows Hello 企业版，并且用户可以设置笔势（可以是 Windows Hello 或 PIN）。 Windows Hello 企业版凭据是非对称密钥对，可在受信任的平台模块（Tpm）的隔离环境中生成。
      有关 Windows Hello 企业版的详细信息，请参阅[Windows Hello 企业版](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification)。
   * **Azure 多重身份验证**：Azure 多重身份验证（MFA）通过监视和基于机器学习的分析为第二个验证因素和增强的保护提供安全性。  Azure MFA 不仅可以保护 Azure 管理员，还可以保护很多其他解决方案，包括 Web 应用程序、Azure Active Directory 和本地解决方案（例如远程访问和远程桌面）。  有关 Azure 多重身份验证的详细信息，请参阅文章[多重身份验证](https://azure.microsoft.com/services/multi-factor-authentication)。

2. **使用 Windows Defender 应用程序控制和/或 AppLocker 将受信任的应用程序列入白名单**。  通过限制不受信任或未签署的代码在 PAW 上运行的功能，可以进一步降低恶意活动和入侵的可能性。  针对应用程序控制，Windows 包括两种主要选项：

   * **AppLocker**：AppLocker 可帮助管理员控制哪些应用程序可以在给定的系统上运行。  AppLocker 可通过组策略进行集中控制，也可应用到特定的用户或组（针对 PAW 用户的目标应用程序）。  有关 AppLocker 的详细信息，请参阅 TechNet 文章 [AppLocker 概述](https://technet.microsoft.com/library/hh831440.aspx)。
   * **Windows Defender 应用程序控制**：新的 "Windows Defender 应用程序控制" 功能提供了基于硬件的增强应用程序控制，不能在受影响的设备上覆盖与 AppLocker 不同的应用程序控制。  与 AppLocker 类似，Windows Defender 应用程序控制可以通过组策略进行控制，并以特定用户为目标。  有关使用 Windows Defender 应用程序控制限制应用程序使用情况的详细信息，请参阅[Windows Defender 应用程序控制部署指南](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)。

3. **使用受保护用户、身份验证策略和身份验证接收器来进一步保护特权帐户**。  受保护用户的成员受制于其他安全策略，这些策略可保护存储在本地安全代理 (LSA) 中的凭据，并可大大降低凭据被盗和重复使用的风险。  身份验证策略和接收器控制特权用户如何访问域中的资源。  总体来说，这些保护功能极大地增强了特权用户的帐户安全性。  有关这些功能的其他详细信息，请参阅 Web 文章[如何配置受保护的帐户](https://technet.microsoft.com/library/dn518179.aspx)。

   > [!NOTE]
   > 这些保护措施是为了补充而不是替代阶段 1 中的现有安全措施。  管理员应仍使用独立帐户进行管理和常规使用。

## <a name="managing-and-updating"></a>管理和更新

PAW 必须具有反恶意软件功能，且软件更新必须得到快速应用，以便维护这些工作站的完整性。

其他配置管理、操作监控和安全管理也可以用于 PAW，但这些功能的集成必须经过慎重考虑，因为引入每个管理功能的同时，也会通过相应工具给 PAW 带来入侵风险。 引入高级管理功能是否有意义取决于多个因素，包括：

* 管理功能的安全状态与做法（包括工具的软件更新做法、管理角色及这些角色中的帐户、工具托管或受管于的操作系统及相应工具的任何其他硬件或软件相关项）
* PAW 上软件部署和更新的频率与数量
* 详细清单和配置信息要求
* 安全监控要求
* 组织标准及其他组织特定因素

根据清洁源原则，用于管理或监控 PAW 的所有工具的受信任等级必须与 PAW 的等级相同或在其之上。 这通常要求这些工具通过 PAW 进行管理，以确保没有来自较低特权工作站的安全依赖关系。

此表列出了可能用于管理和监控 PAW 的不同方法：

|方法|注意事项|
|------|---------|
|PAW 中的默认值<br /><br />- Windows Server Update Services<br />- Windows Defender|- 无需额外付费<br />- 执行所需的基本安全功能<br />- 本指南中的说明|
|使用 [Intune](https://technet.microsoft.com/library/jj676587.aspx) 进行管理|<ul><li>提供了基于云的可见性和控制<br /><br /><ul><li>软件部署</li><li>管理软件更新</li><li>Windows 防火墙策略管理</li><li>反恶意软件保护</li><li>远程协助</li><li>软件许可证管理。</li></ul></li><li>不需要服务器基础结构</li><li>需要执行阶段 2 中的“启用云服务连接”步骤</li><li>如果 PAW 计算机未加入域，这就要求使用安全基线下载中提供的工具，将 SCM 基线应用于逻辑映像。</li></ul>|
|用于管理 PAW 的新 System Center 实例|- 提供配置、软件部署和安全更新的可见性和控制<br />- 需要单独的服务器基础结构，以保护其达到 PAW 级别，并为高权限员工配备技术|
|使用现有管理工具管理 PAW|-如果将现有的管理基础结构启动**到 paw 的**安全级别，则会产生严重的风险，paw   Microsoft 通常会反对此方法，除非你的组织有特定的理由来使用它。 在我们的经验中，将所有这些工具（及其安全性依赖项）引入到 Paw 的安全级别通常会产生极高的成本。<br />- 其中大部分工具提供配置、软件部署和安全更新的可见性和控制|
|安全扫描或监控工具需要管理员访问权限|包括安装代理或要求具有本地管理访问权限的帐户的所有工具。<br /><br />- 要求将工具安全保证提升至 PAW 的级别。<br />- 可能需要下调 PAW 的安全态势来支持工具功能（打开端口、安装 Java 或其他中间件等），以制定安全权衡决策|
|安全信息和事件管理 (SIEM)|<ul><li>如果 SIEM 无代理<br /><br /><ul><li>使用 **Event Log Readers** 组中的帐户，无需管理权限即可访问 PAW 上的事件</li><li>将要求打开网络端口，以便允许来自 SIEM 服务器的入站流量</li></ul></li><li>如果 SIEM 需要代理，请参阅其他行**安全扫描或控视工具需要管理员访问权限**。</li></ul>|
|Windows 事件转发|- 提供一种将来自 PAW 的安全事件转发到外部收集器或 SIEM 的无代理方法<br />-无需管理权限即可访问 Paw 上的事件<br />- 不要求打开网络端口，即可允许来自 SIEM 服务器的入站流量|

## <a name="operating-paws"></a>操作 PAW

PAW 解决方案应使用基于清洁源原则的[操作标准](https://aka.ms/securitystandards)中的标准进行操作。

## <a name="deploy-paws-using-a-guarded-fabric"></a>使用受保护的构造部署 Paw

[受保护的构造](https://aka.ms/shieldedvms)可用于在便携式计算机或跳转服务器上的受防护的虚拟机中运行 PAW 工作负荷。
采用此方法需要额外的基础结构和操作步骤，但可以更轻松地按固定时间间隔重新部署 PAW 映像，并使你能够将多个不同的分层（或分类） Paw 整合到运行的虚拟机单个设备上的并行。
有关受保护的构造拓扑和安全承诺的完整说明，请参阅[受保护的构造文档](https://aka.ms/shieldedvms)。

### <a name="changes-to-the-paw-gpos"></a>对 PAW Gpo 的更改

使用基于防护 VM 的 Paw 时，需要修改上面定义的[建议 GPO 设置](#create-paw-configuration---computer-group-policy-object-gpo)以支持虚拟机的使用。

1. 为物理 PAW 主机创建新的 OU。 物理和虚拟 Paw 具有不同的安全要求，应 Active Directory 相应地分隔。
2. PAW 计算机 GPO 应链接到物理和虚拟 PAW Ou。
3. 为物理 PAW 创建新的 GPO，以将 PAW 用户添加到 Hyper-v 管理员组。 这是允许管理员连接到管理 Vm，并根据需要将其打开/关闭的必要条件。 登录到物理 PAW 的用户不具有管理员权限、对 internet 的访问权限，或者从网络共享或外部存储设备将恶意虚拟机数据复制到物理 PAW 的功能，这一点很重要。
4. 为管理员 Vm 创建一个新的 GPO，以将 PAW 用户添加到 "远程桌面用户" 组。 这将允许用户使用 Hyper-v 增强的控制台会话，从而提供更好的用户体验，并使智能卡能够通过 VM。

### <a name="set-up-the-host-guardian-service"></a>设置主机保护者服务

主机保护者服务负责证明物理 PAW 设备的标识和运行状况。
仅允许使用 HGS 并运行受信任的[代码完整性策略](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control)的计算机启动受防护的 vm。
这有助于保护受防护的 Vm，这些 Vm 运行受信任的工作负荷来管理你的分层资源，从用户桌面环境的威胁。

由于 HGS 负责确定哪些设备可以运行 PAW Vm，因此被视为第0层资源。
它应与其他第0层资源一起部署，并防止未经授权的物理和逻辑访问。
HGS 是一种群集角色，可以方便地向外扩展任意规模的部署。
一般规则是为每个具有的1000设备（至少3个节点）计划1个 HGS 服务器。

1. 若要安装第一个 HGS 服务器，请从 "[安装 hgs-堡垒林" 一](../../security/guarded-fabric-shielded-vm/guarded-fabric-install-hgs-in-a-bastion-forest.md)文开始，然后将 HGS 加入第0层域。

2. 然后，使用企业证书颁发机构[为 HGS 创建证书](../../security/guarded-fabric-shielded-vm/guarded-fabric-obtain-certs.md)。
拥有 HGS 加密和签名证书的任何人都可以解密受防护的 VM，因此，如果你有权访问硬件安全模块来保护私钥，则建议使用 HSM 生成这些证书。
为提高安全性，请选择大于或等于4096位的密钥大小。

3. 最后，按照以下步骤在**TPM 模式下**[初始化 HGS 服务器](../../security/guarded-fabric-shielded-vm/guarded-fabric-initialize-hgs-tpm-mode-bastion.md)。
初始化会设置 Paw 使用的证明和密钥保护 web 服务。
应将 HGS[配置为使用 TLS 证书](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-https.md)来保护这些通信，并且应仅从不受信任的网络向 hgs 打开端口443。

4. 按照这些步骤，为第二个、第三个和其他 HGS 节点[添加其他节点](../../security/guarded-fabric-shielded-vm/guarded-fabric-configure-additional-hgs-nodes.md)。

5. 如果你的 HGS 服务器正在运行 Windows Server 2019 或更高版本，则可以启用一个可选功能来缓存 Paw 上受防护的 Vm 的密钥，以便可以脱机使用这些密钥。 这些密钥会密封到系统的当前安全配置，以防止用户在另一台计算机上使用缓存的密钥，或者在同一台计算机上使用不安全状态。 如果你的 PAW 用户不访问 Internet，但仍需要能够登录到其 PAW Vm，则这可能是一个有用的解决方案。 若要使用此功能，请在任何 HGS 服务器上运行以下命令：

      ```powershell
      Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
      ```

### <a name="set-up-the-physical-paw-device"></a>设置物理 PAW 设备

默认情况下，在受保护的构造解决方案中将物理 PAW 设备视为不受信任。
它可以在证明过程中证明它是可信的，之后它可以获取启动受防护的管理 VM 所需的密钥。
设备必须能够运行 Hyper-v 并启用安全启动和启用 TPM 2.0，以满足[受保护的主机的先决条件](../../security/guarded-fabric-shielded-vm/guarded-fabric-guarded-host-prerequisites.md)。
支持所有 PAW 功能的最低操作系统版本为**Windows 10 版本 1803**。

物理 PAW 应设置为与其他任何设置相同的设置，但例外，任何 PAW 用户都需要是 Hyper-v 管理员才能打开管理 VM 并连接到它。
在您的全新会议室环境中，您将需要为要部署为管理 Vm 的受保护主机的每个独特硬件/软件组合创建一个黄金配置。
在每个黄金配置上完成以下任务：

1. 在计算机上安装 Windows、驱动程序和固件的最新更新，以及任何第三方管理或监视代理。
2. [捕获所需的基线信息](../../security/guarded-fabric-shielded-vm/guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)，包括唯一的 TPM 标识符（认可密钥）、启动度量值（TCG 日志）以及计算机的代码完整性策略。
3. 将这些项目复制到 HGS 服务器，并运行上一篇文章中的 HGS 证明命令来注册该主机。 如果所有主机都使用相同的代码完整性策略和/或使用相同的硬件配置，则只需注册代码完整性策略/TCG 日志一次。

### <a name="create-the-signed-template-disk"></a>创建已签名模板磁盘

受防护的 Vm 是使用已签名模板磁盘创建的。
在部署时，将验证该签名，以便在将机密（如管理员密码）发布到 VM 之前验证磁盘的完整性和真实性。

若要创建已签名的模板磁盘，请按照常规第2代虚拟机上的阶段1部署步骤进行操作。
此计算机将成为管理员 VM 的黄金映像。
可以创建多个模板磁盘，使专用工具在不同的上下文中可用。

根据需要配置 VM 时，请运行 `C:\Windows\System32\sysprep\sysprep.exe` 并选择**通用化**磁盘。 通用化完成**后关闭操作系统**。

最后，在来自 VM 的 VHDX 文件上运行[模板磁盘向导](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-template.md)，以安装 BitLocker 组件并生成磁盘签名。

#### <a name="create-the-shielding-data-file"></a>创建防护数据文件

通用化模板磁盘与防护数据文件配对，其中包含预配受防护的 VM 所需的机密。
防护数据文件包括：
   - 监护人列表，其中定义了允许 VM 运行的结构。 每个 HGS 群集都是它所保护的 PAW 设备的保护者。
   - 受信任的部署的磁盘签名列表。 屏蔽数据文件只会将其机密发布到使用授权源媒体创建的 Vm。
   - 一种安全策略，该策略规定是否应就地添加额外的保护来保护主机中的 VM，以及是否允许 VM 移到另一台计算机。 PAW 管理 Vm 应始终受到完全防护。
   - Unattend.xml 专用化文件，它允许 Windows 自动完成安装，并包含与本地管理员密码类似的机密。
   - 其他文件，如 RDP 或 VPN 证书。

有关如何创建防护数据文件的步骤，请参阅[防护数据文件一文](../../security/guarded-fabric-shielded-vm/guarded-fabric-tenant-creates-shielding-data.md)。

受防护的 Vm 的所有者密钥非常敏感，应保存在 HSM 中或存储在安全的位置。
它们可用于紧急中断玻璃方案，以在没有 HGS 的情况下启动受防护的 VM。

强烈建议 PAW 管理员 Vm 的防护数据包括将 VM 锁定到启动该虚拟机的第一个物理主机的设置。
这会阻止用户将管理员 VM 从一个 PAW 移到相同环境中的另一个 PAW。
若要使用此功能，请使用 PowerShell 创建防护数据文件，并包含**BindToHostTpm**参数：

```powershell
New-ShieldingDataFile -Policy Shielded -BindToHostTpm [...]
```

#### <a name="deploy-an-admin-vm"></a>部署管理员 VM

模板磁盘和防护数据文件准备就绪后，可以在注册到 HGS 的任何 PAW 上部署管理员 VM。

1. 将模板磁盘（.vhdx）和防护数据文件（pdk）复制到受信任的 PAW 设备。
2. 按照说明[使用 PowerShell 部署新的受防护的 VM](../../security/guarded-fabric-shielded-vm/guarded-fabric-create-a-shielded-vm-using-powershell.md)
3. 在部署过程的第1阶段完成剩余步骤，以确保 VM 操作系统的安全并根据需要对其进行配置。


## <a name="related-topics"></a>相关主题

[参与 Microsoft 网络安全服务](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)

@no__t-顶级0Taste：如何减少哈希传递和其他形式的凭据盗用 @ no__t-0

[Microsoft 高级威胁分析](https://aka.ms/ata)

[使用 Credential Guard 保护派生的域凭据](https://technet.microsoft.com/library/mt483740%28v=vs.85%29.aspx)

[Device Guard 概述](https://technet.microsoft.com/library/dn986865(v=vs.85).aspx)

[用安全管理工作站保护高价值资产](https://msdn.microsoft.com/library/mt186538.aspx)

[Windows 10 中的隔离用户模式 Dave Probert （第9频道）](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[Windows 10 中具有 Logan Gabriel （第9频道）的隔离用户模式进程和功能](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[有关 Dave Probert （第9频道）的 Windows 10 独立用户模式下的进程和功能的详细信息](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[使用 Windows 10 隔离用户模式（第9频道）减少凭据被盗](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[在 Windows Kerberos 中启用严格 KDC 验证](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[Windows Server 2012 的 Kerberos 身份验证的新增功能](https://technet.microsoft.com/library/hh831747.aspx)

[Windows Server 2008 R2 中 AD DS 的身份验证机制保证循序渐进指南](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[受信任的平台模块](C:/sd/docs/p_ent_keep_secure/p_ent_keep_secure/trusted_platform_module_technology_overview.xml)
