---
title: 网络策略服务器最佳做法
description: 本主题提供用于部署和管理 Windows Server 2016 中的网络策略服务器的最佳实践。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 90e544bd-e826-4093-8c3b-6a6fc2dfd1d6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6cbd606edec06a80767ee997ef6a1397b66b7843
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834228"
---
# <a name="network-policy-server-best-practices"></a>网络策略服务器最佳做法

>适用于：Windows 服务器 （半年频道），Windows Server 2016

你可以使用本主题以了解有关用于部署和管理网络策略服务器的最佳做法\(NPS\)。

以下部分提供将 NPS 部署的不同方面的最佳做法。

## <a name="accounting"></a>记帐

以下是 NPS 日志记录的最佳实践。

有两种类型的记帐或日志记录，在 NPS 中：

- Nps 事件日志记录。 在系统和安全事件日志中，可以使用事件日志记录记录 NPS 事件。 这是主要用于审核和故障排除的连接尝试。

- 用户身份验证和记帐请求记录。 你可以记录用户身份验证和记帐请求以文本格式或数据库格式日志文件，或可以记录到 SQL Server 2000 数据库中的存储过程。 请求日志记录主要用于连接分析和计费目的，并且仍可用作安全调查工具，使您拥有的攻击者跟踪活动的方法。

若要使 NPS 日志记录最有效地使用：

- 启用日志记录\(最初\)进行身份验证和记帐记录。 你确定哪种适合于您的环境后，请修改这些选择。

- 确保事件日志记录配置足以维护日志的容量。

- 备份定期的所有日志文件，因为它们已损坏或删除时不能重新创建。

- 使用 RADIUS Class 属性以跟踪使用情况并简化要负责管理使用情况的部门或用户的标识。 虽然自动生成的类属性是唯一的每个请求，但可能会丢失访问服务器的回复和重新发送请求中存在重复的记录。 您可能需要删除重复的请求从您的日志，准确地跟踪使用情况。

- 如果你的网络访问服务器和 RADIUS 代理服务器定期将虚构的连接请求消息发送到 NPS 验证 NPS 处于联机状态，请使用**用户名称进行 ping 操作**注册表设置。 此设置配置 NPS 自动拒绝这些虚假的连接请求而不处理它们。 此外，NPS 不会记录涉及虚构用户名中的任何日志文件，因此可以轻松地解释事件日志的事务。

- 禁用 NAS 通知转发。 您可以禁用转发的开始和停止消息从网络访问服务器 (Nas) 到远程 RADIUS 服务器的成员组在 NPS 中配置该 IS。 有关详细信息，请参阅[禁用 NAS 通知转发](nps-disable-nas-notifications.md)。

有关详细信息，请参阅[配置网络策略服务器记帐](nps-accounting-configure.md)。

