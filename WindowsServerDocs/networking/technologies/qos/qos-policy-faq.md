---
title: QoS：常见问题
description: 本主题提供的有关 Windows Server 2016 服务质量 (QoS) 策略问题的解答。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e63cd00a2016411e9f2c532c0e4c301dabdfd816
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-frequently-asked-questions"></a>QoS 策略的常见问题

>适用于：Windows Server（半年通道），Windows Server 2016

以下被常见问题 – 和回答这些问题，即 QoS 策略。
  
1.  **我的域控制器需要运行才能使用 QoS 策略哪些操作系统？**
  
     Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008

2.  **哪些操作系统支持策略应用的 QoS 给用户或计算机？**

     你可以将 QoS 策略适用于用户或运行 Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows Server 2008 和 Windows Vista 的计算机。

3.  **QoS 策略并应用于发件人或的交通接收器上？**

     上发送计算机影响其站交通必须应用 QoS 策略。 为了影响两台计算机的双向流量，请 QoS 策略需要将应用于这两台计算机。

4.  **如果有冲突 QoS 策略程序部署到同一台计算机会发生什么情况？**  
  
     如果多个策略适用，更多具体 QoS 策略优先。 例如，策略主机的状态地址 (192.168.4.12）获取应用而不是太特定网络地址 (192.168.0.0 月 16）。 如果计算机级别和用户级别的策略相同的特定，而不是计算机级别 QoS 策略应用用户级别 QoS 策略。 

5.  **默认情况下启用 QoS 策略？**

     否，未默认情况下启用 QoS 策略。 你必须创建 QoS 策略手动以启用 QoS。  有关详细信息，请参阅[管理 QoS 策略](qos-policy-manage.md)。

本指南中的第一个主题，请参阅[质量服务 (QoS) 策略](qos-policy-top.md)。
