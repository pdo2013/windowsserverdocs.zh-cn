---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: "检查 DNS 概念"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 29c4c3d1d785d1b3b1fa1926eca8298aab2b3ee3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-dns-concepts"></a>检查 DNS 概念

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

域名系统 (DNS) 是一种分布式的数据库表示命名空间。 命名空间包含所有任何客户端任何名称查找所需信息。 任何 DNS 服务器可以回答有关其命名空间的任何名称查询。 DNS 服务器回答查询，通过以下方法之一：  
  
-   如果答案为其缓存中，它回答查询从缓存。  
  
-   如果答案为托管 DNS 服务器的区域中，它回答查询从其区域。 区域是 DNS 树 DNS 服务器上存储的一部分。 当 DNS 服务器托管区域时，就权威有关该区域中的名称（，即 DNS 服务器可能回答查询为区域中的任何姓名）。 例如，主持区域 contoso.com 服务器可以回答查询 contoso.com 中的任何名。  
  
-   如果该服务器无法从其缓存或者区域回答查询，它将查询其他服务器的答案。  
  
它是理解的 DNS，如委派、递归名称分辨率，以及 Active Directory 集成 DNS 区域的核心功能非常重要，因为它们直接影响对您的 Active Directory 的逻辑结构设计。  
  
有关 DNS 和 Active Directory 域服务 (广告 DS) 的详细信息，请参阅[DNS 和广告 DS](../../ad-ds/plan/DNS-and-AD-DS.md)。  
  
## <a name="delegation"></a>委派  
为了回答有关的任何名称查询 DNS 服务器，必须具有权限命名空间中的每个区域直接或间接路径。 创建委派通过这些途径。 委派是列出名称服务器的下一个层次级别中的时区授权家长区域中的记录。 委派使客户端，请参考其他区域中的服务器一个区域中的服务器。 下图显示委派一个的示例。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
根本 DNS 服务器托管显示一个点根区域 (。 ). 根区域包含区域中分层，com 区域的下一步级别的委派。 根区域中的委派告诉根 DNS 服务器，若要查找 com 区域，它必须联系 Com 服务器。 同样，com 区域中的委派告知 Com 服务器，若要查找 contoso.com 区域，它必须联系 Contoso 服务器。  
  
> [!NOTE]  
> 委派使用两种类型的记录。 名称 (NS) 服务器资源记录提供权威服务器的名称。 主机 (A) 和主机 (AAAA) 资源记录提供 IP 版本 4 (IPv4) 和 IP 版本 6 (IPv6) 权威服务器地址。  
  
此系统的区域和委派创建表示 DNS 命名空间分层树。 每个区域表示的中分层，层和每个委派表示树分支。  
  
通过使用的区域和委托分级，根 DNS 服务器可以在 DNS 空间找到任何名称。 根区域包括委派直接间接或通向层次中所有其他区域。 可以查询根 DNS 服务器任何服务器可用于信息委派中找到的任何名称命名空间中。  
  
## <a name="recursive-name-resolution"></a>递归名称分辨率  
递归名称分辨率是依据 DNS 服务器使用的区域和委托层次响应的查询它的未授权的过程。  
  
在一些配置、DNS 服务器包括允许他们以查询根 DNS 服务器根提示（即的姓名和 IP 地址列表）。 在其他配置中，服务器向前它们不能回答到另一个服务器的所有查询。 转移和根的提示是 DNS 服务器可用于解决其他们并不权威查询这两种方法。  
  
### <a name="resolving-names-by-using-root-hints"></a>解决方法是使用根提示名称  
根提示启用了定位根 DNS 服务器任何 DNS 服务器。 DNS 服务器定位根 DNS 服务器后，它可以解决该命名空间任何查询。 下图说明 DNS 如何解决使用根提示的一个名称。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
在此示例中，将发生下列的事件：  
  
1.  客户端将递归查询发送到 DNS 服务器，以便请求名称 ftp.contoso.com 对应的 IP 地址。 递归查询指示客户端想其查询权威的答案。 有效的地址或一条消息，指示找不到该地址，则必须是递归查询响应。  
  
2.  由于 DNS 服务器不可权威的名称，并且在其缓存中没有答案，DNS 服务器使用根提示查找根 DNS 服务器的 IP 地址。  
  
3.  DNS 服务器用于迭代查询要求根 DNS 服务器解决 ftp.contoso.com 的名称。 迭代查询指示服务器接收的另一个服务器来替代查询权威的答案。 因为名称 ftp.contoso.com 结尾与标签 com，根 DNS 服务器会返回到托管 com 区域 Com 服务器参考信息。  
  
4.  DNS 服务器使用迭代查询询问 Com 服务器来解决 ftp.contoso.com 的名称。 由于名称 ftp.contoso.com 结束 contoso.com 的名称，Com 服务器返回引用到 Contoso 服务器该主机 contoso.com 区域。  
  
5.  DNS 服务器用于迭代查询要求 Contoso 服务器解决 ftp.contoso.com 的名称。 Contoso 服务器其区域数据中查找答案，并返回到服务器的答案。  
  
6.  向客户端，服务器再返回的结果。  
  
### <a name="resolving-names-by-using-forwarding"></a>解决方法是使用转移名称  
转移，可以通过特定服务器，而不是使用根提示路线名称分辨率。 下图说明 DNS 如何解决使用转移的名称。  
  
![DNS 概念](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
在此示例中，将发生下列的事件：  
  
1.  客户端查询 DNS 服务器 ftp.contoso.com 的名称。  
  
2.  DNS 服务器转发到另一个 DNS 服务器，称为转发器查询。  
  
3.  转发器不是权威的名称，并在其缓存中没有答案，因为它使用根提示查找根 DNS 服务器的 IP 地址。  
  
4.  转发器使用迭代查询要求根 DNS 服务器解决 ftp.contoso.com 的名称。 因为名称 ftp.contoso.com 名称结尾 com，根 DNS 服务器会返回到托管 com 区域 Com 服务器参考信息。  
  
5.  转发器使用迭代查询要求 Com 服务器解决 ftp.contoso.com 的名称。 由于名称 ftp.contoso.com 结束 contoso.com 的名称，Com 服务器返回引用到 Contoso 服务器该主机 contoso.com 区域。  
  
6.  转发器使用迭代查询要求 Contoso 服务器解决 ftp.contoso.com 的名称。 Contoso 服务器在其区域文件中，查找答案，并返回到服务器的答案。  
  
7.  然后，转发器原始 DNS 服务器返回的结果。  
  
8.  然后，原始 DNS 服务器向客户端返回的结果。  
  


