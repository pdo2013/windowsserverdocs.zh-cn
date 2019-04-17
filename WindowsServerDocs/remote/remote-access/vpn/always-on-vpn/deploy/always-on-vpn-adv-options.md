---
title: 始终启用 VPN 的高级功能
description: 超过此部署中提供的部署方案中，你可以添加其他高级的 VPN 功能，以改进的安全性和 VPN 连接的可用性。
ms.assetid: 51a1ee61-3ffe-4f65-b8de-ff21903e1e74
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: a544ac3c1a121874170a2fc78a34bd401b8bebe1
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067061"
---
# 始终启用 VPN 的高级的功能

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**上一步：** 了解始终启用 VPN 技术](../always-on-vpn-technology-overview.md)<br>
& #187;[**下一步：** 开始菜单规划始终启用 VPN 部署](always-on-vpn-deploy-planning.md)

超出提供的部署方案，你可以添加其他高级的 VPN 功能，以改进的安全性和 VPN 连接的可用性。 例如，此类组件可以帮助确保允许连接之前连接的客户端正常。


## 高可用性

下面是用于实现高可用性的其他选项。


|选项  |说明  |
|---------|---------|
|服务器复原能力和负载平衡     |在环境中需要高可用性或支持大量请求，你可以通过提高性能和复原的远程访问使用多个服务器运行网络策略服务器 (NPS) 并启用远程之间的负载平衡访问服务器群集。<p>相关的文档：<ul><li>[NPS 代理服务器负载平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)</li><li>[在群集中部署远程访问](https://docs.microsoft.com/windows-server/remote/remote-access/ras/cluster/deploy-remote-access-in-cluster)</li></ul>        |
|地理站点复原能力     |对于基于 IP 的地理位置，你可以使用 Windows Server 2016 中 DNS 使用全局流量管理器。 对于更可靠的地理负载平衡，你可以使用全局负载平衡服务器解决方案，如 Microsoft Azure 流量管理器中。<p>相关的文档：<ul><li>[流量管理器概述](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)</li><li>[Microsoft Azure 流量管理器](https://azure.microsoft.com/services/traffic-manager)</li></ul>         |

---

## 高级身份验证

以下是进行身份验证的其他选项。


|选项  |说明  |
|---------|---------|
|Windows Hello 企业版     |在 Windows10 中，Windows Hello 企业版在电脑和移动设备上使用强大的双重身份验证替代密码。 此身份验证包含一种新型用户凭据可绑定到设备，并使用生物识别或个人标识号 (PIN)。<p>Windows 10 VPN 客户端与 Windows Hello 企业版兼容。 在用户下次使用手势后，VPN 连接使用 Windows Hello 企业证书进行基于证书的身份验证。<p>相关的文档：<ul><li>[Windows Hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)</li><li>技术案例研究：[使用适用于企业的 Windows 10 的 Windows Hello 启用远程访问](https://msdn.microsoft.com/library/mt728163.aspx)</li></ul>         |
|Azure 多重身份验证 (MFA)     |Azure MFA 具有云中和本地可与 Windows VPN 身份验证机制集成的版本。<p>有关此机制的工作原理的详细信息，请参阅[使用 Azure 多重身份验证服务器的集成 RADIUS 身份验证](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)。         |

---

## 高级的 VPN 功能

以下是对高级功能的其他选项。


|选项  |说明  |
|---------|---------|
|流量筛选     |如果你需要强制执行哪些应用程序 VPN 客户端可以访问，你可以启用 VPN 流量筛选器。<p>有关详细信息，请参阅[VPN 安全功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)。         |
|应用触发 VPN     |你可以配置用于某些应用程序或类型的应用程序启动时自动连接 VPN 配置文件。<p>有关此选项及其他触发选项的详细信息，请参阅[VPN 自动触发的配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)。         |
|VPN 条件访问   |条件访问以及设备合规性可以要求他们可以连接到 VPN 之前, 符合标准的托管的设备。 一个用于 VPN 条件访问的高级功能允许你限制仅限于 VPN 连接的客户端身份验证证书其中包含 AAD 条件访问的 OID"1.3.6.1.4.1.311.87 的。<p>若要限制的 VPN 连接，你需要：<ol><li>在 NPS 服务器上，打开**网络策略服务器**管理单元中。</li><li>展开**策略** > **网络策略**。</li><li>右键单击**虚拟专用网络 (VPN) 连接**网络策略，然后选择**属性**。</li><li>选择**设置**选项卡。</li><li>选择**特定供应商**，然后单击**添加**。</li><li>选择**允许的证书-OID**选项，然后单击**添加**。</li><li>粘贴**1.3.6.1.4.1.311.87**作为属性值，AAD 条件访问 OID，然后单击**确定**两次。</li><li>单击**关闭**，然后**应用**。<p>现在当 VPN 客户端尝试使用云短期证书以外的任何证书进行连接，连接将失败。</li></ol>有关条件访问的详细信息，请参阅[VPN 和条件访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)。   |

---


## 额外的保护

**受信任的平台模块 (TPM) 密钥证明**<p>
使用 TPM 证明的密钥的用户证书提供更高版本的安全保障，由非可导出性，反攻击和隔离由 TPM 的密钥的备份。

有关 Windows 10 中的 TPM 密钥证明的详细信息，请参阅[TPM 密钥证明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)。

## 下一步
[规划始终启用 VPN 部署开始菜单](always-on-vpn-deploy-planning.md)： 你打算使用为 VPN 服务器的计算机上安装远程访问服务器角色之前，执行以下任务。 正确的规划之后, 可以部署始终启用 VPN，并 （可选） 配置为使用 Azure AD 的 VPN 连接的条件访问。  

---

## 相关主题
- [NPS 代理服务器负载平衡](../../../../../networking/technologies/nps/nps-manage-proxy-lb.md)： 远程身份验证拨入用户服务 (RADIUS) 的客户端，如虚拟专用网络 (VPN) 服务器和无线接入点的网络访问服务器，创建连接请求，并将它们发送到半径如 NPS 服务器。 在某些情况下，NPS 服务器可能会收到连接请求过多，一次导致性能下降或重载。

- [流量管理器概述](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-overview)： 本主题概要的 Azure 流量管理器，它允许你控制用户服务终结点的流量的分布。 流量管理器使用域名系统 (DNS) 直接对基于流量路由的方法和运行状况的终结点的最合适的终结点的客户端请求。 

- [Windows Hello 企业版](https://docs.microsoft.com/windows/access-protection/hello-for-business/hello-identity-verification)： 本主题提供的先决条件，例如云仅部署和混合部署。  本主题还列出了有关 Windows Hello 企业版常见问题。

- [技术案例研究： 启用远程访问与 Windows Hello 适用于企业的 Windows 10](https://msdn.microsoft.com/library/mt728163.aspx)： 此技术案例研究中，你了解 Microsoft 如何实现与 Windows Hello 企业版的远程访问。  Windows Hello 企业版是一个私钥/公钥密钥或基于证书的身份验证方法企业和消费者款超越密码。 这种形式的身份验证依赖于可以替换密码，它们是抵抗泄露、 失窃和网络钓鱼密钥对凭据。 

- [使用 Azure 多重身份验证服务器的集成 RADIUS 身份验证](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-server-radius)： 本主题介绍了通过添加和使用 Azure 多重身份验证服务器配置 RADIUS 客户端身份验证。 RADIUS 是一种标准协议以接受身份验证请求并处理这些请求。 Azure 多重身份验证服务器可以充当 RADIUS 服务器。 

- [VPN 安全功能](https://docs.microsoft.com/windows/access-protection/vpn/vpn-security-features)： 本主题提供你 VPN 安全准则锁定 VPN，Windows 信息保护 (WIP) 与 VPN 的集成和流量筛选器。 

- [VPN 自动触发的配置文件选项](https://docs.microsoft.com/windows/access-protection/vpn/vpn-auto-trigger-profile)： 本主题提供 VPN 自动触发的配置文件选项，例如应用触发器、 基于名称的触发器，和始终启用。

- [VPN 和条件访问](https://docs.microsoft.com/windows/access-protection/vpn/vpn-conditional-access)： 本主题提供基于云的条件访问平台提供用于远程客户端的设备合规性选项的概述。 条件访问是基于策略的评估引擎，允许你为任何 Active Directory (Azure AD) 连接的应用程序创建访问规则。 

- [TPM 密钥证明](https://docs.microsoft.com/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation)： 本主题提供概述的受信任的平台模块 (TPM) 和步骤来部署 TPM 密钥证明。 你还可以找到疑难解答信息和解决问题的步骤。 

---