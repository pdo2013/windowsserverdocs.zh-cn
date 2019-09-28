---
title: 在 Windows Server 2019 中部署文件共享见证
ms.prod: windows-server
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 01/24/2019
description: 文件共享见证服务器允许使用文件共享在群集仲裁中投票。 本主题介绍文件共享见证服务器和新功能，包括使用作为文件共享见证连接到路由器的 USB 驱动器。
ms.localizationpriority: medium
ms.openlocfilehash: 9f0a0c5b48f7c382367e4b1100ff649fe73d3be9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369759"
---
# <a name="deploy-a-file-share-witness"></a>部署文件共享见证

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

文件共享见证是故障转移群集在群集仲裁中用作投票的 SMB 共享。 本主题概述了 Windows Server 2019 中的技术和新功能，包括使用连接到路由器的 USB 驱动器作为文件共享见证。

文件共享见证服务器在以下情况下很方便：  

- 由于并非群集中的所有服务器都具有可靠的 Internet 连接，因此无法使用云见证
- 无法使用磁盘见证，因为没有可用于磁盘见证的任何共享驱动器。 这可能是存储空间直通群集、SQL Server Always On 可用性组（AG）、Exchange 数据库可用性组（DAG）等。这些群集类型都不使用共享磁盘。

## <a name="file-share-witness-requirements"></a>文件共享见证要求

你可以在已加入域的 Windows server 上托管文件共享见证，或者，如果你的群集正在运行 Windows Server 2019，则可以托管 SMB 2 或更高版本文件共享的任何设备。

|文件服务器类型                 | 支持的群集 |
|---------------------------------|--------------------|
|带有 SMB 2 文件共享的任何设备 | Windows Server 2019|
|已加入域的 Windows Server     | Windows Server 2008 及更高版本|

如果群集正在运行 Windows Server 2019，以下是要求：

- *使用 smb 2 或更高版本协议的任何设备上*的 SMB 文件共享，其中包括：
    - 网络连接存储（NAS）设备
    - 加入工作组的 Windows 计算机
    - 具有本地连接的 USB 存储的路由器
- 设备上用于对群集进行身份验证的本地帐户
- 如果使用的是使用文件共享对群集进行身份验证的 Active Directory，则群集名称对象（CNO）必须对共享具有写入权限，并且该服务器必须与群集位于同一 Active Directory 林中
- 文件共享至少有 5 MB 的可用空间

如果群集正在运行 Windows Server 2016 或更早版本，则需要满足以下要求：

- *与群集加入同一 Active Directory 林的 Windows server 上*的 SMB 文件共享
- 群集名称对象（CNO）必须对共享具有写入权限
- 文件共享至少有 5 MB 的可用空间

其他说明：
- 若要使用由已加入域的 Windows server 之外的设备托管的文件共享见证服务器，则当前必须使用**set-clusterquorum-Credential** PowerShell cmdlet 来设置见证服务器，如本主题后面所述。
- 为实现高可用性，可在单独的故障转移群集上使用文件共享见证
- 文件共享可供多个群集使用
- 任何版本的故障转移群集都不支持使用分布式文件系统（DFS）共享或复制的存储。  这可能会导致出现裂脑的情况，即群集服务器彼此独立运行并可能导致数据丢失。

## <a name="creating-a-file-share-witness-on-a-router-with-a-usb-device"></a>使用 USB 设备在路由器上创建文件共享见证

在[Microsoft Ignite 2018](https://azure.microsoft.com/ignite/)， [DataOn Storage](http://www.dataonstorage.com/)在其展台领域有一个存储空间直通的群集。  此群集已连接到[NetGear](https://www.netgear.com) Nighthawk X4S WiFi 路由器，该路由器使用 USB 端口作为文件共享见证，类似于此。

![NetGear 见证服务器](media/File-Share-Witness/FSW1.png)

下面列出了在此特定路由器上使用 USB 设备创建文件共享见证的步骤。  请注意，其他路由器和 NAS 设备上的步骤会有所不同，应使用供应商提供的说明来完成。


1. 登录到连接了 USB 设备的路由器。

   ![NetGear 接口](media/File-Share-Witness/FSW2.png)

2. 在选项列表中，选择 "ReadySHARE"，这是可以创建共享的位置。

   ![NetGear ReadySHARE](media/File-Share-Witness/FSW3.png)

3. 对于文件共享见证，只需要一个基本共享。  选择 "编辑" 按钮将弹出一个对话框，在该对话框中可以在 USB 设备上创建共享。

   ![NetGear 共享接口](media/File-Share-Witness/FSW4.png)

4. 选择 "应用" 按钮后，将会创建共享，并可在列表中查看。

   ![NetGear 共享](media/File-Share-Witness/FSW5.png)

5. 创建共享后，为群集创建文件共享见证是通过 PowerShell 来完成的。

   ```PowerShell
   Set-ClusterQuorum -FileShareWitness \\readyshare\Witness -Credential (Get-Credential)
   ```

   此时将显示一个对话框，用于输入设备上的本地帐户。

可以在具有 USB 功能、NAS 设备或其他 Windows 设备的其他路由器上完成这些相同的步骤。
