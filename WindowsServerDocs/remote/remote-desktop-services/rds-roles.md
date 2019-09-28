---
title: 远程桌面服务角色
description: 介绍桌面托管服务的组件。
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 07/06/2018
ms.topic: article
author: heidilohr
manager: dougkim
ms.openlocfilehash: f26f75b8ce3f5438362c15b84aeca9339b95ebbc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387201"
---
# <a name="remote-desktop-services-roles"></a>远程桌面服务角色

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

本指南介绍远程桌面服务环境中的角色。

## <a name="remote-desktop-session-host"></a>远程桌面会话主机

远程桌面会话主机（RD 会话主机）保存与用户共享的基于会话的应用和桌面。 用户通过在 Windows、MacOS、iOS 和 Android 上运行的远程桌面客户端之一访问这些桌面和应用。 用户还可以使用 Web 客户端通过支持的浏览器连接。

可以将桌面和应用组织到一个或多个 RD 会话主机服务器中（称为“集合”）。 可以为每个租户中的特定用户组自定义这些集合。 例如，可以创建一个集合，其中特定用户组可以访问特定应用，但是指定的组之外的任何人都无法访问这些应用。

对于小型部署，可以将应用程序安装直接到 RD 会话主机服务器上。 对于大型部署，建议生成基本映像并通过该映像预配虚拟机。

可以通过将 RD 会话主机服务器虚拟机添加到集合场来扩展集合，集合中的每个 RDSH 虚拟机都分配给相同可用性集。 这可提供更高的集合可用性，并增大规模以支持更多用户或资源密集型应用程序。

在大多数情况下，多个用户共享相同 RD 会话主机服务器，这样可最高效地将 Azure 资源用于桌面托管解决方案。 在此配置中，用户必须使用非管理帐户登录集合。 还可以通过创建个人会话桌面集合，向某些用户授予对其远程桌面的完全管理访问权限。

甚至可以创建和上传包含 Windows Server 操作系统、可以用作创建新 RD 会话主机虚拟机的模板的虚拟硬盘，从而自定义更多桌面。

有关详细信息，请参阅以下文章：

