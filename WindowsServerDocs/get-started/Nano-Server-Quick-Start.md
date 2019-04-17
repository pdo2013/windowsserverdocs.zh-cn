---
title: Nano Server 快速入门
description: 快速在物理计算机或虚拟机上部署基本 Nano Server 的步骤
ms.prod: windows-server-threshold
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.date: 09/05/2017
ms.tgt_pltfrm: na
ms.topic: get-started-article
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 488d0bed661cf2078d20e491a8c68b2a29a42b73
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081894"
---
# <a name="nano-server-quick-start"></a>Nano Server 快速入门

>适用于：Windows Server 2016

> [!IMPORTANT]
> 自 Windows Server 版本 1709 开始，Nano Server 将仅用作[容器基本操作系统映像](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image)。 查看[对 Nano Server 进行的更改](nano-in-semi-annual-channel.md)以了解这意味着什么。 

按照本部分中的步骤，使用 DHCP 获取 IP 地址，快速启动 Nano Server 的基本部署。 可以在虚拟机上运行 Nano Server VHD 或在物理计算机上启动；步骤会稍有不同。

使用这些快速入门步骤尝试基础操作后，可以查阅[部署 Nano Server](Deploy-Nano-Server.md)，了解创建自定义映像、包管理的几种方法、域操作等详细信息。
  
**虚拟机中的 Nano Server**  
  
请按照下列步骤创建将在虚拟机中运行的 Nano Server VHD。  
  
## <a name="to-quickly-deploy-nano-server-in-a-virtual-machine"></a>在虚拟机中快速部署 Nano Server  
  
1.  将 *NanoServerImageGenerator* 文件夹从 Windows Server 2016 ISO 中 \NanoServer 文件夹复制到你硬盘上的文件夹。  
  
2.  以管理员身份启动 Windows PowerShell，将目录更改为 NanoServerImageGenerator 文件夹所在的文件夹，然后导入模块，其方法为 `Import-Module .\NanoServerImageGenerator -Verbose`  
>[!NOTE]  
>可能必须调整 Windows PowerShell 执行策略。 `Set-ExecutionPolicy RemoteSigned` 应能正常工作。  
  
3.  通过运行以下命令（将提示你输入新 VHD 的管理员密码）创建用于设置计算机名和包括 Hyper-V **来宾驱动程序**的标准版 VHD：  
  
    `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerVM\NanoServerVM.vhd -ComputerName <computer name>` 其中  
  
    -   **-MediaPath <path to root of media\>** 指定 Windows Server 2016 ISO 内容位置的根路径。 例如，如果已将该 ISO 的内容复制到 d:\TP5ISO，则将使用该路径。  
  
    -   **-BasePath**（可选）指定要创建的用于将 Nano Server WIM 和包复制到其中的文件夹。  
  
    -   **-TargetPath** 指定将在其中创建生成的 VHD 或 VHDX 的路径，包括文件名和扩展名。  
  
    -   **Computer_name** 指定正在创建的 Nano Server 虚拟机的计算机名称。  
  
    **例如：** `New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1`  
  
    此示例将从装载为 f: 的 ISO 创建一个 VHD。 在创建 VHD 时，它将使用在其中运行 New-NanoServerImage 的同一目录中一个名为 Base 的文件夹；它会将 VHD（称为 Nano.vhd）放置于从其中运行此命令的名为 Nano1 的文件夹中。 计算机名称将是 Nano1。 生成的 VHD 将包含 Windows Server 2016 Standard，并将适用于 Hyper-V 虚拟机部署。 如果需要第 1 代虚拟机，可以通过为 -TargetPath 指定一个 **.vhd** 扩展名来创建 VHD 映像。 如果需要第 2 代虚拟机，可以通过为 -TargetPath 指定 **.vhdx** 扩展名来创建 VHDX 映像。 还可以通过为 -TargetPath 指定 **.wim** 扩展名直接生成 WIM 文件。  
  
    > [!NOTE]  
    > Windows 8.1、Windows 10、Windows Server 2012 R2 和 Windows Server 2016 均支持 New-NanoServerImage。  
  
4.  在 Hyper-V 管理器中，创建新的虚拟机，并使用在步骤 3 中创建的 VHD。  
  
5.  启动虚拟机，然后在 Hyper-V 管理器中连接到虚拟机。  
  
