---
title: 通过 Azure 流量管理器在 Azure 中部署高可用性跨地理 AD FS 部署 |Microsoft Docs
description: 在本文档中，你将了解如何在 Azure 中部署 AD FS 以实现高可用性。
keywords: 带有 Azure 流量管理器的 Ad fs、包含 Azure 流量管理器的 adfs、地理位置、多数据中心、地理数据中心、多地理数据中心、在 azure 中部署 AD FS、部署 adfs、部署 AD fs、azure ad fs、在 azure 中部署 adfs，在 azure 中部署 AD FS，adfs AD FS azure，azure，azure，AD FS Azure，iaas，ADFS，将 adfs 移到 azure
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: d98eb126513d707bce7abe3e901c8bf584d2319c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868017"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>通过 Azure 流量管理器在 Azure 中跨地理位置 AD FS 部署的高可用性
[Azure 中的 AD FS 部署](how-to-connect-fed-azure-adfs.md)提供了有关如何在 azure 中为组织部署简单的 AD FS 基础结构的分步指南。 本文提供了使用[Azure 流量管理器](https://docs.microsoft.com/azure/traffic-manager/)在 azure 中创建 AD FS 跨地域部署的后续步骤。 Azure 流量管理器通过使用一系列可用于满足基础结构不同需求的路由方法，帮助创建在地理上分散高可用性和高性能的 AD FS 基础结构。

高可用跨地理位置 AD FS 基础结构可实现以下功能：

* **消除单点故障：** 使用 Azure 流量管理器的故障转移功能，即使在地球的某个数据中心出现故障的情况下，也可以实现高可用性的 AD FS 基础结构。
* **提高了性能：** 可以使用本文中建议的部署来提供高性能 AD FS 基础结构，从而帮助用户更快地进行身份验证。 

## <a name="design-principles"></a>设计原则
![整体设计](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

基本设计原则将与在 Azure 中 AD FS 部署一文中的设计原则中列出的相同。 上图显示了基本部署到另一个地理区域的简单扩展。 下面是将部署扩展到新地理区域时要考虑的一些要点

* **虚拟网络：** 你应在要部署其他 AD FS 基础结构的地理区域中创建新的虚拟网络。 在上面的关系图中，你将看到 Geo1 VNET 和 Geo2 VNET 作为每个地理区域中的两个虚拟网络。
* **新地理 VNET 中的域控制器和 AD FS 服务器：** 建议将域控制器部署到新的地理区域，以便新区域中的 AD FS 服务器不必联系另一远离网络的域控制器来完成身份验证，从而提高性能。
* **存储帐户：** 存储帐户与某个区域关联。 由于你将在新的地理区域中部署计算机，因此你必须创建新的存储帐户才能在该区域中使用。  
* **网络安全组：** 作为存储帐户，在某个区域中创建的网络安全组不能在另一个地理区域中使用。 因此，需要为新地理区域中的 INT 和外围网络子网创建类似于第一个地理区域中的新网络安全组。
* **公共 IP 地址的 DNS 标签：** Azure 流量管理器只能通过 DNS 标签引用终结点。 因此，需要为外部负载均衡器的公共 IP 地址创建 DNS 标签。
* **Azure 流量管理器：** Microsoft Azure 流量管理器允许你控制将用户流量分发到在世界各地不同数据中心运行的服务终结点。 Azure 流量管理器在 DNS 级别工作。 它使用 DNS 响应将最终用户流量定向到全球分布的终结点。 然后，客户端直接连接到这些终结点。 对于性能、加权和优先级的不同路由选项，可以轻松选择最适合组织需求的路由选项。 
* **在两个区域之间的 v-net 到 V 的网络连接：** 不需要在虚拟网络本身之间建立连接。 由于每个虚拟网络都有权访问域控制器，并且它本身具有 AD FS 和 WAP 服务器，因此它们可以在不同区域的虚拟网络之间进行任何连接。 

## <a name="steps-to-integrate-azure-traffic-manager"></a>集成 Azure 流量管理器的步骤
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>在新的地理区域部署 AD FS
遵循在[Azure 中 AD FS 部署](how-to-connect-fed-azure-adfs.md)中的步骤和指导原则，在新的地理区域中部署相同的拓扑。

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>面向 Internet 的（公共）负载均衡器的公共 IP 地址的 DNS 标签
如上所述，Azure 流量管理器只能将 DNS 标签作为终结点引用，因此必须为外部负载均衡器的公共 IP 地址创建 DNS 标签。 下面的屏幕截图显示了如何为公共 IP 地址配置 DNS 标签。 

![DNS 标签](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>部署 Azure 流量管理器
按照以下步骤创建流量管理器配置文件。 有关详细信息，还可以参阅[管理 Azure 流量管理器配置文件](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)。

1. **创建流量管理器配置文件：** 为流量管理器配置文件指定唯一名称。 此配置文件的名称是 DNS 名称的一部分，用作流量管理器域名标签的前缀。 名称/前缀将添加到 trafficmanager.net，以创建流量管理器的 DNS 标签。 下面的屏幕截图显示了设置为 mysts 的流量管理器 DNS 前缀，生成的 DNS 标签为 mysts.trafficmanager.net。 
   
    ![流量管理器配置文件创建](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **流量路由方法：** 流量管理器中提供了三种路由选项：
   
   * Priority 
   * 性能
   * 粗
     
     **性能**是实现高响应 AD FS 基础结构的建议选项。 不过，你可以选择最适合你的部署需求的路由方法。 AD FS 功能不受所选路由选项的影响。 有关详细信息，请参阅[流量管理器流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)。 在上面的示例屏幕截图中，可以看到所选的**性能**方法。
3. **配置终结点：** 在 "流量管理器" 页上，单击 "终结点" 并选择 "添加"。 这将打开类似于下面的屏幕截图的 "添加终结点" 页
   
   ![配置终结点](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   对于不同的输入，请遵循以下指导原则：
   
   **类型：** 选择 "Azure 终结点"，因为我们将指向 Azure 公共 IP 地址。
   
   **名称：** 创建要与终结点关联的名称。 这不是 DNS 名称，与 DNS 记录无关。
   
   **目标资源类型：** 选择公共 IP 地址作为此属性的值。 
   
   **目标资源：** 这将提供一个选项，用于从订阅下可用的不同 DNS 标签中进行选择。 选择与要配置的终结点相对应的 DNS 标签。
   
   为希望 Azure 流量管理器将流量路由到的每个地理区域添加终结点。
   有关如何在流量管理器中添加/配置终结点的详细信息和详细步骤，请参阅[添加、禁用、启用或删除终结点](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **配置探测：** 在 "流量管理器" 页上，单击 "配置"。 在 "配置" 页中，需要将监视器设置更改为 "探测 HTTP 端口80和相对路径/adfs/probe"。
   
    ![配置探测](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **确保配置完成后，终结点的状态为 "联机**"。 如果所有终结点都处于 "降级" 状态，则 Azure 流量管理器将尽力尝试路由流量，假设诊断不正确，并且所有终结点都可访问。
   > 
   > 
5. **修改 Azure 流量管理器的 DNS 记录：** 联合身份验证服务应为 Azure 流量管理器 DNS 名称的 CNAME。 在公共 DNS 记录中创建一个 CNAME，以使尝试访问联合身份验证服务的任何人都真正到达 Azure 流量管理器。
   
    例如，要将联合身份验证服务 fs.fabidentity.com 指向流量管理器，需要将 DNS 资源记录更新为以下内容：
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>测试路由并 AD FS 登录
### <a name="routing-test"></a>路由测试
路由的一项非常基本的测试是尝试从每个地理区域中的计算机对联合身份验证服务 DNS 名称进行 ping 操作。 根据选择的路由方法，实际 ping 到的终结点将反映在 ping 显示中。 例如，如果选择了性能路由，则将达到客户端区域最近的终结点。 下面是两个来自两个不同区域客户端计算机的 ping 的快照，一个在 EastAsia 区域，一个在美国西部。 

![路由测试](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS 登录测试
测试 AD FS 的最简单方法是使用 Idpinitiatedsignon.aspx 页。 为了能够执行此操作，需要在 AD FS 属性上启用 Idpinitiatedsignon.aspx。 按照以下步骤验证 AD FS 设置

1. 使用 PowerShell 在 AD FS 服务器上运行以下 cmdlet，以将其设置为 "已启用"。 
   Set-adfsproperties-EnableIdPInitiatedSignonPage $true
2. 从任何外部计算机访问 https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. 应会看到如下所示的 "AD FS" 页：
   
    ![ADFS 测试-身份验证质询](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    成功登录后，它将提供一条成功消息，如下所示：
   
    ![ADFS 测试-身份验证成功](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>相关链接
* [Azure 中的基本 AD FS 部署](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure 流量管理器](https://docs.microsoft.com/azure/traffic-manager/)
* [流量管理器流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>后续步骤
* [管理 Azure 流量管理器配置文件](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [添加、禁用、启用或删除终结点](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

