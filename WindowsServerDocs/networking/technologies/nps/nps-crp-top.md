---
title: 连接请求处理
description: 本主题概述了 Windows Server 2016 中的网络策略服务器连接请求处理。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 268a307336d9a4fae1be5621eec7c32a06a6204e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405398"
---
# <a name="connection-request-processing"></a>连接请求处理

>适用于：Windows Server（半年频道）、Windows Server 2016

您可以使用本主题来了解 Windows Server 2016 中网络策略服务器的连接请求处理。

>[!NOTE]
>除了本主题之外，还提供了以下连接请求处理文档。
> - [连接请求策略](nps-crp-crpolicies.md)
> - [领域名称](nps-crp-realm-names.md)
> - [远程 RADIUS 服务器组](nps-crp-rrsg.md)

您可以使用连接请求处理来指定对连接请求进行身份验证的位置-在本地计算机上或在作为远程 RADIUS 服务器组成员的远程 RADIUS 服务器上。 

如果希望运行网络策略服务器（NPS）的本地服务器对连接请求执行身份验证，则可以使用默认连接请求策略，而无需进行其他配置。 NPS 根据默认策略，对在本地域和受信任域中具有帐户的用户和计算机进行身份验证。

如果要将连接请求转发到远程 NPS 或其他 RADIUS 服务器，请创建一个远程 RADIUS 服务器组，然后配置将请求转发到该远程 RADIUS 服务器组的连接请求策略。 通过此配置，NPS 可以将身份验证请求转发到任何 RADIUS 服务器，并且可以对不受信任域中的帐户的用户进行身份验证。

下图显示了从网络访问服务器到 RADIUS 代理，然后再到远程 RADIUS 服务器组中的 RADIUS 服务器的访问请求消息的路径。 在 RADIUS 代理上，将网络访问服务器配置为 RADIUS 客户端;对于每个 RADIUS 服务器，将 RADIUS 代理配置为 RADIUS 客户端。


![NPS 连接请求处理](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>用于 NPS 的网络访问服务器可以是符合 RADIUS 协议的网关设备，如 802.1 X 无线访问点和身份验证交换机、运行远程访问（配置为 VPN 或拨号服务器）的服务器，或者其他与 RADIUS 兼容的设备。

如果希望 NPS 在将其他请求转发到远程 RADIUS 服务器组时在本地处理一些身份验证请求，请配置多个连接请求策略。

若要配置连接请求策略以指定哪个 NPS 或 RADIUS 服务器组处理身份验证请求，请参阅连接请求策略。

若要指定将身份验证请求转发到的 NPS 或其他 RADIUS 服务器，请参阅远程 RADIUS 服务器组。

## <a name="nps-as-a-radius-server-connection-request-processing"></a>作为 RADIUS 服务器连接请求处理的 NPS

将 NPS 用作 RADIUS 服务器时，RADIUS 消息按以下方式为网络访问连接提供身份验证、授权和记帐：

1. 访问服务器（如拨号网络访问服务器、VPN 服务器和无线访问点）接收来自 access 客户端的连接请求。 

2. 访问服务器（配置为使用 RADIUS 作为身份验证、授权和记帐协议）将创建访问请求消息并将其发送到 NPS。 

3. NPS 会评估访问请求消息。 

4. 如果需要，NPS 会向访问服务器发送一条访问质询消息。 访问服务器将处理质询，并将更新的访问请求发送到 NPS。 

5. 将检查用户凭据，并使用与域控制器的安全连接来获取用户帐户的拨入属性。 

6. 通过用户帐户的拨入属性和网络策略对连接尝试进行授权。 

7. 如果连接尝试同时经过身份验证和授权，则 NPS 会将访问-接受消息发送到访问服务器。 如果未对连接尝试进行身份验证或授权，则 NPS 会将访问-拒绝消息发送到访问服务器。 

8. 访问服务器完成了与访问客户端的连接过程，并将一个记帐请求消息发送到 NPS，其中记录了消息。 

9. NPS 将记帐-响应发送到访问服务器。 

>[!NOTE]
>访问服务器还会在建立连接时、访问客户端连接关闭以及访问服务器启动和停止时发送记帐请求消息。

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>作为 RADIUS 代理连接请求处理的 NPS

当 NPS 用作 RADIUS 客户端和 RADIUS 服务器之间的 RADIUS 代理时，将按以下方式转发网络访问连接尝试的 RADIUS 消息：

1. 访问服务器（如拨号网络访问服务器、虚拟专用网络（VPN）服务器和无线访问点）将从访问客户端接收连接请求。

2. 访问服务器（配置为使用 RADIUS 作为身份验证、授权和记帐协议）会创建访问请求消息，并将其发送到用作 NPS RADIUS 代理的 NPS。

3. NPS RADIUS 代理接收访问请求消息，并根据本地配置的连接请求策略，确定要将访问请求消息转发到何处。

4. NPS RADIUS 代理将访问请求消息转发到相应的 RADIUS 服务器。

5. RADIUS 服务器评估访问请求消息。

6. 如果需要，RADIUS 服务器将向 NPS RADIUS 代理发送访问质询消息，并将其转发到访问服务器。 访问服务器使用访问客户端处理质询，并将更新的访问请求发送到 NPS RADIUS 代理，并将其转发到 RADIUS 服务器。

7. RADIUS 服务器对连接尝试进行身份验证和授权。

8. 如果连接尝试同时经过身份验证和授权，则 RADIUS 服务器会将访问-接受消息发送到 NPS RADIUS 代理，并将其转发到访问服务器。 如果连接尝试未经身份验证或未授权，则 RADIUS 服务器将向 NPS RADIUS 代理发送访问-拒绝消息，并将其转发到访问服务器。

9. 访问服务器使用访问客户端完成连接过程，并向 NPS RADIUS 代理发送记帐请求消息。 NPS RADIUS 代理记录记帐数据，并将消息转发到 RADIUS 服务器。

10. RADIUS 服务器将记帐响应发送到 NPS RADIUS 代理，并将其转发到访问服务器。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。
