---
title: 领域名称
description: 本主题提供使用网络策略服务器连接请求处理在 Windows Server 2016 中领域名称的概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 29a835c9965c2115d0aac1ef9b704e05f430db8b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="realm-names"></a>领域名称

>适用于：Windows Server（半年通道），Windows Server 2016


有关使用网络策略服务器连接请求处理领域名称的概述，你可以使用本主题。

User Name RADIUS 特性是通常包含用户帐户位置和用户帐户名的字符串。 用户帐户位置又称为领域或领域名，相当于域，包括 DNS 域的 Active Directory 的概念® 域和 Windows NT 4.0 域。 例如，如果用户帐户的用户帐户名为 example.com 域数据库中，请 example.com 是领域名称。

在另一个示例中，如果 User Name RADIUS 特性包含的用户名user1@example.com、用户 1 用户帐户名，并且 example.com 领域名称。 可以在用户名展示领域名，作为前缀或职务：

- **Example\user1**。 在此示例中，领域名**示例**是前缀;它也是 Active Directory&reg;域服务 \(AD DS\) 域。

- **user1@example.com**.在此示例中，领域名**example.com**是职务;并且它 DNS 域名称或广告 DS 域名。

可以使用领域名，配置在连接时设计和部署您 RADIUS 的基础结构的请求策略以确保连接请求经过从 RADIUS 客户端，也称为，可验证和授权连接请求 RADIUS 服务器的网络访问权限服务器。

当 NPS 配置为与默认连接请求策略 RADIUS 服务器时、NPS 处理连接请求将在其中 NPS 服务器成员域以及受信任的域。

若要配置 NPS RADIUS 代理和连接请求转发到不受信任的域用作，必须创建一个新的连接请求策略。 在新的连接要求策略中，你必须将包含在你想要向前连接请求 User Name 属性中的领域名称配置 User Name 属性。 您还必须使用远程 RADIUS 服务器组配置连接请求策略。 连接请求策略允许 NPS 计算哪个连接到远程 RADIUS 服务器组基于 User Name 特性领域部分向前请求。

## <a name="acquiring-the-realm-name"></a>获取领域名

尝试用户类型密码基于凭据期间建立连接或连接管理器（厘米）配置文件的用户的计算机上配置为自动提供领域名称时，提供用户名领域名称部分。

你可以指定键入网络连接尝试在其凭据时，你的网络的用户，提供其领域的名称。

例如，你可以要求用户键入中包括的用户帐户名和的领域名，他们用户名**User name**中**连接**对话框中进行拨号或虚拟专用网络 (VPN) 连接时。

此外，使用连接管理器管理工具包 (CMAK) 中创建拨号自定义包，如果你可以通过将领域名自动添加到用户的计算机安装的厘米配置文件中的用户帐户名帮助的用户。 例如，你可以指定领域名称和用户名语法厘米配置文件中语言，以便用户仅有键入凭据时指定的用户帐户名。 在此情况下，用户不必不知道或记住用户帐户的域。

身份验证过程中，用户键入密码基于凭据之后, 的用户名从传递访问客户端到网络的访问权限的服务器。 网络的访问权限服务器构造连接请求，并发送至 RADIUS 代理或服务器访问请求邮件中包括内 User Name RADIUS 特性领域名称。

如果 RADIUS 服务器 NPS 服务器，访问请求邮件针对配置的连接请求策略套评估。 在连接请求策略的情况可能包括 User Name 特性内容规范。

您可以配置一套连接请求策略特定于内传入的消息的 User Name 属性领域名称。 这允许你创建向前与特定领域名为特定的一组 RADIUS 服务器 RADIUS 消息 NPS RADIUS 代理用作时的路由规则。

## <a name="attribute-manipulation-rules"></a>属性操作规则

本地（时使用 NPS RADIUS 服务器）处理或转发给其他 RADIUS 服务器（NPS RADIUS 代理为使用）时 RADIUS 消息之前，消息中的 User Name 属性可以进行修改通过属性操作规则。 你可以通过选择配置 User Name 属性操作规则**User name**上**条件**连接请求策略属性选项卡。 NPS 属性操作规则使用常规语法。

您可以配置属性操作规则 User Name 属性以下更改：

- 从用户名删除领域名 \ (也称为领域 stripping\)。 例如，用户名user1@example.com更改为用户 1。

- 更改，但不是语法领域名称。 例如，用户名user1@example.com更改为user1@wcoast.example.com。

- 更改领域名的语法。 例如，用户名称 example\user1 更改为user1@example.com。

User Name 特性修改根据你配置属性操作规则后的第一个匹配的连接要求策略的其他设置用于确定是否：

- 本地（当 NPS RADIUS 服务器为使用），NPS 服务器处理访问请求的消息。

- （NPS RADIUS 代理为使用）时，NPS 服务器转发给其他 RADIUS 服务器的消息。

## <a name="configuring-the-the-nps-supplied-domain-name"></a>配置 NPS 提供域名

当域名不包含的用户名时，NPS 提供了一项。 默认情况下，NPS 提供域名是 NPS 服务器所在的域。 你可以指定 NPS 提供域名至以下注册表设置：

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>错误地编辑注册表可以严重损害你的系统。 更改注册表项之前，应备份在计算机上的所有重要数据。

某些非 Microsoft 网络访问权限服务器删除，或修改指定用户域名。 因此，网络的访问权限请求针对默认域，可能无法为用户的帐户的域进行身份验证。 若要解决此问题，将配置你 RADIUS 服务器到正确的准确域名称的格式更改的用户名。
