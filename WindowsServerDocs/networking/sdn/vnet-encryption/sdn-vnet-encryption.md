---
title: 虚拟网络加密
description: 虚拟网络加密允许加密虚拟机之间的虚拟网络流量，这些虚拟机在标记为 "已启用加密" 的子网中相互通信。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 056edc49d6089be5f9753d41ebd0cec7feacc0c5
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870039"
---
# <a name="virtual-network-encryption"></a>虚拟网络加密

>适用于：Windows Server

虚拟网络加密允许加密虚拟机之间的虚拟网络流量，这些虚拟机在标记为 "已启用加密" 的子网中相互通信。 它还利用虚拟子网上的数据报传输层安全性 (DTLS) 来加密数据包。 DTLS 可以防止能够访问物理网络的任何人进行窃听、篡改和伪造。

虚拟网络加密要求：
- 在每个启用了 SDN 的 Hyper-v 主机上安装的加密证书。
- 网络控制器中引用该证书的指纹的凭据对象。
- 每个虚拟网络上的配置都包含需要加密的子网。

在子网中启用加密后，该子网中的所有网络流量都将自动进行加密，以及任何可能发生的应用程序级加密。  跨子网的流量（即使标记为已加密）将自动以未加密的方式发送。 跨虚拟网络边界的任何流量也会以未加密状态发送。

>[!TIP]
>如果你必须将应用程序限制为仅在加密的子网上进行通信，则只能使用访问控制列表（Acl）来允许当前子网中的通信。 有关详细信息，请参阅[使用访问控制列表（acl）管理数据中心网络流量流](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。

### <a name="next-steps"></a>后续步骤

[为虚拟网络配置加密](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

