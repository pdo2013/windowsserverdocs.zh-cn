---
title: 为 802.1X 有线和无线部署部署服务器证书
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 769a4165cfd82056a904c79c41e96fb666d05e43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842268"
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>为 802.1X 有线和无线部署部署服务器证书

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本指南可用于将服务器证书部署到你的远程访问和网络策略服务器 (NPS) 基础结构服务器。   

本指南包含下列各节。  

-   [使用本指南的先决条件](#bkmk_pre)  

-   [本指南未提供的内容](#bkmk_not)  

-   [技术概述](#bkmk_tech)  

-   [服务器证书部署概述](Server-Certificate-Deployment-Overview.md)  

-   [服务器证书部署规划](Server-Certificate-Deployment-Planning.md)  

-   [服务器证书部署](Server-Certificate-Deployment.md)  

### <a name="digital-server-certificates"></a>**数字的服务器证书**  
本指南提供有关使用 Active Directory 证书服务 (AD CS) 自动注册到远程访问和 NPS 基础结构服务器的证书的说明。 AD CS 允许你构建公钥基础结构 (PKI) 并为你的组织提供公钥加密、 数字证书和数字签名功能。  

当网络上的计算机之间进行身份验证使用数字服务器证书时，证书将提供：   

1. 通过加密确保机密性。  
2. 通过数字签名的完整性。  
3. 通过将证书密钥与计算机网络上的计算机、 用户或设备帐户相关联的身份验证。  

### <a name="server-types"></a>**服务器类型**  
通过使用本指南，可以将服务器证书部署到以下类型的服务器。  
- 服务器正在运行远程访问服务而言是 DirectAccess 或标准虚拟专用网络 (VPN) 服务器，且均隶属于**RAS 和 IAS 服务器**组。  
- 服务器正在运行的网络策略服务器 (NPS) 服务，是的成员**RAS 和 IAS 服务器**组。  

### <a name="advantages-of-certificate-autoenrollment"></a>**证书自动注册的优点**  
自动注册的服务器证书，也称为自动注册，可提供以下优势。  

- AD CS 证书颁发机构 (CA) 中自动注册的所有 NPS 和远程访问服务器的服务器证书。  
- 在域中的所有计算机自动都接收 CA 证书，请安装在受信任的根证书颁发机构存储在每台域成员计算机上。 因此，在域中的所有计算机都信任由 CA 颁发的证书。 此信任关系允许在身份验证服务器证明其身份，对每个其他和参与安全通信。  
- 非刷新组策略，则不需要手动重新配置的每个服务器。  
- 每个服务器证书增强型密钥用法 (EKU) 扩展中包含的服务器身份验证用途和客户端身份验证目的。  
- 可扩展性。 在部署后使用本指南企业根 CA，可以通过添加企业从属 Ca 来扩展你的公钥基础结构 (PKI)。  
- 可管理性。 您可以通过使用 AD CS 控制台或通过使用 Windows PowerShell 命令和脚本管理 AD CS。  
- 简单易用。 指定通过使用 Active Directory 组帐户和组成员身份注册服务器证书的服务器。   
- 在部署服务器证书，证书基于你使用本指南中的说明配置的模板。 这意味着你可以自定义不同的证书模板的特定服务器类型，或为所有你想要颁发的服务器证书，可以使用同一个模板。  

## <a name="bkmk_pre"></a>使用本指南的先决条件  

本指南将说明了如何将服务器证书部署在 Windows Server 2016 中使用 AD CS 和 Web 服务器 (IIS) 服务器角色。 以下是执行本指南中的过程的先决条件。  

- 必须部署核心网络使用 Windows Server 2016 核心网络指南，或必须已安装并正常网络上核心网络指南中提供的技术。 这些技术包括 TCP/IP v4，DHCP，Active Directory 域服务 (AD DS)、 DNS 和 NPS。  
>[!NOTE]
>Windows Server 2016 核心网络指南现已推出 Windows Server 2016 技术库。 有关详细信息，请参阅[核心网络指南](../../../core-network-guide/Core-Network-Guide.md)。

- 您必须阅读本指南，以确保在执行部署之前准备为此部署的规划部分。  
- 本指南中所出现的顺序，必须执行步骤。 不要跳转和部署您的 CA 没有执行到部署服务器或你的部署步骤将失败。  
- 您必须准备好部署两个新的服务器在网络中的上一台服务器作为企业根 CA，将在其安装 AD CS 和一台服务器时，安装 Web 服务器 (IIS) 时将您的 CA 可以将证书吊销列表 (CRL) 发布到 Web se进行服务器。   

>[!NOTE]  
>你已准备好将静态 IP 地址分配给使用本指南中，以及关于命名根据您的组织的命名约定的计算机部署的 Web 和 AD CS 服务器。 此外，必须将计算机加入到你的域。  

## <a name="bkmk_not"></a>本指南未提供的内容  
本指南不提供设计和使用 AD CS 部署公钥基础结构 (PKI) 的完整说明。 建议您查看 AD CS 文档和 PKI 设计文档，然后再部署本指南中的技术。   

## <a name="bkmk_tech"></a>技术概述  
以下是有关 AD CS 和 Web 服务器 (IIS) 的技术概述。  

### <a name="active-directory-certificate-services"></a>Active Directory 证书服务  
Windows Server 2016 中的 AD CS 提供可自定义服务用于创建和管理采用公钥技术的软件安全系统中使用的 X.509 证书。 组织可以使用 AD CS 来增强安全性，通过将个人、 设备或服务的标识绑定到相应的公共密钥。 AD CS 还包括可用于管理证书注册及吊销在各种可伸缩环境中的功能。  

有关详细信息，请参阅[Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)并[公共密钥基础结构设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。  

### <a name="web-server-iis"></a>Web 服务器 (IIS)  

Windows Server 2016 中的 Web 服务器 (IIS) 角色以可靠地托管网站、 服务和应用程序提供安全、 易于管理、 模块化和可扩展平台。 使用 IIS，可以共享 Internet、 intranet 或 extranet 上与用户的信息。 IIS 是一个集 IIS、 ASP.NET、 FTP 服务、 PHP 和 Windows Communication Foundation (WCF) 的统一的 web 平台。  

有关详细信息，请参阅[Web 服务器 (IIS) 概述](https://technet.microsoft.com/library/hh831725.aspx)。  
