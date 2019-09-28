---
title: 用于快速响应时间的建议的均衡电源计划参数
description: 用于快速响应时间的建议的均衡电源计划参数
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 96037a577c9f2a835e9c49bf9339ed8dc6da1a6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383511"
---
# <a name="recommended-balanced-power-plan-parameters-for-workloads-requiring-quick-response-times"></a>针对需要快速响应时间的工作负荷推荐的均衡电源计划参数

默认的**均衡**电源计划使用**吞吐量**作为优化的性能指标。 在稳定状态下，**吞吐量**在系统完全过载（约 100% 的利用率）之前不会发生变化。  因此，**平衡**电源计划的能力相当高，同时最大限度地减少了处理器频率和最大利用率。

但是，利用率增加**可能会呈**指数级增长。 如今，快速响应时间的要求已大幅增加。 即使 Microsoft 建议用户在需要快速响应时间的情况下切换到**高性能**电源计划，某些用户也不希望在光速到中等负载级别的情况下获得强大的功能。 因此，对于需要快速响应时间的工作负荷，Microsoft 提供了以下一组建议的参数更改。


| 参数 | 描述 | Default Value | 建议值 |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 处理器性能增加阈值 | 要增加的频率的利用率阈值 | 90 | 60 |
| 处理器性能降低阈值 | 降低频率的利用率阈值 | 80 | 40 |
| 处理器性能增加时间 | 频率要增加之前的 PPM 检查窗口数 | 3 | 1 |
| 处理器性能增加策略 | 频率提高的速度 | Single | 理想时间 |

若要设置建议的值，用户可以在具有管理员的窗口中运行以下命令：

``` syntax
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTHRESHOLD 60
Powercfg -setacvalueindex scheme_balanced sub_processor PERFDECTHRESHOLD 40
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCTIME 1
Powercfg -setacvalueindex scheme_balanced sub_processor PERFINCPOL 0
Powercfg -setactive scheme_balanced
```

此更改基于使用以下工作负荷的性能和功率平衡分析。 对于想要利用特定的 SLA 要求进一步优化能源效率的用户，请参阅[服务器硬件性能注意事项](../power.md)。

>[!Note]
> 有关利用电源计划优化虚拟化工作负荷的其他建议和见解，请阅读[Hyper-v 配置](../../role/hyper-v-server/configuration.md)

## <a name="specpower--java-workload"></a>SPECpower – JAVA 工作负荷

[SPECpower @ no__t-1ssj2008](http://spec.org/power_ssj2008/)是最常用的行业标准规范基准，适用于服务器电源和性能特征，用于检查电源影响。 由于它只使用**吞吐量**作为性能指标，因此默认的**均衡**电源计划可提供最佳的电源效率。

建议的参数更改在光上消耗稍微更高的电量（即 < = 20%）加载级别。 但在负载级别较高的情况下，差别会增加，并且在 60% 负载级别后开始使用与**高性能**电源计划相同的电源。 若要使用建议的更改参数，用户应在其机架容量规划过程中了解大中型负载级别的功率消耗。

## <a name="geekbench-3"></a>GeekBench 3

[GeekBench 3](http://www.geekbench.com/geekbench3/)是一个跨平台处理器基准，用于分隔单核和多核性能的分数。 它模拟一组工作负荷，包括整数工作负荷（加密、压缩、图像处理等）、浮点工作负荷（建模、分形、图像锐化、图像模糊等）以及内存工作负荷（流式处理）。

**响应时间**是其分数计算的主要度量值。 在我们测试的系统中，在单核测试中，默认的**均衡**电源计划的回归约为 18%，而在多核心测试中，与**高性能**电源计划相比，这是大约 40% 的回归。 建议的更改会删除这些回归。

## <a name="diskspd"></a>DiskSpd

[Diskspd](https://en.wikipedia.org/wiki/Diskspd)是一个命令行工具，用于 Microsoft 开发的存储基准测试。 它广泛用于针对存储系统生成各种有关存储性能分析的请求。

我们设置了一个 [故障转移群集]，并使用 Diskspd 生成随机和顺序，并使用不同的 IO 大小对本地和远程存储系统进行读取和写入。 我们的测试表明 IO 响应时间对不同电源计划下的处理器频率非常敏感。 在特定工作负载下，**均衡**电源计划可以从**高性能**电源计划中加倍的响应时间。 建议的更改将删除大多数的回归。

>[!Important]
>从运行 Windows Server 2016 的 Intel [Broadwell] 处理器开始，大多数处理器电源管理决策都是在处理器而不是 OS 级别进行的，以实现更快的工作负荷更改原声。 操作系统使用的旧 PPM 参数对实际频率决策的影响最小，但如果处理器应能达到电源或性能，或者将最小和最大频率限制为，则不会对其进行限制。 因此，建议的 PPM 参数更改仅面向预 Broadwell 系统。

## <a name="see-also"></a>请参阅
- [服务器硬件性能注意事项](../index.md)
- [服务器硬件电源注意事项](../power.md)
- [电源和性能优化](power-performance-tuning.md)
- [处理器电源管理优化](processor-power-management-tuning.md)
- [故障转移群集](https://technet.microsoft.com/library/cc725923.aspx)
