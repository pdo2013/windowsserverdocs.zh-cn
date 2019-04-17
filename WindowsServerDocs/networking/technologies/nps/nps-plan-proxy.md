---
title: 计划 NPS RADIUS 代理作为
description: 本主题提供的有关 Windows Server 2016 中计划的网络策略服务器 RADIUS 代理部署的信息。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ca77d64a-065b-4bf2-8252-3e75f71b7734
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 966e555ebcac6c7daf4a26b322f0d29f023f8539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="plan-nps-as-a-radius-proxy"></a>计划 NPS RADIUS 代理作为

>适用于：Windows Server（半年通道），Windows Server 2016

部署为 \(RADIUS\) 用户服务拨号中远程身份验证的代理服务器的网络策略服务器 (NPS) 时，NPS 接收连接请求从 RADIUS 客户端，如网络访问服务器或其他 RADIUS 代理服务器，然后将这些连接请求 NPS 或其他 RADIUS 服务器运行的服务器。 你可以使用这些规划指导简化 RADIUS 部署。

这些规划指南中不包括情况下你希望将其部署 NPS RADIUS 服务器用作。 当部署 NPS RADIUS 服务器用作时，NPS 连接请求地域对和信任本地域的为执行身份验证、 授权和记帐。

NPS RADIUS 代理作为部署网络上之前，请使用以下指南计划部署。

- 计划 NPS server 配置。

- 计划 RADIUS 客户端。

- 计划远程 RADIUS 服务器组。

- 计划消息发送属性操作的规则。

- 计划连接请求策略。

- 套餐 NPS 记帐。

## <a name="plan-nps-server-configuration"></a>套餐 NPS 服务器配置

当你 NPS RADIUS 代理用作、 NPS 转发连接到 NPS 服务器或其他 RADIUS 服务器以处理的请求。 出于此原因，NPS 代理域会员无关。 代理无需注册 Active Directory 域服务 \(AD DS\) 在因为它不需要拨号中属性用户帐户的访问权限。 此外，不需要上 NPS 代理配置网络策略，因为代理不会执行授权连接请求。 它可以与没有域会员独立服务器或 NPS 代理可以加入域。

必须配置 NPS RADIUS 客户端，也称为使用 RADIUS 协议的网络访问权限服务器，与进行通信。 此外，您可以配置的事件类型，用于 NPS 记录事件日志中，并且你可以输入服务器的说明。

### <a name="key-steps"></a>关键步骤

在计划 NPS 代理配置的过程中，你可以使用下面的步骤。

- 确定 NPS 代理使用从 RADIUS 客户端接收 RADIUS 消息并将 RADIUS 消息发送给远程 RADIUS 服务器组成员 RADIUS 端口。 默认用户数据报协议 (UDP) 端口是 1812年和 RADIUS 身份验证消息和 UDP 端口 1813年和 RADIUS 会计消息的 1646年 1645年。

- 如果 NPS 代理配置了多个网络适配器，确定哪些你想要允许的 RADIUS 流量的适配器。

- 确定你希望记录的事件日志中 NPS 的活动类型。 你可以登录的请求被拒绝的连接、 连接成功请求，或两者都。

- 确定是否要部署多个 NPS 代理。 若要提供故障能力，使用至少两个 NPS 代理。 一个 NPS 代理用作主要 RADIUS 代理并用另作为备份。 然后在这两个 NPS 代理配置每个 RADIUS 客户端。 如果主要 NPS 代理按钮不可用，RADIUS 客户端将访问请求消息发送给备用 NPS 代理。

- 计划脚本用于复制到其他 NPS 代理服务器管理开销上保存并阻止不正确配置服务器的一种 NPS 的代理配置。 NPS 提供允许您将复制的全部或部分要导入到另一个 NPS 代理 NPS 代理配置 Netsh 命令。 Netsh 提示时，你可以手动运行命令。 但是，如果你将命令序列另存为脚本，你可以运行脚本晚些时候如果你决定更改你的代理配置。

## <a name="plan-radius-clients"></a>套餐 RADIUS 客户端

