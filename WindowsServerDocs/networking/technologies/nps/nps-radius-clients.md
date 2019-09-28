---
title: RADIUS 客户端
description: 本主题概述了 Windows Server 2016 中的网络策略服务器的 RADIUS 客户端。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1dfc1bb71d2800a8a9587e54147950dfd7fb371f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395995"
---
# <a name="radius-clients"></a>RADIUS 客户端

>适用于：Windows Server（半年频道）、Windows Server 2016

网络访问服务器 \(NAS @ no__t 是一种提供对较大网络的某种级别访问权限的设备。 使用 RADIUS 基础结构的 NAS 也是 RADIUS 客户端，将连接请求和记帐消息发送到 RADIUS 服务器，以进行身份验证、授权和记帐。

>[!NOTE]
>客户端计算机（如便携式计算机和其他运行客户端操作系统的计算机）不是 RADIUS 客户端。 RADIUS 客户端是网络访问服务器（如无线访问点、802.1 X 身份验证交换机、虚拟专用网络 \(VPN @ no__t 服务器和拨号服务器），因为它们使用 RADIUS 协议来与 RADIUS 服务器进行通信，例如作为网络策略服务器 \(NPS @ no__t 服务器。

若要将 NPS 部署为 RADIUS 服务器或 RADIUS 代理，必须在 NPS 中配置 RADIUS 客户端。

## <a name="radius-client-examples"></a>RADIUS 客户端示例

网络访问服务器的示例如下：

- 提供与组织网络或 Internet 的远程访问连接的网络访问服务器。 例如，运行 Windows Server 2016 操作系统的计算机和远程访问服务，为组织 intranet 提供传统的拨号或虚拟专用网络（VPN）远程访问服务。
- 无线接入点，可使用基于无线的传输和接收技术提供对组织网络的物理层访问。
- 提供对组织网络的物理层访问的交换机，使用传统 LAN 技术（如以太网）。
- 向 radius 服务器转发连接请求的 RADIUS 代理，该服务器是在 RADIUS 代理上配置的远程 RADIUS 服务器组的成员。

## <a name="radius-access-request-messages"></a>RADIUS 访问-请求消息

RADIUS 客户端可以创建 RADIUS 访问请求消息，并将其转发到 RADIUS 代理或 RADIUS 服务器，或者将访问请求消息转发到其已从其他 RADIUS 客户端接收但尚未自行创建的 RADIUS 服务器。

RADIUS 客户端不会通过执行身份验证、授权和记帐来处理访问请求消息。 仅 RADIUS 服务器执行这些功能。

但是，可以同时将 NPS 配置为 RADIUS 代理和 RADIUS 服务器，以便它可以处理某些访问请求消息并转发其他消息。

## <a name="nps-as-a-radius-client"></a>作为 RADIUS 客户端的 NPS

将 NPS 配置为 RADIUS 代理，以将访问请求消息转发给其他 RADIUS 服务器进行处理时，NPS 充当 RADIUS 客户端。 将 NPS 用作 RADIUS 代理时，需要以下常规配置步骤：

1. 网络访问服务器（如无线访问点和 VPN 服务器）配置为使用 NPS 代理的 IP 地址作为指定的 RADIUS 服务器或身份验证服务器。 这允许网络访问服务器（根据从访问客户端接收的信息创建访问请求消息）将消息转发到 NPS 代理。

2. 通过将每个网络访问服务器添加为 RADIUS 客户端来配置 NPS 代理。 此配置步骤允许 NPS 代理从网络访问服务器接收消息，并在身份验证过程中与它们进行通信。 此外，NPS 代理上的连接请求策略配置为指定转发到一个或多个 RADIUS 服务器的访问请求消息。 这些策略也是使用远程 RADIUS 服务器组配置的，该组告诉 NPS 向其发送从网络访问服务器接收的消息的位置。

3. Nps 或其他作为 NPS 代理上的远程 RADIUS 服务器组成员的 RADIUS 服务器配置为从 NPS 代理接收消息。 这是通过将 NPS 代理配置为 RADIUS 客户端来实现的。

## <a name="radius-client-properties"></a>RADIUS 客户端属性

通过 NPS 控制台或通过使用适用于 NPS 或 Windows PowerShell 命令的 netsh 命令向 NPS 配置添加 RADIUS 客户端时，你要将 NPS 配置为从网络访问服务器或RADIUS 代理。

在 NPS 中配置 RADIUS 客户端时，可以指定以下属性：

### <a name="client-name"></a>客户端名称

 RADIUS 客户端的友好名称，这使得在使用 nps 管理单元或 NPS 的 netsh 命令时更易于识别。

### <a name="ip-address"></a>IP 地址

Internet 协议版本 4 \(IPv4 @ no__t address 或域名系统 \(DNS @ no__t-3 RADIUS 客户端的名称。

### <a name="client-vendor"></a>客户端-供应商

RADIUS 客户端的供应商。 否则，可以使用客户端供应商的 RADIUS 标准值。

### <a name="shared-secret"></a>共享密钥

用作 RADIUS 客户端、RADIUS 服务器和 RADIUS 代理之间的密码的文本字符串。 使用消息身份验证器属性时，共享机密也用作加密 RADIUS 消息的密钥。 此字符串必须在 RADIUS 客户端和 NPS 管理单元中进行配置。

### <a name="message-authenticator-attribute"></a>消息身份验证器属性

在 RFC 2869 "RADIUS 扩展" 中介绍，消息摘要5对整个 RADIUS 消息 \(MD5 @ no__t 哈希。 如果存在 RADIUS 消息身份验证器属性，则对其进行验证。 如果验证失败，则丢弃 RADIUS 消息。 如果客户端设置需要消息身份验证器属性，但该属性不存在，则会丢弃 RADIUS 消息。 建议使用消息身份验证器属性。

>[!NOTE]
>使用可扩展的身份验证协议时，消息身份验证器属性是必需的，并且默认情况下启用 \(EAP @ no__t authentication。 

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。

