---
title: 虚拟专用网 (VPN)
description: 您可以使用本主题来了解 Windows Server 2016 和 Windows 10 VPN 的特性和功能。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 6b647d8cbf9586408b49c1519b57d32e596fce10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404227"
---
# <a name="virtual-private-networking-vpn"></a>虚拟专用网 (VPN)

>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>作为单租户 VPN 服务器的 RAS 网关

在 Windows Server 2016 中，远程访问服务器角色是以下相关网络访问技术的逻辑分组。

- 远程访问服务（RAS）
- 路由
- Web 应用程序代理

这些技术是远程访问服务器角色的角色服务。

使用 "添加角色和功能向导" 或 Windows PowerShell 安装远程访问服务器角色时，可以安装这三个角色服务中的一个或多个。

安装**DirectAccess 和 VPN （ras）** 角色服务时，将部署远程访问服务网关（**ras 网关**）。 可以将 RAS 网关部署为单租户 RAS 网关虚拟专用网络（VPN）服务器，该服务器提供许多高级功能和增强的功能。

>[!NOTE]
>你还可以将 RAS 网关部署为多租户 VPN 服务器，以与软件定义的网络（SDN）或 DirectAccess 服务器一起使用。 有关详细信息，请参阅[RAS 网关](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway)、[软件定义的网络（SDN）](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)和[DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess)。

## <a name="related-topics"></a>相关主题
- [ALWAYS ON VPN 特性和功能](vpn-map-da.md)：本主题介绍 Always On VPN 的特性和功能。 

- [在 Windows 10 中配置 VPN 设备隧道](vpn-device-tunnel-config.md)：Always On VPN 使你能够为设备或计算机创建专用 VPN 配置文件。 Always On VPN 连接包括两种类型的隧道：_设备隧道_和_用户隧道_。 设备隧道用于预登录连接方案和设备管理用途。 用户隧道允许用户通过 VPN 服务器访问组织资源。

- [Always On 适用于 Windows Server 2016 和 windows 10 的 VPN 部署](always-on-vpn/deploy/always-on-vpn-deploy.md)：提供有关将远程访问部署为点到站点 VPN 连接的单租户 VPN RAS 网关的说明，该网关允许你的远程员工使用 Always On VPN 连接连接到你的组织网络。 建议你查看此部署中使用的每项技术的设计和部署指南。

- [Windows 10 VPN 技术指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide)：指导你完成在企业 VPN 解决方案中针对 Windows 10 客户端做出的决策，以及如何配置你的部署。 你可以找到对 VPNv2 配置服务提供程序（CSP）的引用，并使用 Microsoft Intune 和适用于 Windows 10 的 VPN 配置文件模板提供移动设备管理（MDM）配置说明。

- [如何在 System Center Configuration Manager 中创建 VPN 配置文件](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)：本主题介绍如何在 System Center Configuration Manager （SCCM）中创建 VPN 配置文件。

- [配置 Windows 10 客户端 ALWAYS ON VPN 连接](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections)：本主题介绍 ProfileXML 选项和架构，以及如何创建 ProfileXML VPN。 设置服务器基础结构后，你必须将 Windows 10 客户端计算机配置为使用 VPN 连接与该基础结构进行通信。

- [VPN 配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)：本主题介绍 Windows 10 中的 VPN 配置文件设置，并了解如何使用 Intune 或 SCCM 来配置 VPN 配置文件。 你可以使用 VPNv2 CSP 中的 ProfileXML 节点配置 Windows 10 中的所有 VPN 设置。