RADIUS 客户端是如无线接入点的网络访问权限服务器虚拟专用网络 \(VPN\) 服务器，802.1 X 支持开关，并拨号服务器。 RADIUS 的代理服务器，向前连接到 RADIUS 服务器请求消息，也是 RADIUS 客户端。 NPS 支持所有网络的访问权限服务器和 RADIUS 代理服务器遵守 RADIUS 协议，述 RFC 2865"远程拨号在用户身份验证服务 \(RADIUS\)，"，RFC 可"RADIUS 记帐。"

此外，无线接入点和开关必须 802.1 X 身份验证的支持。 如果你想要部署可扩展身份验证协议 (EAP) 或保护可扩展身份验证协议 (PEAP)，接入点和开关必须支持 EAP 使用。

测试 PPP 连接的无线接入点的基本互操作性、 配置接入点和访问客户端使用密码协议身份验证 (PAP)。 使用其他 PPP 基于身份验证的协议 PEAP，例如，直到经测试所想要使用的网络访问权限。

### <a name="key-steps"></a>关键步骤

在规划 RADIUS 客户端的过程中，你可以使用下面的步骤。

- 你必须配置中 NPS 文档特定供应商属性 (Vsa)。 网络策略配置中 NPS 时，如果你的 Nas 需要 Vsa，请登录 VSA 信息以供以后使用。

- 记录 RADIUS 客户端并 NPS 代理简化配置的所有设备的 IP 地址。 部署你 RADIUS 客户端时，你必须配置他们使用 RADIUS 协议，身份验证的服务器按输入的 NPS 代理 IP 地址。 和 NPS RADIUS 的客户端与通信配置时，你必须 NPS 贴靠到输入 RADIUS 客户端 IP 地址。

- RADIUS 客户端和 NPS 贴靠中创建配置共享机密信息。 你必须使用共享的机密或密码，你还将在 NPS 贴靠的配置中 NPS RADIUS 客户端时所输入配置 RADIUS 客户端。

## <a name="plan-remote-radius-server-groups"></a>计划远程 RADIUS 服务器组

上 NPS 代理配置远程 RADIUS 服务器组时，会告知 NPS 代理发送收到来自网络的访问权限服务器和 NPS 代理或其他 RADIUS 代理服务器的请求消息的部分或所有连接的位置。

你可以使用 NPS RADIUS 代理转发连接到某个的请求，也更远程 RADIUS 服务器组，以及每一组可以包含一个或多个 RADIUS 服务器。 要 NPS 代理邮件转发到多个组中，当配置每个组一个连接请求策略。 连接请求策略包含其他信息，如属性操作规则，告诉 NPS 代理哪些消息发送到远程 RADIUS 服务器组策略中指定。

使用适用于 NPS Netsh 命令、 配置组直接在 NPS 贴靠中下远程 RADIUS 服务器组中，或运行的新连接申请策略向导中，您可以配置远程 RADIUS 服务器组。

### <a name="key-steps"></a>关键步骤

在规划远程 RADIUS 服务器组的过程中，你可以使用下面的步骤。

- 确定包含要向其中 NPS 代理转发连接请求 RADIUS 服务器的域。 这些域包含通过连接到网络你部署 RADIUS 客户端的用户的用户帐户。

- 确定是否需要在其中 RADIUS 不已部署的域中添加新 RADIUS 服务器。

- 记录你想要添加到远程 RADIUS 服务器组 RADIUS 服务器的 IP 的地址。

- 确定你需要先创建的多少远程 RADIUS 服务器组。 在某些情况下，最好创建域，每一个远程 RADIUS 服务器组，然后将 RADIUS 服务器域添加的组。 但是，可能存在情况下一个域，包括大量的用户帐户的用户在域、 大量的域控制器和大量 RADIUS 服务器中有大量的资源。 或者你域可能键盘盖较大的地理区域，导致用户在彼此之间的距离较远的位置有网络访问权限服务器和 RADIUS 服务器。 在这些可能其他情况下，你可以创建多个远程的 RADIUS 服务器组每个域。

- 创建配置共享的机密，NPS 代理和在远程 RADIUS 服务器上。

