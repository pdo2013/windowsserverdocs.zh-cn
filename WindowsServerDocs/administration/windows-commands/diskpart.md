---
Title: DiskPart 命令
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: e04af7b6425e208013277d1aaa6f28af62871bcc
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280083"
---
# <a name="diskpart-commands"></a>DiskPart 命令

适用于：Windows 10、 Windows 8.1，Windows 8、 Windows 7、 Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 和 Windows Server 2008

DiskPart 命令可帮助你管理你的电脑的驱动器 （磁盘、 分区、 卷或虚拟硬盘）。 使用 DiskPart 命令之前，您必须首先列出，，然后选择要为其提供焦点的对象。 当一个对象具有焦点时，您键入的所有 DiskPart 命令将都作用于该对象。

可以列出可用的对象并使用来确定对象的编号或驱动器号**列表磁盘、 卷列表、 列表分区**，并**列出的虚拟磁盘**命令。 **磁盘列表、 列表的虚拟磁盘**并**列出卷**命令在计算机上显示所有磁盘和卷。 但是，**分区**命令只显示具有焦点的磁盘上的分区。 当你使用**列表**命令，星号 (\*) 具有焦点的对象旁边会显示。

当选中一个对象时，焦点一直保留该对象上，直到选择不同的对象。 例如，如果将焦点设置在磁盘 0 上并选择磁盘 2 上的卷 8，焦点会从磁盘 0 转移到磁盘 2 上的卷 8。 某些命令会自动更改焦点。 例如，创建一个新的分区时，焦点自动切换到新分区。

仅可以为所选的磁盘上的分区提供焦点。 当分区具有焦点时，相关的卷 （如果有） 也具有焦点。 当卷具有焦点，相关的磁盘和卷映射到单个特定分区，则还有焦点的分区。 如果这种情况下，在磁盘上的焦点并不分区将丢失。

## <a name="diskpart-commands"></a>DiskPart 命令

若要启动 DiskPart 命令解释器，在命令提示符下键入：

`diskpart`

> [!IMPORTANT]
> 本地成员资格**管理员**组或等效身份是最低要求运行 DiskPart。 

在 Diskpart 命令解释器中，可以运行以下命令：

  - [Active](active.md)  
      
  - [Add](add.md)  
      
  - [Assign](assign.md)  
      
  - [附加的虚拟磁盘](attach-vdisk.md)  
      
  - [属性](attributes.md)  
      
  - [自动装载](automount.md)  
      
  - [中断](break.md)  
      
  - [Clean](clean.md)  
      
  - [压缩的虚拟磁盘](compact-vdisk.md)  
      
  - [Convert](convert.md)  
      
  - [创建](create.md)  
      
  - [删除](delete.md)  
      
  - [分离的虚拟磁盘](detach-vdisk.md)  
      
  - [详细信息](detail.md)  
      
  - [Exit](exit.md)  
      
  - [展开的虚拟磁盘](expand-vdisk.md)  
      
  - [Extend](extend.md)  
      
  - [文件系统](filesystems.md)  
      
  - [Format](format.md)  
      
  - [GPT](gpt.md)  
      
  - [帮助](help.md)  
      
  - [导入](import.md)  
      
  - [Inactive](inactive.md)  
      
  - [列表](list.md)  
      
  - [合并的虚拟磁盘](merge-vdisk.md)  
      
  - [Offline](offline.md)  
      
  - [Online](online.md)  
      
  - [Recover](recover.md)  
      
  - [Rem](rem.md)  
      
  - [删除](remove.md)  
      
  - [Repair](repair.md)  
      
  - [重新扫描](rescan.md)  
      
  - [保留](retain.md)  
      
  - [San](san.md)  
      
  - [Select](select.md)  
      
  - [集 id](set-id.md)  
      
  - [Shrink](shrink.md)  
      
  - [uniqueid](uniqueid.md)  
      

## <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[Windows PowerShell 中的存储 Cmdlet](https://docs.microsoft.com/powershell/module/storage/)
