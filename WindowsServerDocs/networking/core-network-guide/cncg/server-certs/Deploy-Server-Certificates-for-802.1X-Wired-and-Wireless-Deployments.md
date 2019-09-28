---
title: 为 802.1X 有线和无线部署部署服务器证书
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0dce886555167ad651704045120fb92eff0dcea1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356179"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>为 802.1X 有线和无线部署部署服务器证书

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本指南将服务器证书部署到远程访问和网络策略服务器（NPS）基础结构服务器。   

本指南包含下列各节。  

-   [使用本指南的先决条件](#bkmk_pre)  

-   [本指南未提供的内容](#bkmk_not)  

-   [技术概述](#bkmk_tech)  

-   [服务器证书部署概述](Server-Certificate-Deployment-Overview.md)  

-   [服务器证书部署规划](Server-Certificate-Deployment-Planning.md)  

-   [服务器证书部署](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**数字服务器证书**  
本指南提供有关使用 Active Directory 证书服务（AD CS）自动向远程访问和 NPS 基础结构服务器注册证书的说明。 AD CS 使你可以构建公钥基础结构（PKI）并为你的组织提供公钥加密、数字证书和数字签名功能。  

当你在网络中的计算机之间使用数字服务器证书进行身份验证时，证书将提供：   

1. 通过加密的机密性。  
2. 数字签名的完整性。  
3. 通过将证书密钥与计算机网络上的计算机、用户或设备帐户相关联进行身份验证。  

### <a name="server-types"></a>**服务器类型**  
通过使用本指南，你可以将服务器证书部署到下列类型的服务器。  
- 运行远程访问服务的服务器，这些服务器是 DirectAccess 或标准虚拟专用网络（VPN）服务器，并且是**RAS 和 IAS 服务器**组的成员。  
- 运行网络策略服务器（NPS）服务的服务器，该服务是**RAS 和 IAS 服务器**组的成员。  

### <a name="advantages-of-certificate-autoenrollment"></a>**证书自动注册的优点**  
服务器证书的自动注册（也称为自动注册）提供以下优点。  

- AD CS 证书颁发机构（CA）自动向所有 NPS 和远程访问服务器注册服务器证书。  
- 域中的所有计算机都自动接收你的 CA 证书，该证书安装在每个域成员计算机上的 "受信任的根证书颁发机构" 存储中。 因此，域中的所有计算机都信任 CA 颁发的证书。 这种信任使身份验证服务器能够彼此证明其身份，并参与安全通信。  
- 除了刷新组策略以外，无需手动重新配置每个服务器。  
- 每个服务器证书都在增强型密钥用法（EKU）扩展中包括服务器身份验证和客户端身份验证目的。  
- 可扩展性。 使用本指南部署企业根 CA 后，你可以通过添加企业从属 Ca 来扩展你的公钥基础结构（PKI）。  
- 可管理性。 你可以使用 AD CS 控制台或 Windows PowerShell 命令和脚本来管理 AD CS。  
- 简单易用。 使用 Active Directory 组帐户和组成员身份来指定注册服务器证书的服务器。   
- 部署服务器证书时，证书基于你使用本指南中的说明配置的模板。 这意味着，你可以为特定服务器类型自定义不同的证书模板，也可以为要颁发的所有服务器证书使用同一模板。  

## <a name="bkmk_pre"></a>使用本指南的先决条件  

本指南提供有关如何使用 AD CS 和 Windows Server 2016 中的 Web 服务器（IIS）服务器角色部署服务器证书的说明。 下面是执行本指南中的过程的先决条件。  

- 你必须使用 Windows Server 2016 核心网络指南部署核心网络，或者你必须已在网络上安装并正常运行核心网络指南中提供的技术。 这些技术包括 TCP/IP v4、DHCP、Active Directory 域服务（AD DS）、DNS 和 NPS。  
  >[!NOTE]
  >Windows server 2016 Core 网络指南在 Windows Server 2016 技术库中提供。 有关详细信息，请参阅[核心网络指南](../../../core-network-guide/Core-Network-Guide.md)。

- 你必须阅读本指南的 "规划" 部分，以确保你已准备好进行此部署，然后再执行部署。  
- 必须按显示的顺序执行本指南中的步骤。 不要提前尝试并部署 CA，而不执行用于部署服务器的步骤，否则部署将失败。  
- 您必须准备好在您的网络上部署两个新服务器：一台服务器上将 AD CS 安装为企业根 CA，一个服务器用于安装 Web 服务器（IIS），以便您的 CA 可以将证书吊销列表（CRL）发布到 Web se服务器.   

>[!NOTE]  
>准备将静态 IP 地址分配到使用本指南部署的 Web 和 AD CS 服务器，并根据组织的命名约定为计算机命名。 此外，必须将计算机加入到域。  

## <a name="bkmk_not"></a>本指南未提供的内容  
本指南未提供有关使用 AD CS 设计和部署公钥基础结构（PKI）的综合说明。 建议你在部署本指南中的技术之前查看 AD CS 文档和 PKI 设计文档。   

## <a name="bkmk_tech"></a>技术概述  
下面是对 AD CS 和 Web 服务器（IIS）的技术概述。  

### <a name="active-directory-certificate-services"></a>Active Directory 证书服务  
Windows Server 2016 中的 AD CS 提供可自定义的服务，用于创建和管理在采用公钥技术的软件安全系统中使用的 x.509 证书。 组织可以通过将个人、设备或服务的标识绑定到相应的公钥，使用 AD CS 来增强安全性。 AD CS 还包括一些功能，可用于在各种可伸缩环境中管理证书注册和吊销。  

有关详细信息，请参阅[Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)和[公钥基础结构设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。  

### <a name="web-server-iis"></a>Web 服务器 (IIS)  

Windows Server 2016 中的 Web 服务器（IIS）角色提供一个安全、易于管理的模块化和可扩展的平台，以可靠地托管网站、服务和应用程序。 使用 IIS，你可以与 Internet、intranet 或 extranet 上的用户共享信息。 IIS 是一个集成了 IIS、ASP.NET、FTP 服务、PHP 和 Windows Communication Foundation （WCF）的统一 web 平台。  

有关详细信息，请参阅[Web 服务器（IIS）概述](https://technet.microsoft.com/library/hh831725.aspx)。  
