---
title: 使用 Robocopy 为 DFS 复制预先播种文件
description: 如何使用 Robocopy 为 DFS 复制预先播种文件。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: ea5cd954dde6d4fa8fcaa7874f75cb9588115ab1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402128"
---
# <a name="use-robocopy-to-preseed-files-for-dfs-replication"></a>使用 Robocopy 为 DFS 复制预先播种文件

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

本主题说明如何使用命令行工具**Robocopy**在为 Windows Server 中的分布式文件系统（Dfs）复制（也称为 DFSR 或 DFS）设置复制时预先播种文件。 通过在设置 DFS 复制之前种子文件、添加新的复制伙伴或替换服务器，你可以加速初始同步，并在 Windows Server 2012 R2 中启用 DFS 复制数据库的克隆。 Robocopy 方法是以下几种种子方法之一：有关概述，请参阅[Step 1：预先播种 files for DFS 复制](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495046(v%3dws.11)>)。

Windows Server 附带了 Robocopy （稳健文件复制）命令行实用程序。 此实用程序提供了广泛的选项，其中包括复制安全性、备份 API 支持、重试功能和日志记录。 更高版本包括多线程和非缓冲 i/o 支持。

>[!IMPORTANT]
>Robocopy 不复制独占锁定的文件。 如果用户倾向于在文件服务器上长期锁定多个文件，请考虑使用不同的种子方法。 种子在源服务器和目标服务器上的文件列表之间不需要完全匹配项，但当对 DFS 复制执行初始同步时，不存在的文件越多，种子的效率就越低。 若要最大程度地减少锁冲突，请在组织的非高峰时段内使用 Robocopy。 请始终在种子后检查 Robocopy 日志，以确保你了解因排他锁而跳过了哪些文件。

若要使用 Robocopy 为 DFS 复制预先播种文件，请执行以下步骤：

