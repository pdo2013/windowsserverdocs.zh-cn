---
ms.assetid: 3acaa977-ed63-4e38-ac81-229908c47208
title: "如何处理 LDAP 服务器 Cookie"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 89369cb1e52a315520062ca5ecc96b66ac3e2bfc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="how-ldap-server-cookies-are-handled"></a>如何处理 LDAP 服务器 Cookie

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

LDAP，设置较大的结果中某些查询结果。 此类查询到 Windows Server 带来一些挑战。  
  
收集并构建这些结果大集是重大工作。 很多属性需要将从内部的表示形式转换为 LDAP wire 身份特征。 对于许多属性，从内部、 通常二进制，格式的转换需要为基于文本的接格式 LDAP 响应框架中发生的情况。  
  
另一个难题是结果设置具有成千上万个变得较大的对象轻松几个几百万像素-字节。 这些然后需要大量虚拟地址空间，并且当 TCP 会话中断在乘公交时，会丢失整个努力通过网络传输还有问题。  
  
这些容量和物流问题已通向撰写电子邮件开发人员创建了一个名为"分页查询"LDAP 扩展。 它实现 LDAP 控件要将一个巨大查询拆分为小结果集区块的数据。 它已成为作为 RFC 标准[RFC 2696](http://www.ietf.org/rfc/rfc2696)。  
  
## <a name="cookie-handling-on-client"></a>在客户端上处理的 cookie  
分页查询方法使用任一设置客户端，或通过页面大小[LDAP 策略](https://support.microsoft.com/kb/315071/en-us)("MaxPageSize")。 客户端始终需要启用分页发送 LDAP 控制。  

  
当使用多的结果与查询，有时被达到对象允许最大数目。 LDAP 服务器打包响应消息，并添加 cookie，其中包含更高版本继续进行搜索所需信息。  
  
客户端应用程序必须将视为透明斑点的 cookie。 它可以检索计数在响应，并且可以继续进行搜索的 cookie 中存在基于。客户通过将查询发送到再次使用相同的参数基本对象和筛选器，如 ldap 继续搜索并包括对以前响应返回的 cookie 值。  
  
如果对象数不会填满页面，LDAP 查询完毕和响应包含任何页面 cookie。 如果该服务器不返回任何 cookie，则客户端必须考虑已分页的搜索要成功完成。  
  
如果该服务器返回一个错误，则客户端必须考虑已分页的搜索内容，以将失败。 重新尝试收取费用搜索将导致重新第一页中的搜索。  
  
## <a name="server-side-cookie-handling"></a>服务器端 Cookie 处理  
Windows Server 返回给客户端的 cookie 和有时存储在服务器上的 cookie 与相关信息。 此信息存储在服务器缓存中，并遵循某些限制。  
  
在此情况下，服务器发送给客户端的 cookie 还用于服务器来查找在服务器上的缓存中的信息。 当客户端将继续分页的搜索时、 Windows Server 将使用客户端 cookie，以及相关的任何信息从服务器 cookie 缓存继续进行搜索。 如果该服务器找不到因任何原因而服务器缓存中的 cookie 相关的信息，搜索已停用并错误返回给客户。  
  
## <a name="how-the-cookie-pool-is-managed"></a>如何管理 cookie 池  
很显然，LDAP 服务器服务一次的多个客户端，还多个一次的客户端可以启动需要使用服务器 cookie 缓存的查询。这样的 Windows Server 实现是跟踪的 cookie 池使用及限制置于入到位以便 cookie 池不花费太多资源。 限制可以设置由管理员 LDAP 策略中使用以下设置。 默认设置和解释是：  
  
**MinResultSets: 4**  
  
如果有小于 MinResultSets 条目服务器 cookie 缓存中，将不时，如下所述的池最大大小查找 LDAP 服务器。  
  
**MaxResultSetSize： 创建到 262144 字节**  
  
在服务器上的总 cookie 缓存大小不能超过以字节表示 MaxResultSetSize 的最大。 如果是这样，池小于 MaxResultSetSize 字节或 MinResultSets cookie 小于处于池之前，将会删除 cookie 从古老开始。 这意味着，使用默认设置，ldap 考虑的 450 KB 仅有存储 3 cookie 为确定池。  
  
**MaxResultSetsPerConn: 10**  
  
Ldap 允许不超过每个池中 LDAP 连接 MaxResultSetsPerConn cookie。  
  
## <a name="handling-deleted-cookies"></a>处理删除 Cookie  
删除 cookie LDAP 服务器缓存中的信息不会导致应用程序在所有的情况下即时错误。 应用程序可能会重新启动从开始菜单已分页进行搜索，并完成它在另一台尝试。 某些应用程序有此类重试机制要添加的稳定性。  
  
某些应用程序可能会浏览的页面的搜索，并永远不会完成它。 这可能会使条目 LDAP 服务器中的进行处理通过机制第 4 部分中的 cookie 缓存。 这是非常重要，释放活动 LDAP 搜索服务器上的内存。  
  
在服务器上删除此类 cookie 和客户端将继续使用此 cookie 句柄搜索时，会发生什么情况？Ldap 将不 cookie 缓存服务器中查找 cookie，并返回查询错误，错误响应将类似于：  
  
```  
00000057: LdapErr: DSID-xxxxxxxx, comment: Error processing control, data 0, v1db1  
```  
  
> [!NOTE]  
> "DSID"背后的十六进制值因 LDAP 服务器二进制文件的内部版本号。  
  
## <a name="reporting-on-the-cookie-pool"></a>Cookie 池上报告  
LDAP 服务器有能够登录事件通过类别"16 Ldap 接口" [NTDS 诊断键](https://support.microsoft.com/kb/314980/en-us)。 如果您将设置为"2"此类别，你可以通过以下事件：  
  
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
  
这些事件信号移除了存储的 cookie。 并不意味着客户端过 LDAP 错误，但只 Ldap 已达到用于缓存管理限制。  在某些情况下，LDAP 客户端可能已放弃分页的搜索，并可能永远不会看到此错误。  
  
## <a name="monitoring-the-cookie-pool"></a>监视 cookie 池  
你永远不会在域中遇到 LDAP 搜索错误，如果你永远不会可能需要监视 LDAP 服务器页面搜索 cookie 池。 在你看到您的环境中搜索相关的错误 LDAP 页面的情况下可能具有 cookie 池管理员限制出现问题。  
  
事件 2898年和 2899年经过知道 ldap 已达到管理员限制的唯一方法。 由于处理错误上述控制遇到以查看该 LDAP 查询错误，应看增加上一个或多个 4，具体取决于的事件你会收到一节中提到的 LDAP 策略设置的限制。  
  
如果你看到事件 2898 DC /LDAP 服务器上，我们建议你设置为 25 MaxResultSetsPerConn。 通过单个 LDAP 连接的多于 25 并行分页的搜索不常用。 如果你仍看到 2898年事件，请考虑调查遇到该错误 LDAP 客户端应用程序。 某种程度上停滞不检索其他分页的结果，离开待处理的 cookie，并且重启新查询是可疑。 因此查看是否在某些应用程序会有足够的 cookie 有关其用途，你看到事件 2899年域控制器在登录时，还可以增加 MaxResultSetsPerConn 25.之外的值、 该套餐可能会不同。 如果在具有足够的内存 (几个 Gb 的可用内存) 的计算机上运行 DC /LDAP 服务器，我们建议你 MaxResultsetSize 服务器上设置 LDAP 为 > = 250 MB。 此限制为大到足以适应大量 LDAP 页面上的搜索甚至非常大的目录。  
  
如果你是仍查看事件 2899年 250 MB 或更高的池，你有可能与很高的数字返回的对象有许多客户端，，询问非常频繁的方式。 你可以通过收集的数据[Active Directory 的数据收集设置](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx)可以帮助你找到重复保留 LDAP 服务器的已分页的查询闲。 这些查询所有将显示了大量的"条目返回"相匹配所使用的大小。  
  
如果可能，应查看应用程序的设计，并采用不同的做法，使用较低的频率、 数据音量和/或更少的客户端实例查询此数据。您对该源代码访问权，本指南提供的应用程序情形[创建高效 AD-Enabled 应用程序](https://msdn.microsoft.com/en-us/library/ms808539.aspx)可帮助您了解的应用程序访问广告的最佳方式。  
  
如果无法更改查询行为，一种方法还添加更复制的实例命名所需的上下文，还可以重新分发客户端和最终减少加载的各个 LDAP 服务器上。  
  


