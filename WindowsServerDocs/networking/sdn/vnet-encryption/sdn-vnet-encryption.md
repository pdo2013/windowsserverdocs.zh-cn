---
title: 虚拟网络加密
description: 虚拟网络加密允许加密标记为启用加密。 的子网中相互通信的虚拟机之间的虚拟网络流量
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: f2f50ae3146854e2ef6081b0c400a474b53dcf66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851078"
---
# <a name="virtual-network-encryption"></a>虚拟网络加密

>适用于：Windows Server

虚拟网络加密允许加密标记为启用加密。 的子网中相互通信的虚拟机之间的虚拟网络流量 它还利用虚拟子网上的数据报传输层安全性 (DTLS) 来加密数据包。 DTLS 可以防止能够访问物理网络的任何人进行窃听、篡改和伪造。

虚拟网络加密要求：
- 每个已启用 SDN 的 HYPER-V 主机上安装的加密证书。
- 引用该证书的指纹在网络控制器中的凭据对象。
- 每个虚拟网络上的配置包含需要加密的子网。

一旦启用的子网上的加密，该子网中的所有网络流量除了可能也会发生任何应用程序级别加密自动都加密。  流量，即使标记为已加密，跨子网，之间将自动发送未加密。 跨虚拟网络边界的任何流量也发送未加密。

>[!TIP]
>如果必须限制应用程序仅上进行通信的加密的子网，可以使用访问控制列表 (Acl) 仅以允许在当前的子网内的通信。 有关详细信息，请参阅[使用访问控制列表 (Acl) 管理数据中心网络流量流到](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow)。

### <a name="next-steps"></a>后续步骤

[配置虚拟网络的加密](https://docs.microsoft.com/windows-server/networking/sdn/vnet-encryption/sdn-config-vnet-encryption)

