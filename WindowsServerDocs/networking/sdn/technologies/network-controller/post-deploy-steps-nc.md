---
title: 网络控制器的部署后步骤
description: 本主题为 Windows Server 2016 Datacenter 中的网络控制器的非 Kerberos 部署提供证书配置说明。
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3f95d2884a808239c1d171eecbc983e26e799102
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355614"
---
# <a name="post-deployment-steps-for-network-controller"></a>网络控制器的部署后步骤

安装网络控制器时，可以选择 Kerberos 或非 Kerberos 部署。

对于非 @ no__t-0Kerberos 部署，你必须配置证书。

## <a name="configure-certificates-for-non-kerberos-deployments"></a>为非 Kerberos 部署配置证书

如果网络控制器的计算机或虚拟机 \(VMs @ no__t，而管理客户端不是域 @ no__t-2joined，则必须通过完成以下步骤来配置证书 @ no__t-3based authentication。

- 在网络控制器上创建用于计算机身份验证的证书。 证书使用者名称必须与网络控制器计算机或 VM 的 DNS 名称相同。

- 在管理客户端上创建证书。 此证书必须受网络控制器信任。
  
- 在网络控制器计算机或 VM 上注册证书。 证书必须满足以下要求。
  
    -  服务器身份验证和客户端身份验证目的都必须在增强型密钥用法 \(EKU @ no__t 或应用程序策略扩展中进行配置。 服务器身份验证的对象标识符为1.3.6.1.5.5.7.3.1。 客户端身份验证的对象标识符为1.3.6.1.5.5.7.3.2。
  
    - 证书使用者名称应解析为：
  
        - 网络控制器计算机或 VM 的 IP 地址（如果在单台计算机或 VM 上部署了网络控制器）。

        - 如果在多台计算机上部署了网络控制器和/或多个 Vm，则 REST IP 地址。
  
    - 此证书必须受所有 REST 客户端信任。 证书还必须受软件负载平衡（SLB）复用器（MUX）和网络控制器管理的 southbound 主机计算机的信任。
  
    - 证书可以由证书颁发机构（CA）注册，也可以是自签名证书。 不建议将自签名证书用于生产部署，但对于测试实验室环境是可接受的。
  
    - 必须在所有网络控制器节点上设置相同的证书。 在一个节点上创建证书之后，可以导出该证书（包含私钥），并将其导入其他节点。

有关详细信息，请参阅[网络控制器](Network-Controller.md)。
