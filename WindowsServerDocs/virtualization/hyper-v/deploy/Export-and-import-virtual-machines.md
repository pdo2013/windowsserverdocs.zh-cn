---
title: 导出和导入虚拟机
description: 演示如何使用 Hyper-v 管理器或 Windows PowerShell 导出和导入虚拟机。
ms.prod: windows-server
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: 6e130ee8a040cd5b56908d77d91bf196a60de6f7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392975"
---
>适用于：Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

# <a name="export-and-import-virtual-machines"></a>导出和导入虚拟机

本文介绍如何导出和导入虚拟机，这是移动或复制虚拟机的一种快速方法。 本文还讨论了执行导出或导入时要执行的一些选择。

## <a name="export-a-virtual-machine"></a>导出虚拟机

导出将所有必需的文件收集到一个单元-虚拟硬盘文件、虚拟机配置文件和任何检查点文件中。 你可以在处于 "已启动" 或 "已停止" 状态的虚拟机上执行此操作。

### <a name="using-hyper-v-manager"></a>使用 Hyper-V 管理器

若要创建虚拟机导出：

1. 在 "Hyper-v 管理器" 中，右键单击虚拟机并选择 "**导出**"。

2. 选择存储导出文件的位置，并单击 "**导出**"。

导出完成后，可以在导出位置下查看所有导出的文件。

### <a name="using-powershell"></a>使用 PowerShell

在替换 \<vm name @ no__t 和 \<path @ no__t 后，以管理员身份打开会话并运行如下所示的命令：

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

有关详细信息，请参阅[Export-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm)。

## <a name="import-a-virtual-machine"></a>导入虚拟机 

导入虚拟机会向 Hyper-V 主机注册该虚拟机。 你可以导入回主机或新主机。 如果要导入到同一主机，则无需先导出虚拟机，因为 Hyper-v 会尝试通过可用文件重新创建虚拟机。 导入虚拟机会注册虚拟机，使其可在 Hyper-v 主机上使用。

"导入虚拟机" 向导还可以帮助您修复从一个主机移动到另一个主机时可能存在的不兼容问题。 这通常与物理硬件（如内存、虚拟交换机和虚拟处理器）存在差异。

### <a name="import-using-hyper-v-manager"></a>使用 Hyper-v 管理器导入

导入虚拟机：

1. 从 Hyper-v 管理器的 "**操作**" 菜单中，单击 "**导入虚拟机**"。

2. 单击“下一步”。

3. 选择包含导出文件的文件夹，然后单击 "**下一步**"。

4. 选择要导入的虚拟机。

5. 选择导入类型，然后单击 "**下一步**"。 （有关说明，请参阅下面的[导入类型](#import-types)。）

6. 单击 **“完成”** 。

### <a name="import-using-powershell"></a>使用 PowerShell 导入

按照所需导入类型的示例，使用**导入 VM** cmdlet。 有关类型的说明，请参阅下面的[导入类型](#import-types)。 

#### <a name="register-in-place"></a>就地注册

此类导入使用导入时存储它们的文件，并保留虚拟机的 ID。 以下命令显示了导入文件的示例。 使用自己的值运行类似的命令。

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>还原

若要导入虚拟机为虚拟机文件指定自己的路径，请运行如下所示的命令，并将示例替换为值：

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>作为副本导入

若要完成复制导入并将虚拟机文件移动到默认的 Hyper-v 位置，请运行如下所示的命令，并将示例替换为值：

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

有关详细信息，请参阅[导入-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm)。

### <a name="import-types"></a>导入类型

Hyper-v 提供了三种导入类型：

- **就地注册**–此类型假定导出文件位于你将在其中存储和运行虚拟机的位置。 导入的虚拟机具有与导出时相同的 ID。 因此，如果已向 Hyper-v 注册了虚拟机，则需要在导入工作前将其删除。 导入完成后，导出文件将变为运行状态文件，并且无法删除。

- **还原虚拟机**–将虚拟机还原到你选择的位置，或者使用默认的 hyper-v。 此导入类型将创建已导出文件的副本，并将其移动到所选位置。 导入完成后，虚拟机具有与导出时相同的 ID。 因此，如果虚拟机已在 Hyper-v 中运行，则需要先删除该虚拟机，然后才能完成导入。 导入完成后，导出的文件将保持不变，并且可以删除或重新导入。

- **复制虚拟机**–在中，你可以选择文件的位置。 不同之处在于导入的虚拟机具有新的唯一 ID，这意味着你可以多次将虚拟机导入同一主机。

