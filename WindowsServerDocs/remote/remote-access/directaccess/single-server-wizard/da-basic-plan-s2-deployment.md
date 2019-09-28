---
title: 步骤2规划基本 DirectAccess 部署
description: 本主题是使用 Windows Server 2016 的入门向导部署单个 DirectAccess 服务器指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7ddcb162-dd92-406c-acab-d3de7239c644
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3009b6002d9d4cd116795c46305ff02fda02ef63
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388490"
---
# <a name="step-2-plan-the-basic-directaccess-deployment"></a>步骤2规划基本 DirectAccess 部署

>适用于：Windows Server（半年频道）、Windows Server 2016

规划 DirectAccess 基础结构后，在使用基本设置的单个服务器上部署 DirectAccess 的下一步是计划入门向导的设置。  
  
|任务|描述|  
|----|--------|  
|规划客户端部署|默认情况下，入门向导通过将 WMI 筛选器应用到客户端设置 GPO，将 DirectAccess 部署到域中的所有便携式计算机和笔记本计算机|  
|规划 DirectAccess 服务器部署|规划如何部署 DirectAccess 服务器。|  
  
## <a name="bkmk_2_1_client"></a>规划客户端部署  
在规划客户端部署时，要作出两个决策：  
  
1.  DirectAccess 仅对移动计算机可用，还是对任何一台计算机都可用？  
  
    在入门向导中配置 DirectAccess 客户端时，你可以选择仅允许指定安全组中的移动计算机使用 DirectAccess 进行连接。 如果限制对移动计算机的访问，则 DirectAccess 会自动配置 WMI 筛选器，以确保 DirectAccess 客户端 GPO 仅应用于指定安全组中的移动计算机。 DirectAccess 管理员需要创建或修改组策略 WMI 筛选器的权限才能启用此设置。  
  
2.  哪些安全组将包含 DirectAccess 客户端计算机？  
  
    DirectAccess 设置包含在 DirectAccess 客户端 GPO 中。 GPO 应用于属于你在入门向导中指定的安全组的计算机。 你可以指定受支持的任何域中包含的安全组。 在配置 DirectAccess 之前，应创建安全组。 完成 DirectAccess 部署后，你可以将计算机添加到安全组，但请注意，如果将驻留在不同域中的客户端计算机添加到安全组，则客户端 GPO 将不会应用于这些客户端。 例如，如果在域 A 中为 DirectAccess 客户端创建 SG1，之后将客户端从域 B 添加到该组，则客户端 GPO 不会应用于域 B 中的客户端。若要避免此问题，请为每个包含客户端计算机的域创建一个新的客户端安全组。 另外，如果你不希望创建一个新的安全组，则可使用新域的新 GPO 名称运行 Add-DAClient cmdlet。  
  
## <a name="bkmk_2_2_server"></a>规划 DirectAccess 服务器部署  
在规划部署 DirectAccess 服务器时，需要做出许多决策：  
  
-   **网络拓扑**-部署 DirectAccess 服务器时，有两种可用的拓扑：  
  
    -   **两个适配器**-有两个网络适配器，可以配置 DirectAccess，其中一个网络适配器直接连接到 Internet，另一个连接到内部网络。 或者将该服务器安装在边缘设备（如防火墙或路由器）的后面。 在此配置中，将一个网络适配器连接到外围网络，另一个连接到内部网络。  
  
    -   **单网络适配器**-在此配置中，DirectAccess 服务器安装在边缘设备（如防火墙或路由器）的后面。 网络适配器连接到内部网络。  
  
-   **网络适配器**-directaccess 向导会自动检测 DirectAccess 服务器上配置的网络适配器。 可以确保从 "**审阅**" 页中选择正确的适配器。  
  
-   Ip-https**证书**-由于此部署中不需要 PKI，因此向导会自动为 Ip-https 和网络位置服务器设置自签名证书（如果不存在证书），并自动启用 Kerberos代理. 该向导还在仅支持 IPv4 的环境中为协议转换启用 NAT64 和 DNS64。 向导成功完成应用配置后，单击 **“关闭”** 。  
  
-   **Windows 7 客户端**-无法从入门向导启用对 Windows 7 客户端的支持。 可以从高级安装向导启用此功能。 有关更多详细信息，请参阅[使用高级设置部署单个 DirectAccess 服务器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
-   **VPN 配置**-在配置 DirectAccess 之前，请决定是否要向远程客户端提供 VPN 访问权限。 如果你的组织中的客户端计算机不支持 DirectAccess 连接（因为它们不受管理，或者运行不支持 DirectAccess 的操作系统），则应提供 VPN 访问。 入门向导使用 DHCP 配置 VPN IP 地址分配，并将 VPN 客户端配置为使用 Active Directory 进行身份验证。  
  
-   **强制隧道**-如果你打算使用强制隧道，或者将来可能会添加它，则应该使用[部署具有高级设置的单个 DirectAccess 服务器](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)来部署两个隧道配置。 出于安全考虑，不支持单一隧道配置中的强制隧道。  
  
## <a name="BKMK_Links"></a>上一步  
  
-   [步骤 1：规划基础 DirectAccess 基础结构](da-basic-plan-s1-infrastructure.md)  
  


