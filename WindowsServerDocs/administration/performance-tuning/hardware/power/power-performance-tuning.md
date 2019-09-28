---
title: 电源和性能优化
description: Windows Server 平衡电源计划的处理器电源管理（PPM）优化
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Qizha;TristanB
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 1fcb1601169fa8f8b45a8a89b6495179092a0adc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370536"
---
# <a name="power-and-performance-tuning"></a>电源和性能优化

能源效率在企业和数据中心环境中日益重要，并为混合配置选项添加了另一组折衷方案。

Windows Server 2016 针对极佳的能源效率进行了优化，对各种客户工作负荷的性能影响降到了最低。 [Windows Server 平衡电源计划的处理器电源管理（PPM）优化](processor-power-management-tuning.md)介绍了用于优化 windows server 2016 中的默认参数的工作负荷，并为自定义的 tunings 提供了建议。

本部分对能源效率权衡进行了扩展，以帮助你在需要调整服务器上的默认电源设置时作出明智的决策。 但是，在运行 Windows Server 2016 时，大多数服务器硬件和工作负荷不应要求管理员电源优化。

## <a name="calculating-server-energy-efficiency"></a>计算服务器能效

优化服务器以节省能源时，还必须考虑性能。 调整会影响性能和能力，有时会导致不相称的数量。 对于每个可能的调整，请考虑功率消耗和性能目标，以确定是否可以接受交易。

可以为包含电源和性能信息的有用指标计算服务器的能源效率比率。 能源效率是指在指定时间内所需的平均功率所完成的工作量。

![能源效率公式](../../media/perftune-guide-power-formula.png)

你可以使用此指标来设置在电源与性能之间权衡的实际目标。 与此相反，跨数据中心节省 10% 能源的目标无法捕获相应的性能影响，反之亦然。

同样，如果你优化服务器以使性能提高 5%，导致能耗增加 10%，则总体结果对于你的业务目标可能会或可能不是可接受的。 能源效率指标使决策比单独提供更明智的决策。

## <a name="measuring-system-energy-consumption"></a>度量系统能耗

优化服务器之前，应建立基线功率度量，以提高能源效率。

如果你的服务器具有所需的支持，则可以使用 Windows Server 2016 中的电源计量和预算功能，通过使用性能监视器来查看系统级能耗。

