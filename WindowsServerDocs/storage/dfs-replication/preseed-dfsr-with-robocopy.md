---
title: 使用 Robocopy preseed DFS 复制的文件
description: 如何使用 Robocopy.exe preseed DFS 复制的文件。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081903"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>使用 Robocopy preseed DFS 复制的文件

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

本主题介绍如何使用命令行工具， **Robocopy.exe**，preseed 文件时设置的 Windows Server 中的分布式文件系统 (DFS) 复制 （也称为 DFSR 或 DFS-R） 复制。 通过设置 DFS 复制之前，请 preseeding 文件，添加新复制伙伴，或更换服务器，可以提高初始同步速度和启用克隆的 Windows Server 2012 R2 中的 DFS 复制数据库。 Robocopy 方法是若干 preseeding 方法; 之一概述，请参阅[步骤 1: DFS 复制 preseed 文件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>)。

Robocopy （稳固文件复制） 命令行实用程序随 Windows Server。 实用程序提供了大量选项，包括复制安全、 备份 API 支持、 重试能力和日志记录。 更高版本包括多线程和取消缓冲的 I/O 支持。

>[!IMPORTANT]
>Robocopy 不会复制以独占方式锁定的文件。 如果用户往往文件服务器上的长锁定多个文件，请考虑使用不同的 preseeding 方法。 Preseeding 不需要完全一致之间文件列表的源和目标服务器上，但对于 DFS 复制，相对低效 preseeding 执行初始同步时不存在的多个文件。 若要最小化锁定冲突，请在非高峰时间为您的组织使用 Robocopy。 始终后 preseeding 以确保您了解哪些文件由于独占锁定而跳过检查 Robocopy 日志。

若要使用 Robocopy preseed DFS 复制的文件，请按照下列步骤：

