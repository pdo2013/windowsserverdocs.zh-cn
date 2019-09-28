---
title: 不带 WSUS 的月度增量更新 ISV 支持
description: Windows Server Update Service （WSUS）主题-独立软件供应商（ISV）如何暂时使用月度增量更新，而不是使用 WSUS Express 更新交付来减小包大小
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 4607827d73c34f50f721a2774fa498eb95f9dbb8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361727"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>不带 WSUS 的月度增量更新 ISV 支持

>适用于：Windows Server （半年频道），Windows Server 2016，Windows 10

Windows 10 更新下载可能会很大，因为每个包都包含以前发布的所有修补程序，以确保一致性和简洁性。  

自第7版起，Windows 已能够使用名为[Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)的功能减小 Windows 更新下载的大小，但使用者设备在默认情况下支持它，windows 10 企业版设备需要 WINDOWS SERVER UPDATE SERVICES （WSUS）才能执行Express 的优势。 如果你有可用的 WSUS，请参阅[快速更新交付 ISV 支持](express-update-delivery-ISV-support.md)。 建议使用它来启用快速更新交付。 

如果当前未安装 WSUS，但在过渡中需要较小的更新包大小，则可以使用每月增量更新。 增量更新显著减小了包大小，但并不像 WSUS Express 更新交付那么多。 建议你尽可能部署 WSUS Express 更新以最大程度地降低包大小。 以下图表比较了 Windows 10 版本1607的增量、累积和快速下载大小：

![下载大小比较](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>什么是月度增量更新？

每月安全更新有两种变体：增量和累积性。

每月增量更新是全新的，并且没有 WSUS 可用于帮助减少更新包大小的 Isv 的临时解决方案。

>[!IMPORTANT]
>**增量更新适用于 Windows 10 版本1607（周年更新）、版本1703（创意者更新）和版本1709（秋季创意者更新）的维护。** 对于版本1709之后的版本，你将需要实现支持[快速更新交付](express-update-delivery-ISV-support.md)的部署基础结构，以便继续利用增量更新。

使用月度增量更新时，包仅包含一个月的更新。 每月累积包含更新版本的所有更新，导致每个月增长一个大文件。 增量和每月更新都在每个月的第二个星期二发布，也称为 "更新星期二"。 下表比较了增量和累积更新：

|                    | 每月**增量**更新                                                                                                                                                                                                       | 每月**累积**更新                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **范围**          | **只包含本月新修补程序**的单个更新                                                                                                                                                                           | 单个更新，其中包含本月和所有前个月的所有新修补程序                                                                                                                                                   |
| **应用程序**    | 只有在应用了上个月的更新后，才能应用（累计或增量）                                                                                                                                           | 可随时应用                                                                                                                                                                                                |
| **传递**       | **仅发布到 Windows 更新目录**，可在其中下载该目录以便与其他工具或进程一起使用。 未提供给连接到 Windows 更新的 Pc                                                         | 已发布到 Windows 更新（所有消费者电脑将安装它）、WSUS 和 Windows 更新目录                                                                                                                |

增量和累计具有相同的 KB 号，具有相同的分类，并同时发布。 可以通过目录中的更新标题或 msu 名称来区分更新：

-  2017-02 *\*\** 适用于基于 x64 的系统的 Windows 10 版本1607的增量更新（KB1234567）
-  2017-02 *\*\** 适用于基于 x86 的系统的 Windows 10 版本1607累积更新（KB1234567）                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>何时使用月度增量更新

如果对客户端设备进行更新的大小是一个问题，我们建议对具有上个月更新的设备使用增量更新，并对落后的设备进行累积更新。 这样一来，所有设备只需要一个更新，就能使它们保持最新状态。 这需要在总体更新管理过程中进行少量调整，因为你将需要根据设备在组织中的最新版本部署不同的更新：

![下载大小比较](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>防止在同一月内部署增量和累积更新

由于增量更新和累积更新同时可用，因此，如果在同一个月部署两个更新，则必须了解发生了什么情况。

如果你批准并部署相同的增量和累积更新版本，则不会仅生成额外的网络流量，因为这两个流量都将下载到电脑，但在重启后，你可能无法将计算机重新启动到 Windows。

如果不小心安装了增量和累积更新，并且计算机不再启动，则可以通过以下步骤进行恢复：

1. 启动到 WinRE 命令提示符
2. 列出处于挂起状态的包：

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **示例**：` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. 打开管道**包**所在的文本文件。 如果看到任何安装挂起的修补程序，请针对每个包名称运行**删除包**：
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **示例**：`x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >不要删除挂起的修补程序。

>[!IMPORTANT]
>**增量更新适用于 Windows 10 版本1607（周年更新）、版本1703（创意者更新）和版本1709（秋季创意者更新）的维护。** 对于版本1709之后的版本，你将需要实现支持[快速更新交付](express-update-delivery-ISV-support.md)的部署基础结构，以便继续利用增量更新。
