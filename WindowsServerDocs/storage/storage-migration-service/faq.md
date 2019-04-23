---
title: 存储迁移服务方面的常见问题 (FAQ)
description: 有关常见问题存储迁移服务，例如从一台服务器迁移到另一个时，哪些文件从传输中排除。
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 11/06/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: df03f722b7b36a163693f675a2eaade2fabeb82f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860908"
---
# <a name="storage-migration-service-frequently-asked-questions-faq"></a>存储迁移服务方面的常见问题 (FAQ)

本主题包含有关使用常见问题 (Faq) 解答[存储迁移服务](overview.md)将服务器迁移。

## <a name="excluded-files"></a> 从传输中排除哪些文件和文件夹？

存储迁移服务不会传输文件或文件夹，我们知道可能会影响 Windows 运行。 具体而言，以下是什么我们不会传输或移动到 PreExistingData 文件夹在目标上：

- Windows，程序文件，程序文件 (x86)，程序数据的用户
- $Recycle.bin recycler、 Recycled、 系统卷信息 $UpgDrv$，$SysReset，~ BT，$Windows.~ LS、 Windows.old、 启动、 恢复、 文档和设置 $Windows。
- pagefile.sys、 hiberfil.sys、 swapfile.sys、 winpepge.sys、 config.sys、 bootsect.bak、 bootmgr、 bootnxt
- 任何文件或文件夹与冲突的源服务器上排除的文件夹在目标上。 <br>例如，如果在源上没有 N:\Windows 文件夹，并获取映射到 C:\卷的目标上，它不会传输 — 而不考虑它所包含的内容 — 因为它会干扰目标上的 C:\Windows 系统文件夹。

## <a name="domain-migration"></a> 域迁移支持？

存储迁移服务不允许 Active Directory 域之间迁移。 服务器之间的迁移将始终将目标服务器加入到同一个域。 在 Active Directory 林中的不同域中，可以使用迁移凭据。 存储迁移服务支持工作组之间迁移。  

## <a name="cluster-support"></a> 为支持作为源或目标的群集？

存储迁移服务目前不会在 Windows Server 2019 群集之间迁移。 我们计划在未来版本的存储迁移服务中添加群集支持。

## <a name="local-principals"></a> 本地组和本地用户迁移？

存储迁移服务目前不会迁移本地用户或 Windows Server 2019 中的本地组。 我们计划添加本地用户和本地组迁移存储迁移服务的未来版本中支持。

## <a name="domain-controller"></a> 域控制器迁移支持？

存储迁移服务目前不会迁移 Windows Server 2019 中的域控制器。 解决方法是，前提是 Active Directory 域中具有多个域控制器，降级域控制器，然后迁移它，然后将提升目标转换完成后。 我们计划在未来版本的存储迁移服务中添加域控制器迁移支持。

## <a name="share-attributes"></a> 存储迁移服务迁移哪些属性？

存储迁移服务迁移所有标志、 设置和安全性的 SMB 共享。 标志，用于存储迁移服务将迁移该列表包括：

    - 共享状态
    - 可用性类型
    - 共享类型
    - 文件夹枚举模式 *（也称为基于访问的枚举或 ABE）*
    - 缓存模式
    - 释放模式
    - Smb 实例
    - CA 超时
    - 并发用户限制
    - 持续可用
    - 描述           
    - 对数据进行加密
    - 标识远程处理
    - 基础结构
    - 名称
    - 路径
    - 作用域
    - 作用域名称
    - 安全描述符
    - 卷影副本
    - 特殊
    - 临时

## <a name="move-db"></a> 我是否可以迁移存储迁移服务数据库？

存储迁移服务使用隐藏的 c:\programdata\microsoft\storagemigrationservice 文件夹中的默认情况下安装的可扩展存储引擎 (ESE) 数据库。 此数据库将随着添加作业，并且传输已完成，并可以在迁移后使用大量的驱动器空间数以百万计的文件，如果不删除作业。 如果需要移动数据库，请执行以下步骤：

1. 停止业务流程协调程序计算机上的"存储迁移服务"服务。
2. 取得所有权的`%programdata%/Microsoft/StorageMigrationService`文件夹
3. 添加你的用户帐户具有完全控制对此进行共享和所有其文件和子文件夹。
4. 将文件夹移动到另一个驱动器中，业务流程协调程序计算机上。
5. 设置以下注册表 REG_SZ 值：

    HKEY_Local_Machine\Software\Microsoft\SMS DatabasePath =*的其他卷上的新数据库文件夹路径*。 
