---
ms.assetid: 4b844404-36ba-4154-aa5d-237a3dd644be
title: 数据重复删除概述
ms.technology: storage-deduplication
ms.prod: windows-server
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
ms.openlocfilehash: 1050c63d77db66c8e280ea1bea9503390c5d0bae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386297"
---
# <a name="data-deduplication-overview"></a>数据重复删除概述

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server （半年频道）、 

## <a name="what-is-dedup"></a>什么是重复数据删除？

重复数据删除（通常称为重复数据删除）是一项功能，可帮助降低冗余数据对存储成本的影响。 启用后，重复数据删除会检查卷上的数据（检查是否存在重复分区），优化卷上的可用空间。 卷数据集的重复分区只存储一次，并可以压缩，节省更多空间。 重复数据删除可优化冗余，而不会损坏数据保真度或完整性。 若要详细了解重复数据删除的工作原理，请参阅“[重复数据删除是如何工作的？](understand.md#how-does-dedup-work)”部分， 位于[了解重复数据删除](understand.md)页面。

> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334)包含用于重复数据删除的修补程序的汇总，包括重要的可靠性修补程序，我们强烈建议在将重复数据删除与 windows server 2016 和 windows server 2019 一起使用时安装。

## <a name="why-is-dedup-useful"></a>重复数据删除有用的原因是什么？

重复数据删除可帮助存储管理员降低重复数据的相关成本。 大型数据集通常具有 **<u>大量</u>** 重复数据，增加了数据的存储成本。 例如：

- 用户文件共享可能会有相同或类似文件的多个副本。
- 不同 VM 的虚拟化来宾可能几乎完全相同。
- 每天的备份快照差别可能非常小。

通过重复数据删除可以节省的空间取决于卷上的数据集或工作负荷。 重复率很高的数据集的优化率最高可达 95%，存储使用率最高降低 20 倍。 下表主要显示了各种内容类型的典型的重复数据删除节省情况：

| 应用场景       | 内容                                        | 典型的空间节省率 |
|----------------|------------------------------------------------|-----------------------|
| 用户文档 | Office 文档、照片、音乐、视频等  | 30-50%                |
| 部署共享 | 软件二进制文件、cab 文件、符号等 | 70-80%                |
| 虚拟化库 | ISO、虚拟硬盘文件等  | 80-95%                |
| 通用文件共享 | 上述全部                           | 50-60%                |

## <a id="when-can-dedup-be-used"></a>何时可以使用重复数据删除？  
<table>
    <tbody>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-clustered-gpfs.png" alt="Illustration of file servers" /></td>
            <td style="vertical-align:top">@no__t 0General 用途文件服务器 @ no__t-1<br />
常规用途文件服务器是常规使用的文件服务器，可能包含以下任意共享类型： <ul>
                    <li>团队共享</li>
                    <li>用户主页文件夹</li>
                    <li><a href="https://technet.microsoft.com/library/dn265974.aspx">工作文件夹</a></li>
                    <li>软件开发共享</li>
                </ul>
常规用途文件服务器非常适合进行重复数据删除，因为多个用户可能有同一个文件的许多副本或版本。 软件开发共享也适合进行重复数据删除，因为不同内部版本的许多二进制文件基本保持不变。 
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-vdi.png" alt="Illustration of VDI servers" /></td>
            <td style="vertical-align:top">@no__t 0Virtualized 桌面基础结构（VDI）部署 @ no__t-1<br />
VDI 服务器（如<a href="https://technet.microsoft.com/library/cc725560.aspx">远程桌面服务</a>为组织提供了一种向用户设置桌面的轻型选项。 对于一个组织而言，有很多原因要依赖于此类技术： <ul>
                    <li><b>应用程序部署</b>：可以在整个企业中快速部署应用程序。 如果你具有的应用程序经常更新、很少使用或难以管理，这项技术特别有用。</li>
                    <li><b>应用程序合并</b>：从一组集中管理的虚拟机中安装和运行应用程序时，无需在客户端计算机上更新应用程序。 还可以减少访问应用程序所需的网络带宽量。</li>
                    <li><b>远程访问</b>：用户可以从家庭计算机、展台、低功耗硬件和 Windows 以外的操作系统访问企业应用程序。</li>
                    <li><b>分支机构访问</b>：VDI 部署可为需要访问中心数据存储的分支机构工作人员提供更好的应用程序性能。 数据密集型应用程序有时没有针对低速连接进行优化的客户端/服务器协议。</li>
                </ul>
VDI 部署非常适合进行重复数据删除，因为驱动用户远程桌面的虚拟硬盘基本相同。 此外，重复数据删除还可帮助用户应对所谓的 <em>VDI 启动风暴</em>，即当多个用户在早上同时登录到各自的桌面时存储性能下降。
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-backup.png" alt="Illustration of backup applications" /></td>
            <td style="vertical-align:top">@no__t 0Backup 目标，如虚拟化备份应用程序 @ no__t-1<br />
备份应用程序（如 <a href="https://technet.microsoft.com/library/hh758173.aspx">Microsoft Data Protection Manager (DPM)</a>）极其适合进行重复数据删除，因为备份快照存在大量重复。
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-other.png" alt="Illustration of other workloads" /></td>
            <td style="vertical-align:top">
                <b>Other 工作负荷 @ no__t-1<br />
                <a href="install-enable.md#enable-dedup-candidate-workloads" data-raw-source="[Other workloads may also be excellent candidates for Data Deduplication](install-enable.md#enable-dedup-candidate-workloads)">其他工作负荷也可能极其适合进行重复数据删除</a>。
            </td>
        </tr>
    </tbody>
</table>
