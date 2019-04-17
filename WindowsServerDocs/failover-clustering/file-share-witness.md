---
title: 部署 Windows Server 2019 的文件共享见证
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: 文件共享证人允许你使用的文件共享在群集仲裁中投票。 本主题介绍了文件共享证人和新功能，包括使用 USB 驱动器连接到路由器作为文件共享见证。
ms.localizationpriority: medium
ms.openlocfilehash: 1888142f96208800a0417c9caeea89e8a0472e88
ms.sourcegitcommit: d622f7af181ed0063d716b30278d41887a57db19
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/08/2019
ms.locfileid: "9150887"
---
# 部署文件共享见证

> 适用于： Windows Server 2019 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

文件共享见证是故障转移群集使用与在群集仲裁投票 SMB 共享。 本主题提供了技术和 Windows Server 2019，包括使用 USB 驱动器连接到路由器作为见证文件共享中的新功能的概述。

文件共享证人是在以下情况下方便：  

- 不使用云见证，因为在群集中的并非所有服务器都具有可靠的 Internet 连接
- 无法使用磁盘见证，因为没有任何要用于见证磁盘的共享的驱动器。 这可能是在存储空间直通群集，SQL Server 始终上可用性组 (AG)，Exchange 数据库可用性组 (DAG)，等等。 没有任何这些类型的群集使用共享的磁盘。

## 文件共享见证要求

你还可以承载文件共享见证在已加入域的 Windows 服务器上，或如果你群集正在运行 Windows Server 2019，任何设备，可以主机 SMB 2 或更高版本的文件共享。

|文件服务器类型                 | 受支持的群集 |
|---------------------------------|--------------------|
|任何设备 w/了 SMB 2 文件共享 | Windows Server 2019|
|已加入域的 Windows Server     | Windows Server 2008 及更高版本|

如果群集正在运行 Windows Server 2019，下面是要求：

- SMB 文件共享*上的使用 SMB 2 或更高版本的协议的任何设备*，包括：
    - 网络连接存储 (NAS) 设备
    - Windows 计算机加入到工作组
    - 路由器具有本地连接的 USB 存储
- 本地帐户进行身份验证群集在设备上
- 如果你改为使用 Active Directory 进行身份验证的文件共享的群集，群集名称对象 (CNO) 上共享，必须具有写权限和服务器必须位于同一个 Active Directory 林的群集
- 文件共享具有至少 5 MB 的可用空间

如果群集正在运行 Windows Server 2016 或更早版本，下面是要求：

- SMB 文件共享*上 Windows 服务器加入到同一个 Active Directory 林的群集*
- 群集名称对象 (CNO) 必须具有共享上的写入权限
- 文件共享具有至少 5 MB 的可用空间

其他说明：
- 若要使用文件共享见证托管的设备已加入域的 Windows 服务器之外的设备，当前必须使用**Set-clusterquorum-Credential** PowerShell cmdlet 来设置见证，如本主题后面所述。
- 用于实现高可用性，你可以在单独的故障转移群集中使用文件共享见证
- 可由多个群集中的文件共享
- 使用故障转移群集的任何版本不支持分布式文件系统 (DFS) 共享或复制的存储的使用。  这些可能会导致拆分大脑情况群集的服务器独立于彼此运行其中可能会导致数据丢失。

## 使用 USB 设备路由器上创建文件共享见证

在[Microsoft Ignite 2018](https://azure.microsoft.com/ignite/) [DataOn 存储](http://www.dataonstorage.com/)其展台区域中具有存储空间直通群集。  此群集已连接到使用作为文件共享见证与此类似的 USB 端口[NetGear](https://www.netgear.com) Nighthawk X4S WiFi 路由器。

![NetGear 见证](media\File-Share-Witness\FSW1.png)

创建文件共享见证在该特定路由器上使用 USB 设备的步骤如下所示。  请注意，在其他路由器和 NAS 设备上的步骤会有所不同，应使用供应商完成提供路线。


1. 使用在插入的 USB 设备登录到路由器。

   ![NetGear 接口](media\File-Share-Witness\FSW2.png)

2. 从选项的列表，选择 ReadySHARE 这是在其中创建共享。

   ![NetGear ReadySHARE](media\File-Share-Witness\FSW3.png)

3. 对于文件共享见证，基本共享是所需要的。  选择编辑按钮将弹出一个对话框，其中 USB 设备创建共享。

   ![NetGear 共享接口](media\File-Share-Witness\FSW4.png)

4. 后选择应用按钮，该共享创建，并且可以在列表中所示。

   ![NetGear 共享](media\File-Share-Witness\FSW5.png)

5. 创建群集的文件共享见证使用 PowerShell 完成后已创建共享。

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   这将显示一个对话框，以在设备上输入本地帐户。

可以使用 USB 功能、 NAS 设备或其他 Windows 设备其他路由器上完成这些相同的类似步骤。
