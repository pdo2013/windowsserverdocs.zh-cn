---
title: QoS 常见问题
description: 本主题提供有关 Windows Server 2016 中的服务质量（QoS）策略问题的解答。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1bde6c859284fbd1756517d7b08ec96816bb2bb5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395957"
---
# <a name="qos-policy-frequently-asked-questions"></a>QoS 策略常见问题

>适用于：Windows Server（半年频道）、Windows Server 2016

下面是有关 QoS 策略的常见问题以及这些问题的答案。
  
1.  **我的域控制器需要运行什么操作系统才能使用 QoS 策略？**
  
     Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008

2.  **哪些操作系统支持将 QoS 策略应用于用户或计算机？**

     可以将 QoS 策略应用于运行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 的用户或计算机。

3.  **QoS 策略是否适用于通信的发送方或接收方？**

     必须在发送计算机上应用 QoS 策略，以影响其出站流量。 为了影响两台计算机的双向通信，需要将 QoS 策略应用到这两台计算机。

4.  **如果将冲突的 QoS 策略部署到同一台计算机，会发生什么情况？**  
  
     如果应用了多个策略，则优先使用更具体的 QoS 策略。 例如，应用一个规定主机地址（192.168.4.12）的策略，而不是较不具体的网络地址（192.168.0.0/16）。 如果计算机级别和用户级别策略具有相同的具体情况，则会应用用户级 QoS 策略，而不是计算机级别的 QoS 策略。 

5.  **QoS 策略是否默认已启用？**

     否，默认情况下不启用 QoS 策略。 必须手动创建 QoS 策略才能启用 QoS。  有关详细信息，请参阅[管理 QoS 策略](qos-policy-manage.md)。

有关本指南的第一个主题，请参阅[服务质量（QoS）策略](qos-policy-top.md)。
