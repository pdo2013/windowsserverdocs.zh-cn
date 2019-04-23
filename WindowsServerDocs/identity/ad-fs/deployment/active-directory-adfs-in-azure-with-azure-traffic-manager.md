---
title: 在 Azure 与 Azure 流量管理器中的部署高可用性跨地域 AD FS |Microsoft Docs
description: 在本文档中，您将学习如何将 Azure 中的 AD FS 部署以实现高可用性。
keywords: Ad fs 与 Azure 流量管理器中，使用 Azure 流量管理器、 地理、 多数据中心、 地理数据中心、 多地理数据中心，adfs 部署在 azure 中的 AD FS、 部署 azure adfs，azure adfs，azure ad fs、 部署 adfs，部署 ad fs，adfs，在 azure 中，部署 adfs，在 azure 中，azure，adfs azure，AD FS，Azure，在 Azure 中 iaas，ADFS，AD FS 简介中部署 AD FS 将 adfs 移到 azure
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
ms.openlocfilehash: 4af0386ef97694baa9ba7f1e5e7163554d79734a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874118"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Azure 使用 Azure 流量管理器中的高可用性跨地域 AD FS 部署
[在 Azure 中的部署 AD FS](how-to-connect-fed-azure-adfs.md)提供了有关如何为你的组织在 Azure 中部署简单 AD FS 基础结构的分步指导。 本文提供了后续步骤来创建在 Azure 中使用的跨地理部署 AD FS [Azure 流量管理器](https://docs.microsoft.com/azure/traffic-manager/)。 Azure 流量管理器可帮助创建地理上分散的高可用性和高性能 AD FS 基础结构为你的组织，从而使用范围的路由方法可用于满足不同需求从基础结构。

高可用性跨地域 AD FS 基础结构能够：

* **消除了单一故障点：** 故障转移功能的 Azure 流量管理器中，使用时可实现高度可用的 AD FS 基础结构甚至在全球范围内的部分中的数据中心之一出现故障
* **改进的性能：** 可以使用本文中建议的部署提供高性能 AD FS 基础结构，可帮助用户更快地进行身份验证。 

## <a name="design-principles"></a>设计原则
![整体设计](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

基本设计原则将如下所示在 Azure 中部署 AD FS 的文章中的设计原则相同。 上图显示了基本部署到另一个地理区域的简单扩展。 下面是将部署扩展到新地理区域时要考虑的一些要点

* **虚拟网络：** 应在你想要部署其他 AD FS 基础结构的地理区域中创建新的虚拟网络。 在上面的关系图中每个地理区域的两个虚拟网络作为看到 Geo1 VNET 和 Geo2 VNET。
* **域控制器和新的地理 VNET 中的 AD FS 服务器：** 建议部署域控制器中的新的地理区域，以便在新区域中的 AD FS 服务器无需联系中另一个距离遥远的域控制器网络，以完成身份验证，从而改进性能。
* **存储帐户：** 存储帐户是与区域关联。 由于你将部署新的地理区域中的计算机，您必须创建新的存储帐户的区域中使用。  
* **网络安全组：** 因为不能在另一个地理区域中使用的存储帐户，在区域中创建的网络安全组。 因此，需要创建新的网络安全组类似的 INT 和外围网络子网的第一个地理区域中的新的地理区域。
* **公共 IP 地址的 DNS 标签：** Azure 流量管理器可以指终结点仅通过 DNS 标签。 因此，需要为外部负载均衡器的公共 IP 地址创建 DNS 标签。
* **Azure 流量管理器：** Microsoft Azure 流量管理器可以控制向世界各地不同数据中心中运行的服务终结点的用户流量分布。 Azure 流量管理器在 DNS 级别工作。 它使用 DNS 响应将最终用户流量定向到全球分布的终结点。 客户端然后直接连接到这些终结点。 使用不同的性能、 加权和优先级的路由选项，你可以轻松选择最适合你组织的需求的路由选项。 
* **两个区域之间的虚拟网络到虚拟网络连接：** 不需要具有虚拟网络之间的连接本身。 由于每个虚拟网络有权访问域控制器，并且本身具有 AD FS 和 WAP 服务器，它们可以处理而不同的区域中的虚拟网络之间的任何连接。 

## <a name="steps-to-integrate-azure-traffic-manager"></a>集成 Azure 流量管理器的步骤
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>在新地理区域中部署 AD FS
遵循的步骤和中的指导原则[在 Azure 中的部署 AD FS](how-to-connect-fed-azure-adfs.md)以部署新的地理区域中相同的拓扑。

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>面向 Internet 的 （公共） 负载均衡器的公共 IP 地址的 DNS 标签
如上所述，Azure 流量管理器只能引用的 DNS 标签用作终结点，因此它必须为外部负载均衡器的公共 IP 地址创建 DNS 标签。 下面的屏幕快照显示了如何配置的公共 IP 地址的 DNS 标签。 

![DNS 标签](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>部署 Azure 流量管理器
请按照以下步骤来创建流量管理器配置文件。 有关详细信息，您还可以指[管理 Azure 流量管理器配置文件](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)。

1. **创建流量管理器配置文件：** 为流量管理器配置文件指定唯一名称。 此配置文件的名称是 DNS 名称的一部分，并且可作为前缀的流量管理器域名标签。 名称 / 前缀添加到.trafficmanager.net，创建 DNS 标签，流量管理器。 下面的屏幕截图显示了流量管理器设置为 mysts 的 DNS 前缀和生成的 DNS 标签是 mysts.trafficmanager.net。 
   
    ![创建流量管理器配置文件](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **流量路由方法：** 流量管理器中有三个路由选项：
   
   * Priority 
   * 性能
   * 加权
     
     **性能**是推荐的选项，以实现高响应 AD FS 基础结构。 但是，可以选择最适合您的部署需要的任何路由方法。 AD FS 功能不受所选路由选项。 请参阅[流量管理器流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)有关详细信息。 在此示例中可以看到上面的屏幕截图**性能**选择方法。
3. **配置终结点：** 在流量管理器页中，单击终结点，并选择添加。 这将打开添加终结点页面类似于下面的屏幕截图
   
   ![配置终结点](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   不同的输入，请遵循以下指南：
   
   **类型：** 选择 Azure 终结点，因为我们将指向 Azure 公共 IP 地址。
   
   **名称：** 创建你想要将与该终结点相关联的名称。 这不是 DNS 名称和 DNS 记录没有任何影响。
   
   **目标资源类型：** 选择公共 IP 地址作为此属性的值。 
   
   **目标资源：** 这将为您提供选择从不同的 DNS 标签，必须在订阅下可用的选项。 选择对应于正在配置的终结点的 DNS 标签。
   
   添加所需 Azure 流量管理器将流量路由到每个地理区域的终结点。
   有关详细信息和如何添加 / 配置终结点，流量管理器的详细的步骤，请参阅[添加、 禁用、 启用或删除终结点](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints)
4. **配置探测：** 在流量管理器页中，单击配置。 在配置页中，您需要更改监视设置，以在 HTTP 端口 80 和相对路径 /adfs/probe 探测
   
    ![配置探测](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **确保配置完成后，终结点的状态都将为联机**。 如果所有终结点都处于降级状态，Azure 流量管理器将执行尽量尝试路由流量，但前提是诊断不正确并且所有终结点可以访问。
   > 
   > 
5. **有关 Azure 流量管理器中修改 DNS 记录：** 联合身份验证服务应是 Azure 流量管理器 DNS 名称的 CNAME。 在公共 DNS 记录中创建 CNAME，以便无论谁正在尝试访问的联合身份验证服务实际上到达 Azure 流量管理器。
   
    例如，要将指向联合身份验证服务 fs.fabidentity.com 到流量管理器中，将需要 DNS 资源记录进行以下更新：
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>测试路由和 AD FS 登录
### <a name="routing-test"></a>路由测试
用于路由的基本测试包括尝试从每个地理区域中的机联合身份验证服务 DNS 名称进行 ping 操作。 具体取决于选择的路由方法，实际 ping 的终结点都反映在 ping 显示。 例如，如果您选择性能路由，则最靠近的区域的客户端终结点将会到达。 下面是两个 ping 命令来自两个不同区域客户端计算机的快照，一个位于东亚区域，一个位于美国西部。 

![路由测试](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS 登录测试
若要测试 AD FS 的最简单方法是使用 IdpInitiatedSignon.aspx 页。 若要能够执行操作，需要启用对 AD FS 属性 IdpInitiatedSignOn。 请按照以下步骤来验证 AD FS 设置

1. 运行以下 cmdlet 使用 PowerShell，若要将其设置为已启用的 AD FS 服务器上。 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. 从任何外部计算机访问 https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx
3. 您会看到 AD FS 页如下所示：
   
    ![ADFS 测试-身份验证质询](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    并在成功登录，它将为您提供一条成功消息，如下所示：
   
    ![ADFS 测试-身份验证成功](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>相关链接
* [在 Azure 中的基本 AD FS 部署](how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/)
* [流量管理器流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods)

## <a name="next-steps"></a>后续步骤
* [管理 Azure 流量管理器配置文件](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-profiles)
* [添加、 禁用、 启用或删除终结点](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-manage-endpoints) 

