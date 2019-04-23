---
title: 使用 Robocopy 预先播种为 DFS 复制的文件
description: 如何使用 Robocopy.exe 预先播种为 DFS 复制的文件。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: a7a14b1a1e0f91002b201869e4c68187ffaf3f8f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865078"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>使用 Robocopy 预先播种为 DFS 复制的文件

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012 中，Windows Server 2008 R2 和 Windows Server 2008

本主题说明如何使用命令行工具**Robocopy.exe**，以在 Windows Server 中设置的分布式文件系统 (DFS) 复制 （也称为 DFSR 或 DFS-R） 复制时，预先播种文件。 通过设置 DFS 复制之前 preseeding 文件，添加新的复制伙伴，或替换服务器，可以加速初始同步并启用 Windows Server 2012 R2 中 DFS 复制数据库的克隆。 Robocopy 方法是一种多个 preseeding 方法;有关概述，请参阅[步骤 1： 预先播种为 DFS 复制的文件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>)。

Robocopy （可靠文件副本） 命令行实用程序是包含在 Windows Server。 该实用工具提供了大量选项，包括复制安全、 备份 API 支持、 重试功能和日志记录。 更高版本中包括多线程处理和无缓冲 I/O 支持。

>[!IMPORTANT]
>Robocopy 不会复制以独占方式锁定的文件。 如果用户习惯于在文件服务器上长时间锁定多个文件，请考虑使用另一种 preseeding 方法。 Preseeding 不需要在源和目标服务器上的文件列表之间的完美匹配但不是存在时为 DFS 复制，那么有效 preseeding 执行初始同步的文件越多。 若要尽量减少锁冲突，请在非高峰时间内为你的组织使用 Robocopy。 始终在 preseeding 以确保您了解哪些文件已跳过由于排他锁后检查 Robocopy 日志。

若要使用 Robocopy 预先播种为 DFS 复制的文件，请执行以下步骤：