6. 请确保系统具有完全控制权限的所有文件和文件夹的子文件夹
7. 删除你自己的帐户权限。
8. 启动"存储迁移服务"服务。

## <a name="transfer-threads"></a> 可以增加同时复制的文件的数？

存储迁移服务代理服务在某个给定的作业中同时复制 8 个文件。 此服务上运行业务流程协调程序在传输期间如果目标计算机位于 Windows Server 2012 R2 或 Windows Server 2016 中，但也在所有 Windows Server 2019 目标节点上运行。 可以通过调整以下注册表 REG_DWORD 值名称以十进制运行 SMS 代理每个节点上增加同时复制线程的数：

    HKEY_Local_Machine\Software\Microsoft\SMSProxy
    FileTransferThreadCount

 有效范围是 1 到 128 在 Windows Server 2019。 

 更改后必须重新上所有计算机 partipating 的存储迁移服务代理服务启动迁移。 我们计划此数和存储迁移服务的未来版本中。

## <a name="non-windows"></a> 可以迁移从 Windows Server 以外的源？

在 Windows Server 2019 中提供的存储迁移服务版本支持从 Windows Server 2003 和更高版本操作系统迁移。 在 Linux、 Samba、 NetApp、 EMC 或其他 SAN 和 NAS 存储设备，但当前不能迁移。 我们计划为实现这一初始 Linux Samba 支持的存储迁移服务的未来版本中。

## <a name="previous-versions"></a> 可以将迁移以前的文件版本？

在 Windows Server 2019 中提供的存储迁移服务版本不支持迁移以前版本 （使其与卷影复制服务） 的文件。 仅最新版本将迁移。 

## <a name="ntfs-refs"></a> 是否可以迁移从 NTFS 到 REFS？

在 Windows Server 2019 中提供的存储迁移服务版本不支持从 NTFS 迁移到 REFS 文件系统。 对 NTFS 和 REFS 为 ReFS，可以从 NTFS 进行迁移。 这是默认设置，由于功能、 元数据和 ReFS 不会重复从 NTFS 其他方面的许多差异。 ReFS 旨在作为应用程序工作负荷文件系统，不是常规文件系统。 有关详细信息，请参阅[复原文件系统 (ReFS) 概述](../refs/refs-overview.md)

## <a name="consolidate-servers"></a> 可以将多个服务器合并到一台服务器？

在 Windows Server 2019 中提供的存储迁移服务版本不支持将多个服务器整合到一台服务器。 合并的示例将迁移三个单独的源服务器-可能具有相同的共享名，并且到的虚拟化这些路径和共享，以防止任何重叠或冲突时，一台新服务器上的本地文件路径-然后回答所有这三个以前的服务器名称和 IP 地址。 存储迁移服务的未来版本中，我们可以添加此功能。  


## <a name="give-feedback"></a> 我要提供反馈、 报告 bug，或获取支持的选项有哪些？

若要提供有关存储迁移服务的反馈：

- 使用包含在 Windows 10 中，单击"建议功能"，并指定"Windows Server"的类别和子类别的"存储迁移"的反馈中心工具
- 使用[Windows Server UserVoice](https://windowsserver.uservoice.com)站点
- 电子邮件 smsfeed@microsoft.com

文件的 bug:

- 使用包含在 Windows 10 中，单击"报告问题"，并指定"Windows Server"的类别和子类别的"存储迁移"的反馈中心工具
- 通过支持案例[Microsoft 支持部门](https://support.microsoft.com)

若要获取支持：

 - 可以将问题发布[Windows Server 技术社区](https://techcommunity.microsoft.com/t5/Windows-Server/ct-p/Windows-Server)
 - 将它发布在[Windows Server 2019 Technet 论坛](https://social.technet.microsoft.com/Forums/en-US/home?forum=ws2019&filter=alltypes&sort=lastpostdesc) 
 - 通过支持案例[Microsoft 支持部门](https://support.microsoft.com)


## <a name="see-also"></a>请参阅

- [存储迁移服务概述](overview.md)
