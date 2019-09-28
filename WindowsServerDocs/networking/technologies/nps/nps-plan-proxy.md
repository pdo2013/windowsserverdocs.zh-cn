---
title: 将 NPS 规划为 RADIUS 代理
description: 本主题提供有关 Windows Server 2016 中的网络策略服务器 RADIUS 代理部署规划的信息。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29a48275dfd56cbf223e0fca0c9c276f35a675cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396022"
---
# <a name="plan-nps-as-a-radius-proxy"></a>将 NPS 规划为 RADIUS 代理

>适用于：Windows Server（半年频道）、Windows Server 2016

将网络策略服务器（NPS）作为远程身份验证拨入用户服务 \(RADIUS @ no__t 代理部署时，NPS 会接收来自 RADIUS 客户端的连接请求，例如网络访问服务器或其他 RADIUS 代理，然后将这些请求转发到对运行 NPS 或其他 RADIUS 服务器的服务器的连接请求。 您可以使用这些规划指南来简化您的 RADIUS 部署。

这些规划准则不包括希望将 NPS 部署为 RADIUS 服务器的情况。 将 NPS 部署为 RADIUS 服务器时，NPS 将为本地域和信任本地域的域的连接请求执行身份验证、授权和记帐。

在你的网络上将 NPS 部署为 RADIUS 代理之前，请使用以下准则来规划你的部署。

- 规划 NPS 配置。

- 规划 RADIUS 客户端。

- 规划远程 RADIUS 服务器组。

- 规划消息转发的属性操作规则。

- 规划连接请求策略。

- 规划 NPS 计帐。

## <a name="plan-nps-configuration"></a>规划 NPS 配置

将 NPS 用作 RADIUS 代理时，NPS 会将连接请求转发到 NPS 或其他 RADIUS 服务器进行处理。 因此，NPS 代理的域成员身份是不相关的。 不需要在 Active Directory 域服务 \(AD DS @ no__t-1 中注册代理，因为它不需要访问用户帐户的拨入属性。 此外，无需在 NPS 代理上配置网络策略，因为代理不对连接请求进行授权。 NPS 代理可以是域成员，也可以是没有域成员身份的独立服务器。

必须将 NPS 配置为使用 RADIUS 协议与 RADIUS 客户端（也称为网络访问服务器）进行通信。 此外，你还可以配置 NPS 记录在事件日志中的事件类型，并且可以输入服务器的说明。

### <a name="key-steps"></a>关键步骤

在规划 NPS 代理配置的过程中，可以使用以下步骤。

- 确定 NPS 代理用于接收来自 RADIUS 客户端的 RADIUS 消息的 RADIUS 端口，并用于向远程 RADIUS 服务器组的成员发送 RADIUS 消息。 RADIUS 身份验证消息的默认用户数据报协议（UDP）端口为1812，1645用于 RADIUS 记帐消息的 UDP 端口1813和1646。

- 如果使用多个网络适配器配置了 NPS 代理，请确定允许 RADIUS 流量使用的适配器。

- 确定希望 NPS 在事件日志中记录的事件类型。 可以记录拒绝的连接请求、成功的连接请求，或同时记录两者。

- 确定是否要部署多个 NPS 代理。 若要提供容错，请使用至少两个 NPS 代理。 一个 NPS 代理用作主 RADIUS 代理，另一个用作备份。 然后，在两个 NPS 代理上都配置每个 RADIUS 客户端。 如果主 NPS 代理变为不可用，则 RADIUS 客户端会将访问请求消息发送到备用 NPS 代理。

- 规划脚本，该脚本用于将一个 NPS 代理配置复制到其他 NPS 代理以节省管理开销，并阻止服务器的配置不正确。 NPS 提供 Netsh 命令，可用于复制全部或部分 NPS 代理配置以导入到另一个 NPS 代理。 可以在 Netsh 提示符下手动运行这些命令。 但是，如果你将命令序列另存为脚本，则可以在以后运行该脚本，前提是你决定更改代理配置。

## <a name="plan-radius-clients"></a>规划 RADIUS 客户端

RADIUS 客户端是网络访问服务器，例如无线访问点、虚拟专用网络 \(VPN @ no__t 服务器、802.1 支持 X 的交换机和拨号服务器。 将连接请求消息转发到 RADIUS 服务器的 RADIUS 代理也是 RADIUS 客户端。 NPS 支持符合 RADIUS 协议的所有网络访问服务器和 RADIUS 代理，如 RFC 2865 "远程身份验证拨入用户服务 \(RADIUS @ no__t-1" 中所述，RFC 2866 "RADIUS 记帐"。

此外，无线访问点和交换机都必须能够 802.1 X 身份验证。 如果要部署可扩展身份验证协议（EAP）或受保护的可扩展身份验证协议（PEAP），则访问点和交换机必须支持使用 EAP。

若要为无线访问点的 PPP 连接测试基本互操作性，请将访问点和访问客户端配置为使用密码身份验证协议（PAP）。 使用其他基于 PPP 的身份验证协议（如 PEAP），直到你测试了你打算用于网络访问的身份验证协议。

### <a name="key-steps"></a>关键步骤

在规划 RADIUS 客户端的过程中，可以使用以下步骤。

- 记录必须在 NPS 中配置的供应商特定属性（Vsa）。 如果 Nas 需要 Vsa，则在 NPS 中配置网络策略时，记录 VSA 信息以供以后使用。

- 记录 RADIUS 客户端和 NPS 代理的 IP 地址，以简化所有设备的配置。 部署 RADIUS 客户端时，必须将其配置为使用 RADIUS 协议，并将 NPS 代理 IP 地址作为身份验证服务器输入。 将 NPS 配置为与 RADIUS 客户端通信时，必须在 NPS 管理单元中输入 RADIUS 客户端 IP 地址。

- 在 RADIUS 客户端和 NPS 管理单元中创建用于配置的共享机密。 在 NPS 中配置 RADIUS 客户端时，你必须使用共享机密或密码配置 RADIUS 客户端。

## <a name="plan-remote-radius-server-groups"></a>规划远程 RADIUS 服务器组

当你在 NPS 代理上配置远程 RADIUS 服务器组时，你会告诉 NPS 代理从网络访问服务器和 NPS 代理或其他 RADIUS 代理发送部分或全部连接请求消息的位置。

可以将 NPS 用作 RADIUS 代理，将连接请求转发到一个或多个远程 RADIUS 服务器组，每个组可以包含一个或多个 RADIUS 服务器。 如果希望 NPS 代理将消息转发给多个组，请为每个组配置一个连接请求策略。 连接请求策略包含其他信息，如属性操作规则，这些信息通知 NPS 代理要发送到策略中指定的远程 RADIUS 服务器组的消息。

可以通过使用 NPS 的 Netsh 命令来配置远程 RADIUS 服务器组，方法是直接在 "远程 RADIUS 服务器组" 下的 NPS 管理单元中配置组，或者运行新建连接请求策略向导。

### <a name="key-steps"></a>关键步骤

在规划远程 RADIUS 服务器组的过程中，可以使用以下步骤。

- 确定包含希望 NPS 代理转发连接请求的 RADIUS 服务器的域。 这些域包含通过部署的 RADIUS 客户端连接到网络的用户的用户帐户。

- 确定是否需要在尚未部署 RADIUS 的域中添加新的 RADIUS 服务器。

- 记录要添加到远程 RADIUS 服务器组中的 RADIUS 服务器的 IP 地址。

- 确定需要创建多少个远程 RADIUS 服务器组。 在某些情况下，最好为每个域创建一个远程 RADIUS 服务器组，然后将域的 RADIUS 服务器添加到组中。 但是，在某些情况下，在一个域中有大量的资源，包括用户帐户在域中的用户帐户、大量域控制器和大量 RADIUS 服务器。 或者，您的域可能涵盖了一个大的地理区域，导致您将网络访问服务器和 RADIUS 服务器置于远离远处的位置。 在这些情况和其他情况下，可以为每个域创建多个远程 RADIUS 服务器组。

- 在 NPS 代理和远程 RADIUS 服务器上创建用于配置的共享机密。

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>规划消息转发的特性操作规则

使用在连接请求策略中配置的属性操作规则，可以标识要转发到特定远程 RADIUS 服务器组的访问请求消息。

可以将 NPS 配置为将所有连接请求转发到一个远程 RADIUS 服务器组，而无需使用特性操作规则。

但是，如果你想要将连接请求转发到多个位置，则必须为每个位置创建一个连接请求策略，然后将该策略配置为要将消息转发到的远程 RADIUS 服务器组以及带有用于告知 NPS 要转发哪些消息的属性操作规则。

可以为以下属性创建规则。

- 接收站 ID。 网络访问服务器（NAS）的电话号码。 此属性的值为字符串。 您可以使用模式匹配语法来指定区号。

- 调用工作站 ID。 调用方使用的电话号码。 此属性的值为字符串。 您可以使用模式匹配语法来指定区号。

- 用户名。 由 access 客户端提供并且在 RADIUS 访问请求消息中由 NAS 包含的用户名。 此属性的值是一个字符串，通常包含领域名称和用户帐户名称。

若要正确地替换或转换连接请求的用户名中的领域名称，必须在相应的连接请求策略中为用户名属性配置属性操作规则。

### <a name="key-steps"></a>关键步骤

在规划特性操作规则的过程中，可以使用以下步骤。

- 规划从 NAS 到远程 RADIUS 服务器的消息路由，验证是否具有将消息转发到 RADIUS 服务器的逻辑路径。

- 确定要用于每个连接请求策略的一个或多个属性。

- 记录计划用于每个连接请求策略的属性操作规则，并将规则与要将消息转发到的远程 RADIUS 服务器组匹配。

## <a name="plan-connection-request-policies"></a>规划连接请求策略

将 NPS 用作 RADIUS 服务器时，会为 NPS 配置默认连接请求策略。 可以使用其他连接请求策略来定义更具体的条件，创建属性操作规则，告诉 NPS 要转发到远程 RADIUS 服务器组的消息，以及指定高级属性。 使用新建连接请求策略向导创建常见或自定义连接请求策略。

### <a name="key-steps"></a>关键步骤

在规划连接请求策略的过程中，可以使用以下步骤。

- 删除每台运行 NPS 的服务器上的默认连接请求策略，该服务器仅充当 RADIUS 代理。

- 规划每个策略所需的其他条件和设置，并将此信息与远程 RADIUS 服务器组以及为策略计划的属性操作规则结合起来。

- 设计计划以便将常见连接请求策略分发到所有 NPS 代理。 创建一个 NPS 上的多个 NPS 代理公用的策略，然后使用适用于 NPS 的 Netsh 命令导入所有其他代理上的连接请求策略和服务器配置。

## <a name="plan-nps-accounting"></a>规划 NPS 记帐

将 NPS 配置为 RADIUS 代理时，可以通过使用 NPS 格式日志文件、数据库兼容格式日志文件或 NPS SQL Server 日志记录将其配置为执行 RADIUS 记帐。

还可以通过使用其中一种日志记录格式将记帐消息转发到执行记帐的远程 RADIUS 服务器组。

### <a name="key-steps"></a>关键步骤

在规划 NPS 记帐期间，可以使用以下步骤。

- 确定是否希望 NPS 代理执行记帐服务，或将记帐消息转发到远程 RADIUS 服务器组进行记帐。

- 如果打算将记帐消息转发到其他服务器，则计划禁用本地 NPS 代理记帐。

- 如果打算将记帐消息转发到其他服务器，请规划连接请求策略配置步骤。 如果禁用 NPS 代理的本地记帐，则在该代理上配置的每个连接请求策略都必须启用并正确配置了记帐消息转发。

- 确定要使用的日志记录格式：IAS 格式的日志文件、数据库兼容的格式日志文件或 NPS SQL Server 日志记录。

若要将 NPS 的负载平衡配置为 RADIUS 代理，请参阅[NPS 代理服务器负载平衡](nps-manage-proxy-lb.md)。

