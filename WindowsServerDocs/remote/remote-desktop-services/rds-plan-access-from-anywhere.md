---
title: 远程桌面服务-从任意位置访问
description: 有关 RD 网关的规划信息
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5c38fab1-3586-4b7a-8bf0-7d85a8d5361d
author: lizap
ms.author: elizapo
ms.date: 11/03/2016
manager: dongill
ms.openlocfilehash: 2b10428ff90c50c4d7e113552ddda3cca5447b06
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845828"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>远程桌面服务-从任意位置访问

>适用于：Windows 服务器 （半年频道），Windows Server 2016

最终用户可以连接到外部企业防火墙通过 RD 网关安全地从内部网络资源。

不管如何配置最终用户桌面，可以轻松地插入 RD 网关的快速、 安全地连接的连接流程。 对于最终用户通过已发布的源连接，可以配置总体部署属性配置 RD 网关属性。 对于最终用户通过连接到其桌面，而无需源，他们可以轻松添加组织的 RD 网关的名称，为他们使用的远程桌面客户端应用程序无论连接属性。

RD 网关，以连接序列顺序的三个主要目的是：
1. **建立最终用户的设备和 RD 网关服务器之间的加密的 SSL 隧道**:若要连接任何 RD 网关服务器，通过 RD 网关服务器必须已安装的证书的最终用户的设备识别。 在测试和概念验证的概念，可以使用自签名的证书，但仅使用公开的受信任证书从证书颁发机构应在任何生产环境中使用。
2. **对用户进行身份验证到环境**:RD 网关使用 IIS 服务来执行身份验证，并甚至可以使用 RADIUS 协议来利用收件箱[多重身份验证](rds-plan-mfa.md)解决方案，例如 Azure MFA。 除了创建的默认策略，可以创建其他 RD 资源授权策略 (RD Rap) 和 RD 连接授权策略 (RD Cap) 更专门定义哪些用户应具有访问权限的资源中的安全环境。
3. **最终用户的设备和指定的资源之间来回传递流量**:RD 网关将继续执行此任务，只要建立的连接。 在 RD 网关服务器用于维护环境的安全性，以防用户走远离该设备上，可以指定不同的超时属性。

您可以找到更多详细信息的远程桌面服务部署的整体体系结构[桌面托管参考体系结构中](desktop-hosting-reference-architecture.md)。