* [远程桌面服务 - 保护数据存储](rds-plan-secure-data-storage.md)
* [上传通用 VHD 并使用它在 Azure 中创建新 VM](https://docs.microsoft.com/azure/virtual-machines/windows/upload-generalized-managed?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json)
* [更新 RDSH 集合（ARM 模板）](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/)

## <a name="remote-desktop-connection-broker"></a>远程桌面连接代理

远程桌面连接代理（RD 连接代理）管理与 RD 会话主机服务器场之间的传入远程桌面连接。 RD 连接代理处理与完整桌面集合和远程应用集合之间的连接。 建立新连接时，RD 连接代理可以在集合的服务器间平衡负载。 如果某个会话断开连接，则 RD 连接代理会将用户重新连接到正确的 RD 会话主机服务器及其中断的会话，该会话仍存在于 RD 会话主机场中。

需要在 RD 连接代理服务器和客户端上安装匹配的数字证书以支持单一登录和应用程序发布。 开发或测试网络时，可以使用自行生成的自签名证书。 但是，已发布的服务需要来自受信任的证书颁发机构的数字证书。 为证书提供的名称必须与 RD 连接代理虚拟机内部完全限定的域名 (FQDN) 相同。

可以在与 AD DS 相同的虚拟机上安装 Windows Server 2016 RD 连接代理以降低成本。 如果需要扩大到更多用户，还可以在相同可用性集中添加其他 RD 连接代理虚拟机以创建 RD 连接代理群集。

必须先在租户的环境中部署 Azure SQL 数据库或创建 SQL Server AlwaysOn 可用性组，然后才能创建 RD 连接代理群集。

有关详细信息，请参阅以下文章：

* [向部署添加 RD 连接代理服务器和配置高可用性](rds-connection-broker-cluster.md)
* 桌面托管服务中的 [SQL 数据库](desktop-hosting-service.md#sql-database)。

## <a name="remote-desktop-gateway"></a>远程桌面网关

远程桌面网关（RD 网关）向用户授予对 Microsoft Azure 云服务中托管的 Windows 桌面和应用程序的公用网络访问权限。

RD 网关组件使用安全套接字层 (SSL) 对客户端与服务器之间的通信通道进行加密。 RD 网关虚拟机必须可通过允许与端口 443 建立入站 TCP 连接以及与端口 3391 建立入站 UDP 连接的公用 IP 地址进行访问。 这使用户可以分别使用 HTTPS 通信传输协议和 UDP 协议通过 Internet 连接。

在服务器和客户端上安装的数字证书必须匹配才能使此功能正常工作。 开发或测试网络时，可以使用自行生成的自签名证书。 但是，已发布的服务需要来自受信任的证书颁发机构的证书。 证书名称必须与用于访问 RD 网关的 FQDN 匹配，无论该 FQDN 是公用 IP 地址面向外部的 DNS 名称还是指向公用 IP 地址的 CNAME DNS 记录。

对于用户较少的租户，可以在单个虚拟机上合并 RD Web 访问和 RD 网关角色以降低成本。 还可以将更多 RD 网关虚拟机添加到 RD 网关场，以提高服务可用性和扩大到更多用户。 较大 RD 网关场中的虚拟机应在负载平衡集中进行配置。 在 Windows Server 2016 虚拟机上使用 RD 网关时不需要 IP 关联，但是在 Windows Server 2012 R2 虚拟机上运行该网关时需要。

有关详细信息，请参阅以下文章：

* [向 RD Web 和网关 Web 前端添加高可用性](rds-rdweb-gateway-ha.md)
* [远程桌面服务 - 从任意位置访问](rds-plan-access-from-anywhere.md)
* [远程桌面服务 - 多重身份验证](rds-plan-mfa.md)

## <a name="remote-desktop-web-access"></a>远程桌面 Web 访问

远程桌面 Web 访问（RD Web 访问）使用户可以通过 Web 门户访问桌面和应用程序，并通过设备的本机 Microsoft 远程桌面客户端应用程序启动它们。 可以使用 Web 门户将 Windows 桌面和应用程序发布到 Windows 和非 Windows 客户端设备，还可以有选择地将桌面或应用发布到特定用户或组。

RD Web 访问需要 Internet Information Services (IIS) 才能正常工作。 安全超文本传输协议 (HTTPS) 连接在客户端与 RD Web 服务器之间提供加密通信通道。 RD Web 访问虚拟机必须可通过允许与端口 443 建立入站 TCP 连接的公用 IP 地址进行访问，以允许租户的用户使用 HTTPS 通信传输协议从 Internet 连接。

必须在服务器和客户端上安装匹配的数字证书。 对于开发和测试用途，这可以是自行生成的自签名证书。 对于已发布的服务，必须从受信任的证书颁发机构获取数字证书。 证书的名称必须与用于访问 RD Web 访问的完全限定的域名 (FQDN) 匹配。 可能的 FQDN 包括公用 IP 地址面向外部的 DNS 名称和指向公用 IP 地址的 CNAME DNS 记录。

对于用户较少的租户，可以通过将 RD Web 访问和远程桌面网关工作负载合并到单个虚拟机中来降低成本。 还可以将其他 RD Web 虚拟机添加到 RD Web 访问场，以提高服务可用性和扩大到更多用户。 在具有多个虚拟机的 RD Web 访问场中，必须在负载平衡集中配置虚拟机。

有关如何配置 RD Web 访问的详细信息，请参阅以下文章：

* [为用户设置远程桌面 Web 客户端](clients/remote-desktop-web-client-admin.md)
* [创建和部署远程桌面服务集合](rds-create-collection.md)
* [创建远程桌面服务集合以便桌面和应用运行](rds-create-collection.md)

## <a name="remote-desktop-licensing"></a>远程桌面授权

已激活的远程桌面授权（RD 授权）服务器让用户可以连接到托管租户桌面和应用的 RD 会话主机服务器。 租户环境通常都已安装了 RD 授权服务器，但是对于托管环境，必须在每用户模式下配置服务器。

服务提供商需要足够的 RDS 订户访问许可证 (Sal) 来涵盖每个月登录服务的所有授权的唯一（非并发）用户。 服务提供商可以直接购买 Microsoft Azure 基础结构服务，并且可以通过 Microsoft 服务提供商许可协议 (SPLA) 计划购买 SAL。 寻找托管桌面解决方案的客户必须从服务提供商购买完整的托管解决方案（Azure 和 RDS）。

小型租户可以通过将文件服务器和 RD 授权组件合并到单个虚拟机来降低成本。 若要提供更高的服务可用性，租户可以在同一个可用性集中部署两个 RD 许可证服务器虚拟机。 租户环境中的所有 RD 服务器都与这两台 RD 许可证服务器关联，以便即使其中一台服务器出现故障，用户也能够连接到新会话。

有关详细信息，请参阅以下文章：

* [使用客户端访问许可证 (CAL) 许可 RDS 部署](rds-client-access-license.md)
* [激活远程桌面服务许可证服务器](rds-activate-license-server.md)
* [跟踪远程桌面服务客户端访问许可证 (RDS CAL)](rds-track-cals.md)
* [Microsoft 批量许可：适用于服务提供商的许可选项](https://www.microsoft.com/en-us/Licensing/licensing-programs/spla-program.aspx)