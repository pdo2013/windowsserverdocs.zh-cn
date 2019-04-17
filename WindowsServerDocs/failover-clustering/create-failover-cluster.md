---
title: 创建故障转移群集
description: 如何为 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2016 中创建故障转移群集。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f919e69488c4f2272ddd07e535ba4e2248ddf79c
ms.sourcegitcommit: 5cbaca9685720f11d896c4ca167c86e74c032feb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2018
ms.locfileid: "6068975"
---
# 创建故障转移群集

>适用于： Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2016

本主题介绍了如何通过使用故障转移群集管理器管理单元或 Windows PowerShell 创建故障转移群集。 本主题介绍典型的部署中，其中群集和其关联的群集的角色的计算机对象就创建在 Active Directory 域服务 (AD DS)。 如果你正在部署存储空间直通群集改为看到[部署存储空间直通](../storage/storage-spaces/deploy-storage-spaces-direct.md)。

你还可以部署 Active Directory 分离的群集。 这种部署方法使你能够创建故障转移群集，而无需创建 AD DS 中的计算机对象或需要请求该对象预备 AD DS 中的计算机的权限。 此选项仅可通过 Windows PowerShell 中，并且仅适用于特定方案。 有关详细信息，请参阅[部署 Active Directory-Detached 群集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

#### 清单： 创建故障转移群集

|状态|任务|参考|
|:---:|---|---|
|☐|验证必备条件|[验证必备条件](#verify-the-prerequisites)|
|☐|在你想要添加作为群集节点的每个服务器上安装故障转移群集功能|[安装故障转移群集功能](#install-the-failover-clustering-feature)|
|☐|运行群集验证向导来验证配置|[验证配置](#validate-the-configuration)|
|☐|运行创建群集向导来创建故障转移群集|[创建故障转移群集](#create-the-failover-cluster)|
|☐|创建群集的角色到主机群集工作负荷|[创建群集的角色](#create-clustered-roles)|

## 验证必备条件

在开始之前，验证以下先决条件：

- 请确保你想要添加作为群集节点的所有服务器都运行相同版本的 Windows Server。
- 检查以确保你的配置受支持的硬件要求。 有关详细信息，请参阅[故障转移群集硬件要求和存储选项](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11))。 如果你要创建一个存储空间直通群集，请参阅[存储空间直通的硬件要求](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)。
- 若要在创建群集期间添加群集的存储，确保所有服务器都可以都访问存储。 （你还可以添加群集的存储在创建群集后。）
- 请确保你想要添加作为群集节点的所有服务器都加入同一个 Active Directory 域。
- （可选）创建组织单位 (OU) 和移动你想要作为群集节点添加到 OU 的服务器的计算机帐户。 作为最佳实践，我们建议将故障转移群集放置在 AD DS 中自己 OU。 这可以帮助你更好地控制哪些组策略设置或安全模板设置影响群集节点。 通过隔离自己 OU 中的群集，它还有助于防止意外删除群集计算机对象。

此外，验证以下帐户要求：

- 请确保你想要用于创建群集的帐户是你想要添加作为群集节点的所有服务器具有管理员权限的域用户。
- 请确保以下任一情况：
    - 创建群集的用户有权 OU 或将窗体群集服务器所在的容器之间的**创建计算机对象**。
    - 如果用户没有所**创建的计算机对象**的权限，请求域管理员预留群集计算机对象为群集。 有关详细信息，请参阅[Prestage Active Directory 域服务中的群集计算机对象](prestage-cluster-adds.md)。

>[!NOTE]
>不适用于此要求，如果你想要在 Windows Server 2012 R2 中创建一个 Active Directory 分离的群集。 有关详细信息，请参阅[部署 Active Directory-Detached 群集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

## 安装故障转移群集功能

你必须在你想要添加作为故障转移群集节点的每个服务器上安装故障转移群集功能。

### 安装故障转移群集功能

1. 启动服务器管理器。
2. 在**管理**菜单中，选择**添加角色和功能**。
3. 在**开始之前**页面上，选择**下一步**。
4. 在**选择安装类型**页面上选择**基于角色或基于功能的安装**，，然后选择**下一步**。
5. 在**选择目标服务器**页面中，选择想要安装的功能，的服务器，然后选择**下一步**。
6. 在**Select server roles**页上，选择**下一步**。
7. 在**Select features**页上选择**故障转移群集**复选框。
8. 若要安装的故障转移群集管理工具，选择**添加功能**，，然后选择**下一步**。
9. 在**确认安装选择**页面上，选择**安装**。
<br>重新启动服务器不需要故障转移群集功能。

10. 安装完成后，选择**关闭**。
11. 重复此过程在你想要作为故障转移群集节点添加每个服务器上。

>[!NOTE]
>安装故障转移群集功能后，我们建议你从 Windows 更新应用最新的更新。 此外，对于基于 Windows Server 2012 故障转移群集，查看[推荐修补程序和更新基于 Windows Server 2012 故障转移群集](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove)Microsoft 支持文章和安装应用的任何更新。

## 验证配置

创建故障转移群集之前，我们强烈建议你验证配置以确保硬件和硬件设置与故障转移群集兼容。 Microsoft 支持的群集解决方案，仅当完成配置通过所有验证测试和如果所有硬件已都认证的群集节点都在运行的 Windows Server 版本。

>[!NOTE]
>你必须具有至少两个节点运行所有测试。 如果你有只有一个节点，许多关键存储测试不运行。

### 运行群集验证测试

1. 在具有从远程服务器管理工具，安装的故障转移群集管理工具的计算机或服务器故障转移群集功能的安装位置上，开始菜单故障转移群集管理器。 若要执行此操作在服务器上，启动服务器管理器，然后选择**工具**菜单上的**故障转移群集管理器**。
2. 在**故障转移群集管理器**窗格中的在**管理**下，选择**验证配置**。
3. 在**开始之前**页面上选择**下一步**。
4. **选择服务器或群集**中的页面上， **Enter 名称**框中，输入的 NetBIOS 名称或你打算为故障转移群集节点中，添加服务器的完全限定的域名，然后选择**添加**。 你想要添加的每个服务器重复此步骤。 若要同时添加多个服务器，请在由逗号或分号分隔的名称。 例如，在格式*server1.contoso.com、 server2.contoso.com*中输入的名称。 完成后，选择**下一步**。
5. 在**测试选项**页上，选择**运行所有测试 （推荐）**，然后选择**下一步**。
6. 在**确认**页上，选择**下一步**。

    验证页显示正在运行的测试的状态。
7. 在**摘要**页面上，执行以下任一操作：
    
      - 如果结果表明已成功完成测试和配置适用于群集，并想要立即创建群集，请确保选择时**创建现在使用经过验证的节点的群集**复选框，然后选择**完成**。 然后，继续转到步骤 4 中[创建故障转移群集](#create-the-failover-cluster)过程。
      - 如果结果表明了警告或故障，选择**查看报告**以查看详细信息，并确定必须纠正哪些问题。 意识到特定验证测试一条警告，指示这一方面的故障转移群集可以受到支持，但可能无法满足推荐的最佳做法。
        
        >[!NOTE]
        >如果你收到一条警告，验证存储空间永久保留测试，请参阅博客文章[Windows 故障转移群集验证警告指示磁盘不支持的永久保留为存储空间](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/)以了解详细信息。

有关硬件验证测试的详细信息，请参阅[验证故障转移群集的硬件](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>)。

## 创建故障转移群集

若要完成此步骤，请确保为登录的用户帐户满足本主题的[验证必备条件](#verify-the-prerequisites)部分所述的要求。

1. 启动服务器管理器。
2. 在**工具**菜单中，选择**故障转移群集管理器**。
3. 在**故障转移群集管理器**窗格中的在**管理**下，选择**创建群集**。
    
    创建群集向导将打开。
4. 在**开始之前**页面上选择**下一步**。
5. 如果出现**选择服务器**页，请在**输入名称**框中，输入的 NetBIOS 名称或你打算为故障转移群集节点中，添加服务器的完全限定的域名，然后选择**添加**。 你想要添加的每个服务器重复此步骤。 若要同时添加多个服务器，单独的以逗号或分号的名称。 例如，在格式*server1.contoso.com; server2.contoso.com*中输入的名称。 完成后，选择**下一步**。
    
    >[!NOTE]
    >如果你选择要[配置验证过程](#validate-the-configuration)中运行验证后立即创建群集，你将不会看到**选择服务器**页。 已验证的节点会自动添加到创建群集向导，以便你无需重新输入它们。
6. 如果你跳过验证更早版本，不会显示**验证警告**页面。 我们强烈建议你运行群集验证。 Microsoft 支持仅通过所有验证测试的群集。 若要运行验证测试，选择****，，然后选择**下一步**。 [验证配置](#validate-the-configuration)中所述，请完成验证配置向导。
7. 在**管理群集的接入点**页面上，执行以下操作：
    
    1. 在**群集名称**框中，输入你想要用于管理群集的名称。 之前，请检查以下信息：
        
          - 群集在创建期间，此名称被注册为群集计算机对象 （也称为*群集名称对象*或*CNO*） 在 AD DS 中。 如果你指定群集的 NetBIOS 名称，群集节点的计算机对象驻留在相同位置创建 CNO。 这可以是默认计算机容器或 OU。
          - 若要为 CNO 中指定一个不同的位置，可以**群集名称**框中输入 OU 的可分辨的名称。 例如： *CN = ClusterName，OU = 群集，DC = Contoso，DC = com*。
          - 如果域管理员已预备 CNO 比驻留在群集节点的不同 OU 中，指定的可分辨的名称，域管理员提供。
    2. 如果服务器没有配置为使用 DHCP 的网络适配器，你必须配置一个或多个静态 IP 地址为故障转移群集。 选择你想要用于群集管理每个网络旁边的复选框。 选择**地址**字段中选择的网络，旁边，然后输入你想要分配到群集 IP 地址。 此 IP 地址 （或地址） 将与群集名称中域名系统 (DNS) 相关联。
    3. 完成后，选择**下一步**。
8. 在**确认**页面上查看的设置。 默认情况下，选择**添加到群集的所有有资格存储**复选框。 如果你想要执行以下任一操作，请清除此复选框：
    
      - 你想要更高版本配置存储。
      - 你打算创建群集的存储空间通过故障转移群集管理器或通过故障转移群集的 Windows PowerShell cmdlet 和尚未创建的存储空间中文件和存储服务。 有关详细信息，请参阅[部署群集存储空间](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>)。
9. 选择**下一步**创建故障转移群集。
10. 在**摘要**页面上，确认已成功创建故障转移群集。 如果没有任何警告或错误，查看摘要输出，或选择**查看报告**以查看完整报告。 选择**完成**。
11. 若要确认已创建群集，验证群集名称**故障转移群集管理器**下面列出在导航树中。 你可以展开群集名称，，然后选择项下**节点**、**存储**或**网络**，以查看关联的资源。
    
    认识到，可能需要一些时间来在 DNS 中成功复制群集名称。 后成功 DNS 注册和复制，如果你在服务器管理器中，选择**所有服务器**的群集名称应列为**可管理性**状态为**联机**的服务器。

创建群集后，你可以执行操作如验证群集仲裁配置，并 （可选） 创建群集共享卷 (CSV)。 有关详细信息，请参阅[了解存储空间直通中的仲裁](../storage/storage-spaces/understand-quorum.md)和[使用故障转移群集中的群集共享卷](failover-cluster-csvs.md)。

## 创建群集的角色

创建故障转移群集后，你可以创建到主机群集工作负荷的群集的角色。

>[!NOTE]
>适用于需要客户端访问点的群集角色，在 AD DS 中创建虚拟计算机对象 (VCO)。 默认情况下，为群集的所有 Vco 都创建相同的容器或 OU 中为 CNO。 认识到，在创建群集后，你可以移动 CNO 到任何 OU。

下面介绍了如何创建群集的角色：

1. 使用服务器管理器或 Windows PowerShell 安装的角色或功能所需的每个故障转移群集节点上的群集角色。 例如，如果你想要创建的群集的文件服务器，所有群集节点上安装文件服务器角色。
    
    下表显示你可以配置高可用性向导和关联的服务器角色或功能，必须安装作为必备条件中的群集的角色。
    
    <table>
    <thead>
    <tr class="header">
    <th>群集的角色</th>
    <th>角色或功能的先决条件</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>DFS Namespace 服务器</td>
    <td>DFS 命名空间 （文件服务器角色的一部分）</td>
    </tr>
    <tr class="even">
    <td>DHCP 服务器</td>
    <td>DHCP 服务器角色</td>
    </tr>
    <tr class="odd">
    <td>分布式的事务处理协调器 (DTC)</td>
    <td>无</td>
    </tr>
    <tr class="even">
    <td>文件服务器</td>
    <td>文件服务器角色</td>
    </tr>
    <tr class="odd">
    <td>通用应用程序</td>
    <td>不适用</td>
    </tr>
    <tr class="even">
    <td>通用脚本</td>
    <td>不适用</td>
    </tr>
    <tr class="odd">
    <td>通用服务</td>
    <td>不适用</td>
    </tr>
    <tr class="even">
    <td>HYPER-V 副本代理</td>
    <td>HYPER-V 角色</td>
    </tr>
    <tr class="odd">
    <td>iSCSI 目标服务器</td>
    <td>iSCSI 目标服务器 （文件服务器角色的一部分）</td>
    </tr>
    <tr class="even">
    <td>iSNS 服务器</td>
    <td>iSNS 服务器服务功能</td>
    </tr>
    <tr class="odd">
    <td>消息队列</td>
    <td>消息队列服务功能</td>
    </tr>
    <tr class="even">
    <td>另一台服务器</td>
    <td>无</td>
    </tr>
    <tr class="odd">
    <td>虚拟机</td>
    <td>HYPER-V 角色</td>
    </tr>
    <tr class="even">
    <td>WINS 服务器</td>
    <td>WINS 服务器功能</td>
    </tr>
    </tbody>
    </table>
2. 故障转移群集管理器中，展开群集名称，右键单击**角色**，然后选择**配置角色**。
3. 按照高可用性向导操作以创建群集的角色中的步骤。
4. 若要验证已创建群集的角色，在**角色**窗格中，确保角色已**运行**的状态。 角色窗格还指示在所有者节点。 若要测试故障转移，右键单击角色，指向**移动**，然后依次选择**选择节点**。 在**移动群集角色**对话框中，选择所需的群集节点中，，然后选择**确定**。 在**所有者节点**列中，验证在所有者节点更改。

## 使用 Windows PowerShell 创建故障转移群集

以下 Windows PowerShell cmdlet 执行本主题中的功能与前面的过程相同。 输入一行，每个 cmdlet，即使它们有可能显示自动换行跨多个行由于格式约束。

>[!NOTE]
>你必须使用 Windows PowerShell 来创建在 Windows Server 2012 R2 的 Active Directory 分离的群集。 有关语法的信息，请参阅[部署 Active Directory-Detached 群集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))。

下面的示例安装故障转移群集功能。

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

下面的示例在名为*服务器 1*和*Server2*的计算机上运行所有群集验证测试。

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>**测试群集**cmdlet 将结果输出到日志文件中的当前工作目录。 例如： < 用户名 > C:\Users\ \AppData\Local\Temp。

以下示例创建故障转移群集节点*服务器 1* *MyCluster*和*Server2*名为、 分配静态 IP 地址*192.168.1.12*，并将符合条件的所有存储都添加到故障转移群集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

下面的示例创建相同的故障转移群集，如前面的示例中所示，但它不会有资格存储添加到故障转移群集。

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

以下示例创建了名为*MyCluster* *群集*的域*Contoso.com*的 OU 中的群集。

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

有关如何添加群集的角色的示例，请参阅主题[添加 ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps)和[添加 ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps)等。

## 详细信息

  - [故障转移群集](failover-clustering.md)
  - [部署 HYPER-V 群集](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [应用程序数据的横向扩展文件服务器](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [部署 Active Directory 分离的群集](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [使用来宾群集以实现高可用性](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [群集感知更新](cluster-aware-updating.md)
  - [新群集](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [测试群集](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