1. [下载并安装最新版本的 Robocopy。](#step-1-download-and-install-the-latest-version-of-robocopy)
2. [要复制的稳定文件。](#step-2-stabilize-files-that-will-be-replicated)
3. [将复制的文件复制到目标服务器。](#step-3-copy-the-replicated-files-to-the-destination-server)

## <a name="prerequisites"></a>先决条件

由于种子不直接涉及 DFS 复制，因此你只需满足使用 Robocopy 执行文件复制的要求。

- 你需要一个帐户，该帐户是源服务器和目标服务器上本地管理员组的成员。

- 在将用于复制文件的服务器（源服务器或目标服务器）上安装最新版本的 Robocopy;你将需要安装最新版本的操作系统版本。 有关说明，请[参阅步骤2：要复制](#step-2-stabilize-files-that-will-be-replicated)的稳定文件。 除非您从运行 Windows Server 2003 R2 的服务器种子文件，否则您可以在源服务器或目标服务器上运行 Robocopy。 目标服务器通常是最新的操作系统版本，它使你能够访问最新版本的 Robocopy。

- 请确保目标驱动器上有足够的存储空间可用。 不要在要复制到的路径上创建文件夹：Robocopy 必须创建根文件夹。
    
    >[!NOTE]
    >确定要为 preseeded 文件分配多少空间时，请考虑预计的数据增长时间和 DFS 复制的存储要求。 有关规划帮助，请参阅[管理 DFS 复制](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754771(v=ws.11)>)中[的编辑暂存文件夹的配额大小和冲突和已删除文件夹](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754229(v=ws.11))。

- 在源服务器上，可以选择安装 "进程监视器" 或 "进程资源管理器"，您可以使用它来检查正在锁定文件的应用程序。 有关下载信息，请参阅[进程监视器](https://docs.microsoft.com/sysinternals/downloads/procmon)和[进程资源管理器](https://docs.microsoft.com/sysinternals/downloads/process-explorer)。

## <a name="step-1-download-and-install-the-latest-version-of-robocopy"></a>第 1 步：下载并安装最新版本的 Robocopy

在使用 Robocopy 预先播种文件之前，应下载并安装**Robocopy**的最新版本。 这可确保 DFS 复制由于 Robocopy 发货版本中的问题而不会跳过文件。

最新兼容的 Robocopy 版本的源取决于服务器上运行的 Windows Server 的版本。 有关使用最新版本的 Robocopy 为 Windows Server 2008 R2 或 Windows Server 2008 下载修补程序的信息，请参阅[Windows server 2008 和中的分布式文件系统（DFS）技术的当前可用修补程序列表Windows Server 2008 R2](https://support.microsoft.com/help/968429/list-of-currently-available-hotfixes-for-distributed-file-system-dfs-t)。

或者，你可以通过执行以下步骤，查找并安装适用于操作系统的最新修补程序。

### <a name="locate-and-install-the-latest-version-of-robocopy-for-a-specific-version-of-windows-server"></a>为特定版本的 Windows Server 查找并安装最新版本的 Robocopy

1. 在 web 浏览器中， [https://support.microsoft.com](https://support.microsoft.com/)打开。

2. 在 "**搜索支持**" 中，输入以下字符串`<operating system version>` ，将替换为相应的操作系统，然后按 enter 键：
    
    ```robocopy.exe kbqfe "<operating system version>"```
    
    例如，输入**robocopy kbqfe "Windows Server 2008 R2"** 。

3. 查找并下载具有最高 ID 编号（即最新版本）的修补程序。

4. 在服务器上安装该修补程序。

## <a name="step-2-stabilize-files-that-will-be-replicated"></a>步骤 2：要复制的稳定文件

在服务器上安装最新版本的 Robocopy 后，应使用下表中所述的方法阻止锁定文件阻止复制。 大多数应用程序不会以独占方式锁定文件。 但是，在正常操作期间，可能会在文件服务器上锁定一小部分文件。

|锁的源|说明|缓解|
|---|---|---|
|用户远程打开共享上的文件。|员工连接到标准文件服务器，并编辑文档、多媒体内容或其他文件。 有时称为传统主文件夹或共享数据工作负荷。|仅在非工作时间的非高峰时间执行 Robocopy 操作。 这会最大程度地减少种子期间 Robocopy 必须跳过的文件数。<br><br>请考虑在将使用 Windows PowerShell **SmbShareAccess**和**SmbSession** cmdlet 复制的文件共享上暂时设置只读访问权限。 如果为公用组（如 Everyone 或经过身份验证的用户）设置权限来读取，则标准用户可能不太可能使用独占锁定打开文件（如果在打开文件时其应用程序检测到只读访问权限）。<br><br>你还可以考虑将 SMB 端口445入站的临时防火墙规则设置为该服务器，以阻止对文件的访问或使用**SmbShareAccess** cmdlet。 但是，这两种方法都对用户操作有中断。|
|应用程序在本地打开文件。|在文件服务器上运行的应用程序工作负荷有时会锁定文件。|临时禁用或卸载正在锁定文件的应用程序。 您可以使用进程监视器或进程资源管理器来确定哪些应用程序正在锁定文件。 若要下载进程监视器或进程资源管理器，请访问[进程监视器](https://docs.microsoft.com/sysinternals/downloads/procmon)和[进程资源管理器](https://docs.microsoft.com/sysinternals/downloads/process-explorer)页。|

## <a name="step-3-copy-the-replicated-files-to-the-destination-server"></a>步骤 3:将复制的文件复制到目标服务器

最大程度地减少要复制的文件的锁定后，可将文件从源服务器预先播种到目标服务器。

>[!NOTE]
>您可以在源计算机或目标计算机上运行 Robocopy。 下面的过程介绍了如何在目标服务器上运行 Robocopy，这通常是运行较新的操作系统，以便充分利用最新操作系统可能提供的任何其他 Robocopy 功能。

### <a name="preseed-the-replicated-files-onto-the-destination-server-with-robocopy"></a>通过 Robocopy 将复制的文件预先播种到目标服务器

1. 使用在源服务器和目标服务器上本地管理员组的成员帐户登录到目标服务器。

2. 打开提升的命令提示符。

3. 若要将文件从源服务器预先播种到目标服务器，请运行以下命令，并将括号中的值替换为你自己的源、目标和日志文件路径：
    
    ```PowerShell
    robocopy "<source replicated folder path>" "<destination replicated folder path>" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:<log file path> /v
    ```
    
    此命令会将源文件夹的所有内容复制到目标文件夹，并具有以下参数：
    
    |参数|描述|
    |---|---|
    |"\<源已复制文件夹\>路径"|指定目标服务器上要预先播种的源文件夹。|
    |"\<目标已复制文件夹\>路径"|指定将存储 preseeded 文件的文件夹的路径。<br><br>目标文件夹不能已存在于目标服务器上。 若要获取匹配的文件哈希，Robocopy 在 preseeds 文件时必须创建根文件夹。|
    |/e|复制子目录及其文件以及空子目录。|
    |/b|在备份模式下复制文件。|
    |/copyall|复制所有文件信息，包括数据、属性、时间戳、NTFS 访问控制列表（ACL）、所有者信息和审核信息。|
    |/r：6|出现错误时，重试操作六次。|
    |/w：5|两次重试之间等待5秒。|
    |MT：64|同时复制64文件。|
    |/xd DfsrPrivate|排除 DfsrPrivate 文件夹。|
    |/tee|将状态输出写入控制台窗口以及日志文件。|
    |/log \<日志文件路径 >|指定要写入的日志文件。 覆盖文件的现有内容。 （若要将条目追加到现有日志文件，请`/log+ <log file path>`使用。）|
    |/v|生成包含跳过的文件的详细输出。|
    
    例如，以下命令将源复制文件夹（E：\\RF01）中的文件复制到目标服务器上的数据驱动器 D：
    
    ```PowerShell
    robocopy.exe "\\srv01\e$\rf01" "d:\rf01" /e /b /copyall /r:6 /w:5 /MT:64 /xd DfsrPrivate /tee /log:c:\temp\preseedsrv02.log
    ```
    
    >[!NOTE]
    >建议在使用 Robocopy 为 DFS 复制预先播种文件时使用上述参数。 但是，您可以更改其一些值或添加其他参数。 例如，你可能会发现测试是否具有为 */mt*参数设置较高值（线程计数）的能力。 此外，如果你主要复制较大的文件，则可以通过为无缓冲 i/o 添加 **/j**选项来提高复制性能。 有关 Robocopy 参数的详细信息，请参阅[robocopy](https://docs.microsoft.com/windows-server/administration/windows-commands/robocopy)命令行参考。

    >[!WARNING]
    >若要在使用 Robocopy 为 DFS 复制预先播种文件时避免潜在的数据丢失，请不要对建议的参数进行以下更改：
    >- 不要使用 */mir*参数（镜像目录树）或 */mov*参数（移动文件，然后从源中删除这些文件）。
    >-  请勿删除 **/e**、 **/b**和 **/copyall**选项。

4. 复制完成后，检查日志中是否存在任何错误或已跳过的文件。 使用 Robocopy 逐个复制任何跳过的文件，而不是正在整个文件集。 如果由于独占锁定而跳过了文件，请稍后尝试使用 Robocopy 复制单个文件，或者在初始同步期间通过 DFS 复制来接受这些文件将需要通过网络复制。

## <a name="next-step"></a>下一步

完成初始复制后，若要使用 Robocopy 解决所跳过的文件的问题，请使用 Windows PowerShell 中的**DfsrFileHash** Cmdlet 或**dfsrdiag.exe**命令通过比较源服务器和目标服务器上的文件哈希。 有关详细说明，请[参阅步骤2：验证 DFS 复制](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn495042(v%3dws.11)>)的 Preseeded 文件。
