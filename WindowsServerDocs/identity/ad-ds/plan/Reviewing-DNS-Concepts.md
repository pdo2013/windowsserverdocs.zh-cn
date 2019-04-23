---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: 查看 DNS 概念
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d077ecc30caca9b8f3b382af624121fec729afae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890498"
---
# <a name="reviewing-dns-concepts"></a>查看 DNS 概念

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

域名系统 (DNS) 是一个分布式的数据库，表示一个命名空间。 该命名空间包含的所有信息所需的任何客户端，若要查找任何名称。 任何 DNS 服务器可以回答有关其命名空间中的任何名称的查询。 DNS 服务器应答查询的以下方法之一：  
  
- 如果回答是在其缓存中，它从缓存应答查询。  
- 如果回答是 DNS 服务器托管的区域中，将从该区域回答查询。 区域是存储在 DNS 服务器上的 DNS 树的一部分。 当 DNS 服务器托管了一个区域时，它是该区域中的名称的授权 （即，DNS 服务器可以响应针对查询该区域中的任何名称）。 例如，托管区域 contoso.com 的服务器可以响应针对 contoso.com 中的任何名称的查询。  
- 如果服务器不能从其缓存或区域中回答查询，它会查询答案的其他服务器。  

它是务必了解 DNS，例如委派、 递归式名称解析和 Active Directory 集成的 DNS 区域的核心功能，因为它们对 Active Directory 逻辑结构设计有直接的影响。  
  
有关 DNS 和 Active Directory 域服务 (AD DS) 的详细信息，请参阅[DNS 和 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)。  
  
## <a name="delegation"></a>委派

若要回答有关的任何名称的查询的 DNS 服务器，它必须具有到命名空间中的每个区域的直接或间接路径。 这些路径是通过委托创建的。 委派是列出在下一级别的层次结构中区域具有权威的名称服务器的父区域中的记录。 委派使一个区域来引用到其他区域中的服务器的客户端中的服务器。 下图显示了一个示例中的委派。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
DNS 根服务器承载根区域表示为一个点 (。 )。 根区域包含到下一级别的层次结构中，com 区域中的区域的委派。 在根区域委派告诉 DNS 根服务器，若要查找 com 区域，它必须联系 Com 服务器。 同样，在 com 区域委派指示 Com 服务器，若要查找 contoso.com 区域，它必须联系 Contoso 服务器。  
  
> [!NOTE]  
> 委派使用两种类型的记录。 名称服务器 (NS) 资源记录提供权威服务器的名称。 主机 (A) 和主机 (AAAA) 资源记录提供 IP 版本 4 (IPv4) 和 IP 版本 6 (IPv6) 地址的权威服务器。  
  
此系统的区域和委派创建表示 DNS 命名空间的层次结构树。 每个区域表示在层次结构中的层，每个委托表示树的分支。  
  
通过使用区域和委派的层次结构，DNS 根服务器可以找到 DNS 命名空间中的任何名称。 根区域包含会直接或间接导致层次结构中的所有其他区域的委派。 任何服务器可以查询 DNS 根服务器的信息可用于委派中的命名空间中找到的任何名称。  
  
## <a name="recursive-name-resolution"></a>递归式名称解析

递归式名称解析是按其 DNS 服务器使用的区域和委派的层次结构响应的查询，其不具有权威的过程。  
  
在某些配置中，DNS 服务器包含使其能够查询 DNS 根服务器根提示 （即，名称和 IP 地址列表）。 在其他配置中，服务器将转发到另一台服务器在不能回答的所有查询。 转发和根提示是可以使用 DNS 服务器来解析为其它们不具有权威的查询这两种方法。  
  
### <a name="resolving-names-by-using-root-hints"></a>通过使用根提示解析名称

根提示启用查找 DNS 根服务器的任何 DNS 服务器。 DNS 服务器查找 DNS 根服务器后，它可以解析为该命名空间的任何查询。 下图介绍了 DNS 如何通过使用根提示解析名称。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
在此示例中，将发生以下事件：  
  
1. 客户端发送以请求为名称 ftp.contoso.com 对应的 IP 地址的 DNS 服务器递归查询。 递归查询指示客户端想要向其查询是明确的答案。 递归查询的响应必须是有效的地址或一条消息指出找不到该地址。  
2. 因为 DNS 服务器不具有对该名称，并在其缓存中没有答案，DNS 服务器将使用根提示来查找 DNS 根服务器的 IP 地址。  
3. DNS 服务器使用的迭代查询要求 DNS 根服务器以解析名称 ftp.contoso.com。 迭代的查询指示服务器将接受对另一台服务器来对查询是明确的答案代替的检索。 因为名称 ftp.contoso.com 结尾标签 com，DNS 根服务器返回一个引用到承载 com 区域的 Com 服务器。  
4. DNS 服务器使用的迭代查询要求 Com 服务器以解析名称 ftp.contoso.com。 因为名称 ftp.contoso.com 结束了名为 contoso.com，Com 服务器返回一个引用与 Contoso 服务器托管 contoso.com 区域。  
5. DNS 服务器使用的迭代的查询来要求 Contoso 服务器以解析名称 ftp.contoso.com。 Contoso 服务器在其区域数据中查找答案，然后返回到服务器的答案。  
6. 然后，在服务器到客户端返回结果。  
  
### <a name="resolving-names-by-using-forwarding"></a>通过使用转发解析名称

转发使您能够通过特定服务器而不是使用根提示的路由名称解析。 下图介绍了 DNS 如何通过使用转发解析名称。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
在此示例中，将发生以下事件：  
  
1. 客户端查询名称 ftp.contoso.com 的 DNS 服务器。  
2. DNS 服务器将转发到另一个 DNS 服务器，名为转发器的查询。  
3. 转发器不具有对该名称，并且在其缓存中没有答案，因为它使用根提示来查找 DNS 根服务器的 IP 地址。  
4. 转发器使用迭代查询要求 DNS 根服务器以解析名称 ftp.contoso.com。 因为名称 ftp.contoso.com 的名称结尾 com，DNS 根服务器返回一个引用到承载 com 区域的 Com 服务器。  
5. 转发器使用迭代查询要求 Com 服务器以解析名称 ftp.contoso.com。 因为名称 ftp.contoso.com 结束了名为 contoso.com，Com 服务器返回一个引用与 Contoso 服务器托管 contoso.com 区域。  
6. 转发器使用迭代的查询来要求 Contoso 服务器以解析名称 ftp.contoso.com。 Contoso 服务器在其区域文件，查找答案，然后返回到服务器的答案。  
7. 然后，转发器将结果返回到原来的 DNS 服务器。  
8. 然后，原始的 DNS 服务器到客户端返回结果。  
