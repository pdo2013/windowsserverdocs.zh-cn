---
title: 存储
description: ''
author: JasonGerend
manager: elizapo
layout: LandingPage
ms.prod: windows-server-threshold
ms.technology: storage
ms.assetid: 6b74bc7c-a58d-4915-af8e-2cc27f2c4726
ms.topic: landing-page
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 03/08/2019
ms.openlocfilehash: d83d51ebf56d38f93c176d403ea5c6c14f625ee2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833798"
---
# <a name="storage"></a>存储

>适用于：Windows Server 2019，Windows Server 2016 中，Windows Server （半年频道）

>[!TIP]
> 要查找有关较旧版 Windows Server 的信息？ 在 docs.microsoft.com 上查看我们的其他 [Windows Server 库](/previous-versions/windows/)。 也可以[搜索此站点](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)了解具体信息。

<hr />
Windows Server 中的存储为专注虚拟化工作负载的软件定义数据中心 (SDDC) 客户提供了新的和改进的功能。 Windows Server 还为使用文件服务器处于现有工作负荷的企业客户提供广泛的支持。

<hr />
<ul class="cardsF panelContent">
<li>
 <a href="whats-new-in-storage.md">
                            <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                            <h2>新增功能</h2>
                                            <p>了解什么是 Windows Server 存储中的新增功能</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
<hr />
<ul class="cardsF panelContent">
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>适用于虚拟化工作负荷的软件定义存储</h3>
<HR />
                        <p><h3><a href="storage-spaces/storage-spaces-direct-overview.md">存储空间直通</a></h3> 直接连接的本地存储，包括 SATA 和 NVME 设备，以添加新的物理磁盘后优化磁盘使用情况并更快的虚拟磁盘修复时间。 另请参阅<a href="storage-spaces/overview.md">存储空间</a>了解有关共享 SAS 和独立存储空间的信息。</p>
<HR />
                        <p><h3><a href="storage-replica/storage-replica-overview.md">存储副本</a></h3> 群集或灾难预防准备和恢复，以及跨以实现高可用性站点延伸故障转移群集的服务器之间的存储不可知、 块级、 同步复制。 同步复制支持使用与崩溃时一致的卷镜像物理站点中的数据，以确保在文件系统级别的零数据损失。</p>
<HR />
                        <p><h3><a href="storage-qos/storage-qos-overview.md">存储服务质量 (QoS)</a></h3> 集中监视和管理自动使用 HYPER-V 和横向扩展文件服务器角色的虚拟机的存储性能 improveing 之间使用相同的文件服务器群集的多个虚拟机的存储资源公平度。</p>
<HR />
                        <p><h3><a href="data-deduplication/overview.md">重复数据删除</a></h3> 通过检查重复的卷上的数据来优化卷上的可用空间。 标识后，卷的数据集的重复部分只存储一次，并（可选）进行压缩以节省更多空间。 重复数据删除可优化冗余，且不会损坏数据的保真度或完整性。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>常规用途文件服务器</h3>
<HR />
                        <p><h3><a href="storage-migration-service/overview.md">存储迁移服务</a></h3>服务器迁移到较新版本的 Windows Server 使用一个图形工具，列出服务器上的数据的清单、 将数据和配置传输到较新的服务器，然后再根据需要将移动旧服务器的标识到新服务器因此该应用程序和用户无需更改任何内容。</p>
<HR />
                        <p><h3><a href="work-folders/work-folders-overview.md">工作文件夹</a></h3> 存储和访问工作上的个人计算机和设备，通常称为自带办公设备 (BYOD)，除公司电脑外的文件。 用户可以在方便的位置存储工作文件，随时随地访问这些文件。 通过在集中管理的文件服务器上存储文件，并有选择地指定用户设备策略（如加密和锁屏密码），组织可以维持对公司数据的控制。</p>
<HR />
                        <p><h3><a href="folder-redirection/folder-redirection-rup-overview.md">脱机文件和文件夹重定向</a></h3> 缓存以提高的速度和可用性的本地内容时重定向到网络位置、 本地文件夹 （例如文档文件夹中） 的路径。</p>