## <a name="plan-attribute-manipulation-rules-for-message-forwarding"></a>计划消息发送属性操作的规则

在连接请求策略配置的属性操作规则允许你以找出你想要为某个特定远程 RADIUS 服务器组向前访问请求邮件。

您可以配置 NPS 转发给一个远程 RADIUS 服务器组所有连接的请求，而无需使用属性操作规则。

如果你有多个位置为你想要向前连接请求，但是，必须创建连接请求策略为每个位置，，然后使用到你希望将消息转发远程 RADIUS 服务器组或者告诉 NPS 哪些邮件转发属性操作规则配置策略。

你可以创建规则以下属性。

- 称为站 id。 电话号码的网络访问权限服务器 (NAS)。 此属性的价值是一个字符串。 你可以使用模式匹配语法指定地区代码。

- 呼叫-站-id。 呼叫方使用电话号码。 此属性的价值是一个字符串。 你可以使用模式匹配语法指定地区代码。

- 用户名。 由访问客户端，而且 nas RADIUS 访问请求邮件中包含用户的名称。 此属性的价值是，通常包含领域名称和用户帐户名的字符串。

若要正确替换或转换领域中连接请求的用户名的名称，必须上的相应连接请求策略配置属性操作规则 User Name 属性。

### <a name="key-steps"></a>关键步骤

在计划属性操作规则的过程中，你可以使用下面的步骤。

- 计划消息从通过代理 NAS 路由到远程 RADIUS 服务器来验证你有一个逻辑路径 RADIUS 服务器邮件转发。

- 确定你想要使用的每个连接的请求策略的一个或多个属性。

- 文档你打算使用的每个连接请求策略，属性操作规则，并且匹配到邮件转发远程 RADIUS 服务器组规则。

## <a name="plan-connection-request-policies"></a>计划连接请求策略

当用作 RADIUS 服务器 nps 配置默认连接请求策略。 连接其他请求策略可以用于定义更多具体情况，创建特性操作规则告诉 NPS 哪些邮件转发给远程 RADIUS 服务器组中，并指定高级的属性。 使用新的连接要求策略向导请求策略创建通用或自定义的连接。

### <a name="key-steps"></a>关键步骤

在计划连接请求策略的过程中，你可以使用下面的步骤。

- 删除的每个单独为 RADIUS 代理运行 NPS 该功能的服务器上的默认连接请求策略。

- 计划其他条件和所需的每个策略，与远程 RADIUS 服务器组策略计划属性操作规则组合此信息的设置。

- 设计分发常见的连接请求策略为所有 NPS 的代理服务器的套餐。 创建通用多个 NPS 的代理服务器的策略，在一个 NPS 服务器上，并使用用于 NPS Netsh 命令导入所有其他代理服务器上的连接要求策略和服务器配置。

## <a name="plan-nps-accounting"></a>套餐 NPS 记帐

NPS RADIUS 代理作为配置时，你可以将其执行日志记录记帐使用 NPS 格式日志文件、 数据库兼容的格式日志文件中或 NPS SQL Server RADIUS 配置。

你还可以对通过这些日志记录格式执行记帐远程 RADIUS 服务器组转发记帐消息。

### <a name="key-steps"></a>关键步骤

在计划 NPS 会计的过程中，你可以使用下面的步骤。

- 确定是否要 NPS 代理执行记帐服务或记帐邮件转发到记帐远程 RADIUS 服务器组。

- 计划禁用本地 NPS 代理记帐，如果你计划转发给其他服务器记帐消息。

- 如果你计划转发给其他服务器记帐消息，计划连接请求策略配置步骤。 禁用 NPS 代理本地记帐，如果你在该代理配置每个连接请求策略必须记帐邮件转发启用，并且正确配置。

- 确定你想要使用的日志记录格式： IAS 格式日志文件、 数据库兼容的格式日志文件中或 NPS SQL Server 日志记录。

若要配置负载平衡 nps RADIUS 代理作为，请参阅[NPS 的代理服务器负载平衡](nps-manage-proxy-lb.md)。

