---
title: 步骤 2 计划多站点基础结构
description: 本主题是指南的一部分部署多台远程访问服务器在 Windows Server 2016 中的多站点部署中。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2655f4de83576ef62b113419a69badaacc868f9
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280999"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>步骤 2 计划多站点基础结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

部署远程访问多站点拓扑中的下一步是完成多站点基础结构规划;Active Directory、 安全组和组策略对象等。  
## <a name="bkmk_2_1_AD"></a>2.1 规划 Active Directory  
可以通过多种拓扑配置远程访问多站点部署：  
  
-   **单个 Active Directory 站点、 多个入口点**-为整个组织中的站点，所有的快速 intranet 链接在此拓扑中，具有单个 Active Directory 站点，但你有多个部署的远程访问服务器在整个组织，每个作为入口点。 此拓扑的地理示例是具有单个 Active Directory 站点代表美国东海岸和西海岸的入口点。  
  
    ![多站点基础结构](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **多个 Active Directory 站点、 多个入口点**-部署为每个站点的入口点的远程访问服务器在此拓扑中，有两个或多个 Active Directory 站点。 每个远程访问服务器将与 Active Directory 域控制器，该站点关联。 此拓扑的地理示例是与每个站点的单入口点已在美国的 Active Directory 站点，一个用于欧洲。 请注意，是否有多个 Active Directory 站点不需要具有与每个站点相关联的入口点。 此外，某些 Active Directory 站点可以有与之关联的多个入口点。  
  
    ![多站点拓扑](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
在多站点入口点，可以配置单个远程访问服务器，多个远程访问服务器或远程访问服务器群集。   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Active Directory 最佳做法和建议  
请注意以下建议与 Active Directory 部署在多站点方案中的约束：  
  
1.  每个 Active Directory 站点可以包含一个或多个远程访问服务器或服务器群集中，为客户端计算机的多站点入口点正常工作。 但是 Active Directory 站点不需要有一个入口点。  
  
2.  只能与单个 Active Directory 站点关联的多站点入口点。 当运行 Windows 8 客户端计算机连接到特定入口点时，它们将被视为属于该入口点相关联的 Active Directory 站点。  
  
3.  我们建议每个 Active Directory 站点具有域控制器。 域控制器可以是只读的。  
  
4.  如果每个 Active Directory 站点包含域控制器，由一个与该终结点相关联的 Active Directory 站点中的域控制器管理 GPO 中的入口点的服务器。 如果在该站点中不有任何启用写操作的域控制器，然后找到最接近的入口点中配置的第一个远程访问服务器的启用写操作的域控制器上管理服务器 GPO。 最近取决于成本计算的链接。 请注意，在此方案中之后管理 GPO 的域控制器和服务器的 Active Directory 站点中的只读域控制器之间复制时进行配置更改可能存在延迟。  
  
5.  客户端 Gpo 和可选的应用程序服务器 Gpo 在主域控制器 (PDC) 仿真程序运行的域控制器上进行管理。 这意味着 Gpo 一定不包含入口点在 Active Directory 站点中管理该客户端连接到哪些客户端。  
  
6.  如果无法访问 Active Directory 站点的域控制器时，远程访问服务器将连接到站点 （如果可用） 中的备用域控制器。 如果没有，它会连接到另一个站点检索更新的 GPO 以及如何对客户端进行身份验证的域控制器。 在这两种情况下，不能使用远程访问管理控制台和 PowerShell cmdlet 来检索或修改配置设置，直到域控制器可用。 请注意以下事项：  
  
    1.  如果与 PDC 仿真器运行的服务器不可用，则必须指定已更新 Gpo 与 PDC 模拟器可用的域控制器。  
  
    2.  如果管理服务器 GPO 的域控制器不可用时，使用 Set-daentrypointdc PowerShell cmdlet 将新的域控制器与入口点相关联。 新的域控制器应运行 cmdlet 之前具有最新的 Gpo。  
  
## <a name="bkmk_2_2_SG"></a>2.2 规划安全组  
在部署期间的一台服务器使用高级设置，访问内部网络通过 DirectAccess 的所有客户端计算机已收集到一个安全组。 在多站点部署中，此安全组用于 Windows 8 客户端计算机。 对于多站点部署，Windows 7 客户端计算机将收集为单独的安全组，以便在多站点部署中每个入口点。 例如，如果您以前分组组 DA_Clients 中的所有客户端计算机，必须现在从该组中删除任何 Windows 7 计算机，并将其放在不同的安全组。 例如，在多个 Active Directory 站点拓扑的多个入口点，创建美国入口点 (DA_Clients_US) 的安全组，另一个用于欧洲入口点 (DA_Clients_Europe)。 将位于 DA_Clients_US 组中的 United States 和 DA_Clients_Europe 组中的任何位于的欧洲的任何 Windows 7 客户端计算机。 如果还没有任何 Windows 7 客户端计算机，您不需要计划 Windows 7 计算机的安全组。  
  
所需的安全组如下所示：  
  
-   所有 Windows 8 客户端计算机的一个安全组。 建议为这些客户端为每个域创建独特的安全组。  
  
-   包含每个入口点的 Windows 7 客户端计算机的唯一安全组。 建议创建唯一的每个域组。 例如：Domain1\DA_Clients_Europe; Domain2\DA_Clients_Europe; Domain1\DA_Clients_US; Domain2\DA_Clients_US.  
  
## <a name="bkmk_2_3_GPO"></a>2.3 规划组策略对象  
在远程访问部署过程中配置的 DirectAccess 设置将收集到 Gpo。 已在单服务器部署 DirectAccess 客户端，远程访问服务器，并且根据需要为应用程序服务器使用的 Gpo。 多站点部署需要以下 Gpo:  
  
-   用于每个入口点的服务器 GPO。  
  
-   包含运行 Windows 8 的客户端计算机的每个域的 GPO。  
  
-   用于每个入口点和每个域包含 Windows 7 客户端计算机的 GPO。  
  
可以按如下所示配置 Gpo:  
  
-   **自动**-可以指定远程访问自动创建 Gpo。 默认名称指定为每个 GPO，并且可以修改。  
  
-   **手动**-你可以使用已由 Active Directory 管理员手动创建的 Gpo。  
  
> [!NOTE]  
> DirectAccess 配置为使用特定的 Gpo，它无法将配置为使用不同的 Gpo。  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 自动创建的的 Gpo  
使用自动创建的 GPO 时，请注意以下事项：  
  
-   将根据位置和链接目标参数应用自动创建的 GPO，如下所示：  
  
    -   对于服务器 GPO，位置和链接参数都指向包含远程访问服务器的域。  
  
    -   对于客户端 Gpo 中，链接目标设置为在其中创建 GPO 的域的根。 对于每个包含客户端计算机的域创建一个 GPO 和 GPO 将链接到每个域的根。 .  
  
-   自动创建 gpo，若要应用 DirectAccess 设置远程访问服务器管理员需要以下权限：  
  
    -   GPO 创建所需域的权限。  
  
    -   所有选定客户端域根的链接权限。  
  
    -   如果 DirectAccess 配置为仅移动计算机上工作，则需要为 Gpo 创建 WMI 筛选器的权限。  
  
    -   与入口点 （服务器 GPO 域） 关联的域根的链接权限  
  
    -   我们建议远程访问管理员具有对每个所需的域的 GPO 读取权限。 这使远程访问可以验证为多站点部署中创建 Gpo 时，不存在具有相同名称的 Gpo。  
  
    -   对与入口点相关联的域的 active Directory 复制权限。 这是因为在最初添加入口点时，该入口点最接近的域控制器上创建入口点的服务器 GPO。 但是，由于只能在 PDC 模拟器上支持链接创建，因此必须从创建到之前创建该链接与 PDC 仿真器运行的域控制器所在的域控制器复制 GPO。  
  
请注意不存在用于复制和链接 Gpo 的正确权限，发出警告。 远程访问操作将继续，但复制和链接将不会发生。 如果警告发出的链接将不会自动创建链接，即使权限就位。 管理员将需要手动创建链接。  
  
### <a name="232-manually-created-gpos"></a>2.3.2 手动创建的 Gpo  
使用手动创建的 GPO 时，请注意以下事项：  
  
-   多站点部署，应手动创建以下 Gpo:  
  
    -   **服务器 GPO**每个入口点的服务器 GPO （中的入口点所在的域）。 将每个入口点中的远程访问服务器上应用此 GPO。  
  
    -   **客户端 GPO (Windows 7)** -A GPO 为每个入口点和每个域包含 Windows 7 客户端计算机将连接到多站点部署中的入口点。 例如 Domain1\DA_W7_Clients_GPO_Europe;Domain2\DA_W7_Clients_GPO_Europe;Domain1\DA_W7_Clients_GPO_US;Domain2\DA_W7_Clients_GPO_US。 如果没有 Windows 7 客户端计算机将连接到入口点，则不需要的 Gpo。  
  
-   没有任何要求，若要创建其他 Gpo 适用于 Windows 8 客户端计算机。 部署单一远程访问服务器时，已创建用于包含客户端计算机的每个域的 GPO。 在多站点部署中这些客户端 Gpo 将用作 Gpo 适用于 Windows 8 的客户端。  
  
-   单击提交按钮中的多站点部署向导之前，这些 Gpo 应存在。  
  
-   在使用手动创建的 GPO 时，将在整个域中搜索到 GPO 的链接。 如果未在域中链接 GPO，将在域的根中自动创建链接。 如果未提供创建链接所需的权限，则会发出警告。  
  
-   在使用手动创建的 Gpo，若要应用 DirectAccess 设置远程访问服务器管理员需要具有完全 GPO 权限 （编辑、 删除、 修改安全性） 对手动创建的 Gpo。  
  
请注意不存在用于复制和链接 Gpo 的正确权限，发出警告。 远程访问操作将继续，但复制和链接将不会发生。 如果警告发出的链接将不会自动创建链接，即使权限就位。 管理员将需要手动创建链接。  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 在多域控制器环境中管理 Gpo  
每个 GPO 将会被管理特定的域控制器，，如下所示：  
  
-   服务器 GPO 由一个服务器，与关联的 Active Directory 站点中的域控制器或该站点中的域控制器都是只读的如果启用写操作的域控制器的项中的第一个服务器最接近的点。  
  
-   客户端 Gpo 将通过与 PDC 仿真器运行的域控制器进行管理。  
  
如果你想手动修改 GPO 设置注意如下：  
  
-   对于服务器 Gpo，若要确定哪个域控制器与特定入口点相关联，运行 PowerShell cmdlet `Get-DAEntryPointDC -EntryPointName <name of entry point>`。  
  
-   默认情况下，当您更改与网络 PowerShell cmdlet 或组策略管理控制台中，使用充当 PDC 仿真器的域控制器。  
  
-   如果修改不是与入口点 （针对服务器 Gpo） 或 PDC 仿真器 （适用于客户端 Gpo） 相关联的域控制器的域控制器上的设置，请注意以下问题：  
  
    1.  在修改设置之前, 请确保域控制器复制使用最新的 GPO，并[备份 GPO 设置](https://go.microsoft.com/fwlink/?LinkID=257928)之前进行更改。 如果未更新 GPO，在复制期间可能会发生合并冲突，从而导致远程访问配置损坏。  
  
    2.  修改后的设置，你必须等待更改复制到与 Gpo 相关联的域控制器。 不要复制完成之前使用的远程访问管理控制台或远程访问 PowerShell cmdlet 的其他更改。 如果复制完成之前，两个不同的域控制器上编辑 GPO，可能会发生合并冲突，从而导致配置已损坏  
  
-   或者可以更改默认设置使用**更改域控制器**对话框框中组策略管理控制台中，或使用**Open-netgpo** PowerShell cmdlet，因此，更改发出使用控制台或网络 cmdlet 使用你指定的域控制器。  
  
    1.  若要执行此操作的组策略管理控制台中，右键单击域或站点容器，然后单击**更改域控制器**。  
  
    2.  若要执行此操作在 PowerShell 中，指定 Open-netgpo cmdlet DomainController 参数。 例如，若要启用名为 domain1\DA_Server_GPO 要使用名为欧洲 dc.corp.contoso.com 的域控制器的 GPO 上的 Windows 防火墙中的专用和公用配置文件，执行以下操作：  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>修改域控制器关联  
要保持多站点部署中的配置一致性，应确保每个 GPO 是由一个单独的域控制器管理的，这一点很重要。 在某些情况下，它可能需要在 gpo 中分配不同的域控制器：  
  
-   **域控制器维护和停机时间**-时管理 GPO 的域控制器不可用，它可能需要以不同的域控制器上将 GPO 进行管理。  
  
-   **配置分布优化**-网络基础结构更改后，可能需要管理的服务器 GPO 的入口点作为入口点位于同一 Active Directory 站点中的域控制器上。   
  
## <a name="bkmk_2_4_DNS"></a>2.4 规划 DNS  
多站点部署规划 DNS 时，请注意以下：  
  
1.  客户端计算机使用 ConnectTo 地址才能连接到远程访问服务器。 在部署中的每个入口点需要不同的 ConnectTo 地址。 每个入口点的 ConnectTo 地址必须是在公共 DNS 中可用，并且你选择的地址必须匹配你为 IP-HTTPS 连接部署的 IP-HTTPS 证书的使用者名称。   
  
2.  此外，远程访问强制执行对称路由;因此，只有 Teredo 和 IP-HTTPS 客户端可以连接启用多站点部署。 若要允许本机 IPv6 客户端进行连接，ConnectTo 地址 (IP-HTTPS URL) 必须解析为这两个外部 （面向 Internet） IPv6 和 IPv4 地址远程访问服务器上。  
  
3.  远程访问将创建可由 DirectAccess 客户端计算机用来验证到内部网络的连接的默认 Web 探测。 在单个服务器的配置过程的以下名称应具有已在 DNS 中注册：  
  
    1.  directaccess WebProbeHost 应为远程访问服务器的内部 IPv4 地址或仅 IPv6 环境中的 IPv6 地址的解析。  
  
    2.  directaccess CorpConnectivityHost 应解析为本地主机 （环回） 地址 (或者:: 1 或 127.0.0.1，具体取决于是将 IPv6 部署在企业网络中)。  
  
    在多站点部署中必须为每个入口点创建 Directaccess-webprobehost 的附加 DNS 条目。 添加时的入口点，远程访问将尝试自动创建此额外 Directaccess-webprobehost 条目。 但是，如果失败将显示一条警告，并且必须手动创建条目。  
  
    > [!NOTE]  
    > DNS 基础结构中启用 DNS 清理后，建议禁用远程访问自动创建 DNS 条目进行清理。  
  

  



