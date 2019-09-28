---
title: Azure 服务和桌面托管注意事项
description: 了解 Azure 特有的注意事项以及远程桌面托管解决方案。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f402ae3-5391-4c7d-afea-2c5c9044de46
author: heidilohr
manager: dougkim
ms.openlocfilehash: 5c3b1ef044be70002918b7ef1379513bdbfb930c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387956"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Azure 服务和桌面托管注意事项

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

以下部分介绍 Azure 基础结构服务。
  
## <a name="azure-portal"></a>Azure 门户

提供程序创建 Azure 订阅后，Azure 门户可用于手动创建每个租户的环境。 还可以使用 PowerShell 脚本自动执行此过程。  

有关详细信息，请参阅 [Microsoft Azure](https://www.azure.microsoft.com) 网站。
  
## <a name="azure-load-balancer"></a>Azure 	负载均衡器

租户的组件运行于在隔离网络上互相通信的虚拟机上。 在部署过程中，可以通过 Azure 负载均衡器使用远程桌面协议终结点或远程 PowerShell 终结点从外部访问这些虚拟机。 部署完成后，通常将删除这些终结点以缩减受攻击的外围应用。 唯一终结点将是为运行 RD Web 和 RD 网关组件的虚拟机创建的 HTTPS 和 UDP 终结点。 这使 Internet 上的客户端可以连接到在租户的桌面托管服务中运行的会话。 如果用户打开连接到 Internet 的应用程序（例如 Web 浏览器），则连接将通过 Azure 负载均衡器。  
  
有关详细信息，请参阅 [Azure 负载均衡器是什么？](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>安全注意事项

此 Azure 桌面托管参考体系结构指南旨在为每个租户提供高度安全且隔离的环境。 系统安全还取决于提供程序在部署和操作托管服务期间执行的安全措施。 以下列表介绍了提供程序为了确保基于此参考体系结构的桌面托管解决方案安全而应遵循的一些注意事项。

- 所有管理密码必须为强密码、随机生成（理想情况下）、频繁更改并保存在仅对精选出来的少数提供程序管理员可见的安全中心位置。  
- 当为新租户复制租户环境时，请避免使用相同或弱管理密码。
- RD Web 访问站点 URL、名称和证书必须是唯一的且容易被每个租户识别，以防止受到欺骗攻击。  
- 在正常操作桌面托管服务期间，应删除所有虚拟机（可让用户安全地连接到租户的桌面托管云服务的 RD Web 和 RD 网关虚拟机除外）的所有公共 IP 地址。 出于管理任务需要，可能会临时添加公共 IP 地址，但随后务必删除。  
  
有关详细信息，请参阅以下文章：

- [安全和保护](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [IIS 8 安全最佳做法](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [保护 Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>设计注意事项

设计多租户桌面托管服务时，请务必考虑 Microsoft Azure 基础结构服务的约束。 以下列表介绍了提供程序为了获得基于此参考体系结构的正常运行且具有成本效益的桌面托管解决方案而必须遵循的注意事项。  
  
- Azure 订阅具有最大数量的可用虚拟网络、VM 核心和云服务。 如果提供程序需要更多资源，则可能需要创建多个订阅。
- Azure 云服务具有最大数量的可用虚拟机。 提供程序可能需要为超过最大数量的租户创建多个云服务。  
- Azure 部署成本部分基于虚拟机的数量和大小。 提供程序应为每个租户优化虚拟机的数量和大小，从而以最低成本提供正常运行且高度安全的桌面托管环境。  
- Azure 数据中心中的物理计算机资源通过使用 Hyper-V 进行虚拟化。 Hyper-V 主机未在主机群集中配置，因此虚拟机的可用性依赖于 Azure 基础结构中使用的各个服务器的可用性。 若要提供更高的可用性，可以在可用性集中创建每个角色服务虚拟机的多个实例，随后即可在虚拟机内实现来宾群集。  
- 在典型的存储配置中，服务提供程序将具有包含多个容器（例如，每个租户有一个容器）的单个存储帐户，其中每个容器内有多个磁盘。 但是，可为单个存储帐户实现的存储总量和性能存在限制。 对于支持大量租户或具有高存储容量或性能要求的租户的服务提供程序，服务提供程序可能需要创建多个存储帐户。  
  
有关详细信息，请参阅以下文章：

- [云服务的大小](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Microsoft Azure 虚拟机定价详细信息](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [Hyper-V 概述](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Azure 存储可伸缩性和性能目标](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory 应用程序代理

Azure Active Directory (AD) 应用程序代理是 Azure AD 的付费 SKU 中提供的一项服务，这些 SKU 使用户能够通过 Azure 自身的反向代理服务连接到内部应用程序。 这样，RD Web 和 RD 网关终结点可以隐藏在虚拟网络内，而无需通过公共 IP 地址向 Internet 公开。 主机托管服务提供商可以使用 Azure AD 应用程序代理减少租户环境中的虚拟机数，同时仍保持完整部署。 Azure AD 应用程序代理还支持 Azure AD 提供的很多优势，例如条件访问和多重身份验证。

有关详细信息，请参阅[开始使用应用程序代理并安装连接器](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable)。