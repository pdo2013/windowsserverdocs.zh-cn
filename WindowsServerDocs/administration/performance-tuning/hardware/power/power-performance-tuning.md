---
title: 能力和性能优化
description: 处理器电源管理 (PPM) 优化 Windows Server 平衡的电源计划
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 4ad58e9b477f61844dedd9f6638efb12f1a96500
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811564"
---
# <a name="power-and-performance-tuning"></a>电源和性能优化

能源效率是企业版和数据中心环境中越来越重要，它将另一组权衡添加到配置选项的组合。

Windows Server 2016 是之间对范围广泛的客户工作负荷优化的极好的能源效率的最小性能影响。 [有关 Windows Server 平衡电源计划的处理器电源管理 (PPM) 优化](processor-power-management-tuning.md)介绍用于优化 Windows Server 2016 中的默认参数的工作负荷，并提供自定义 tunings 的建议。

本部分扩展了能源效率权衡，以便帮助你做出明智的决策，如果您需要调整您的服务器上的默认电源设置。 但是，大多数服务器硬件和工作负荷应该不需要管理员电源优化运行 Windows Server 2016 时。

## <a name="calculating-server-energy-efficiency"></a>计算服务器能源效率

在优化你的服务器用于能源节省时, 还必须考虑性能。 优化影响性能和电源，有时在不相称的金额中。 对于每个可能的调整，请考虑以确定是否需要权衡的是可接受您电源预算和性能目标。

您可以计算非常有用的指标，其中包含能力和性能信息的服务器的能源效率比率。 能源效率是工作的时间的指定内所需的平均幂执行的比率。

![能源效率公式](../../media/perftune-guide-power-formula.png)

此指标可用于设置的遵守能力和性能之间的权衡的实际目标。 与此相反，跨数据中心的 10%能源节省的目标无法捕获相应影响性能，反之亦然。

同样，如果优化你的服务器以提高性能的 5%，并导致更高版本 10%的能耗的方式，总结果将可能或可能不是可接受的业务目标。 能源效率指标可实现更明智的决策比单独的功能或性能指标。

## <a name="measuring-system-energy-consumption"></a>测量系统能源消耗

调整以提高能源效率服务器之前，应建立基线功率测量。

如果你的服务器具有必要的支持，可以使用 power 计量和预算功能在 Windows Server 2016 中的使用性能监视器查看系统级能源消耗。

