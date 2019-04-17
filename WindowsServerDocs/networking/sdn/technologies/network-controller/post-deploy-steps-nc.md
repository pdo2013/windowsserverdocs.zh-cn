---
title: 后 Post-Deployment 步骤网络控制器
description: 本主题提供非 Kerberos 部署 Windows Server 2016 数据中心中的网络控制器的证书配置说明进行的操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="post-deployment-steps-for-network-controller"></a>后 Post-Deployment 步骤网络控制器

当你安装了网络控制器时，你可以选择 Kerberos 或非 Kerberos 部署。

对于 non\ Kerberos 部署，你必须配置证书。

## <a name="configure-certificates-for-non-kerberos-deployments"></a>配置为非 Kerberos 部署证书

如果网络控制器和管理客户端计算机或虚拟机 \(VMs\) 不 domain\ 加入，必须通过完成以下步骤来配置 certificate\ 基于身份验证。

- 计算机身份验证的网络控制器上创建证书。 证书主题名称必须与网络控制器计算机或 VM DNS 名称相同。

- 管理 client 上创建证书。 必须通过网络控制器信任该证书。
  
- 注册网络控制器计算机或 VM 证书。 证书必须满足以下要求。
  
    -  必须在增强键使用量 \(EKU\) 或应用程序策略扩展配置服务器身份验证目的和客户端身份验证的用途。 为服务器身份验证的对象标识符是 1.3.6.1.5.5.7.3.1。 客户端身份验证的对象标识符是 1.3.6.1.5.5.7.3.2。
  
    - 解决应证书主题名称：
  
        - 网络控制器计算机或 VM 中，如果网络控制器部署 VM 或台计算机上的 IP 地址。

        - 如果在多台计算机和 / 或多个虚拟机的功能部署了网络控制器其余 IP 地址。
  
    - 必须通过其余部分的所有客户端信任该证书。 此外必须通过软件负载平衡 (SLB) 集线器（输入混合），并由网络控制器的 southbound 主机受信任该证书。
  
    - 该证书可登记通过认证颁发机构（加拿大），也可以是自签名的证书。 自签名的证书建议不要生产部署，但接受的测试实验环境。
  
    - 必须是同一个证书预配上所有网络控制器节点。 创建一个节点证书之后, 可以导出证书（专用密钥），并将其导入其他节点。

有关详细信息，请参阅[网络控制器](Network-Controller.md)。
