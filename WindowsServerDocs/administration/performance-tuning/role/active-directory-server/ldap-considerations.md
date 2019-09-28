---
title: 添加性能优化中的 LDAP 注意事项
description: Active Directory 工作负荷中的 LDAP 注意事项
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f6670c8cfd718360518869f0551461c45e5aed27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370280"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>添加性能优化中的 LDAP 注意事项

> [!IMPORTANT]
> 下面概述了优化 Active Directory 工作负荷的服务器硬件的关键建议和注意事项，详细介绍了 Active Directory 域服务文章的[容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)。 为了更好地理解和影响这些建议，强烈建议读者查看[Active Directory 域服务的容量规划](https://go.microsoft.com/fwlink/?LinkId=324566)。

## <a name="verify-ldap-queries"></a>验证 LDAP 查询

验证 LDAP 查询是否符合创建高效查询的建议。

有关如何正确编写、构建和分析用于 Active Directory 的查询的详细信息，请参阅 MSDN。 有关详细信息，请参阅[创建更有效的支持 Microsoft Active Directory 的应用程序](https://msdn.microsoft.com/library/ms808539.aspx)。

## <a name="optimize-ldap-page-sizes"></a>优化 LDAP 页面大小

当返回包含多个对象的结果以响应客户端请求时，域控制器必须在内存中临时存储结果集。 增加页大小将导致更多的内存使用量，并可能不必要地将项排除在缓存之外。 在这种情况下，默认设置是最佳的。 在某些情况下，建议增加页面大小设置。 建议使用默认值，除非特别标识为 "不充分"。

当查询具有多个结果时，可能会遇到同时执行的类似查询的限制。  出现这种情况的原因是 LDAP 服务器可能会耗尽称为 cookie 池的全局内存区域。  可能需要增加池的大小，如[处理 LDAP 服务器 cookie 的方式](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled)中所述。

若要调整这些设置，请参阅[Windows Server 2008 和更新的域控制器在 LDAP 响应中仅返回5000值](https://support.microsoft.com/kb/2009267)。

## <a name="determine-whether-to-add-indices"></a>确定是否添加索引

在筛选器中搜索具有属性名称的对象时，索引属性非常有用。 索引可以减少评估筛选器时必须访问的对象数。 但是，这会降低写入操作的性能，因为在修改或添加相应的属性时必须更新索引。 它还增加了目录数据库的大小，但优点通常超过了存储成本。 日志记录可用于查找成本高昂且效率低下的查询。 确定之后，请考虑为在相应的查询中使用的某些属性创建索引，以提高搜索性能。 有关 Active Directory 搜索工作原理的详细信息，请参阅[Active Directory 搜索工作](https://technet.microsoft.com/library/cc755809.aspx)原理。

### <a name="scenarios-that-benefit-in-adding-indices"></a>添加索引的好处方案

-   请求数据的客户端负载会生成大量 CPU 使用率，并且不能更改或优化客户端查询行为。 按重大负载，请考虑在服务器性能顾问或内置 Active Directory 数据收集器集的最高10个罪犯列表中显示自身，并且使用超过 1% 的 CPU。

-   由于未索引的属性，客户端负载正在服务器上生成大量磁盘 i/o，无法更改或优化客户端查询行为。

-   查询需要花费很长时间，并且不会在可接受的时间范围内完成，因为缺少覆盖索引。

- 具有较长持续时间的大量查询导致 ATQ LDAP 线程消耗和耗尽。 监视以下性能计数器：

    - **NTDS @ no__t-1Request 延迟**–这取决于请求处理所需的时间。 Active Directory 在120秒后超时请求数（默认值），则大多数运行速度应快得多，并且长时间运行的查询应隐藏在总数字中。 查找此基线中的更改，而不是绝对阈值。

        > [!NOTE]
        > 此处较高的值还可以指示对其他域和 CRL 检查的 "代理" 请求中的延迟。

    - **NTDS @ no__t-1Estimated Queue Delay** –理想情况下，理想情况下应接近0以获得最佳性能，因为这意味着请求不会花费时间等待维护。

可以使用以下一种或多种方法来检测这些方案：

-   [用 Statistics 控件确定查询计时](https://msdn.microsoft.com/library/ms808539.aspx)

-   [跟踪代价高昂且低效的搜索](https://msdn.microsoft.com/library/ms808539.aspx)

-   在性能监视器中 Active Directory 诊断数据收集器集（@no__t-SPA 的0Son：AD 数据收集器集在 Win2008 中以及 @ no__t 之外

-   [Microsoft Server 性能顾问](../../../server-performance-advisor/microsoft-server-performance-advisor.md)Active Directory Advisor 包

-   使用 "（objectClass = \*）" 以外的任何筛选器进行搜索，该筛选器使用祖先索引。

### <a name="other-index-considerations"></a>其他索引注意事项

-   请确保在优化查询已用完选项后，创建索引是解决问题的正确解决方案。 正确调整硬件大小非常重要。 仅当正确的修复是为属性编制索引，而不是尝试模糊处理硬件问题时，才应添加索引。

-   索引将增加数据库的大小，最小值为索引的属性的总大小。 因此，可以通过在属性中获取数据的平均大小并乘以将填充属性的对象数来计算数据库增长的估计值。 通常，这是数据库大小增加 1% 的情况。 有关详细信息，请参阅[数据存储的工作方式](https://technet.microsoft.com/library/cc772829.aspx)。

-   如果搜索行为主要在组织单位级别完成，则考虑为容器化搜索编制索引。

-   元组索引比普通索引大，但估计大小要困难得多。 使用常规索引大小估算作为增长的地面，最大值为 20%。 有关详细信息，请参阅[数据存储的工作方式](https://technet.microsoft.com/library/cc772829.aspx)。

-   如果搜索行为主要在组织单位级别完成，则考虑为容器化搜索编制索引。

-   需要元组索引以支持词中搜索字符串和最终搜索字符串。 初始搜索字符串不需要元组索引。

    -   初始搜索字符串–（samAccountName = MYPC @ no__t-0）

    -   词中搜索字符串-（samAccountName = \*MYPC @ no__t）

    -   最终搜索字符串–（samAccountName = \*MYPC $）

-   创建索引时，将在生成索引时生成磁盘 i/o。 这是在优先级较低的后台线程上完成的，传入的请求优先于索引生成。 如果已正确执行环境的容量规划，则这应该是透明的。 然而，编写繁重的方案或域控制器存储上的负载未知的环境可能会降低客户端体验，并应在工作时间结束。

-   由于在本地生成索引，对复制流量的影响最小。

有关详细信息，请参阅以下内容：

-   [创建更有效的 Microsoft Active Directory 应用程序](https://msdn.microsoft.com/library/ms808539.aspx)

-   [在 Active Directory 域服务中搜索](https://msdn.microsoft.com/library/aa746427.aspx)

-   [索引属性](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)

## <a name="see-also"></a>请参阅

- [性能优化 Active Directory 服务器](index.md)
- [硬件注意事项](hardware-considerations.md)
- [域控制器的正确放置和站点注意事项](site-definition-considerations.md)
- [ADDS 性能疑难解答](troubleshoot.md) 
- [Active Directory 域服务的容量计划](https://go.microsoft.com/fwlink/?LinkId=324566)