---
title: 远程桌面服务角色
description: 介绍的桌面托管服务的组件。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: 48efbffa4f82b707b63e33e6416da43eb105221f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844718"
---
# <a name="remote-desktop-services-roles"></a>远程桌面服务角色

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本指南介绍了远程桌面服务环境中的角色。

## <a name="remote-desktop-session-host"></a>远程桌面会话主机

远程桌面会话主机 （RD 会话主机） 保存的基于会话的应用和桌面与用户共享。 用户获得这些桌面和应用程序通过 Windows、 MacOS、 iOS 和 Android 运行的远程桌面客户端之一。 用户还可以使用 web 客户端通过支持的浏览器连接。

可以将桌面和应用程序组织到一个或多个 RD 会话主机服务器，名为"集合"。 您可以自定义特定组中每个租户的用户的这些集合。 例如，可以创建一个集合，其中某个特定用户组可以访问特定的应用，但您指定的组之外的任何人都无法再访问这些应用程序。

对于小型部署，可以安装直接到 RD 会话主机服务器上的应用程序。 对于大型部署，我们建议生成基本映像和预配虚拟机从该映像。

可以通过将 RD 会话主机服务器虚拟机添加到集合场与分配给同一个可用性集集合中每个 RDSH 虚拟机扩展集合。 这提供了更高版本收集可用性并提高了规模以支持多个用户或资源密集型应用程序。

在大多数情况下，多个用户共享同一个 RD 会话主机服务器，最有效地利用 Azure 桌面托管解决方案的资源。 在此配置中，用户必须登录到使用非管理帐户的集合。 你还可以允许某些用户完全管理访问权限到自己的远程桌面通过创建个人会话桌面集合。

您可以自定义桌面更多通过创建和上载包含 Windows Server 操作系统中可用于为模板创建新的 RD 会话主机虚拟机的虚拟硬盘。

有关详细信息，请参阅以下文章：

