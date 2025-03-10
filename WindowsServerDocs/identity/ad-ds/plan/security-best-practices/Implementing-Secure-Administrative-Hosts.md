---
ms.assetid: eafdddc3-40d7-4a75-8f4f-a45294aabfc8
title: 实现安全管理主机
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 56986f2ea9f49bdfc1194ae5342798793524e86c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408619"
---
# <a name="implementing-secure-administrative-hosts"></a>实现安全管理主机

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

安全管理主机指的是为创建安全平台而专门配置的工作站或服务器，特权帐户可以在 Active Directory 或域控制器上执行管理任务。已加入域的系统，以及在已加入域的系统上运行的应用程序。 在这种情况下，"特权帐户" 不仅指作为 Active Directory 中大多数特权组的成员的帐户，而且还引用已委派了允许执行管理任务的权限的任何帐户。  
  
这些帐户可能是能够为域中大多数用户重置密码的技术支持帐户、用于管理 DNS 记录和区域的帐户，或者是用于配置管理的帐户。 安全管理主机专用于管理功能，不运行诸如电子邮件应用程序、web 浏览器或生产力软件（如 Microsoft Office）的软件。  
  
尽管 "最具特权" 的帐户和组应对应于最受得到的保护，但这并不是为了保护与这些标准用户帐户的权限高于其权限的任何帐户和组。  
  
安全管理主机可以是仅用于管理任务的专用工作站、运行远程桌面网关 server 角色的成员服务器，以及用户连接到的服务器，以执行目标主机的管理或运行的服务器Hyper-v 角色并为每个 IT 用户提供唯一的虚拟机以用于其管理任务。 在许多环境中，可能会实现所有这三种方法的组合。  
  
实现安全管理主机要求规划和配置与组织的规模、管理实践、风险兴趣和预算一致。 此处提供了用于实现安全管理主机的注意事项和选项，供你在开发适用于你的组织的管理策略时使用。  
  
## <a name="principles-for-creating-secure-administrative-hosts"></a>用于创建安全管理主机的原则  
为了有效地保护系统免受攻击，应牢记几个一般原则：  
  
1.  从不受信任的主机（也就是不能与它所管理的系统的安全级别相同的工作站）管理受信任的系统（即域控制器等安全服务器）。  
  
2.  执行特权活动时，不应依赖于单个身份验证因素;也就是说，用户名和密码组合不应被视为可接受的身份验证，因为只表示一个因素（您知道的内容）。 你应考虑在管理方案中生成和缓存凭据或存储凭据的位置。  
  
3.  尽管当前威胁环境中的大多数攻击都利用了恶意软件和恶意攻击，但在设计和实现安全管理主机时，不要忽略物理安全性。  
  
### <a name="account-configuration"></a>帐户配置  
即使你的组织当前未使用智能卡，也应考虑为特权帐户和安全管理主机实现它们。 管理主机应配置为需要所有帐户的智能卡登录，方法是修改链接到包含管理主机的 Ou 的 GPO 中的以下设置：  
  
@no__t 0Computer 配置 \windows \ 本地策略 \ 本地策略 \ Options\Interactive 登录：需要智能卡 @ no__t-0  
  
此设置将要求所有交互式登录都使用智能卡，而不考虑 Active Directory 中个人帐户的配置。  
  
还应将安全管理主机配置为仅允许授权帐户登录，可以在中配置这些帐户：  
  
**计算机配置 \windows \ 本地策略 \ 本地策略 \ 特权分配**  
  
这会向安全管理主机的授权用户授予交互（仅限，并向其授予远程桌面服务）登录权限。  
  
### <a name="physical-security"></a>物理安全性  
对于被视为可信管理主机的管理主机，必须将它们配置和保护为与它们所管理的系统相同的级别。 [保护域控制器免遭攻击](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)所提供的大多数建议也适用于用于管理域控制器和 AD DS 数据库的主机。 在大多数环境中实施安全管理系统所面临的挑战之一是，物理安全性可能更难以实现，因为这些计算机通常驻留在与数据中心托管的服务器不安全的区域中，例如管理用户的桌面。  
  
物理安全性包括控制对管理主机的物理访问。 在小型组织中，这可能意味着你维护专用管理工作站，该工作站在办公室或办公桌抽屉内保持锁定状态，但未使用。 这可能意味着，当你需要对 Active Directory 或域控制器执行管理时，你可以直接登录到域控制器。  
  
在中型组织中，你可以考虑实施位于办公室安全位置的安全管理 "跳转服务器"，并在需要管理 Active Directory 或域控制器时使用。 你还可以在不使用的情况下，使用或不使用跳转服务器来实现锁定在安全位置的管理工作站。  
  
