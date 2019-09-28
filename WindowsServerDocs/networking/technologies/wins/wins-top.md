---
title: Windows Internet 名称服务 (WINS)
description: 本主题提供有关使 WINS 退役并在网络上使用 DNS 进行名称解析服务的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c313dfeb26319cd5f537df417724de7044648
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405250"
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet 名称服务 (WINS)

>适用于：Windows Server（半年频道）、Windows Server 2016

Windows Internet 名称服务 (WINS) 是传统的计算机名称注册和解析访问服务，该服务将计算机 NetBIOS 名称映射到 IP 地址。

如果你尚未在网络上部署 WINS，请不要部署 WINS，而应部署域名系统 \(DNS @ no__t-1。 DNS 还提供计算机名称注册和解析服务，并在 WINS 上包含许多其他权益，如与 Active Directory 域服务的集成。

有关详细信息，请参阅[域名系统（DNS）](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

如果已在网络上部署了 WINS，则建议部署 DNS，并使 WINS 停止。
