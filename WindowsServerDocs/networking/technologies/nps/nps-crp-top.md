---
title: 连接请求处理
description: 本主题概述了网络策略服务器的连接请求处理在 Windows Server 2016。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ab08dced15cfadd5a0fc773efc2ddaa2da24c08
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829108"
---
# <a name="connection-request-processing"></a>连接请求处理

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题，了解如何在 Windows Server 2016 中的网络策略服务器中处理连接请求。

>[!NOTE]
>除了本主题，以下连接请求处理文档不可用。
> - [连接请求策略](nps-crp-crpolicies.md)
> - [领域名称](nps-crp-realm-names.md)
> - [远程 RADIUS 服务器组](nps-crp-rrsg.md)

连接请求处理可用于指定执行身份验证的连接请求的位置-本地计算机上或在远程 RADIUS 服务器的远程 RADIUS 服务器组的成员。 

如果你想运行网络策略服务器 (NPS) 的连接请求执行身份验证的本地服务器，可以使用默认连接请求策略，而无需其他配置。 根据默认策略，NPS 进行身份验证的用户和计算机中本地域和受信任域中有一个帐户。

如果你想要将连接请求转发到远程 NPS 或其他 RADIUS 服务器，创建一个远程 RADIUS 服务器组，然后配置将请求转发到该远程 RADIUS 服务器组的连接请求策略。 使用此配置，NPS 可以身份验证请求转发给任何 RADIUS 服务器，并使用不受信任域中帐户的用户可以进行身份验证。

下图显示远程 RADIUS 服务器组中的访问-请求消息从网络访问服务器到 RADIUS 代理，并返回到 RADIUS 服务器的路径。 在 RADIUS 代理服务器上，网络访问服务器配置为 RADIUS 客户端;并在每个 RADIUS 服务器上，在 RADIUS 代理配置为 RADIUS 客户端。


![NPS 连接请求处理](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>与 NPS 结合使用的网络访问服务器可以是与 RADIUS 协议，如 802.1x 无线访问点和身份验证交换机、 运行远程访问的服务器配置为 VPN 或拨号服务器兼容的网关设备或其他 RADIUS 兼容的设备。

如果要让 NPS 以本地处理某些身份验证请求，同时其他请求转发给远程 RADIUS 服务器组，配置多个连接请求策略。

若要配置连接请求策略以指定哪个 NPS 或 RADIUS 服务器组来处理身份验证请求，请参阅连接请求策略。

若要指定 NPS 或其他 RADIUS 服务器将请求转发到的身份验证，请参阅远程 RADIUS 服务器组。

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS 用作 RADIUS 服务器连接请求处理

当将 NPS 用作 RADIUS 服务器时，RADIUS 消息按以下方式提供身份验证、 授权和记帐为网络访问连接：

1. 访问服务器，如拨号网络访问服务器、 VPN 服务器和无线访问点，从访问客户端接收连接请求。 

2. 配置为使用 RADIUS 作为身份验证、 授权和记帐协议的访问服务器创建访问请求消息并将其发送到 NPS。 

3. NPS 将评估访问请求消息。 

4. 如果需要，NPS 会将访问-质询消息发送到访问服务器。 访问服务器处理质询，并将更新的访问请求发送到 NPS。 

5. 将检查用户凭据，并通过使用安全连接到域控制器获得的用户帐户的拨入属性。 

6. 对连接尝试授权使用这两个拨入属性的用户帐户和网络策略。 

7. 如果连接尝试进行身份验证和授权，NPS 会将访问-接受消息发送到访问服务器。 如果连接尝试未经过身份验证，或者未获得授权，NPS 会将访问-拒绝消息发送到访问服务器。 

8. 访问服务器完成与访问客户端连接过程，并将记帐-请求消息发送到 NPS，则记录消息。 

9. NPS 将记帐-响应发送到访问服务器。 

>[!NOTE]
>访问服务器建立连接、 关闭访问客户端连接时，在和中访问服务器启动和停止时间期间还会发送记帐请求消息。

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS 用作 RADIUS 代理服务器连接请求处理

当 NPS 用作 RADIUS 客户端和 RADIUS 服务器之间的 RADIUS 代理时，RADIUS 消息的网络访问的连接尝试转发按以下方式：

1. 访问服务器，如拨号网络访问服务器、 虚拟专用网络 (VPN) 服务器和无线访问点，从访问客户端接收连接请求。

2. 配置为使用 RADIUS 作为身份验证、 授权和记帐协议的访问服务器创建访问请求消息并将其发送到用作 NPS RADIUS 代理的 NPS。

3. NPS RADIUS 代理接收访问请求消息，根据本地配置的连接请求策略，确定将访问-请求消息转发的位置。

4. NPS RADIUS 代理将转发到相应的 RADIUS 服务器的访问请求消息。

5. RADIUS 服务器评估访问请求消息。

6. 如果需要，RADIUS 服务器发送访问-质询消息到 NPS RADIUS 代理，其转发到访问服务器。 访问服务器处理访问客户端的挑战，并将更新的访问请求发送到 NPS RADIUS 代理，它转发到 RADIUS 服务器。

7. RADIUS 服务器进行身份验证和授权连接尝试。

8. 如果连接尝试进行身份验证和授权，RADIUS 服务器发送访问-接受消息到 NPS RADIUS 代理，其中将其转发到访问服务器。 如果连接尝试未经过身份验证或未获得授权，RADIUS 服务器发送访问-拒绝消息到 NPS RADIUS 代理，其转发到访问服务器。

9. 访问服务器完成与访问客户端连接过程，并将记帐-请求消息发送到 NPS RADIUS 代理。 NPS RADIUS 代理记录记帐数据，并将消息转发到 RADIUS 服务器。

10. RADIUS 服务器将记帐-响应发送到 NPS RADIUS 代理，它转发到访问服务器。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。