在大型组织中，你可以部署数据中心托管的跳转服务器，这些服务器提供对 Active Directory 严格控制的访问权限;域控制器;和文件、打印或应用程序服务器。 跳转服务器体系结构的实现最有可能包括大型环境中安全工作站和服务器的组合。  
  
不管你的组织的规模和管理主机的设计如何，你都应该保护物理计算机不受未经授权的访问或被盗，并应使用 BitLocker 驱动器加密对管理主机上的驱动器进行加密和保护. 即使主机被盗或删除了其磁盘，也可以通过在管理主机上实施 BitLocker，来确保未授权用户无法访问驱动器上的数据。  
  
### <a name="operating-system-versions-and-configuration"></a>操作系统版本和配置  
所有管理主机（无论服务器还是工作站）都应出于本文档前面所述的原因，运行在你的组织中使用的最新操作系统。 通过运行当前的操作系统，你的管理人员将从新的安全功能、完整的供应商支持和操作系统中引入的其他功能中获益。 而且，在评估新的操作系统时，如果首先将它部署到管理主机，则需要熟悉它所提供的新功能、设置和管理机制，从而可在规划中利用这些功能更广泛的操作系统部署。 然后，你的组织中最复杂的用户还会成为熟悉新操作系统并获得支持的最佳定位的用户。  
  
### <a name="microsoft-security-configuration-wizard"></a>Microsoft 安全配置向导  
如果将跳转服务器作为管理主机策略的一部分来实现，则应使用内置安全配置向导来配置服务、注册表、审核和防火墙设置，以减少服务器的受攻击面。 如果已收集和配置安全配置向导配置设置，则可以将设置转换为用于在所有跳转服务器上强制执行一致的基线配置的 GPO。 你可以进一步编辑 GPO 以实现特定于跳转服务器的安全设置，并可以将所有设置与从 Microsoft 安全合规管理器中提取的附加基线设置组合在一起。  
  
