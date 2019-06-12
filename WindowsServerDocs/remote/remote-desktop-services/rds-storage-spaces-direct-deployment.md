---
title: 部署为 UPD 存储在 Azure 中两个节点存储空间直通 SOFS
description: 了解如何使用存储空间直通与 rds。
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
ms.openlocfilehash: 792c9320f6976a4fc7f2ccd235f66daa0cb19b19
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66805189"
---
# <a name="deploy-a-two-node-storage-spaces-direct-scale-out-file-server-for-upd-storage-in-azure"></a>部署为 UPD 存储在 Azure 中的双节点存储空间直通的横向扩展文件服务器

>适用于：Windows Server （半年频道），Windows Server 2019，Windows Server 2016

远程桌面服务 (RDS) 的用户配置文件磁盘 (Upd) 需要一个已加入域的文件服务器。 若要在 Azure 中部署高可用性已加入域的横向扩展文件服务器 (SOFS)，使用存储空间直通使用 Windows Server 2016。 如果您不熟悉 Upd 或远程桌面服务，请查看[欢迎使用远程桌面服务](welcome-to-rds.md)。

> [!NOTE] 
> Microsoft 刚刚发布[部署存储空间直通的横向扩展文件服务器的 Azure 模板](https://azure.microsoft.com/documentation/templates/301-storage-spaces-direct/)！ 可以使用模板创建你的部署，或使用本文中的步骤。 

我们建议部署在 SOFS 使用 DS 系列 Vm 和高级存储数据磁盘，其中有相同数量和每个 VM 上的数据磁盘的大小。 你将需要至少两个存储帐户。 

对于小型部署，建议的 2 节点群集并且云见证服务器，其中 2 个副本与镜像卷。 通过添加数据磁盘而增长小型部署。 通过添加节点 (Vm) 中增加更大的部署。 

这些说明适用于的 2 节点部署。 下表显示的 VM 和磁盘大小都需要在业务中存储的用户数的 Upd。 

| 用户 | 总计 (GB) | VM | # 个磁盘 | 磁盘类型 | 磁盘大小 (GB) | 配置   |
|-------|------------|----|---------|-----------|----------------|-----------------|
| 10    | 50         | DS1 | 2       | P10       | 128            | 2 x （DS1 + 2 P10）  |
| 25    | 125        | DS1 | 2       | P10       | 128            | 2 x （DS1 + 2 P10）  |
| 50    | 250        | DS1 | 2       | P10       | 128            | 2 x （DS1 + 2 P10）  |
| 100   | 500        | DS1 | 2       | P20       | 512            | 2 x （DS1 + 2 P20）  |
| 250   | 1250       | DS1 | 2       | P30       | 1024           | 2 x （DS1 + 2 P30）  |
| 500   | 2500       | DS2 | 3       | P30       | 1024           | 2 x （DS2 + 3 P30）  |
| 1000  | 5000       | DS3 | 5       | P30       | 1024           | 2 x （DS3 + 5 P30）  |
| 2500  | 12500      | DS4 | 13      | P30       | 1024           | 2 x （DS4 + 13 P30） |
| 5000  | 25000      | DS5 | 25      | P30       | 1024           | 2 x （ds5 共 + 25 P30） | 

使用以下步骤来创建域控制器 （我们称为"my dc"下面） 和两个节点 （"我 fsn1"和"我 fsn2"） 的 Vm 并配置要 2 节点存储空间直通 SOFS 的 Vm。

1. 创建[Microsoft Azure 订阅](https://azure.microsoft.com)。
2. 登录到 [Azure 门户](https://ms.portal.azure.com)。
3. 创建[Azure 存储帐户](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/#create-a-storage-account)Azure 资源管理器中。 创建新的资源组中，并使用以下配置：
   - 部署模型：资源管理器
   - 存储帐户的类型：常规用途
   - 性能层：高级
   - 复制选项：LRS
4. 通过使用快速入门模板或手动部署林设置 Active Directory 林。 
   - 使用 Azure 快速入门模板进行部署：
      - [创建新的 AD 林的 Azure VM](https://azure.microsoft.com/documentation/templates/active-directory-new-domain/)
      - [使用 2 个域控制器创建新的 AD 域](https://azure.microsoft.com/documentation/templates/active-directory-new-domain-ha-2-dc/)（适用于高可用性）
   - 手动[部署林](https://azure.microsoft.com/documentation/articles/active-directory-new-forest-virtual-machine/)采用以下配置：
      - 与存储帐户相同的资源组中创建虚拟网络。
      - 建议的大小：DS2 （如果域控制器将承载多个域对象，则增加大小）
      - 使用自动生成的 VNet。
      - 按照安装 AD DS 的步骤。
5. 设置文件服务器群集节点。 您可以执行此操作通过部署[Windows Server 2016 存储空间直通 SOFS 群集 Azure 模板](https://azure.microsoft.com/resources/templates/301-storage-spaces-direct/)或者按照以下步骤 6-11 以手动部署。
6. 若要手动设置文件服务器群集节点：
   1. 创建第一个节点： 
      1. 创建新的虚拟机使用 Windows Server 2016 映像。 (单击**新 > 虚拟机 > Windows Server 2016。** 选择**资源管理器**，然后单击**创建**。)
      2. 设置基本配置，如下所示：
         - 我 fsn1 名称：
         - SSD 的 VM 磁盘类型
         - 使用现有的资源组，在步骤 3 中创建一个。 
      3. 尺寸：DS1、 DS2、 DS3、 DS4 或具体取决于你的用户的 ds5 共需要 （请参阅这些说明的开始处的表）。 确保选择高级磁盘支持。
      4. 设置： 
         - 存储帐户：选择在步骤 3 中创建的存储帐户。
         - 高可用性-创建新的可用性集。 (单击**高可用性 > 新建**，然后输入名称 （例如，s2d 的群集）。 使用的默认值**更新域**并**容错域**。)
   2. 创建第二个节点。 重复上述步骤有以下更改：
      - 我 fsn2 名称：
      - 高可用性-上面创建的可用性集的选择。  
7. [将数据磁盘附加](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-attach-disk-portal/)到群集节点 Vm 用户根据需要 （如上面的表中所示）。 创建并附加到 VM，设置磁盘的数据之后**主机缓存**到**None**。
8. 对所有 vm 设置 IP 地址**静态**。 
   1. 在资源组中，选择一个 VM，然后单击**网络接口**(下**设置**)。 选择所列出的网络接口，然后依次**IP 配置**。 选择列出的 IP 配置中，选择**静态**，然后单击**保存**。
   2. 记下的域控制器 (我的 dc 对于我们的示例) (10.x.x.x) 专用 IP 地址。
9. 在群集节点 Vm 的 Nic 上设置主 DNS 服务器地址，为我 dc 服务器。 选择 VM，然后依次**网络接口 > DNS 服务器 > 自定义 DNS**。 输入上面，记下的专用 IP 地址，然后单击**保存**。
10. 创建[Azure 存储帐户是云见证](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)。 （如果使用的链接的说明进行操作，停止到"配置云见证服务器与故障转移群集管理器 GUI 的"获取-我们将执行下面的步骤。）
11. 设置存储空间直通的文件服务器。 连接到 VM 的节点，然后运行以下 Windows PowerShell cmdlet。
    1. 在两个文件服务器群集节点 Vm 上安装故障转移群集功能和文件服务器功能：

       ```powershell
       $nodes = ("my-fsn1", "my-fsn2")
       icm $nodes {Install-WindowsFeature Failover-Clustering -IncludeAllSubFeature -IncludeManagementTools} 
       icm $nodes {Install-WindowsFeature FS-FileServer} 
       ```
    2. 验证群集节点 Vm，并创建 2 节点的 SOFS 群集：

       ```powershell
       Test-Cluster -node $nodes
       New-Cluster -Name MY-CL1 -Node $nodes –NoStorage –StaticAddress [new address within your addr space]
       ``` 
    3. 配置云见证。 使用云见证服务器存储帐户名称和访问密钥。

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
       若要查看在 SOFS 群集上的群集共享卷的相关信息，请运行以下 cmdlet:

       ```powershell
       Get-ClusterSharedVolume
       ```
   
    6. 创建横向扩展文件服务器 (SOFS):

       ```powershell
       Add-ClusterScaleOutFileServerRole -Name my-sofs1 -Cluster MY-CL1
       ```

    7. 在 SOFS 群集上创建新的 SMB 文件共享。

       ```powershell
       New-Item -Path C:\ClusterStorage\Volume1\Data -ItemType Directory
       New-SmbShare -Name UpdStorage -Path C:\ClusterStorage\Volume1\Data
       ```

现可在共享`\\my-sofs1\UpdStorage`，可以使用为 UPD 存储时您[启用 UPD](https://social.technet.microsoft.com/wiki/contents/articles/15304.installing-and-configuring-user-profile-disks-upd-in-windows-server-2012.aspx)为你的用户。 
