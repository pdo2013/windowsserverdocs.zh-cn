---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: 如何处理 LDAP 服务器 Cookie
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c825ae9c9b52068b58b99bc6ff597304c9643d17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390078"
---
# <a name="how-ldap-server-cookies-are-handled"></a>如何处理 LDAP 服务器 Cookie

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 LDAP 中，一些查询会生成大型结果集。 此类查询给 Windows Server 带来了一些难题。  
  
收集和构建这些大型结果集是重要的工作。 许多属性需要从内部表示形式转换为 LDAP 网络表示形式。 对于许多属性而言，需要在响应帧中从内部格式（通常是二进制）转换为基于文本的 UTF-8 格式。  
  
另一个难题是包含数万个对象的结果集会变得很大，很容易几百兆字节。 然后这些结果集需要大量虚拟地址空间，并且通过网络传输也有问题，因为在传输过程中 TCP 会话中断时，整个工作将丢失。  
  
这些容量和逻辑问题促使 Microsoft LDAP 开发人员创建称为 "分页查询" 的 LDAP 扩展。 它正在实现一个 LDAP 控件，用于将一个大型查询分解成多个较小的结果集块。 它已经是作为 [RFC 2696](http://www.ietf.org/rfc/rfc2696)的 RFC 标准。  
  
## <a name="cookie-handling-on-client"></a>在客户端上处理的 Cookie  
分页查询方法使用由客户端或通过[LDAP 策略](https://support.microsoft.com/kb/315071/en-us)（"MaxPageSize"）设置的页大小。 客户端始终需要通过发送 LDAP 控件来启用分页。  

  
处理包含多个结果的查询时，在某一时刻会达到允许的最大对象数。 LDAP 服务器将打包响应消息，并添加包含稍后继续搜索所需信息的 Cookie。  
  
客户端应用程序必须将 Cookie 视为不透明的 Blob。 它可以检索响应中的对象计数，并可以根据存在的 Cookie 继续搜索。客户端通过再次使用相同的参数（如基对象和筛选器）将查询发送到 LDAP 服务器，并提供前一响应返回的 Cookie 值来继续搜索。  
  
如果对象数未填满页面，则 LDAP 查询已完成，并且响应将不包含页 cookie。 如果服务器未返回任何 Cookie，则客户端必须认为分页搜索成功完成。  
  
如果服务器返回任何错误，则客户端必须认为分页搜索未成功。 重试搜索将导致从第一页重新开始搜索。  
  
## <a name="server-side-cookie-handling"></a>服务器端的 Cookie 处理  
Windows Server 将 Cookie 返回给客户端，并有时在服务器上存储与 Cookie 相关的信息。 此信息存储在服务器的缓存中，并受到某些限制的约束。  
  
在这种情况下，由服务器发送到客户端的 Cookie 也用于从服务器的缓存中查找信息。 当客户端继续分页搜索时，Windows Server 将使用客户端 Cookie 以及服务器 Cookie 缓存中的任何相关信息继续搜索。 如果由于任何原因服务器从服务器缓存中找不到相关 Cookie 信息，搜索将停止，并向客户端返回错误。  
  
## <a name="how-the-cookie-pool-is-managed"></a>如何管理 Cookie 池  
显然，LDAP 服务器同时为多个客户端提供服务，同时多个客户端也可以启动需要使用服务器 Cookie 缓存的查询。因此，该处的 Windows Server 实现是跟踪 Cookie 池使用情况，并将限制付诸实施，使 Cookie 池不会占用过多资源。 管理员可以使用 LDAP 策略中的以下设置设置限制。 默认值和说明如下：  
  
**MinResultSets：4 @ no__t-0  
  
如果服务器 Cookie 缓存中的条目数小于 MinResultSets，LDAP 服务器将不会查看下文讨论的最大池大小。  
  
**MaxResultSetSize：262144字节 @ no__t-0  
  
服务器上的总 Cookie 缓存大小不得超过 MaxResultSetSize 的最大值（以字节为单位）。 如果超过了，将从最旧的 Cookie 开始删除，直到该池小于 MaxResultSetSize 个字节或池中的 Cookie 数小于 MinResultSets 值。 这意味着使用默认设置，如果仅存储了 3 条 Cookie，LDAP 服务器会认为 450KB 的池是正常的。  
  
**MaxResultSetsPerConn：10 @ no__t-0  
  
LDAP 服务器不允许针对池中的每个 LDAP 连接有超过 MaxResultSetsPerConn 条 Cookie。  
  
## <a name="handling-deleted-cookies"></a>处理删除的 Cookie  
在所有情况下，从 LDAP 服务器缓存中删除 Cookie 信息不会导致应用程序出现即时错误。 应用程序可以在另一次尝试中从头开始重新启动分页搜索，并完成它。 某些应用程序利用这种重试机制来添加稳健性。  
  
某些应用程序可能会实行页搜索，但从不完成它。 这可能会在 LDAP 服务器 Cookie 缓存中留下条目，这些条目将通过第 4 节中的机制来处理。 至关重要的是要释放服务器上活动 LDAP 搜索所占用的内存。  
  
当在服务器上删除了此类 Cookie，而客户端继续使用此 Cookie 句柄搜索时，会发生什么情况？LDAP 服务器将不会在服务器 Cookie 缓存中找到该 Cookie，而是会针对查询返回错误，错误响应将类似于：  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> "DSID" 背后的十六进制值将因 LDAP 服务器二进制文件的内部版本而异。  
  
## <a name="reporting-on-the-cookie-pool"></a>关于 Cookie 池的报告  
LDAP 服务器能够通过[NTDS 诊断密钥](https://support.microsoft.com/kb/314980/en-us)中的类别 "16 LDAP 接口" 记录事件。 如果将此类别设置为 "2"，则可以获得以下事件：  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2898  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has reached the limit of the number of Result Sets it will maintain for a single connection.  A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.  
Maximum number of Result Sets allowed per LDAP connection:  
10  
Current number of Result Sets for this LDAP connection:  
11  
  
User Action  
The client should consider a more efficient search filter.  The limit for Maximum Result Sets per Connection may also be increased.  
  
```  
  
```  
Log Name:      Directory Service  
Source:        Microsoft-Windows-ActiveDirectory_DomainService  
Event ID:      2899  
Task Category: LDAP Interface  
Level:         Information  
Description:  
Internal event: The LDAP server has exceeded the limit of the LDAP Maximum Result Set Size. A stored Result Set will be discarded.  This will result in a client being unable to continue a paged LDAP search.   
  
Number of result sets currently stored:   
4   
Current Result Set Size:   
263504   
Maximum Result Set Size:   
262144   
Size of single Result Set being discarded:   
40876   
User Action   
The client should consider a more efficient search filter.  The limit for Maximum Result Set Size may also be increased.  
  
```  
  
这些事件发出“已删除存储的 Cookie”的信号。 它并不意味着客户端已发现 LDAP 错误，而只是 LDAP 服务器已达到缓存的管理限制。  在某些情况下，LDAP 客户端可能已放弃分页搜索，并可能永远不会发现该错误。  
  
## <a name="monitoring-the-cookie-pool"></a>监视 Cookie 池  
如果你在域中从未遇到 LDAP 搜索错误，则可能从不需要监视 LDAP 服务器页搜索 Cookie 池。 如果你在环境中看到 LDAP 页搜索相关错误，则可能遇到 Cookie 池管理员限制的问题。  
  
事件 2898 和 2899 是了解 LDAP 服务器已达到管理员限制的唯一方式。 当你因处理上述错误的控件而遇到 LDAP 查询出错时，应查看第 4 节中提到的一个或多个 LDAP 策略设置的限制是否增加，具体取决于你收到的是哪个事件。  
  
如果你在 DC/LDAP 服务器上看到事件 2898，建议你将 MaxResultSetsPerConn 设为 25。 在单个 LDAP 连接上通常不会有超过 25 个并行分页搜索。 如果你仍看到事件 2898，请考虑调查遇到该错误的 LDAP 客户端应用程序。 怀疑是在检索后续分页结果时它不知何故停止响应了，将 Cookie 挂起，并重新启动一个新查询。 因此，请查看在某一时刻应用程序是否有足够多的 Cookie 用于其用途，你还可以将 MaxResultSetsPerConn 的值增加到超出 25。当你看到域控制器上记录的事件 2899 时，计划将会不同。 如果你的 DC/LDAP 服务器运行在具有足够内存（几 GB 的可用内存）的计算机上，建议你在 LDAP 服务器上将 MaxResultsetSize 设为 >= 250MB。 此限制已足够大，甚至可以针对非常大的目录提供大量 LDAP 页搜索。  
  
如果你在使用 250MB 或更大容量的池时仍看到事件 2899，则你很可能正在使用许多客户端非常频繁地查询大量对象并返回这些对象。 使用 [Active Directory 数据收集器集](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx) 可以帮你查找使 LDAP 服务器繁忙的重复性分页查询。 这些查询全部显示，其中包含与所用页面大小匹配的 "返回的条目数"。  
  
如果可能，你应查看应用程序设计，并以较低的频率、数据量和/或更少的客户端实例查询此数据来实现其他方法。如果你有权访问源代码的应用程序，本[指南可帮助](https://msdn.microsoft.com/library/ms808539.aspx)你了解应用程序访问 ad 的最佳方式。  
  
如果不能更改查询行为，一种方法也会添加所需命名上下文的更多复制实例，并重新分发客户端，并最终降低单个 LDAP 服务器上的负载。  
  


