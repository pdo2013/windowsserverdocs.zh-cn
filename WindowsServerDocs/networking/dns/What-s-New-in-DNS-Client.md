---
title: Windows Server 中 DNS 客户端的新增功能
description: 本主题概述了 Windows Server 和 Windows 10 中 DNS 客户端的新增功能
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 696ea0b499a4132d630cc0cda15a1d7efdac37a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356057"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Windows Server 2016 中 DNS 客户端的新增功能

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍 Windows 10 和 Windows Server 2016 以及这些操作系统的更高版本中新的或更改的域名系统（DNS）客户端功能。
  
## <a name="updates-to-dns-client"></a>DNS 客户端的更新

**DNS 客户端服务绑定**：在 Windows 10 中，DNS 客户端服务为具有多个网络接口的计算机提供了增强的支持。 对于多宿主计算机，可通过以下方式优化 DNS 解析：  
  
-   当使用在特定接口上配置的 DNS 服务器解析 DNS 查询时，DNS 客户端服务在发送 DNS 查询之前将绑定到此接口。  
  
    通过绑定到特定接口，DNS 客户端可以明确指定进行名称解析的接口，从而使应用程序可以通过此网络接口优化与 DNS 客户端的通信。  
  
-   如果使用的 DNS 服务器由名称解析策略表（NRPT）中的组策略设置指定，则 DNS 客户端服务不会绑定到特定的接口。  
  
> [!NOTE]  
> 在运行 Windows Server 2016 及更高版本的计算机上，Windows 10 中的 DNS 客户端服务的更改也存在。  
  
## <a name="see-also"></a>请参阅  
  
-   [Windows Server 2016 中 DNS 服务器的新增功能](What-s-New-in-DNS-Server.md)  
  

