---
title: 网络策略 Server 最佳做法
description: 本主题提供部署和管理网络策略服务器，在 Windows Server 2016 的最佳的实践。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a9a68965d0d19504352ad67f7ab268b3d344fb2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="network-policy-server-best-practices"></a>网络策略 Server 最佳做法

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解有关部署和管理网络策略服务器 \(NPS\) 的最佳实践。

以下部分提供最佳实践 NPS 部署的各个方面。

## <a name="accounting"></a>记帐

以下是 NPS 日志记录的最佳实践。

有两种类型的记帐，或登录 NPS:

- NPS 的事件日志记录。 在系统和安全事件日志中，你可以使用 NPS 事件记录的事件日志记录。 这是主要用于审核和疑难解答尝试连接。

- 日志记录用户身份验证和记帐请求。 你可以用户身份验证和帐户请求登录格式或数据库格式的文件或登录您可以到存储在数据库中 SQL Server 2000 过程。 请求记录主要用于连接分析和帐单目的，而且还有用安全调查工具，从而为您提供的攻击跟踪的活动的方法。

若要使 NPS 日志记录的最有效使用：

- 启用日志记录 \(initially\) 身份验证和会计记录。 之后你已确定什么是适合您的环境进行修改这些选项。

- 确保事件日志记录配置的容量不足，无法维护你的日志。

- 备份定期所有日志文件，因为它们损坏或已删除它们时无法重新创建。

- 使用 RADIUS 类特性同时跟踪的使用量，并简化来收取使用哪些商业部或用户的身份。 虽然自动生成的类特性唯一的每个请求时，重复记录可能存在情况下访问服务器回复失去并且重新发送该请求。 你可能需要从你准确跟踪的使用情况的日志中删除重复请求。

- 如果你的网络访问权限服务器和 RADIUS 代理服务器定期虚构连接请求消息发送给 NPS 验证 NPS 服务器处于联机状态，请使用**ping 用户名**注册表设置。 此设置可配置 NPS 自动拒绝这些 false 连接请求，而不会处理它们。 此外，NPS 不交易涉及虚构用户名在任何日志文件中，从而使更轻松地解释事件日志记录。

- 禁用 NAS 通知转移。 您可以禁用开始转发并远程 RADIUS 服务器中的成员停止从网络的访问权限服务器 (Nas) 的消息进行分组配置 NPS 在该是。 有关详细信息，请参阅[禁用 NAS 通知转发](nps-disable-nas-notifications.md)。

有关详细信息，请参阅[配置网络策略服务器记帐](nps-accounting-configure.md)。

