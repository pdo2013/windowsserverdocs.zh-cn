---
title: "Windows Server Essentials 日志收集错误疑难解答"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-windows-server-essentials-log-collector-errors"></a>Windows Server Essentials 日志收集错误疑难解答

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

日志回收运行时，可能会遇到的以下错误之一。 若要解决问题，请按照本指南提供相关的错误。  
  
> [!NOTE]
>  在本文中，你的网络，而不是你的服务器上的计算机称为网络计算机。？  
  
###  <a name="BKMK_TheDestinationFolderIsNotValid"></a>不正确的目标文件夹  
 **原因：**要复制日志文件的文件夹可能不存在，或者可能没有足够空间来容纳文件。  
  
 **解决方法：**检查是否存在选定的文件夹，没有足够的可用空间可用的文件的驱动器上。 你还应确保没有足够的可用空间保留在驱动器上的临时文件夹中。  
  
###  <a name="BKMK_ANetworkErrorHasOccurred"></a>网络出错了  
 **原因：**可能网络计算机或服务器上与网络有关的问题。  
  
 **解决方法：**确保所有计算机和网络的设备都已打开都电源，并且在连接到该网络。 如果你不能解决该问题，请联系维护你的网络以获得帮助的人员。  
  
###  <a name="BKMK_CannotCollectLogFiles"></a>不能收集计算机日志文件  
 **原因：**日志收集可能不会安装在计算机上因为计算机所做的那样不成功使用连接到服务器计算机连接到服务器向导。  
  
 **解决方法：**有关如何解决问题有关连接到服务器的信息，请参阅[将计算机连接到服务器的疑难解答](https://go.microsoft.com/fwlink/p/?LinkID=241492)？ (https://go.microsoft.com/fwlink/p/?LinkID=241492) 到 Microsoft 网站上。  
  
 如果你仍然无法连接到服务器的计算机，然后你可以日志文件手动复制到 U 盘，如下所示：  
  
-   对于运行 Windows 7、Windows 8 或 Windows 多点服务器的客户端计算机，你可以将复制**日志**文件夹位于**%sysdir%\programdata\ Microsoft \ Windows Server**。  
  
###  <a name="BKMK_YouDoNotHavePermission"></a>没有保存到所选文件夹日志文件权限  
 **原因：**你可能未安装写入权限到您选择保存日志文件的文件夹。  
  
 **解决方法：**如果使用的默认路径保存日志文件，确保你拥有的共享文件夹写权限**\\\ < ServerName\ > \Logs**。 如果你的网络计算机上存储日志，请确保你已为你选择保存日志文件的文件夹写权限。  
  
###  <a name="BKMK_TheComputerIsNotConfiguredProperly"></a>计算机未配置无误收集日志文件  
 **原因：**计算机已未正确配置日志回收。  
  
 **解决方法：**重新安装日志收集。 请参阅[重新安装日志收集](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。  
  
###  <a name="BKMK_AnUnknownErrorOccurred"></a>未知出错  
 **原因：**未知。  
  
 **解决方案 1:**重新运行日志收集。 如果再次出现错误，请确保没有连接的问题。 你还可以尝试重新安装日志收集。 请参阅[重新安装日志收集](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果你不能解决该问题，请联系维护你的网络以获得帮助的人员。  
  
 **解决方案 2:**首先，尝试打开保存日志文件的文件夹。 如果已生成的计算机名称 zip 文件，则忽略此错误，以及改为使用日志文件。 如果没有产生日志文件，重新运行日志收集。 如果再次出现错误，请确保没有连接的问题。 你还可以尝试重新安装日志收集。 请参阅[重新安装日志收集](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_Reinstall)。 如果你不能解决该问题，请联系维护你的网络以获得帮助的人员。