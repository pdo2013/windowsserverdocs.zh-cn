---
title: 虚拟机资源控件
description: 使用虚拟机 CPU 组
keywords: windows 10, hyper-v
author: allenma
ms.date: 06/18/2018
ms.topic: article
ms.prod: windows-10-hyperv
ms.service: windows-10-hyperv
ms.assetid: cc7bb88e-ae75-4a54-9fb4-fc7c14964d67
ms.openlocfilehash: 7c4ddf3e5d2ff58eef844c50960327c27a3e0a3d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854758"
---
>适用于：Windows Server 2016 中，Microsoft 的 HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

# <a name="virtual-machine-resource-controls"></a>虚拟机资源控件

本文描述了虚拟机的 HYPER-V 资源和隔离控件。  这些功能，我们将引用为虚拟机 CPU 组或只是"CPU 组"，是 Windows Server 2016 中引入的。  CPU 组可让 HYPER-V 管理员能够更好地管理和来宾虚拟机之间分配主机的 CPU 资源。  使用 CPU 组，HYPER-V 管理员可以：

* 使用具有不同分配虚拟化主机的总 CPU 资源，在整个组之间共享的每个组创建多组虚拟机。 这允许主机管理员来实现不同类型的 Vm 的服务的类。

* 设置 CPU 资源限制为特定组。 此"组 cap"设置为主机上限可能会占用整个组，有效地强制实施所需的类的服务为该组的 CPU 资源。

* 限制 CPU 组仅在一组特定的主机系统的处理器上运行。 这可以用于隔离每个属于不同的 CPU 组的 Vm。

## <a name="managing-cpu-groups"></a>管理 CPU 组

