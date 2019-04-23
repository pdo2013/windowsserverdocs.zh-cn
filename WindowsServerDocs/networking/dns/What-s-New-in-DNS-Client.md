---
title: 什么是 Windows Server 中 DNS 客户端中的新增功能
description: 本主题概述 Windows Server 和 Windows 10 中的 DNS 客户端中的新增功能
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 34b1a64465e217fbd7e6b3ae55e89832a7a4e48c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860558"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Windows Server 2016 中 DNS 客户端的新增功能

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍了新增或更改在 Windows 10 和 Windows Server 2016 和更高版本的这些操作系统中的域名系统 (DNS) 客户端功能。
  
## <a name="updates-to-dns-client"></a>DNS 客户端的更新

**DNS 客户端服务绑定**:在 Windows 10 中，DNS 客户端服务提供的增强的支持具有多个网络接口的计算机。 对于多宿主计算机，DNS 解析优化以下方式：  
  
-   当特定接口配置的 DNS 服务器用于解析的 DNS 查询时，DNS 客户端服务将绑定到此接口发送 DNS 查询之前。  
  
    通过清楚地绑定到特定的接口，则 DNS 客户端可以指定出现名称解析的接口，启用应用程序来优化与 DNS 客户端之间的通信通过此网络接口。  
  
-   如果使用的 DNS 服务器通过组策略设置从名称解析策略表 (NRPT) 指定，DNS 客户端服务将不绑定到特定的接口。  
  
> [!NOTE]  
> 对 Windows 10 中的 DNS 客户端服务的更改也会存在于运行 Windows Server 2016 和更高版本的计算机。  
  
## <a name="see-also"></a>请参阅  
  
-   [什么是 Windows Server 2016 中 DNS 服务器中的新增功能](What-s-New-in-DNS-Server.md)  
  