确定服务器是否支持计量和预算的一种方法是查看[Windows Server 目录](http://www.windowsservercatalog.com)。 如果服务器型号适用于 Windows 硬件认证计划中新的增强型电源管理资格，则保证支持计量和预算功能。

检查计量支持的另一种方法是在 "性能监视器" 中手动查找计数器。 打开性能监视器，选择 "**添加计数器**"，然后找到 "**电源指示器**" 计数器组。

如果 power 计量的命名实例出现在标记为 "**所选对象的实例**" 的框中，则平台支持计量。 显示电源的**功率**计数器将出现在 "所选计数器组" 中。 未指定电源数据值的确切派生。 例如，它可能是一段时间间隔内的即时电源或平均功率消耗。

如果你的服务器平台不支持计数，则可以使用连接到电源输入的物理计量设备来度量系统电源消耗或能耗。

若要建立基线，应测量各种系统加载点（从空闲百分比到 100% （最大吞吐量））所需的平均功率，以生成负载行。 下图显示了三个示例配置的负载行：

![示例加载行](../../media/perftune-guide-sample-loadlines.png)

您可以使用加载行来评估和比较所有负载点上的配置的性能和能源消耗情况。 在此特定示例中，可以轻松查看最佳配置。 但是，在很多情况下，一种配置最适合用于繁重的工作负荷，另一种配置最适合于轻型工作负荷。

你需要全面了解工作负荷要求，才能选择最佳配置。 不要假设在您找到良好的配置后，它将始终保持最佳状态。 你应定期度量系统利用率和能耗，并在工作负荷、工作负荷级别或服务器硬件发生更改后进行。

## <a name="diagnosing-energy-efficiency-issues"></a>诊断能源效率问题

**PowerCfg**支持命令行选项，你可以使用它来分析服务器的空闲能源效率。 当使用 **/energy**选项运行 PowerCfg 时，该工具将执行60秒测试来检测潜在的能源效率问题。 该工具将在当前目录中生成一个简单的 HTML 报告。

> [!Important]
> 若要确保准确的分析，请在运行**PowerCfg**之前确保所有本地应用都已关闭。 

缩短计时器滴答速率、缺乏电源管理支持的驱动程序和过多的 CPU 使用率是**powercfg/energy**命令检测到的几个行为问题。 此工具提供了一种简单的方法来识别和修复电源管理问题，在大型数据中心中可能会显著节省成本。

有关 PowerCfg 的详细信息，请参阅[使用 Powercfg 评估系统能源效率](https://msdn.microsoft.com/windows/hardware/gg463250.aspx)。

## <a name="using-power-plans-in-windows-server"></a>使用 Windows Server 中的电源计划

Windows Server 2016 有三个内置电源计划，旨在满足不同的业务需求。 这些计划为您提供了一种简单的方法，可用于自定义服务器以满足电源或性能目标。 下表介绍了这些计划，并列出了使用每个计划的常见方案，并提供了每个计划的一些实现细节。

| **规划** | **说明** | **常见适用方案** | **实现亮点** |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 平衡（推荐） | 默认设置。 以极小的性能影响提高良好的能源效率。 | 常规计算 | 匹配需求的容量。 节能功能平衡了电源和性能。 |
| 高性能 | 提高性能，代价是消耗高能耗。 电源和散热限制、运营支出和可靠性注意事项。 | 对处理器性能更改敏感的低延迟应用和应用代码 | 处理器始终处于最高性能状态（包括 "turbo" 频率）。 所有内核都已被离开。 温度输出可能很重要。 |
| 节能程序 | 限制性能以节省能源并降低运营成本。 不建议进行全面测试，以确保性能够用。 | 具有有限功率预算和热量约束的部署 | 以最大百分比（如果支持）计算处理器频率，并启用其他节能功能。 |


这些电源计划存在于适用于其他当前（AC）和直接当前（DC）驱动系统的 Windows 中，但我们假定服务器始终使用交流电源。

有关电源计划和电源策略配置的详细信息，请参阅[Windows 中的电源策略配置和部署](https://msdn.microsoft.com/windows/hardware/gg463243.aspx)。

> [!Note]
> 某些服务器制造商通过 BIOS 设置提供自己的电源管理选项。 如果操作系统无法控制电源管理，在 Windows 中更改电源计划不会影响系统功能和性能。

## <a name="tuning-processor-power-management-parameters"></a>优化处理器电源管理参数

每个电源计划代表大量基础电源管理参数的组合。 内置计划是三个建议设置集合，涵盖各种工作负荷和方案。 但是，我们认识到这些计划不能满足每个客户的需求。

以下各节介绍了调整某些特定处理器电源管理参数以满足三个内置计划未解决的目标的方法。 如果需要了解更多的电源参数，请参阅[Windows 中的电源策略配置和部署](https://msdn.microsoft.com/windows/hardware/gg463243.aspx)。

## <a name="processor-performance-boost-mode"></a>处理器性能提升模式

Intel Turbo 提升和 AMD Turbo 核心技术是允许处理器在最有用的情况下（即，在系统负载较高时）实现额外性能的功能。 但是，此功能增加了 CPU 核心能量消耗，因此 Windows Server 2016 基于正在使用的电源策略和特定处理器实现来配置 Turbo 技术。

在所有 Intel 和 AMD 处理器上，为高性能电源计划启用了 Turbo，并为 Power memory 电源计划禁用了该功能。 对于依赖传统的基于 P 状态的频率管理的系统上的均衡电源计划，默认情况下，仅当平台支持 EPB 注册时才启用 Turbo。

> [!Note]
> 仅 Intel Westmere 和更高版本的处理器支持 EPB 注册。

对于 Intel Nehalem 和 AMD 处理器，默认情况下，在基于 P 状态的平台上禁用 Turbo。 但是，如果系统支持协作处理器性能控制（CPPC），这是操作系统与硬件（在 ACPI 5.0 中定义）之间的一种新的替代模式，则在 Windows 操作系统会动态请求硬件，以提供可能的最高性能级别。

若要启用或禁用 Turbo 提升功能，必须由管理员或所选电源计划的默认参数设置来配置处理器性能提升模式参数。 处理器性能提升模式包含5个允许的值，如表5所示。

对于基于 P 状态的控制，已禁用、已启用（Turbo 适用于每次请求名义性能时的硬件）和高效（Turbo 仅在 EPB 收银机实现时可用）。

对于基于 CPPC 的控件，将禁用、启用有效的选项（Windows 指定要提供的 Turbo 的精确数量）以及主动（Windows 要求 "最高性能" 来启用 Turbo）。

在 Windows Server 2016 中，提升模式的默认值为3。

| **名称** | **P 基于状态的行为** | **CPPC 行为** |
|--------------------------|------------------------|-------------------|
| 0（已禁用） | Disabled | Disabled |
| 1（已启用） | Enabled | 高效启用 |
| 2（积极） | Enabled | 紧 |
| 3（有效启用） | 高效 | 高效启用 |
| 4（高效地） | 高效 | 紧 |

 
以下命令对当前电源计划启用处理器性能提升模式（使用 GUID 别名指定策略）：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_current
```

> [!Important]
> 必须运行**setactive**命令来启用新设置。 不需要重新启动服务器。

若要为当前所选计划以外的其他电源计划设置此值，可以使用 "方案\_最大值" （节电模式）、"方案\_最小值" （\_"高性能"）和 "方案平衡（平衡）\_"等别名。当前。 将以前显示的 setactive 命令中的 "方案的当前方案" 替换为所需的别名，以启用该电源计划。

例如，若要在节能模式下调整提升模式并使其成为当前计划，请运行以下命令：

``` syntax
Powercfg -setacvalueindex scheme_max sub_processor PERFBOOSTMODE 1
Powercfg -setactive scheme_max
```

## <a name="minimum-and-maximum-processor-performance-state"></a>最小和最大处理器性能状态

处理器在性能状态（P 状态）之间进行了快速的更改，以满足需求，在必要时提供性能并节约能源。 如果你的服务器具有特定的高性能或最小功耗要求，你可能会考虑配置**最小处理器性能状态**参数或**最大处理器性能状态**参数。

**最小处理器性能状态**和**最大处理器性能状态**参数的值以最大处理器频率的百分比表示，值介于0–100。

如果服务器需要超高延迟、固定 CPU 频率（例如，对于可重复测试）或最高性能级别，则可能不希望处理器切换到较低性能状态。 对于这种服务器，可以使用以下命令将最小处理器性能状态限制为 100%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMIN 100
Powercfg -setactive scheme_current
```

如果你的服务器需要较低的能耗，你可能想要以最大的百分比来计算处理器性能状态。 例如，可以使用以下命令将处理器限制为其最大频率的 75%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor PROCTHROTTLEMAX 75
Powercfg -setactive scheme_current
```

> [!Note]
> 以最大百分比覆盖处理器性能需要处理器支持。 请查看处理器文档，确定是否存在此类支持，或查看**处理器**组中的性能监视器计数器 **% 的最大频率**，查看是否应用了任何频率上限。

## <a name="processor-performance-increase-and-decrease-of-thresholds-and-policies"></a>处理器性能增加和降低阈值和策略

处理器性能状态增加或降低的速度由多个参数控制。 以下四个参数的影响最明显：

-   **处理器性能增加阈值**定义一个利用率值，超过此值将会增加处理器的性能状态。 较大的值会降低性能状态的增加速度，以响应增加的活动。

-   "**处理器性能降低阈值**" 定义降低处理器性能状态的使用率值。 较大的值会增加空闲期间性能状态的降低率。

-   **处理器性能增加策略和处理器性能降低**策略确定发生更改时应设置的性能状态。 "单一" 策略意味着它选择下一个状态。 "火箭" 表示最大或最小电源性能状态。 "理想" 尝试在电源和性能之间找到平衡点。

例如，如果你的服务器需要超高延迟，但仍希望在空闲期间从低功耗中获益，则你可以 quicken 负载增加时的性能状态增加，并在负载下降时降低降低。 以下命令将增加策略设置为 "火箭" 以提高状态，并将 "降低" 策略设置为 "单一"。 增大和减小阈值分别设置为10和8。

``` syntax
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCPOL 2
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECPOL 1
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFINCTHRESHOLD 10
Powercfg.exe -setacvalueindex scheme_current sub_processor PERFDECTHRESHOLD 8
Powercfg.exe /setactive scheme_current
```

## <a name="processor-performance-core-parking-maximum-and-minimum-cores"></a>处理器性能核心休止最大和最小内核数

核心停车是在 Windows Server 2008 R2 中引入的一项功能。 处理器电源管理（PPM）引擎和计划程序一起工作以动态调整可用于运行线程的内核数。 PPM 引擎为将要计划的线程选择最小内核数。

通常情况下，暂停的内核不会安排任何线程，并且在处理中断、Dpc 或其他严格关联的工作时，它们将降到非常低的电源状态。 其余内核负责工作负荷的其余部分。 在使用率较低的情况下，核心停车可能会提高能源效率。

对于大多数服务器，默认的核心驻留行为提供合理的吞吐量和能源效率平衡。 在核心放置可能并不会对一般工作负荷显示更多优点的处理器上，默认情况下可以禁用它。

如果你的服务器具有特定的核心休止要求，你可以通过使用**处理器性能核心休止最大**核心参数或**处理器性能核心休止最小内核，来控制可用于寄存的内核数**Windows Server 2016 中的参数。

核心停车场并非始终适合的一种情况是：有一个或多个活动线程关联到 NUMA 节点中的一小部分 Cpu （即大于1个 CPU，但低于节点上的整个 Cpu 集）。 当核心休止算法将核心提取到 unpark （假设发生了工作负荷强度增加）时，它可能不会始终选取活动关联子集（或多个子集）中的核心来进行 unpark，因此可能最终不会出现 unparking 的核心超载.

这些参数的值是介于0–100范围内的百分比。 **处理器性能核心 "停车最大核心**数" 参数控制可随时离开的内核（可用于运行线程）的最大百分比，以及**处理器性能核心停车最小内核**参数控制可离开的内核的最小百分比。 若要关闭核心停车，请使用以下命令将**处理器性能核心休止最小核心**参数设置为 100%：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMINCORES 100
Powercfg -setactive scheme_current
```

若要将可计划内核数降低到最大计数的 50%，请将**处理器性能核心休止最大核心**参数设置为50，如下所示：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor CPMAXCORES 50
Powercfg -setactive scheme_current
```

## <a name="processor-performance-core-parking-utility-distribution"></a>处理器性能核心休止实用程序分布

实用工具分发是 Windows Server 2016 中的一种算法优化，旨在提高某些工作负荷的能源效率。 它跟踪不可移动 CPU 活动（即 Dpc、中断或严格关联的线程），并根据假设可以在所有已开始的内核之间平均分布所有可移动工作，预测每个处理器上的未来工作。

默认情况下，为某些处理器的均衡电源计划启用了实用工具分发。 它可以通过降低处于合理稳定状态的工作负荷的所需 CPU 频率来减少处理器功耗。 但是，对于受高活动突发情况的工作负荷或工作负荷在多个处理器之间快速而随机变化的程序，实用工具分发不一定是良好的算法选择。

对于此类工作负荷，建议使用以下命令禁用实用工具分发：

``` syntax
Powercfg -setacvalueindex scheme_current sub_processor DISTRIBUTEUTIL 0
Powercfg -setactive scheme_current
```

## <a name="see-also"></a>请参阅
- [服务器硬件性能注意事项](../index.md)
- [服务器硬件电源注意事项](../power.md)
- [处理器电源管理优化](processor-power-management-tuning.md)
- [建议的平衡计划参数](recommended-balanced-plan-parameters.md)
