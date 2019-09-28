---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: 查看 DNS 概念
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0a1ffe065991e76c91fa95a6ac080a8e8d54bcce
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408705"
---
# <a name="reviewing-dns-concepts"></a>查看 DNS 概念

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

域名系统（DNS）是表示命名空间的分布式数据库。 命名空间包含任何客户端查找任何名称所需的所有信息。 任何 DNS 服务器都可以回答有关其命名空间中的任何名称的查询。 DNS 服务器使用以下方式之一来应答查询：  
  
- 如果答案在其缓存中，则从缓存应答查询。  
- 如果答案位于 DNS 服务器托管的区域中，则它会从其区域应答查询。 区域是 dns 服务器上存储的 DNS 树的一部分。 当 DNS 服务器承载某个区域时，它将对该区域中的名称具有权威（即，DNS 服务器可以响应区域中任何名称的查询）。 例如，承载区域 contoso.com 的服务器可以应答 contoso.com 中任何名称的查询。  
- 如果服务器无法从其缓存或区域应答查询，它会查询其他服务器以获取答案。  

了解 DNS 的核心功能（例如委托、递归名称解析和 Active Directory 集成的 DNS 区域）非常重要，因为它们对 Active Directory 逻辑结构设计有直接的影响。  
  
有关 DNS 和 Active Directory 域服务（AD DS）的详细信息，请参阅[dns 和 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)。  
  
## <a name="delegation"></a>委派

若要让 DNS 服务器回答有关任何名称的查询，它必须具有指向命名空间中每个区域的直接或间接路径。 这些路径是通过委托创建的。 委托是父区域中的一条记录，它列出了在层次结构的下一级别中对区域具有权威的名称服务器。 委托使一个区域中的服务器可以将客户端引用到其他区域中的服务器。 下图显示了一个委托示例。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
DNS 根服务器承载以圆点（。 )。 根区域包含对层次结构的下一级别中的区域的委派，即 com 区域。 根区域中的委托告知 DNS 根服务器，若要查找 com 区域，必须联系 Com 服务器。 同样，com 区域中的委派会告诉 Com 服务器，若要查找 contoso.com 区域，必须联系 Contoso 服务器。  
  
> [!NOTE]  
> 委托使用两种类型的记录。 名称服务器（NS）资源记录提供权威服务器的名称。 主机（A）和主机（AAAA）资源记录提供权威服务器的 IP 版本4（IPv4）和 IP 版本6（IPv6）地址。  
  
此区域和委托系统创建一个表示 DNS 命名空间的层次结构树。 每个区域代表层次结构中的一个层，每个委托表示一个树的分支。  
  
通过使用区域和委托的层次结构，DNS 根服务器可以查找 DNS 命名空间中的任何名称。 根区域包括直接或间接导致层次结构中的所有其他区域的委托。 可以查询 DNS 根服务器的任何服务器都可以使用委托中的信息来查找命名空间中的任何名称。  
  
## <a name="recursive-name-resolution"></a>递归名称解析

递归名称解析是指 DNS 服务器使用区域和委托的层次结构来响应其不具有权威的查询的过程。  
  
在某些配置中，DNS 服务器包括根提示（即名称和 IP 地址的列表），使其能够查询 DNS 根服务器。 在其他配置中，服务器会将其无法回答的所有查询转发到另一台服务器。 转发和根提示都是 DNS 服务器可用于解析其不具有权威的查询的方法。  
  
### <a name="resolving-names-by-using-root-hints"></a>使用根提示解析名称

根提示允许任何 DNS 服务器定位 DNS 根服务器。 DNS 服务器定位 DNS 根服务器后，可以解析该命名空间的任何查询。 下图介绍了如何使用根提示来解析 DNS 的名称。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
在此示例中，将发生以下事件：  
  
1. 客户端向 DNS 服务器发送递归查询，请求与名称 ftp.contoso.com 对应的 IP 地址。 递归查询指示客户端希望其查询有一个明确的答案。 对递归查询的响应必须是有效的地址或消息，指示找不到该地址。  
2. 由于 DNS 服务器不是名称的权威服务器，并且在其缓存中没有应答，因此 DNS 服务器使用根提示来查找 DNS 根服务器的 IP 地址。  
3. DNS 服务器使用迭代查询来请求 DNS 根服务器解析名称 ftp.contoso.com。 迭代查询指示服务器将接受对其他服务器的引用以替代查询的确切答案。 因为名称 ftp.contoso.com 以标签 com 结尾，所以 DNS 根服务器将返回对托管 com 区域的 Com 服务器的引用。  
4. DNS 服务器使用迭代查询来请求 Com 服务器解析名称 ftp.contoso.com。 由于名称 ftp.contoso.com 以名称 contoso.com 结尾，因此 Com 服务器将向承载 contoso.com 区域的 Contoso 服务器返回一个引用。  
5. DNS 服务器使用迭代查询来请求 Contoso 服务器解析名称 ftp.contoso.com。 Contoso 服务器在其区域数据中查找答案，然后将答案返回给服务器。  
6. 服务器随后将结果返回给客户端。  
  
### <a name="resolving-names-by-using-forwarding"></a>使用转发来解析名称

通过转发，可以通过特定服务器路由名称解析，而不是使用根提示。 下图说明了 DNS 如何使用转发来解析名称。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
在此示例中，将发生以下事件：  
  
1. 客户端在 DNS 服务器中查询名称 ftp.contoso.com。  
2. DNS 服务器将查询转发到另一台 DNS 服务器，称为转发器。  
3. 因为转发器不是名称的权威，并且它的缓存中没有答案，所以它使用根提示来查找 DNS 根服务器的 IP 地址。  
4. 转发器使用迭代查询来请求 DNS 根服务器解析名称 ftp.contoso.com。 由于名称 ftp.contoso.com 以 "com" 名称结尾，因此 DNS 根服务器将返回对托管 com 区域的 Com 服务器的引用。  
5. 转发器使用迭代查询来请求 Com 服务器解析名称 ftp.contoso.com。 由于名称 ftp.contoso.com 以名称 contoso.com 结尾，因此 Com 服务器将向承载 contoso.com 区域的 Contoso 服务器返回一个引用。  
6. 转发器使用迭代查询来请求 Contoso 服务器解析名称 ftp.contoso.com。 Contoso 服务器在其区域文件中查找答案，然后将答案返回给服务器。  
7. 然后，转发器将结果返回给原始 DNS 服务器。  
8. 然后，原始 DNS 服务器会将结果返回给客户端。  
