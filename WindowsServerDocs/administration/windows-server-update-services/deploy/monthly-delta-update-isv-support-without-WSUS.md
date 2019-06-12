---
title: 每月的增量更新而无需 WSUS ISV 支持
description: Windows Server Update Service (WSUS) 主题-如何独立的软件供应商 (ISV) 可以暂时使用每月的增量更新而不是 WSUS Express 更新交付来降低包大小
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
author: sakitong
ms.author: coreyp
manager: dougkim
ms.date: 10/16/2017
ms.openlocfilehash: 272d3865bbe1a9853f5349c5e878155351525ef0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439938"
---
# <a name="monthly-delta-update-isv-support-without-wsus"></a>每月的增量更新而无需 WSUS ISV 支持

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows 10

Windows 10 更新的下载可能很大，因为每个包包含所有先前发布的修补程序，以确保一致性和简洁性。  

之后的版本 7，Windows 便提供了名为的功能，降低 Windows 更新下载的大小[Express](https://technet.microsoft.com/library/cc708456(v=ws.10).aspx#Anchor_2)，并且尽管消费型电子设备支持它默认情况下，Windows 10 企业版设备需要 Windows Server Update服务 (WSUS)，以利用 Express。 如果有可用的 WSUS，请参阅[Express 更新交付 ISV 支持](express-update-delivery-ISV-support.md)。 我们建议使用它来启用快速更新交付。 

如果你当前没有 WSUS 安装，但需要较小的更新包大小，可以使用每月的增量更新。 增量更新但没有与使用 WSUS Express 更新交付明显降低包大小。 我们建议您部署 WSUS Express 更新，只要有可能的包大小的最大减少。 下面是适用于 Windows 10 版本 1607年比较增量、 累计和 Express 下载大小的图表：

![下载大小比较](../../media/express-update-delivery-isv-support/delta-1.png)

## <a name="what-is-monthly-delta-update"></a>什么是每月的增量更新？

有两种变体的每月安全更新：增量和累积。

每月的增量更新是新的并更新包大小不具有可用于帮助减少 WSUS 的 Isv 的临时解决方案。

>[!IMPORTANT]
>**增量更新是适用于服务的 Windows 10，版本 1607 （周年更新），版本 1703 （创意者更新），以及版本 1709 (Fall Creators Update)。** 后的版本 1709年的版本中，你将需要实现一个支持的部署基础结构[Express 更新交付](express-update-delivery-ISV-support.md)继续利用增量更新。

通过使用每月的增量更新，包将仅包含一个月的更新。 每月的累计包含导致大型文件的增长每月最多的更新版本中，所有更新。 在每个月，也称为"更新 Tuesday。"的第二个星期二发布的增量和每月更新 下表比较了增量和累积更新：

|                    | 每月**增量**更新                                                                                                                                                                                                       | 每月**累计**更新                                                                                                                                                                                             |
|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **范围**          | 与单个更新**该月的唯一的新修补程序**                                                                                                                                                                           | 与为该月和所有前几个月的所有新修补程序的单个更新                                                                                                                                                   |
| **应用程序**    | 仅可在上一个月的更新时应用 （累计或增量）                                                                                                                                           | 可以在任何时候应用                                                                                                                                                                                                |
| **传递**       | **发布到 Windows 更新目录仅**，可以下载它以用于其他工具或进程。 不会提供给连接到 Windows 更新的电脑                                                         | 发布到 Windows 更新 （所有使用者的 Pc 将安装它）、 WSUS 和 Windows 更新目录                                                                                                                |

增量和累计具有相同的 KB 数，具有相同的分类，并同时发布。 更新可以通过在目录中，更新标题或 msu 的名称来区分：

- 2017-02 *\***增量更新**\**  针对基于 x64 的系统 (KB1234567) 的 Windows 10 版本 1607年
- 2017-02 *\***累积更新**\**  针对基于 x86 的系统 (KB1234567) 的 Windows 10 版本 1607年                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      

### <a name="when-to-use-monthly-delta-update"></a>何时使用每月的增量更新

如果客户端设备的更新的大小是一个问题，我们建议在具有上一个月的更新和累积更新正落在后面的设备上的设备上使用增量更新。 这样一来，所有设备只都需要单个更新，以使其保持最新。 这需要的小调整在整体更新管理过程，因为您将必须部署基于组织中设备的更新的不同更新：

![下载大小比较](../../media/express-update-delivery-isv-support/delta-2.png)

### <a name="prevent-deployment-of-delta-and-cumulative-updates-in-the-same-month"></a>阻止部署的同一月份中的增量和累计更新

由于在同一时间都可以增量更新和累积更新，务必要了解会发生什么情况如果部署在同一个月中的两个更新。

如果批准并部署相同的增量和累积更新版本，您不会仅生成额外的网络流量因为均将下载到计算机，但您可能不能重启后重新启动到 Windows 计算机。

如果无意中安装的增量和累积更新，并且无法再启动您的计算机，您可以通过执行以下步骤进行恢复：

1. 引导到 WinRE 命令提示符
2. 列出处于挂起状态的包：

    `x:\windows\system32\dism.exe /image:<drive letter for windows directory> /Get-Packages >> <path to text file>`
 
    > **示例**: ` x:\windows\system32\dism.exe /image:c:\ /Get-Packages >> c:\temp\packages.txt`
 
3. 打开的文本文件，其中你通过管道传递**get 包**。 如果您看到的任何挂起的修补程序安装，运行**删除包**对于每个包名称：
 
   `dism.exe /image:<drive letter for windows directory> /remove-package /packagename:<package name>`
 
    > **示例**: `x:\windows\system32\dism.exe /image:c:\ /remove-package /packagename:Package_for_KB4014329~31bf3856ad364e35~amd64~~10.0.1.0`
 
    >[!NOTE]
    >不要删除挂起的修补程序卸载。

>[!IMPORTANT]
>**增量更新是适用于服务的 Windows 10，版本 1607 （周年更新），版本 1703 （创意者更新），以及版本 1709 (Fall Creators Update)。** 后的版本 1709年的版本中，你将需要实现一个支持的部署基础结构[Express 更新交付](express-update-delivery-ISV-support.md)继续利用增量更新。
