---
title: 远程访问 Always On VPN 迁移概述
description: Always On VPN 以前填补了 Windows Vpn 和 DirectAccess，以及如何将 DirectAccess 迁移到 Always On VPN 之间。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 05/29/2018
ms.openlocfilehash: 402d8ff72fe869572c9e6129cdf1aa7e755c354a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845978"
---
# <a name="overview-of-the-directaccess-to-always-on-vpn-migration"></a>DirectAccess 到始终启用 VPN 迁移概述 

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

&#187;[**下一步：** 规划 DirectAccess 添加到 Always On VPN 迁移](da-always-on-migration-planning.md)

在以前版本的 Windows VPN 体系结构，平台限制进行难提供替换 DirectAccess，例如在用户登录之前启动的自动连接所需的关键功能。 Always On VPN，但是，缓解大部分这些限制或扩展之外的 DirectAccess 功能的 VPN 功能。 Always On VPN 以前填补了 Windows Vpn 和 DirectAccess 之间。

DirectAccess – 到 – 始终打开 VPN 迁移过程包括四个主要组件的高级别进程：


1.  **计划 Always On VPN 迁移。** 规划可帮助标识用户阶段分离，以及基础结构和功能的目标客户端。

    1.  [!INCLUDE [build-migration-rings-shortdesc-include](../includes/build-migration-rings-shortdesc-include.md)]

    2.  [!INCLUDE [review-feature-mapping-shortdesc-include](../includes/review-feature-mapping-shortdesc-include.md)] 

    3.  [!INCLUDE [review-the-enhancements-shortdesc-include](../includes/review-the-enhancements-shortdesc-include.md)] 

    4.  [!INCLUDE [review-the-technology-overview-shortdesc-include](../includes/review-the-technology-overview-shortdesc-include.md)]

2.  **部署由并行 VPN 基础结构。** 已确定迁移阶段和想要在部署中包含的功能后，你将部署与现有的 DirectAccess 基础结构并行的 Always On VPN 基础结构。  

3.  **部署到客户端证书和配置。**  VPN 基础结构准备就绪后，创建并发布到客户端所需的证书。 当客户端收到了证书时，还会部署 VPN_Profile.ps1 配置脚本。 或者，可以使用 Intune 配置 VPN 客户端。 使用 Microsoft System Center Configuration Manager 或 Microsoft Intune 成功 VPN 配置部署的监视。

4.  **删除和取消标记。** 迁移所有人关闭 DirectAccess 后，正确地解除授权环境。

    1.  [!INCLUDE [remove-da-from-client-shortdesc-include](../includes/remove-da-from-client-shortdesc-include.md)]

    2.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]


## <a name="directaccess-deployment-scenario"></a>DirectAccess 部署方案

在此部署方案中，使用一个简单的 DirectAccess 部署方案作为起始点本指南介绍了的迁移。 不需要迁移到 Always On VPN 之前, 匹配此部署方案中，但对于许多组织而言，此简单的安装程序是其当前的 DirectAccess 部署的准确表示形式。 下表提供了此安装程序的一系列基本功能。

许多 DirectAccess 部署方案和选项存在，因此您的实现很可能是不同于此处所述。 如果是这样，请参阅[DirectAccess 和 Always On VPN 之间的功能映射](../vpn/vpn-map-da.md)来确定将为你当前添加件，映射设置为 Always On VPN 功能，并将这些功能添加到你的配置。 此外，请参阅[Always On VPN 增强功能](../vpn/always-on-vpn/always-on-vpn-enhancements.md)若要添加到 Always On VPN 部署选项。

>[!NOTE] 
>对于已加入域的设备，有一些其他注意事项，如证书注册。 有关详细信息，请参阅[Always On VPN 部署的 Windows Server 和 Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md)。

### <a name="deployment-scenario-feature-list"></a>部署方案功能列表

| DirectAccess 功能 | 典型的方案 |
|-----|----|
| 部署方案                   | 全面部署 DirectAccess 客户端访问和远程管理                                               |
| 网络适配器                      | 2                                                                                                              |
| 用户身份验证                   | Active Directory 凭据                                                                                   |
| 使用计算机证书             | 是                                                                                                            |
| 安全组                       | 是                                                                                                            |
| 单个 DirectAccess 服务器            | 是                                                                                                            |
| 网络拓扑                      | 网络地址转换 (NAT) 边缘防火墙后面的两个网络适配器                            |
| 访问模式                           | 端到边缘                                                                                                    |
| 隧道                             | 拆分隧道                                                                                                   |
| 身份验证                        | 使用计算机证书和 Kerberos (不 KerbProxy) 标准公钥基础结构 (PKI) 身份验证 |
| 协议                             | 通过 HTTPS (IP-HTTPS) 的 IP                                                                                       |
| 网络位置服务器 (NLS) 在即 | 是                                                                                                            |

## <a name="always-on-vpn-deployment-scenario"></a>Always On VPN 部署方案

在此部署方案中，您集中精力将简单的 DirectAccess 环境迁移到简单的 Always On VPN 环境，这是 DirectAccess 替代解决方案。 下表提供了此简单的解决方案中使用的功能。 有关详细有关 Always On VPN 客户端的其他增强功能的信息，请参阅[Always On VPN 增强功能](../vpn/always-on-vpn/always-on-vpn-enhancements.md)。

### <a name="always-on-vpn-features-used-in-the-simple-environment"></a>在简单的环境中使用 always On VPN 功能

| VPN 功能 | 部署方案配置 |
|-----|-----|
| 连接类型 | 本机 Internet 密钥交换版本 2 (IKEv2) |
| 网络适配器   | 2        |
| 用户身份验证  | Active Directory 凭据            |
| 使用计算机证书        | 是                          |
| 路由 | 分拆隧道 |
| 名称解析 | 域名称信息列表和域名系统 (DNS) 后缀 |
| 触发 | 始终启用以及受信任网络检测 |
| 身份验证  | 受保护可扩展身份验证协议-传输层安全 (PEAP-TLS) 与受保护的受信任的平台模块 – 用户证书 |

## <a name="next-step"></a>下一步

[规划 DirectAccess 添加到 Always On VPN 迁移](da-always-on-migration-planning.md)。 迁移的主要目标是保持远程连接到在整个过程中的 office 用户。

---