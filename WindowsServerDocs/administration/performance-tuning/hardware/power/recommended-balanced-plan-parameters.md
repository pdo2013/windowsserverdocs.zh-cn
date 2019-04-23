---
title: 建议用于快速响应时间的平衡的电源计划参数
description: 建议用于快速响应时间的平衡的电源计划参数
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 134e868e1400729f754039fc8120cea0c73945bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878788"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>平衡的电源计划参数建议用于工作负荷需要快速响应时间

默认值**平衡**电源计划使用**吞吐量**作为进行优化的性能指标。 在稳定状态下，期间**吞吐量**直到系统完全重载 （~ 100%利用率） 不会更改与不同的利用率。  因此，**平衡**电源计划倾向于性能很多和最大程度减少处理器的频率和最大化利用率。

但是**响应时间**会成倍增加使用率增加。 如今，快速响应时间的要求已大大提高了。 即使 Microsoft 建议以切换到的用户也是如此**高性能**电源计划在需要快速响应时间时，某些用户不希望在 light 过程中丢失 power 权益与中等负载级别。 因此，Microsoft 提供的建议的参数更改以下一组用于需要快速响应时间的工作负荷。


| 参数 | 描述 | 默认值 | 建议的值 |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 处理器性能增加阈值 | 利用率阈值以上的频率是增加 | 90 | 60 |
| 处理器性能降低阈值 | 利用率阈值下它频率将减少 | 80 | 40 |
| 处理器性能增加时间 | PPM 检查 windows 频率可增加前的数 | 3 | 1 |
| 处理器性能增加策略 | 如何快速频率将增加 | 单个 | 理想时间 |

若要设置的建议的值，用户可以运行以下命令在窗口中与管理员：

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

此更改基于性能和使用以下工作负荷 power 权衡分析。 有关用户想要进一步微调优化电源效率特定 SLA 的要求，请参阅[服务器硬件的性能注意事项](../power.md)。

>[!Note]
> 有关其他建议和利用电源计划，以优化虚拟化工作负荷的深入，读取[hyper-v 配置](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower – JAVA 工作负荷

[SPECpower\_ssj2008](http://spec.org/power_ssj2008/)、 最热门行业标准规范基准服务器的能力和性能特征，用于检查 power 影响。 因为它仅使用**吞吐量**性能指标，默认值为**平衡**电源计划提供了最佳的电源效率。

建议的参数更改使用在光线稍有更高的能力 (即，< = 20%)负载级别。 但使用较大负载级别，差异会增加，并开始使用相同**高性能**后的 60%负载级别的电源计划。 若要使用建议的更改参数，用户应在机架容量规划期间在中型到高负载级别能源成本的注意。

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/)是分隔单核和多核性能分数的跨平台处理器基准。 模拟工作负荷包括整数工作负荷 （加密、 压缩文档、 图像处理等）、 （建模，不规则图形、 图像锐化、 图像模糊，等等） 的浮动点工作负载和内存工作负荷 （流式处理） 的一组。

**响应时间**是主要的度量值在分数计算过程中。 在我们测试系统中，默认值**平衡**电源计划在多核测试与比较中的单核测试和大约为 40%回归具有 ~ 18%回归**高性能**电源计划。 建议的更改会删除这些回归。

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd)是用于存储进行基准测试 Microsoft 开发的命令行工具。 它是广泛用于生成各种针对存储性能分析的存储系统的请求。

我们设置 [故障转移群集]，并使用 Diskspd 以生成随机和连续的以及读取和写入到具有不同的 IO 大小的本地和远程存储系统的 Io。 我们的测试表明 IO 响应时间是在不同的电源计划对处理器的频率非常敏感。 **平衡**电源计划无法两倍的响应时间，从**高性能**某些工作负荷下的电源计划。 建议的更改删除大部分的回归测试。

>[!Important]
>从 Intel [Broadwell] 处理器运行 Windows Server 2016 开始，大多数处理器电源管理决策中所做的处理器而不是 OS 级别以实现更快地原声工作负载变化。 OS 使用的旧 PPM 参数对实际频率决策，除告诉处理器是否它应更倾向于功能或性能，或最小和最大频率将达到上限的影响降到最低。 因此，所建议的 PPM 参数更改为仅定目标为 pre Broadwell 系统中。

## <a name="see-also"></a>请参阅
- [服务器硬件的性能注意事项](../index.md)
- [服务器硬件功能的注意事项](../power.md)
- [电源和性能优化](power-performance-tuning.md)
- [处理器电源管理优化](processor-power-management-tuning.md)
- [故障转移群集](https://technet.microsoft.com/library/cc725923.aspx)
