---
title: "加强的访问权限"
description: "Windows Server 安全"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5dec0c2-06fe-4c91-9bdc-67cc6a3ede60
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: eb83903204b00ef6c1eb116554ec54bc2211a399
ms.sourcegitcommit: 7b01b54032ec56432116626e08fbd92508c3a7d5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/09/2018
---
# <a name="securing-privileged-access"></a>加强的访问权限

>适用于：Windows Server 2016

保护特权访问是建立中现代组织的业务资产安全保证关键第一步。 大部分或全部业务资产某个组织中的安全取决于特权帐户管理和管理 IT 系统完整性。 这些帐户和其他元素授权的访问权限，以快速访问有针对性的数据和使用类似的凭据盗窃攻击的系统的目标充满网络攻击者[Pass--希代码以及 Pass 票证](https://www.microsoft.com/pth)。

保护针对管理访问确定对手需要你采取了完整和认真方法找到这些系统的风险。 该图显示的分隔和保护管理此的规划图中的三个阶段建议：

![建议的分隔和保护管理此的规划图中的三个阶段的图示](../media/securing-privileged-access/PAW_LP_Fig1.JPG)

路线图目标：

-   **2-4 每周计划**： 快速减少攻击最常使用的技巧

-   **计划 1-3 个月**： 版本查看和控制管理员活动

-   **6 + 个月的套餐**： 继续生成防御到更多前瞻性的安全状态

Microsoft 建议，请按照以下步骤来保护针对确定对手授权的访问权限。 你可以调整以下步骤来容纳你现有的性能以及在你的组织中的特定要求。

> [!NOTE]
> 保护特权访问这要求更广泛的元素包括 technical 组件 （主机防御、 帐户保护、 标识管理等），以及更改处理，并管理惯例和知识。

## <a name="why-is-securing-privileged-access-important"></a>为什么很重要保护授权的访问权限？
在大多数组织中，特权帐户管理和管理 IT 系统完整性取决于大部分或全部业务资产的安全。 如 Active Directory 来快速访问所有组织目标数据，充满网络攻击者将着重于对系统的访问权限。

作为，主要安全外围使用公司网络入口和出口点重点传统安全方法，但通过两种趋势大幅减少了效率的网络安全：

-   组织控制数据和资源之外传统网络边界移动企业版的电脑上、 手机和平板电脑、 云服务，如设备和 BYOD 设备

-   对手已被证明一致且正在进行的功能以获取在网络通过网络钓鱼网站和其他 web 和电子邮件攻击边界工作站上的访问权限。

网络安全外围复杂的现代企业中的自然替代是在组织身份层身份验证和授权的控件。 权限管理的帐户中的此新的"安全外围"控制实际上是以便关键保护访问权限：

![显示组织身份层的图示](../media/securing-privileged-access/PAW_LP_Fig2.JPG)

对手获得管理的帐户的控制，可以使用这些权限把握住其增益付费目标组织的情况下，如下所示：

![显示入侵者，获得管理的帐户的控制如何使用这些权限把握住其增益付费目标组织的情况下的图示](../media/securing-privileged-access/PAW_LP_Fig3.JPG)

攻击通常导致攻击者掌控管理的帐户类型的详细信息，请访问[传递哈希网站](https://www.microsoft.com/pth)信息性白色文章、 视频等。

该图显示了单独的"信道"路线图建立隔离从 web 浏览和访问电子邮件等高风险标准用户任务的访问权限的任务的管理。

![显示单独的"信道"管理路线图建立隔离从 web 浏览和访问电子邮件等高风险标准用户任务的访问权限的任务的图示](../media/securing-privileged-access/PAW_LP_Fig4.JPG)

本指南中所述，便可以获得使用多种方法授权的访问权限的控制，因为缓解了此风险需要整体和详细的技术方法。 这些内容将隔离及加强您的环境中启用通过生成防御列在下图中的每个区域中缓解的访问权限的元素：

![显示攻击和 defense 列表](../media/securing-privileged-access/PAW_LP_Fig5.JPG)

## <a name="security-privileged-access-roadmap"></a>安全特权访问路线图
路线图旨在最大限度技术，你可能已使用部署，利用密钥当前和即将推出的安全技术和集成你可能已经部署任何第三方安全工具。

Microsoft 建议路线图分为 3 阶段：

-   2-4 每周计划-快速减少攻击最常使用的技巧

-   计划 1-3 个月-版本查看和控制管理员活动

-   6 + 月计划-将继续进行生成防御到更多前瞻性的安全状态

路线图的每个阶段旨在筹集成本和对手攻击授权的访问有关你在本地和云资产的难度。 路线图优先安排最有效和首先基于这些攻击和解决方案实现体验的最快实现。

> [!NOTE]
> 对于路线图时间线都大致和均基于我们的客户实现体验。 在你的组织复杂性根据您的环境和你更改管理流程的而有所不同持续时间。

### <a name="security-privileged-access-roadmap-stage-1"></a>安全特权访问路线图： 阶段 1
1 阶段路线图致力于快速缓解凭据被盗和滥用的危害攻击最常使用的技巧。 Stage 1 旨在实现在大约 2-4 几周内，并且会在此图示描述：

![该图显示该阶段 1 旨在实现在大约 2-4 几周](../media/securing-privileged-access/PAW_LP_Fig6.JPG)

1 阶段安全授权的访问权限的规划图中包含这些组件：

**1.单独管理任务管理员的帐户**

若要帮助的分隔 internet 风险 (网络钓鱼攻击 web 浏览) 从管理权限为具有管理员权限的所有人员创建专用的帐户。 有关此其他指南包含在爪说明发布[此处](http://Aka.ms/CyberPAW)。

**2.特权访问工作站 （猫爪） 阶段 1: Active Directory 管理员**

若要帮助的分隔 internet 风险 (网络钓鱼攻击 web 浏览) 从域管理权限为与广告管理权限的人员创建专用授权的访问权限工作站 （猫爪）。 这是爪计划的第一步的发布的指南阶段 1[此处](http://Aka.ms/CyberPAW)。

**3.对于工作站唯一的本地管理员密码**

**4.服务器的唯一的本地管理员密码**

缓解对手盗取本地三千数据库中的本地管理员帐户密码哈希和滥用它攻击其他计算机的风险，你应使用分圈工具配置每个工作站和服务器上的唯一随机的密码，并在 Active Directory 注册这些密码。 你可以获取用于工作站和服务器上使用的本地管理员密码解决方案[此处](http://Aka.ms/LAPS)。

找不到其他指南操作分圈和猫爪环境[此处](http://aka.ms/securitystandards)。

### <a name="security-privileged-access-roadmap-stage-2"></a>安全特权访问路线图： 阶段 2
Stage 2 版本上缓解从阶段 1，并且专门用于在大约 1-3 个月中实现。 此阶段步骤此图所示：

![显示阶段 2 步骤的图示](../media/securing-privileged-access/PAW_LP_Fig7.JPG)

**1.爪阶段 2 到 3： 所有管理员和其他强化**

若要所有权限管理的帐户的分隔 internet 风险，继续使用你开始使用阶段 1 爪，并为的访问权限的所有人员实现专用的工作站。 发布此阶段 2 到 3 平板电脑的指南到地图[此处](http://Aka.ms/CyberPAW)。

**2.时间限制权限 （没有永久管理员）**

为了降低曝光度时间的权限，并提高了解它们的使用，只需的时间 (JIT)，使用合适的解决方案，如以下提供了权限：

-   对于 Active Directory 域服务 (广告 DS)，请使用 Microsoft 身份管理器 (MIM) 的[特权访问权限管理器 (PAM)](https://technet.microsoft.com/en-us/library/mt150258.aspx)功能。

-   使用 Azure Active Directory [Azure AD 特权身份管理 (PIM)](http://aka.ms/AzurePIM)功能。

**3.多重提升时间限制**

为了提高管理员身份验证的保障级别，应需要多重身份验证之前授予访问权限。
这可以实现 MIM PAM 和 Azure AD PIM 使用 Azure 多重身份验证 (MFA)。

**4.对于 DC 维护只需足够管理员 (JEA)**

为了减少帐户关联的风险域管理权限与的数量，只需足够管理 (JEA) 功能用于在 PowerShell 中执行常见维护域控制器上的操作。 JEA 技术允许特定对服务器 （如域控制器） 执行特定的管理任务，而提供它们管理员权限的用户。 本指南中的下载[TechNet](http://aka.ms/JEA)。

**5.低攻击图面域，域控制器**

为了减少对手森林控制的机会，你应该可以减少攻击可以采取进行控制的域控制器或对象的域控件中的路径。 按照指导，以便减少发布此风险[此处](http://aka.ms/HardenAD)。

**6。 检测攻击**

插入活动凭据被盗，身份获取可见性攻击，以便你可以快速地响应事件，并包含损坏，部署和配置[Microsoft 高级威胁分析 (ATA)](https://www.microsoft.com/ata)。

安装 ATA 之前, 你应确保你有过程来处理 ATA 可能会检测到的主要安全事件。

-   设置事件响应进程的详细信息，请参阅[响应 IT 安全事件](https://aka.ms/irr)并"响应可疑活动"和"从违反恢复"部分[Mitigating Pass--希代码以及其他凭据被盗](https://www.microsoft.com/pth)，第 2 版。

-   玩 Microsoft 服务，来帮助进行 ATA 生成事件和部署 ATA 准备红外线增强进程的详细信息，方法是访问联系 Microsoft 代表[本页](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)。

-   访问[本页](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)上玩 Microsoft 服务，以帮助研究，并从中事件的详细信息

-   实现 ATA，请按照部署指南可用[此处](http://aka.ms/ata)。

### <a name="security-privileged-access-roadmap-stage-3"></a>安全特权访问路线图： 阶段 3
路线图阶段 3 版本上缓解阶段 1 和 2，以增强和添加对所有缓解。 此图表视力描述了步骤 3:

![显示阶段 3 平板电脑的图示](../media/securing-privileged-access/PAW_LP_Fig8.JPG)

这些功能将在从上一个阶段缓解上生成，并将你的防护移到更多前瞻性状况。

**1.现代化角色和委派模型**

为降低安全风险，应该重新设计你角色和委派模型符合层型号规则、 容纳云服务管理角色和合并为一条关键原则管理员可用性的所有方面。 此模型应利用 JIT 和 JEA 功能的早期阶段以及任务自动化技术以部署为了实现这些目标。

**2.智能卡或护照所有管理员的身份验证**

为了提高保障级别和可用性管理员身份验证，应托管和中 Azure Active Directory （包括联盟到云服务的帐户） 将 Windows Server Active Directory 的所有管理的帐户需要强大的身份验证。

**3.适用于 Active Directory 管理员管理员森林**

若要提供最强的 Active Directory 管理员保护，设置环境，并不的安全依赖于生产 Active Directory 独立于从所有攻击但生产环境中最受信任的系统。 有关详细信息 ESAE 体系结构访问[本页](http://aka.ms/esae)。

**4.代码完整性策略 Dc (Server 2016)**

若要限制未经授权的程序，从对手攻击操作和无意管理错误域控制器上的风险，配置 Windows Server 2016 代码完整性内核 （驱动程序） 和用户模式 （应用程序） 仅允许在计算机上运行的授权可执行文件。

**5.屏蔽 Vm 的虚拟 Dc （Server 2016 HYPER-V 光纤）**

若要从攻击源利用虚拟机的内在胡子物理安全的保护虚拟化的域控制器，使用这一新 Server 2016 HYPER-V 功能来帮助防止 Active Directory 机密从虚拟 Dc 盗窃。 使用此解决方法，你可以加密代 2 Vm 保护 VM 数据免受检查、 被盗，和存储和网络管理员篡改以及 HYPER-V 主机管理员攻击 VM 到加强的访问权限。

## <a name="am-i-done"></a>我已经完成？
完成此路线图将获得今天目前已知和适用于对手攻击强授权的访问权限保护措施。 遗憾的是，安全威胁不断将不断改进和 shift，因此我们建议你查看作为持续进程侧重于一些引发成本和降低成功率对手定位您的环境的安全。

保护特权访问是关键第一步建立安全保证业务资产在现代组织中，但它不是像策略、 操作，信息安全、 服务器、 应用、 电脑、 设备、 云结构和其他组件保证安全你需要将包括元素完整的安全程序的唯一的一部分。

生成一个完整的安全路线图的详细信息，请参阅"客户责任和路线图"部分中的可用的企业设计师文档 Microsoft 云安全[此处](http://aka.ms/securecustomer)。

有关玩 Microsoft 服务，使用这些主题助手的详细信息，请联系你的 Microsoft 代表或访问[本页](https://www.microsoft.com/en-us/microsoftservices/campaigns/cybersecurity-protection.aspx)。

### <a name="related-topics"></a>相关的主题
[品尝的顶级： 如何减少通过希代码以及其他形式的凭据被盗](https://channel9.msdn.com/Blogs/Taste-of-Premier/Taste-of-Premier-How-to-Mitigate-Pass-the-Hash-and-Other-Forms-of-Credential-Theft)

[Microsoft 高级威胁 Analytics](http://aka.ms/ata)

[保护与 Credential Guard 派生的域凭据](https://technet.microsoft.com/en-us/library/mt483740%28v=vs.85%29.aspx)

[设备 Guard 概述](https://technet.microsoft.com/en-us/library/dn986865(v=vs.85).aspx)

[保护安全管理员工作站高端设备](https://msdn.microsoft.com/en-us/library/mt186538.aspx)

[与 Dave Probert （第 9 频道） 的 Windows 10 中的独立的用户模式](https://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-in-Windows-10-with-Dave-Probert)

[独立用户模式进程和日志与 Windows 10 中的功能 Gabriel （第 9 频道）](http://channel9.msdn.com/Blogs/Seth-Juarez/Isolated-User-Mode-Processes-and-Features-in-Windows-10-with-Logan-Gabriel)

[在进程和 Dave Probert （第 9 频道） 独立的用户模式 Windows 10 中的功能](https://channel9.msdn.com/Blogs/Seth-Juarez/More-on-Processes-and-Features-in-Windows-10-Isolated-User-Mode-with-Dave-Probert)

[降低凭据盗窃使用 Windows 10 独立用户模式 (第 9 频道)](https://channel9.msdn.com/Blogs/Seth-Juarez/Mitigating-Credential-Theft-using-the-Windows-10-Isolated-User-Mode)

[启用 Windows Kerberos 中的严格 KDC 验证](https://www.microsoft.com/en-us/download/details.aspx?id=6382)

[在适用于 Windows Server 2012 Kerberos 身份验证的新增功能](https://technet.microsoft.com/library/hh831747.aspx)

[Windows Server 2008 R2 的分步指南中的广告 DS 的身份验证机制保证](https://technet.microsoft.com/library/dd378897(v=ws.10).aspx)

[受信任的平台模块](https://docs.microsoft.com/en-us/windows/device-security/tpm/trusted-platform-module-overview)
