---
title: Windows Server Essentials 日志收集器错误疑难解答
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fa2e1685-31c0-4d4f-a10a-6c8885dfc493
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e92236c8e5d956b50f657ebcbe1a942b5841fded
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836658"
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Windows Server Essentials 日志收集器错误疑难解答

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

运行日志收集器时，你可能会遇到以下错误之一。 若要解决某个问题，请按照针对相关联的错误提供的指南进行操作。  
  
> [!NOTE]
>  在本文档中，不是服务器，在网络上的计算机称为网络计算机。？  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a> 目标文件夹不是有效  
 **原因：** 尝试复制日志文件的文件夹可能不存在，或者可能没有足够的空间来保存这些文件。  
  
 **解决方案：** 检查所选的文件夹是否存在以及驱动器上是否有足够的可用空间用于保存这些文件。 你还应该确保驱动器上的临时文件夹中剩余了足够的可用空间。  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a> 发生了网络错误  
 **原因：** 网络计算机或服务器上可能存在与网络相关的问题。  
  
 **解决方案：** 确保所有计算机和网络设备的电源都已打开，并且它们已连接到网络。 如果你无法解决此问题，请联系你的网络维护人员以获取帮助。  
  
###  <a name="BKMK_CannotCollectLogFiles"></a> 无法收集计算机的日志文件  
 **原因：** 由于计算机未使用“将计算机连接到服务器”向导成功连接到服务器，日志收集器可能未安装在该计算机上。  
  
 **解决方案：** 有关如何解析为你的服务器的连接有关的问题的信息，请参阅[连接到服务器的计算机进行故障排除](https://go.microsoft.com/fwlink/p/?LinkID=241492)？ (https://go.microsoft.com/fwlink/p/?LinkID=241492) Microsoft 网站上。  
  
 如果你仍然无法将计算机连接到服务器，那么你可以将日志文件手动复制到 U 盘，如下所示：  
  
-   对于运行 Windows 7、Windows 8 或 Windows Multipoint Server 的客户端计算机，你可以复制位于 **%sysdir%\programdata\Microsoft\Windows Server** 的“日志” 文件夹。  
  
###  <a name="BKMK_YouDoNotHavePermission"></a> 没有将日志文件保存到所选文件夹的权限  
 **原因：** 你可能不具有对用于保存日志文件的所选文件夹的写入权限。  
  
 **解决方案：** 如果你使用默认路径保存日志文件，请确保你具有对共享文件夹的写入权限 **\\ \\< 服务器名\>\Logs**。 如果你要在网络计算机上存储日志，请确保你拥有对用于保存日志文件的所选文件夹的写入权限。  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a> 计算机未正确配置以收集日志文件  
 **原因：** 计算机未针对日志收集器进行正确配置。  
  
 **解决方案：** 重新安装日志收集器。 请参阅 [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a> 出现未知的错误  
 **原因：** 未知。  
  
 **解决方法 1:** 重新安装日志收集器。 如果再次发生错误，请确保不存在连接性问题。 你还可以尝试重新安装日志收集器。 请参阅 [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果你无法解决此问题，请联系你的网络维护人员以获取帮助。  
  
 **解决方案 2:** 首先，尝试打开保存日志文件的文件夹。 如果已生成带有计算机名称的 zip 文件，则忽略此错误并改用日志文件。 如果未生成日志文件，则重新运行日志收集器。 如果再次发生错误，请确保不存在连接性问题。 你还可以尝试重新安装日志收集器。 请参阅 [Reinstalling the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果你无法解决此问题，请联系你的网络维护人员以获取帮助。