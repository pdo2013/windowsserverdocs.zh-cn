---
title: diskshadow
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e962537d-b759-4368-b6f1-e8391cf7b221
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b2c5648235a1c856c6aef09621e2381e74d08d70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869178"
---
# <a name="diskshadow"></a>diskshadow

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

diskshadow.exe 是一个工具，公开由卷影复制服务提供的功能\(VSS\)。 默认情况下，diskshadow 使用类似于 diskraid 或 DiskPart 的交互式命令解释器。 diskshadow 还包括可编写脚本的模式。  
  
> [!NOTE]  
> 本地 Administrators 组或同等成员资格是运行 diskshadow 所需的最低。  
  
有关如何使用 diskshadow 命令的示例，请参阅[示例](#BKMK_examples)。  
  
## <a name="syntax"></a>语法  
有关交互模式下，键入以下命令在命令提示符下启动 diskshadow 命令解释器：  
  
```  
diskshadow  
```  
  
对于脚本模式，请键入以下命令，其中*script.txt*是其中包含 diskshadow 的命令的脚本文件：  
  
```  
diskshadow -s script.txt  
```  
  
## <a name="diskshadow-commands"></a>diskshadow 的命令  
Diskshadow 命令解释器中或通过脚本文件，可以运行以下命令：  
  
|参数|描述|  
|-------|--------|  
|[set_2](set_2.md)|设置上下文、 选项、 详细模式下，和创建卷影副本的元数据文件。|  
|[模拟还原](simulate-restore.md)|在计算机上的还原会话而无需发出测试编写器参与**PreRestore**或**PostRestore**到编写器的事件。|  
|[加载元数据](load-metadata.md)|加载元数据.cab 文件导入可传送卷影副本之前，或加载在还原过程中的编写器元数据。|  
|[writer](writer.md)|验证编写器或组件包含或排除从备份或还原过程的编写器或组件。|  
|[add_1](add_1.md)|将卷添加到要进行卷影复制，卷的集或将别名添加到别名环境。|  
|[create_1](create_1.md)|启动卷影副本创建过程，使用当前的上下文和选项设置。|  
|[exec](exec.md)|执行本地计算机上的文件。|  
|[开始备份](begin-backup.md)|启动完整备份的会话。|  
|[结束备份](end-backup.md)|结束完整备份会话和问题**Backupcomplete**具有合适的编写器状态，如果所需的事件。|  
|[开始还原](begin-restore.md)|启动还原会话和问题**PreRestore**所涉及的编写器的事件。|  
|[结束还原](end-restore.md)|结束还原会话和问题**PostRestore**所涉及的编写器的事件。|  
|[reset](reset.md)|将 diskshadow 重置为默认状态。|  
|[list](list.md)|列出了编写器、 卷影副本或在系统上的当前已注册的卷影复制提供程序。|  
|[删除阴影](delete-shadows.md)|删除卷影副本。|  
|[import](import.md)|将可传送卷影副本加载元数据文件中导入到系统。|  
|[mask](mask.md)|删除已导入使用的硬件卷影副本**导入**命令。|  
|[expose](expose.md)|公开为驱动器号、 共享或装入点的永久性卷影副本。|  
|[unexpose](unexpose.md)|使用公开的卷影副本 unexposes**公开**命令。|  
|[break_2](break_2.md)|取消关联 vss 卷影副本卷|  
|[revert](revert.md)|还原恢复到指定的卷影副本卷。|  
|[exit_1](exit_1.md)|退出 diskshadow。|  
  
## <a name="remarks"></a>备注  
  
-   最小值，仅**添加**并**创建**所需创建卷影副本。 但是，这会丢失上下文和选项设置，将为副本备份，并且将仅使用没有备份执行脚本中创建的卷影副本。  
  
## <a name="BKMK_examples"></a>示例  
这是将创建备份的卷影副本的命令的示例序列。 可以保存到文件作为 script.dsh，并使用 diskshadow 执行\/s script.dsh  
  
假设：  
  
-   具有现有目录，名为 c:\\diskshadowdata。  
  
-   系统卷是 c： 和你的数据量为 d:。  
  
-   您有 c： 驱动器中的 backupscript.cmd 文件\\diskshadowdata。  
  
-   Backupscript.cmd 文件将执行卷影数据 p： 和问到你的备份驱动器： 的副本。  
  
可以手动输入以下命令，或其编写脚本：  
  
```  
#diskshadow script file  
set context persistent nowriters  
set metadata c:\diskshadowdata\example.cab  
set verbose on  
begin backup  
add volume c: alias Systemvolumeshadow  
add volume d: alias Datavolumeshadow  
  
create  
  
expose %Systemvolumeshadow% p:  
expose %Datavolumeshadow% q:  
exec c:\diskshadowdata\backupscript.cmd  
end backup  
#End of script  
```  
  
#### <a name="additional-references"></a>其他参考  
[命令行语法解答](command-line-syntax-key.md)  
  

