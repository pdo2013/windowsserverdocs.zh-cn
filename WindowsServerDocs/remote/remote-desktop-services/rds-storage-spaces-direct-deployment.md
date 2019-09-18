---
title: 在 Azure 中为 UPD 存储部署双节点存储空间直通 SOFS
description: 了解如何将存储空间直通与 RDS 配合使用。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1099f21d-5f07-475a-92dd-ad08bc155da1
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
manager: scottman
ms.openlocfilehash: 30f2d97c93c3df72eaf21896d596a4a10666013c
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870750"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>在 Azure 中为 UPD 存储部署双节点存储空间直通横向扩展文件服务器

>适用于：Windows Server（半年频道）、Windows Server 2019、Windows Server 2016

远程桌面服务 (RDS) 需要一个已加入域的文件服务器来存储用户配置文件磁盘 (UPD)。 若要在 Azure 中部署已加入域的高可用性横向扩展文件服务器 (SOFS)，请将存储空间直通与 Windows Server 2016 配合使用。 如果不熟悉 UPD 或远程桌面服务，请查看[欢迎使用远程桌面服务](welcome-to-rds.md)。

> [!NOTE] 
> Microsoft 刚刚发布了一个[用于部署存储空间直通横向扩展文件服务器的 Azure模板](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/)！ 你可以使用该模板来创建部署，也可以按本文中的步骤操作。 

我们建议使用 DS 系列 VM 和高级存储数据磁盘来部署 SOFS，其中每个 VM 上数据磁盘的数量和大小均相同。 至少需要两个存储帐户。 

对于小型部署，我们建议使用包含云见证的双节点群集，其中，卷使用 2 个副本进行镜像。 通过添加数据磁盘来扩展小型部署。 通过添加节点 (VM) 来扩展更大的部署。 

以下说明适用于双节点部署。 下表显示了为企业的用户数存储 UPD 所需的 VM 和磁盘大小。 

| 用户 | 总计 (GB) | VM | 磁盘数 | 磁盘类型 | 磁盘大小 (GB) | 配置   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2x(DS1 + 2 P10)  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2x(DS1 + 2 P20)  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2x(DS1 + 2 P30)  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2x(DS2 + 3 P30)  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2x(DS3 + 5 P30)  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2x(DS4 + 13 P30) |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2x(DS5 + 25 P30) | 

使用以下步骤创建一个域控制器（下文称为“my-dc”）和两个节点 VM（“my-fsn1”和“my-fsn2”），并将 VM 配置为双节点存储空间直通 SOFS。

1. 创建一个 [Microsoft Azure 订阅](https://azure.microsoft.com)。
2. 登录 [Azure 门户](https://ms.portal.azure.com)。
3. 在 Azure 资源管理器中创建一个 [Azure 存储帐户](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)。 在新资源组中创建一个，并使用以下配置：
   - 部署模型：资源管理器
   - 存储帐户类型：常规用途
   - 性能层：高级
   - 复制选项：LRS
4. 通过使用快速入门模板或手动部署林来设置 Active Directory 林。 
   - 使用 Azure 快速入门模板进行部署：
      - [使用新的 AD 林创建 Azure VM](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [使用 2 个域控制器创建新的 AD 域](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/)（实现高可用性）
   - 使用以下配置手动[部署林](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)：
      - 在与存储帐户相同的资源组中创建虚拟网络。
      - 建议大小：DS2（如果域控制器将托管更多域对象，则增加大小）
      - 使用自动生成的 VNet。
      - 按照步骤安装 AD DS。
5. 设置文件服务器群集节点。 可以通过部署 [Windows Server 2016 存储空间直通 SOFS 群集 Azure 模板](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/)或按照步骤 6-11 手动部署来执行此操作。
6. 若要手动设置文件服务器群集节点，请执行以下操作：
   1. 创建第一个节点： 
      1. 使用 Windows Server 2016 映像创建新虚拟机。 （单击“新建”>“虚拟机”>“Windows Server 2016”  。 选择“资源管理器”，然后单击“创建”   。）
      2. 按下面所述设置基本配置：
         - 名称：my-fsn1
         - VM 磁盘类型 SSD
         - 使用现有资源组，即，在步骤 3 中创建的资源组。 
      3. 尺寸：DS1、DS2、DS3、DS4 或 DS5，具体取决于用户需求（请参阅这些说明开头处的表格）。 确保选择了高级磁盘支持。
      4. 设置： 
         - 存储帐户：选择在步骤 3 中创建的存储帐户。
         - 高可用性 - 创建新的可用性集。 （单击“高可用性”>“新建”，然后输入名称（例如，s2d-cluster）  。 对“更新域”和“容错域”使用默认值   。）
   2. 创建第二个节点。 重复上述步骤并进行以下更改：
      - 名称：my-fsn2
      - 高可用性 - 选择上面创建的可用性集。  
7. 根据用户需求向群集节点 VM [附加数据磁盘](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/)（如上表所示）。 创建数据磁盘并将其附加到 VM 后，将“主机缓存”设置为“无”   。
8. 将所有 VM 的 IP 地址均设置为“静态”  。 
   1. 在资源组中，选择一个 VM，然后单击“网络接口”（在“设置”下）   。 选择列出的网络接口，然后单击“IP 配置”  。 选择列出的 IP 配置，选择“静态”，然后单击“保存”   。
   2. 记下域控制器（我们的示例中为 my-dc）专用 IP 地址 (10.x.x.x)。
9. 将群集节点 VM 的 NIC 上的主 DNS 服务器地址设置为 my-dc 服务器。 选择 VM，然后单击“网络接口”>“DNS 服务器”>“自定义 DNS”  。 输入上面记下的专用 IP 地址，然后单击“保存”  。
10. 创建一个 [Azure 存储帐户作为云见证](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。 （如果使用链接的说明，请在浏览到“使用故障转移群集管理器 GUI 配置云见证”时停止 - 我们将在下面执行该步骤。）
11. 设置存储空间直通文件服务器。 连接到节点 VM，然后运行以下 Windows PowerShell cmdlet。
    1. 在两个文件服务器群集节点 VM 上安装故障转移群集功能和文件服务器功能：

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. 验证群集节点 VM 并创建双节点 SOFS 群集：

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. 配置云见证。 使用你的云见证存储帐户名称和访问密钥。

       ```powershell
       Set-ClusterQuorum –CloudWitness –AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> 
       ```
    4. 启用存储空间直通。

       ```powershell
       Enable-ClusterS2D 
       ```
      
    5. 创建虚拟磁盘卷。

       ```powershell
       New-Volume -StoragePoolFriendlyName S2D* -FriendlyName VDisk01 -FileSystem CSVFS_REFS -Size 120GB 
       ```
       若要查看 SOFS 群集上的群集共享卷的相关信息，请运行以下 cmdlet：

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. 创建横向扩展文件服务器 (SOFS)：

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. 在 SOFS 群集上创建新的 SMB 文件共享。

       ```powershell
       New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
       ```

现在在 `\\my-sofs1\UpdStorage` 中有一个共享，为用户[启用 UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx) 时，可将其用于 UPD 存储。 
