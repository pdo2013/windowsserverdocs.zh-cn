---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 01f231ca730a19ac0e7e868bcb7180377830afe1
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935097"
---
通过 Hyper-v 端口，Hyper-v 主机上配置的 NIC 团队为 Vm 提供独立 MAC 地址。  Vm MAC 地址或连接到 Hyper-v 交换机的 VM 端口可用于划分 NIC 组成员之间的网络流量。 你不能在具有 Hyper-v 端口负载平衡模式的 Vm 中配置创建的 NIC 组。 请改用地址哈希模式。 