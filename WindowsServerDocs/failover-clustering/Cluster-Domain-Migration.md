---
title: 跨域群集迁移 Windows Server 2016/2019年中
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: 本文介绍了将 Windows Server 2019 群集从一个域移动到另一个
ms.localizationpriority: medium
ms.openlocfilehash: bcfd458c94d33820f434cde3313dc069fc42ffd9
ms.sourcegitcommit: 21677706eb85cb1396c1f40bf443146c09ef1b0d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/31/2019
ms.locfileid: "9042031"
---
# 故障转移群集域迁移

> 适用于： Windows Server 2019、 Windows Server 2016

本主题提供从一个域群集到另一台的概述，以便从 Windows Server 故障转移。

## 为什么域之间迁移

有几种其中将群集从一个 doamin 迁移到另一个是必要的方案。

- 公司将与 CompanyB 合并，必须将所有群集都移到公司域
- 群集的主数据中心中构建和交付至远程位置
- 群集生成作为工作组群集以及现在必须是域的一部分
- 群集已生成为域群集，并且现在需要是工作组的一部分
- 群集移到另一个公司的一个区域，是不同子域

Microsoft 不会向尝试将资源从一个域移动到另一个不受支持的基础的应用程序操作是否管理员提供支持。 例如，Microsoft 不会向尝试从一个域的 Microsoft Exchange server 移动到另一台管理员提供支持。

   > [!WARNING]
   > 我们建议你执行在群集中所有共享存储的完整备份移动群集之前。

## Windows Server 2016 及更早版本

在 Windows Server 2016 及更早版本，群集服务没有从一个域移动到另一个功能。  这是由于增加依赖于 Active Directory 域服务和创建的虚拟名称。   

## 选项

若要执行此类移动，有两个选项。

销毁群集和重建新域中涉及到第一个选项。

![销毁并重新生成](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-1.gif)

如动画所示，此选项是破坏性的步骤所：

1. 销毁群集。
2. 更改到新域节点的域成员身份。
3. 重新创建更新的域中为新群集。  这需要无需重新创建所有资源。

第二个选项是破坏性较小，但需要额外的硬件，因为新的群集需要生成新的域中。  新域群集后，运行群集迁移向导将迁移资源。 请注意，这不会迁移数据-你将需要使用其他工具将迁移数据，例如[存储迁移服务](../storage/storage-migration-service/overview.md)（后添加群集支持）。

![生成和迁移](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-2.gif)

如动画所示，此选项不是破坏性，但确实需要不同的硬件或从现有的群集节点比已被删除。

1. 创建新 clusterin 同时仍有可用的旧群集新域。
2. 使用[群集迁移向导](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10))将所有资源迁移到新群集。 提醒，这不会复制数据，因此需要单独完成。
3. 解除授权或销毁旧群集。

在这两个选项中，新群集需要具有所有[群集的应用程序](https://technet.microsoft.com/aa369082(v=vs.90))安装，驱动程序所有最新的并且可能测试，以确保所有将正常运行。  如果数据还需要移动，这非常耗时的过程。

## Windows Server 2019

在 Windows Server 2019，我们引入了跨群集域迁移能力。  现在，可以轻松地完成上面列出的方案，不再需要重建的需求。  

从一个域移动群集是一个非常简单的过程。 若要实现此目的，有两个新 PowerShell commandlet。

**新建 ClusterNameAccount** – 在 Active Directory**删除 ClusterNameAccount**创建群集名称帐户 – 从 Active Directory 中删除群集名称帐户

若要完成此操作的过程是从一个域更改群集到工作组并返回到新域。  需要销毁群集，重新生成群集，安装应用程序等不要求。 例如，它将如下所示：

![迁移](media\Cross-Domain-Cluster-Migration\Cross-Cluster-Domain-Migration-3.gif)

## 将群集迁移到新域

在以下步骤中，群集是正在从 Contoso.com 域移动到新的 Fabrikam.com 域。  群集名称是*CLUSCLUS*和名为*FS CLUSCLUS*文件服务器角色。

1. 具有相同的名称和密码在群集中的所有服务器上创建本地管理员帐户。  这可能会需要登录，而服务器在域之间移动。
2. 登录到具有 Active Directory 权限到群集名称对象 (CNO)，虚拟计算机对象 (VCO) 域用户或管理员帐户的第一个服务器有权访问的群集和打开 PowerShell。
3. 确保所有群集网络名称资源都处于脱机状态并运行以下命令。  此命令将删除群集可能具有的 Active Directory 对象。

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. 使用 Active Directory 用户和计算机以确保 CNO 和 VCO 计算机与所有群集名称关联的对象已被删除。

   > [!NOTE]
   > 它是停止在群集中的所有服务器上的群集服务和手动设置服务启动类型，以便群集服务不会启动时更改域时重启服务器，一个好主意。

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. 更改到工作组服务器的域成员身份、 重启服务器、 将服务器加入到新的域，然后再次重新启动。
6. 新域服务器后，登录到具有 Active Directory 权限，以创建对象，但有权访问群集，域用户或管理员帐户的服务器，并打开 PowerShell。 开始菜单群集服务，并将其设置为自动。

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. 到联机状态使群集名称和所有其他群集网络名称资源。

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. 更改群集一部分相关联的 active directory 对象的新域。 若要执行此操作，下面的命令是和网络名称资源必须处于联机状态。  此命令将执行的操作是重新创建在 Active Directory 中的名称对象。

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    注意： 如果你没有任何其他组网络名称 （即仅虚拟机与 HYPER-V 群集），则不需要-UpgradeVCOs 参数开关。

9. 使用 Active Directory 用户和计算机检查新域，确保已创建的相关联的计算机对象。 如果已经分享过，然后将联机的组中剩余的资源。

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## 已知问题

如果你使用的新的 USB 见证功能，你将无法将群集添加到新的域。  原因是文件共享见证类型必须进行身份验证利用 Kerberos。  更改为无之前添加到域群集见证。  完成后，重新创建 USB 见证。  你将看到的错误是：

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

