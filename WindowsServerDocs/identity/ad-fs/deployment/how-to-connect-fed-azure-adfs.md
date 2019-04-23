---
title: 在 Azure 中的 active Directory 联合身份验证服务 |Microsoft Docs
description: 在本文档中，您将学习如何将 Azure 中的 AD FS 部署以实现高可用性。
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
ms.openlocfilehash: 588bc3f87c78feccac47d18d31d37be3b1a02d2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835098"
---
# <a name="deploying-active-directory-federation-services-in-azure"></a>在 Azure 中部署 Active Directory 联合身份验证服务
AD FS 提供简化、 安全的标识联合身份验证和 Web 单一登录 (SSO) 功能。 使用 Azure AD 的联合身份验证或 O365 使用户能够使用本地凭据进行身份验证和访问云中的所有资源。 因此，就务必建立高度可用的 AD FS 基础结构，确保这两个本地的资源的访问权限、 在云中。 Azure 中部署 AD FS 有助于实现高可用性所需的最少量的工作。
部署在 Azure 中的 AD FS 的几个优点，下面列出了其中几以下内容：

* **高可用性**-与 Azure 可用性集的能力，请确保高可用性基础结构。
* **易于缩放**– 需要更高的性能？ 轻松地迁移到更强大的计算机中按几下鼠标，在 Azure 中
* **异地冗余**– 使用 Azure 异地冗余可以确保你的基础结构在全球各地均具有高可用性
* **易于管理**– 使用 Azure 门户中高度简化的管理选项，管理基础结构是可以轻松地轻松 

## <a name="design-principles"></a>设计原则
![部署设计](./media/how-to-connect-fed-azure-adfs/deployment.png)

上面的关系图显示了建议的基本拓扑以开始部署 Azure 中的 AD FS 基础结构。 下面列出了拓扑的各个组件的原则：

* **DC / ADFS 服务器**:如果必须少于 1,000 个用户只需在域控制器上安装 AD FS 角色。 如果不希望域控制器上的造成任何性能影响，或者您有 1,000 多个用户，然后将不同的服务器上的 AD FS 部署。
* **WAP 服务器**– 它是必须部署 Web 应用程序代理服务器，以便用户可以访问的 AD FS 时它们也不在公司网络。
* **外围网络**:Web 应用程序代理服务器将位于外围网络和外围网络与内部子网之间允许仅 TCP/443 访问。
* **负载均衡器**:若要确保 AD FS 和 Web 应用程序代理服务器的高可用性，我们建议对 Web 应用程序代理服务器的 AD FS 服务器和 Azure 负载均衡器使用内部负载均衡器。
* **可用性集**:若要向 AD FS 部署提供冗余，建议你将在可用性集中的类似工作负荷的两个或多个虚拟机。 此配置可以确保在计划内或计划外维护事件时，至少一个虚拟机将可用
* **存储帐户**:建议要有两个存储帐户。 具有单个存储帐户会导致创建单点故障，可能会导致部署变得不可用的存储帐户出现故障的可能方案中。 两个存储帐户有助于将为每条故障路线的一个存储帐户相关联。
* **网络隔离**:应在不同的外围网络中部署 web 应用程序代理服务器。 你可以将一个虚拟网络划分为两个子网，并随后部署独立的子网中的 Web 应用程序代理服务器。 只需配置的每个子网的网络安全组设置，并允许两个子网之间只需要的通信。 每种部署方案下提供更多详细信息

## <a name="steps-to-deploy-ad-fs-in-azure"></a>Azure 中部署 AD FS 的步骤
在本部分中所述的步骤概述指南来部署如下所示在 Azure 中的 AD FS 基础结构。

### <a name="1-deploying-the-network"></a>1.部署网络
如上所述，可以是在单个虚拟网络中创建两个子网也可以创建两个完全不同的虚拟网络 (VNet)。 本文重点介绍部署的单个虚拟网络，并将其划分成两个子网。 这是当前较简单的方法，因为两个单独的 Vnet 要求 VNet 到 VNet 网关进行通信。

**1.1 创建虚拟网络**

![创建虚拟网络](./media/how-to-connect-fed-azure-adfs/deploynetwork1.png)

在 Azure 门户中，选择虚拟网络和你可以部署虚拟网络和一个子网，只需单击一下立即使用。 INT 子网也定义，并随时可供 Vm 要添加的。
下一步是将另一个子网添加到网络，即外围网络子网。 若要创建外围网络子网，只需

