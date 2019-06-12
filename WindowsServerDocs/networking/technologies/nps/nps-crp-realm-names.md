---
title: 领域名称
description: 本主题概述了在网络策略服务器的连接请求处理在 Windows Server 2016 中使用领域名称。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d011eaad-f72a-4a83-8099-8589c4ee8994
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 65a272873a60d74efcf417a16fdc84670f5878da
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447001"
---
# <a name="realm-names"></a>领域名称

>适用于：Windows 服务器 （半年频道），Windows Server 2016


本主题可用于在网络策略服务器连接请求处理过程中使用领域名称的概述。

用户名 RADIUS 属性是通常包含用户帐户位置和用户帐户名称的字符串。 用户帐户位置也称为领域或领域名称和域，其中包括 DNS 域、 Active Directory® 域和 Windows NT 4.0 域的概念与是同义词。 例如，如果位于名为 example.com 的域用户帐户数据库中的用户帐户，则 example.com 是领域名称。

在另一个示例中，如果用户名 RADIUS 属性包含的用户名称user1@example.com、 user1 是用户帐户名称和 example.com 是领域名称。 领域名称可以表示用户名称作为前缀或后缀：

- **Example\user1**。 在此示例中，领域名称**示例**是前缀; 并且它也是 Active Directory 名称&reg;域服务\(AD DS\)域。

- <strong>user1@example.com</strong>. 在此示例中，领域名称**example.com**是后缀; 并且它是 DNS 域名或 AD DS 域的名称。

可以使用在设计和部署 RADIUS 基础结构的同时在连接请求策略中配置的领域名称以确保连接请求路由从 RADIUS 客户端，也称为网络访问服务器，到可的 RADIUS 服务器身份验证和授权连接请求。

当 NPS 配置为 RADIUS 服务器使用默认连接请求策略时，NPS 会处理连接请求中 NPS 是成员的域和受信任的域。

若要配置 NPS 充当 RADIUS 代理和连接请求转发到不受信任域，必须创建新的连接请求策略。 在新的连接请求策略中，必须使用领域名称将包含你想要转发的连接请求的用户名称属性中配置用户名属性。 此外必须与远程 RADIUS 服务器组配置连接请求策略。 连接请求策略允许 NPS 计算哪些连接请求转发到远程 RADIUS 服务器组根据用户名属性的领域部分。

## <a name="acquiring-the-realm-name"></a>获取领域名称

当尝试连接期间的用户类型基于密码的凭据或用户的计算机上的连接管理器 (CM) 配置文件配置为自动提供领域名称时提供的用户名的领域名称部分。

可以将你的网络的用户提供其领域名称，在网络连接尝试期间键入其凭据时指定。

例如，可以要求用户键入其用户名，包括中的用户帐户名和领域名称**用户名**中**Connect**对话框进行拨号或虚拟专用网络 (VPN) 时连接。

此外，如果使用连接管理器管理工具包 (CMAK) 创建自定义拨号程序包，你可以帮助用户通过自动将领域名称添加到用户的计算机安装的 CM 配置文件中的用户帐户名称。 例如，可以在 CM 配置文件中指定领域名称和用户名称语法，以便用户只具有键入凭据时指定的用户帐户名称。 在此情况下，用户不必知道或记住其用户帐户所在的域。

在身份验证过程中，用户键入其基于密码的凭据之后, 用户传递的名称是从访问客户端网络访问服务器。 网络访问服务器构造连接请求并发送到 RADIUS 代理或服务器的访问请求消息中包括用户名 RADIUS 属性中的领域名称。

如果 RADIUS 服务器，NPS 针对已配置的连接请求策略的集计算访问-请求消息。 在连接请求策略条件可以包括用户-名称属性的内容的规范。

可以配置一组特定于传入消息用户名属性中的领域名称的连接请求策略。 这样，您可以创建当 NPS 用作 RADIUS 代理时使用特定领域名称到一组特定的 RADIUS 服务器的 RADIUS 消息转发的路由规则。

## <a name="attribute-manipulation-rules"></a>属性操作规则

处理本地 （当 NPS 用作 RADIUS 服务器） 或转发到另一个 RADIUS 服务器 （当 NPS 用作 RADIUS 代理） 的 RADIUS 消息之前，可以通过属性操作规则修改消息中的用户名属性。 可以通过选择配置的用户名属性的属性操作规则**用户名**上**条件**的连接请求策略属性中的选项卡。 NPS 属性操作规则使用正则表达式语法。

可以配置为用户名属性用于更改下列属性操作规则：

- 从用户名中删除领域名称\(也称为领域去除\)。 例如，用户名user1@example.com更改为 user1。

- 更改领域名称但其语法不能更改。 例如，用户名user1@example.com更改为user1@wcoast.example.com。

- 更改领域名称的语法。 例如，用户名称 example\user1 更改为user1@example.com。

根据你配置的属性操作规则修改用户名属性后，其他设置的第一个匹配的连接请求策略用于确定是否：

- 本地 （当 NPS 用作 RADIUS 服务器），NPS 会处理访问请求消息。

- （当 NPS 用作 RADIUS 代理） 时，NPS 将转发到另一个 RADIUS 服务器的消息。

## <a name="configuring-the-nps-supplied-domain-name"></a>配置 NPS 提供的域名

当用户名称不包含域名称时，NPS 会提供一个。 默认情况下，NPS 提供的域名是 NPS 所属的域。 可以指定 NPS 提供的域名通过以下注册表设置：

    
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\RasMan\PPP\ControlProtocols\BuiltIn\DefaultDomain
    

>[!CAUTION]
>注册表编辑不当可能会严重损坏系统。 在更改注册表之前，应备份计算机上任何有价值的数据。

某些非 Microsoft 网络访问服务器删除或修改由用户指定的域名。 因此，网络访问请求针对默认域，这可能不是用户的帐户的域进行身份验证。 若要解决此问题，请配置您的 RADIUS 服务器以将用户名称更改为具有准确域名的正确格式。
