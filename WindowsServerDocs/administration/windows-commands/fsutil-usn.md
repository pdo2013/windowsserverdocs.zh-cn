---
ms.assetid: faad34aa-4ba1-4129-bc1f-08088399e2fa
title: Fsutil usn
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: 4bef63f4938b5ce786bbbfdbf3bdd2081280ce95
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829978"
---
# <a name="fsutil-usn"></a>Fsutil usn
>适用于：Windows Server （半年频道）、 Windows Server 2016 中，Windows 10、 Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

管理更新序列号 (USN) 变更日志。

## <a name="syntax"></a>语法

```
fsutil usn [createjournal] m=<MaxSize> a=<AllocationDelta> <VolumePath>
fsutil usn [deletejournal] {/D | /N} <volumepath>
fsutil usn [enablerangetracking] <volumepath> [options]
fsutil usn [enumdata] <FileRef> <LowUSN> <HighUSN> <VolumePath>
fsutil usn [queryjournal] <VolumePath>
fsutil usn [readdata] <FileName>
fsutil usn [readjournal] [c= <chunk-size> s=<file-size-threshold>] <volumepath>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|-------------|---------------|
|createjournal|创建 USN 更改日志。|
|m=\<MaxSize>|指定的最大大小，以字节为单位，NTFS 将变更日志分配的。|
|a=\<AllocationDelta>|以字节为单位的内存分配添加到末尾并从开始处的变更日志中删除指定的大小。|
|\<VolumePath>|指定 （后跟一个冒号） 的驱动器号。|
|deletejournal|删除或禁用 active USN 更改日志。 **警告：** 删除变更日志会影响文件复制服务 (FRS) 和索引服务中，因为它将需要这些服务执行卷的完整 （和耗时） 扫描。 这又有负面影响 FRS SYSVOL 复制和 DFS 链接备用项时重新扫描卷时之间的复制。|
|/d|禁用活动的 USN 更改日志，并返回输入/输出 (I/O) 控制，而被禁用的变更日志。|
|/n|禁用 active USN 更改日志和变更日志被禁用后才会返回 I/O 控制。|
|enablerangetracking|可实现 USN 写范围跟踪卷。|
|c=\<chunk-size>|指定要跟踪的卷上的区块大小。|
|s=\<file-size-threshold>|指定跟踪的范围内的文件大小阈值。|
|enumdata|枚举并列出了两个指定的边界之间的更改日志条目。|
|\<FileRef>|指定枚举将开始在卷上的文件中的序号位置。|
|\<LowUSN>|指定用来筛选返回的记录的范围的 USN 值的下限。 仅记录其最后的更改日志 USN 是之间或等于*LowUSN*并*HighUSN*返回成员值。|
|\<HighUSN>|指定用来筛选返回的文件的范围的 USN 值的上限。|
|queryjournal|查询卷的 USN 数据，以收集有关当前变更日志，记录和其容量的信息。|
|readdata|读取文件的 USN 数据。|
|\<FileName>|指定的文件，例如包括文件名和扩展名的完整路径：C:\documents\filename.txt|
|readjournal|读取 USN 日志中的 USN 记录。|
|minver=\<number>|USN_RECORD 要返回的最小主版本。 默认值 = 2。|
|maxver=\<number>|USN_RECORD 要返回的最大主版本。 默认值 = 4。|
|startusn =\<USN 数 >|若要开始阅读从 USN 日志的 USN。 默认值 = 0。|


## <a name="remarks"></a>备注

-   有关 USN 更改日志

    USN 更改日志提供持久卷上的文件所做的所有更改的日志。 添加、 删除和修改文件、 目录和其他 NTFS 对象时，NTFS 将记录输入到 USN 更改日志，一个用于在计算机上每个卷。 每条记录都指明了变更的类型以及所更改的对象。 新的记录被追加到流的末尾。

    程序可以查询 USN 更改日志，以确定对一组文件所做的所有修改。 USN 更改日志是比检查时间戳或注册的文件通知高效得多。 启用和索引服务、 文件复制服务 (FRS)、 远程安装服务 (RIS) 和远程存储使用 USN 更改日志。

-   使用**createjournal**

    如果变更日志的卷上已存在**createjournal**参数更新更改日志*MaxSize*并*AllocationDelta*参数。 这使您可以展开而无需将其禁用维护活动的日志的记录数。

-   使用**m**

    变更日志可以增长到大于此目标值，但在为此值小于下一个 NTFS 检查点处截断变更日志。 NTFS 检查变更日志和其大小超过的值时对其进行修剪*MaxSize*加上的值*AllocationDelta*。 在 NTFS 检查点，操作系统将记录写入到启用 NTFS 来确定哪些处理所需从故障中恢复的 NTFS 日志文件。

-   使用

    变更日志可以增长到的值的总和超过*MaxSize*并*AllocationDelta*之前要修剪。

-   使用**deletejournal**

    删除或禁用活动更改日志是非常耗时，因为系统必须访问主文件表 (MFT) 中的所有记录，并将最后一个 USN 属性设置为 0 （零）。 此过程可能需要几分钟，并且可以继续在系统重新启动后是否需要重新启动。 在此过程中，变更日志不被视为处于活动状态，也不被禁用。 系统正在禁用日志，而无法访问，与所有日志操作的都返回错误。 因为对日志使用其他应用程序产生负面影响，应禁用活动的日志时使用格外小心。

## <a name="BKMK_examples"></a>示例
若要创建 USN 更改日志在驱动器 C，类型：

```
fsutil usn createjournal m=1000 a=100 c:
```

若要删除活动 USN 更改日志在驱动器 C，类型：

```
fsutil usn deletejournal /d c:
```

若要启用跟踪与指定的块区大小和文件大小阈值的范围，请键入：

```
fsutil usn enablerangetracking c=16384 s=67108864 C:
```

若要枚举并列出驱动器 C 上的两个指定的边界之间的更改日志条目，请键入：

```
fsutil usn enumdata 1 0 1 c:
```

若要查询的驱动器 C 上的卷的 USN 数据，请键入：

```
fsutil usn queryjournal c:
```

若要读取的驱动器 C 上的 \Temp 文件夹中的文件的 USN 数据，请键入：

```
fsutil usn readdata c:\temp\sample.txt
```

若要阅读与特定的开始 USN USN 日志，请键入：

```
fsutil usn readjournal startusn=0xF00
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)