- SQL Server 日志记录与提供故障转移和冗余，放置运行不同的子网 SQL Server 两台计算机。 使用 SQL Server**创建发布向导**数据库复制两个服务器设置。 有关详细信息，请参阅[SQL Server Technical 文档](https://msdn.microsoft.com/library/ms130214.aspx)和[SQL Server 复制](https://msdn.microsoft.com/library/ms151198.aspx)。

## <a name="authentication"></a>身份验证

以下是身份验证的最佳实践。

- 用于强大的身份验证证书基于身份验证方法保护可扩展身份验证协议 \(PEAP\) 和可扩展身份验证协议 \(EAP\) 等。 不要使用仅密码的身份验证方法，因为它们易受攻击各种并且不安全。 对于安全无线身份验证，使用 PEAP\ MS\ CHAP v2 建议，因为 NPS 服务器通过服务器证书，用户证明自己的身份的用户名和密码与无线客户端向证明自己的身份。  有关在无线部署 NPS 时使用的详细信息，请参阅[基于部署密码 802.1 X 身份验证无线访问](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。
- 部署证书颁发机构 \(CA\) 使用 Active Directory&reg;证书服务 \(AD CS\) 当您使用强证书基于身份验证方法，如 PEAP 和 EAP，需要 NPS 服务器上的服务器证书的使用。 你还可以使用你 CA 计算机证书和用户证书的注册。 有关部署到 NPS 和远程访问服务器服务器证书的详细信息，请参阅[802.1 X 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="client-computer-configuration"></a>客户端计算机配置

以下是客户端计算机配置的最佳实践。

- 自动将所有你域成员 802.1 X 的客户端计算机配置使用组策略中。 有关详细信息，请参阅部分主题中的"配置的无线网络 (IEEE 802.11) 策略"[无线访问部署](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## <a name="installation-suggestions"></a>安装建议

以下是安装 NPS 的最佳实践。

- 安装 NPS 之前, 安装，并且每次都之前将其配置为在 NPS RADIUS 客户端使用本地身份验证方法网络的访问权限服务器的测试。

- 安装和配置 NPS 后，通过使用 Windows PowerShell 命令保存配置[导出 NpsConfiguration](https://technet.microsoft.com/en-us/library/jj872749.aspx)。 与每次你重新启动 NPS 此命令保存 NPS 配置。

>[!CAUTION]
>- 导出的 NPS 配置文件包含 RADIUS 客户端和远程 RADIUS 服务器组成员加密共享机密信息。 出于此原因，确保将文件保存到某个安全的位置。
>- 导出过程不包括导出文件中的 Microsoft SQL Server 记录设置。 如果将导出的文件导入到另一台 NPS 服务器，你必须手动配置日志记录 SQL Server 新的服务器上。

## <a name="performance-tuning-nps"></a>调优 NPS 性能

以下是调优 NPS 性能的最佳实践。

- 优化 NPS 身份验证和授权响应时间和最小化网络通信，域控制器上安装 NPS。

- 使用通用主要名称 \(UPNs\) 或 Windows Server 2008 和 Windows Server 2003 域，NPS 使用全球目录用户进行身份验证。 最小的时间来执行此操作，可在全球目录服务器或与全球目录服务器相同子网服务器上安装 NPS。

- 当你有配置远程 RADIUS 服务器组并且，NPS 连接请求策略，在你清除**录制以下远程 RADIUS 服务器组中记帐的服务器上的信息**复选框，这些组仍发送网络的访问权限服务器 \(NAS\) 启动和停止通知消息。 这将创建不必要的网络流量。 若要消除该通信，通过清除禁用每个远程 RADIUS 服务器组中的个别服务器的 NAS 通知转移**网络启动和停止对此服务器通知转发**复选框。

## <a name="using-nps-in-large-organizations"></a>大型公司中使用 NPS

以下是使用 NPS 大型公司中的最佳实践。

- 如果你使用的网络策略来限制对所有，但某些组创建一个通用组为所有用户为其你想要允许访问权限，，然后创建一个网络策略授予访问此通用组。 不要将您的所有用户直接插入通用组中，尤其是，如果你有大量的这些网络上。 不过，创建单独的组成员通用组中，并添加到这些组的用户。

- 使用用户名主体尽可能的用户，请参考。 用户可具有相同无论域会员用户主要的名称。 这种做法提供可扩展性，可能需要在组织中极大的域。

- 如果域控制器之外的计算机上安装网络策略服务器 \(NPS\) NPS 服务器接收大量每秒进行身份验证请求，你可以通过增加并行身份验证之间 NPS 服务器和域控制器允许的数量来提高 NPS 性能。 有关详细信息，请参阅 

## <a name="security-issues"></a>安全问题

下面是减少安全问题的最佳实践。

在远程管理 NPS 服务器时，不要通过纯文本该网络发送敏感或机密的数据（例如，共享的机密或密码）。 有两个远程服务器的管理 NPS 推荐的方法：

- 使用远程桌面服务访问 NPS 服务器。 当你使用远程桌面服务时，数据不会发送之间客户端和服务器。 （例如，桌面操作系统和 NPS 控制台图像）服务器的用户界面发送远程桌面服务客户端，名为 Windows 中的远程桌面连接到&reg;10。 客户端发送键盘和鼠标输入，由已启用的远程桌面服务服务器本地处理。 当远程桌面服务用户在登录时，他们可以查看仅其单独的客户端会话，其中由服务器管理和独立于彼此。 此外，远程桌面连接提供之间客户端和服务器 128 位加密。

- 使用 Internet 协议安全加密机密的数据。 你可以使用 IPsec 加密 NPS 服务器和您使用管理 NPS 远程客户端计算机之间进行通信。 若要远程管理服务器，你可以安装[远程服务器管理工具适用于 Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)客户端计算机上。 安装完成后，使用 Microsoft 管理控制台 (MMC) 添加 NPS 服务器管理单元到主机。

>[!IMPORTANT]
>仅在 Windows 10 专业版或 Windows 10 企业版的完整版本上，你可以安装远程服务器管理工具适用于 Windows 10。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。