* 选择新创建的网络
* 在属性中选择子网
* 在子网面板中单击添加按钮
* 提供要创建子网的子网名称和地址空间信息

![子网](./media/how-to-connect-fed-azure-adfs/deploynetwork2.png)

![子网外围网络](./media/how-to-connect-fed-azure-adfs/deploynetwork3.png)

**1.2.创建网络安全组**

网络安全组 (NSG) 包含允许或拒绝流向 VM 实例的虚拟网络中的网络流量的访问控制列表 (ACL) 规则的列表。 可以将 Nsg 与子网或该子网中的各个 VM 实例相关联。 当 NSG 与子网关联时，ACL 规则应用到该子网中的所有 VM 实例。
在本指南中，我们将创建两个 Nsg： 分别用于内部网络和外围网络。 他们将为 NSG_INT 和 NSG_DMZ 分别。

![创建 NSG](./media/how-to-connect-fed-azure-adfs/creatensg1.png)

创建 NSG 后，将 0 的入站和 0 出站规则。 安装并正常工作在各自的服务器角色后，可以根据所需的安全级别进行的入站和出站规则。

![初始化 NSG](./media/how-to-connect-fed-azure-adfs/nsgint1.png)

创建 Nsg 后，将 NSG_INT 与子网关联 INT 并将 NSG_DMZ 与外围网络子网。 下面给出了示例屏幕截图：

![NSG 配置](./media/how-to-connect-fed-azure-adfs/nsgconfigure1.png)

* 单击要打开的面板的子网的子网
* 选择要将 NSG 与相关联的子网 

完成配置后，子网面板应看到如下所示：

![子网创建 NSG 后](./media/how-to-connect-fed-azure-adfs/nsgconfigure2.png)

**1.3.创建连接到本地**

若要将部署在 azure 中的域控制器 (DC)，我们将需要连接到本地。 Azure 提供了多种连接选项用于你的本地基础结构连接到 Azure 基础结构。

* 点到站点
* 站点到站点虚拟网络
* ExpressRoute