* [远程桌面服务-安全的数据存储](rds-plan-secure-data-storage.md)
* [上传通用化的 VHD 并使用它来在 Azure 中创建新的 Vm](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [更新 RDSH 集合 （ARM 模板）](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>远程桌面连接代理

远程桌面连接代理 （RD 连接代理） 管理传入的远程桌面连接到 RD 会话主机服务器场。 RD 连接代理处理连接这两个集合的完整桌面和远程应用程序的集合。 建立新连接时，RD 连接代理可以在集合的服务器之间平衡负载。 如果会话将断开连接，RD 连接代理将用户重新连接到正确的 RD 会话主机服务器和其中断的会话，在 RD 会话主机服务器场中仍存在。

你将需要安装匹配的 RD 连接代理服务器和客户端支持单一登录和应用程序发布的数字证书。 当开发或测试网络时，可以使用自行生成和自签名证书。 但是，已发布的服务需要从受信任的证书颁发机构的数字证书。 为证书提供的名称必须是作为内部完全限定域 (FQDN) 的 RD 连接代理虚拟机的相同。

您可以与 AD DS 以降低成本相同的虚拟机上安装 Windows Server 2016 RD 连接代理。 如果你需要向外扩展到更多的用户，还可以在同一可用性集中创建的 RD 连接代理群集中添加其他 RD 连接代理虚拟机。

创建一个 RD 连接代理群集之前，必须部署中租户的环境的 Azure SQL 数据库或创建 SQL Server AlwaysOn 可用性组。

有关详细信息，请参阅以下文章：

* [将 RD 连接代理服务器添加到部署和配置高可用性](rds-connection-broker-cluster.md)
* [SQL 数据库](desktop-hosting-service.md#sql-database)桌面托管服务中。

## <a name="remote-desktop-gateway"></a>远程桌面网关

远程桌面网关 （RD 网关） 授予用户对 Windows 桌面和应用程序托管在 Microsoft Azure 云服务的公共网络访问权限。

RD 网关组件使用安全套接字层 (SSL) 加密客户端和服务器之间的通信通道。 RD 网关虚拟机必须可通过允许到端口 443 入站的 TCP 连接和入站的 UDP 连接到端口 3391 的公共 IP 地址。 这允许用户通过分别使用 HTTPS 通信传输协议和 UDP 协议，internet 连接。

数字证书安装在服务器和客户端必须匹配为实现此目的。 当你开发或测试网络时，可以使用自行生成和自签名证书。 但是，发布的服务需要来自受信任的证书颁发机构的证书。 证书名称必须与用来访问 RD 网关 FQDN 是公共 IP 地址的面向外部的 DNS 名称还是指向公共 IP 地址的 CNAME DNS 记录的 FQDN 匹配。

对于使用较少的用户的租户，可以将 RD Web 访问和 RD 网关角色组合在单个虚拟机以降低成本。 到 RD 网关场以提高服务的可用性和横向扩展到更多的用户，还可以添加更多的 RD 网关虚拟机。 更大的 RD 网关场中的虚拟机应配置负载平衡集中。 针对 Windows Server 2016 虚拟机，都使用 RD 网关，但它是 Windows Server 2012 R2 虚拟机上运行它时，不需要使用 IP 关联。

有关详细信息，请参阅以下文章：

* [将可用性组添加到 RD Web 和网关的 web 前端](rds-rdweb-gateway-ha.md)
* [远程桌面服务-从任意位置访问](rds-plan-access-from-anywhere.md)
* [远程桌面服务-多重身份验证](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>远程桌面 Web 访问

远程桌面 Web 访问 （RD Web 访问） 允许用户访问桌面和应用程序通过 web 门户，并通过设备的本机 Microsoft 远程桌面客户端应用程序启动它们。 您可以使用 web 门户发布 Windows 桌面和应用程序到 Windows 和非 Windows 客户端设备，并有选择地将桌面或应用程序发布到特定用户或组。

RD Web 访问需要 Internet 信息服务 (IIS) 才能正常工作。 安全超文本传输协议 (HTTPS) 连接提供了客户端和 RD Web 服务器之间的加密的通信通道。 RD Web 访问虚拟机必须可通过允许入站的 TCP 连接到端口 443 以允许租户的用户从 internet 连接使用 HTTPS 通信的传输协议的公共 IP 地址。

必须在服务器和客户端上安装匹配的数字证书。 对于开发和测试目的，这可能是自行生成和自签名证书。 对于已发布的服务，必须从受信任的证书颁发机构获取数字证书。 证书名称必须与匹配的完全限定域名 (FQDN) 用来访问 RD Web 访问。 可能的 Fqdn 包括面向外部 DNS 名称的公共 IP 地址和指向公共 IP 地址的 CNAME DNS 记录。

对于使用较少的用户的租户，可以通过合并到单个虚拟机的 RD Web 访问和远程桌面网关工作负荷降低成本。 到 RD Web 访问的场以提高服务的可用性和横向扩展到更多的用户，还可以添加其他 RD Web 虚拟机。 在多个虚拟机使用的 RD Web 访问场，你必须配置负载均衡集中虚拟机。

有关如何配置 RD Web 访问的详细信息，请参阅以下文章：

* [设置你的用户的远程桌面 web 客户端](clients/remote-desktop-web-client-admin.md)
* [创建和部署远程桌面服务集合](rds-create-collection.md)
* [创建适用于桌面和应用程序以运行远程桌面服务集合](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>远程桌面授权

已激活的远程桌面授权 （RD 授权） 服务器让用户可以连接到托管租户的桌面和应用的 RD 会话主机服务器。 租户环境通常都附带了 RD 授权服务器已安装，但托管环境的需要在每个用户模式下配置服务器。

服务提供程序需要足够 RDS 订户访问许可证 (Sal) 以涵盖所有唯一 （非并发） 的授权的用户登录到该服务每个月。 服务提供商可以直接购买 Microsoft Azure 基础结构服务，并可以通过 Microsoft 服务提供商许可协议 (SPLA) 计划购买 Sal。 想要托管的桌面解决方案的客户必须从服务提供商购买完整的托管的解决方案 （Azure 和 RDS）。

小型租户可以通过合并的文件服务器和 RD 授权组件放在单个虚拟机降低成本。 若要提供更高的服务可用性，租户可以将同一个可用性集中的两个远程桌面许可证服务器虚拟机部署。 使用这两种远程桌面许可证服务器，以确保用户能够连接到新的会话，即使其中一个服务器出现故障，租户的环境中的所有 RD 服务器都相关联。

有关详细信息，请参阅以下文章：

* [使用客户端访问许可证 (Cal) 许可 RDS 部署](rds-client-access-license.md)
* [激活远程桌面服务许可证服务器](rds-activate-license-server.md)
* [跟踪远程桌面服务客户端访问许可证 (RDS Cal)](rds-track-cals.md)
* [Microsoft 批量许可： 许可服务提供商选项](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)