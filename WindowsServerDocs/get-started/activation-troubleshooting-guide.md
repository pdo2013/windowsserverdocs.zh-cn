---
title: 排查 Windows 批量激活问题
description: 列表资源，提供有关批量激活最佳做法的信息，以及有关激活问题排查的信息
ms.topic: troubleshooting
ms.date: 09/24/2019
ms.technology: server-general
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: 09206b90dc8b829aaa70d0cca34bd05a9e0eb693
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963043"
---
# <a name="troubleshooting-windows-volume-activation"></a>排查 Windows 批量激活问题

产品激活是在特定计算机上安装软件后验证该软件的过程。 激活确认产品为正版（而不是欺骗性副本），并且产品密钥或序列号有效，并未泄露或吊销。 激活还将建立产品密钥与安装之间的链接或关系。

批量激活是激活批量许可产品的过程。 要成为批量许可客户，组织必须与 Microsoft 签订批量许可协议。 Microsoft 提供定制的批量许可计划，可根据组织大小和购买偏好进行调整。 有关详细信息，请参阅 [Microsoft Volume Licensing Service Center](https://www.microsoft.com/Licensing/servicecenter/default.aspx)（Microsoft 批量许可服务中心）。

[Windows Server 2016 激活指南](server-2016-activation.md)重点介绍密钥管理服务 (KMS) 激活技术。 本部分解决常见问题，并提供针对 KMS 和多个其他批量激活技术的故障排除指南。

## <a name="best-practices-for-volume-activation"></a>批量激活的最佳做法

以下文章提供有关 Microsoft 批量激活技术的技术信息和最佳做法。

### <a name="key-management-service-kms"></a>密钥管理服务 (KMS)

- [规划批量激活](https://docs.microsoft.com/windows/deployment/volume-activation/plan-for-volume-activation-client)
- [Understanding KMS](https://docs.microsoft.com/previous-versions/tn-archive/ff793434(v=technet.10))（了解 KMS）
- [Deploying KMS Activation](https://docs.microsoft.com/previous-versions/tn-archive/ff793409%28v=technet.10%29)（部署 KMS 激活）
- [Configuring KMS Hosts](https://docs.microsoft.com/previous-versions/tn-archive/ff793407%28v%3dtechnet.10%29)（配置 KMS 主机）
- [Configuring DNS](https://docs.microsoft.com/previous-versions/tn-archive/ff793405%28v%3dtechnet.10%29)（配置 DNS）
- [使用密钥管理服务进行激活](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-key-management-service-vamt)

### <a name="active-directory-based-activation-adba"></a>基于 Active Directory 的激活 (ADBA)

- [Deploy Active-Directory-based Activation](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn502534%28v%3Dws.11%29)（部署基于 Active Directory 的激活）
- [使用基于 Active Directory 的激活进行激活](https://docs.microsoft.com/windows/deployment/volume-activation/activate-using-active-directory-based-activation-client)
- [基于 Active Directory 的激活概述](https://docs.microsoft.com/windows/deployment/volume-activation/active-directory-based-activation-overview)

### <a name="multiple-activation-key-mak-activation"></a>多次激活密钥 (MAK) 激活

- [Using MAK Activation](https://docs.microsoft.com/previous-versions/tn-archive/ff793438%28v=technet.10%29)（使用 MAK 激活）
- [Understanding MAK Activation](https://docs.microsoft.com/previous-versions/tn-archive/ff793435%28v%3dtechnet.10%29)（了解 MAK 激活）
- [Activating MAK Clients](https://docs.microsoft.com/previous-versions/tn-archive/ff793398%28v%3dtechnet.10%29)（激活 MAK 客户端）

### <a name="subscription-activation"></a>订阅激活

- [Windows 10 订阅激活](https://docs.microsoft.com/windows/deployment/windows-10-subscription-activation)
- [部署 Windows 10 企业版许可证](https://docs.microsoft.com/windows/deployment/deploy-enterprise-licenses)
- [CSP 中的 Windows 10 企业版 E3](https://docs.microsoft.com/windows/deployment/windows-10-enterprise-e3-overview)

## <a name="resources-for-troubleshooting-activation-issues"></a>用于排查激活问题的资源

以下文章提供用于排查批量激活问题的工具的相关指南和信息：

- [密钥管理服务 (KMS) 故障排除指南](activation-troubleshoot-kms-general.md)
- [用于获取批量激活信息的 Slmgr.vbs 选项](activation-slmgr-vbs-options.md)
- [示例：排查无法激活 ADBA 客户端的问题](activation-troubleshoot-adba-clients.md)

以下文章提供用于解决更多特定激活问题的指南：

- [解决常见的激活错误代码](activation-error-codes.md)
- [KMS 激活：已知问题](activation-troubleshoot-KMS-issues.md)
- [MAK 激活：已知问题](activation-troubleshoot-MAK-issues.md)
- [用于排查 DNS 相关激活问题的指南](common-troubleshooting-procedures-kms-dns.md)
- [如何重新生成 Tokens.dat 文件](activation-rebuild-tokens-dat-file.md)