建议使用 ExpressRoute。 ExpressRoute 允许你的 Azure 数据中心与你的本地环境或共同租用环境中的基础结构之间创建专有连接。 ExpressRoute 连接并不绕过公共 Internet。 它们通过 Internet 提供更多的可靠性、 更快的速度、 更低的延迟和典型连接相比，更高的安全性。
尽管建议使用 ExpressRoute，你可以选择最适合你的组织的任何连接方法。 若要了解有关 ExpressRoute 的详细信息和使用 ExpressRoute 的各种连接选项，请阅读[ExpressRoute 技术概述](https://aka.ms/Azure/ExpressRoute)。

### <a name="2-create-storage-accounts"></a>2.创建存储帐户
为了保持高可用性并避免依赖单个存储帐户，可以创建两个存储帐户。 将每个可用性组中的计算机划分为两个组，然后将每个组分配单独的存储帐户。

![创建存储帐户](./media/how-to-connect-fed-azure-adfs/storageaccount1.png)

### <a name="3-create-availability-sets"></a>3.创建可用性集
对于每个角色 （DC/AD FS 和 WAP），创建可用性集，将包含 2 个计算机每个最小值。 这将有助于实现更高可用性的每个角色。 在创建可用性集时很重要以下决定：

* **容错域**:在同一个容错域中的虚拟机共享同一个电源和物理网络交换机。 建议使用最少 2 个容错域。 默认值为 3 时，可以保留它进行此部署本文所述
* **更新域**:属于同一更新域的计算机一起重新启动更新过程。 你想要具有 2 个更新域的最小值。 默认值为 5，可将其进行此部署本文所述

![可用性集](./media/how-to-connect-fed-azure-adfs/availabilityset1.png)

创建以下可用性集

| 可用性集 | 角色 | 容错域 | 更新域 |
|:---:|:---:|:---:|:--- |
| contosodcset |DC/ADFS |3 |5 |
| contosowapset |WAP |3 |5 |

### <a name="4-deploy-virtual-machines"></a>4.部署虚拟机
下一步是部署将承载您的基础结构中的不同角色的虚拟机。 建议使用最少两台计算机中每个可用性集。 创建为基本部署的四个虚拟机。

| 计算机 | 角色 | 子网 | 可用性集 | 存储帐户 | IP 地址 |
|:---:|:---:|:---:|:---:|:---:|:---:|
| contosodc1 |DC/ADFS |INT |contosodcset |contososac1 |Static |
| contosodc2 |DC/ADFS |INT |contosodcset |contososac2 |Static |
| contosowap1 |WAP |外围网络 |contosowapset |contososac1 |Static |
| contosowap2 |WAP |外围网络 |contosowapset |contososac2 |Static |

您可能已经注意到，尚未指定任何 NSG。 这是因为 azure 可让你可以在子网级别使用 NSG。 然后，可以通过使用与是子网，否则 NIC 对象关联的单个 NSG 来控制计算机的网络流量。 详细了解[什么是网络安全组 (NSG)](https://aka.ms/Azure/NSG)。
如果要管理 DNS，建议使用静态 IP 地址。 可以使用 Azure DNS，并改为在你的域的 DNS 记录，是指由 Azure Fqdn 的新计算机。
虚拟机窗格应看到如下所示部署完成后：

![部署的虚拟机](./media/how-to-connect-fed-azure-adfs/virtualmachinesdeployed_noadfs.png)

### <a name="5-configuring-the-domain-controller--ad-fs-servers"></a>5.配置域控制器 / AD FS 服务器
 为了对任何传入的请求进行身份验证，AD FS 将需要联系域控制器。 若要从 Azure 的成本高昂的行程保存到本地 DC 进行身份验证中，建议部署在 Azure 中的域控制器的副本。 为了实现高可用性，建议创建至少 2 个域控制器的可用性集。

| 域控制器 | 角色 | 存储帐户 |
|:---:|:---:|:---:|
| contosodc1 |副本 |contososac1 |
| contosodc2 |副本 |contososac2 |

* 将两台服务器提升为使用 DNS 的副本域控制器
* 安装 AD FS 角色使用服务器管理器配置 AD FS 服务器。

### <a name="6-deploying-internal-load-balancer-ilb"></a>6.部署内部负载均衡器 (ILB)
**6.1.创建 ILB**

若要部署 ILB，选择负载均衡器在 Azure 门户上的单击加号 （+）。

> [!NOTE]
> 如果没有看到**负载均衡器**在菜单中，单击**浏览**左下角的门户和滚动，直至看到**负载均衡器**。  然后，单击黄色星号将其添加到你的菜单。 现在，选择新建负载均衡器图标以打开面板，并开始配置负载均衡器。
> 
> 

![浏览负载均衡器](./media/how-to-connect-fed-azure-adfs/browseloadbalancer.png)

* **名称**：为负载均衡器的任何合适的名称
* **方案**:由于此负载均衡器将放置在 AD FS 服务器的前面，并且适用于仅限内部的网络连接，选择"内部"
* **虚拟网络**:选择你在其中部署 AD FS 的虚拟网络
* **子网**：选择此处的内部子网
* **IP 地址分配**:Static

![内部负载均衡器](./media/how-to-connect-fed-azure-adfs/ilbdeployment1.png)

单击后创建和部署 ILB，应在负载均衡器列表中看到它：

![创建 ILB 后的负载均衡器](./media/how-to-connect-fed-azure-adfs/ilbdeployment2.png)

下一步是配置后端池和后端探测。

**6.2.配置 ILB 后端池**

在负载均衡器面板中选择新创建的 ILB。 它将打开设置面板。 

1. 从设置面板中选择后端池
2. 在添加后端池面板中，单击添加虚拟机上
3. 将显示一个面板，您可以在其中选择可用性集
4. 选择 AD FS 可用性集

![配置 ILB 后端池](./media/how-to-connect-fed-azure-adfs/ilbdeployment3.png)

**6.3.配置探测**

在 ILB 设置面板中，选择运行状况探测。

1. 单击添加
2. 提供探测详细信息。 **名称**：探测名称 b。 **协议**：HTTP c。 **端口**：80 (HTTP) d. **路径**: / adfs/探测 e。 **间隔**：5 （默认值） – 这是 ILB 探测后端池 f 中的计算机的间隔。 **不正常阈值限制**:2 （默认值） – 这是在其后 ILB 将声明中的后端池的机无响应，并停止向它发送流量的连续探测失败阈值。

![配置 ILB 探测](./media/how-to-connect-fed-azure-adfs/ilbdeployment4.png)

我们将使用在 AD FS 环境中创建运行状况检查的显式 /adfs/probe 终结点位置的完整 HTTPS 路径检查不会发生。  这是大大优于基本端口 443 检查，不能准确反映最新的 AD FS 部署的状态。  对此的详细信息可从 https://blogs.technet.microsoft.com/applicationproxyblog/2014/10/17/hardware-load-balancer-health-checks-and-web-application-proxy-ad-fs-2012-r2/。

**6.4.创建负载均衡规则**

为了有效地平衡流量，应使用负载均衡规则配置 ILB。 若要创建负载均衡规则， 

1. 选择负载均衡规则与 ILB 设置面板
2. 在添加负载均衡规则面板中单击
3. 添加负载均衡规则面板中。 **名称**：提供规则 b 的名称。 **协议**：选择 TCP c。 **端口**：443 d。 **后端端口**:443 e。 **后端池**:选择你创建的 AD FS 群集早期 f 的池。 **探测**:选择前面创建的 AD FS 服务器的探测

![配置 ILB 平衡规则](./media/how-to-connect-fed-azure-adfs/ilbdeployment5.png)

**6.5.更新 ILB 的 DNS**

转到你的 DNS 服务器，并为 ILB 创建 CNAME。 该 CNAME 应指向 ILB 的 IP 地址的 IP 地址与联合身份验证服务。 例如，如果 ILB DIP 地址是 10.3.0.8，而安装的联合身份验证服务是 fs.contoso.com，然后创建指向 10.3.0.8 的 fs.contoso.com 的 CNAME。
这将确保 fs.contoso.com 结束有关的所有通信在 ILB 上和适当地路由。

### <a name="7-configuring-the-web-application-proxy-server"></a>7.配置 Web 应用程序代理服务器
**7.1.配置 Web 应用程序代理服务器以访问 AD FS 服务器**

为了确保 Web 应用程序代理服务器能够访问 ILB 后面的 AD FS 服务器，创建一条记录 %systemroot%\system32\drivers\etc\hosts 中为 ILB。 请注意，可分辨的名称 (DN) 应是联合身份验证服务名称，例如 fs.contoso.com。 和 IP 条目应是 ILB 的 IP 地址 (如示例所示的 10.3.0.8)。

**7.2.安装 Web 应用程序代理角色**

确保 Web 应用程序代理服务器都能够访问 ILB 后面的 AD FS 服务器后，接下来可以安装 Web 应用程序代理服务器。 Web 应用程序代理服务器不需要加入域。 通过选择远程访问角色，两个 Web 应用程序代理服务器上安装 Web 应用程序代理角色。 服务器管理器将引导你完成 WAP 安装。
有关如何部署 WAP 的详细信息，请阅读[安装和配置 Web 应用程序代理服务器](https://technet.microsoft.com/library/dn383662.aspx)。

### <a name="8--deploying-the-internet-facing-public-load-balancer"></a>8.部署面向 Internet （公共） 负载均衡器
**8.1.创建面向 Internet （公共） 负载均衡器**

在 Azure 门户中，选择负载均衡器，然后单击添加。 在创建负载均衡器面板中，输入以下信息

1. **名称**：负载均衡器的名称
2. **方案**:公共 – 此选项告知 Azure，此负载均衡器需要公共地址。
3. **IP 地址**:创建新的 IP 地址 （动态）

![面向 Internet 的负载均衡器](./media/how-to-connect-fed-azure-adfs/elbdeployment1.png)

在部署后，负载均衡器会在负载均衡器列表中。

![负载均衡器列表](./media/how-to-connect-fed-azure-adfs/elbdeployment2.png)

**8.2.向公共 IP 分配 DNS 标签**

单击负载均衡器面板，以显示配置面板中的新创建的负载均衡器条目。 遵循以下步骤来为公共 IP 配置的 DNS 标签：

1. 单击公共 IP 地址。 这将打开的公共 IP 和其设置面板
2. 单击配置
3. 提供的 DNS 标签。 这将成为可从任何位置，例如 contosofs.westus.cloudapp.azure.com 访问的公共 DNS 标签。 您可以解析为外部负载均衡器 (contosofs.westus.cloudapp.azure.com) 的 DNS 标签 （例如 fs.contoso.com) 的联合身份验证服务在外部 DNS 中添加一个条目。

![配置面向 internet 负载均衡器](./media/how-to-connect-fed-azure-adfs/elbdeployment3.png) 

![配置面向 internet 负载均衡器 (DNS)](./media/how-to-connect-fed-azure-adfs/elbdeployment4.png)

**8.3.配置面向 Internet 的 （公共） 负载均衡器后端池** 

请遵循创建内部负载均衡器，来为面向 Internet 的 （公共） 负载均衡器的后端池配置的可用性集为 WAP 服务器所用的相同步骤。 例如，contosowapset。

![配置面向 Internet 负载均衡器的后端池](./media/how-to-connect-fed-azure-adfs/elbdeployment5.png)

**8.4.配置探测**

遵循配置内部负载均衡器来配置 WAP 服务器的后端池的探测所用的相同步骤。

![配置面向 Internet 负载均衡器探测](./media/how-to-connect-fed-azure-adfs/elbdeployment6.png)

**8.5.创建负载均衡规则**

按照配置的负载均衡规则 TCP 443 的 ILB 所用的相同步骤。

![配置面向 Internet 负载均衡器的平衡规则](./media/how-to-connect-fed-azure-adfs/elbdeployment7.png)

### <a name="9-securing-the-network"></a>9.确保网络安全
**9.1.保护内部子网**

总体而言，需要以下规则来有效保护内部子网 （按如下所示的顺序）

| 规则 | 描述 | 流 |
|:--- |:--- |:---:|
| AllowHTTPSFromDMZ |允许从外围网络的 HTTPS 通信 |入站 |
| DenyInternetOutbound |不能访问 internet |出站 |

![INT 访问规则 （入站）](./media/how-to-connect-fed-azure-adfs/nsg_int.png)

<!--
[comment]: <> (![INT access rules (inbound)](./media/how-to-connect-fed-azure-adfs/nsgintinbound.png))
[comment]: <> (![INT access rules (outbound)](./media/how-to-connect-fed-azure-adfs/nsgintoutbound.png))
-->

**9.2.保护外围网络子网**

| 规则 | 描述 | 流 |
|:--- |:--- |:---:|
| AllowHTTPSFromInternet |允许从 internet 到外围网络的 HTTPS |入站 |
| DenyInternetOutbound |所有非 HTTPS 到互联网被阻止 |出站 |

![EXT 访问规则 （入站）](./media/how-to-connect-fed-azure-adfs/nsg_dmz.png)

<!--
[comment]: <> (![EXT access rules (inbound)](./media/how-to-connect-fed-azure-adfs/nsgdmzinbound.png))
[comment]: <> (![EXT access rules (outbound)](./media/how-to-connect-fed-azure-adfs/nsgdmzoutbound.png))
-->

> [!NOTE]
> 如果客户端用户证书身份验证 (进行 clientTLS 身份验证使用 X509 用户证书) 是必需的则 AD FS 需要 TCP 端口 49443 启用为入站访问。
> 
> 

### <a name="10-test-the-ad-fs-sign-in"></a>10.测试 AD FS 单一登录
最简单方法是测试 AD FS 是使用 IdpInitiatedSignon.aspx 页。 若要能够执行操作，需要启用对 AD FS 属性 IdpInitiatedSignOn。 请按照以下步骤来验证 AD FS 设置

1. 运行以下 cmdlet 使用 PowerShell，若要将其设置为已启用的 AD FS 服务器上。
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true 
2. 从任何外部计算机访问 https:\//adfs-server.contoso.com/adfs/ls/IdpInitiatedSignon.aspx。  
3. 您会看到 AD FS 页如下所示：

![测试登录页](./media/how-to-connect-fed-azure-adfs/test1.png)

在成功登录，它将为您提供一条成功消息，如下所示：

![测试成功](./media/how-to-connect-fed-azure-adfs/test2.png)

## <a name="template-for-deploying-ad-fs-in-azure"></a>Azure 中部署 AD FS 的模板
该模板部署一个 6 台计算机安装中，2 每个域控制器、 AD FS 和 WAP。

[Azure 部署模板中的 AD FS](https://github.com/paulomarquesc/adfs-6vms-regular-template-based)

可以使用现有的虚拟网络，也可以在部署此模板的过程中创建新的 VNET。 在部署过程的参数的用法说明下面列出了可用于自定义部署的各种参数。 

| 参数 | 描述 |
|:--- |:--- |
| Location |要将资源部署到，例如 East 我们的区域。 |
| StorageAccountType |创建的存储帐户的类型 |
| VirtualNetworkUsage |指示是否将创建新的虚拟网络，或使用现有 |
| VirtualNetworkName |创建，必需的对于现有或新虚拟网络使用情况的虚拟网络的名称 |
| VirtualNetworkResourceGroupName |指定现有的虚拟网络所在的资源组的名称。 使用现有的虚拟网络时，这就强制参数，以便部署可以找到现有虚拟网络的 ID |
| VirtualNetworkAddressRange |新 VNET，如果创建新的虚拟网络，则为强制的地址范围 |
| InternalSubnetName |内部的子网，必需的对于这两个虚拟网络使用情况选项 （新的或现有的） 的名称 |
| InternalSubnetAddressRange |如果创建新的虚拟网络包含域控制器和 ADFS 服务器，必需的内部子网地址范围。 |
| DMZSubnetAddressRange |如果创建新的虚拟网络包含 Windows 应用程序代理服务器，必需的外围网络子网地址范围。 |
| DMZSubnetName |内部的子网，必需的对于这两个虚拟网络使用情况选项 （新的或现有的） 的名称。 |
| ADDC01NICIPAddress |第一个域控制器的内部 IP 地址，此 IP 地址将以静态方式分配给 DC，并且必须是内部子网中的有效 ip 地址 |
| ADDC02NICIPAddress |第二个域控制器的内部 IP 地址，此 IP 地址将以静态方式分配给 DC，并且必须是内部子网中的有效 ip 地址 |
| ADFS01NICIPAddress |第一个 ADFS 服务器的内部 IP 地址，此 IP 地址将以静态方式分配给 ADFS 服务器，并且必须是内部子网中的有效 ip 地址 |
| ADFS02NICIPAddress |第二个 ADFS 服务器的内部 IP 地址，此 IP 地址将以静态方式分配给 ADFS 服务器，并且必须是内部子网中的有效 ip 地址 |
| WAP01NICIPAddress |第一个 WAP 服务器的内部 IP 地址，此 IP 地址静态分配给 WAP 服务器，并且必须是外围网络子网中的有效 ip 地址 |
| WAP02NICIPAddress |第二个 WAP 服务器的内部 IP 地址，此 IP 地址静态分配给 WAP 服务器，并且必须是外围网络子网中的有效 ip 地址 |
| ADFSLoadBalancerPrivateIPAddress |ADFS 负载均衡器的内部 IP 地址，此 IP 地址静态分配给负载均衡器和必须是内部子网中的有效 ip 地址 |
| ADDCVMNamePrefix |域控制器的虚拟机名称前缀 |
| ADFSVMNamePrefix |ADFS 服务器的虚拟机名称前缀 |
| WAPVMNamePrefix |WAP 服务器的虚拟机名称前缀 |
| ADDCVMSize |域控制器的 vm 大小 |
| ADFSVMSize |ADFS 服务器的 vm 大小 |
| WAPVMSize |WAP 服务器的 vm 大小 |
| AdminUserName |虚拟机的本地管理员的名称 |
| AdminPassword |虚拟机的本地管理员帐户密码 |

## <a name="additional-resources"></a>其他资源
* [可用性集](https://aka.ms/Azure/Availability) 
* [Azure 负载均衡器](https://aka.ms/Azure/ILB)
* [内部负载均衡器](https://aka.ms/Azure/ILB/Internal)
* [Internet 面向负载均衡器](https://aka.ms/Azure/ILB/Internet)
* [存储帐户](https://aka.ms/Azure/Storage)
* [Azure 虚拟网络](https://aka.ms/Azure/VNet)
* [AD FS 和 Web 应用程序代理链接](https://aka.ms/ADFSLinks) 

## <a name="next-steps"></a>后续步骤
* [将你的本地标识与 Azure Active Directory 集成](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
* [配置和管理使用 Azure AD Connect 中的 AD FS](/azure/active-directory/hybrid/how-to-connect-fed-whatis)
* [Azure 使用 Azure 流量管理器中的高可用性跨地域 AD FS 部署](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
