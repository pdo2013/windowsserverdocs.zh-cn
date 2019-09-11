---
title: 虚拟机资源控制
description: 使用虚拟机 CPU 组
keywords: windows 10, hyper-v
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: 41390421c9e3126915cdf2e827e251e84495bafd
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872025"
---
>适用于：Windows Server 2016，Microsoft Hyper-V Server 2016，Windows Server 2019，Microsoft Hyper-V 服务器2019

# <a name="virtual-machine-resource-controls"></a>虚拟机资源控制

本文介绍了虚拟机的 Hyper-v 资源和隔离控制。  Windows Server 2016 中引入了这些功能，这些功能称为虚拟机 CPU 组，或只是 "CPU 组"。  利用 CPU 组，Hyper-v 管理员可以更好地管理和分配跨来宾虚拟机的主机 CPU 资源。  使用 CPU 组，Hyper-v 管理员可以：

* 创建虚拟机组，其中每个组都有不同的虚拟化主机 CPU 资源分配，在整个组中共享。 这允许主机管理员为不同类型的 Vm 实现服务类。

* 将 CPU 资源限制设置为特定组。 此 "组上限" 设置整个组可能使用的主机 CPU 资源的上限，为该组有效地强制实施所需的服务类别。

* 约束一个 CPU 组，使其仅在一组特定主机系统的处理器上运行。 这可用于隔离属于不同 CPU 组的虚拟机。

## <a name="managing-cpu-groups"></a>管理 CPU 组

CPU 组通过 Hyper-v 主机计算服务或 HCS 进行管理。 Microsoft 虚拟化团队的博客[介绍了主机计算服务（HCS）](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/)的发布，其中提供了有关 HCS 及其 GENESIS、HCS api 的链接等内容的很好的说明。

>[!NOTE] 
>只有 HCS 可以用于创建和管理 CPU 组;Hyper-v 管理器 applet、WMI 和 PowerShell 管理接口不支持 CPU 组。

