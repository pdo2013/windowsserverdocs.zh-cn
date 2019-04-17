---
title: 连接请求处理
description: 本主题提供在 Windows Server 2016 处理网络策略服务器连接请求的概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 849d661a-42c1-4f93-b669-6009d52aad39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6756c89dadbd01998ffef6a6d785c079076f2532
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="connection-request-processing"></a>连接请求处理

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解有关在 Windows Server 2016 中的网络策略服务器处理连接请求。

>[!NOTE]
>本主题中，除了处理文档以下连接请求才可用。
> - [连接请求策略](nps-crp-crpolicies.md)
> - [领域名称](nps-crp-realm-names.md)
> - [远程 RADIUS 服务器组](nps-crp-rrsg.md)

可以使用连接请求处理指定执行连接请求的身份验证在-在本地计算机上或以远程 RADIUS 服务器远程 RADIUS 服务器组中的成员。 

如果你想本地运行网络策略 Server (NPS) 来执行连接请求身份验证的服务器，你可以使用默认连接请求策略，而无需额外的配置。 根据默认策略，NPS 验证身份的用户和地域及受信任的域中有一个帐户的计算机。

如果你想要将连接请求转发给远程 NPS 或其他 RADIUS 服务器、创建远程 RADIUS 服务器组，然后配置转发给远程 RADIUS 服务器该组请求连接请求策略。 使用该配置，NPS 可以转发给任何 RADIUS 服务器、身份验证请求，并使用不受信任的域中的帐户的用户可以进行身份验证。

下图显示远程 RADIUS 服务器组中访问请求封电子邮件来自网络的访问权限服务器 RADIUS 代理，然后保持 RADIUS 服务器的路径。 在 RADIUS 代理服务器上，该网络的访问权限服务器配置作为 RADIUS 客户端;并且在每个 RADIUS 服务器上，RADIUS 代理配置为 RADIUS 客户端。


![NPS 连接请求处理](../../media/Nps-Connection-Request-Processing/Nps-Connection-Request-Processing.jpg)


>[!NOTE]
>使用 NPS 的网络访问权限服务器可以符合 RADIUS 协议，例如 802.1 X 的无线接入点和身份验证交换机、运行远程访问配置为 VPN 的服务器或拨号服务器、网关设备或其他 RADIUS 兼容设备。

如果你想 NPS 转移到远程 RADIUS 服务器组其他请求时的本地处理某些身份验证的请求，将配置连接的多个请求策略。

若要配置指定哪些 NPS 或 RADIUS 服务器组处理身份验证请求连接请求策略，请参阅连接请求策略。

若要指定 NPS 或请求转发给的身份验证其他 RADIUS 服务器，请参阅远程 RADIUS 服务器组。

## <a name="nps-as-a-radius-server-connection-request-processing"></a>NPS RADIUS 服务器连接请求处理用作

当你 NPS RADIUS 服务器用作，RADIUS 消息提供身份验证、授权和网络的访问连接记帐方式如下：

1. 访问服务器，如拨号网络的访问权限服务器、VPN 服务器和无线接入点，从访问客户端接收连接请求。 

2. 访问，配置为服务器身份验证、授权和记帐协议作为使用 RADIUS 创建访问请求邮件中，并将其发送到 NPS 服务器。 

3. NPS 服务器计算访问请求消息。 

4. 如有必要，NPS 服务器向访问服务器发送访问挑战消息。 访问服务器处理挑战，并向 NPS 服务器发送更新的访问权限请求。 

5. 检查用户凭据，并通过安全连接到域控制器获得是用户帐户拨号中的属性。 

6. 连接尝试授权的用户帐户和网络策略这两个刻度盘中属性。 

7. 如果连接尝试是身份验证和授权，NPS 服务器将发送到访问服务器访问接受的消息。 如果连接尝试未进行身份验证或未经授权的情况，NPS 服务器将发送到访问服务器访问拒绝消息。 

8. 访问服务器完成访问客户端与连接过程，并将会计请求消息发送到 NPS 服务器上，记录消息的位置。 

9. NPS 服务器访问服务器发送一个记帐响应。 

>[!NOTE]
>此外可以访问服务器将会计请求消息发送在连接建立、关闭访问客户端连接时，以及启动和停止访问服务器时的时间内。

## <a name="nps-as-a-radius-proxy-connection-request-processing"></a>NPS RADIUS 代理连接请求处理用作

为之间 RADIUS 客户端和 RADIUS 服务器 RADIUS 代理使用 NPS 时，网络 RADIUS 消息访问转发尝试通过以下方式连接：

1. 访问服务器，如拨号网络的访问权限服务器、虚拟专用网络 (VPN) 服务器和无线接入点，从访问客户端接收连接请求。

2. 访问，配置为服务器身份验证、授权和记帐协议作为使用 RADIUS 创建访问请求邮件中，并将其发送到正在用作 NPS RADIUS 代理 NPS 服务器。

3. NPS RADIUS 代理收到访问请求邮件，基于本地配置的连接请求策略，确定位置的访问权限请求邮件转发。

4. NPS RADIUS 代理访问请求消息转发给相应 RADIUS 服务器。

5. RADIUS 服务器计算访问请求消息。

6. 如有必要，RADIUS 服务器访问挑战向发送消息 NPS RADIUS 代理，其中转发给访问服务器。 访问服务器处理与访问客户端挑战，并了转发给 RADIUS 服务器向 NPS RADIUS 代理，发送更新的访问权限请求。

7. RADIUS 服务器身份验证，并授权连接尝试。

8. 如果连接尝试是身份验证和授权的 RADIUS 服务器访问接受向发送消息 NPS RADIUS 代理，其中转发给访问服务器。 如果连接尝试未进行身份验证或未经授权的情况，RADIUS 服务器访问拒绝向发送消息 NPS RADIUS 代理，其中转发给访问服务器。

9. 访问服务器完成访问客户端的连接过程，并将会计请求消息发送给 NPS RADIUS 代理。 NPS RADIUS 代理日志记帐数据，并将转发给 RADIUS 服务器消息。

10. RADIUS 服务器将记帐响应消息发送到 NPS RADIUS 代理，其中转发给访问服务器。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。
