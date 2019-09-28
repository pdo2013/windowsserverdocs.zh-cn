---
title: 步骤2配置 DirectAccess-VPN 服务器
description: 本主题是将 DirectAccess 添加到 Windows Server 2016 现有远程访问（VPN）部署的指南的一部分
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fe221fc9-c7d9-4508-b8a1-000d2515283c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ee691a02df385e29bdac9656d50bc2c6d3af087
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388739"
---
#  <a name="step-2-configure-the-directaccess-vpn-server"></a>步骤2配置 DirectAccess-VPN 服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题介绍如何使用启用 DirectAccess 向导配置基本远程访问部署所需的客户端和服务器设置。

下表概述了可通过使用本主题完成的步骤。

|任务       |描述|
|-----------|-----------|
|配置 DirectAccess 客户端|使用包含 DirectAccess 客户端的安全组配置远程访问服务器。|
|配置网络拓扑|配置远程访问服务器设置。|
|配置 DNS 后缀搜索列表|如果需要，可修改后缀搜索列表。|
|GPO 配置|如果需要，可修改 GPO。|

## <a name="to-start-the-enable-directacces-wizard"></a>启动启用 DirectAcces 向导

1. 在服务器管理器中，单击 "**工具**"，然后单击 "**远程访问**"。启用 DirectAccess 向导将自动启动，除非你选择了 "不再**显示此屏幕**"。 

2. 如果该向导没有自动启动，请右键单击 "路由和远程访问" 树中的服务器节点，然后单击 "**启用 DirectAccess**"。

3. 单击“下一步”。

## <a name="configure-directaccess-clients"></a>配置 DirectAccess 客户端

要将客户端计算机设置为使用 DirectAccess，它必须属于所选的安全组。 在配置 DirectAccess 后，将安全组中的客户端计算机设置为接收 DirectAccess 组策略。

1. 在“选择组”页上，单击“添加”。

2. 在 **“选择组”** 对话框中，选择包含 DirectAccess 客户端计算机的安全组。

3. 选中“仅为移动计算机启用 DirectAccess” 复选框以仅允许移动计算机访问内部网络。

4. 选中“使用强制隧道”复选框，以通过远程访问服务器路由所有客户端通信（到内部网络和 Internet）。

5. 单击“下一步”。

## <a name="configure-the-network-topology"></a>配置网络拓扑

若要部署远程访问，你需要配置具有正确网络适配器的远程访问服务器、客户端计算机可以连接到的远程访问服务器的公共 URL（连接到地址），以及使用者匹配连接到地址的 IP-HTTPS 证书。

1. 在“网络拓扑” 页上，单击将在你的组织中使用的部署拓扑。 在“键入客户端用于连接到远程访问服务器的公用名称或 IPv4 地址”中，输入部署的公用名称（此名称与 IP-HTTPS 证书的使用者名称相匹配，例如 edge1.contoso.com），然后单击“下一步”。

## <a name="configure-the-dns-suffix-search-list"></a>配置 DNS 后缀搜索列表

对于 DNS 客户端，你可以配置一个扩展或修改其 DNS 搜索功能的 DNS 域后缀搜索列表。 通过将其他后缀添加到列表，你可以在多个指定 DNS 域中搜索较短的、不合格的计算机名称。 然后，如果 DNS 查询失败，则 DNS 客户端服务可以使用此列表将其他名称后缀添加到原始名称末尾，并为这些备用 Fqdn 重复 DNS 服务器的 DNS 查询。

1. 选择“配置具有 DNS 客户端后缀搜索列表的 DirectAccess 客户端” 以指定用于客户端名称搜索的其他后缀。

2. 在 "**新后缀**" 中键入新后缀名称，然后单击 "**添加**"。 此外，你可以更改搜索顺序并从**域后缀中删除要使用**的后缀。

>纪录在非连续命名空间方案中 \(where 一个或多个域计算机具有的 DNS 后缀与计算机所属的 Active Directory 域（@ no__t）不匹配，应确保将搜索列表自定义为包括所有必需的一直. 默认情况下，远程访问向导会将 Active Directory DNS 名称配置为客户端上的主 DNS 后缀。 管理员应确保添加客户端使用的 DNS 后缀以进行名称解析。

对于计算机和服务器，以下默认 DNS 搜索行为是预先确定的，并且在完成和解析简短的非限定名称时使用。如果后缀搜索列表为空或未指定，则会将计算机的主 DNS 后缀追加到简短的非限定名称，并使用 DNS 查询来解析产生的 FQDN。 

如果此查询失败，计算机可以通过附加为网络连接配置的任何特定于连接的 DNS 后缀来尝试其他 Fqdn 查询。如果未配置连接特定的后缀，或者这些导致连接特定的 Fqdn 的查询失败，则客户端可以开始根据主后缀（也称为传递）的系统减少来重试查询。

例如，如果主后缀是 "example.microsoft.com"，则传递过程可以通过在 "microsoft.com" 和 "com" 域中搜索短名称来重试查询。

如果后缀搜索列表不为空且至少指定了一个 DNS 后缀，则会限制和解析短 DNS 名称，仅搜索指定后缀列表可能会出现的 Fqdn。 

如果通过附加和尝试列表中的每个后缀而形成的所有 FQDN 查询未得到解析，则查询过程失败，会产生“找不到名称”的结果。 

> [!WARNING]
> 如果使用了域后缀列表，则在查询未得到应答或解析时，客户端将根据不同的 DNS 域名继续发送其他备选查询。 一旦使用后缀列表中的某个条目解析名称，则不会再尝试未使用的列表条目。 因此，将最常用的域后缀排在第一位的列表排序方式效率最高。
> 
> 仅当 DNS 名称条目不是完全限定时，才使用域名后缀搜索。 若要使 DNS 名称完全合格，请在名称末尾输入结尾句点 (.)。

## <a name="gpo-configuration"></a>GPO 配置

配置远程访问时，DirectAccess 设置将收集到组策略对象（GPO）中。 

在 " **GPO 设置**" 中，将列出 DIRECTACCESS 服务器 gpo 名称和客户端 GPO 名称。 此外，你可以修改 GPO 选择设置。

这两个 Gpo 将自动填充 DirectAccess 设置，并以这种方式进行分发：

1. **DirectAccess 客户端 GPO**。 此 GPO 包含客户端设置，包括 IPv6 转换技术设置、NRPT 条目和高级安全 Windows 防火墙连接安全规则。 将 GPO 应用于为客户端计算机指定的安全组。

2. **DirectAccess 服务器 GPO**。 此 GPO 包含的 DirectAccess 配置设置适用于在部署中配置为远程访问服务器的任何服务器。 它还包含高级安全 Windows 防火墙连接安全规则。

## <a name="summary"></a>总结

远程访问配置完成后，将显示**摘要**。 您可以更改配置的设置，或单击 "**完成**" 以应用配置。