通过 HYPER-V 主机计算服务或 HCS，CPU 组进行管理。 很好的 HCS，其生成描述 HCS Api 及其他内容的链接可在文章中的 Microsoft 虚拟化团队博客[简介主机计算服务 (HCS)](https://blogs.technet.microsoft.com/virtualization/2017/01/27/introducing-the-host-compute-service-hcs/)。

>[!NOTE] 
>仅 HCS 可能用于创建和管理 CPU 组;Hyper-v 管理器小程序，WMI 和 PowerShell 管理界面不支持 CPU 组。

Microsoft 提供的命令行实用工具、 cpugroups.exe 上, [Microsoft Download Center](https://go.microsoft.com/fwlink/?linkid=865968)使用 HCS 接口来管理 CPU 组。  此实用程序还可以显示主机的 CPU 拓扑。

## <a name="how-cpu-groups-work"></a>CPU 组工作原理

主机跨 CPU 组计算资源的分配由 HYPER-V 虚拟机监控程序，使用计算的 CPU 组 cap 强制执行。 CPU 组上限是 CPU 组的总 CPU 容量的一小部分。 组上限的值取决于组类或级别分配优先级。 计算的组上限可视为"的 CPU 时间值得 LP's 是数字"。 此组预算被共享的因此如果只有一个单独的 VM 处于活动状态，它可以为其自身使用整个组的 CPU 分配。

CPU 组 cap 计算为 G = *n* x *C*，其中：

    *G* is the amount of host LP we'd like to assign to the group
    *n* is the total number of logical processors (LPs) in the group
    *C* is the maximum CPU allocation — that is, the class of service desired for the group, expressed as a percentage of the system’s total compute capacity

例如，请考虑配置了 4 个逻辑处理器 (LPs) 和上限的 50%的 CPU 组。

    G = n * C
    G = 4 * 50%
    G = 2 LP's worth of CPU time for the entire group

在此示例中，CPU 组 G 分配 2 LP's 值得的 CPU 时间。  

请注意，组上限适用无论包含几个虚拟机或虚拟处理器绑定到组，并且无论状态 (例如，关闭或启动) 分配给 CPU 组的虚拟机。 因此，绑定到相同的 CPU 组每个 VM 将收到的组的总 CPU 分配的一小部分，这将更改与绑定到 CPU 组的 Vm 的数目。 因此，Vm 绑定或 CPU 组已解除 Vm 时，必须重新调整总体 CPU 组上限，并将设置保持生成所需的每个 VM 上限。 VM 主机的管理员或虚拟化管理软件层负责根据需要来实现所需的每个 VM 的 CPU 资源分配管理组 cap。

## <a name="example-classes-of-service"></a>服务的示例类

让我们看一些简单的示例。 开始时，假定的 HYPER-V 主机管理员想要支持的来宾 Vm 的服务的两个层：

1. 低端硬件的"C"层。 我们将提供此层的整个主机的计算资源的 10%。

1. 中型"B"层。 此层是已分配的整个主机的计算资源的 50%。

此时在我们的示例，我们将断言没有其他 CPU 资源控制是在使用中，单个 VM 上限，如权重，并保留。
但是，单个 VM cap 是重要的是，正如我们稍后将看到。

为简单起见，我们假设每个 VM 都有 1 副总裁，并且我们主机具有 8 LPs。 我们将开始空主机。

若要创建的"B"层，主机 adminstartor 组上限设置为 50%:

    G = n * C
    G = 8 * 50%
    G = 4 LP's worth of CPU time for the entire group

主机管理员将添加一个"B"层 VM。
此时，我们的"B"层 VM 可以使用最多 50%值得的主机的 CPU 或相当于 4 LPs 我们示例中的系统中。

现在，管理员将添加第二个"层 B"VM。 CPU 组的分配，所有 Vm 之间平均划分。 我们已经有 2 个 Vm 总共组 B 中，因此每个 VM 现在可获取组 B 的总数的 50%、 25%或相当于 2 LPs 值得的计算时间的一半。

## <a name="setting-cpu-caps-on-individual-vms"></a>在单个 Vm 上进行设置 CPU 限制

除了组上限，每个 VM 还可以具有单独的"VM cap"。 每个 VM CPU 资源控制，包括 CPU 上限、 粗细和保留，推出以来已 HYPER-V 的一部分。
结合组上限，VM cap 指定可以获取每个副总裁，CPU 的最长，即使组具有可用的 CPU 资源。

例如，主机管理员可能想要"C"的 Vm 上放置的 10 %vm 线帽。
这样一来，即使大多数"C"VPs 处于空闲状态，每个副总裁永远不可能获得超过 10%。
不 VM 设限制，"C"的 Vm 可适时地获得超出允许通过其层级别的性能。

## <a name="isolating-vm-groups-to-specific-host-processors"></a>隔离到特定主机处理器的 VM 组

HYPER-V 主机管理员还可以将计算资源专用于 VM 的功能。
例如，假设管理员想要提供高级具有类上限为 100%的"A"VM。
这些高级 Vm 还需要计划最低的延迟和抖动也就是说，它们可能不会取消计划的任何其他 VM。
若要实现这种分离，CPU 组可以还配置了特定的 LP 关联映射。

例如，若要适合我们的示例中的主机上的"A"VM，管理员将创建一个新的 CPU 组，并设置组的处理器关联的主机的 LPs 子集。
组 B 和 C 将关联到剩余 LPs。
管理员可以创建组，从而会将对所有 LPs 独占访问权限组 A，同时可能较低层组 B 中的单个 VM 和 C 将共享剩余 LPs。

## <a name="segregating-root-vps-from-guest-vps"></a>从来宾 VPs 隔开到根 VPs

默认情况下，HYPER-V 将每个基础物理 LP 上创建根副总裁。
这些根 VPs 严格映射的 1 对 1 与系统 LPs，并且不会迁移 — 即副总裁的每个根将始终在上执行相同的物理 LP。
来宾 VPs 可能在任何可用 LP 上运行，并将与根 VPs 共享执行。

但是，它可能需要完全独立根副总裁活动从来宾 VPs。
请考虑我们上面我们在其中实施高级"A"层 VM 的示例。
为了确保我们"A"的 VM 的 VPs 具有可能最低延迟和"抖动"，或计划的变体，我们想要在一组专用 LPs 上运行它们，并确保根不在这些 LPs 中运行。

这可以使用"minroot"配置限制为在系统总逻辑处理器，以及一个的子集上运行的主机 OS 分区的组合或多个关联 CPU 组。

虚拟化主机可以配置为通过关联到剩余 LPs 的一个或多个 CPU 组限制对特定 LPs 的主分区。
在这种方式，根和来宾分区可以在专用的 CPU 资源上运行，并完全隔离，无 CPU 共享。

有关"minroot"配置的详细信息，请参阅[的 HYPER-V 主机 CPU 资源管理](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-minroot-2016)。

## <a name="using-the-cpugroups-tool"></a>使用 CpuGroups 工具

让我们看一下如何使用 CpuGroups 工具的一些示例。

>[!NOTE] 
>使用仅为空格作为分隔符将 CpuGroups 工具的命令行参数传递。 没有 / 或-字符应继续执行所需的命令行开关。

### <a name="discovering-the-cpu-topology"></a>发现 CPU 拓扑

执行与 GetCpuTopology CpuGroups 返回有关当前系统的信息，如下所示，其中包括 LP 索引，LP 可包核心 Id 和根副总裁索引属于的 NUMA 节点。

下面的示例演示具有 2 个 CPU 套接字的系统的 NUMA 节点，总共 32 LPs 和多线程处理启用，并配置为启用 Minroot 8 根 VPs，从每个 NUMA 节点 4。
具有根 VPs LPs 具有 RootVpIndex > = 0;与为-1 RootVpIndex LPs 不可用于根分区中，但仍由虚拟机监控程序托管和将运行来宾 VPs 所允许的其他配置设置。

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

### <a name="example-2--print-all-cpu-groups-on-the-host"></a>示例 2-在主机上打印所有 CPU 组

在这里，我们会列出当前主机、 其 GroupId、 组的 CPU 上限和 LPs 分配给该组的索引上的所有 CPU 组。

请注意，有效的 CPU 上限值为 [0，65536] 范围内，这些值 express 以百分比表示的组 cap (例如，32768 = 50%)。

```console
C:\vm\tools>CpuGroups.exe GetGroups

CpuGroupId                          CpuCap  LpIndexes
------------------------------------ ------ --------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536  24,25,26,27,28,29,30,31
```

### <a name="example-3--print-a-single-cpu-group"></a>示例 3 – 打印单个 CPU 组

在此示例中，我们将查询作为筛选器使用 GroupId 单个 CPU 组。

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000003
CpuGroupId                          CpuCap   LpIndexes
------------------------------------ ------ ----------
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
```

### <a name="example-4--create-a-new-cpu-group"></a>示例 4 – 创建新的 CPU 组

在这里，我们将创建一个新的 CPU 组，指定要分配到组的组 ID 和 LPs 的集。

```console
C:\vm\tools>CpuGroups.exe CreateGroup /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /GroupAffinity:0,1,16,17
```

现在会显示我们新添加的组。

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 65536 0,1,16,17
36AB08CB-3A76-4B38-992E-000000000002 32768 4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536 12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-5--set-the-cpu-group-cap-to-50"></a>示例 5 – 设置为 50%的 CPU 组上限

在这里，我们将设置为 50%的 CPU 组上限。

```console
C:\vm\tools>CpuGroups.exe SetGroupProperty /GroupId:36AB08CB-3A76-4B38-992E-000000000001 /CpuCap:32768
```

现在让我们通过显示我们刚刚更新的组来确认我们设置。

```console
C:\vm\tools>CpuGroups.exe GetGroups /GroupId:36AB08CB-3A76-4B38-992E-000000000001

CpuGroupId                          CpuCap LpIndexes
------------------------------------ ------ ---------
36AB08CB-3A76-4B38-992E-000000000001 32768 0,1,16,17
```

### <a name="example-6--print-cpu-group-ids-for-all-vms-on-the-host"></a>示例 6 – 适用于所有 Vm 在主机上的打印 CPU 组 id

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

### <a name="example-7--unbind-a-vm-from-the-cpu-group"></a>示例 7 – 取消绑定从 CPU 组的 VM

若要从 CPU 组中删除 VM，请设置为 VM 的 CpuGroupId 为 NULL 的 GUID。 这将解除绑定从 CPU 组 VM。

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

### <a name="example-8--bind-a-vm-to-an-existing-cpu-group"></a>示例 8 – 将一个虚拟机绑定到现有的 CPU 组

在这里，我们将添加到现有的 CPU 组的 VM。
请注意，VM 不能绑定到任何现有的 CPU 组，或者设置 CPU 组 id 将会失败。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:36AB08CB-3A76-4B38-992E-000000000001
```

现在，确认 VM G1 处于所需的 CPU 组。

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

### <a name="example-9--print-all-vms-grouped-by-cpu-group-id"></a>示例 9-打印按 CPU 组 id 分组的所有 Vm

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

### <a name="example-10--print-all-vms-for-a-single-cpu-group"></a>示例 10 – 打印单个 CPU 组的所有 Vm

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36ab08cb-3a76-4b38-992e-000000000002

CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36ab08cb-3a76-4b38-992e-000000000002     G2 4ABCFC2F-6C22-498C-BB38-7151CE678758
36ab08cb-3a76-4b38-992e-000000000002     G3 B0F3FCD5-FECF-4A21-A4A2-DE4102787200
```

### <a name="example-11--attempting-to-delete-a-non-empty-cpu-group"></a>示例 11 – 正在尝试删除非空 CPU 组

仅空 CPU 组 — 绑定，即不使用的 CPU 组 Vm，可以删除。
尝试删除非空 CPU 组将失败。

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
(null)
Failed with error 0xc0350070
```

### <a name="example-12--unbind-the-only-vm-from-a-cpu-group-and-delete-the-group"></a>示例 12 – 解除绑定从 CPU 组的唯一 VM，并删除组

在此示例中，我们将使用多个命令检查 CPU 组，请删除属于该组，单个 VM，然后删除组。

首先，让我们来枚举组中的 Vm。

```console
C:\vm\tools>CpuGroups.exe GetGroupVms /GroupId:36AB08CB-3A76-4B38-992E-000000000001
CpuGroupId                           VmName                                VmId
------------------------------------ ------ ------------------------------------
36AB08CB-3A76-4B38-992E-000000000001     G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC
```

我们可以看到只有一个单独的 VM，名为 G1，属于此组。
让我们通过将 VM 的组 ID 设置为 NULL 删除从我们的组的 G1 VM。

```console
C:\vm\tools>CpuGroups.exe SetVmGroup /VmName:g1 /GroupId:00000000-0000-0000-0000-000000000000
```

并验证我们的更改...

```console
C:\vm\tools>CpuGroups.exe GetVmGroup /VmName:g1
VmName                                 VmId                           CpuGroupId
------ ------------------------------------ ------------------------------------
    G1 F699B50F-86F2-4E48-8BA5-EB06883C1FDC 00000000-0000-0000-0000-000000000000
```

现在，是空的组，我们可以安全地删除它。

```console
C:\vm\tools>CpuGroups.exe DeleteGroup /GroupId:36ab08cb-3a76-4b38-992e-000000000001
```

并确认我们的组将消失。

```console
C:\vm\tools>CpuGroups.exe GetGroups
CpuGroupId                          CpuCap                     LpIndexes
------------------------------------ ------ -----------------------------
36AB08CB-3A76-4B38-992E-000000000002 32768  4,5,6,7,8,9,10,11,20,21,22,23
36AB08CB-3A76-4B38-992E-000000000003 65536  12,13,14,15
36AB08CB-3A76-4B38-992E-000000000004 65536 24,25,26,27,28,29,30,31
```

### <a name="example-13--bind-a-vm-back-to-its-original-cpu-group"></a>示例 13-将虚拟机绑定回其原始的 CPU 组

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
