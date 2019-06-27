---
title: cleanmgr
description: 了解如何使用命令行选项来配置磁盘清理工具 (Cleanmgr.exe) 来自动清除某些文件。
ms.prod: windows-server-threshold
ms.reviewer: cosmosdarwin
author: iangpgh
ms.author: jgerend
manager: daveba
ms.technology: storage-spaces
ms.date: 06/20/2019
ms.openlocfilehash: fd722dda8476891860773126c84ed10f125715f0
ms.sourcegitcommit: 545dcfc23a81943e129565d0ad188263092d85f6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2019
ms.locfileid: "67407884"
---
# <a name="cleanmgr"></a>cleanmgr

> 适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 中，Windows Server 2008 R2 和 Windows Server （半年频道）

清除不必要的文件从您的计算机的硬盘。 命令行选项可用于指定 Cleanmgr 清理临时文件、 Internet 文件、 下载的文件和回收站文件。 然后可以计划要使用的计划任务工具在特定时间运行的任务。

有关如何使用此命令的示例，请参阅[示例](#examples)。

## <a name="syntax"></a>语法

```
cleanmgr [/d <driveletter>] [/sageset:n]  [/sagerun:n] [/TUNEUP:n] [/LOWDISK] [/VERYLOWDISK]
```

## <a name="parameters"></a>Parameters

|      参数      |    描述     |
| ------------------- | ------------------ |
|  /d \<driveletter>          | 指定所需磁盘清理清理的驱动器。<br>**注意：** 使用 /sagerun:n，不使用 /d 选项。 |
| /sageset:n | 显示磁盘清理设置对话框中，并还会创建注册表项来存储您选择的设置。 `n`值，该值存储在注册表中，可以指定用于运行磁盘清理任务。 `n`值可以是任何介于 0 和 65535 之间的整数值。 若要让所有可用的选项使用 /sageset 选项时，指定安装 Windows 的驱动器。  |
|  /sagerun:n  |  运行时使用 \sageset 选项分配给 n 值的指定的任务。 枚举在计算机上的所有驱动器并对每个驱动器运行所选配置文件。           |
| /TUNEUP:n    | 针对同一个运行 /sageset 和 /sagerun `n` 。 |
| /LOWDISK     | 使用默认设置运行。 |
| /VERYLOWDISK | 使用默认设置运行，没有用户提示。 |
| /?           | 显示帮助。 |

## <a name="options"></a>选项

可以使用 /sageset 和 /sagerun 对于磁盘清理指定的文件的选项包括：

- **安装程序的临时文件**-这些是由已停止运行的安装程序创建的文件。

- **下载的程序文件**-下载的程序文件是 ActiveX 控件和 Java 程序，查看某些页面时从 Internet 自动下载。 这些文件暂时存储在硬盘上的已下载的程序文件文件夹中。 此选项，以便磁盘清理删除它们之前，可以看到这些文件包括一个视图文件按钮。 此按钮将打开 C:\Winnt\Downloaded Program Files 文件夹。

- **临时 Internet 文件**-Temporary Internet Files 文件夹包含存储在以便快速查看您的硬盘的网页。 磁盘清理将删除这些页，但使您 Web 页的个性化的设置保持不变。 此选项还包括一个查看文件按钮，这将打开 C:\Documents and settings\ Settings\Temporary Internet Files\Content.IE5 文件夹。 

- **旧的 Chkdsk 文件**-当 Chkdsk 检查磁盘的错误，Chkdsk 可能在磁盘上作为根文件夹中的文件中保存丢失的文件碎片。 这些文件是不必要的。

- **回收站**-回收站包含已从计算机中删除的文件。 不会永久删除这些文件，直到清空回收站。 此选项包括一个查看文件按钮来打开回收站。 **注意：** 回收站中可能出现在多个驱动器，例如，不只是在 %systemroot%。

- **临时文件**-程序有时 Temp 文件夹中存储临时信息。 在程序退出之前，该程序通常会删除此信息。 您可以安全地删除不在过去一周内已修改的临时文件。

- **临时脱机文件**-临时脱机文件是最近使用的网络文件的本地副本。 这些文件是自动缓存，以便从网络断开连接后，可以使用它们。 查看文件按钮将打开脱机文件文件夹。

- **脱机文件**-脱机文件是你确实需要其可供脱机，以便从网络断开连接后，可以使用它们的网络文件的本地副本。 查看文件按钮将打开脱机文件文件夹。

- **旧文件压缩**-Windows 可以压缩文件最近使用的。 压缩文件可以节省磁盘空间，但您仍可以使用文件。 不删除任何文件。 因为不同的速率压缩文件，显示你将获得的磁盘空间量是一个近似值。 选项按钮允许您指定的磁盘清理将压缩的未使用的文件之前要等待的天数。

- **内容索引器的编录文件**-索引服务的速度并通过在磁盘上的文件的索引维护改进了文件搜索。 这些目录文件从以前的索引操作保持状态，并可以安全地删除。 **注意：** 目录文件可能会出现在多个驱动器，例如，不只是在 %systemroot%。

**请注意**如果指定清理包含 Windows 安装的驱动器，这些选项都可以在磁盘清理选项卡。如果指定任何其他驱动器，仅回收站和内容索引选项的目录文件是可在磁盘清理选项卡上。 

## <a name="examples"></a>示例

若要运行磁盘清理应用程序，以便您可以使用其对话框为更高版本，使用指定选项将设置保存到一组**1**，键入以下内容：

```
cleanmgr /sageset:1
```

若要运行磁盘清理并包括 cleanmgr /sageset:1 命令使用指定的选项，请键入：

```
cleanmgr /sagerun:1
```

若要运行```cleanmgr /sageset:1```和```cleanmgr /sagerun:1```一起，键入：

```
cleanmgr /tuneup:1
```

## <a name="additional-references"></a>其他参考

[释放在 Windows 10 中的驱动器空间](https://support.microsoft.com/en-us/help/12425/windows-10-free-up-drive-space)
