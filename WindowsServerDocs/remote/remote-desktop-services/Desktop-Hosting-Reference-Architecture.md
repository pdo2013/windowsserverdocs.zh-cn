---
title: 桌面托管参考体系结构
description: 使用 RDS 和 Azure 创建桌面托管解决方案的体系结构指南。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 11/02/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1bac5dd3-8430-46ee-8bef-10cc4b7cc437
author: lizap
manager: dongill
ms.openlocfilehash: b325117c6fecc41bc91fc4384a663c4112d9ddca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387896"
---
# <a name="desktop-hosting-reference-architecture"></a>桌面托管参考体系结构

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

本文定义了一组体系结构块，用于使用远程桌面服务 (RDS) 和 Microsoft Azure 虚拟机创建多租户的托管 Windows 桌面和应用程序服务（我们称之为“桌面托管”）。 可以使用此体系结构参考来为拥有 5 到 5000 个用户的中小型组织创建高度安全、可缩放和可靠的桌面托管解决方案。    
  
此参考体系结构的主要受众是以下类型的托管提供商：他们希望利用 Microsoft Azure 基础结构服务通过 [Microsoft 服务提供商许可协议](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx) (SPLA) 计划向多个租户交付桌面托管服务和订阅者访问许可证 (SAL)。 此参考体系结构的次要受众是希望使用 [RDS 用户 CAL 通过软件保障 (SA) 扩展权限](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf)在 Microsoft Azure 基础结构服务中为自己的员工创建和管理桌面托管解决方案的最终客户。   
  
若要交付桌面托管解决方案，托管合作伙伴和 SA 客户可利用 Windows Server 向 Windows 用户交付企业用户和消费者所熟悉的应用程序体验。 基于 Windows 10，Windows Server 2016 提供熟悉的应用程序支持和用户体验。    
  
本文档范围仅限于：   
  
* 桌面托管服务的体系结构设计指南。 部署过程、性能和容量计划等详细信息在单独文档中说明。 有关 Azure 基础结构服务的更多常规信息，请参阅 [Microsoft Azure 虚拟机](https://azure.microsoft.com/documentation/services/virtual-machines/)。   
  
* 使用 Windows Server 2016 远程桌面会话主机（RD 会话主机）的基于会话的桌面、 RemoteApp 应用程序和基于服务器的个人桌面。 不包括基于 Windows 客户端的虚拟桌面基础结构，因为没有针对 Windows 客户端操作系统的服务提供商许可协议 (SPLA)。 在 SPLA 下，允许使用基于 Windows Server 的虚拟桌面基础结构，在特定场景中，允许在具有最终客户许可证的专用硬件上使用基于 Windows 客户端的虚拟桌面基础结构。 但是，基于客户端的虚拟桌面基础结构在本文档范围外。   
  
* Microsoft 产品和功能，主要包括 Windows Server 2016 和 Microsoft Azure 基础结构服务。   
  
* 桌面托管服务的租户规模从 5 到 5000 个用户不等。   对于较大租户，可能需要修改此体系结构，以提供足够性能。 对于超过 500 个用户的部署，不建议使用服务器管理器 RDS 图形用户界面 (GUI)。 对于管理介于 500 和 5000 之间的用户的 RDS 部署，建议使用 PowerShell。   
  
* 桌面托管服务所需的最小组件和服务集。 可以添加许多可选组件和服务来增强桌面托管服务，但这些超出了本文档的范围。    
  
阅读本文档后，读者应了解：   
- 提供基于 Microsoft Azure 服务的安全、可靠、多租户桌面托管解决方案所需的构建基块。  
- 每个构建基块的用途以及它们的组合方式。  
  
有多种方法来构建基于此体系结构的桌面托管解决方案。 此体系结构概述了 Azure 与 Windows Server 2016 的集成和改进。 其他部署选项也可用于 Windows Server 2012 R2 [桌面托管参考体系结构指南](https://go.microsoft.com/fwlink/p/?LinkId=517389)。    
  
论述了以下主题：  
- [桌面托管逻辑体系结构](Desktop-hosting-logical-architecture.md)  
- [了解 RDS 角色](Understanding-RDS-roles.md)
- [了解桌面托管环境](Understanding-the-desktop-hosting-environment.md)  
- [Azure 服务和桌面托管注意事项](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