Microsoft 在[Microsoft 下载中心](https://go.microsoft.com/fwlink/?linkid=865968)上提供了一个命令行实用程序 cpugroups，它使用 HCS 界面来管理 CPU 组。  此实用程序还可以显示主机的 CPU 拓扑。

## <a name="how-cpu-groups-work"></a>CPU 组的工作方式

Hyper-v 虚拟机监控程序使用计算的 CPU 组上限来强制对 CPU 组分配主机计算资源。 CPU 组上限是 CPU 组的总 CPU 容量的一部分。 组帽的值取决于组类或分配的优先级。 计算出的组帽可以视为 "一定数量的 LP CPU 时间"。 此组预算是共享的，因此，如果只有一个 VM 处于活动状态，则它可以使用整个组的 CPU 分配。

CPU 组上限计算为 G = *n* x *C*，其中：

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system's total compute capacity

例如，假设某个 CPU 组配置有4个逻辑处理器（LPs），上限为 50%。

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

在此示例中，CPU 组 G 分配了2个 LP 的 CPU 时间。  

请注意，无论绑定到组的虚拟机或虚拟处理器的数量是多少，无论分配给 CPU 组的虚拟机的状态（例如，关闭或启动）如何，组 cap 都适用。 因此，绑定到同一个 CPU 组的每个 VM 都将收到该组的总 CPU 分配，这将更改与绑定到 CPU 组的 Vm 的数目。 因此，由于 Vm 是来自 CPU 组的绑定或未绑定 Vm，因此总体 CPU 组上限必须为重新调整，并设置为维护所需的每 VM 上限。 VM 主机管理员或虚拟化管理软件层负责根据需要管理组帽以实现所需的每个 VM 的 CPU 资源分配。

## <a name="example-classes-of-service"></a>服务的示例类

让我们看一些简单的示例。 首先，假定 Hyper-v 主机管理员要为来宾 Vm 支持两个服务层：

1. 低端 "C" 层。 我们将为此第10层的整个主机计算资源分配。

1. 中间范围 "B" 层。 此级别分配了整个主机计算资源的 50%。

在本示例中，我们将断言未使用其他 CPU 资源控制，如单个 VM cap、权重和预留。
不过，单个 VM cap 非常重要，因为我们稍后会看到。

为简单起见，我们假设每个 VM 都有1个副总裁，主机有 8 LPs。 我们将从一个空主机开始。

若要创建 "B" 层，主机 adminstartor 会将组上限设置为 50%：

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

主机管理员添加了一个 "B" 层 VM。
此时，"B" 层 VM 最多可使用 50% 的主机 CPU，或在我们的示例系统中相当于 4 LPs。

现在，管理员添加了第二个 "B 层" VM。 CPU 组的分配-在所有 Vm 之间平均划分。 我们总共在 B 组中有2个 Vm，因此，每个 VM 现在获取全部 50%、25% 或等于 2 LPs 计算时间的一半。

## <a name="setting-cpu-caps-on-individual-vms"></a>在单个 Vm 上设置 CPU 上限

除组帽外，每个 VM 还可以有一个单独的 "VM cap"。 自其简介以来，每个 VM 的 CPU 资源控制（包括 CPU 上限、权重和预留）都属于 Hyper-v。
与组帽结合使用时，VM cap 会指定每个 VP 可以获得的最大 CPU 数量，即使该组具有可用的 CPU 资源。

例如，主机管理员可能希望在 "C" Vm 上放置 10% 的 VM cap。
这样，即使大多数 "C" VPs 处于空闲状态，每个副总裁也永远不会超过 10%。
如果没有 VM cap，"C" Vm 就会找机会在其层所允许的级别之外实现性能。

## <a name="isolating-vm-groups-to-specific-host-processors"></a>将 VM 组隔离到特定主机处理器

Hyper-v 主机管理员可能还希望能够将计算资源专用于 VM。
例如，假设管理员想要提供一个高级的 "A" VM，其类帽为 100%。
这些高级 Vm 还需要尽可能低的计划延迟和抖动;也就是说，它们不能由任何其他 VM 取消计划。
若要实现这种分离，还可以使用特定的 LP 关联映射来配置一个 CPU 组。

例如，若要在我们的示例中适应主机上的 "A" VM，管理员将创建新的 CPU 组，并将组的处理器关联设置为主机的 LPs 的子集。
组 B 和 C 会被关联到剩余的 LPs 中。
管理员可以在组 A 中创建单个 VM，然后对组 A 中的所有 LPs 具有独占访问权限，而可能较低的层组 B 和 C 会共享剩余的 LPs。

## <a name="segregating-root-vps-from-guest-vps"></a>将根 VPs 与来宾 VPs 分离

默认情况下，Hyper-v 将在每个基础物理 LP 上创建根副总裁。
这些根 VPs 与系统 LPs 严格映射1:1，不迁移，也就是说，每个根副总将始终在同一物理 LP 上执行。
可以在任何可用的 LP 上运行来宾 VPs，并将执行与根 VPs 共享。

但是，可能需要从来宾 VPs 完全分离根副总裁活动。
在上面的示例中，我们实现了高级的 "A" 层 VM。
若要确保 "A" VM 的 VPs 具有最低的延迟和 "抖动" 或计划变化，我们想要在一组专用的 LPs 上运行它们，并确保根本不会在这些 LPs 上运行。

这可以使用 "minroot" 配置的组合来完成，这会将主机 OS 分区限制为在总系统逻辑处理器的子集上运行，以及一个或多个关联 CPU 组。

可将虚拟化主机配置为将主机分区限制为特定的 LPs，并将一个或多个 CPU 组关联到剩余的 LPs。
通过这种方式，可以在专用 CPU 资源上运行根和来宾分区，并完全隔离，无 CPU 共享。

有关 "minroot" 配置的详细信息，请参阅[Hyper-v 主机 CPU 资源管理](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016)。

## <a name="using-the-cpugroups-tool"></a>使用 CpuGroups 工具

让我们看看如何使用 CpuGroups 工具的一些示例。

>[!NOTE] 
>CpuGroups 工具的命令行参数只使用空格作为分隔符。 "/" 或 "-" 字符不应继续所需的命令行开关。

### <a name="discovering-the-cpu-topology"></a>发现 CPU 拓扑

通过 GetCpuTopology 执行 CpuGroups 将返回有关当前系统的信息，如下所示，其中包括 LP 索引、LP 所属的 NUMA 节点、包和核心 Id 以及根 VP 索引。

以下示例显示了一个具有2个 CPU 插槽和 NUMA 节点的系统，总计为 32 LPs，已启用多线程，并且配置为从每个 NUMA 节点启用具有8个根 VPs，4的 Minroot。
具有根 VPs 的 LPs 具有 RootVpIndex > = 0;RootVpIndex 为-1 的 LPs 不可用于根分区，但仍由虚拟机监控程序进行管理，并将按其他配置设置允许的方式运行来宾 VPs。

```console
C:\vm\tools>CpuGroups.exe GetCpuTopology

LpIndex NodeNumber PackageId CoreId RootVpIndex
------- ---------- --------- ------ -----------
      0          0         0      0           0
      1          0         0      0           1
      2          0         0      1           2
      3          0         0      1           3
      4          0         0      2          -1
      5          0         0      2          -1
      6          0         0      3          -1
      7          0         0      3          -1
      8          0         0      4          -1
      9          0         0      4          -1
     10          0         0      5          -1
     11          0         0      5          -1
     12          0         0      6          -1
     13          0         0      6          -1
     14          0         0      7          -1
     15          0         0      7          -1
     16          1         1     16           4
     17          1         1     16           5
     18          1         1     17           6
     19          1         1     17           7
     20          1         1     18          -1
     21          1         1     18          -1
     22          1         1     19          -1
     23          1         1     19          -1
     24          1         1     20          -1
     25          1         1     20          -1
     26          1         1     21          -1
     27          1         1     21          -1
     28          1         1     22          -1
     29          1         1     22          -1
     30          1         1     23          -1
     31          1         1     23          -1
```

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>示例2–打印主机上的所有 CPU 组

此处列出了当前主机上的所有 CPU 组、其 GroupId、组的 CPU 上限，以及分配给该组的 LPs 的索引。

请注意，有效的 CPU 上限值介于 [0，65536] 范围内，这些值表示以百分比表示的组上限（例如 32768 = 50%）。

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>示例3–打印单个 CPU 组

在此示例中，我们将使用 GroupId 作为筛选器来查询单个 CPU 组。

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>示例4–创建新的 CPU 组

在这里，我们将创建一个新的 CPU 组，并指定组 ID 以及要分配给该组的 LPs 集。

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

现在会显示新添加的组。

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>示例5–将 CPU 组上限设置为 50%

在这里，我们将 CPU 组上限设置为 50%。

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

现在，让我们通过显示刚刚更新的组来确认设置。

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>示例6–为主机上的所有 Vm 打印 CPU 组 id

```console
C:\vm\tools>CpuGroups.exe GetVmGroup

VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36ab08cb-3a76-4b38-992e-000000000002
```

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>示例7–从 CPU 组中取消绑定 VM

若要从 CPU 组中删除 VM，请将设置为 VM 的 CpuGroupId 为 NULL GUID。 这会将 VM 从 CPU 组中解除绑定。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000

C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>示例 8-将 VM 绑定到现有 CPU 组

此处，我们将 VM 添加到现有 CPU 组。
请注意，VM 不能绑定到任何现有 CPU 组，或设置 CPU 组 id 会失败。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

现在，确认 VM G1 是否处于所需的 CPU 组中。

```console
C:\vm\tools>CpuGroups.exe GetVmGroup
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G2 4ABCFC2F-6C22-498C-BB38-7151CE678758 36ab08cb-3a76-4b38-992e-000000000002
    P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC 36ab08cb-3a76-4b38-992e-000000000003
    P2 A593D93A-3A5F-48AB-8862-A4350E3459E8 36ab08cb-3a76-4b38-992e-000000000004
    G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200 36ab08cb-3a76-4b38-992e-000000000002
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 36AB08CB-3A76-4B38-992E-000000000001
```

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>示例9–打印按 CPU 组 id 分组的所有 Vm

```console
C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId                           VmName                                 VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36ab08cb-3a76-4b38-992e-000000000003     P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004     P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>示例10–打印单个 CPU 组的所有 Vm

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>示例 11-尝试删除非空的 CPU 组

只有空 CPU 组（即没有绑定 Vm 的 CPU 组）可以被删除。
尝试删除非空的 CPU 组将失败。

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>示例12–取消绑定 CPU 组中的唯一 VM 并删除组

在此示例中，我们将使用几个命令来检查 CPU 组，删除属于该组的单个 VM，然后删除该组。

首先，让我们来枚举组中的 Vm。

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

我们发现只有单个 VM （名为 G1）属于此组。
让我们从组中删除 G1 VM，方法是将 VM 的组 ID 设置为 NULL。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

并验证我们的更改 。

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

现在，组为空，可以安全地将其删除。

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

并确认我们的组已消失。

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>示例13–将 VM 重新绑定到其原始 CPU 组

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000002

C:\vm\tools>CpuGroups.exe GetGroupVms
CpuGroupId VmName VmId
------------------------------------ -------------------------------- ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002 G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002 G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
36AB08CB-3A76-4B38-992E-000000000002 G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
36ab08cb-3a76-4b38-992e-000000000003 P1 973B9426-0711-4742-AD3B-D8C39D6A0DEC
36ab08cb-3a76-4b38-992e-000000000004 P2 A593D93A-3A5F-48AB-8862-A4350E3459E8
```
