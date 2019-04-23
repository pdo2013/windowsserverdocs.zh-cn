---
ms.assetid: 01c8cece-66ce-4a83-a81e-aa6cc98e51fc
title: 高级重复数据删除设置
ms.prod: windows-server-threshold
ms.technology: storage-deduplication
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 15cfc054810a2cab85aae9a04d6195c3ae6fe0b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861208"
---
# <a name="advanced-data-deduplication-settings"></a>高级重复数据删除设置

> 适用于 Windows Server（半年频道）、Windows Server 2016

本文档介绍如何修改高级[重复数据删除](overview.md)设置。 对于[建议的工作负荷](install-enable.md#enable-dedup-candidate-workloads)，需具有足够的默认设置。 修改这些设置的主要原因是提升其他类型的工作负荷的重复数据删除性能。

## <a id="modifying-job-schedules"></a>修改重复数据删除作业计划
[默认重复数据删除作业计划](understand.md#job-info)设计为可以在建议的工作负荷中很好地工作并尽可能不产生干扰（不包括为[**备份**使用类型](understand.md#usage-type-backup)启用的*优先级优化*作业）。 当工作负荷具有较大资源需求时，则可能确保作业仅在空闲时间运行，或者减少或增加允许重复数据删除作业使用的系统资源量。

### <a id="modifying-job-schedules-change-schedule"></a>更改重复数据删除计划
重复数据删除作业通过 Windows 任务计划程序进行计划，且可以在路径 Microsoft\Windows\Deduplication 下查看并编辑。 重复数据删除包括可轻松执行计划的几个 cmdlet。
* [`Get-DedupSchedule`](https://technet.microsoft.com/library/hh848446.aspx) 显示当前的计划的作业。
* [`New-DedupSchedule`](https://technet.microsoft.com/library/hh848445.aspx) 创建新的计划的作业。
* [`Set-DedupSchedule`](https://technet.microsoft.com/library/hh848447.aspx) 修改现有的计划的作业。
* [`Remove-DedupSchedule`](https://technet.microsoft.com/library/hh848451.aspx) 删除计划的作业。

在重复数据删除作业运行时对其进行更改的最常见的原因是确保作业在空闲时间运行。 以下分步示例演示如何为*晴天*方案（在周末和工作日晚上 7:00 以后空闲的超聚合 Hyper-V 主机）修改重复数据删除计划。 若要更改计划，在管理员上下文中运行以下 PowerShell cmdlet。

1. 禁用计划的每小时[优化](understand.md#job-info-optimization)作业。  
    ```PowerShell
    Set-DedupSchedule -Name BackgroundOptimization -Enabled $false
    Set-DedupSchedule -Name PriorityOptimization -Enabled $false
    ```

2. 删除当前计划的[垃圾回收](understand.md#job-info-gc)和[完整性清理](understand.md#job-info-scrubbing)作业。
    ```PowerShell
    Get-DedupSchedule -Type GarbageCollection | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    Get-DedupSchedule -Type Scrubbing | ForEach-Object { Remove-DedupSchedule -InputObject $_ }
    ```

3. 使用高优先级以及系统上所有可用的 CPU 和内存创建在网上 7:00 运行的夜间优化作业。
    ```PowerShell
    New-DedupSchedule -Name "NightlyOptimization" -Type Optimization -DurationHours 11 -Memory 100 -Cores 100 -Priority High -Days @(1,2,3,4,5) -Start (Get-Date "2016-08-08 19:00:00")
    ```

    >[!NOTE]  
    > 为 `-Start` 提供的 `System.Datetime` 的*日期*部分不相关（只要它处于过去），但*时间*部分指定作业应该启动的时间。
4. 使用高优先级以及系统上所有可用的 CPU 和内存创建在星期六早上 7:00 开始运行的每周垃圾回收作业。
    ```PowerShell
    New-DedupSchedule -Name "WeeklyGarbageCollection" -Type GarbageCollection -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(6) -Start (Get-Date "2016-08-13 07:00:00")
    ```

5. 使用高优先级以及系统上所有可用的 CPU 和内存创建在星期日早上 7:00 开始运行的每周完整性清理作业。
    ```PowerShell
    New-DedupSchedule -Name "WeeklyIntegrityScrubbing" -Type Scrubbing -DurationHours 23 -Memory 100 -Cores 100 -Priority High -Days @(0) -Start (Get-Date "2016-08-14 07:00:00")
    ```

### <a id="modifying-job-schedules-available-settings"></a>可用的作业范围设置
你可以为新的或计划的重复数据删除作业切换以下设置：

<table>
    <thead>
        <tr>
            <th style="min-width:125px">参数名称</th>
            <th>定义</th>
            <th>可接受的值</th>
            <th>为什么想要设置此值？</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>在任务栏的搜索框中键入</td>
            <td>应计划的作业类型</td>
            <td>
                <ul>
                    <li>优化</li>
                    <li>GarbageCollection</li>
                    <li>推移</li>
                </ul>
            </td>
            <td>需要该值，因为它是你想要计划的作业类型。 任务计划后，该值无法更改。</td>
        </tr>
        <tr>
            <td>Priority</td>
            <td>计划作业的系统优先级</td>
            <td>
                <ul>
                    <li>高</li>
                    <li>中等</li>
                    <li>低</li>
                </ul>
            </td>
            <td>该值帮助系统确定如何分配 CPU 时间。 *高*将使用更多 CPU 时间，*低*将使用更少 CPU 时间。</td>
        </tr>
        <tr>
            <td>天</td>
            <td>作业被计划在周几执行</td>
            <td>0-6 的整数数组，表示每周的哪一天：<ul>
                <li>0 = 星期日</li>
                <li>1 = 星期一</li>
                <li>2 = 星期二</li>
                <li>3 = 星期三</li>
                <li>4 = 星期四</li>
                <li>5 = 星期五</li>
                <li>6 = 星期六</li>
            </ul></td>
            <td>计划的作业必须至少运行一天。</td>
        </tr>
        <tr>
            <td>核心</td>
            <td>系统上作业应使用的核心数的百分比</td>
            <td>整数 0-100（表示百分比）</td>
            <td>控制作业对系统上的计算资源具有的影响级别</td>
        </tr>
        <tr>
            <td>DurationHours</td>
            <td>允许作业应运行的最大小时数</td>
            <td>正整数</td>
            <td>若要防止作业在工作负荷的非空闲时间运行</td>
        </tr>
        <tr>
            <td>Enabled</td>
            <td>作业是否将运行</td>
            <td>True/false</td>
            <td>在不删除作业的情况下将其禁用</td>
        </tr>
        <tr>
            <td>完全</td>
            <td>用于计划完整的垃圾回收作业</td>
            <td>开关 (true/false)</td>
            <td>默认情况下，每个第四个作业为完整的垃圾回收作业。 使用此开关可以计划完整的垃圾回收以更频繁地运行。</td>
        </tr>
        <tr>
            <td>InputOutputThrottle</td>
            <td>指定适用于作业的输入/输出限制量。</td>
            <td>整数 0-100（表示百分比）</td>
            <td>限制确保作业不会干扰其他 I/O 密集型进程。</td>
        </tr>
        <tr>
            <td>内存</td>
            <td>系统上作业应使用的内存百分比</td>
            <td>整数 0-100（表示百分比）</td>
            <td>控制作业对系统的内存资源具有的影响级别</td>
        </tr>
        <tr>
            <td>名称</td>
            <td>计划作业的名称</td>
            <td>字符串</td>
            <td>作业必须具有唯一的可识别名称。</td>
        </tr>
        <tr>
            <td>ReadOnly</td>
            <td>指示清理作业进程及其找到的损坏的报告，但不会运行任何修复操作</td>
            <td>开关 (true/false)</td>
            <td>你想要手动还原位于坏的磁盘区域的文件。</td>
        </tr>
        <tr>
            <td>开始时间</td>
            <td>指定作业应开始的时间</td>
            <td>`System.DateTime`</td>
            <td>为*开始*提供的 `System.Datetime` 的*日期*部分不相关（只要它处于过去），但*时间*部分指定作业应该启动的时间。</td>
        </tr>
        <tr>
            <td>StopWhenSystemBusy</td>
            <td>指定在系统繁忙时重复数据删除是否应停止</td>
            <td>开关 (true/false)</td>
            <td>此开关为你提供控制重复数据删除行为的功能 - 当你想要在你的工作负荷处于非空闲状态下运行重复数据删除时，此功能尤其重要。</td>
        </tr>
    </tbody>
</table>

## <a id="modifying-volume-settings"></a>修改重复数据删除卷范围设置
### <a id="modifying-volume-settings-how-to-toggle"></a>切换卷设置
你可以通过在为卷启用删除重复时所选的[使用类型](understand.md#usage-type)为重复数据删除设置卷范围的默认设置。 重复数据删除包括使编辑卷范围的设置易于操作的 cmdlet：

* [`Get-DedupVolume`](https://technet.microsoft.com/library/hh848448.aspx)
* [`Set-DedupVolume`](https://technet.microsoft.com/library/hh848438.aspx)

从所选的使用类型修改卷设置的主要原因是为特定文件提升读取性能（例如多媒体或其他已被压缩的文件类型）或微调重复数据删除以便为你的特定工作负荷实现更好的优化。 以下示例演示了如何为工作负荷修改重复数据删除卷设置，该工作负荷最接近类似于一般用途的文件服务器工作负荷，但使用频繁更改的大型文件。

1. 请参阅群集共享卷 1 的当前卷设置。
    ```PowerShell
    Get-DedupVolume -Volume C:\ClusterStorage\Volume1 | Select *
    ```

2. 启用群集共享卷 1 上的 OptimizePartialFiles 以便 MinimumFileAge 策略适用于文件的某些部分而非整个文件。 此操作确保文件的大部分获得优化，即使文件的各部分定期更改。
    ```PowerShell
    Set-DedupVolume -Volume C:\ClusterStorage\Volume1 -OptimizePartialFiles
    ```

### <a id="modifying-volume-settings-available-settings"></a>可用的卷范围设置
<table>
    <thead>
        <tr>
            <th style="min-width:125px">设置名称</th>
            <th>定义</th>
            <th>可接受的值</th>
            <th>为什么想要修改此值？</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>ChunkRedundancyThreshold</td>
            <td>区块在复制到区块存储的热点区域前被引用的次数。 热点区域的价值在于被频繁引用的所谓的“热点”区块具有提升访问时间的多个访问路径。</td>
            <td>正整数</td>
            <td>修改此数量的主要原因是为具有高重复的卷增加节约率。 通常情况下，默认值 (100) 是推荐的设置，你无需修改此值。</td>
        </tr>
        <tr>
            <td>ExcludeFileType</td>
            <td>从优化中排除的文件类型</td>
            <td>文件扩展数组</td>
            <td>某些文件类型，尤其是多媒体文件或已被压缩的文件，不会从优化中获益太多。 此设置允许你配置排除的类型。</td>
        </tr>
        <tr>
            <td>ExcludeFolder</td>
            <td>指定不应考虑优化的文件夹路径。</td>
            <td>文件夹路径数组</td>
            <td>如果你想要在优化时提升性能或将内容保留在特定路径中，你可以在考虑优化时排除卷上的某些路径。</td>
        </tr>
        <tr>
            <td>InputOutputScale</td>
            <td>为重复数据删除指定 IO 并行化级别（IO 队列）以供其在后处理作业期间的卷上使用。</td>
            <td>正整数的范围是 1-36</td>
            <td>修改该值的主要原因是通过限制允许重复数据删除在卷上使用的 IO 队列数来减少对高 IO 工作负荷性能的影响。 请注意，修改此默认设置可能导致重复数据删除的后处理作业运行缓慢。</td>
        </tr>
        <tr>
            <td>MinimumFileAgeDays</td>
            <td>文件创建后，在它被视为符合策略可进行优化之前的天数。</td>
            <td>正整数（包括零）</td>
            <td>**默认**和 **HyperV** 使用类型将此值设置为 3 以使热点文件或最近创建的文件的性能最大化。 如果你想要重复数据删除具有更高性能或你并不在意与删除重复相关的额外延迟，则可能想要对其修改。</td>
        </tr>
        <tr>
            <td>MinimumFileSize</td>
            <td>文件被视为符合策略可进行优化的最小文件大小。</td>
            <td>正整数（字节）大于 32 KB</td>
            <td>更改该值的主要原因是排除可能具有有限优化值的小型文件以节省计算时间。</td>
        </tr>
        <tr>
            <td>NoCompress</td>
            <td>将区块置入区块存储前是否需要压缩</td>
            <td>True/False</td>
            <td>某些类型的文件，尤其是多媒体文件和已压缩的文件类型，其压缩效果可能不好。 此设置允许你关闭卷上对所有文件的压缩。 如果你正在优化包含许多已压缩的文件的数据集，则此设置是理想之选。</td>
        </tr>
        <tr>
            <td>NoCompressionFileType</td>
            <td>区块被置入区块存储前不应被压缩的文件类型。</td>
            <td>文件扩展数组</td>
            <td>某些类型的文件，尤其是多媒体文件和已压缩的文件类型，其压缩效果可能不好。 此设置允许对这些文件关闭压缩，以节省 CPU 资源。</td>
        </tr>
        <tr>
            <td>OptimizeInUseFiles</td>
            <td>启用时，对其具有活动句柄的文件将被视为符合策略可进行优化。</td>
            <td>True/false</td>
            <td>如果你的工作负荷使文件在较长时间处于打开状态，则启用此设置。 如果该设置未启用，则文件将永远不会被优化（如果工作负荷对其具有开放的句柄），即使它只是偶尔在结尾处追加数据。</td>
        </tr>
        <tr>
            <td>OptimizePartialFiles</td>
            <td>如果启用，MinimumFileAge 值适用于文件的分段，而不是整个文件。</td>
            <td>True/false</td>
            <td>如果你的工作负荷使用通常为已被编辑的大型文件（其中大部分文件内容被原样保留），则启用此设置。 如果未启用此设置，则这些文件将永远不会被优化，因为它们会一直更改，即使大部分文件内容已准备好被优化。</td>
        </tr>
        <tr>
            <td>验证</td>
            <td>启用时，如果某个区块的哈希与我们的区块存储中已有的区块相匹配，则将区块按字节进行比较以确保它们完全相同。</td>
            <td>True/false</td>
            <td>这是一项完整性功能，它通过比较实际不同但具有相同哈希的两个数据区块来确保比较区块的哈希算法不会犯错。 在实践中，发生这种情况的可能性极低。 启用验证功能大大增加了优化作业的开销。</td>
        </tr>
    </tbody>
</table>

## <a id="modifying-dedup-system-settings"></a>修改重复数据删除系统范围设置
重复数据删除具有其他系统范围设置，它可通过[注册表](https://technet.microsoft.com/library/cc755256(v=ws.11).aspx)配置。 这些设置适用于在系统上运行的所有作业和卷。 无论何时对注册表进行编辑，都必须格外小心。

例如，你可能想要禁用完整垃圾回收。 有关此设置为什么对你的方案有用的详细信息，请参阅[常见问题](#faq-why-disable-full-gc)。 若要使用 PowerShell 编辑注册表：

* 如果在群集中运行重复数据删除：
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    Set-ItemProperty -Path HKLM:\CLUSTER\Dedup -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

* 如果不在群集中运行重复数据删除：
    ```PowerShell
    Set-ItemProperty -Path HKLM:\System\CurrentControlSet\Services\ddpsvc\Settings -Name DeepGCInterval -Type DWord -Value 0xFFFFFFFF
    ```

### <a id="modifying-dedup-system-settings-available-settings"></a>可用的系统范围设置
<table>
    <thead>
        <tr>
            <th style="min-width:125px">设置名称</th>
            <th>定义</th>
            <th>可接受的值</th>
            <th>为什么想要更改此设置？</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>WlmMemoryOverPercentThreshold</td>
            <td>此设置允许作业使用比重复数据删除判断为实际可用更多的内存。 例如，300 的设置意味着作业可能必须使用分配的内存的三倍以进行取消。</td>
            <td>正整数（300 的值意味着 300% 或 3 倍）</td>
            <td>如果你有其他任务，则当重复数据删除占用更多内存时，该任务将停止</td>
        </tr>
        <tr>
            <td>DeepGCInterval</td>
            <td>此设置配置常规垃圾回收作业变为[完整垃圾回收作业](advanced-settings.md#faq-full-v-regular-gc)的时间间隔。 如果设置为 n，则意味着每 n<sup></sup> 个作业均为完整的垃圾回收作业。 请注意，对于具有[备份用途类型](understand.md#usage-type-backup)的卷，完整的垃圾回收始终处于禁用状态（与注册表值无关）。 `Start-DedupJob -Type GarbageCollection -Full` 如果备份卷上需要完整的垃圾回收，则可以使用。</td>
            <td>整数（-1 表示禁用）</td>
            <td>请参阅[此常见问题](advanced-settings.md#faq-why-disable-full-gc)</td>
        </tr>
    </tbody>
</table>

## <a id="faq"></a>常见问题
<a id="faq-use-responsibly"></a>**我更改了重复数据删除设置，并且现在作业缓慢或无法完成，或我的工作负荷性能已降低。为什么?**  
这些设置为你提供了控制重复数据删除如何运行的许多权限。 负责任地使用它们，并[监视性能](run.md#monitoring-dedup)。

<a id="faq-running-dedup-jobs-manually"></a>**我想要运行重复数据删除作业，但我不想创建新计划-我可以这样做？**  
可以，[所有作业都可手动运行](run.md#running-dedup-jobs-manually)。

<a id="faq-full-v-regular-gc"></a>**完整和常规垃圾回收之间的区别是什么？**  
有两种类型的[垃圾回收](understand.md#job-info-gc)：

- *常规垃圾回收*使用统计算法查找符合某个条件的大型未引用的区块（内存和 IOP 较低）。 常规垃圾回收仅在引用区块的百分比未达到最小值时才对区块存储容器进行压缩。 与完整垃圾回收相比，该类型的垃圾回收运行更快且占用更少资源。 常规垃圾回收作业的默认计划是一周运行一次。
- *完整垃圾回收*可以更全面的查找未引用的区块并释放更多的磁盘空间。 完整垃圾回收压缩每个容器，即使容器中仅有一个区块未引用。 完整垃圾回收也会释放已在使用的空间（如果在优化作业期间出现崩溃或电源故障）。 完整垃圾回收作业将恢复 100% 的可用空间（该空间可在删除了重复数据的卷上恢复），与常规垃圾回收作业相比，该作业需要更多的时间和系统资源。 与常规垃圾回收作业相比，完整垃圾回收作业通常将找到并释放超过 5% 的未引用数据。 完整垃圾回收作业的默认计划是在每第四次垃圾回收时运行。

<a id="faq-why-disable-full-gc"></a>**我为什么要禁用完整垃圾回收？**  
- 垃圾回收可能会对卷的生命周期卷影副本和增量备份的大小具有负面影响。 使用完整垃圾回收作业，高改动或 I/O 密集型工作负荷可能会出现性能下降。           
- 你可以从 PowerShell 手动运行完整垃圾回收作业以清理泄漏（如果你知道系统已崩溃）。
