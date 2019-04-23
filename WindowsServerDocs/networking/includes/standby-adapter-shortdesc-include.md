---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: d1bfc74c4daa751e3081b26c87dd0d2c88f5f095
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879848"
---
备用适配器的选项都**None （所有适配器处于活动状态）** 或您选择的充当待机适配器 NIC 组中的特定网络适配器。 在 NIC 配置作为备用适配器时，其他所有未选定的团队成员都处于活动状态，并发送到或可用的 NIC 失败之前的适配器处理无网络通信流量。 可用的 NIC 失败后，备用 NIC 将变为活动状态，并且进程网络流量。 所有团队成员获取还原到服务，备用团队成员将返回备用状态。  