### <a name="microsoft-security-compliance-manager"></a>Microsoft Security Compliance Manager  
[Microsoft 安全合规管理器](https://technet.microsoft.com/library/cc677002.aspx)是一个免费的工具，可将 microsoft 推荐的安全配置与操作系统版本和角色配置相集成，并在可用于创建和配置域控制器的基准安全设置。 Microsoft 安全合规管理器模板可以与 "安全配置向导" 设置结合使用，以生成由部署在服务器的 Ou 中的 Gpo 部署和强制执行的位于 Active Directory。  
  
> [!NOTE]  
> 在撰写本文时，Microsoft 安全合规管理器不包括特定于跳转服务器或其他安全管理主机的设置，但仍可使用安全合规管理器（SCM）为管理创建初始基线主机. 但是，若要正确保护主机的安全，你应该应用适用于高度安全工作站和服务器的其他安全设置。  
  
### <a name="applocker"></a>AppLocker  
通过 AppLocker 或第三方应用程序限制软件，使用脚本、工具和应用程序白名单来配置管理主机和虚拟 machinesshould。 不符合安全设置的任何管理应用程序或实用程序应升级或替换为遵守安全开发和管理实践的工具。 当管理主机上需要新的或其他工具时，应全面测试应用程序和实用程序，如果工具适用于在管理主机上进行部署，则可以将其添加到系统的白名单。  
  
### <a name="rdp-restrictions"></a>RDP 限制  
尽管特定配置因管理系统的体系结构而异，但你应包括对哪些帐户和计算机可用于与托管系统建立远程桌面协议（RDP）连接的限制。例如，使用远程桌面网关（RD 网关）跳转服务器来控制从已授权的用户和系统访问域控制器和其他托管系统。  
  
你应允许授权用户进行交互式登录，并且应删除或甚至阻止服务器访问时不需要的其他登录类型。  
  
### <a name="patch-and-configuration-management"></a>修补和配置管理  
规模较小的组织可能依赖于 Windows 更新或[Windows Server Update Services](https://technet.microsoft.com/windowsserver/bb332157) （WSUS）等产品来管理 Windows 系统更新的部署，而较大的组织可能实施企业修补和配置管理软件，如 System Center Configuration Manager。 无论你使用何种机制将更新部署到一般服务器和工作站总体，你都应该考虑为高度安全的系统（如域控制器、证书颁发机构和管理主机）进行单独的部署。 通过将这些系统与常规管理基础结构隔离，如果管理软件或服务帐户遭到入侵，则无法轻松地将此折衷扩展到基础结构中的最安全系统。  
  
尽管不应为安全系统执行手动更新过程，但是，你应该配置单独的基础结构来更新安全系统。 即使在非常大的组织中，此基础结构通常也可以通过专用的 WSUS 服务器和 Gpo 来实现安全系统。  
  
### <a name="blocking-internet-access"></a>阻止 Internet 访问  
不应允许管理主机访问 Internet，也不应能浏览组织的 intranet。 不应允许在管理主机上使用 Web 浏览器和类似的应用程序。 你可以通过将外围防火墙设置、WFAS 配置和安全主机上的 "黑洞" 代理配置结合在一起阻止安全主机的 Internet 访问。 你还可以使用 application 允许列表阻止在管理主机上使用 web 浏览器。  
  
### <a name="virtualization"></a>虚拟化  
如果可能，请考虑将虚拟机作为管理主机实现。 使用虚拟化，你可以创建集中存储和管理的每个用户的管理系统，并且可以在不使用时轻松关闭，从而确保在管理系统上不会激活凭据。 你还可以要求在每次使用后将虚拟管理主机重置为初始快照，以确保虚拟机保持处于纯洁。 下一节中提供了有关管理主机的虚拟化选项的详细信息。  
  
## <a name="sample-approaches-to-implementing-secure-administrative-hosts"></a>实现安全管理主机的示例方法  
不管你如何设计和部署管理主机基础结构，你都应记住本主题前面的 "创建安全管理主机的原理" 中提供的指导原则。 此处所述的每种方法都提供了有关如何将 "管理" 和 "生产力" 系统与 IT 员工一起使用的一般信息。 生产力系统是 IT 管理员用于检查电子邮件、浏览 Internet 并使用常规生产力软件（如 Microsoft Office）的计算机。 管理系统是一种被强制使用且专用于在 IT 环境中进行日常管理的计算机。  
  
要实现安全管理主机，最简单的方法是为 IT 人员提供安全工作站，使其能够执行管理任务。 在仅工作站实现中，每个管理工作站用于启动管理工具和 RDP 连接，以管理服务器和其他基础结构。 仅限工作站的实现可在较小的组织中有效，尽管更大、更复杂的基础结构可受益于使用专用管理服务器和工作站的管理主机的分布式设计，例如本主题后面的 "实现安全管理工作站和跳转服务器" 中所述。  
  
### <a name="implementing-separate-physical-workstations"></a>实现单独的物理工作站  
实现管理主机的一种方法是向每个 IT 用户颁发两个工作站。 一个工作站与一个 "常规" 用户帐户一起使用，以执行检查电子邮件和使用工作效率应用程序等活动，同时第二个工作站完全专用于管理功能。  
  
对于生产力工作站，可以为 IT 人员提供普通的用户帐户，而不是使用特权帐户登录到不安全的计算机。 管理工作站应配置有得到控制的配置，IT 人员应使用不同的帐户登录到管理工作站。  
  
如果已实现智能卡，则管理工作站应配置为需要智能卡登录，并且应为 IT 人员提供独立的帐户以供管理，同时配置为需要智能卡进行交互式登录。 管理主机应按照前面所述的方式进行强制，并且只有指定的 IT 用户才允许本地登录到管理工作站。  
  
#### <a name="pros"></a>优点  
通过实现单独的物理系统，可以确保为每台计算机适当地配置其角色，并确保 IT 用户不会无意中将管理系统暴露给风险。  
  
#### <a name="cons"></a>缺点  
  
-   实现单独的物理计算机会增加硬件成本。  
  
-   使用用于管理远程系统的凭据登录到物理计算机会将凭据缓存在内存中。  
  
-   如果未安全地存储管理工作站，则它们可能会通过诸如物理硬件密钥记录器或其他物理攻击之类的机制泄露。  
  
### <a name="implementing-a-secure-physical-workstation-with-a-virtualized-productivity-workstation"></a>使用虚拟化的生产力工作站实现安全的物理工作站  
在此方法中，为 IT 用户提供了一个安全的管理工作站，用户可以使用该工作站执行日常管理功能，并在其职责范围内使用远程服务器管理工具（RSAT）或到服务器的 RDP 连接。 当 IT 用户需要执行工作效率任务时，他们可以通过 RDP 连接到作为虚拟机运行的远程生产力工作站。 应该为每个工作站使用单独的凭据，并应实现智能卡等控件。  
  
#### <a name="pros"></a>优点  
  
-   管理工作站和生产力工作站是分离的。  
  
-   使用安全工作站连接到工作效率工作站的 IT 人员可以使用单独的凭据和智能卡，而不是在不太安全的计算机上存放特权凭据。  
  
#### <a name="cons"></a>缺点  
  
-   实现解决方案需要设计和实现工作，并使用可靠的虚拟化选项。  
  
-   如果物理工作站没有安全地存储，则可能很容易受到破坏硬件或操作系统的物理攻击，并使其容易遭受通信拦截。  
  
### <a name="implementing-a-single-secure-workstation-with-connections-to-separate-productivity-and-administrative-virtual-machines"></a>实现单个安全工作站，使其连接到不同的 "生产力" 和 "管理" 虚拟机  
在此方法中，你可以向其用户颁发一个被锁定的单个物理工作站，并按前面所述进行锁定，并在其上用户不具有特权访问权限。 你可以提供与托管在专用服务器上的虚拟机的远程桌面服务连接，为 IT 人员提供运行电子邮件和其他工作效率应用程序的一个虚拟机，并将另一个虚拟机配置为用户的专用管理主机。  
  
对于虚拟机，你应需要智能卡或其他多因素登录，并使用不同于登录到物理计算机的帐户以外的帐户。 IT 用户登录到物理计算机后，他们可以使用其工作效率智能卡连接到其远程生产力计算机，并使用单独的帐户和智能卡连接到其远程管理计算机。  
  
#### <a name="pros"></a>优点  
  
-   IT 用户可以使用单个物理工作站。  
  
-   通过要求对虚拟机使用单独的帐户并使用虚拟机的远程桌面服务连接，不会在本地计算机的内存中缓存 IT 用户的凭据。  
  
-   物理主机的安全级别与管理主机相同，从而降低了本地计算机的安全可能性。  
  
-   如果 IT 用户的生产力虚拟机或其管理虚拟机可能已泄露，则可以轻松地将虚拟机重置为 "已知良好" 状态。  
  
-   如果物理计算机遭到破坏，则不会在内存中缓存任何特权凭据，使用智能卡可以防止击键记录器泄露凭据。  
  
#### <a name="cons"></a>缺点  
  
-   实现解决方案需要设计和实现工作，并使用可靠的虚拟化选项。  
  
-   如果物理工作站没有安全地存储，则可能很容易受到破坏硬件或操作系统的物理攻击，并使其容易遭受通信拦截。  
  
### <a name="implementing-secure-administrative-workstations-and-jump-servers"></a>实现安全的管理工作站和跳转服务器  
作为保护管理工作站或结合使用的一种替代方法，你可以实施安全的跳转服务器，管理用户可以使用 RDP 和智能卡连接到跳转服务器来执行管理任务。  
  
应将 "跳转服务器" 配置为运行远程桌面网关角色，以允许你对跳转服务器的连接以及将从该服务器管理的目标服务器实施限制。 如果可能，还应安装 Hyper-v 角色并创建[个人虚拟](https://technet.microsoft.com/library/dd759174.aspx)机或其他每个用户的虚拟机，以便管理用户在跳转服务器上用于其任务。  
  
通过在跳转服务器上向管理用户提供每用户虚拟机，你可以为管理工作站提供物理安全性，管理用户可以在不使用时重置或关闭虚拟机。 如果你不想在相同的管理主机上安装 Hyper-v 角色和远程桌面网关角色，则可以将它们安装在不同的计算机上。  
  
应该尽可能使用远程管理工具来管理服务器。 远程服务器管理工具（RSAT）功能应安装在用户的虚拟机（或跳转服务器上，如果你未实现用于管理的每个用户的虚拟机），并且管理人员应通过 RDP 连接到用于执行管理任务的虚拟机。  
  
如果管理用户必须通过 RDP 连接到目标服务器才能直接对其进行管理，则应将 RD 网关配置为仅当使用适当的用户和计算机建立与目标的连接时才允许建立连接服务. 在未指定管理系统的系统（如一般使用的工作站和不是跳转服务器的成员服务器）上，应禁止执行 RSAT （或类似的）工具。  
  
#### <a name="pros"></a>优点  
  
-   通过创建跳转服务器，你可以将特定服务器映射到你的网络中的 "区域" （具有类似配置、连接和安全要求的系统集合），并要求管理人员实现每个区域的管理从安全的管理主机连接到指定的 "区域" 服务器。  
  
-   通过将跳转服务器映射到区域，你可以为连接属性和配置要求实现精细控制，并可以轻松地识别从未经授权的系统进行连接的尝试。  
  
-   通过在跳转服务器上实现每个管理员虚拟机，可以在完成管理任务时强制虚拟机关机并重置为已知的干净状态。 在完成管理任务后，通过强制执行虚拟机的关闭（或重新启动），攻击者无法针对虚拟机进行攻击，也不会导致凭据被盗攻击。  
  
#### <a name="cons"></a>缺点  
  
-   跳转服务器（无论是物理的还是虚拟的）需要专用服务器。  
  
-   实现指定的跳转服务器和管理工作站需要认真规划和配置，这些配置将映射到环境中配置的任何安全区域。  
  


