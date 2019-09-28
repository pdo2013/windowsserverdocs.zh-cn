---
title: 网络策略服务器最佳做法
description: 本主题提供有关在 Windows Server 2016 中部署和管理网络策略服务器的最佳实践。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 364783e2188152fc5c57bba04991ae124b0bb8d2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405500"
---
# <a name="network-policy-server-best-practices"></a>网络策略服务器最佳做法

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用本主题来了解有关部署和管理网络策略服务器 \(NPS @ no__t-1 的最佳实践。

以下各节提供了有关 NPS 部署的不同方面的最佳实践。

## <a name="accounting"></a>记帐

下面是有关 NPS 日志记录的最佳实践。

NPS 中有两种类型的记帐或日志记录：

- NPS 的事件日志记录。 您可以使用事件日志记录来记录系统和安全事件日志中的 NPS 事件。 这主要用于对连接尝试进行审核和故障排除。

- 记录用户身份验证和记帐请求。 您可以将用户身份验证和记帐请求记录到以文本格式或数据库格式记录的文件中，也可以记录到 SQL Server 2000 数据库中的存储过程。 请求日志记录主要用于连接分析和计费目的，也可以用作一种安全调查工具，提供一种方法来跟踪攻击者的活动。

最有效地使用 NPS 日志记录：

- 对于身份验证和记帐记录，启用日志记录 \(initially @ no__t-1。 确定适用于你的环境的内容之后，请修改这些选择。

- 确保为事件日志配置的容量足以维持日志。

- 定期备份所有日志文件，因为这些日志文件在被损坏或删除时无法重新创建。

- 使用 RADIUS 类特性跟踪使用情况，并简化标识哪个部门或用户对使用情况收费。 尽管自动生成的类特性对于每个请求都是唯一的，但在对访问服务器的回复丢失且重新发送请求时，可能存在重复记录。 你可能需要从日志中删除重复的请求以准确跟踪使用情况。

- 如果网络访问服务器和 RADIUS 代理服务器定期将虚构的连接请求消息发送给 NPS，以验证 NPS 是否处于联机状态，请使用**ping 用户名**注册表设置。 此设置将 NPS 配置为自动拒绝这些错误的连接请求而不进行处理。 此外，NPS 不会将涉及虚拟用户名的事务记录到任何日志文件中，这使得事件日志更易于解释。

- 禁用 NAS 通知转发。 你可以禁止将启动和停止消息从网络访问服务器（Nas）转发到在 NPS 中配置的远程 RADIUS 服务器组的成员。 有关详细信息，请参阅[禁用 NAS 通知转发](nps-disable-nas-notifications.md)。

有关详细信息，请参阅[配置网络策略服务器记帐](nps-accounting-configure.md)。

- 若要通过 SQL Server 日志记录提供故障转移和冗余，请将两台运行 SQL Server 的计算机置于不同的子网中。 使用 SQL Server**创建发布向导**来设置两个服务器之间的数据库复制。 有关详细信息，请参阅[SQL Server 技术文档](https://msdn.microsoft.com/library/ms130214.aspx)和[SQL Server 复制](https://msdn.microsoft.com/library/ms151198.aspx)。

## <a name="authentication"></a>身份验证

下面是身份验证的最佳实践。

- 使用基于证书的身份验证方法（例如受保护的可扩展身份验证协议 \(PEAP @ no__t）和可扩展的身份验证协议 \(EAP @ no__t-3 以进行强身份验证。 不要使用只使用密码的身份验证方法，因为它们容易受到各种攻击，而且不安全。 对于安全无线身份验证，建议使用 PEAP @ no__t-0MS @ no__t-1CHAP v2，因为 NPS 使用服务器证书向无线客户端证明其身份，而用户使用其用户名和密码证明其身份。  有关在无线部署中使用 NPS 的详细信息，请参阅[部署基于密码 802.1 x 身份验证的无线访问](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。
- 当你使用需要使用服务器的基于证书的强身份验证方法（如 PEAP 和 EAP）时，使用 Active Directory @ no__t-2 证书服务部署你自己的证书颁发机构 \(CA @ no__t-1 \(AD CS @ no__t-4NPSs 上的证书。 你还可以使用你的 CA 来注册计算机证书和用户证书。 有关将服务器证书部署到 NPS 和远程访问服务器的详细信息，请参阅为[802.1 x 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

> [!IMPORTANT]
> 网络策略服务器（NPS）不支持在密码中使用扩展 ASCII 字符。

## <a name="client-computer-configuration"></a>客户端计算机配置

下面是客户端计算机配置的最佳实践。

- 使用组策略自动配置所有域成员 802.1 X 客户端计算机。 有关详细信息，请参阅[无线访问部署](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)主题中的 "配置无线网络（IEEE 802.11）策略" 一节。

## <a name="installation-suggestions"></a>安装建议

下面是安装 NPS 的最佳实践。

- 在安装 NPS 之前，请先使用本地身份验证方法安装和测试每个网络访问服务器，然后再将其配置为 NPS 中的 RADIUS 客户端。

- 安装并配置 NPS 后，请使用 Windows PowerShell 命令[export-npsconfiguration](https://technet.microsoft.com/library/jj872749.aspx)保存配置。 每次重新配置 NPS 时，请将 NPS 配置与此命令一起保存。

>[!CAUTION]
>- 导出的 NPS 配置文件包含 RADIUS 客户端和远程 RADIUS 服务器组成员的未加密共享机密。 因此，请确保将该文件保存到安全位置。
>- 导出过程不包括导出文件中 Microsoft SQL Server 的日志记录设置。 如果将导出的文件导入到另一个 NPS，则必须在新服务器上手动配置 SQL Server 日志记录。

## <a name="performance-tuning-nps"></a>性能优化 NPS

下面是性能优化 NPS 的最佳实践。

- 若要优化 NPS 身份验证和授权响应时间并将网络流量降到最低，请在域控制器上安装 NPS。

- 当使用通用主体名称 \(UPNs @ no__t 或 Windows Server 2008 和 Windows server 2003 域时，NPS 将使用全局编录对用户进行身份验证。 若要最大程度地减少执行此操作所用的时间，请在全局编录服务器或与全局编录服务器位于同一子网中的服务器上安装 NPS。

- 配置远程 RADIUS 服务器组后，在 NPS 连接请求策略中，清除 "在**以下远程 radius 服务器组中的服务器上记录记帐信息**" 复选框，这些组仍将发送网络访问权限服务器 \(NAS @ no__t-2 启动和停止通知消息。 这会产生不必要的网络流量。 若要消除此流量，请清除 "将**网络启动和停止通知转发到此服务器**" 复选框，禁用每个远程 RADIUS 服务器组中各个服务器的 NAS 通知转发。

## <a name="using-nps-in-large-organizations"></a>在大型组织中使用 NPS

下面是在大型组织中使用 NPS 的最佳实践。

- 如果你使用网络策略来限制除某些组以外的所有组的访问权限，请为你要允许其访问的所有用户创建一个通用组，然后创建一个为此通用组授予访问权限的网络策略。 不要将所有用户直接放入通用组中，特别是在网络中有大量用户时。 请改为创建属于通用组成员的单独组，并将用户添加到这些组。

- 尽可能使用用户主体名称引用用户。 无论域成员身份如何，用户都可以具有相同的用户主体名称。 这种做法提供了在拥有大量域的组织中可能需要的可伸缩性。

- 如果在域控制器之外的计算机上安装了网络策略服务器 \(NPS @ no__t，且 NPS 每秒接收了大量的身份验证请求，则可以通过增加并发NPS 和域控制器之间允许进行身份验证。 有关详细信息，请参阅 

## <a name="security-issues"></a>安全问题

下面是减少安全问题的最佳做法。

远程管理 NPS 时，不要以纯文本形式通过网络发送敏感数据或机密数据（例如，共享机密或密码）。 远程管理 NPSs 有两种建议的方法：

- 使用远程桌面服务访问 NPS。 如果使用远程桌面服务，则不会在客户端和服务器之间发送数据。 只有服务器的用户界面（例如，操作系统桌面和 NPS 控制台映像）才会发送到 Windows @ no__t 中名为远程桌面连接的远程桌面服务客户端。 客户端发送键盘和鼠标输入，该输入由启用了远程桌面服务的服务器本地处理。 当远程桌面服务用户登录时，他们只能查看各自的客户端会话，这些会话由服务器管理并且彼此独立。 此外，远程桌面连接在客户端和服务器之间提供128位加密。

- 使用 Internet 协议安全（IPsec）加密机密数据。 您可以使用 IPsec 对 NPS 和用于管理 NPS 的远程客户端计算机之间的通信进行加密。 若要远程管理服务器，你可以在客户端计算机上安装[适用于 Windows 10 的远程服务器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)。 安装完成后，使用 Microsoft 管理控制台（MMC）将 NPS 管理单元添加到控制台。

>[!IMPORTANT]
>你只能在 Windows 10 专业版或 Windows 10 企业版的完整版本上安装适用于 Windows 10 的远程服务器管理工具。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。

