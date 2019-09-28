---
title: 使用入门向导部署单台 DirectAccess 服务器
description: 本主题是使用 Windows Server 2016 的入门向导部署单个 DirectAccess 服务器指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb0cf464-0668-40f8-8222-feb6bae6d3d5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d927971094396a8df44eece80732a9d7a301ed33
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388610"
---
# <a name="deploy-a-single-directaccess-server-using-the-getting-started-wizard"></a>使用入门向导部署单台 DirectAccess 服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍使用单台 DirectAccess 服务器的 DirectAccess 方案，并让你能够通过几个简单的步骤部署 DirectAccess。  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>在开始部署之前，请参阅不受支持的配置、已知问题和先决条件的列表  
在部署 DirectAccess 之前，可以使用以下主题查看先决条件和其他信息。  
  
-   [DirectAccess 不受支持的配置](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [部署 DirectAccess 的先决条件](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>方案描述  
在此方案中，在几个简单的向导步骤中，运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的单台计算机配置为 DirectAccess 服务器，而无需配置基础结构设置，例如作为证书颁发机构（CA）或 Active Directory 安全组。  
  
> [!NOTE]  
> 如果想要使用自定义设置配置高级部署，请参阅 [Deploy a Single DirectAccess Server with Advanced Settings](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
  
## <a name="in-this-scenario"></a>本方案内容  
若要设置基本 DirectAccess 服务器，需要执行多个规划和部署步骤。  
  
### <a name="prerequisites"></a>先决条件  
在开始部署此方案之前，请查看此列表以了解重要要求：  
  
-   必须在所有配置文件上启用 Windows 防火墙  
  
-   仅当客户端计算机运行的是 Windows 10、Windows 8.1 或 Windows 8 时，才支持此方案。  
  
-   企业网络中的 ISATAP 不受支持。 如果你使用的是 ISATAP，应该将其删除并使用本机 IPv6。  
  
-   不需要公钥基础结构。  
  
-   不支持部署双重身份验证。 身份验证需要域凭据。  
  
-   自动将 DirectAccess 部署到当前域中的所有移动计算机。  
  
-   到 Internet 的流量不通过 DirectAccess 隧道。 不支持强制隧道配置。  
  
-   DirectAccess 服务器是网络位置服务器。  
  
-   不支持网络访问保护 (NAP)。  
  
-   不支持在 DirectAccess 管理控制台或 PowerShell cmdlet 之外更改策略。  
  
-   若要在以后或将来部署多站点，请先[部署具有高级设置的单个 DirectAccess 服务器](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
### <a name="planning-steps"></a>规划步骤  
规划分成以下两个阶段：  
  
1.  规划 DirectAccess 基础结构。 本阶段描述在开始 DirectAccess 部署之前设置网络基础结构所需的规划。 它包括规划网络和服务器拓扑以及 DirectAccess 网络位置服务器。  
  
2.  规划 DirectAccess 部署。 本阶段描述准备 DirectAccess 部署所需的规划步骤。 它包括规划 DirectAccess 客户端计算机、服务器和客户端身份验证要求、VPN 设置、基础设施服务器以及管理和应用程序服务器。  
  
有关详细的规划步骤，请参阅[规划高级 DirectAccess 部署](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md)。  
  
### <a name="deployment-steps"></a>部署步骤  
部署分成以下三个阶段：  
  
1.  配置 DirectAccess 基础结构-此阶段包括配置网络和路由、配置防火墙设置（如果需要）、配置证书、DNS 服务器、Active Directory 和 GPO 设置以及 DirectAccess 网络位置服务.  
  
2.  配置 DirectAccess 服务器设置。 本阶段包括用于配置 DirectAccess 客户端计算机、DirectAccess 服务器、基础结构服务器、管理和应用程序服务器的步骤。  
  
3.  验证部署。 此阶段包括验证部署是否按要求工作的步骤。  
  
有关详细的部署步骤，请参阅 [Install and Configure Basic DirectAccess](../../../remote-access/directaccess/single-server-wizard/Install-and-Configure-Basic-DirectAccess.md)。  
  
## <a name="BKMK_APP"></a>实用应用程序  
部署单一远程访问服务器可提供以下优势：  
  
-   易于访问。 你可以将运行 Windows 10、Windows 8.1、Windows 8 或 Windows 7 的托管客户端计算机配置为 DirectAccess 客户端。 这些客户端只要位于 Internet 上，就可以随时通过 DirectAccess 访问内部网络资源，无须登录 VPN 连接。 未运行这些操作系统之一的客户端计算机可以通过使用传统 VPN 连接来连接到内部网络。  
  
-   易于管理。 远程访问管理员可以通过 DirectAccess 远程管理 Internet 上的 DirectAccess 客户端计算机，即使客户端计算机不在企业内部网络中也是如此。 管理服务器可以自动修正不符合公司要求的客户端计算机。 DirectAccess 和 VPN 由同一控制台管理并具有相同的向导集。 此外，可从单个远程访问管理控制台管理一台或多台远程访问服务器  
  
## <a name="BKMK_NEW"></a>此方案中包含的角色和功能  
下表列出了本方案所需的角色和功能：  
  
|角色/功能|如何支持本方案|  
|---------|-----------------|  
|远程访问角色|使用服务器管理器控制台或 Windows PowerShell 安装或卸载此角色。 本角色包括 DirectAccess（以前是 Windows Server 2008 R2 中的功能）以及路由和远程访问服务（以前是网络策略和访问服务 (NPAS) 服务器角色项下的角色服务）。 远程访问角色由以下两个组件组成：<br /><br />1.DirectAccess 和路由和远程访问服务（RRAS） VPN。 DirectAccess 和 VPN 在远程访问管理控制台中一起进行管理。<br />2.RRAS 路由。 RRAS 路由功能在旧版路由和远程访问控制台中进行管理。<br /><br />远程访问服务器角色依赖以下服务器角色/功能：<br /><br />-Internet Information Services （IIS） Web 服务器-在远程访问服务器上配置网络位置服务器和默认 Web 探测需要使用此功能。<br />-Windows 内部数据库。 用于远程访问服务器上的本地计帐。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-在安装远程访问角色时，它默认安装在远程访问服务器上，并支持远程管理控制台用户界面和 Windows PowerShell cmdlet。<br />-可选择将它安装在不运行远程访问服务器角色的服务器上。 在这种情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />-远程访问 GUI<br />-适用于 Windows PowerShell 的远程访问模块<br /><br />依赖项包括：<br /><br />-组策略管理控制台<br />-RAS 连接管理器管理工具包（CMAK）<br />-Windows PowerShell 3。0<br />-图形管理工具和基础结构|  
  
## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  
  
-   服务器要求：  
  
    -   满足 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 硬件要求的计算机。  
  
    -   服务器必须至少安装和启用了一个网络适配器，并连接至内部网络。 当使用两个适配器时，应有一个适配器连接至内部企业网络，另一个连接至外部网络（Internet 或专用网络）。  
  
    -   至少一个域控制器。 远程访问服务器和 DirectAccess 客户端都必须是域成员。  
  
-   客户端的要求：  
  
    -   客户端计算机必须运行 Windows 10、Windows 8.1 或 Windows 8。  
  
        > [!IMPORTANT]  
        > 如果某些或所有客户端计算机运行的是 Windows 7，则必须使用高级安装向导。 本文档中所述的入门安装向导不支持运行 Windows 7 的客户端计算机。 请参阅使用[高级设置部署单个 DirectAccess 服务器](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)，以获取有关如何将 Windows 7 客户端与 DirectAccess 一起使用的说明。  
  
        > [!NOTE]  
        > 仅可将以下操作系统用作 DirectAccess 客户端：Windows 10 企业版、Windows 8.1 企业版、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 8 企业版、Windows Server 2008 R2、Windows 7 企业版和 Windows 7 旗舰版。  
  
-   基础机构和管理服务器要求：  
  
    -   如果启用了 VPN 且没有配置静态 IP 地址池，则必须部署 DHCP 服务器以将 IP 地址自动分配给 VPN 客户端。  
  
-   需要运行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 SP2 或 Windows Server 2008 R2 的 DNS 服务器。  
  
## <a name="BKMK_SOFT"></a>软件要求  
存在许多对本方案的要求。  
  
-   服务器要求：  
  
    -   远程访问客户端必须使用 V.90 调制解调器。 可以将服务器部署在内部网络边缘，或边缘防火墙或其他设备的后面。  
  
    -   如果远程访问服务器位于边缘防火墙或 NAT 设备的后面，则该设备必须配置为允许远程访问服务器进出流量。  
  
    -   在服务器上部署远程访问需要拥有服务器本地管理员权限和域用户权限。 此外，管理员需要具备在 DirectAccess 部署中使用 GPO 的权限。 若要利用将 DirectAccess 部署局限在移动计算机的功能，需要在域控制器上创建 WMI 筛选器的权限。  
  
-   远程访问客户端要求：  
  
    -   DirectAccess 客户端必须是域成员。 包含客户端的域可以属于与远程访问服务器相同的林，或拥有远程访问服务器林的双向信任。  
  
    -   包含配置为 DirectAccess 客户端的的计算机需要 Active Directory 安全组。 如果配置 DirectAccess 客户端设置时未指定安全组，客户端 GPO 默认应用于 Domain Computers 安全组中的所有便携式计算机。 仅可将以下操作系统用作 DirectAccess 客户端：Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows 8 企业版、Windows 7 企业版和 Windows 7 旗舰版。  
  
## <a name="BKMK_LINKS"></a>另请参阅  
下表提供其他资源的链接。  
  
|内容类型|参考资料|  
|--------|-------|  
|**TechNet 上的远程访问**|[远程访问技术中心](https://technet.microsoft.com/network/bb545655.aspx)|  
|**工具和设置**|[远程访问 PowerShell cmdlet](https://technet.microsoft.com/library/hh918399.aspx)|  
|**社区资源**|[DirectAccess Wiki 条目](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**相关技术**|[IPv6 的工作原理](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


