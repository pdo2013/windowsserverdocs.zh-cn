---
title: 在 Azure 中 Active Directory 联合身份验证服务 |Microsoft Docs
description: 在本文档中，你将了解如何在 Azure 中部署 AD FS 以实现高可用性。
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 692a188c-badc-44aa-ba86-71c0e8074510
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/28/2018
ms.subservice: hybrid
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 15ebf21973f78ad705aca77e178005dfa9529cf2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868216"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>在 Azure 中部署 Active Directory 联合身份验证服务
AD FS 提供简化、安全的联合身份验证和 Web 单一登录（SSO）功能。 与 Azure AD 或 O365 联合可让用户使用本地凭据进行身份验证，并访问云中的所有资源。 因此，拥有高可用性 AD FS 基础结构，确保在本地和云中访问资源变得非常重要。 在 Azure 中部署 AD FS 有助于实现最小工作所需的高可用性。
在 Azure 中部署 AD FS 有多个优点，下面列出了其中的一些优点：

* **高可用性**-通过 Azure 可用性集的强大功能，你可以确保高可用性基础结构。
* **易于缩放**–需要更高的性能？ 只需在 Azure 中单击几下鼠标，就能轻松迁移到更强大的计算机
* **跨异地冗余**–使用 Azure 异地冗余可以确保基础结构在全球范围内高度可用
* **易于管理**–利用 Azure 门户中高度简化的管理选项，管理基础结构非常简单且无障碍 

## <a name="design-principles"></a>设计原则
![部署设计](./media/how-to-connect-fed-azure-adfs/deployment.png)

上图显示了在 Azure 中开始部署 AD FS 基础结构的建议基本拓扑。 下面列出了该拓扑的各个组件背后的原则：

* **DC/ADFS 服务器**：如果用户数少于1000，你只需将 AD FS 角色安装在域控制器上即可。 如果你不希望对域控制器产生任何性能影响，或者如果你的用户超过1000，则在单独的服务器上部署 AD FS。
* **WAP 服务器**–需要部署 Web 应用程序代理服务器，以便在用户不在公司网络中时，用户可以访问 AD FS。
* **DMZ**：Web 应用程序代理服务器将位于外围网络中，并且外围网络和内部子网之间只允许进行 TCP/443 访问。
* **负载均衡**器：为了确保 AD FS 和 Web 应用程序代理服务器的高可用性，我们建议对 AD FS 服务器使用内部负载均衡器，并为 Web 应用程序代理服务器使用 Azure 负载均衡器。
* **可用性集**：若要为 AD FS 部署提供冗余，建议将两个或更多虚拟机组合到一个可用性集中，以获得类似的工作负载。 此配置可确保在计划内或计划外维护事件期间，至少有一个虚拟机可用
* **存储帐户**：建议使用两个存储帐户。 使用单个存储帐户可能会导致创建单点故障，并可能导致在存储帐户出现故障的情况下，部署无法使用。 两个存储帐户将帮助每个错误行关联一个存储帐户。
* **网络隔离**：Web 应用程序代理服务器应部署在不同的外围网络中。 可以将一个虚拟网络分割成两个子网，然后在隔离的子网中部署 Web 应用程序代理服务器。 只需为每个子网配置网络安全组设置，并只允许在两个子网之间进行所需的通信。 下面是针对每个部署方案提供的详细信息

## <a name="steps-to-deploy-ad-fs-in-azure"></a>在 Azure 中部署 AD FS 的步骤
本部分中所述的步骤概述了在 Azure 中部署以下所示 AD FS 基础结构的指南。

### <a name="1-deploying-the-network"></a>1.部署网络
如上所述，可以在单个虚拟网络中创建两个子网，或者创建两个完全不同的虚拟网络（VNet）。 本文重点介绍如何部署单个虚拟网络并将其划分为两个子网。 这是一种更简单的方法，因为两个单独的 Vnet 需要 VNet 到 VNet 网关才能进行通信。

**1.1 创建虚拟网络**

