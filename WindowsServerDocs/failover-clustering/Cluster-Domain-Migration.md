---
title: Windows Server 2016/2019 中的跨域群集迁移
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/18/2019
description: 本文介绍如何将 Windows Server 2019 群集从一个域迁移到另一个域
ms.localizationpriority: medium
ms.openlocfilehash: 68f49795124dedf0655726853a4d865686f6d697
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361407"
---
# <a name="failover-cluster-domain-migration"></a>故障转移群集域迁移

> 适用于：Windows Server 2019、Windows Server 2016

本主题概述了如何将 Windows Server 故障转移群集从一个域移到另一个域。

## <a name="why-migrate-between-domains"></a>为什么在域之间迁移

在某些情况下，需要将群集从一个 doamin 迁移到另一个。

- CompanyA 与 CompanyB 合并，并且必须将所有群集移到 CompanyA 域中
- 群集内置于主数据中心，并提供给远程位置
- 群集是作为工作组群集构建的，现在需要成为域的一部分
- 群集构建为域群集，现在需要成为工作组的一部分
- 群集正在移动到公司的一个区域，另一个是不同的子域

如果基础应用程序操作不受支持，Microsoft 不会为尝试将资源从一个域移到另一个域的管理员提供支持。 例如，Microsoft 不为尝试将 Microsoft Exchange 服务器从一个域移到另一个域的管理员提供支持。

   > [!WARNING]
   > 建议在移动群集之前，对群集中的所有共享存储执行完整备份。

## <a name="windows-server-2016-and-earlier"></a>Windows Server 2016 及更早版本

在 Windows Server 2016 及更早版本中，群集服务没有从一个域迁移到另一个域的功能。  这是因为增加了对 Active Directory 域服务和创建的虚拟名称的依赖。   

## <a name="options"></a>选项

为了执行此类迁移，有两个选项。

第一个选项涉及销毁群集并在新域中重新生成它。

![销毁并重新生成](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-1.gif)

如动画所示，此选项具有破坏性，步骤如下：

1. 销毁群集。
2. 将节点的域成员身份更改为新域。
3. 重新创建更新域中的新群集。  这需要重新创建所有资源。

第二个选项的破坏性较低，但需要额外的硬件，因为需要在新域中构建新群集。  一旦群集位于新域中，就运行群集迁移向导来迁移资源。 请注意，这不会迁移数据-需要使用其他工具来迁移数据，如[存储迁移服务](../storage/storage-migration-service/overview.md)（添加群集支持后）。

![生成和迁移](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-2.gif)

如动画所示，此选项不具有破坏性，但需要使用不同的硬件或现有群集中的节点，而不会被删除。

1. 创建新的 clusterin 新域，同时保持旧群集可用。
2. 使用[群集迁移向导](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754481(v=ws.10))将所有资源迁移到新群集。 提醒，此操作不会复制数据，因此需要单独执行此操作。
3. 停止或销毁旧群集。

在这两个选项中，新群集需要安装所有[群集感知应用程序](https://technet.microsoft.com/aa369082(v=vs.90))，驱动程序都是最新的，并且可能会进行测试，以确保所有这些应用程序正常运行。  如果还需要移动数据，这是一个耗时的过程。

## <a name="windows-server-2019"></a>Windows Server 2019

在 Windows Server 2019 中，我们引入了跨群集域迁移功能。  现在，可以轻松完成上面列出的方案，不再需要重新生成。  

从一个域移动群集是一个直接的过程。 为此，有两个新的 PowerShell commandlet。

**ClusterNameAccount** –在 Active Directory **ClusterNameAccount**中创建群集名称帐户–从 Active Directory 中删除群集名称帐户

完成此操作的过程是将群集从一个域更改为工作组并返回到新域。  不需要销毁群集、重建群集、安装应用程序等。 例如，如下所示：

![迁移](media/Cross-Domain-Cluster-Migration/Cross-Cluster-Domain-Migration-3.gif)

## <a name="migrating-a-cluster-to-a-new-domain"></a>将群集迁移到新域

在以下步骤中，群集将从 Contoso.com 域移动到新的 Fabrikam.com 域。  群集名称是*CLUSCLUS* ，并具有名为*CLUSCLUS*的文件服务器角色。

1. 在群集中的所有服务器上使用相同的名称和密码创建一个本地管理员帐户。  当服务器在域之间移动时，可能需要进行登录。
2. 使用对群集名称对象（CNO）具有 Active Directory 权限的域用户或管理员帐户登录到第一台服务器，虚拟计算机对象（VCO）有权访问群集，并打开 PowerShell。
3. 确保所有群集网络名称资源处于脱机状态，并运行以下命令。  此命令将删除群集可能具有的 Active Directory 对象。

   ```PowerShell
   Remove-ClusterNameAccount -Cluster CLUSCLUS -DeleteComputerObjects
   ```
4. 使用 Active Directory 用户和计算机，以确保删除与所有群集名称关联的 CNO 和 VCO 计算机对象。

   > [!NOTE]
   > 最好停止群集中所有服务器上的群集服务，并将服务启动类型设置为 "手动"，以便在更改域时服务器重新启动时，群集服务不会启动。

   ```PowerShell
   Stop-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Manual
   ```

5. 将服务器的域成员身份更改为工作组，重新启动服务器，将服务器加入到新域，然后重新启动。
6. 一旦将服务器置于新域中，请使用拥有创建对象的 Active Directory 权限的域用户或管理员帐户登录到群集，并打开 PowerShell。 启动群集服务，并将其重新设置为 "自动"。

   ```PowerShell
   Start-Service -Name ClusSvc

   Set-Service -Name ClusSvc -StartupType Automatic
   ```
7. 使群集名称和所有其他群集网络名称资源处于联机状态。

   ```PowerShell
   Start-ClusterResource -Name "Cluster Name"

   Start-ClusterResource -Name FS-CLUSCLUS
   ```

8. 使用关联的 active directory 对象将群集更改为新域的一部分。 为此，命令如下所示，网络名称资源必须处于 "联机" 状态。  此命令将执行的操作将 Active Directory 中的名称对象重新创建。

   ```PowerShell
   New-ClusterNameAccount -Name CLUSTERNAME -Domain NEWDOMAINNAME.com -UpgradeVCOs
   ```

    注意：如果没有包含网络名称的任何其他组（即仅具有虚拟机的 Hyper-v 群集），则不需要-UpgradeVCOs 参数开关。

9. 使用 Active Directory 用户和计算机检查新域，并确保创建了关联的计算机对象。 如果有，请将其余资源联机。

   ```PowerShell
   Start-ClusterGroup -Name "Cluster Group"

   Start-ClusterGroup -Name FS-CLUSCLUS
   ```

## <a name="known-issues"></a>已知问题

如果你使用的是新的 USB 见证功能，则将无法将群集添加到新域。  原因是文件共享见证类型必须使用 Kerberos 进行身份验证。  将见证服务器更改为 "无"，然后将群集添加到域。  完成后，重新创建 USB 见证。  你将看到以下错误：

```
New-ClusternameAccount : Cluster name account cannot be created.  This cluster contains a file share witness with invalid permissions for a cluster of type AdministrativeAccesssPoint ActiveDirectoryAndDns. To proceed, delete the file share witness.  After this you can create the cluster name account and recreate the file share witness.  The new file share witness will be automatically created with valid permissions.
```

