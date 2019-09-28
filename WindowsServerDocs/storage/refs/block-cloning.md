---
ms.assetid: fd427da3-3869-428f-bf2a-56c4b7d99b40
title: ReFS 上的块克隆
description: ''
author: gawatu
ms.author: gawatu
manager: gawatu
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage-file-systems
ms.openlocfilehash: 81186624e19f9235cbdf8c7f0d44bd2927a68099
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394019"
---
# <a name="block-cloning-on-refs"></a>ReFS 上的块克隆

>适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

块克隆指示文件系统代表应用程序复制某个范围的文件字节，其中，目标文件可与源文件相同或不同。 遗憾的是，复制操作的系统成本较高，因为它们会触发很耗费资源的对基础物理数据的读取和写入。 

不过，ReFS 中的块克隆会以低成本的元数据操作形式执行复制，而不是对文件数据执行读取操作。 因为 ReFS 支持多个文件共享相同的逻辑群集（卷上的物理位置），所以，复制操作仅需将文件的某个区域重新映射到单独的物理位置，并且将成本较高的物理操作转换为快速的逻辑操作。 这使得能够更快地完成复制操作，并且生成与基本存储的更少 I/O。 这一改进还会改善虚拟化工作负载，因为在使用块克隆操作时 .vhdx 检查点合并操作速度会明显加快。 此外，因为多个文件可以共享相同逻辑群集，所以，完全相同的数据不会实际存储多次，从而可提高存储容量。 
  
## <a name="how-it-works"></a>工作原理 

ReFS 上的块克隆将文件数据操作转换为元数据操作。 为了实现此优化，ReFS 将引用计数引入其用于已复制区域的元数据中。 此引用计数记录引用相同物理区域的不同文件区域的数目。 这允许多个文件共享相同的物理数据：

![在多个文件引用相同区域时显示引用计数更新](media/ref-count-example.gif)

通过为每个逻辑群集保留引用计数，ReFS 不会破坏文件之间的隔离：写入共享区域会触发按写入分配的机制，在该机制中，ReFS 会为传入的写入分配新区域。 该机制会保留共享逻辑群集的完整性。 

### <a name="example"></a>示例
假定有两个文件，分别是 X 和 Y，每个文件都由三个区域构成，而每个区域都映射到单独的逻辑群集。

![两个文件分别具有三个不同区域，它们全都映射到引用计数为 1 的区域](media/block-clone-1.png)

现在，假定对于要按区域 E 的偏移量复制的区域 A 和 B，某一应用程序发出从文件 X 到文件 Y 的块克隆操作。将生成以下文件系统状态：

![引用计数对于块克隆区域显示 2](media/block-clone-2.png)

该文件系统状态展示块克隆区域的成功复制。 因为 ReFS 通过仅更新 VCN 到 LCN 映射来执行此复制操作，所以，既不会读取物理数据，也不会覆盖文件 Y 中的物理数据。 文件 X 和 Y 现在共享表中的引用计数所反映的逻辑群集。 由于没有实际复制数据，因此 ReFS 会减少卷上的容量消耗。 

现在，假定该应用程序尝试覆盖文件 X 中的区域 A。ReFS 将复制共享区域，相应更新引用计数，并且执行对新复制区域的传入写入。 这确保保留文件之间的隔离。   

![通过写入新区域 G 和更新引用计数保留的隔离](media/block-clone-3.png)

在修改写入后，区域 B 仍由两个文件共享。 请注意，如果区域 A 大于某个群集，则只会复制已修改的群集，并且剩余部分会保持共享。


## <a name="functionality-restrictions-and-remarks"></a>功能限制和备注
- 源和目标区域必须在某个群集边界开始和结束。 
- 克隆的区域必须在长度上小于 4GB。 
- 可映射到相同物理区域的最大文件区域数目为 8175。
- 目标区域不得超过文件的末尾。 如果应用程序希望扩展具有克隆数据的目标，则它必须首先调用 [SetEndOfFile](https://msdn.microsoft.com/library/windows/desktop/aa365531(v=vs.85).aspx)。 
- 如果源和目标区域处于用一个文件中，则它们不得重叠。 （应用程序可通过将块克隆操作拆分为多个不再重叠的块克隆来继续操作。）
- 源文件和目标文件必须处于相同的 ReFS 卷上。 
- 源文件和目标文件必须具有相同的 [完整性流](https://msdn.microsoft.com/library/windows/desktop/gg258117(v=vs.85).aspx) 设置。 
- 如果源文件是稀疏的，则目标文件必须也是稀疏的。 
- 块克隆操作将破坏共享机会锁（也称作 [2 级机会锁](https://msdn.microsoft.com/library/windows/desktop/aa365713(v=vs.85).aspx)）。
- ReFS 卷必须已使用 Windows Server 2016 进行了格式化；此外，如果正在使用故障转移群集，则在格式化时群集功能级别必须已是 Windows Server 2016 或更高版本。 

## <a name="see-also"></a>请参阅

-   [ReFS 概述](refs-overview.md)
-   [ReFS 完整性流](integrity-streams.md)
-   [存储空间直通概述](../storage-spaces/storage-spaces-direct-overview.md)
-   [DUPLICATE_EXTENTS_DATA](https://msdn.microsoft.com/library/windows/desktop/mt590821(v=vs.85).aspx)
-   [FSCTL_DUPLICATE_EXTENTS_TO_FILE](https://msdn.microsoft.com/library/windows/desktop/mt590823(v=vs.85).aspx)
