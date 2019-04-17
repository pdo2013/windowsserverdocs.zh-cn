---
title: Windows Internet 名称服务(WINS)
description: 本主题提供停止使用 WINS 和使用 DNS 网络上的名称分辨率服务的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a3d132ada7b1ede83b046499058399a9da12190
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet 名称服务(WINS)

>适用于：Windows Server（半年通道），Windows Server 2016

Windows Internet 名称服务(WINS) 是旧计算机名称注册和分辨率服务将计算机 netbios 映射到 IP 地址。

如果你已经没有 WINS 部署在你的网络，不改用部署 WINS-、 部署域名系统 \(DNS\)。 DNS 还提供了计算机名称注册和分辨率服务，并包括通过 WINS，许多其他权益，如使用 Active Directory 域服务的集成。

有关详细信息，请参阅[域名系统 (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

如果你已经在你的网络部署 WINS，建议你部署 DNS，然后停止使用 WINS。
