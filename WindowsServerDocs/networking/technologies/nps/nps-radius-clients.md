---
title: RADIUS 客户端
description: 本主题概述了用于 Windows Server 2016 中的网络策略服务器的 RADIUS 客户端。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d3a09ac9-75f8-4f57-aab4-b0fdfe110118
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fdca45237d971c1b2e08443112b63d3ce77e4a2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874308"
---
# <a name="radius-clients"></a>RADIUS 客户端

>适用于：Windows 服务器 （半年频道），Windows Server 2016

网络访问服务器\(NAS\)是提供的访问权限较大网络某些级别的设备。 使用 RADIUS 基础结构的 NAS 还是 RADIUS 客户端，将连接请求和记帐消息发送到 RADIUS 服务器进行身份验证、 授权和记帐。

>[!NOTE]
>客户端计算机，如便携式计算机和运行客户端操作系统，其他计算机不是 RADIUS 客户端。 RADIUS 客户端是网络访问服务器-如无线访问点、 802.1 X 身份验证交换机、 虚拟专用网络\(VPN\)服务器和拨号服务器-因为它们使用 RADIUS 协议来与 RADIUS 通信服务器，例如网络策略服务器\(NPS\)服务器。

若要将 NPS 部署为 RADIUS 服务器或 RADIUS 代理，必须在 NPS 中配置 RADIUS 客户端。

## <a name="radius-client-examples"></a>RADIUS 客户端示例

网络访问服务器的示例包括：

- 提供到组织网络或 Internet 的远程访问连接的网络访问服务器。 例如，组织的 intranet 到运行 Windows Server 2016 操作系统和远程访问服务提供是传统拨号或虚拟专用网络 (VPN) 远程访问服务的计算机。
- 提供对使用基于无线的传输和接收技术的组织网络的物理层访问权限的无线访问点。
- 提供到组织的网络，使用传统的 LAN 技术，例如以太网的物理层访问权限的开关。
- 连接请求转发到 RADIUS 代理配置远程 RADIUS 服务器组的成员的 RADIUS 服务器的 RADIUS 代理。

## <a name="radius-access-request-messages"></a>RADIUS 访问-请求消息

RADIUS 客户端创建 RADIUS 访问-请求消息并将其转发到 RADIUS 代理或 RADIUS 服务器，或它们转发到 RADIUS 服务器，它们已从另一个 RADIUS 客户端接收但尚未创建自己的访问-请求消息。

RADIUS 客户端通过执行身份验证、 授权和记帐不处理访问请求消息。 只有 RADIUS 服务器执行这些功能。

NPS 中，但是，可以配置为 RADIUS 代理和 RADIUS 服务器，以便它处理某些访问-请求消息和转发其他消息。

## <a name="nps-as-a-radius-client"></a>NPS 用作 RADIUS 客户端

配置为 RADIUS 代理将访问-请求消息转发到其他 RADIUS 服务器进行处理时，NPS 充当 RADIUS 客户端。 当将 NPS 用作 RADIUS 代理时，则需要以下常规配置步骤：

1. 网络访问服务器，如无线访问点和 VPN 服务器配置 NPS 代理为指定的 RADIUS 服务器或身份验证服务器的 IP 地址。 这允许创建它们从访问客户端，将消息转发到 NPS 代理接收访问请求消息根据信息的网络访问服务器。

2. 通过将每个网络访问服务器添加为 RADIUS 客户端配置 NPS 代理。 此配置步骤允许 NPS 代理从网络访问服务器接收消息并在整个身份验证与它们通信。 此外，在 NPS 代理服务器上的连接请求策略配置来指定哪些访问-请求消息转发到一个或多个 RADIUS 服务器。 这些策略还会配置远程 RADIUS 服务器组，以告诉 NPS 将它从网络访问服务器接收的消息发送到何处。

3. NPS 或其他 RADIUS 服务器在 NPS 代理上远程 RADIUS 服务器组的成员配置为从 NPS 代理接收消息。 这是通过将 NPS 代理配置为 RADIUS 客户端实现。

## <a name="radius-client-properties"></a>RADIUS 客户端属性

当通过 NPS 控制台或通过使用 NPS 的 netsh 命令或 Windows PowerShell 命令的 NPS 配置添加 RADIUS 客户端时，请在配置 NPS 从网络访问服务器接收 RADIUS 访问-请求消息或RADIUS 代理。

当在 NPS 中配置 RADIUS 客户端时，您可以指定以下属性：

### <a name="client-name"></a>客户端名称

 RADIUS 客户端，因此可以轻松地识别 nps 使用 NPS 管理单元或 netsh 命令时一个友好名称。

### <a name="ip-address"></a>IP 地址

Internet 协议版本 4 \(IPv4\)地址或域名系统\(DNS\) RADIUS 客户端的名称。

### <a name="client-vendor"></a>客户端供应商

RADIUS 客户端的供应商。 否则，你可以为客户端供应商使用 RADIUS 标准值。

### <a name="shared-secret"></a>共享密钥

作为 RADIUS 客户端、 RADIUS 服务器和 RADIUS 代理之间密码使用文本字符串。 当使用消息身份验证器属性时，共享的机密还用于为键加密 RADIUS 消息。 此字符串必须配置 RADIUS 客户端上和 NPS 管理单元中。

### <a name="message-authenticator-attribute"></a>消息身份验证器属性

RFC 2869，"扩展"Message Digest 5 中所述\(MD5\)整个 RADIUS 消息的哈希值。 如果存在 RADIUS 消息身份验证器属性，则对其进行验证。 如果验证失败，则丢弃 RADIUS 消息。 如果客户端设置需要消息身份验证器属性并且不存在，则丢弃 RADIUS 消息。 建议使用消息身份验证器属性。

>[!NOTE]
>消息身份验证器属性是所需的当你使用可扩展身份验证协议时，默认情况下启用\(EAP\)身份验证。 

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。

