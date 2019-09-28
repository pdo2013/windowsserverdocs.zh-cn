---
title: 中的站点定义和域控制器放置增加了性能优化
description: Active Directory 性能优化中的站点定义和域控制器布局注意事项。
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ba3c9e8792b425fd24d01ab997a5f7c2ac573814
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370248"
---
# <a name="proper-placement-of-domain-controllers-and-site-considerations"></a>正确放置域控制器和站点注意事项

正确的站点定义对性能至关重要。 失去站点的客户端可能会遇到身份验证和查询性能不佳的情况。 此外，通过在客户端上引入 IPv6，请求可以来自 IPv4 或 IPv6 地址，Active Directory 需要为 IPv6 正确定义站点。 如果配置了这两个，则操作系统首选 IPv6。

从 Windows Server 2008 开始，域控制器会尝试使用名称解析来执行反向查找，以确定客户端应处于的站点。 这可能会导致 ATQ 线程池耗尽，并导致域控制器停止响应。 为此，合适的解决方法是正确定义 IPv6 的站点拓扑。 作为一种解决方法，可以对名称解析基础结构进行优化，以便快速响应域控制器请求。 有关详细信息，请参阅[Windows server 2008 或 Windows server 2008 R2 域控制器延迟响应 LDAP 或 Kerberos 请求](https://support.microsoft.com/kb/2668820)。

还有一个需要考虑的其他方面，就是在使用 Rodc 的情况下查找读/写 Dc。  当只读域控制器足以满足特定操作时，某些操作需要访问可写域控制器或针对可写域控制器。  优化这些方案需要两个路径：
-   当只读域控制器足以满足时，请与可写域控制器联系。  这需要更改应用程序代码。
-   可写域控制器可能是必需的。  将读写域控制器置于中心位置，以最大程度地减少延迟。

有关详细信息，请参考：
-   [应用程序与 Rodc 的兼容性](https://technet.microsoft.com/library/cc772597.aspx)
-   [Active Directory Service Interface （ADSI）和只读域控制器（RODC）–避免性能问题](https://blogs.technet.microsoft.com/fieldcoding/2012/06/24/active-directory-service-interface-adsi-and-the-read-only-domain-controller-rodc-avoiding-performance-issues/)

## <a name="optimize-for-referrals"></a>优化推荐

引用是在域控制器不承载所查询分区副本时，如何重定向 LDAP 查询。 返回引用时，它将包含分区的可分辨名称、DNS 名称和端口号。 客户端使用此信息在托管分区的服务器上继续进行查询。 这是一种 Dc 定位程序方案，并保留了所有建议站点定义和域控制器的位置，但依赖于引用的应用程序通常会被忽略。 建议确保 AD 拓扑（包括站点定义和域控制器布局）正确反映客户端的需求。 另外，这可能包括在单个站点中有多个域的域控制器、优化 DNS 设置或重新定位应用程序的站点。

## <a name="optimization-considerations-for-trusts"></a>信任的优化注意事项

在林内的方案中，将根据以下域层次结构处理信任：总计子域-&gt; 子域-@no__t 目录林根域 &gt; 子域-@no__t 总计-子域。 这意味着，林根和每个父项的安全通道可能会因为对信任层次结构中的 Dc 传输的身份验证请求进行聚合而过载。 当身份验证还必须传输高度潜在的链接来影响上述流时，这也可能会导致大型地理分散的 Active Directory 延迟。 在林间和下级信任方案中可能会发生重载。 以下建议适用于所有方案：

-   正确调整 MaxConcurrentAPI 以支持安全通道上的负载。 有关详细信息，请参阅[如何使用 MaxConcurrentApi 设置对 NTLM 身份验证执行性能优化](https://support.microsoft.com/kb/2688798/EN-US)。

-   根据负载的需要创建快捷方式信任。

-   确保域中的每个域控制器都能够执行名称解析，并与受信任域中的域控制器进行通信。

-   请确保考虑了位置考虑的信任。

-   在可能的情况下启用 Kerberos，并尽量减少使用安全通道以降低遇到 MaxConcurrentAPI 瓶颈的风险。

跨域信任方案是一种一直为多个客户提供困难点的区域。 名称解析和连接问题通常是由于防火墙，导致信任域控制器上的资源耗尽并影响所有客户端。 而且，经常被忽略的情况是优化对受信任域控制器的访问。 确保此功能正常工作的关键区域如下所示：

-   确保信任域控制器正在使用的 DNS 和 WINS 名称解析可解析受信任域的域控制器的准确列表。

    -   静态添加的记录可能会在一段时间内变得陈旧并再次出现连接问题。 DNS 转发、动态 DNS 和合并 WINS/DNS 基础结构在长时间运行中更易于维护。

    -   确保在客户端可能需要访问的环境中的每个资源的转发和反向查找区域中，正确配置转发器、条件转发和辅助副本。 同样，这需要手动维护，并且往往会过时。 基础结构的合并非常理想。

-   信任域中的域控制器将首先尝试查找位于同一站点中的受信任域中的域控制器，然后故障回复到一般定位符。

    -   有关 Dc 定位程序工作原理的详细信息，请参阅[在最近的站点中查找域控制器](https://technet.microsoft.com/library/cc978016.aspx)。

    -   聚合可信域和信任域之间的站点名称，以反映同一位置中的域控制器。 确保子网和 IP 地址映射正确地链接到两个林中的站点。 有关详细信息，请参阅[跨林信任的域定位器](http://blogs.technet.com/b/askds/archive/2008/09/24/domain-locator-across-a-forest-trust.aspx)。

    -   确保根据 Dc 定位程序的需要，为域控制器位置打开端口。 如果域之间存在防火墙，请确保为所有信任正确配置防火墙。 如果防火墙未打开，则信任域控制器仍将尝试访问受信任的域。 如果通信由于任何原因而失败，则信任域控制器最终会将请求超时到受信任的域控制器。 但是，对于每个请求，这些超时可能需要几秒钟，并且如果传入请求的数量很高，则可能会耗尽信任域控制器上的网络端口。 客户端可能会在域控制器上经历等待时间作为挂起线程超时，这可能会转换为挂起的应用程序（如果应用程序在前台线程中运行请求）。 有关详细信息，请参阅[如何为域和信任关系配置防火墙](https://support.microsoft.com/kb/179442)。

    -   使用 DnsAvoidRegisterRecords 从广告到一般定位符，以消除工作不良或延迟的域控制器（如卫星站点中的控制器）。 有关详细信息，请参阅[如何优化位于客户端站点之外的域控制器或全局编录的位置](https://support.microsoft.com/kb/306602)。

        > [!NOTE]
        > 对于客户端可以使用的域控制器数量，实际限制为大约50。 它们应该是最适合站点的和最大容量的域控制器。

    
    -  考虑将受信任域和信任域中的域控制器放置在同一个物理位置。

对于所有信任方案，凭据将根据身份验证请求中指定的域进行路由。 这也适用于 LookupAccountName 和 LsaLookupNames 的查询（以及其他参数，它们只是最常用的 Api）。 如果为这些 Api 的域参数传递了 NULL 值，则域控制器将尝试查找每个可用受信任域中指定的帐户名称。

-   如果指定了 NULL 域，则禁用检查所有可用的信任。 [如何使用 LsaLookupRestrictIsolatedNameLevel 注册表项限制外部受信任域中的隔离名称的查找](https://support.microsoft.com/kb/818024)

-   禁用跨所有可用信任传递具有 NULL 域的身份验证请求。 [如果对 Active Directory 域控制器具有许多外部信任，则 Lsass.exe 进程可能会停止响应](https://support.microsoft.com/kb/923241/EN-US)

## <a name="see-also"></a>请参阅
- [性能优化 Active Directory 服务器](index.md)
- [硬件注意事项](hardware-considerations.md)
- [LDAP 注意事项](ldap-considerations.md)
- [ADDS 性能疑难解答](troubleshoot.md) 
- [Active Directory 域服务的容量计划](https://go.microsoft.com/fwlink/?LinkId=324566)