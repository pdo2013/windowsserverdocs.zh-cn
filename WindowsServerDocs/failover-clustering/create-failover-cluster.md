---
title: 创建故障转移群集
description: 如何为 Windows Server 2012 R2、Windows Server 2012、Windows Server 2016 和 Windows Server 2019 创建故障转移群集。
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 496ae061d438a429ba13ee49d9da94127f308f37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369796"
---
# <a name="create-a-failover-cluster"></a>创建故障转移群集

>适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012

本主题介绍如何通过使用故障转移群集管理器管理单元或 Windows PowerShell 创建故障转移群集。 该主题包含一个典型的部署，其中群集及其相关联的群集角色的计算机对象在 Active Directory 域服务 (AD DS) 中创建。 如果要部署存储空间直通群集，请参阅[部署存储空间直通](../storage/storage-spaces/deploy-storage-spaces-direct.md)。

你还可以部署 Active Directory 分离的群集。 通过此部署方法可创建故障转移群集，无需拥有在 AD DS 中创建计算机对象的权限，也无需请求在 AD DS 中预留计算机对象。 此选项仅通过 Windows PowerShell 提供，并且仅建议用于特定方案。 有关详细信息，请参阅[部署与 Active Directory 分离的群集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

#### <a name="checklist-create-a-failover-cluster"></a>清单：创建故障转移群集

| 状态 | 任务 | 参考 |
| ---    | ---  | ---       |
| ☐    | 验证先决条件 | [验证先决条件](#verify-the-prerequisites) |
| ☐    | 在你想要添加为群集节点的每个服务器上安装故障转移群集功能 | [安装故障转移群集功能](#install-the-failover-clustering-feature) |
| ☐    | 运行群集验证向导来验证配置 | [验证配置](#validate-the-configuration) |
| ☐ | 运行创建群集向导来创建故障转移群集 | [创建故障转移群集](#create-the-failover-cluster) |
| ☐ | 创建群集角色来托管群集工作负载 | [创建群集角色](#create-clustered-roles) |

## <a name="verify-the-prerequisites"></a>验证先决条件

在开始之前，验证以下先决条件：

- 请确保你想要添加为群集节点的所有服务器都运行相同版本的 Windows Server。
- 检查硬件要求，以确保你的配置受支持。 有关详细信息，请参阅 [Failover Clustering Hardware Requirements and Storage Options](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11))。 如果要创建存储空间直通群集，请参阅[存储空间直通硬件要求](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)。
- 若要在群集创建过程中添加群集存储，请确保所有服务器均可访问存储。 （你还可以在创建群集之后添加群集存储。）
- 请确保将你想要添加为群集节点的所有服务器都加入到相同的 Active Directory 域中。
- （可选）创建组织单位 (OU)，并将你想要添加为群集节点的服务器的计算机帐户移动到该 OU 中。 作为最佳做法，我们建议将故障转移群集放置在 AD DS 中它们各自的 OU 中。 这可以帮助你更好地控制哪些组策略设置或安全模板设置会影响群集节点。 通过在它们各自的 OU 中隔离群集，它还帮助防止意外删除群集计算机对象。

此外，请验证以下帐户要求：

- 请确保你想要用于创建群集的帐户是域用户，该用户在你要添加为群集节点的所有服务器上具有管理员权限。
- 请确保满足以下任一情况：
    - 创建群集的用户具有向 OU 或将形成群集的服务器所在的容器 **创建计算机对象** 权限。
    - 如果用户没有**创建计算机对象**权限，则要求域管理员为该群集预留群集计算机对象。 有关详细信息，请参阅[在 Active Directory 域服务中预留群集计算机对象](prestage-cluster-adds.md)。

> [!NOTE]
> 如果要在 Windows Server 2012 R2 中创建 Active Directory 分离的群集，则此要求不适用。 有关详细信息，请参阅[部署与 Active Directory 分离的群集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

## <a name="install-the-failover-clustering-feature"></a>安装故障转移群集功能

必须在你想要添加为故障转移群集节点的每个服务器上安装故障转移群集功能。

### <a name="install-the-failover-clustering-feature"></a>安装故障转移群集功能

1. 启动“服务器管理器”。
2. 在 "**管理**" 菜单上，选择 "**添加角色和功能**"。
3. 在 "**开始之前**" 页上，选择 "**下一步**"。
4. 在 "**选择安装类型**" 页上，选择 "**基于角色或基于功能的安装**"，然后选择 "**下一步**"。
5. 在 "**选择目标服务器**" 页上，选择要安装该功能的服务器，然后选择 "**下一步**"。
6. 在 "**选择服务器角色**" 页上，选择 "**下一步**"。
7. 在“选择功能”页面上，选中“故障转移群集”复选框。
8. 若要安装故障转移群集管理工具，请选择 "**添加功能**"，然后选择 "**下一步**"。
9. 在 "**确认安装选择**" 页上，选择 "**安装**"。
<br>无需为故障转移群集功能重新启动服务器。

10. 安装完成后，选择 "**关闭**"。
11. 在你想要添加为故障转移群集节点的每个服务器上重复此过程。

> [!NOTE]
> 安装故障转移群集功能之后，我们建议你从 Windows 更新应用最新更新。 此外，对于基于 Windows Server 2012 的故障转移群集，请查看[建议的修补程序和更新，了解基于 Windows server 2012 的故障转移群集](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove)Microsoft 支持部门一文，并安装应用的任何更新。

## <a name="validate-the-configuration"></a>验证配置

在创建故障转移群集之前，我们强烈建议你验证配置，以确保硬件和硬件设置与故障转移群集兼容。 仅当完整配置通过所有验证测试，并且针对运行群集节点的 Windows Server 版本认证了所有硬件时，Microsoft 才支持群集解决方案。

> [!NOTE]
> 必须至少有两个节点才能运行所有测试。 如果你只有一个节点，许多关键存储测试都不能运行。

### <a name="run-cluster-validation-tests"></a>运行群集验证测试

1. 在已从远程服务器管理工具安装了故障转移群集管理工具的计算机上，或在安装了故障转移群集功能的服务器上，启动故障转移群集管理器。 若要在服务器上执行此操作，请启动服务器管理器，然后在 "**工具**" 菜单上，选择 "**故障转移群集管理器**"。
2. 在**故障转移群集管理器**窗格中的 "**管理**" 下，选择 "**验证配置**"。
3. 在 "**开始之前**" 页上，选择 "**下一步**"。
4. 在 "**选择服务器或群集**" 页上的 "**输入名称**" 框中，输入你计划添加为故障转移群集节点的服务器的 NetBIOS 名称或完全限定的域名，然后选择 "**添加**"。 为要添加的每台服务器重复此步骤。 若要同时添加多台服务器，请用逗号或分号分隔这些名称。 例如，以 `server1.contoso.com, server2.contoso.com` 格式输入名称。 完成后，选择 "**下一步**"。
5. 在 "**测试选项**" 页上，选择 "**运行所有测试（推荐）** "，然后选择 "**下一步**"。
6. 在 "**确认**" 页上，选择 "**下一步**"。

    验证页面显示运行测试的状态。
7. 在“摘要”页面上，执行以下任一操作：
    
      - 如果结果指示测试已成功完成且配置适用于群集，并且你想要立即创建群集，请确保选中 "**使用已验证的节点创建群集**" 复选框，然后选择 "**完成**"。 然后，继续执行[创建故障转移群集](#create-the-failover-cluster)过程的步骤 4。
      - 如果结果指示出现警告或失败，请选择 "**查看报告**" 以查看详细信息并确定必须更正的问题。 请注意，特定验证测试的警告指示可以支持故障转移群集的这个方面，但是可能不符合推荐的最佳做法。
        
        > [!NOTE]
        > 如果你收到“验证存储空间永久保留”测试的警告，请参阅博客文章 [Windows 故障转移群集验证警告指示你的磁盘不支持存储空间的永久保留](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) 以获取详细信息。

有关硬件验证测试的详细信息，请参阅 [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

## <a name="create-the-failover-cluster"></a>创建故障转移群集

若要完成此步骤，确保登录的用户帐户满足本主题的[验证先决条件](#verify-the-prerequisites)部分中概述的要求。

1. 启动“服务器管理器”。
2. 在 "**工具**" 菜单中，选择**故障转移群集管理器**。
3. 在**故障转移群集管理器**窗格中的 "**管理**" 下，选择 "**创建群集**"。
    
    将会打开“创建群集向导”。
4. 在 "**开始之前**" 页上，选择 "**下一步**"。
5. 如果显示 "**选择服务器**" 页，则在 "**输入名称**" 框中，输入你计划添加为故障转移群集节点的服务器的 NetBIOS 名称或完全限定的域名，然后选择 "**添加**"。 为要添加的每台服务器重复此步骤。 若要同时添加多台服务器，请用逗号或分号分隔这些名称。 例如，采用格式 *server1.contoso.com; server2.contoso.com*输入名称。 完成后，选择 "**下一步**"。
    
    > [!NOTE]
    > 如果在[配置验证过程](#validate-the-configuration)中运行验证后立即选择创建群集，将不会看到 "**选择服务器**" 页。 已验证的节点会自动添加到创建群集向导中，以便你无需再次输入它们。
6. 如果你提前跳过验证，则会出现“验证警告” 页面。 我们强烈建议你运行群集验证。 Microsoft 仅支持通过所有验证测试的群集。 若要运行验证测试，请选择 **"是"** ，然后选择 "**下一步**"。 完成验证配置向导，如[验证配置](#validate-the-configuration)中所述。
7. 在“用于管理群集的访问点”页面上，执行以下操作：
    
    1. 在“群集名称”框中，输入你要用于管理群集的名称。 在执行此操作之前，请查看以下信息：
        
          - 在群集创建期间，在 AD DS 中将此名称注册为群集计算机对象（也称为 *群集名称对象* 或 *CNO*）。 如果为群集指定 NetBIOS 名称，则在群集节点的计算机对象所在的同一位置中创建 CNO。 这可以是默认的计算机容器或 OU。
          - 若要为 CNO 指定不同的位置，你可以在“群集名称” 框中输入 OU 的可分辨名称。 例如：*CN=ClusterName, OU=Clusters, DC=Contoso, DC=com*。
          - 如果域管理员在与群集节点所在的不同 OU 中预留了 CNO，则指定域管理员提供的可分辨名称。
    2. 如果该服务器没有配置为使用 DHCP 的网络适配器，则必须为该故障转移群集配置一个或多个静态 IP 地址。 选中你想要用于群集管理的每个网络旁边的复选框。 选择所选网络旁边的 "**地址**" 字段，然后输入要分配给群集的 IP 地址。 此 IP 地址（或多个地址）将与域名系统 (DNS) 中的群集名称相关联。
    3. 完成后，选择 "**下一步**"。
8. 在“确认”页面上，查看这些设置。 默认情况下，选中“将所有符合条件的存储添加到群集” 复选框。 如果你想要执行以下任一操作，请清除此复选框：
    
      - 你想要稍后配置存储。
      - 你打算通过故障转移群集管理器或通过故障转移群集 Windows PowerShell cmdlet 创建群集存储空间，并且尚未在文件和存储服务中创建存储空间。 有关详细信息，请参阅[部署群集存储空间](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)。
9. 选择 "**下一步**" 以创建故障转移群集。
10. 在“摘要”页面上，确认已成功创建故障转移群集。 如果出现任何警告或错误，请查看摘要输出或选择 "**查看报告**" 以查看完整报告。 选择 "**完成**"。
11. 若要确认已创建群集，请验证该群集名称在导航树中的“故障转移群集管理器”下列出。 你可以展开群集名称，然后选择 "**节点**"、"**存储**" 或 "**网络**" 下的项以查看关联的资源。
    
    请注意，在 DNS 中成功复制群集名称可能需要花一些时间。 成功进行 DNS 注册和复制之后，如果选择 "服务器管理器中的"**所有服务器**"，则群集名称应作为**可管理性**状态为"**联机**"的服务器列出。

创建该群集后，你可以执行以下操作：验证群集仲裁配置和（可选）创建群集共享卷 (CSV)。 有关详细信息，请参阅[了解存储空间直通中的仲裁](../storage/storage-spaces/understand-quorum.md)和在[故障转移群集中使用群集共享卷](failover-cluster-csvs.md)。

## <a name="create-clustered-roles"></a>创建群集角色

创建故障转移群集后，你可以创建群集角色来托管群集工作负载。

>[!NOTE]
>对于需要客户端接入点的群集角色，在 AD DS 中创建虚拟计算机对象 (VCO)。 默认情况下，在 CNO 所在的相同容器或 OU 中创建群集的所有 VCO。 请注意，创建群集后，你可以将 CNO 移动到任何 OU。

下面介绍如何创建群集角色：

1. 使用服务器管理器或 Windows PowerShell 安装每个故障转移群集节点上的群集角色都需要的角色或功能。 例如，如果你想要创建群集文件服务器，则在所有群集节点上安装文件服务器角色。
    
    下表显示了可以在高可用性向导中配置的群集角色，以及必须作为先决条件安装的关联的服务器角色或功能。

   | 群集角色  | 角色或功能先决条件  |
   | ---------       | ---------                    |
   | 命名空间服务器     |   命名空间（文件服务器角色的一部分）       |
   | DFS 命名空间服务器     |  DHCP 服务器角色       |
   | 分布式事务处理协调器 (DTC)     | 无        |
   | 文件服务器     |  文件服务器角色       |
   | 通用应用程序     |  不适用       |
   | 通用脚本     |   不适用      |
   | 通用服务     |   不适用      |
   | Hyper-V 副本代理     |   Hyper-V 角色      |
   | iSCSI 目标服务器     |    iSCSI 目标服务器（文件服务器角色的一部分）     |
   | iSNS 服务器     |  iSNS 服务器服务功能       |
   | 消息队列     |  消息队列服务功能       |
   | 其他服务器     |  无       |
   | 虚拟机     |  Hyper-V 角色       |
   | WINS 服务器     |   WINS 服务器功能      |

2. 在故障转移群集管理器中，展开群集名称，右键单击 "**角色**"，然后选择 "**配置角色**"。
3. 按照“高可用性向导”中的步骤创建群集角色。
4. 若要验证是否已创建群集角色，请在“角色”窗格中，确保该角色的状态为“运行”。 “角色”窗格中还指示所有者节点。 若要测试故障转移，请右键单击该角色，指向 "**移动**"，然后选择 "**选择节点**"。 在 "**移动群集角色**" 对话框中，选择所需的群集节点，然后选择 **"确定"** 。 在“所有者节点” 列中，验证该所有者节点是否已更改。

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>通过使用 Windows PowerShell 来创建故障转移群集

以下 Windows PowerShell cmdlet 执行与本主题中前面的过程相同的功能。 在单行中输入每个 cmdlet，即使可能因格式限制而出现多行换行也是如此。

> [!NOTE]
> 必须使用 Windows PowerShell 在 Windows Server 2012 R2 中创建 Active Directory 分离的群集。 有关该语法的信息，请参阅 [Deploy an Active Directory-Detached Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

以下示例安装故障转移群集功能。

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

以下示例在名为 *Server1* 和 *Server2*的计算机上运行所有群集验证测试。

```PowerShell
Test-Cluster –Node Server1, Server2
```

> [!NOTE]
> **测试群集**cmdlet 会将结果输出到当前工作目录中的日志文件。 例如：C:\Users @ no__t-0username > \Appdata\local\temp。

以下示例创建一个名为 *MyCluster* 且带有节点 *Server1* 和 *Server2*的故障转移群集、分配静态 IP 地址 *192.168.1.12*，并将所有符合条件的存储添加到故障转移群集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

以下示例创建与前面的示例中相同的故障转移群集，但它不会将符合条件的存储添加到故障转移群集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

以下示例在域 *Contoso.com* 的 *Cluster* OU 中创建一个名为 *MyCluster*的群集。

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

有关如何添加群集角色的示例，请参阅 [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) 和 [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps)等主题。

## <a name="more-information"></a>详细信息

  - [故障转移群集](failover-clustering.md)
  - [部署 Hyper-v 群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [应用程序数据的横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [部署 Active Directory 分离的群集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [使用来宾群集实现高可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [群集感知更新](cluster-aware-updating.md)
  - [新群集](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [测试-群集](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