6.  使用在步骤 3 中运行脚本时提供的管理员帐户和密码登录到恢复控制台（请参阅本指南中的“Nano Server 恢复控制台”部分）。  
 > [!NOTE]  
    > 恢复控制台仅支持基本的键盘功能。 不支持键盘指示灯、10 键部分和键盘布局切换（例如大写锁定和数字锁定）。
  
7.  获取 Nano Server 虚拟机的 IP 地址并使用 Windows PowerShell 远程处理或其他远程管理工具连接到虚拟机并对其进行远程管理。  
  
**物理计算机上的 Nano Server**  
  
还可以使用预安装的设备驱动程序，创建将在物理计算机上运行 VHD 的 Nano Server。 如果你的硬件需要一个尚未提供的驱动程序以启动或连接到网络，请按照本指南的“添加其他驱动程序”部分中的步骤执行。  
  
## <a name="to-quickly-deploy-nano-server-on-a-physical-computer"></a>在物理计算机上快速部署 Nano Server  
  
1.  将 *NanoServerImageGenerator* 文件夹从 Windows Server 2016 ISO 中 \NanoServer 文件夹复制到你硬盘上的文件夹。  
  
2.  以管理员身份启动 Windows PowerShell，将目录更改为 NanoServerImageGenerator 文件夹所在的文件夹，然后导入模块，其方法为 `Import-Module .\NanoServerImageGenerator -Verbose`  
  
>[!NOTE]  
>可能必须调整 Windows PowerShell 执行策略。 `Set-ExecutionPolicy RemoteSigned` 应能正常工作。  
  
3.  通过运行以下命令（将提示你输入新 VHD 的管理员密码）创建用于设置计算机名和包括 OEM 驱动程序及 Hyper-V 的 VHD：  
  
    `New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath <path to root of media> -BasePath .\Base -TargetPath .\NanoServerPhysical\NanoServer.vhd -ComputerName <computer name> -OEMDrivers -Compute -Clustering` 其中  
  
    -   **-MediaPath <path to root of media\>** 指定 Windows Server 2016 ISO 内容位置的根路径。 例如，如果已将该 ISO 的内容复制到 d:\TP5ISO，则将使用该路径。  
  
    -   **BasePath** 指定要创建用于将 Nano Server WIM 和包复制到其中的文件夹。 （该参数为可选参数。）  
  
    -   **TargetPath** 指定将在其中创建生成的 VHD 或 VHDX 的路径，包括文件名和扩展名。  
  
    -   **Computer_name** 是正在创建的 Nano Server 的计算机名称。  
  
    **例如：**`New-NanoServerImage -Edition Standard -DeploymentType Host -MediaPath F:\ -BasePath .\Base -TargetPath .\Nano1\NanoServer.vhd -ComputerName Nano-srv1 -OEMDrivers -Compute -Clustering`  
  
    此示例将从装载为 f:\\ 的 ISO 创建一个 VHD。 在创建 VHD 时，它将使用在其中运行 New-NanoServerImage 的同一目录中一个名为 Base 的文件夹；它会将 VHD 放置于从其中运行此命令的名为 Nano1 的文件夹中。 计算机名称为 Nano srv1，将为大多数普通硬件安装 OEM 驱动程序并启用 Hyper-V 角色和群集功能。 已使用标准 Nano 版本。  
  
4.  在想要运行 Nano Server VHD 的物理服务器上以管理员身份登录。  
  
5.  将此脚本创建的 VHD 复制到物理计算机并将其配置为从此新的 VHD 启动。 要完成该操作，请执行以下步骤：  
  
    1.  装载生成的 VHD。 在此示例中，它将安装在 D:\ 下。  
  
    2.  运行 **bcdboot d:\windows**。  
  
    3.  卸载 VHD。  
  
6.  将物理计算机引导入 Nano Server VHD。  
  
7.  使用在步骤 3 中运行脚本时提供的管理员帐户和密码登录到恢复控制台（请参阅本指南中的“Nano Server 恢复控制台”部分）。
> [!NOTE]  
    > 恢复控制台仅支持基本的键盘功能。 不支持键盘指示灯、10 键部分和键盘布局切换（例如大写锁定和数字锁定）。 
  
8.  获取 Nano Server 计算机的 IP 地址并使用 Windows PowerShell 远程处理或其他远程管理工具连接到虚拟机并对其进行远程管理。  
