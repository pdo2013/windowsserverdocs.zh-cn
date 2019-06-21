---
title: 跨域在 Windows Server 2016/2019年中的群集迁移
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: 本文介绍将 Windows Server 2019 群集从一个域移到另一个
ms.localizationpriority: medium
ms.openlocfilehash: 5d5aaa333d2e20fa25e4738e343f326d63f75c6b
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280214"
---
# <a name="failover-cluster-domain-migration"></a>故障转移群集域迁移

> 适用于：Windows Server 2019、Windows Server 2016

本主题提供了从一个域群集之间的面向移动 Windows Server 故障转移的概述。

## <a name="why-migrate-between-domains"></a>为什么在域之间迁移

有几种方案，其中将群集从一个 doamin 迁移到另一个是有必要。

- 公司将与 CompanyB 合并，必须将所有群集都移到公司域
- 在主数据中心内生成并发送到远程地点群集
- 群集作为工作组群集构建的现在需要域的一部分
- 群集作为域的群集而构建，现在需要工作组的一部分
- 群集移到一个区域到另一个公司，是不同的子域

Microsoft 不会尝试将资源从一个域移到另一个如果基础的应用程序操作不受支持的管理员提供支持。 例如，Microsoft 不会尝试将 Microsoft Exchange server 从一个域移到另一个管理员提供支持。

   > [!WARNING]
   > 我们建议在移动群集之前，在群集中执行完整备份的所有共享存储。

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 及更早版本

在 Windows Server 2016 及更早版本，群集服务没有从一个域移动到另一个的能力。  这是由于对 Active Directory 域服务和创建的虚拟名称的增加依赖。   

## <a name="options"></a>选项

若要执行这种移动操作，有两个选项。

第一个选项涉及销毁群集和重新生成新的域中。

![销毁并重新生成](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

如动画所示，此选项是破坏性的步骤要：

1. 销毁该群集。
2. 更改到新域的域成员身份的节点。
3. 重新创建已更新的域中为新群集。  这将需要无需重新创建所有资源。

第二个选项是破坏性较小，但需要额外的硬件，如新的群集需要生成新的域中。  群集后在新域中，运行群集迁移向导迁移资源。 请注意，这不会将数据迁移-将需要使用另一种工具来迁移数据，如[存储迁移服务](../storage/storage-migration-service/overview.md)（一旦添加了群集支持）。

![生成并迁移](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

如动画所示，此选项不是破坏性，但确实需要不同的硬件或从现有群集节点不是已删除。

1. 同时仍可在旧群集中创建新 clusterin 新域。
2. 使用[群集迁移向导](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10))将迁移到新群集的所有资源。 请注意，这不复制数据，因此将需要单独完成。
3. 解除授权或销毁旧群集。

在这两个选项，在新群集将需要具有所有[群集感知应用程序](https://technet.microsoft.com/aa369082(v=vs.90))安装，驱动程序最新版本，并且可能测试，以确保所有将正常运行。  如果还需要移动数据，这是一个耗时的过程。

## <a name="windows-server-2019"></a>Windows Server 2019

在 Windows Server 2019，我们引入了跨群集域迁移功能。  现在，可以轻松地完成上面列出的方案，并且不再需要重新生成的需求。  

从一个域中移动群集是一个简单直接的过程。 若要实现此目的，有两个新的 PowerShell commandlet。

**新 ClusterNameAccount** – Active Directory 中创建群集名称帐户**删除 ClusterNameAccount** – 从 Active Directory 中删除群集名称帐户

若要完成此过程是从一个域更改群集到工作组并返回到新域。  要销毁群集、 重新生成群集，请安装应用程序，等等的要求不是一项要求。 例如，它将类似如下：

![迁移](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>将群集迁移到新域

在以下步骤中，群集正在从移动 Contoso.com 域到新的 Fabrikam.com 域。  群集名称是*CLUSCLUS*和名为的文件服务器角色*FS CLUSCLUS*。

1. 创建具有相同名称和密码在群集中的所有服务器上的本地管理员帐户。  这可能需要登录，而服务器在域之间移动。
2. 登录到第一个服务器与已到群集名称对象 (CNO)，虚拟计算机对象 (VCO) 的 Active Directory 权限的域用户或管理员帐户有权访问群集，并打开 PowerShell。
3. 确保所有群集网络名称资源都处于脱机状态并运行以下命令。  此命令将删除该群集可能具有的 Active Directory 对象。

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. 使用 Active Directory 用户和计算机以确保所有群集的名称与关联的对象已删除的 CNO 和 VCO 计算机。

   > [!NOTE]
   > 它是在群集中的所有服务器上停止群集服务和服务启动类型设置为手动，以便群集服务在服务器会更改域时重新启动时不会启动一个好办法。

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. 将服务器的域成员身份更改为工作组、 重启服务器，将服务器加入到新域和再次重启。
6. 新的域服务器后，登录到服务器上创建对象的 Active Directory 权限，有权访问该群集的域用户或管理员帐户并打开 PowerShell。 启动群集服务，并将其设置为自动。

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. 将群集名称和其他群集网络名称资源到联机状态。

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. 更改群集处于与关联的 active directory 对象的新域的一部分。 若要执行此操作，下面的命令是和网络名称资源必须处于联机状态。  此命令将执行的操作是重新创建命名 Active Directory 中的对象。

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    注意：如果没有具有网络名称 （即具有只有虚拟机的 HYPER-V 群集） 的任何其他组，则不需要-UpgradeVCOs 参数开关。

9. 使用 Active Directory 用户和计算机来检查新的域，并确保已创建了关联的计算机对象。 如果他们有帐户，则将剩余的资源组联机。

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>已知问题

如果使用新的 USB 见证服务器功能，您将不能将群集添加到新域。  原因在于文件共享见证服务器类型必须进行身份验证使用 Kerberos。  更改为无之前添加到域的群集的见证服务器。  完成后，重新创建 USB 见证服务器。  你将看到的错误是：

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