1. [下载并安装 Robocopy 的最新版本。](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [预热将复制的文件。](#step-2:-stabilize-files-that-will-be-replicated)
3. [复制的文件复制到目标服务器。](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>先决条件

因为 preseeding 不直接涉及 DFS 复制，您只需满足的要求执行与 Robocopy 文件复制。

- 所需的源和目标服务器上本地 Administrators 组的成员的帐户。

- 将用于将文件复制的服务器上安装最新版本的 Robocopy — 源服务器或目标服务器。您将需要安装最新版本的操作系统版本。 有关说明，请参阅[步骤 2： 预热将复制的文件](#step-2:-stabilize-files-that-will-be-replicated)。 除非 preseeding 从运行 Windows Server 2003 R2 的服务器的文件，您可以在源或目标服务器上运行 Robocopy。 目标服务器上，该文件通常具有较新的操作系统版本，使您可以访问 Robocopy 的最新版本。

- 确保目标驱动器上有足够的存储空间。 不在您打算将复制到路径上创建文件夹： Robocopy 必须创建的根文件夹。
    
    >[!NOTE]
    >当您决定为 preseeded 文件分配的空间大小时，通过 DFS 复制的时间和存储要求考虑预期的数据增长。 有关规划帮助，请参阅[管理 DFS](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>)复制[编辑的临时文件夹以及冲突和删除文件夹的配额大小](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11))。

- 在源服务器上，也可以安装进程监视器或进程的浏览器，可用于检查正锁定文件的应用程序。 下载信息，请参阅[进程监视器](https://docs.microsoft.com/sysinternals/downloads/procmon)和[过程资源管理器](https://docs.microsoft.com/sysinternals/downloads/process-explorer)。

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>步骤 1： 下载并安装最新版本的 Robocopy

在使用 Robocopy preseed 文件之前，您应下载并安装**Robocopy.exe**的最新版本。 这样可确保的 DFS 复制不跳过文件由于 Robocopy 的传送版本中的问题。

最新的兼容 Robocopy 版本的源取决于在服务器运行的 Windows Server 的版本。 有关下载最新版本的 Robocopy for Windows Server 2008 R2 或 Windows Server 2008 的修补程序的信息，请参阅[目前修补程序分布式文件系统 (DFS) 技术和 Windows Server 2008 中的列表Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t)。

此外，您可以找到并按照以下步骤安装用于操作系统的最新修补程序。

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>找到并安装 Robocopy 的最新版本的 Windows Server 的特定版本

1. 在 web 浏览器中，打开[https://support.microsoft.com](https://support.microsoft.com/)。

2. 在**搜索支持**，输入以下字符串替换`<operating system version>`适当的操作系统，然后按 Enter 键：
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    例如，输入**robocopy.exe kbqfe 《 Windows Server 2008 R2"**。

3. 查找并下载具有最高的 ID 号 （即，最新版本） 的修补程序。

4. 在服务器上安装该修补程序。

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>步骤 2： 预热将复制的文件

在服务器上安装 Robocopy 的最新版本后，您应该防止锁定的文件阻止复制使用下表中所述的方法。 大多数应用程序不会以独占方式锁定文件。 但是，在正常操作期间少量文件可能已被锁定文件服务器上。

|锁定的内容源|说明|缓解|
|---|---|---|
|用户远程打开共享上的文件。|员工连接到标准文件服务器和编辑文档、 多媒体内容或其他文件。 有时称为传统的主文件夹或共享的数据工作负荷。|仅执行 Robocopy 操作期间非高峰，非营业时间。 这将最小化 Robocopy preseeding 期间必须跳过的文件数。<br><br>考虑暂时将使用 Windows PowerShell**授予 SmbShareAccess**和**关闭 SmbSession** cmdlet 复制的文件共享上设置只读访问权限。 如果将常见如每个人或 Authenticated Users 组的权限设置为读，标准用户可能不太可能与独占锁定打开文件 （如果其应用程序检测到的只读访问打开文件时）。<br><br>您可能还考虑设置临时防火墙规则 SMB 端口 445 入站到该服务器，以阻止对文件的访问或使用**块 SmbShareAccess** cmdlet。 但是，这两种方法是用户操作非常问题。|
|应用程序打开本地文件。|有时文件服务器上运行的应用程序工作负荷锁定文件。|暂时禁用或卸载正锁定文件的应用程序。 您可以使用进程监视器或过程资源管理器来确定哪些应用程序正锁定文件。 若要下载进程监视器或过程资源管理器，请访问[进程监视器](https://docs.microsoft.com/sysinternals/downloads/procmon)和[过程资源管理器](https://docs.microsoft.com/sysinternals/downloads/process-explorer)页。|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>步骤 3： 将复制的文件复制到目标服务器

减少上将复制的文件的锁定后，您可以 preseed 从源服务器到目标服务器的文件。

>[!NOTE]
>您可以在源计算机或目标计算机上运行 Robocopy。 下面的过程说明如何在目标服务器上，这通常运行较新操作系统，利用晚操作系统可能提供任何其他 Robocopy 功能运行 Robocopy。

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>Preseed 复制到目标服务器上使用 Robocopy 文件

1. 登录到目标服务器使用的源和目标服务器上本地 Administrators 组的成员的帐户。

2. 打开提升的命令提示符。

3. 若要 preseed 从源到目标服务器的文件，请运行以下命令，取代您自己的源、 目标和日志文件路径的括在括号中的值：
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    此命令将源文件夹的所有内容都复制到目标文件夹，使用以下参数：
    
    |参数|描述|
    |---|---|
    |"< 源已复制的文件夹 path\ > \"|指定要 preseed 目标服务器上的源文件夹。|
    |"\ < 目标复制文件夹 path\ >"|指定将存储 preseeded 的文件的文件夹的路径。<br><br>目标文件夹必须已存在的目标服务器上。 若要获取匹配文件哈希，Robocopy 必须时它 preseeds 文件创建的根文件夹。|
    |/e|将复制的子目录和其文件，以及空的子目录中。|
    |/b|备份模式复制文件。|
    |/ copyal|复制所有文件信息，包括数据、 属性、 时间戳、 NTFS 访问控制列表 (ACL)、 所有者信息和审核信息。|
    |/r:6|六次上重试该操作时出错。|
    |/w:5|等待 5 秒之间重试次数。|
    |MT:64|同时将复制 64 文件。|
    |/xd DfsrPrivate|排除 DfsrPrivate 文件夹。|
    |/tee|将状态输出写入到控制台窗口中，以及日志文件。|
    |记录 / \ < 日志文件路径 >|指定要写入的日志文件。 覆盖该文件的现有内容。 (若要追加到现有的日志文件的条目，使用`/log+ <log file path>`。)|
    |/v|生成包含详细输出跳过的文件。|
    
    例如，以下命令将复制文件从源复制文件夹，E:\\RF01，到目标服务器上的数据驱动器 D:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >我们建议您使用上述使用 Robocopy preseed DFS 复制的文件时的参数。 但是，您可以更改其值的一些或添加其他参数。 例如，您可能找出通过测试已设置为 */MT*参数更高的值 （线程计数） 的容量。 此外，如果将主要复制较大的文件，您可能能够通过添加无缓冲的 I/O 的 **/j**选项来提高副本性能。 有关 Robocopy 参数的详细信息，请参阅[Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy)命令行参考。

    >[!WARNING]
    >若要避免潜在数据丢失，使用 Robocopy preseed DFS 复制的文件时，不要建议参数进行以下更改：
    >- 不要使用 */mir*参数 （即镜像目录树） 或 */mov*参数 （即移动文件，然后从源中删除）。
    >-  不要删除 **/e**、 **/b**和 **/copyall**选项。

4. 复制完成后，请检查日志中的任何错误或跳过的文件。 使用 Robocopy 复制而重新复制整个一组文件不是单独的任何跳过的文件。 如果由于独占锁定而跳过文件，请尝试复制单个文件 Robocopy 更高版本，或接受这些文件需要按 DFS 复制初始同步过程中的线上复制。

## <a name="next-step"></a>下一步

在完成初始的副本，并使用 Robocopy 尽可能多个跳过的文件与解决问题后，您将使用 Windows PowerShell 或**Dfsrdiag**命令中的**Get DfsrFileHash** cmdlet 来进行比较，来验证 preseeded 的文件源和目标服务器上的文件哈希。 有关详细说明，请参阅[步骤 2： 验证 Preseeded 文件 DFS 复制](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)。