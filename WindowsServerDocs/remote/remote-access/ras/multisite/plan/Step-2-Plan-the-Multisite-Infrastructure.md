---
title: 步骤2规划多站点基础结构
description: 本主题是在 Windows Server 2016 中的多站点部署中部署多台远程访问服务器指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff8a58aa679691132d074ef52b876cea05366ab5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367102"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>步骤2规划多站点基础结构

>适用于：Windows Server（半年频道）、Windows Server 2016

在多站点拓扑中部署远程访问的下一步是完成多站点基础结构规划;包括、Active Directory、安全组和组策略对象。  
## <a name="bkmk_2_1_AD"></a>2.1 计划 Active Directory  
可以在多个拓扑中配置远程访问多站点部署：  
  
-   **单个 Active Directory 站点，多个入口点**-在此拓扑中，在整个站点中有一个具有快速 intranet 链接的整个组织的单一 Active Directory 站点，但在整个站点中部署了多个远程访问服务器你的组织，每个充当入口点。 此拓扑的地理示例是，在东海岸和西 coast 上具有入口点美国的单个 Active Directory 站点。  
  
    ![多站点基础结构](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **多个 Active Directory 网站，多个入口点**-在此拓扑中，你有两个或多个 Active Directory 站点，其中包含部署为每个站点的入口点的远程访问服务器。 每个远程访问服务器都与站点的 Active Directory 域控制器关联。 此拓扑的地理示例是为美国创建一个 Active Directory 站点，将一个站点用于每个站点的单个入口点。 请注意，如果有多个 Active Directory 站点，则无需具有与每个站点相关联的入口点。 此外，某些 Active Directory 网站可以有多个关联的入口点。  
  
    ![多站点拓扑](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
在多站点入口点中，你可以配置单个远程访问服务器、多个远程访问服务器或远程访问服务器群集。   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Active Directory 最佳实践和建议  
请注意以下有关多站点方案中 Active Directory 部署的建议和约束：  
  
1.  每个 Active Directory 站点可包含一个或多个远程访问服务器或服务器群集，作为客户端计算机的多站点入口点。 但 Active Directory 站点无需具有入口点。  
  
2.  多站点入口点只能与单个 Active Directory 站点相关联。 当运行 Windows 8 的客户端计算机连接到特定入口点时，它们被视为属于与该入口点相关联的 Active Directory 站点。  
  
3.  建议每个 Active Directory 站点都有一个域控制器。 域控制器可以是只读的。  
  
4.  如果每个 Active Directory 站点都包含一个域控制器，则入口点中某个服务器的 GPO 由与该终结点关联的 Active Directory 站点中的一个域控制器进行管理。 如果该站点中没有启用写功能的域控制器，则会在已启用写入权限的域控制器上管理服务器的 GPO，该控制器与在入口点中配置的第一个远程访问服务器最接近。 最接近的由链接开销计算确定。 请注意，在这种情况下，在进行配置更改后，在服务器的 Active Directory 站点中管理 GPO 和只读域控制器的域控制器之间进行复制时，可能会有延迟。  
  
5.  在作为主域控制器（PDC）仿真器运行的域控制器上管理客户端 Gpo 和可选的应用程序服务器 Gpo。 这意味着不一定要在包含客户端连接到的入口点的 Active Directory 站点中管理客户端 Gpo。  
  
6.  如果 Active Directory 站点的域控制器不可访问，则远程访问服务器将连接到站点中的备用域控制器（如果可用）。 如果没有，则它会连接到其他站点的域控制器，以检索更新的 GPO 并对客户端进行身份验证。 在这两种情况下，除非域控制器可用，否则不能使用远程访问管理控制台和 PowerShell cmdlet 来检索或修改配置设置。 请注意以下事项：  
  
    1.  如果作为 PDC 模拟器运行的服务器不可用，则必须指定一个具有已更新 Gpo 的可用域控制器作为 PDC 仿真器。  
  
    2.  如果管理服务器 GPO 的域控制器不可用，请使用 Set-daentrypointdc PowerShell cmdlet 将新的域控制器与入口点相关联。 在运行 cmdlet 之前，新的域控制器应具有最新的 Gpo。  
  
## <a name="bkmk_2_2_SG"></a>2.2 计划安全组  
在使用高级设置部署单个服务器的过程中，将通过 DirectAccess 访问内部网络的所有客户端计算机都收集到安全组中。 在多站点部署中，此安全组仅用于 Windows 8 客户端计算机。 对于多站点部署，将为多站点部署中的每个入口点将 Windows 7 客户端计算机收集到单独的安全组中。 例如，如果你先前已将组 DA_Clients 中的所有客户端计算机分组，你现在必须删除该组中的所有 Windows 7 计算机并将它们放在不同的安全组中。 例如，在多个 Active Directory 站点、多个入口点拓扑中，你为美国入口点（DA_Clients_US）创建一个安全组，并为欧洲入口点（DA_Clients_Europe）创建一个安全组。 将位于美国中的任何 Windows 7 客户端计算机放在 DA_Clients_US 组中，并将其放置在 DA_Clients_Europe 组中的任何位置。 如果没有任何 Windows 7 客户端计算机，则不需要为 Windows 7 计算机规划安全组。  
  
所需的安全组如下所示：  
  
-   适用于所有 Windows 8 客户端计算机的一个安全组。 建议为每个域的客户端创建一个唯一的安全组。  
  
-   包含每个入口点的 Windows 7 客户端计算机的唯一安全组。 建议为每个域创建唯一组。 例如：Domain1\DA_Clients_Europe;Domain2\DA_Clients_Europe;Domain1\DA_Clients_US;Domain2\DA_Clients_US.  
  
## <a name="bkmk_2_3_GPO"></a>2.3 计划组策略对象  
在远程访问部署过程中配置的 DirectAccess 设置将收集到 Gpo 中。 你的单一服务器部署已为 DirectAccess 客户端、远程访问服务器和应用程序服务器使用 Gpo。 多站点部署需要以下 Gpo：  
  
-   每个入口点的服务器 GPO。  
  
-   包含运行 Windows 8 的客户端计算机的每个域的 GPO。  
  
-   每个入口点和每个包含 Windows 7 客户端计算机的域的 GPO。  
  
可按如下所示配置 Gpo：  
  
-   **自动**-可以指定通过远程访问自动创建 gpo。 为每个 GPO 指定默认名称，并且可以对其进行修改。  
  
-   **手动**-你可以使用已由 Active Directory 管理员手动创建的 gpo。  
  
> [!NOTE]  
> 将 DirectAccess 配置为使用特定的 Gpo 后，无法将其配置为使用不同的 Gpo。  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 自动创建的 Gpo  
使用自动创建的 GPO 时，请注意以下事项：  
  
-   将根据位置和链接目标参数应用自动创建的 GPO，如下所示：  
  
    -   对于服务器 GPO，位置和链接参数都指向包含远程访问服务器的域。  
  
    -   对于客户端 Gpo，将链接目标设置为在其中创建了 GPO 的域的根。 为每个包含客户端计算机的域创建一个 GPO，并将该 GPO 链接到每个域的根。 .  
  
-   对于自动创建的 Gpo，若要应用 DirectAccess 设置，远程访问服务器管理员需要以下权限：  
  
    -   对所需的域的 GPO 创建权限。  
  
    -   所有选定客户端域根的链接权限。  
  
    -   如果 DirectAccess 配置为仅在移动计算机上工作，则需要创建 Gpo 的 WMI 筛选器的权限。  
  
    -   与入口点（服务器 GPO 域）关联的域根的链接权限  
  
    -   我们建议远程访问管理员具有对每个所需的域的 GPO 读取权限。 这使远程访问可以验证在为多站点部署创建 Gpo 时不存在具有相同名称的 Gpo。  
  
    -   Active Directory 与入口点相关联的域上的复制权限。 这是因为，当最初添加入口点时，会在与该入口点最接近的域控制器上创建入口点的服务器 GPO。 但是，由于仅在 PDC 模拟器上支持链接创建，因此必须将 GPO 从创建它的域控制器复制到作为 PDC 仿真器运行的域控制器，然后再创建链接。  
  
请注意，如果复制和链接 Gpo 的正确权限不存在，则会发出警告。 远程访问操作将继续，但不会进行复制和链接。 如果发出了链接警告，则即使在设置了权限后，也不会自动创建链接。 管理员将需要手动创建链接。  
  
### <a name="232-manually-created-gpos"></a>2.3.2 手动创建的 Gpo  
使用手动创建的 GPO 时，请注意以下事项：  
  
-   应为多站点部署手动创建以下 Gpo：  
  
    -   **服务器 gpo**-每个入口点（在入口点所在的域中）的服务器 gpo。 此 GPO 将应用于入口点中的每个远程访问服务器。  
  
    -   **客户端 GPO （Windows 7）** -每个入口点的 GPO 和包含将连接到多站点部署中入口点的 Windows 7 客户端计算机的每个域。 例如 Domain1\DA_W7_Clients_GPO_Europe;Domain2\DA_W7_Clients_GPO_Europe;Domain1\DA_W7_Clients_GPO_US;Domain2\DA_W7_Clients_GPO_US. 如果 Windows 7 客户端计算机将不会连接到入口点，则不需要 Gpo。  
  
-   不需要为 Windows 8 客户端计算机创建其他 Gpo。 部署单一远程访问服务器时，已经创建了包含客户端计算机的每个域的 GPO。 在多站点部署中，这些客户端 Gpo 将充当适用于 Windows 8 客户端的 Gpo。  
  
-   在单击 "多站点部署向导" 中的 "提交" 按钮之前，Gpo 应存在。  
  
-   在使用手动创建的 GPO 时，将在整个域中搜索到 GPO 的链接。 如果未在域中链接 GPO，将在域的根中自动创建链接。 如果未提供创建链接所需的权限，则会发出警告。  
  
-   使用手动创建的 Gpo 时，若要应用 DirectAccess 设置，远程访问服务器管理员需要对手动创建的 Gpo 具有完全的 GPO 权限（编辑、删除、修改安全）。  
  
请注意，如果复制和链接 Gpo 的正确权限不存在，则会发出警告。 远程访问操作将继续，但不会进行复制和链接。 如果发出了链接警告，则即使在设置了权限后，也不会自动创建链接。 管理员将需要手动创建链接。  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 在多域控制器环境中管理 Gpo  
每个 GPO 都由特定的域控制器管理，如下所示：  
  
-   服务器 GPO 由与服务器关联的 Active Directory 站点中的一个域控制器进行管理，或者，如果该站点中的域控制器是只读的，则为与入口点中第一个服务器最接近的已启用写入域控制器。  
  
-   客户端 Gpo 将由作为 PDC 模拟器运行的域控制器进行管理。  
  
如果要手动修改 GPO 设置，请注意以下事项：  
  
-   对于服务器 Gpo，若要确定哪个域控制器与特定入口点相关联，请运行 PowerShell cmdlet `Get-DAEntryPointDC -EntryPointName <name of entry point>`。  
  
-   默认情况下，当你使用网络 PowerShell cmdlet 或组策略管理控制台进行更改时，将使用充当 PDC 模拟器的域控制器。  
  
-   如果在域控制器上修改的设置不是与入口点（对于服务器 Gpo）或 PDC 模拟器（对于客户端 Gpo）关联的域控制器，请注意以下事项：  
  
    1.  在修改这些设置之前，请确保使用最新的 GPO 复制域控制器，并在进行更改之前[备份 gpo 设置](https://go.microsoft.com/fwlink/?LinkID=257928)。 如果未更新 GPO，在复制期间可能会发生合并冲突，从而导致远程访问配置损坏。  
  
    2.  修改设置之后，必须等待将更改复制到与 Gpo 关联的域控制器。 在复制完成之前，请勿使用远程访问管理控制台或远程访问 PowerShell cmdlet 进行其他更改。 如果在复制完成之前，在两个不同的域控制器上编辑了 GPO，则可能会发生合并冲突，从而导致配置损坏  
  
-   或者，你可以在组策略管理控制台中使用 "**更改域控制器**" 对话框或使用**open-netgpo** PowerShell cmdlet 更改默认设置，以便使用控制台或网络 cmdlet 进行更改使用指定的域控制器。  
  
    1.  若要在组策略管理控制台中执行此操作，请右键单击域或站点容器，然后单击 "**更改域控制器**"。  
  
    2.  若要在 PowerShell 中执行此操作，请为 Open-netgpo cmdlet 指定 "输入" 参数。 例如，若要使用名为 europe-dc.corp.contoso.com 的域控制器在名为 domain1\DA_Server_GPO 要的 GPO 上的 Windows 防火墙中启用专用和公用配置文件，请执行以下操作：  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>修改域控制器关联  
要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 在某些情况下，可能需要为 GPO 分配不同的域控制器：  
  
-   **域控制器维护和停机时间**-管理 gpo 的域控制器不可用时，可能需要在不同的域控制器上管理 gpo。  
  
-   **配置分发优化**-在网络基础结构更改之后，可能需要在与入口点相同的 Active Directory 站点中管理域控制器上某个入口点的服务器 GPO。   
  
## <a name="bkmk_2_4_DNS"></a>2.4 规划 DNS  
为多站点部署规划 DNS 时，请注意以下事项：  
  
1.  客户端计算机使用 ConnectTo 地址连接到远程访问服务器。 部署中的每个入口点都需要不同的 ConnectTo 地址。 每个入口点 ConnectTo 地址必须在公共 DNS 中提供，并且你选择的地址必须与你为 ip-https 连接部署的 ip-https 证书的使用者名称匹配。   
  
2.  此外，远程访问强制实施对称路由;因此，当启用了多站点部署时，只有 Teredo 和 ip-https 客户端可以连接。 若要允许本机 IPv6 客户端连接，ConnectTo 地址（ip-https URL）必须解析为远程访问服务器上的外部（面向 Internet 的） IPv6 和 IPv4 地址。  
  
3.  远程访问将创建可由 DirectAccess 客户端计算机用来验证到内部网络的连接的默认 Web 探测。 在配置单台服务器的过程中，应已在 DNS 中注册了以下名称：  
  
    1.  Directaccess-webprobehost-应解析为远程访问服务器的内部 IPv4 地址，或解析为仅限 IPv6 的环境中的 IPv6 地址。  
  
    2.  Directaccess-corpconnectivityhost-应解析为本地主机（环回）地址（：：1或127.0.0.1，具体取决于是否在公司网络中部署了 IPv6）。  
  
    在多站点部署中，必须为每个入口点创建 Directaccess-webprobehost 的其他 DNS 条目。 添加入口点时，远程访问尝试自动创建此附加的 Directaccess-webprobehost 条目。 但是，如果该操作失败，将显示一条警告，并且你必须手动创建该条目。  
  
    > [!NOTE]  
    > 在 DNS 基础结构中启用 DNS 清理后，建议对远程访问自动创建的 DNS 条目禁用清理。  
  

  



