---
title: 领域名称
description: 本主题概述了如何在 Windows Server 2016 中的网络策略服务器连接请求处理中使用领域名称。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f9c611b793df36c2e588b2fa099df4e5382194c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405465"
---
# <a name="realm-names"></a>领域名称

>适用于：Windows Server（半年频道）、Windows Server 2016


您可以使用本主题来了解在网络策略服务器连接请求处理中使用领域名称的概述。

用户名 RADIUS 属性是一个字符串，通常包含用户帐户位置和用户帐户名称。 用户帐户位置也称为领域名称或领域名称，与域的概念（包括 DNS 域、Active Directory®域和 Windows NT 4.0 域）同义。 例如，如果用户帐户位于名为 example.com 的域的用户帐户数据库中，则 example.com 是领域名称。

在另一个示例中，如果用户名 RADIUS 属性包含用户名 user1@example.com，user1 是用户帐户名，example.com 是领域名称。 可以在用户名中以前缀或后缀形式显示领域名称：

- **Example\user1**。 在此示例中，领域名称**示例**是前缀;它也是 Active Directory @ no__t-1 域服务 \(AD DS @ no__t 域的名称。

- <strong>user1@example.com</strong>： 在此示例中，领域名称**example.com**为后缀;它可以是 DNS 域名或 AD DS 域的名称。

设计和部署 RADIUS 基础结构时，你可以使用在连接请求策略中配置的领域名称，以确保将连接请求从 RADIUS 客户端（也称为网络访问服务器）路由到可对连接请求进行身份验证和授权。

当 NPS 配置为具有默认连接请求策略的 RADIUS 服务器时，NPS 将处理 NPS 所属的域和受信任域的连接请求。

若要将 NPS 配置为充当 RADIUS 代理，并将连接请求转发到不受信任的域，必须创建新的连接请求策略。 在新的连接请求策略中，你必须用将包含在要转发的连接请求的用户名属性中的领域名称来配置用户名属性。 还必须使用远程 RADIUS 服务器组配置连接请求策略。 连接请求策略允许 NPS 根据用户名属性的领域部分来计算要转发到远程 RADIUS 服务器组的连接请求。

## <a name="acquiring-the-realm-name"></a>获取领域名称

当用户在连接尝试期间或在用户计算机上的连接管理器（CM）配置文件配置为自动提供领域名称时，将提供用户名的领域名称部分。

你可以指定网络用户在网络连接尝试期间键入其凭据时提供其领域名称。

例如，你可以要求用户在进行拨号或虚拟专用网络（VPN）连接时，在 "**连接**" 对话框的 "**用户名**" 中键入其用户名，包括用户帐户名称和领域名称。

此外，如果使用连接管理器管理工具包（CMAK）创建自定义拨号包，你可以通过将领域名称自动添加到用户计算机上安装的 CM 配置文件中的用户帐户名来帮助用户。 例如，可以在 CM 配置文件中指定领域名称和用户名语法，以便用户在键入凭据时只需要指定用户帐户名。 在这种情况下，用户不需要知道或记住其用户帐户所在的域。

在身份验证过程中，用户键入基于密码的凭据后，用户名将从访问客户端传递到网络访问服务器。 网络访问服务器会构造连接请求，并在发送到 RADIUS 代理或服务器的访问请求消息中的用户名 RADIUS 属性内包含领域名称。

如果 RADIUS 服务器是 NPS，则会根据配置的连接请求策略集来评估访问请求消息。 连接请求策略的条件可以包含用户名属性的内容规范。

你可以配置一组特定于传入消息的用户名属性中的领域名称的连接请求策略。 这样，当 NPS 用作 RADIUS 代理时，可以创建路由规则，以便将具有特定领域名称的 RADIUS 消息转发到一组特定的 RADIUS 服务器。

## <a name="attribute-manipulation-rules"></a>特性操作规则

在本地处理 RADIUS 消息（将 NPS 用作 RADIUS 服务器时）或将其转发到其他 RADIUS 服务器（将 NPS 用作 RADIUS 代理时）之前，可以通过属性操作规则修改消息中的用户名属性。 您可以通过在连接请求策略属性中的 "**条件**" 选项卡上选择 "**用户名**"，为用户名属性配置属性操作规则。 NPS 特性操作规则使用正则表达式语法。

您可以为用户名属性配置属性操作规则，以更改以下各项：

- 从用户名中删除领域名称 @no__t 0also 称为领域去除 @ no__t-1。 例如，用户名 user1@example.com 将更改为 user1。

- 更改领域名称，但不更改其语法。 例如，用户名 user1@example.com 将更改为 user1@wcoast.example.com。

- 更改领域名称的语法。 例如，用户名 example\user1 更改为 user1@example.com。

根据您配置的属性操作规则修改用户名属性后，将使用第一个匹配的连接请求策略的其他设置来确定是否：

- NPS 在本地处理访问请求消息（将 NPS 用作 RADIUS 服务器时）。

- NPS 将消息转发到另一个 RADIUS 服务器（当 NPS 用作 RADIUS 代理时）。

## <a name="configuring-the-nps-supplied-domain-name"></a>配置 NPS 提供的域名

如果用户名不包含域名，则 NPS 会提供一个域名称。 默认情况下，NPS 提供的域名是 NPS 所属的域。 可以通过以下注册表设置指定 NPS 提供的域名：

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>注册表编辑不当可能会严重损坏系统。 在更改注册表之前，应备份计算机上任何有价值的数据。

某些非 Microsoft 网络访问服务器删除或修改用户指定的域名。 因此，网络访问请求是根据默认域进行身份验证的，该域可能不是用户帐户的域。 若要解决此问题，请将 RADIUS 服务器配置为将用户名更改为具有准确域名的正确格式。
