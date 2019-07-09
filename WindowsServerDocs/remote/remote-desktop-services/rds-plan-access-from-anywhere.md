---
title: 远程桌面服务 - 从任意位置访问
description: RD 网关规划信息
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
ms.openlocfilehash: 0d3d8ed036b3befd81da6d5bbe8702ee866c6aa8
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "63748821"
---
# <a name="remote-desktop-services---access-from-anywhere"></a>远程桌面服务 - 从任意位置访问

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

最终用户可以通过 RD 网关从企业防火墙外部安全连接到内部网络资源。

无论如何为最终用户配置桌面，都可以轻松地将 RD 网关插入连接流，以实现快速、安全连接。 对于通过已发布源连接的最终用户，可以在配置总体部署属性时配置 RD 网关属性。 对于在没有源的情况下连接到桌面的最终用户，他们可以轻松将组织的 RD 网关的名称添加为连接属性，无论他们使用哪个远程桌面客户端应用程序。

RD 网关的三个主要用途（按连接顺序排列）如下：
1. **在最终用户设备和 RD 网关服务器之间建立加密 SSL 隧道**：为了通过任何 RD 网关服务器进行连接，RD 网关服务器必须安装最终用户的设备能够识别的证书。 在概念测试和证明中，可以使用自签名证书，但在任何生产环境中都应该只使用来自证书颁发机构的公开受信任证书。
2. **对进入环境的用户进行身份验证**：RD 网关使用收件箱 IIS 服务进行身份验证，甚至可以使用 RADIUS 协议来利用[多重身份验证](rds-plan-mfa.md)解决方案，例如 Azure MFA。 除了创建的默认策略之外，还可以创建其他 RD 资源授权策略 (RD RAP) 和 RD 连接授权策略 (RD CAP)，以便更具体地定义哪些用户应有权访问安全环境中的哪些资源。
3. **在最终用户设备和指定资源之间来回传递流量**：只要建立连接，RD 网关就会继续执行此任务。 可以在 RD 网关服务器上指定不同的超时属性，以在用户离开设备时维护环境的安全性。

可以[在桌面托管参考体系结构中](desktop-hosting-reference-architecture.md)找到有关远程桌面服务部署的整体体系结构的其他详细信息。