一种方法来确定是否在服务器具有计数的支持，并且预算是查看[Windows Server 目录](http://www.windowsservercatalog.com)。 如果服务器模型有资格获得 Windows 硬件认证计划中新的增强型电源管理限定条件，它可确保以支持计量和预算方面的功能。

检查计数的支持的另一种方法是手动查找的性能监视器中的计数器。 打开性能监视器中，选择**添加计数器**，然后找到**电表**计数器组。

如果电表的命名实例显示在标记为框中**实例的所选对象**、 您计数的平台支持。 **电源**以瓦为单位显示电源的计数器将出现在所选的计数器组。 未指定的电源数据值的确切派生。 例如，它可能是即时 power 绘图或在某些时间间隔内平均功耗。

如果你的服务器平台不支持计数，可以使用连接到电源输入的物理计数设备来测量系统电源绘制或能源消耗。

若要建立一个基准，应在各种系统负载点，从空闲状态到 100%（最大吞吐量） 来生成负载行所需的平均电源进行测量。 下图显示了三个示例配置的负载行：

![示例负载行](../../media/perftune-guide-sample-loadlines.png)

负载行可用于评估和比较的性能和能源消耗的配置在所有负载点。 在此特定示例中，很容易看到最佳配置。 但是，可以轻松地有一种配置适合大量工作负荷并且其中一个最适合于轻型工作负荷的情况。

您需要充分了解您的工作负荷需求选择最佳配置。 不要假定，找到正确的配置后，它将始终保持最佳。 应定期和工作负载、 工作负荷级别或服务器硬件发生更改后系统利用率和能耗情况进行测量。

## <a name="diagnosing-energy-efficiency-issues"></a>诊断能源效率问题

**PowerCfg.exe**支持可用于分析你的服务器的空闲能源效率的命令行选项。 当你运行使用 PowerCfg.exe **/energy**选项时，此工具将执行一个 60 秒测试，以检测潜在能源效率问题。 该工具在当前目录中生成一个简单的 HTML 报表。

> [!Important]
> 若要确保准确分析，请确保在运行之前关闭所有本地应用**PowerCfg.exe**。 

缩短计时器刻度线费率，缺少电源管理支持，以及 CPU 使用率过高所检测到的行为问题的一些驱动程序**powercfg /energy**命令。 此工具提供了简单的方法，以识别并修复电源管理问题，可能会导致在大型数据中心中的显著的成本节约。

PowerCfg.exe 的详细信息，请参阅[使用 PowerCfg 评估系统能效](https://msdn.microsoft.com/windows/hardware/gg463250.aspx)。

## <a name="using-power-plans-in-windows-server"></a>使用 Windows Server 中的电源计划

Windows Server 2016 具有为满足不同的业务需求而设计的三种内置电源计划。 这些计划提供了使您可以自定义服务器轻松地以满足功能或性能目标。 下表介绍了计划，列出了要在其中使用每个计划的常见方案并为每个计划提供一些实现细节。

| **规划** | **说明** | **常见的适用方案** | **实现要点** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 均衡 （推荐） | 默认设置。 目标很好的能源效率的最小性能影响。 | 常规计算 | 与容量需求相匹配。 节能功能平衡能力和性能。 |
| 高性能 | 提高性能，但高能量消耗。 电源和散热限制，运营费用和可靠性的注意事项适用。 | 低延迟的应用和的处理器性能更改比较敏感的应用程序代码 | 处理器始终在最高的性能状态 （包括"turbo"频率），将其锁定。 所有核心都是 unparked。 热感输出可能很大。 |
| 节能程序 | 限制性能，以节约能源并降低运营成本。 不建议这样做没有彻底的测试，将确保性能已足够。 | 与有限的电源预算和散热约束部署 | 指针顶端才处理器的频率在达到的最大值 （如果支持），并允许其他节能功能。 |


交流电 (AC) 在 Windows 中存在这些电源计划和直流 (DC) 提供支持的系统，但我们将假定服务器始终使用交流电源。

有关电源计划和电源策略配置的详细信息，请参阅[电源策略配置和部署在 Windows 中的](https://msdn.microsoft.com/windows/hardware/gg463243.aspx)。

> [!Note]
> 某些服务器制造商必须通过 BIOS 设置提供其自己电源管理选项。 如果操作系统不具有对电源管理的控制，更改在 Windows 中的电源计划不会影响系统功能和性能。

## <a name="tuning-processor-power-management-parameters"></a>优化处理器电源管理参数

每个电源计划表示大量基础的电源管理参数的组合。 内置的计划是涵盖各种工作负荷和方案的推荐设置的三个集合。 但是，我们认为这些计划将不满足每个客户的需求。

以下部分介绍如何调整以满足目标不由三种内置计划解决一些特定的处理器电源管理参数。 如果您需要了解更广泛的 power 参数，请参阅[电源策略配置和部署在 Windows 中的](https://msdn.microsoft.com/windows/hardware/gg463243.aspx)。

## <a name="processor-performance-boost-mode"></a>处理器性能提升模式下

Intel Turbo Boost 和 AMD Turbo 核心技术是允许处理器以实现额外的性能时最有用的功能 （也就是说，在高系统加载中）。 但是，此功能会增加 CPU 核心能源消耗，因此 Windows Server 2016 配置 Turbo 技术根据使用和特定处理器实现中的电源策略。

Turbo 启用的所有 Intel 和 AMD 处理器上的高性能电源计划，它已禁用的节能程序电源计划。 对于依赖于传统的基于 P 状态的频率管理的系统上平衡的电源计划，Turbo 才会启用默认情况下该平台支持的 EPB 寄存器。

> [!Note]
> EPB 注册仅支持 Intel Westmere 和更高版本的处理器。

以及 Intel Nehalem 和 AMD 处理器、 Turbo P 基于状态的平台上的默认情况下禁用。 但是，如果系统支持协作处理器性能控件 (CPPC)，这是操作系统和硬件 （在 ACPI 5.0 中定义） 之间的性能通信的新备用模式，Turbo 可能会触发如果 Windows 操作系统系统动态请求的硬件来提供最高性能级别。

若要启用或禁用智能加速功能，必须由管理员或所选的电源计划的默认参数设置配置处理器性能提升模式参数。 处理器性能提升模式下有五个允许的值，如表 5 中所示。

对于基于 P 状态的控件，这些选项将被禁用，已启用 （Turbo 是可用于硬件请求名义性能时），并高效 （Turbo 是实现 EPB 注册时才可用）。

CPPC 基于控件的选项将被禁用，高效启用 （Windows 指定的确切 Turbo 提供量），和主动 (Windows 会询问有关"最大性能"来启用 Turbo)。

Windows Server 2016 中，在提升模式下的默认值为 3。

| **名称** | **基于 P 状态的行为** | **CPPC 行为** |
|--------------------------|------------------------|-------------------|
| 0 （禁用） | Disabled | Disabled |
| 1 （已启用） | Enabled | 有效启用 |
| 2 （主动） | Enabled | 积极 |
| 3 （高效已启用） | 高效 | 有效启用 |
| 4 (高效积极) | 高效 | 积极 |

 
以下命令启用处理器性能提升模式上的当前电源计划 （通过使用 GUID 别名指定的策略）：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> 您必须运行**powercfg setactive**命令以启用新的设置。 不需要重新启动服务器。

若要设置此值为当前所选计划之外的电源计划，可以使用如方案的别名\_最大 （节能）、 方案\_最小值 （高性能） 和方案\_平衡 （平衡） 代替方案\_当前。 将为"当前"中方案 powercfg-setactive 命令前面所示使用所需的别名来启用该电源计划。

例如，提升模式下调整节能程序计划中并使该节能程序是当前的计划，运行以下命令：

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>最小值和最大处理器性能状态

处理器更改性能状态 （P 状态） 之间非常快速地为匹配供应请求、 在必要时可以实现性能和节约了能源在可能的情况。 如果你的服务器具有特定的高性能或最小功率消耗要求，则可能会考虑配置**最小处理器性能状态**参数或**最大处理器性能状态**参数。

值**最小处理器性能状态**并**最大处理器性能状态**参数表示为最大处理器的频率，范围 0 – 中值的百分比100。

如果你的服务器需要超低的延迟、 固定 CPU 频率 （例如，用于可重复测试） 或最高的性能级别，您可能不希望切换到更低的性能状态的处理器。 对于此类的服务器，可以限制为 100%的最小处理器性能状态，通过使用以下命令：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

如果你的服务器要求较低的能耗，可能想要限制在最大值的百分比的处理器性能状态。 例如，可以限制对其最大频率的 75%处理器通过使用以下命令：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> 在达到的最大处理器性能将达到上限需要处理器的支持。 检查处理器文档以确定是否存在这种支持，或查看性能监视器计数器 **%的最大频率**中**处理器**组以查看是否所有频率盖应用。

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>处理器性能增加和减少的阈值和策略

由多个参数控制的速度的处理器性能状态增加或减少。 以下四个参数具有的最明显的影响：

-   **处理器性能增加阈值**定义使用率值，上面这将增加处理器的性能状态。 较大的值会降低速度提高了性能状态，以提高活动响应率。

-   **处理器性能降低阈值**定义使用率值，下面这将减少一个处理器性能状态。 较大的值增加的在空闲时段减少性能状态的速率。

-   **处理器性能提高策略和处理器性能降低**策略确定发生变化时，应设置的性能状态。 "单个"策略意味着它会选择下一个状态。 "火箭"意味着在最大或最小电源性能状态。 "理想"尝试查找能力和性能之间的平衡。

例如，如果你的服务器仍想要受益于低能耗空闲期间时需要超低的延迟，无法 quicken 任何增加的负载的性能状态增加和负载降低时降低此下降。 以下命令增加将策略设置为"火箭"速度更快的状态增加，并设置为"Single"减少策略。 增加和减少阈值分别设置为 10，8。

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>处理器性能核心驻最大和最小内核数

核心停车是在 Windows Server 2008 R2 中引入的功能。 处理器电源管理 (PPM) 引擎和计划程序协同工作来动态调整的可运行线程的核心数。 PPM 引擎会选择最小核心，以将计划的线程数。

通常托管的内核并没有计划，任何线程，它们将非常低的电源状态时进入他们未处理中断、 dpc 进行标记或其他严格关联的工作。 剩余内核数负责工作负荷的其余部分。 核心停车可以在使用较低的情况有可能提高能源效率。

对于大多数服务器，默认核心停车行为提供合理的平衡的吞吐量和能源效率。 在其中核心停车可能不会显示为很大的收益泛型工作负荷的处理器中，可以默认情况下禁用它。

如果你的服务器具有特定的核心驻要求，则可以控制可用于通过使用放置的内核数**处理器性能核心停置最大内核数**参数或**处理器性能核心停车最小核心数**Windows Server 2016 中的参数。

核心停车并不总是最适合的一种情况是有一个或多个活动线程关联到 NUMA 节点中不常用的 Cpu 子集 (即多个 CPU，但小于节点上的 Cpu 的整个集)。 核心停车算法选取核心 unpark 时 （假设在工作负荷强度增加发生），不可能会始终选取活动关联的子集 （或子集） 以 unpark 中的内核数并因此可能最终会 unparking 个核心，实际上不会使用过度。

这些参数的值是在范围 0-100 的百分比。 **处理器性能核心停置最大内核数**参数控制可以 unparked （可运行线程） 的核心数的最大百分比在任何时间，而**处理器性能 Core Parking最小内核数**参数控制可以是 unparked 的核心数的最小百分比。 若要关闭核心停车，设置**处理器性能核心停车最小核心数**参数 %到 100%通过使用以下命令：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

若要减少到 50%的最大计数的可计划的核心数，将设置**处理器性能核心停置最大内核数**参数为 50，如下所示：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>处理器性能核心驻实用程序分发

实用程序分发是设计用于提高某些工作负荷的电源效率的 Windows Server 2016 中的算法优化。 它跟踪不可移动的 CPU 活动 （即 Dpc、 中断或严格关联的线程），并预测将来的工作上基于任何可移动工作可以会平均分布到所有 unparked 内核的假设每个处理器。

对于某些处理器的平衡的电源计划默认情况下被启用实用程序分发。 它可以通过降低工作负荷中相当稳定状态的请求的 CPU 频率来减少处理器的电源消耗。 但是，实用程序分发不一定是个不错的算法选择可能会有所高活动喷发情况的工作负荷或其中工作负荷快速并随机地转移跨处理器的程序。

对于此类工作负荷，我们建议使用以下命令禁用实用工具分发：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>请参阅
- [服务器硬件的性能注意事项](../index.md)
- [服务器硬件电源注意事项](../power.md)
- [处理器电源管理优化](processor-power-management-tuning.md)
- [建议的平衡计划参数](recommended-balanced-plan-parameters.md)
