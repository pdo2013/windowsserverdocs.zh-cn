---
title: Windows Internet 名称服务 (WINS)
description: 本主题提供有关停用 WINS 和使用 DNS 进行名称解析服务在网络上的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bbc1871d29021aa3c99f14368a4711dac63f4cee
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843628"
---
#  <a name="windows-internet-name-service-wins"></a>Windows Internet 名称服务 (WINS)

>适用于：Windows 服务器 （半年频道），Windows Server 2016

Windows Internet 名称服务 (WINS) 是传统的计算机名称注册和解析访问服务，该服务将计算机 NetBIOS 名称映射到 IP 地址。

如果你还没有在网络上部署 WINS，不部署 WINS-而是，部署域名系统\(DNS\)。 DNS 还提供了计算机名称注册和解析服务，并通过 WINS，包括其他许多好处，例如，与 Active Directory 域服务集成。

有关详细信息，请参阅[域名系统 (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

如果你已在网络上部署 WINS，建议部署 DNS，然后停止使用 WINS。
