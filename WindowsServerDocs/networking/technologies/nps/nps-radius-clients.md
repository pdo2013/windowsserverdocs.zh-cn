---
title: RADIUS 客户端
description: 本主题提供适用于 Windows Server 2016 中的网络策略 Server RADIUS 客户端的概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a970dabcc6f3f4fc4d8ed5a0dedd01b5a7dee71a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="radius-clients"></a>RADIUS 客户端

>适用于：Windows Server（半年通道），Windows Server 2016

网络的访问权限服务器 \(NAS\) 是提供一些较大的网络的访问权限的级别的设备。 使用 RADIUS 基础结构 NAS 也是 RADIUS 客户端，将连接要求和记帐消息发送给 RADIUS 服务器的身份验证、和记帐。

>[!NOTE]
>客户端计算机，如笔记本电脑和其他运行客户端操作系统的计算机不是 RADIUS 客户端。 RADIUS 客户端将网络的访问权限服务器的无线接入点如、802.1 X 身份验证交换机、虚拟专用网络 \(VPN\) 服务器，并拨号服务器-，因为他们使用 RADIUS 协议如网络策略服务器 \(NPS\) 服务器 RADIUS 服务器与其通信。

部署 NPS RADIUS 服务器或 RADIUS 代理中，你必须配置中 NPS RADIUS 客户端。

## <a name="radius-client-examples"></a>RADIUS 客户端示例

网络的访问权限服务器的示例如下：

- 提供远程访问组织网络或 Internet 连接的网络访问权限服务器。 一个示例是一台运行 Windows Server 2016 操作系统和提供或者传统拨号远程访问服务虚拟专用网络 (VPN) 远程访问服务组织 intranet 到计算机。
- 提供物理层网络的访问权限组织使用无线传输和接收技术的无线接入点。
- 提供物理层网络的访问权限的组织，使用以太网如的传统 LAN 技术的开关。
- RADIUS 连接请求转发到 RADIUS 服务器 RADIUS 代理配置远程 RADIUS 服务器组成员的代理服务器。

## <a name="radius-access-request-messages"></a>RADIUS 访问请求消息

RADIUS 客户端创建 RADIUS 访问请求邮件并将它们转发给 RADIUS 代理或 RADIUS 服务器或它们转发给从另一台 RADIUS 客户收到，但无法创建自己的 RADIUS 服务器访问请求邮件中。

RADIUS 客户端无法通过执行身份验证、和记帐处理访问请求消息。 仅 RADIUS 服务器执行这些功能。

NPS，但是，可以配置为 RADIUS 代理和 RADIUS 服务器同时，以便处理一些访问请求邮件和转发其他消息。

## <a name="nps-as-a-radius-client"></a>NPS RADIUS 客户端用作

NPS RADIUS 客户端时作为为 RADIUS 代理转发给其他 RADIUS 服务器，以便处理访问请求邮件对其进行配置。 当你 NPS RADIUS 代理用作，是必需的常规配置以下步骤：

1. 如无线接入点和 VPN 服务器的网络访问权限服务器配置 NPS 代理指定的 RADIUS 服务器或身份验证的服务器的 IP 地址。 这将使网络的访问权限服务器创建访问请求邮件访问客户邮件转发到 NPS 代理从这里收到基于信息。

2. 通过添加为 RADIUS 客户端的每个网络的访问权限服务器配置 NPS 代理。 此配置步骤允许 NPS 代理接收来自网络的访问服务器消息并与其通信身份验证过程。 此外，连接要求策略 NPS 的代理服务器上配置为指定的访问权限请求邮件转发给一个或多个 RADIUS 服务器。 这些策略还配置了告知 NPS 哪个地址来发送电子邮件收到来自网络的访问服务器远程 RADIUS 服务器组。

3. NPS 或在 NPS 代理远程 RADIUS 服务器组成员的其他 RADIUS 服务器配置为接收来自 NPS 代理消息。 作为来配置代理 NPS RADIUS 客户端完成此操作。

## <a name="radius-client-properties"></a>RADIUS 客户端属性

当你添加到 NPS 配置通过 NPS 控制台或通过使用用于 NPS netsh 命令或 Windows PowerShell 命令 RADIUS 客户端时，将配置 NPS RADIUS 访问请求邮件接收网络的访问服务器或 RADIUS 代理。

在配置中 NPS RADIUS 客户端时，你可以指定以下属性：

### <a name="client-name"></a>客户端名称

 对于 RADIUS 客户端，从而使其更易于识别使用适用于 NPS NPS 管理单元或 netsh 命令时的友好名称。

### <a name="ip-address"></a>IP 地址

Internet 协议第四版 \(IPv4\) 地址或 RADIUS 客户端的域名系统 \(DNS\) 名称。

### <a name="client-vendor"></a>客户端供应商

RADIUS 客户供应商。 否则，你可以使用 RADIUS 标准值为客户端供应商。

### <a name="shared-secret"></a>共享的机密

用作 RADIUS 客户端、RADIUS 服务器和 RADIUS 代理服务器之间密码的文本字符串。 当使用该消息 Authenticator 特性时，共享的幽静还用于键加密 RADIUS 消息。 这一串必须配置，打开 RADIUS 客户端和 NPS 贴靠中。

### <a name="message-authenticator-attribute"></a>消息 Authenticator 属性

RFC 2869，"RADIUS 扩展，"邮件摘要 5 \(MD5\) 希的整个 RADIUS 邮件中所述。 如果存在 RADIUS 邮件 Authenticator 属性，已经过验证。 如果它未通过验证，则丢弃 RADIUS 消息。 如果客户端设置需要消息 Authenticator 特性，并且不存在，请将被丢弃 RADIUS 消息。 建议使用消息 Authenticator 属性。

>[!NOTE]
>消息 Authenticator 特性要求，并且默认情况下启用，当您使用扩展验证协议 \(EAP\) 身份验证。 

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。

