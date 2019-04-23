---
title: 网络控制器的部署后步骤
description: 本主题提供非 Kerberos 的 Windows Server 2016 Datacenter 中的网络控制器部署证书配置的说明。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871078"
---
# <a name="post-deployment-steps-for-network-controller"></a>网络控制器的部署后步骤

在安装网络控制器时，可以选择 Kerberos 或非 Kerberos 的部署。

对于非\-Kerberos 的部署，则必须配置证书。

## <a name="configure-certificates-for-non-kerberos-deployments"></a>配置非 Kerberos 部署证书

如果计算机或虚拟机\(Vm\)网络控制器和管理客户端不是域\-加入，则必须配置证书\-通过完成以下基于身份验证步骤。

- 创建进行计算机身份验证在网络控制器上的证书。 证书使用者名称必须与网络控制器计算机或虚拟机的 DNS 名称相同。

- 管理客户端上创建一个证书。 此证书必须受网络控制器。
  
- 注册网络控制器计算机或虚拟机上的证书。 该证书必须满足以下要求。
  
    -  增强型密钥用法必须配置的服务器身份验证用途和客户端身份验证目的\(EKU\)或应用程序策略扩展。 服务器身份验证的对象标识符为 1.3.6.1.5.5.7.3.1。 客户端身份验证的对象标识符为 1.3.6.1.5.5.7.3.2。
  
    - 证书使用者名称应解析为：
  
        - 网络控制器计算机或 VM，如果在单台计算机或 VM 上部署网络控制器的 IP 地址。

        - 如果在多台计算机、 多个 Vm，或这两者上部署网络控制器 REST IP 地址。
  
    - 所有 REST 客户端必须都信任此证书。 此外必须通过软件负载平衡 (SLB) 多路复用器 (MUX) 和由网络控制器托管 southbound 主机计算机受信任证书。
  
    - 证书可以由证书颁发机构 (CA) 注册，也可以是自签名的证书。 自签名的证书不建议用于生产部署，但对于测试实验室环境是可接受。
  
    - 网络控制器的所有节点上都必须提供相同的证书。 后一个节点上创建证书时，可以导出 （包含私钥） 的证书，并将其导入在其他节点上。

有关详细信息，请参阅[网络控制器](Network-Controller.md)。
