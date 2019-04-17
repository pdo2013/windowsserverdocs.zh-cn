---
title: 虚拟网络加密
description: 本主题提供的虚拟网络加密的软件定义联网在 Windows Server 的信息
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 7da0f509-7b02-4a0f-90fb-d97c83a2bc4e
ms.author: pashort
author: grcusanz
ms.openlocfilehash: 425cc1cff8c7241aeec7764b60c89b4586fd323b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="virtual-network-encryption"></a>虚拟网络加密

>适用于： Windows Server

虚拟网络加密提供的虚拟网络通信加密之间内标记为"启用加密"子网互相的虚拟机的功能。

此功能在虚拟子网加密数据包利用数据报传输层安全性 (DTLS)。  DTLS 可提供抵御窃取、被篡改以及伪造人物理网络的访问权限。

虚拟网络加密 requries 加密证书，才能安装每个 SDN 上启用 Hyper-V 主机中引用的证书，指纹网络控制器, 的凭据对象和在每个虚拟网络包含子网需要加密配置。

一旦子网上启用了加密，该子网内的所有网络通信已自动都加密。  这将除了任何应用程序级别的加密，可能还会生效。  将自动发送加密交叉之间子网，即使标记为两个子网加密的交通。  超过虚拟网络界限任何交通也会发送加密。

配置虚拟网络加密的信息，请参阅[虚拟网络配置加密](sdn-config-vnet-encryption.md)。

如果你必须限制为仅的应用程序进行通信上的加密子网。  你可以使用访问控制列表 (Acl) 仅允许在当前子网通信。  

配置访问控制列表中的信息，请参阅[使用访问控制列出 (Acl) 到管理数据中心网络交通流](../manage/use-acls-for-traffic-flow.md)。