![创建虚拟网络](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

在 Azure 门户中，选择 "虚拟网络"，只需单击一下即可立即部署虚拟网络和一个子网。 还定义了 INT 子网，现已准备好添加 Vm。
下一步是在网络中添加另一个子网，即外围网络子网。 若要创建外围网络子网，只需

* 选择新创建的网络
* 在属性中选择子网
* 在 "子网" 面板中，单击 "添加" 按钮
* 提供子网名称和地址空间信息，以创建子网

![Subnet](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![子网外围网络](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1.2。创建网络安全组**

网络安全组（NSG）包含一系列访问控制列表（ACL）规则，这些规则允许或拒绝到虚拟网络中的 VM 实例的网络流量。 Nsg 可以与子网或该子网内的单个 VM 实例关联。 当 NSG 与子网相关联时，ACL 规则将应用到该子网中的所有 VM 实例。
出于本指南的目的，我们将创建两个 Nsg：分别用于内部网络和外围网络。 它们分别标记为 "NSG_INT" 和 "NSG_DMZ"。

![创建 NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

创建 NSG 后，将有0个入站和0个出站规则。 一旦各个服务器上的角色已安装且正常运行，就可以根据所需的安全级别进行入站和出站规则。

![初始化 NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

创建 Nsg 后，请将 NSG_INT 与子网 INT 和 NSG_DMZ 关联到子网 DMZ。 下面提供了示例屏幕截图：

![NSG 配置](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* 单击 "子网" 打开子网面板
* 选择要与 NSG 关联的子网 

配置后，子网的面板应如下所示：

![NSG 后的子网](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1.3。创建到本地的连接**

我们需要连接到本地才能在 azure 中部署域控制器（DC）。 Azure 提供了各种连接选项，可将本地基础结构连接到 Azure 基础结构。

* 点到站点
* 虚拟网络站点到站点
* ExpressRoute

建议使用 ExpressRoute。 ExpressRoute 使你能够在 Azure 数据中心与你的本地环境或共同位置环境中的基础结构之间创建专用连接。 ExpressRoute 连接不通过公共 Internet 。 与通过 Internet 的典型连接相比，它们提供的可靠性更高、速度更快、延迟更低，安全性更低。
尽管建议使用 ExpressRoute，但你可以选择最适合组织的任何连接方法。 若要详细了解 ExpressRoute 以及使用 ExpressRoute 的各种连接选项，请阅读[ExpressRoute 技术概述](https://aka.ms/Azure/ExpressRoute)。

### <a name="2-create-storage-accounts"></a>2.创建存储帐户
为了保持高可用性并避免依赖单个存储帐户，你可以创建两个存储帐户。 将每个可用性集中的计算机划分为两组，然后将每个组分配给单独的存储帐户。

![创建存储帐户](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3.创建可用性集
对于每个角色（DC/AD FS 和 WAP），请创建每个可用性集，其中每个都至少包含2台计算机。 这将有助于为每个角色实现更高的可用性。 创建可用性集时，必须确定以下各项：

* **容错域**：同一容错域中的虚拟机共享同一个电源和物理网络交换机。 建议至少使用2个容错域。 默认值为3，你可以将其保留为用于此部署的目的
* **更新域**：在更新过程中，属于同一更新域的计算机将一起重新启动。 需要至少2个更新域。 默认值为5，你可以将其保留为用于此部署的目的

![可用性集](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

创建以下可用性集

| 可用性集 | Role | 容错域 | 更新域 |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4.部署虚拟机
下一步是部署将在你的基础结构中托管不同角色的虚拟机。 建议每个可用性集中至少有两台计算机。 为基本部署创建四个虚拟机。

| Machine | Role | Subnet | 可用性集 | 存储帐户 | IP 地址 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |整形 |contosodcset |contososac1 |Static |
| contosodc2 |DC/ADFS |整形 |contosodcset |contososac2 |Static |
| contosowap1 |WAP |外围网络 |contosowapset |contososac1 |Static |
| contosowap2 |WAP |外围网络 |contosowapset |contososac2 |Static |

正如您可能已经注意到的，未指定 NSG。 这是因为 azure 允许在子网级别使用 NSG。 然后，可以使用与子网或 NIC 对象关联的单个 NSG 来控制计算机网络流量。 有关详细信息[，请参阅什么是网络安全组（NSG）](https://aka.ms/Azure/NSG)。
如果要管理 DNS，建议使用静态 IP 地址。 你可以使用 Azure DNS，而不是在你的域的 DNS 记录中，通过 Azure Fqdn 引用新计算机。
部署完成后，虚拟机窗格应如下所示：

![已部署虚拟机](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5.配置域控制器/AD FS 服务器
 为了对任何传入的请求进行身份验证，AD FS 需要联系域控制器。 为了节省从 Azure 到本地 DC 的昂贵行程以进行身份验证，建议在 Azure 中部署域控制器的副本。 为了实现高可用性，建议创建至少2个域控制器的可用性集。

| 域控制器 | Role | 存储帐户 |
|:---:|:---:|:---:|
| contosodc1 |副本 |contososac1 |
| contosodc2 |副本 |contososac2 |

* 将两个服务器提升为具有 DNS 的副本域控制器
* 使用服务器管理器安装 AD FS 角色来配置 AD FS 服务器。

### <a name="6-deploying-internal-load-balancer-ilb"></a>6.部署内部负载均衡器（ILB）
**6.1。创建 ILB**

若要部署 ILB，请在 Azure 门户中选择 "负载均衡器"，然后单击 "添加" （+）。

> [!NOTE]
> 如果菜单中未显示 "**负载均衡**器"，请单击门户左下角的 "**浏览**" 并滚动，直到看到 "**负载均衡**器"。  然后单击黄色星号将它添加到菜单中。 现在，选择 "新建负载均衡器" 图标以打开面板，开始配置负载均衡器。
> 
> 

![浏览负载均衡器](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **名称**：为负载均衡器指定适当的名称
* **方案**：由于此负载均衡器将放在 AD FS 服务器的前面，仅适用于内部网络连接，因此请选择 "内部"
* **虚拟网络**：选择要在其中部署 AD FS 的虚拟网络
* **子网**：在此处选择内部子网
* **IP 地址分配**：Static

![内部负载均衡器](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

单击 "创建" 并部署 ILB 后，你应在负载平衡器列表中看到它：

![ILB 后的负载均衡器](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

下一步是配置后端池和后端探测。

**6.2。配置 ILB 后端池**

在 "负载均衡器" 面板中选择新创建的 ILB。 它将打开 "设置" 面板。 

1. 从设置面板中选择后端池
2. 在 "添加后端池" 面板中，单击 "添加虚拟机"
3. 此时会显示一个面板，可以在其中选择可用性集
4. 选择 AD FS 可用性集

![配置 ILB 后端池](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6.3。配置探测**

在 ILB 设置面板中，选择 "运行状况探测"。

1. 单击 "添加"
2. 提供探测的详细信息。 **名称**：探测名称 b。 **协议**：HTTP c。 **端口**：80（HTTP） d。 **路径**：/adfs/probe e。 **间隔**：5（默认值）–这是 ILB 在后端池 f 中探测计算机的时间间隔。 不**正常阈值限制**：2（默认值）–这是连续探测失败阈值，之后 ILB 会将后端池中的计算机声明为无响应，并停止向它发送流量。

![配置 ILB 探测](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

我们正在使用/adfs/probe 终结点，该终结点在不会进行完全 HTTPS 路径检查的 AD FS 环境中创建显式用于运行状况检查。  此功能明显优于基本端口443检查，它无法准确反映新式 AD FS 部署的状态。  有关详细信息，请参阅 https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/ 。

**6.4。创建负载均衡规则**

为了有效地平衡流量，应该为 ILB 配置负载均衡规则。 若要创建负载均衡规则，请 

1. 从 ILB 的 "设置" 面板中选择 "负载均衡规则"
2. 单击 "负载均衡规则" 面板中的 "添加"
3. 在 "添加负载均衡规则" 面板中。 **名称**：提供规则 b 的名称。 **协议**：选择 "TCP c"。 **端口**：443 d。 **后端端口**:443 e。 **后端池**:选择早于 f 的 AD FS 群集创建的池。 **探测**：为之前的 AD FS 服务器选择创建的探测

![配置 ILB 平衡规则](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6.5。用 ILB 更新 DNS**

请在 DNS 服务器上，为 ILB 创建一个 CNAME。 CNAME 应为联合身份验证服务，其中 IP 地址指向 ILB 的 IP 地址。 例如，如果 ILB DIP 地址是10.3.0.8，而安装的联合身份验证服务是 fs.contoso.com，则创建一个指向10.3.0.8 的 fs.contoso.com 的 CNAME。
这可确保所有与 fs.contoso.com 相关的通信都在 ILB 上结束并经过适当的路由。

### <a name="7-configuring-the-web-application-proxy-server"></a>7.配置 Web 应用程序代理服务器
**7.1。配置 Web 应用程序代理服务器以访问 AD FS 服务器**

为了确保 Web 应用程序代理服务器能够访问 ILB 后面的 AD FS 服务器，请在%systemroot%\system32\drivers\etc\hosts 中为 ILB 创建一条记录。 请注意，可分辨名称（DN）应是联合身份验证服务名称，例如 fs.contoso.com。 IP 条目应为 ILB 的 IP 地址（如示例中的10.3.0.8）。

**7.2。安装 Web 应用程序代理角色**

在确保 Web 应用程序代理服务器能够访问 ILB 后面的 AD FS 服务器后，接下来可以安装 Web 应用程序代理服务器。 不需要将 Web 应用程序代理服务器加入域。 通过选择 "远程访问" 角色，将 Web 应用程序代理角色安装在两个 Web 应用程序代理服务器上。 服务器管理器将引导完成 WAP 安装。
有关如何部署 WAP 的详细信息，请参阅[安装和配置 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383662.aspx)。

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.部署面向 Internet 的（公共）负载均衡器
**8.1。创建面向 Internet 的（公共）负载均衡器**

在 Azure 门户中，选择 "负载均衡器"，然后单击 "添加"。 在 "创建负载均衡器" 面板中输入以下信息

1. **名称**：负载均衡器的名称
2. **方案**：公共–此选项告知 Azure 此负载均衡器需要公共地址。
3. **IP 地址**：创建新 IP 地址（动态）

![面向 Internet 的负载均衡器](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

部署后，负载均衡器将显示在负载均衡器列表中。

![负载均衡器列表](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8.2。向公共 IP 分配 DNS 标签**

在 "负载均衡器" 面板中单击新建的负载均衡器条目，以显示配置的面板。 按照以下步骤配置公共 IP 的 DNS 标签：

1. 单击 "公共 IP 地址"。 这会打开公共 IP 及其设置的面板
2. 单击 "配置"
3. 提供 DNS 标签。 这将成为可从任意位置访问的公共 DNS 标签，例如 contosofs.westus.cloudapp.azure.com。 可以在联合身份验证服务（如 fs.contoso.com）的外部 DNS 中添加一个条目，该条目解析为外部负载均衡器（contosofs.westus.cloudapp.azure.com）的 DNS 标签。

![配置面向 internet 的负载均衡器](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![配置面向 internet 的负载均衡器（DNS）](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8.3。为面向 Internet 的（公共）负载均衡器配置后端池** 

遵循创建内部负载均衡器中的相同步骤，将面向 Internet 的（公共）负载均衡器的后端池配置为 WAP 服务器的可用性集。 例如，contosowapset。

![配置面向 Internet 的负载均衡器的后端池](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8.4。配置探测**

遵循配置内部负载均衡器中的相同步骤来配置 WAP 服务器后端池的探测。

![配置面向 Internet 的负载均衡器的探测](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8.5。创建负载均衡规则**

按照 ILB 中的相同步骤配置 TCP 443 的负载均衡规则。

![配置面向 Internet 的负载均衡器的平衡规则](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9.保护网络
**9.1。保护内部子网**

总体而言，需要以下规则来有效保护内部子网（按如下所列的顺序）

| 规则 | 描述 | 流 |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |允许从外围网络进行 HTTPS 通信 |入站 |
| DenyInternetOutbound |无权访问 internet |出站 |

![INT 访问规则（入站）](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

**9.2。保护外围网络子网**

| 规则 | 描述 | 流 |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |允许从 internet 到外围网络的 HTTPS |入站 |
| DenyInternetOutbound |阻止了除 HTTPS 以外的任何内容 |出站 |

![EXT 访问规则（入站）](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

> [!NOTE]
> 如果需要客户端用户证书身份验证（使用 X509 用户证书的 clientTLS authentication），AD FS 需要启用 TCP 端口49443以进行入站访问。
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10.测试 AD FS 登录
最简单的方法是使用 Idpinitiatedsignon.aspx 页测试 AD FS。 为了能够执行此操作，需要在 AD FS 属性上启用 Idpinitiatedsignon.aspx。 按照以下步骤验证 AD FS 设置

1. 使用 PowerShell 在 AD FS 服务器上运行以下 cmdlet，以将其设置为 "已启用"。
   Set-adfsproperties-EnableIdPInitiatedSignonPage $true 
2. 从任何外部计算机访问 https：\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx。  
3. 应会看到如下所示的 "AD FS" 页：

![测试登录页](./media/how-to-connect-fed-azure-adfs/test1.png)

成功登录后，它将提供一条成功消息，如下所示：

![测试成功](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>用于在 Azure 中部署 AD FS 的模板
该模板部署6台计算机安装程序，其中每个都适用于域控制器 AD FS 和 WAP。

[Azure 部署模板中的 AD FS](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

部署此模板时，可以使用现有虚拟网络，也可以创建新的 VNET。 下面列出了可用于自定义部署的各种参数，以及部署过程中的参数用法说明。 

| 参数 | 描述 |
|:--- |:--- |
| Location |要将资源部署到的区域，例如 "美国东部"。 |
| StorageAccountType |创建的存储帐户的类型 |
| VirtualNetworkUsage |指示是否将创建新的虚拟网络或使用现有虚拟网络 |
| VirtualNetworkName |要创建的虚拟网络的名称，对现有虚拟网络或新的虚拟网络使用是必需的 |
| VirtualNetworkResourceGroupName |指定现有虚拟网络所在的资源组的名称。 使用现有的虚拟网络时，这将成为必需的参数，以便部署可以找到现有虚拟网络的 ID |
| VirtualNetworkAddressRange |新 VNET 的地址范围，若要创建新的虚拟网络，则必须是必需的 |
| InternalSubnetName |内部子网的名称，对这两个虚拟网络使用选项是必需的（新的或现有的） |
| InternalSubnetAddressRange |内部子网的地址范围，其中包含域控制器和 ADFS 服务器，在创建新的虚拟网络时是必需的。 |
| DMZSubnetAddressRange |外围网络子网的地址范围，其中包含 Windows 应用程序代理服务器（如果创建新的虚拟网络，则为必需）。 |
| DMZSubnetName |内部子网的名称，在两个虚拟网络使用选项（新的或现有的）上都是必需的。 |
| ADDC01NICIPAddress |第一个域控制器的内部 IP 地址，此 IP 地址静态分配给 DC，并且必须是内部子网中的有效 IP 地址 |
| ADDC02NICIPAddress |第二个域控制器的内部 IP 地址，此 IP 地址静态分配给 DC，必须是内部子网中的有效 IP 地址 |
| ADFS01NICIPAddress |第一个 ADFS 服务器的内部 IP 地址，此 IP 地址静态分配给 ADFS 服务器，必须是内部子网中的有效 IP 地址 |
| ADFS02NICIPAddress |第二个 ADFS 服务器的内部 IP 地址，此 IP 地址静态分配给 ADFS 服务器，必须是内部子网中的有效 IP 地址 |
| WAP01NICIPAddress |第一个 WAP 服务器的内部 IP 地址，此 IP 地址静态分配给 WAP 服务器，必须是外围网络子网中的有效 IP 地址 |
| WAP02NICIPAddress |第二个 WAP 服务器的内部 IP 地址，此 IP 地址静态分配给 WAP 服务器，必须是外围网络子网中的有效 IP 地址 |
| ADFSLoadBalancerPrivateIPAddress |ADFS 负载均衡器的内部 IP 地址，此 IP 地址静态分配给负载均衡器，必须是内部子网中的有效 IP 地址 |
| ADDCVMNamePrefix |域控制器的虚拟机名称前缀 |
| ADFSVMNamePrefix |ADFS 服务器的虚拟机名称前缀 |
| WAPVMNamePrefix |WAP 服务器的虚拟机名称前缀 |
| ADDCVMSize |域控制器的 vm 大小 |
| ADFSVMSize |ADFS 服务器的 vm 大小 |
| WAPVMSize |WAP 服务器的 vm 大小 |
| adminUserName |虚拟机的本地管理员的名称 |
| adminPassword |虚拟机的本地管理员帐户的密码 |

## <a name="additional-resources"></a>其他资源
* [可用性集](https://aka.ms/Azure/Availability) 
* [Azure 负载均衡器](https://aka.ms/Azure/ILB)
* [内部负载均衡器](https://aka.ms/Azure/ILB/Internal)
* [面向 Internet 的负载均衡器](https://aka.ms/Azure/ILB/Internet)
* [存储帐户](https://aka.ms/Azure/Storage)
* [Azure 虚拟网络](https://aka.ms/Azure/VNet)
* [AD FS 和 Web 应用程序代理链接](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>后续步骤
* [将本地标识与 Azure Active Directory 集成](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [使用 Azure AD Connect 配置和管理 AD FS](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [通过 Azure 流量管理器在 Azure 中跨地理位置 AD FS 部署的高可用性](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
