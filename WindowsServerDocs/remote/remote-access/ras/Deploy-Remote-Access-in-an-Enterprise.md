---
title: 在企业中部署远程访问
description: 本主题介绍适用于企业的 Windows Server 2016 中的 DirectAccess 方案。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4781df0a-158b-4562-b8f5-32b27615a4f8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ab337da85387be8c7d960315bb28644fa3a8a93
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367497"
---
# <a name="deploy-remote-access-in-an-enterprise"></a>在企业中部署远程访问

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题提供适用于企业的 DirectAccess 方案的简介。  
  
  
> [!IMPORTANT]  
> 若要使用本指南部署 DirectAccess，必须使用运行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012 的 DirectAccess 服务器。  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>在开始部署之前，请参阅不受支持的配置、已知问题和先决条件的列表  
  
-   [DirectAccess 不受支持的配置](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-unsupported-configurations)  
  
-   [DirectAccess 的已知问题](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/directaccess-known-issues)  
  
-   [部署 DirectAccess 的先决条件）先决条件](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/prerequisites-for-deploying-directaccess)  
  
## <a name="BKMK_OVER"></a>方案描述  
远程访问具有一系列企业功能，包括部署用 Windows 网络负载平衡 (NLB) 或外部负载平衡器平衡的群集负载中的多台远程访问服务器、使用处于不同地理位置的远程访问服务器设置多站点部署以及使用一次性密码 (OTP) 部署带有双因素客户端身份验证的 DirectAccess。  
  
## <a name="in-this-scenario"></a>本方案内容  
每个企业方案均载于含有计划和部署说明的文档中。 有关详细信息，请参阅：  
  
-   [在群集中部署远程访问](cluster/Deploy-Remote-Access-In-Cluster.md)  
  
-   [在多站点部署中部署多台远程访问服务器](multisite/Deploy-Multiple-Remote-Access-Servers-in-a-Multisite-Deployment.md)  
  
-   [部署带有 OTP 身份验证的远程访问](otp/Deploy-RA-OTP.md)  
  
-   [在多林环境中部署远程访问](multi-forest/Deploy-Remote-Access-in-a-Multi-Forest-Environment.md)  
  
## <a name="BKMK_APP"></a>实用应用程序  
远程访问企业方案具有以下优势：  
  
-   **提高了可用性**。 在群集中部署多台远程访问服务器可提供可伸缩性并增加吞吐量和用户数量的容量。 平衡群集的负载可实现高可用性。 如果群集中的服务器出现故障，远程用户可继续通过群集中的其他服务器访问企业内部网络。 故障是透明的，因为客户端使用虚拟的 IP (VIP) 地址连接到群集。  
  
-   **易于管理**。 可以使用在其中一台群集服务器上运行的远程访问管理控制台，将群集或多站点部署作为单一实体进行配置和管理。 此外，多站点部署可让管理员根据 Active Directory 站点的情况部署远程访问，从而简化体系结构。 可在各个群集服务器之间或所有的多站点入口点服务器上轻松设置共享设置。 可从群集或部署中的任何服务器，或远程使用远程服务器管理工具 (RSAT) 对远程访问设置进行管理。 此外，可从单个远程访问管理控制台监视整个群集或多站点部署。  
  
-   **成本效率**。 远程访问多站点部署可让企业在与客户端位置相应的多个站点中部署远程访问服务器。 不管在哪里，这可为远程客户端提供可预见的访问体验，并通过将 Internet 上的客户端流量路由到最近的远程访问服务器，降低成本和减少 Intranet 带宽。  
  
-   **安全**。 使用一次性密码（OTP）而不是标准 Active Directory 密码来部署强客户端身份验证可提高安全性。  
  
## <a name="BKMK_NEW"></a>此方案中包含的角色和功能  
下表列出了本企业方案所需的角色和功能。  
  
|角色/功能|如何支持本方案|  
|---------|-----------------|  
|远程访问服务器角色|该角色可使用服务器管理器控制台加以安装和卸载。 本角色包括 DirectAccess（以前是 Windows Server 2008 R2 中的功能）以及路由和远程访问服务（以前是网络策略和访问服务 (NPAS) 服务器角色项下的角色服务）。 远程访问角色由以下两个组件组成：<br /><br />1.DirectAccess 和路由和远程访问服务（RRAS） VPN-DirectAccess 和 VPN 在远程访问管理控制台中一起进行管理。<br />2.RRAS 路由-RRAS 路由功能在旧版路由和远程访问控制台中进行管理。<br /><br />远程访问服务器角色取决于以下服务器功能：<br /><br />-Internet Information Services （IIS）-配置网络位置服务器和默认 web 探测需要此功能。<br />-组策略管理控制台功能-DirectAccess 需要使用功能来创建和管理 Active Directory 中的组策略对象（Gpo），并且必须安装为服务器角色所需的功能。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-在安装远程访问角色时，它默认安装在远程访问服务器上，并支持远程管理控制台用户界面。<br />-可选择将它安装在不运行远程访问服务器角色的服务器上。 在这种情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />1.远程访问 GUI 和命令行工具<br />2.Windows PowerShell 的远程访问模块<br /><br />依赖项包括：<br /><br />1.组策略管理控制台<br />2.RAS 连接管理器管理工具包 (CMAK)<br />3.Windows PowerShell 3.0<br />4.图形管理工具和基础结构|  
|Windows NLB|此功能可让多台远程访问服务器的负载平衡。|  
  

  