- 若要使用 SQL Server 日志记录提供故障转移和冗余，将放置在不同子网上运行 SQL Server 的两台计算机。 使用 SQL Server**创建发布向导**设置两个服务器之间的数据库复制。 有关详细信息，请参阅[SQL Server 技术文档](https://msdn.microsoft.com/library/ms130214.aspx)并[SQL Server 复制](https://msdn.microsoft.com/library/ms151198.aspx)。

## <a name="authentication"></a>身份验证

以下是用于身份验证的最佳实践。

- 使用基于证书的身份验证方法，如受保护的可扩展身份验证协议\(PEAP\)和可扩展身份验证协议\(EAP\)进行强身份验证。 不要使用的仅密码身份验证方法，因为它们容易受到各种攻击，是不安全的。 对于安全的无线身份验证，使用 PEAP\-MS\-建议 CHAP v2，因为 NPS 证明其身份为无线客户端使用服务器证书，而用户证明自己的身份使用其用户名和密码。  有关在无线部署中使用 NPS 的详细信息，请参阅[部署基于密码的 802.1 X 身份验证无线访问](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/a-deploy-8021x-wireless-access)。
- 部署您自己的证书颁发机构\(CA\)与 Active Directory&reg;证书服务\(AD CS\)时可使用强大的基于证书的身份验证方法，如 PEAP 和 EAP 的需要使用 NPSs 上的服务器证书。 此外可以使用 CA 来注册计算机证书和用户证书。 将服务器证书部署到 NPS 和远程访问服务器的详细信息，请参阅[802.1x 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments)。

## <a name="client-computer-configuration"></a>客户端计算机配置

以下是客户端计算机配置的最佳实践。

- 自动配置所有域成员 802.1x 客户端计算机通过使用组策略。 有关详细信息，请参阅主题中的"配置的无线网络 (IEEE 802.11) 策略"一节[无线访问部署](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/wireless/e-wireless-access-deployment#bkmk_policies)。

## <a name="installation-suggestions"></a>安装建议

以下是安装 NPS 的最佳实践。

- 安装 NPS 之前, 安装和测试每个网络访问服务器之前将它们配置为 NPS 中的 RADIUS 客户端使用本地身份验证方法。

- 安装和配置 NPS 后，保存配置使用 Windows PowerShell 命令[导出 NpsConfiguration](https://technet.microsoft.com/library/jj872749.aspx)。 使用以下命令，每次你重新配置 NPS 保存 NPS 配置。

>[!CAUTION]
>- 导出的 NPS 配置文件包含 RADIUS 客户端和远程 RADIUS 服务器组成员的未加密的共享的机密。 由于此操作，请确保将该文件保存到安全位置。
>- 导出过程中不包括导出的文件中的 Microsoft SQL Server 日志记录设置。 如果导出的文件导入到另一个 NPS 中，则必须手动配置 SQL Server 日志记录在新服务器上。

## <a name="performance-tuning-nps"></a>性能优化 NPS

以下是性能优化 NPS 最佳实践。

- 若要优化 NPS 身份验证和授权响应时间和网络流量降到最低，请在域控制器上安装 NPS。

- 当通用主体名称\(Upn\)或 Windows Server 2008 和 Windows Server 2003 域，则 NPS 使用全局编录对用户进行身份验证。 为了尽量减少所要执行此操作的时间，请在全局编录服务器或全局编录服务器位于同一子网中的服务器上安装 NPS。

- 如果配置的远程 RADIUS 服务器组，并且，在 NPS 连接请求策略，则清除**以下远程 RADIUS 服务器组中记录记帐信息的服务器上**复选框，这些组是仍发送网络访问服务器\(NAS\)启动和停止通知消息。 这将创建不必要的网络流量。 若要消除此流量，禁用每个远程 RADIUS 服务器组中的各个服务器 NAS 通知转发，通过清除**网络启动和停止通知发送到此服务器转发**复选框。

## <a name="using-nps-in-large-organizations"></a>在大型组织中使用 NPS

以下是在大型组织中使用 NPS 的最佳实践。

- 如果使用网络策略来限制对所有而不是特定组创建想要允许访问，其用户的所有通用组，然后创建一个网络策略，授予访问权限为此通用组。 不要将你的所有用户直接置于通用组中，尤其是有大量这些网络上。 相反，创建单独的组的成员的通用组，并将用户添加到这些组。

- 使用用户主体名称来引用用户只要有可能。 用户可以不考虑域成员身份的相同用户主体名称。 这种做法提供了可能需要在具有大量域的组织中的可伸缩性。

- 如果已安装网络策略服务器\(NPS\)域之外的计算机上控制器和 NPS 正在接收大量的每秒的身份验证请求，您可以通过增加的数量提高 NPS 性能NPS 和域控制器之间允许的并发身份验证。 有关详细信息，请参阅 

## <a name="security-issues"></a>安全问题

以下是减少安全问题的最佳实践。

在远程管理 NPS 时，不要以纯文本形式在网络上发送敏感或机密数据 （例如，共享的机密或密码）。 有两个推荐的方法 NPSs 进行远程管理：

- 使用远程桌面服务来访问 NPS。 当你使用远程桌面服务时，数据不会发送客户端和服务器之间。 服务器 （例如，操作系统桌面和 NPS 控制台映像） 的用户界面发送到远程桌面服务客户端，其名称为在 Windows 中的远程桌面连接&reg;10。 客户端发送键盘和鼠标输入，这由已启用远程桌面服务服务器在本地处理。 当远程桌面服务用户在登录时，他们可以查看仅其单个客户端会话，这些会话由服务器管理，并且是相互独立。 此外，远程桌面连接提供了客户端和服务器之间的 128 位加密。

- 使用 Internet 协议安全 (IPsec) 来加密机密数据。 可以使用 IPsec 对 NPS 和用来管理 NPS 的远程客户端计算机之间的通信进行加密。 若要远程管理服务器，可以安装[远程服务器管理工具适用于 Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)客户端计算机上。 安装完成后，使用 Microsoft 管理控制台 (MMC) 将 NPS 管理单元中添加到控制台。

>[!IMPORTANT]
>您可以仅在完整版本的 Windows 10 专业版或 Windows 10 企业版上安装远程服务器管理工具适用于 Windows 10。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。