<HR />
                        <p><h3><a href="folder-redirection/deploy-roaming-user-profiles.md">漫游用户配置文件</a></h3> 将用户配置文件重定向到网络位置。</p>
<HR />
                        <p><h3><a href="dfs-namespaces/dfs-overview.md">DFS 命名空间</a></h3> 分组到一个或多个逻辑结构的命名空间位于不同服务器的共享的文件夹。 每个命名空间作为具有一系列子文件夹的单个共享文件夹显示给用户。 但是，命名空间的基本结构可以包含位于不同服务器以及多个站点中的大量文件共享。</p>
<HR />
                        <p><h3><a href="dfs-replication/dfsr-overview.md">DFS 复制</a></h3> 跨多个服务器和站点复制文件夹 （包括那些由 DFS 命名空间路径引用）。 DFS 复制使用一种称为远程差分压缩 (RDC) 的压缩算法。 RDC 检测对文件中数据的更改，并使 DFS 复制仅复制已更改文件块而非整个文件。</p>
<HR />
                        <p><h3><a href="fsrm/fsrm-overview.md">文件服务器资源管理器</a></h3> 管理和分类文件服务器上存储的数据。<p>
<HR />
                        <p><h3><a href="iscsi/iscsi-target-server.md">iSCSI 目标服务器</a></h3> 通过使用 Internet SCSI (iSCSI) 标准向网络上的其他服务器和应用程序提供块存储。</p>
<HR />
                       <p><h3><a href="iscsi/iscsi-boot-overview.md">iSCSI 目标服务器</a></h3> 可以启动数百台计算机从存储在一个集中位置中的一个操作系统映像。 这样可提高效率、可管理性、可用性以及安全性。</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
<li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage">
                            <img src="../media/i-store.svg" alt="" />
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>文件系统、协议等</h3>
<HR />
                        <p><h3><a href="refs/refs-overview.md">ReFS</a></h3> 弹性文件系统的最大数据可用性、 跨各种工作负载高效扩展到非常大的数据集并通过 （不管是软件或硬件故障） 的损坏恢复提供数据完整性。<p>
<HR />
                        <p><h3><a href="file-server/file-server-smb-overview.md">服务器消息块 (SMB) 协议</a></h3> 网络文件共享协议，允许读取和写入到文件，然后从计算机网络中的服务器程序请求服务的计算机上的应用程序。 SMB 协议可在其 TCP/IP 协议或其他网络协议上使用。 使用 SMB 协议时，应用程序（或应用程序用户）可访问远程服务器上的文件或其他资源。 这让应用程序可以读取、创建和更新远程服务器上的文件。 它还可以与任何设置为接收 SMB 客户端请求的服务器程序通信。<p>
<HR />
                        <p><h3><a href="storage-spaces/Storage-class-memory-health.md">存储类内存</a></h3> 提供性能类似于计算机内存 （非常快），但与常规存储驱动器的数据持久性。 Windows 将存储类内存视为与常规驱动器类似（只是速度更快），但设备运行状态的管理方式有一些不同。<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/cc766295(v=ws.10).aspx">BitLocker 驱动器加密</a></h3> 即使计算机被篡改或操作系统未运行时，将数据存储中以加密格式卷上。 这有助于保护计算机免受如下攻击：脱机攻击、禁用或绕过所安装操作系统进行的攻击，或以物理方式删除硬盘单独攻击数据的攻击。<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/dn466522(v=ws.11).aspx">NTFS</a></h3> 最新版本的 Windows 和 Windows Server 的主文件系统，提供了一套完整的功能，包括安全描述符、 加密、 磁盘配额以及丰富的元数据，并可用于与群集共享卷 (CSV) 提供持续可以从故障转移群集的多个节点同时访问的可用卷。<p>
<HR />
                        <p><h3><a href="https://technet.microsoft.com/library/jj592688(v=ws.11).aspx">网络文件系统 (NFS)</a></h3> 提供文件共享拥有 Windows 和非 Windows 计算机组成的异类环境的企业解决方案。<p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

---


## <a name="in-azure"></a>在 Azure 中

* [Azure 存储](https://azure.microsoft.com/documentation/services/storage/)
* [Azure StorSimple](https://www.microsoft.com/en-us/cloud-platform/azure-storsimple)