1. [下载并安装最新版本的 Robocopy。](#step-1:-download-and-install-the-latest-version-of-robocopy)
2. [稳定将复制的文件。](#step-2:-stabilize-files-that-will-be-replicated)
3. [将复制的文件复制到目标服务器。](#step-3:-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>先决条件

因为 preseeding 不直接涉及 DFS 复制，只需满足的要求执行使用 Robocopy 文件复制。

- 需要该帐户是源和目标服务器上的本地管理员组的成员。

- 将用于将文件复制在服务器上安装最新版本的 Robocopy，源服务器或目标服务器;你将需要安装最新版本的操作系统版本。 有关说明，请参阅[步骤 2:稳定将复制的文件](#step-2:-stabilize-files-that-will-be-replicated)。 除非 preseeding 文件从运行 Windows Server 2003 R2 的服务器，可以在源或目标服务器上运行 Robocopy。 目标服务器上，通常具有较新的操作系统版本，使您可以访问到最新版本的 Robocopy。

- 确保足够的存储空间可在目标驱动器上。 请勿在你打算将复制到的路径上创建一个文件夹：Robocopy 必须创建的根文件夹。
    
    >[!NOTE]
    >如果你决定为 preseeded 文件分配的空间大小，通过 DFS 复制的时间和存储要求考虑后数据预期的增长。 有关计划的帮助，请参阅[编辑暂存文件夹以及冲突和已删除文件夹的配额大小](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11))中[管理 DFS 复制](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>)。

- 在源服务器上，选择安装进程监视器或进程资源管理器，它可用于检查正在锁定文件的应用程序。 有关下载信息，请参阅[Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon)并[Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer)。

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>第 1 步：下载并安装最新版本的 Robocopy

在使用 Robocopy 预先播种文件之前，你应下载和安装最新版**Robocopy.exe**。 这可确保，DFS 复制不会跳过文件由于 Robocopy 的传送版本内的问题。

有关最新的兼容 Robocopy 版本的源取决于在服务器运行的 Windows Server 的版本。 有关下载 Robocopy for Windows Server 2008 R2 或 Windows Server 2008 的最新版本的修补程序的信息，请参阅[的 Windows Server 2008 中的分布式文件系统 (DFS) 技术的当前可用修补程序的列表和 Windows Server 2008 R2 中](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t)。

或者，可以找到并执行以下步骤来安装操作系统的最新修补程序。

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>查找并安装特定版本的 Windows Server 的最新版本的 Robocopy

1. 在 web 浏览器中，打开[ https://support.microsoft.com ](https://support.microsoft.com/)。

2. 在中**搜索支持**，输入以下字符串替换为`<operating system version>`替换相应的操作系统，然后按 Enter 键：
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    例如，输入**robocopy.exe kbqfe"Windows Server 2008 R2"**。

3. 查找并下载使用最高 ID 号 （即，最新版本） 的修补程序。

4. 在服务器上安装该修补程序。

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>步骤 2：稳定将复制的文件

在服务器上安装 Robocopy 的最新版本后，应防止通过使用下表中所述的方法阻止复制锁定的文件。 大多数应用程序不以独占方式锁定文件。 但是，在正常操作期间极少比例的文件可能被锁定的文件服务器上。

|锁的源|说明|缓解|
|---|---|---|
|用户远程打开共享上的文件。|员工连接到标准文件服务器和编辑文档、 多媒体内容或其他文件。 有时称为传统的主文件夹或共享的数据的工作负荷。|只能执行期间的 Robocopy 操作高峰，非工作时间。 这可以尽量降低 Robocopy preseeding 期间必须跳过的文件数。<br><br>请考虑暂时将通过使用 Windows PowerShell 复制文件共享上设置只读访问权限**授予 SmbShareAccess**并**关闭 SmbSession** cmdlet。 如果读取设置的一组常见如 Everyone 或 Authenticated Users 的权限，标准用户可能不太可能使用排他锁下打开文件 （如果他们的应用程序检测的只读访问权限时打开的文件）。<br><br>您还可以考虑临时防火墙规则设置为 SMB 端口 445 入站到该服务器以阻止对文件的访问或使用**块 SmbShareAccess** cmdlet。 但是，这两种方法是对用户操作非常造成的干扰。|
|应用程序打开本地文件。|有时在文件服务器上运行的应用程序工作负荷锁定文件。|暂时禁用或卸载正在锁定文件的应用程序。 可以使用进程监视器或进程资源管理器来确定哪些应用程序正在锁定文件。 若要下载进程监视器或进程资源管理器，请访问[Process Monitor](https://docs.microsoft.com/sysinternals/downloads/procmon)并[Process Explorer](https://docs.microsoft.com/sysinternals/downloads/process-explorer)页。|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>步骤 3:将复制的文件复制到目标服务器

最大程度减少将复制的文件上的锁后，可以预先播种到目标服务器中的源服务器的文件。

>[!NOTE]
>可以在源计算机或目标计算机上运行 Robocopy。 以下过程说明如何在目标服务器上，这通常运行较新操作系统的系统，以充分利用较新操作系统可能会提供任何其他 Robocopy 功能运行 Robocopy。

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>预先播种到目标服务器上，使用 Robocopy 复制的文件

1. 登录到目标服务器是源和目标服务器上的本地管理员组的成员的帐户。

2. 打开提升的命令提示符。

3. 若要预先播种从源到目标服务器的文件，运行以下命令，替换为你自己的源、 目标和用括号括起来的值的日志文件路径：
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    此命令将源文件夹的所有内容都复制到目标文件夹中，使用以下参数：
    
    |参数|描述|
    |---|---|
    |"\<源复制的文件夹路径\>"|指定要在目标服务器上预先播种的源文件夹。|
    |"\<目标复制文件夹路径\>"|指定将存储 preseeded 的文件的文件夹的路径。<br><br>目标文件夹必须已经存在目标服务器上。 若要匹配的文件哈希值，Robocopy 必须 preseeds 文件时创建的根文件夹。|
    |/e|将复制子目录及其文件，以及空的子目录。|
    |/b|在备份模式下将文件复制。|
    |/ copyal|将复制所有文件信息，包括数据、 属性、 时间戳、 NTFS 访问控制列表 (ACL)、 所有者信息和审核信息。|
    |/r:6|六次将重试该操作发生错误时。|
    |/w:5|等待 5 秒重试之间。|
    |MT:64|同时将 64 文件的复制。|
    |/xd DfsrPrivate|不包括 DfsrPrivate 文件夹。|
    |/tee|将状态输出写入到控制台窗口中，以及日志文件。|
    |/ 日志\<日志文件路径 >|指定要写入的日志文件。 将覆盖该文件的现有内容。 (若要附加到现有的日志文件条目，请使用`/log+ <log file path>`。)|
    |/v|生成详细输出，包括跳过的文件。|
    
    例如，以下命令会复制文件从复制的源文件夹中，e:\\RF01，到目标服务器上的数据驱动器 D:
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >我们建议使用上文所述，当你使用 Robocopy 预先播种为 DFS 复制的文件的参数。 但是，可以更改其值的一些或添加其他参数。 例如，你可能找出通过的测试，必须设置较高的值 （线程计数） 的容量 */MT*参数。 此外，如果主要将复制较大的文件，您可能能够通过添加提高复制性能 **/j**无缓冲 I/O 的选项。 有关 Robocopy 参数的详细信息，请参阅[Robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy)命令行参考。

    >[!WARNING]
    >若要使用 Robocopy 预先播种为 DFS 复制的文件时避免数据丢失，建议参数不请使用以下更改：
    >- 不要使用 */mir*参数 （即镜像目录树） 或 */mov*参数 （即移动文件，然后从源中删除）。
    >-  不要删除 **/e**， **/b**，并 **/copyall**选项。

4. 复制完成后，检查任何错误或跳过的文件的日志。 使用 Robocopy 复制而重新复制文件的整个集不是单独的任何已跳过的文件。 如果文件已跳过由于排他锁，尝试复制单个文件使用 Robocopy 更高版本，或接受这些文件，将需要在初始同步过程中由 DFS 复制的线上复制。

## <a name="next-step"></a>下一步

在完成初始复制，并使用 Robocopy 尽可能与尽可能多的已跳过文件解决问题后，将使用**Get DfsrFileHash**在 Windows PowerShell cmdlet 或**Dfsrdiag**命令通过比较源和目标服务器上的文件哈希值验证 preseeded 的文件。 有关详细说明，请参阅[步骤 2:为 DFS 复制验证 Preseeded 的文件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)。