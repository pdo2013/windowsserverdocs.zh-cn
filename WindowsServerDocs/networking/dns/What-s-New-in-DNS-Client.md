---
title: 在 Windows Server DNS 客户端中的新增功能
description: 本主题提供 Windows Server 和 Windows 10 中的 DNS 客户端中的新功能的概述
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: defe88fe2a1e4f5393be4d5d5938d9f5bfbfb5d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>什么是 Windows Server 2016 中的 DNS 客户端中的新增功能

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍了新或更改在 Windows 10 和 Windows Server 2016 和更高版本的以下操作系统域名系统 (DNS) 客户端功能。
  
## <a name="updates-to-dns-client"></a>对 DNS 客户端的更新

**DNS 客户端服务绑定**： 在 Windows 10 中，在 DNS 客户服务提供增强具有多个网络接口计算机的支持。 对于穴多的计算机，通过以下方式优化 DNS 分辨率：  
  
-   当配置的特定接口 DNS 服务器用于解决了 DNS 查询时，DNS 客户端服务将对此接口绑定发送 DNS 查询之前。  
  
    通过绑定到特定界面，DNS 客户端可以清晰地指定的名称分辨率发生的接口，、 启用应用程序通过此优化通信 DNS 客户端与网络接口。  
  
-   如果使用 DNS 服务器由组策略设置从名称分辨率策略表 (NRPT)，DNS 客户端服务不绑定特定的界面。  
  
> [!NOTE]  
> 对 DNS 客户服务，在 Windows 10 中更改存在，还在运行 Windows Server 2016 及更高版本的计算机。  
  
## <a name="see-also"></a>请参阅  
  
-   [在 DNS 服务器，在 Windows Server 2016 的新增功能](What-s-New-in-DNS-Server.md)  
  

