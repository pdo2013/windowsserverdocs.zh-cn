---
title: DiskPart 命令
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 7155dbf34f9986b3ebdd8b549b6a861cf7fcfe3a
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560423"
---
# <a name="diskpart-commands"></a>DiskPart 命令

适用于：Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008

DiskPart 命令可帮助你管理电脑的驱动器 (磁盘、分区、卷或虚拟硬盘)。 你必须首先列出, 然后选择一个对象来使其成为焦点, 然后才能使用 DiskPart 命令。 当对象具有焦点时, 键入的任何 DiskPart 命令都将作用于该对象。

可以通过使用**list disk、list volume、list partition**和**list vdisk**命令列出可用对象, 并确定对象的编号或驱动器号。 **List disk、list vdisk**和**list volume**命令显示计算机上的所有磁盘和卷。 但是,**列表分区**命令只显示有焦点的磁盘上的分区。 使用**list**命令时, 会在具有焦点的\*对象旁边显示一个星号 ()。

选择对象时, 焦点将保留在该对象上, 直至选择其他对象。 例如, 如果焦点设置在磁盘0上, 并且选择了磁盘2上的卷 8, 则焦点将从磁盘0转移到磁盘2、卷8。 某些命令会自动更改焦点。 例如, 在创建新的分区时, 焦点会自动切换到新的分区。

只能将焦点放到所选磁盘上的分区。 当分区具有焦点时, 相关卷 (如果有) 也具有焦点。 当卷具有焦点时, 如果卷映射到单个特定分区, 则相关磁盘和分区也会获得焦点。 如果不是这样, 则对磁盘和分区的关注会丢失。

## <a name="diskpart-commands"></a>DiskPart 命令

若要启动 DiskPart 命令解释器, 请在命令提示符下键入:

`diskpart`

> [!IMPORTANT]
> 本地**Administrators**组中的成员身份或等效身份是运行 DiskPart 所需的最低要求。 

可以在 Diskpart 命令解释器中运行以下命令:

  - [Active](active.md)  
      
  - [Add](add.md)  
      
  - [Assign](assign.md)  
      
  - [附加 vdisk](attach-vdisk.md)  
      
  - [属性](attributes.md)  
      
  - [自动装载](automount.md)  
      
  - [分](break.md)  
      
  - [整洁](clean.md)  
      
  - [Compact vdisk](compact-vdisk.md)  
      
  - [Convert](convert.md)  
      
  - [创建](create.md)  
      
  - [删除](delete.md)  
      
  - [分离 vdisk](detach-vdisk.md)  
      
  - [仔细](detail.md)  
      
  - [Exit](exit.md)  
      
  - [展开 vdisk](expand-vdisk.md)  
      
  - [Extend](extend.md)  
      
  - [文件系统](filesystems.md)  
      
  - [Format](format.md)  
      
  - [GPT](gpt.md)  
      
  - [帮助](help.md)  
      
  - [导入](import.md)  
      
  - [不用](inactive.md)  
      
  - [成员列表](list.md)  
      
  - [Merge vdisk](merge-vdisk.md)  
      
  - [断开](offline.md)  
      
  - [联机](online.md)  
      
  - [恢复](recover.md)  
      
  - [剩余](rem.md)  
      
  - [删除](remove.md)  
      
  - [修正](repair.md)  
      
  - [扫描](rescan.md)  
      
  - [变化](retain.md)  
      
  - [北京](san.md)  
      
  - [单击](select.md)  
      
  - [设置 id](set-id.md)  
      
  - [收缩](shrink.md)  
      
  - [Uniqueid](uniqueid.md)  
      

## <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[Windows PowerShell 中的存储 Cmdlet](https://docs.microsoft.com/powershell/module/storage/)
