---
title: QoS 方面的常见问题
description: 本主题提供有关 Windows Server 2016 中的服务质量 (QoS) 策略的问题的解答。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9fb54cc3bda9a259188ae02fb2ef2836dd8718d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833748"
---
# <a name="qos-policy-frequently-asked-questions"></a>QoS 策略方面的常见问题

>适用于：Windows 服务器 （半年频道），Windows Server 2016

以下被常见问题解答 – 和解答这些问题 – QoS 策略。
  
1.  **我的域控制器需要处于运行状态才能使用 QoS 策略哪种操作系统？**
  
     Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 或 Windows Server 2008

2.  **QoS 策略应用于用户或计算机支持哪些操作系统？**

     可以将 QoS 策略应用于用户或运行 Windows Server 2016、 Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows Server 2008 和 Windows Vista 的计算机。

3.  **QoS 策略应用于发送方或接收方的流量？**

     必须在发送计算机来影响其出站流量应用 QoS 策略。 若要影响的两台计算机的双向通信，QoS 策略需要应用于这两台计算机。

4.  **如果冲突的 QoS 策略部署到同一台计算机，则会发生什么情况？**  
  
     如果多个策略应用，更具体的 QoS 策略优先。 例如，一个策略，规定主机地址 (192.168.4.12) 而不是更具体的网络地址 (192.168.0.0/16) 应用。 如果计算机级和用户级别策略具有相同的特征，而不是计算机级别 QoS 策略应用的用户级 QoS 策略。 

5.  **默认情况下启用 QoS 策略？**

     否，默认情况下不启用 QoS 策略。 必须创建 QoS 策略，手动以启用 QoS。  有关详细信息，请参阅[管理 QoS 策略](qos-policy-manage.md)。

本指南中的第一个主题，请参阅[服务质量 (QoS) 策略](qos-policy-top.md)。
