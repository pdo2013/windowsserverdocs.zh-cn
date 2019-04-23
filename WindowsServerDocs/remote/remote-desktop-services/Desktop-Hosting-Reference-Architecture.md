---
title: 桌面托管参考体系结构
description: 用于创建的桌面托管解决方案使用 RDS 和 Azure 的体系结构指南。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 6f235fd89c34c00601c802f4ea71e440af630169
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890238"
---
# <a name="desktop-hosting-reference-architecture"></a>桌面托管参考体系结构

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本文定义了一组体系结构基块，使用远程桌面服务 (RDS) 和 Microsoft Azure 虚拟机创建多租户，托管 Windows 桌面和应用程序的服务，我们称之为"桌面托管"。 此体系结构引用可用于创建高度安全、 可缩放且可靠的桌面托管为 5 到 5000 用户小型和中等规模组织的解决方案。    
  
此参考体系结构的主要受众托管提供商想要向多个租户通过利用 Microsoft Azure 基础结构服务提供桌面托管服务和订阅者访问许可证 (Sal) [Microsoft 服务提供商许可协议](https://www.microsoft.com/hosting/en/us/licensing/splabenefits.aspx)(SPLA) 计划。 此参考体系结构的第二个访问群体是想要创建和管理自己员工使用 Microsoft Azure 基础结构服务中的桌面托管解决方案的最终客户[RDS 用户 Cal 扩展通过软件的权限保障](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf)(SA)。   
  
若要交付桌面托管解决方案，托管合作伙伴和 SA 客户利用 Windows Server 提供可为业务用户和消费者所熟悉的应用程序体验的 Windows 用户。 Windows 10 的基础上构建，Windows Server 2016 提供了熟悉的应用程序支持和用户体验。    
  
本文档的范围仅限于：   
  
* 桌面托管服务的体系结构设计指南。 详细的信息，例如部署过程、 性能和容量规划是在单独的文档中所述。 有关 Azure 基础结构服务的更多常规信息，请参阅[Microsoft Azure 虚拟机](https://azure.microsoft.com/documentation/services/virtual-machines/)。   
  
* 基于会话的桌面、 RemoteApp 应用程序和基于服务器的个人桌面，使用 Windows Server 2016 远程桌面会话主机 （RD 会话主机）。 因为没有任何服务提供商许可协议 (SPLA) 的 Windows 客户端操作系统不包含在 Windows 客户端基于虚拟桌面基础结构。 Windows 基于服务器的虚拟桌面基础结构允许在 SPLA，和 Windows 客户端基于虚拟桌面基础结构允许最终客户许可证在某些情况下使用的专用硬件上。 但是，基于客户端虚拟桌面基础结构是范围外的本文档。   
  
* Microsoft 产品和功能，主要是 Windows Server 2016 和 Microsoft Azure 基础结构服务。   
  
* 桌面托管服务的租户范围的大小从 5 到 5000 个用户。   对于较大的租户，可能需要修改此体系结构，以提供足够的性能。 服务器管理器 RDS 图形用户界面 (GUI) 建议不要部署超过 500 个用户。 PowerShell 管理 500 和 5000 用户之间的 RDS 部署的建议。   
  
* 组件和服务所需的桌面托管服务的最小集。 有许多可选组件和服务，可以添加增强的桌面托管服务，但这些是范围外的本文档。    
  
阅读本文档之后, 读取器应了解：   
- 提供的安全、 可靠、 多租户的桌面托管解决方案在 Microsoft Azure 服务中基于所需构建基块。  
- 每个构建基块的用途以及它们如何组合在一起。  
  
有多种方法来构建的桌面托管解决方案基于此体系结构。 此体系结构概述了集成和使用 Windows Server 2016 在 Azure 中的改进。 可以使用与其他部署选项[桌面托管参考体系结构指南](https://go.microsoft.com/fwlink/p/?LinkId=517389)适用于 Windows Server 2012 R2。    
  
论述了以下主题：  
- [桌面托管逻辑体系结构](Desktop-hosting-logical-architecture.md)  
- [了解 RDS 角色](Understanding-RDS-roles.md)
- [了解桌面托管环境](Understanding-the-desktop-hosting-environment.md)  
- [Azure 服务和桌面托管注意事项](Azure-services-and-considerations-for-desktop-hosting.md)
  
 


