---
title: 步骤 2 规划基本 DirectAccess 部署
description: 本主题是指南部署单个 DirectAccess 服务器使用获取启动向导为 Windows Server 2016 的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8c21b7fa62246170caeb07cb5865c1ff311e0f09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848748"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>步骤 2 规划基本 DirectAccess 部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

规划 DirectAccess 基础结构之后, 使用基本设置一台服务器上部署 DirectAccess 的下一步是规划开始向导的设置。  
  
|任务|描述|  
|----|--------|  
|规划客户端部署|默认情况下，入门向导将 DirectAccess 部署到所有便携式计算机和笔记本计算机域中的通过将 WMI 筛选器应用于客户端设置 GPO|  
|规划 DirectAccess 服务器部署|规划如何部署 DirectAccess 服务器。|  
  
## <a name="bkmk_2_1_client"></a>规划客户端部署  
在规划客户端部署时，要作出两个决策：  
  
1.  DirectAccess 仅对移动计算机可用，还是对任何一台计算机都可用？  
  
    在中开始向导配置 DirectAccess 客户端，您可以选择仅允许移动计算机使用 DirectAccess 进行连接的指定的安全组中。 如果你限制对移动计算机访问，DirectAccess 会自动配置 WMI 筛选器以确保 DirectAccess 客户端 GPO 仅应用于指定的安全组中的移动计算机。 DirectAccess 管理员需要权限才能创建或修改组策略 WMI 筛选器，以启用此设置。  
  
2.  哪些安全组将包含 DirectAccess 客户端计算机？  
  
    DirectAccess 设置包含在 DirectAccess 客户端 GPO 中。 GPO 将应用于在入门向导中指定的安全组的一部分的计算机。 你可以指定受支持的任何域中包含的安全组。 在配置 DirectAccess 之前，应创建安全组。 您可以完成 DirectAccess 部署后将计算机添加到安全组，但请注意，是否您添加到安全组不同的域中的客户端计算机，则客户端 GPO 将不会应用到这些客户端。 例如，如果在域 A 中为 DirectAccess 客户端创建 SG1，之后将客户端从域 B 添加到该组，则客户端 GPO 不会应用于域 B 中的客户端。若要避免此问题，请为每个包含客户端计算机的域创建一个新的客户端安全组。 另外，如果你不希望创建一个新的安全组，则可使用新域的新 GPO 名称运行 Add-DAClient cmdlet。  
  
## <a name="bkmk_2_2_server"></a>规划 DirectAccess 服务器部署  
有大量的决策可以在规划部署 DirectAccess 服务器时使用：  
  
-   **网络拓扑**-有两种拓扑可部署 DirectAccess 服务器时：  
  
    -   **两个适配器**-通过两个网络适配器，可以使用一个网络适配器直接连接到 Internet，配置 DirectAccess 和其他已连接到内部网络。 或者将该服务器安装在边缘设备（如防火墙或路由器）的后面。 在此配置中，将一个网络适配器连接到外围网络，另一个连接到内部网络。  
  
    -   **单个网络适配器**-在此配置将 DirectAccess 服务器安装在边缘设备，例如防火墙或路由器后面。 网络适配器连接到内部网络。  
  
-   **网络适配器**-DirectAccess 向导会自动检测 DirectAccess 服务器上配置的网络适配器。 您可以确保从是否选择了正确的适配器**评审**页。  
  
-   **IP-HTTPS 证书**-由于未在此部署所需的 PKI，该向导会自动为 IP-HTTPS 和网络位置服务器 （如果不不存在任何证书），预配自签名的证书，并会自动启用Kerberos 代理。 向导还会仅使用 IPv4 的环境中的协议转换启用 NAT64 和 DNS64。 向导成功完成应用配置后，单击 **“关闭”**。  
  
-   **Windows 7 客户端**-无法启用对从入门向导的 Windows 7 客户端的支持。 可以从高级安装向导中启用此项。 有关更多详细信息，请参阅[部署单个 DirectAccess 服务器使用高级设置](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
-   **VPN 配置**-配置 DirectAccess 之前，确定是否要为远程客户端提供 VPN 访问。 如果必须在组织中不支持 DirectAccess 连接的客户端计算机应该提供 VPN 访问 （无论是因为它们不受管理或运行不支持 DirectAccess 的操作系统）。 开始向导配置 VPN IP 地址分配使用 DHCP，并配置 VPN 客户端以使用 Active Directory 进行身份验证。  
  
-   **强制隧道**-如果计划使用强制隧道，或者可能在将来添加它，则应使用[部署单个 DirectAccess 服务器使用高级设置](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)部署双隧道配置。 因为出于安全考虑，不支持强制隧道在单一隧道配置中。  
  
## <a name="BKMK_Links"></a>上一步  
  
-   [步骤 1：规划基本 DirectAccess 基础结构](da-basic-plan-s1-infrastructure.md)  
  


