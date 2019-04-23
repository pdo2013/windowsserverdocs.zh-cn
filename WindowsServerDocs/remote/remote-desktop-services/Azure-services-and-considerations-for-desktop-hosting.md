---
title: Azure 服务和桌面托管注意事项
description: 了解如何对 Azure 的远程桌面托管解决方案的特定注意事项。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 37210a5d75399309c53364f5b8ee9e06d26d6f32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849798"
---
# <a name="azure-services-and-considerations-for-desktop-hosting"></a>Azure 服务和桌面托管注意事项

>适用于：Windows 服务器 （半年频道），Windows Server 2016

以下各节介绍 Azure 基础结构服务。
  
## <a name="azure-portal"></a>Azure 门户

提供程序创建一个 Azure 订阅后，可以使用 Azure 门户手动创建每个租户的环境。 此外可以使用 PowerShell 脚本自动化此过程。  

有关详细信息，请访问[Microsoft Azure](https://www.azure.microsoft.com)网站。
  
## <a name="azure-load-balancer"></a>Azure 负载均衡器

租户的组件进行通信的虚拟机上运行与每个其他隔离网络上。 在部署过程中，可以通过 Azure 负载均衡器使用远程桌面协议终结点或远程 PowerShell 终结点中从外部访问这些虚拟机。 部署完成后，通常将删除这些终结点以减小攻击面。 唯一的终结点将创建运行 RD Web 和 RD 网关组件的虚拟机的 HTTPS 和 UDP 终结点。 这允许客户端上 internet 连接到运行中租户的桌面托管服务的会话。 如果用户打开的应用程序连接到 internet，例如 web 浏览器中，连接将通过 Azure 负载均衡器。  
  
有关详细信息，请参阅[什么是 Azure 负载均衡器？](https://azure.microsoft.com/documentation/articles/virtual-machines-linux-load-balance/)
  
## <a name="security-considerations"></a>安全注意事项

此 Azure 桌面托管参考体系结构指南 》 旨在为每个租户提供高度安全且隔离的环境。 系统安全还取决于提供程序在部署和托管的服务操作期间执行的安全措施。 以下列表介绍了提供程序应采用以保持其基于安全此参考体系结构的桌面托管解决方案的一些注意事项。

- 理想情况下随机生成，通常情况下，更改并保存在安全的中心位置仅供选择的几个提供程序管理员，则必须是强，所有管理密码。  
- 当复制为新租户的租户环境，请避免使用相同或弱管理密码。
- RD Web 访问站点 URL、 名称和证书必须是唯一的且容易识别的每个租户，以防止电子欺骗攻击。  
- 在桌面托管服务的正常操作时，所有公共 IP 地址应删除除 RD Web 和 RD 网关虚拟机，可让用户安全地连接到租户的桌面托管云服务的所有虚拟机。 有关管理任务，必要时，公共 IP 地址可能会暂时添加，但它们应始终之后删除。  
  
有关详细信息，请参阅以下文章：

- [安全和保护](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831778(v=ws.11))  
- [IIS 8 的最佳安全方案](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj635855(v=ws.11))  
- [Secure Windows Server 2012 R2](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831360(v=ws.11))  
  
## <a name="design-considerations"></a>设计注意事项

请务必在设计多租户的桌面托管服务时，请考虑 Microsoft Azure 基础结构服务的约束。 以下列表介绍该提供程序必须执行实现功能且具有成本效益桌面托管解决方案根据此参考体系结构的注意事项。  
  
- Azure 订阅的虚拟网络、 VM 核心数和可用的云服务的最大数目。 如果提供程序需要比这更多资源，它们可能需要创建多个订阅。
- Azure 云服务具有可用的虚拟机的最大数目。 提供程序可能需要创建多个云服务的较大的租户数超过此上限。  
- Azure 部署成本基于部分的数量和大小的虚拟机。 提供程序应进行优化的数量和每个租户虚拟机的大小，以提供正常运行且高度安全的桌面托管环境以最低的成本。  
- Azure 数据中心中的物理计算机资源是通过使用 HYPER-V 虚拟化。 未在主机群集中配置的 HYPER-V 主机，因此虚拟机的可用性是依赖于使用 Azure 基础结构中的单个服务器的可用性。 若要提供更高的可用性，每个角色服务虚拟机的多个实例可以创建在可用性集中，则来宾群集可实现对虚拟机内部。  
- 在典型的存储配置中，服务提供程序将具有多个容器 （例如，每个租户一个），并在每个容器内的多个磁盘具有的单个存储帐户。 但是，没有限制的总存储空间和单个存储帐户，可以实现的性能。 对于服务提供程序支持大量租户或租户具有高存储容量或性能要求，服务提供商可能需要创建多个存储帐户。  
  
有关详细信息，请参阅以下文章：

- [云服务的大小](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs)  
- [Microsoft Azure 虚拟机定价详细信息](https://azure.microsoft.com/pricing/details/virtual-machines/)  
- [HYPER-V 概述](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))  
- [Azure 存储可伸缩性和性能目标](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)  

## <a name="azure-active-directory-application-proxy"></a>Azure Active Directory 应用程序代理

Azure Active Directory (AD) 应用程序代理是付费 Sku 的 Azure AD，使用户能够连接到通过 Azure 的反向代理服务的内部应用程序中提供的服务。 这样，要隐藏在虚拟网络，无需通过公共 IP 地址向 internet 公开的 RD Web 和 RD 网关终结点。 宿主可以使用 Azure AD 应用程序代理仍保持完整部署时压缩在租户的环境中的虚拟机数。 Azure AD 应用程序代理还支持很多优势，Azure AD 提供，例如条件访问和多重身份验证。

有关详细信息，请参阅[开始使用应用程序代理并安装连接器](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-enable)。