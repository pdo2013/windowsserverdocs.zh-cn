---
title: 部署服务器证书 802.1 X 有线和无线部署
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: 0a39ecae-39cc-4f26-bd6f-b71ed02fc4ad
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fda9b2b53d088cc389e98cf96fecd748419bc6e2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-server-certificates-for-8021x-wired-and-wireless-deployments"></a>部署服务器证书 802.1 X 有线和无线部署

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本指南部署到你远程访问和网络策略 Server (NPS) 的基础结构服务器服务器证书。   

本指南包含以下部分。  

-   [使用本指南先决条件](#bkmk_pre)  

-   [本指南不提供的内容](#bkmk_not)  

-   [技术概述](#bkmk_tech)  

-   [服务器证书部署概述](Server-Certificate-Deployment-Overview.md)  

-   [服务器证书部署计划](Server-Certificate-Deployment-Planning.md)  

-   [服务器证书部署](Server-Certificate-Deployment.md)  

### **<a name="digital-server-certificates"></a>数字服务器证书**  
本指南提供了使用 Active Directory 证书服务（广告客户服务）自动注册远程访问和 NPS 基础结构服务器对证书的说明进行操作。 广告客户服务允许你生成公共基础结构 (PKI)，并提供有关你的组织的公用加密，数字签名，数字签名的功能。  

当你使用的身份验证你的网络上计算机之间的数字服务器证书时，的证书提供：   

1. 保密通过加密。  
2. 通过数字签名完整性。  
3. 通过将证书键使用计算机、用户或设备计算机网络上的帐户相关联的身份验证。  

### **<a name="server-types"></a>服务器类型**  
使用本指南，你可以将服务器证书部署到以下类型的服务器。  
- 运行的服务器远程访问服务，该了 DirectAccess 或标准虚拟专用网络 (VPN) 服务器以及的成员**RAS 和 IAS 服务器**组。  
- 运行的服务器的网络策略 Server (NPS) 服务，属于**RAS 和 IAS 服务器**组。  

### **<a name="advantages-of-certificate-autoenrollment"></a>自动证书的注册的优势**  
自动注册 server 证书，也称为自动注册，提供了以下的优势。  

- 广告客户服务证书颁发机构）将自动注册到所有 NPS 和远程访问服务器服务器证书。  
- 所有计算机在域中的自动都接收你已安装在受信任的根证书颁发机构的 CA 证书域成员的每台计算机上存储。 出于此原因，在域中的所有计算机都信任由你 CA 证书。 此信任允许你的身份验证服务器证明彼此其身份并从事安全通信。  
- 以外恢复组策略，不需要的所有服务器手动配置。  
- 每个 server 证书包含增强密钥使用量 (EKU) 扩展服务器身份验证目的和客户端身份验证的用途。  
- 可扩展性。 部署本指南你企业根之后, 你可以通过添加企业附属 Ca 扩展公共基础结构 (PKI)。  
- 可管理性。 通过使用广告客户服务主机或使用 Windows PowerShell 命令和脚本复制，你可以管理广告客户服务。  
- 简单。 指定注册服务器证书使用 Active Directory 组帐户和组成员的服务器。   
- 服务器证书部署时，证书均基于你使用本指南中的说明进行操作配置模板。 这意味着你可以自定义特定服务器类型、模板不同的证书，或者你可以对你想要发出的所有服务器证书使用同一模板。  

## <a name="bkmk_pre"></a>使用本指南先决条件  

本指南提供有关如何使用 Windows Server 2016 中广告客户服务和 Web 服务器 (IIS) 服务器角色部署服务器证书的说明进行操作。 以下是执行过程本指南中的先决条件。  

- 必须部署 core 网络使用 Windows Server 2016 Core 网络指南，或者你必须已安装，并且运行正常网络上的核心网络指南中提供的技术。 这些技术包括 TCP/IP v4，DHCP、Active Directory 域服务 (广告 DS)，DNS 和 NPS。  
>[!NOTE]
>Windows Server 2016 Technical 库中提供了 Windows Server 2016 Core 网络指南。 有关详细信息，请参阅[Core 网络指南](../../../core-network-guide/Core-Network-Guide.md)。

- 你必须读取规划本指南，以确保在执行部署之前准备此部署部分。  
- 你必须执行列出的顺序本指南中的步骤。 不要跳过和部署您 CA 未执行步骤部署服务器上或你的部署导致将失败。  
- 你必须准备部署两个新网络上的服务器-依据将为企业根安装广告客户服务的一个服务器和一个依据你将安装 Web 服务器 (IIS)，以便你 CA 可以将证书吊销列表 (CRL) 发布到 Web 服务器的服务器。   

>[!NOTE]  
>你已准备好使用本指南，以及移到命名根据你的组织命名约定计算机部署 Web 和广告客户服务服务器指定的静态 IP 地址。 此外，你必须计算机加入你的域。  

## <a name="bkmk_not"></a>本指南不提供的内容  
本指南不设计和使用广告客户服务部署公共基础结构 (PKI) 提供完整的说明进行操作。 建议你查看广告客户服务文档和部署本指南中的技术前 PKI 设计文档。   

## <a name="bkmk_tech"></a>技术概述  
以下是技术概述广告客户服务和 Web 服务器 (IIS)。  

### <a name="active-directory-certificate-services"></a>Active Directory 证书服务  
在 Windows Server 2016 的广告客户服务提供用于创建和管理于使用公钥技术的软件的安全系统 X.509 证书可自定义的服务。 组织可用于广告的客户服务通过的联系人、设备或服务识别绑定到相应的公钥增强安全性。 广告客户服务也包含允许您管理证书的注册和吊销各种可缩放的环境中的功能。  

有关详细信息，请参阅[Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)和[公共键基础结构设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。  

### <a name="web-server-iis"></a>Web 服务器 (IIS)  

Windows Server 2016 中的 Web 服务器 (IIS) 角色提供适用于可靠举办的网站、服务和应用安全、易于管理、模块化和扩展的平台。 通过 IIS 中，您可以共享与用户 Internet、 intranet，或外部网络上的信息。 IIS 是集成 IIS、 ASP.NET、 FTP 服务、 PHP，以及 Windows 通信基础 (WCF) 统一的 web 平台。  

有关详细信息，请参阅[Web 服务器 (IIS) 概述](https://technet.microsoft.com/library/hh831725.aspx)。  
