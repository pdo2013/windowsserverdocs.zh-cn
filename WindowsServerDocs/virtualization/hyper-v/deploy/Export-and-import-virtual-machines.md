---
title: 导出和导入虚拟机
description: 演示如何导出和导入虚拟机使用 Hyper-v 管理器或 Windows PowerShell。
ms.prod: windows-server-threshold
author: KBDAzure
ms.author: kathydav
manager: dongill
ms.technology: compute-hyper-v
ms.date: 12/13/2016
ms.topic: article
ms.assetid: 7fd996f5-1ea9-4b16-9776-85fb39a3aa34
ms.openlocfilehash: b326fe8d7ff05ba73fc94225fa38921b42eb3fc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844318"
---
>适用于：Windows 10、 Windows Server 2016、 Microsoft HYPER-V Server 2016，Windows Server 2019，Microsoft HYPER-V Server 2019

# <a name="export-and-import-virtual-machines"></a>导出和导入虚拟机

本文介绍如何导出和导入虚拟机，这是一种快速方法移动或复制它们。 本文还讨论的某些选择，请执行导出时或导入。

## <a name="export-a-virtual-machine"></a>导出虚拟机

导出到一个单元-虚拟硬盘文件、 虚拟机配置文件和任何检查点文件收集所有所需的文件。 可以在启动或停止状态的虚拟机上执行此操作。

### <a name="using-hyper-v-manager"></a>使用 Hyper-V 管理器

若要创建虚拟机导出：

1. 在 HYPER-V 管理器中，右键单击虚拟机，并选择**导出**。

2. 选择位置存储已导出的文件，并单击**导出**。

完成导出后，可以看到所有已导出的文件的导出位置下。

### <a name="using-powershell"></a>使用 PowerShell

以管理员身份打开会话并运行命令如下所示，替换后\<vm 名称\>并\<路径\>:

```powershell
Export-VM -Name \<vm name\> -Path \<path\>
```

有关详细信息，请参阅[EXPORT-VM](https://docs.microsoft.com/powershell/module/hyper-v/export-vm)。

## <a name="import-a-virtual-machine"></a>导入虚拟机 

导入虚拟机会向 Hyper-V 主机注册该虚拟机。 您可以导入回主机或新的主机。 如果您正在导入到同一主机中，不需要导出虚拟机首先，因为尝试重新创建发布的文件中的虚拟机的 HYPER-V。 导入虚拟机将其注册的 HYPER-V 主机上使用它。

导入虚拟机向导还帮助您修复可以从一个主机之间移动时存在的不兼容问题。 这是常见物理硬件，例如内存、 虚拟交换机和虚拟处理器之间的差异。

### <a name="import-using-hyper-v-manager"></a>使用 Hyper-v 管理器的导入

若要导入虚拟机：

1. 从**操作**菜单中的 HYPER-V 管理器中，单击**导入虚拟机**。

2. 单击“下一步” 。

3. 选择包含所导出的文件的文件夹，然后单击**下一步**。

4. 选择要导入的虚拟机。

5. 选择导入类型，然后单击**下一步**。 (有关说明，请参阅[导入类型](#import-types)下文。)

6. 单击 **“完成”**。

### <a name="import-using-powershell"></a>使用 PowerShell 导入

使用**导入-VM** cmdlet，按照你想导入的类型的示例。 类型的说明，请参阅[导入类型](#import-types)下文。 

#### <a name="register-in-place"></a>就地注册

这种类型的导入使用导入时存储的文件，并保留虚拟机的 id。 以下命令显示导入文件的示例。 使用你自己的值运行类似的命令。

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' 
```

#### <a name="restore"></a>还原

若要导入虚拟机指定你自己的虚拟机文件的路径，请运行如下，命令示例替换为你的值：

```powershell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -VhdDestinationPath 'D:\Virtual Machines\WIN10DOC' -VirtualMachinePath 'D:\Virtual Machines\WIN10DOC'
```

#### <a name="import-as-a-copy"></a>作为副本导入

若要完成复制导入并将虚拟机文件移动到默认的 HYPER-V 位置，运行示例替换为你的值如下，命令：

``` PowerShell
Import-VM -Path 'C:\<vm export path>\2B91FEB3-F1E0-4FFF-B8BE-29CED892A95A.vmcx' -Copy -GenerateNewId
```

有关详细信息，请参阅[导入-VM](https://docs.microsoft.com/powershell/module/hyper-v/import-vm)。

### <a name="import-types"></a>导入类型

HYPER-V 提供了三个导入类型：

- **就地注册**– 此类型假定导出文件位于你将在这里存储和运行虚拟机的位置。 像以前那样在导出时，导入的虚拟机具有相同的 ID。 因此，如果虚拟机已注册的 HYPER-V，它需要导入工作之前，删除。 完成导入后，导出文件将变为运行状态文件并且不能删除。

- **还原虚拟机**– 虚拟机还原到所选的位置或使用默认值为 HYPER-V。 此导入类型创建导出文件的副本，并将其移动到所选位置。 导入完成后，虚拟机具有与导出时相同的 ID。 因此，如果虚拟机已在运行的 HYPER-V 中，它需要导入后，才能完成删除。 完成导入后，导出的文件保持不变，可删除或重新导入。

- **复制虚拟机**– 这是类似于还原类型，选择文件的位置。 不同之处在于，导入的虚拟机有新的唯一 ID，这意味着您可以导入虚拟机到同一主机多次。

