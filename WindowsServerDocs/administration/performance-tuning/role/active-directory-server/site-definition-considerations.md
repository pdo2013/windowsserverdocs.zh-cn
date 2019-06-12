---
title: 站点定义和域控制器放置在 ADDS 性能优化
description: 站点定义和域控制器放置注意事项在 Active Directory 性能优化。
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9861703e5ae88dcaec5e76d9fab426b928d0cb9a
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811496"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>正确放置域控制器和站点的注意事项

正确的站点定义对性能至关重要。 从站点的客户端可能会遇到的身份验证和查询性能不佳。 此外，客户端上引入 IPv6 后，请求可以来自 IPv4 或 IPv6 地址和 Active Directory 需要具有 IPv6 的正确定义的站点。 操作系统首选 IPv6 到 IPv4 时二者都已配置。

从 Windows Server 2008 开始，尝试使用名称解析以便确定站点客户端执行反向查找域控制器应为。 这可能会导致 ATQ 线程池耗尽，并导致域控制器没有响应。 到此相应的解决方法是正确的 IPv6 定义站点拓扑。 作为一种解决方法，其中一个可以优化用于快速响应的域控制器请求的名称解析基础结构。 有关详细信息请参阅[Windows Server 2008 或 Windows Server 2008 R2 域控制器延迟响应 LDAP 或 Kerberos 请求](https://support.microsoft.com/kb/2668820)。

另一个区域的考虑因素查找读/写 Dc 的 Rodc 都在使用的方案。  某些操作需要访问可写域控制器，或可写的域控制器为只读域控制器都能满足需求时为目标。  优化这些方案应采用两个路径：
-   只读域控制器都能满足需求时，请联系可写域控制器。  这要求应用程序代码更改。
-   其中可写域控制器可能有必要。  将读写域控制器放在中心位置以尽量降低延迟。

有关进一步信息参考：
-   [应用程序与 Rodc 的兼容性](https://technet.microsoft.com/library/cc772597.aspx)
-   [Active Directory 服务接口 (ADSI) 和读取仅域控制器 (RODC) – 避免性能问题](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>优化的引用

引用是分区的如何 LDAP 查询将重定向时的域控制器未承载查询的副本。 返回一个引用后，它包含分区的可分辨的名称、 DNS 名称和端口号。 客户端使用此信息在承载该分区的服务器上继续查询。 这是 dc 定位程序方案和推荐的所有站点定义和维护域控制器放置，但依赖于引用的应用程序经常被忽视。 建议以确保 AD 拓扑包括网站定义和域控制器放置正确反映了客户端的需求。 此外，这可能包括优化 DNS 设置，或重新定位应用程序的站点的单个站点中的多个域中具有域控制器。

## <a name="optimization-considerations-for-trusts"></a>有关信任关系的优化注意事项

在林内方案中，信任关系的处理根据以下域层次结构：Grand 子域-&gt;子域-&gt;林根级域-&gt;子域-&gt; Grand 子域。 安全通道在林根和每个父级，这意味着可能会超负荷由于传输信任层次结构中的域控制器的身份验证请求的聚合。 当身份验证还必须传输很高的延迟会影响上述数据流的链接时，这也可能产生较大的地理分布的活动目录中的延迟。 在林间和低级别的信任方案中可以进行重载。 以下建议适用于所有方案：

-   正确调整 MaxConcurrentAPI 以通过安全通道支持的负载。 有关详细信息，请参阅[如何执行性能调整为 NTLM 身份验证使用 MaxConcurrentApi 设置](https://support.microsoft.com/kb/2688798/EN-US)。

-   创建相应的快捷方式信任在加载基于。

-   确保域中的每个域控制器能够执行名称解析，并与受信任域中的域控制器进行通信。

-   请确保区域注意事项纳入信任的帐户。

-   在可能的情况启用 Kerberos 并且尽可能少地使用安全通道以降低风险，消除了 MaxConcurrentAPI 瓶颈。

跨域信任的方案是已被一致地对许多客户痛点的区域。 名称解析和连接性问题，通常是由于防火墙，导致资源耗尽信任的域控制器上，并影响所有客户端。 此外，一个经常被忽视的方案优化对受信任的域控制器的访问。 若要确保这能正常工作的关键领域如下所示：

-   确保信任的域控制器使用的 DNS 和 WINS 名称解析可解析成的受信任域的域控制器的准确列表。

    -   以静态方式添加的记录具有一种趋势会变得陈旧和随着时间的推移中重新引入连接问题。 DNS 转发，动态 DNS 和合并 WINS/DNS 基础结构是从长远来看更易于维护。

    -   请确保正确配置转发器、 条件性转发和客户端可能需要访问的环境中的每个资源这两个正向和反向查找区域的辅助副本。 同样，这就要求手动维护，很可能会变得陈旧。 合并的基础结构是理想之选。

-   信任域中的域控制器将尝试查找域控制器在第一次是在同一站点中的受信任的域和故障回复到泛型定位符。

    -   Dc 定位程序的工作原理的详细信息，请参阅[在最近的站点中查找域控制器](https://technet.microsoft.com/library/cc978016.aspx)。

    -   聚合之间的受信任和信任的域以反映在同一位置中的域控制器的站点名称。 请确保映射正确地链接到这两个林中的站点的子网和 IP 地址。 有关详细信息，请参阅[域定位器跨林信任](http://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx)。

    -   请确保端口处于打开状态，根据 dc 定位程序的需要，为域控制器位置。 如果域之间存在防火墙，请确保为所有信任都正确配置防火墙。 如果不打开防火墙，信任的域控制器仍会尝试访问受信任的域。 如果出于任何原因失败通信，则信任的域控制器将最终超时对受信任的域控制器的请求。 但是，这些超时需要几秒钟，每个请求，并且可以用完可用抵信任的域控制器上的网络端口当传入请求的量过高。 客户端可能会遇到到的域控制器上的超时等待作为挂起的线程，可以将转换为挂起的应用程序 （如果应用程序在前台线程中运行该请求）。 有关详细信息，请参阅[如何为域和信任关系配置防火墙](https://support.microsoft.com/kb/179442)。

    -   使用 DnsAvoidRegisterRecords 消除效果不佳性能或高延迟的域控制器，如附属站点，从泛型定位符的广告中。 有关详细信息，请参阅[如何优化域控制器或全局编录，位于客户端的站点之外的位置](https://support.microsoft.com/kb/306602)。

        > [!NOTE]
        > 没有为客户端可以使用的域控制器的数量大约为 50 的实际限制。 这些应是最大站点最佳和最高容量的域控制器。

    
    -  请考虑将域控制器放置在同一物理位置中的受信任及信任域中。

对于所有信任方案，凭据将被路由根据身份验证请求中指定的域。 这也是如此的查询的 LookupAccountName 和 LsaLookupNames （以及其他人，这些只是最常用） Api。 这些 Api 的域参数传递 NULL 值，当域控制器将尝试查找每个受信任的域中指定的帐户名称。

-   禁用指定 NULL 域时检查可用的所有信任。 [如何通过使用 LsaLookupRestrictIsolatedNameLevel 注册表项来限制的独立外部受信任域中的名称查找](https://support.microsoft.com/kb/818024)

-   禁用与 NULL 域指定跨所有可用信任传递身份验证请求。 [Lsass.exe 进程可能会停止响应，如果在 Active Directory 域控制器上有许多外部信任](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>请参阅
- [性能优化 Active Directory 服务器](index.md)
- [硬件注意事项](hardware-considerations.md)
- [LDAP 注意事项](ldap-considerations.md)
- [ADDS 性能疑难解答](troubleshoot.md) 
- [Active Directory 域服务的容量计划](https://go.microsoft.com/fwlink/?LinkId=324566)