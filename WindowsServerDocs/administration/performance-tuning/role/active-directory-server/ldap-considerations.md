---
title: LDAP 中 ADDS 性能优化的注意事项
description: 在 Active Directory 工作负荷中的 LDAP 注意事项
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9032988c65581ea602451d224f40719b932ab7f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821688"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>LDAP 中 ADDS 性能优化的注意事项

>[!Important]
> 以下是需要优化服务器硬件的详细介绍的更高版本中的 Active Directory 工作负荷的关键的建议和注意事项的摘要[Active Directory 域服务的容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)一文。 读者都高度建议，若要查看[Active Directory 域服务的容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)的更高版本的技术知识和这些建议的影响。

## <a name="verify-ldap-queries"></a>验证 LDAP 查询

验证 LDAP 查询符合创建高效的查询建议。

没有大量文档在 MSDN 上有关如何正确编写的结构，并分析针对 Active Directory 使用的查询。 有关详细信息，请参阅[Creating More Efficient Microsoft Active Directory-Enabled 应用程序](https://msdn.microsoft.com/library/ms808539.aspx)。

## <a name="optimize-ldap-page-sizes"></a>优化 LDAP 页大小

当客户端请求的响应中返回多个对象的结果，域控制器必须将结果集在内存中临时存储。 不断增加的页面大小将导致使用更多内存，并会不必要地老化超出缓存的项。 在这种情况下，默认设置是最佳的。 有几种方案建议已做出以增加的页面大小设置。 我们建议使用默认值，除非专门标识为不能满足需要。

如果查询具有多个结果，可能会遇到的并发执行的类似查询的限制。  发生这种情况是因为 LDAP 服务器可能会耗尽称为 cookie 池的全局内存区域。  它可能需要增加池的大小，如中所述[LDAP 服务器 Cookie 的处理方式](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled)。

若要调整这些设置，请参阅[Windows Server 2008 和更高版本的域控制器在 LDAP 响应中返回仅 5000 值](https://support.microsoft.com/kb/2009267)。

## <a name="determine-whether-to-add-indices"></a>确定是否添加索引

索引属性可搜索的筛选器中具有的属性名称的对象时的不同而不同。 索引可以减少必须评估筛选器时访问的对象数。 但是，必须更新索引，如果修改或添加相应的属性，因此这将减少写入操作的性能。 它也会增加目录数据库的大小，但通常利存储成本。 可以使用日志记录以查找昂贵且效率低下的查询。 确定后，请考虑索引在相应的查询中使用以提高搜索性能的某些属性。 Active Directory 搜索的工作原理的详细信息，请参阅[Active Directory 搜索工作原理](https://technet.microsoft.com/library/cc755809.aspx)。

### <a name="scenarios-that-benefit-in-adding-indices"></a>添加索引中受益的方案

-   在请求数据的客户端负载正在生成大量的 CPU 使用率和客户端查询行为不能更改或优化。 很高的负荷，请考虑它在服务器性能审查程序或内置 Active Directory 数据收集器集的前 10 个恶意程序列表中显示其自身，并使用多个 CPU 的 1%。

-   客户端负载正在生成大量的磁盘 I/O，由于未编入索引的属性的服务器上并不能更改或优化客户端查询行为。

-   查询需要很长时间，并且无法完成向客户端由于可接受时间范围内缺乏涵盖索引。

-   使用情况和的 ATQ LDAP 线程耗尽导致高持续时间的查询量很大。 监视以下性能计数器：

    -   **NTDS\\请求的延迟时间**– 这是请求进入进程受多长时间。 Active Directory 后 120 秒 （默认值） 超时的请求，但是，大多数应运行速度更快并且极长时间运行的查询应获取隐藏状态中总体的数字。 查找此基线，而不是绝对阈值中的更改。

        **请注意**  较高值也可以是"代理程序中延迟指标？ 与其他域和 CRL 检查的请求。


    -   **NTDS\\估计队列延迟**– 这理想情况下应接近于 0 以获得最佳性能是因为这意味着，请求没有时间等待服务。

这些方案可以检测到使用一个或多个以下方法：

-   [确定查询计时和统计信息控件](https://msdn.microsoft.com/library/ms808539.aspx)

-   [跟踪成本高昂且效率低下的搜索](https://msdn.microsoft.com/library/ms808539.aspx)

-   Active Directory 诊断数据收集器集在性能监视器 ([的 SPA:AD 数据收集器集及其以后版本中 Win2008](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx))

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Active Directory Advisor 包

-   搜索使用除了任何筛选器"(objectClass =\*)？ 使用上级索引。

### <a name="other-index-considerations"></a>其他索引注意事项

-   请确保，对查询进行优化已耗尽作为一个选项后，创建该索引是解决问题的正确解决方案。 正确调整硬件大小是非常重要。 仅在正确的解决方法是索引属性，并且不尝试进行模糊处理的硬件问题时，应添加索引。

-   索引由要编制索引的属性的总大小至少增加数据库的大小。 因此可以通过属性中获取数据的平均大小并乘以将具有填充的属性的对象数来计算数据库增长的估计值。 通常这是有关在数据库大小增加了 1%。 有关详细信息，请参阅[数据存储区的工作原理](https://technet.microsoft.com/library/cc772829.aspx)。

-   如果在组织单位级别主要是为了完成搜索行为，请考虑按容器进行搜索索引。

-   元组索引是大于常规索引，但若要估计大小困难得多。 最多 20%的增长，用作基底正常大小估计的索引。 有关详细信息，请参阅[数据存储区的工作原理](https://technet.microsoft.com/library/cc772829.aspx)。

-   如果在组织单位级别主要是为了完成搜索行为，请考虑按容器进行搜索索引。

-   元组索引需要支持词中搜索字符串和最后一个搜索字符串。 初始搜索字符串不需要元组索引。

    -   初始搜索字符串-(samAccountName = MYPC\*)

    -   所得的搜索字符串-(samAccountName =\*MYPC\*)

    -   最后一个搜索字符串-(samAccountName =\*MYPC$)

-   创建索引将生成的磁盘 I/O，而正在生成索引。 具有低优先级后台线程上执行此操作，并传入的请求将优先于索引生成。 如果已正确完成环境的容量规划，这应是透明的。 但是，写入密集型方案或域控制器存储上的负载是未知的环境可能会降低客户端体验，并且应完成非工作时间。

-   因为构建索引都是在本地，会对复制流量影响很小。

有关详细信息，请参阅以下文章：

-   [创建更高效的 Microsoft Active Directory 启用应用程序](https://msdn.microsoft.com/library/ms808539.aspx)

-   [在 Active Directory 域服务中搜索](https://msdn.microsoft.com/library/aa746427.aspx)

-   [编入索引的属性](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)


## <a name="see-also"></a>请参阅
- [性能优化 Active Directory 服务器](index.md)
- [硬件注意事项](hardware-considerations.md)
- [正确放置域控制器和站点的注意事项](site-definition-considerations.md)
- [ADDS 性能故障排除](troubleshoot.md) 
- [Active Directory 域服务的容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)