---
title: 部署 Windows Server 2019 中的文件共享见证
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: 文件共享见证服务器，可使用文件共享来群集仲裁投票。 本主题介绍文件共享见证服务器和新功能，包括使用 USB 驱动器连接到路由器作为文件共享见证服务器。
ms.localizationpriority: medium
ms.openlocfilehash: 1888142f96208800a0417c9caeea89e8a0472e88
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831748"
---
# <a name="deploy-a-file-share-witness"></a>部署的文件共享见证

> 适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

文件共享见证服务器是作为群集仲裁投票的故障转移群集使用的 SMB 共享。 本主题提供的技术和 Windows Server 2019，包括使用 USB 驱动器连接到作为文件共享见证服务器的路由器中的新功能的概述。

文件共享见证服务器可以方便地在以下情况下：  

- 不能使用云见证服务器，因为并非所有服务器群集中的都拥有可靠的 Internet 连接
- 无法使用磁盘见证服务器，因为没有任何要使用磁盘见证服务器的共享的驱动器。 这可能是存储空间直通的群集，SQL Server 始终在可用性组 (AG)，Exchange 数据库可用性组 (DAG)，等等。没有任何这些类型的群集使用共享的磁盘。

## <a name="file-share-witness-requirements"></a>文件共享见证服务器要求

你可以托管文件共享见证在已加入域的 Windows 服务器上，或如果你的群集正在运行 Windows Server 2019，任何设备，可以主机 SMB 2 或更高版本的文件共享。

|文件服务器类型                 | 受支持的群集 |
|---------------------------------|--------------------|
|任何设备的 w/SMB 2 文件共享 | Windows Server 2019|
|已加入域的 Windows Server     | Windows Server 2008 和更高版本|

如果群集正在运行 Windows Server 2019，如下要求：

- SMB 文件共享*SMB 2 或更高版本的协议使用任何设备上*，其中包括：
    - 网络连接存储 (NAS) 设备
    - Windows 计算机加入到工作组
    - 路由器使用本地连接的 USB 存储
- 对群集进行身份验证的设备上本地帐户
- 如果改为使用 Active Directory 进行身份验证与文件共享群集，群集名称对象 (CNO) 必须在共享上，具有写入权限和服务器必须位于与群集相同的 Active Directory 林
- 文件共享具有最少 5 MB 的可用空间

如果该群集正在运行 Windows Server 2016 或更早版本，如下要求：

- SMB 文件共享*加入到与群集相同的 Active Directory 林的 Windows 服务器上*
- 群集名称对象 (CNO) 必须对共享具有写入权限
- 文件共享具有最少 5 MB 的可用空间

其他注意事项：
- 若要使用托管的设备已加入域的 Windows server 以外的文件共享见证，目前必须使用**Set-clusterquorum-凭据**PowerShell cmdlet 设置见证服务器，如本主题后面所述。
- 以实现高可用性，可以使用单独的故障转移群集上的文件共享见证
- 文件共享可供多个群集
- 使用分布式文件系统 (DFS) 共享或复制的存储不支持与任何版本的故障转移群集。  这些可能会导致群集的服务器均可独立运行，可能会导致数据丢失的拆分脑情况。

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>使用 USB 设备在路由器上创建文件共享见证

在[Microsoft ignite 大会 2018年](https://azure.microsoft.com/ignite/)， [DataOn 存储](http://www.dataonstorage.com/)其展台区域中具有存储空间直通群集。  已连接到此群集[NetGear](https://www.netgear.com) Nighthawk X4S WiFi 路由器使用的 USB 端口为文件共享见证服务器与此类似。

![NetGear 见证服务器](media\File-Share-Witness\FSW1.png)

下面列出了用于创建此特定路由器上使用 USB 设备的文件共享见证的步骤。  请注意，其他路由器和 NAS 设备上的步骤会有所不同，并且应使用供应商来完成提供说明。


1. 使用接通电源的 USB 设备登录到路由器。

   ![NetGear 接口](media\File-Share-Witness\FSW2.png)

2. 从选项列表中，选择 ReadySHARE 这是在其中创建共享。

   ![NetGear ReadySHARE](media\File-Share-Witness\FSW3.png)

3. 对于文件共享见证，基本共享是所需要的。  选择编辑按钮将弹出一个对话框，其中 USB 设备创建共享。

   ![NetGear 共享接口](media\File-Share-Witness\FSW4.png)

4. 后选择应用按钮，创建共享，且可以看到列表中。

   ![NetGear 共享](media\File-Share-Witness\FSW5.png)

5. 一旦创建共享后，创建群集的文件共享见证服务器是通过 PowerShell。

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   这将显示一个对话框，在设备上输入本地帐户。

在其他路由器使用 USB 功能、 NAS 设备或其他 Windows 设备上可以进行类似上述相同步骤。
