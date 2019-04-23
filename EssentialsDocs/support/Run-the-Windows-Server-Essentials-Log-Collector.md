---
title: 运行 Windows Server Essentials 日志收集器
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6b49fee7ca4a19d5a501cf96c1ce356f8242c81f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830918"
---
# <a name="run-the-windows-server-essentials-log-collector"></a>运行 Windows Server Essentials 日志收集器
在网络上，可以从服务器或计算机运行 Windows Server Essentials 日志收集器。 如果你从服务器运行日志收集器，则仅可以从该服务器收集日志。 如果你从网络计算机运行日志收集器，则除了该计算机的日志之外，你还可以选择从该服务器收集日志。  
  
 你必须具有相应的管理权限才能运行日志收集器。 如果你收集服务器的日志文件，则你必须是服务器管理员；如果你收集网络计算机上的日志文件，则你必须是该计算机的客户端管理员。  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>使用向导在服务器上运行日志收集器的步骤  
  
1.  上**启动**页上的服务器上，单击**Windows Server Essentials Log Collector**。  
  
    > [!NOTE]
    >  -   如果日志收集器程序不会显示在**启动**页上，浏览到 **%system%\Program 文件 (x86) \Windows Server Essentials Log Collector**，然后双击**LogCollector**.  
    > -   如果你未使用管理权限登录到该服务器，则日志收集器将提示你输入你的凭据。  
  
2.  系统提示输入用于保存收集的日志文件的位置，可以选择默认位置 **\\ \\< 服务器名\>\logs**，或指定其他位置。 若要接受默认位置，请单击 **“下一步”**。 若要更改位置，请单击 **“浏览”**，导航到要保存日志文件的文件夹，然后单击 **“保存”**。  
  
    > [!NOTE]
    >  你无需提供日志文件的文件名。 日志收集器通过连接的计算机名称和文件的时间戳来命名 zip 文件集合。  
  
3.  收集日志时将显示进度栏。  
  
4.  若要查看日志集文件的内容，请选中 **“打开保存日志的文件位置”** 复选框，然后单击 **“关闭”** 以关闭向导并打开日志集合文件。  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>使用向导在网络计算机上运行日志收集器的步骤  
  
1.  浏览到 **%system%\Program 文件 (x86) \Windows Server Essentials Log Collector**，然后双击文件**LogCollector.exe**。  
  
    > [!NOTE]
    >  如果你未使用管理权限登录到网络计算机，请在收到提示时输入用户名和密码，然后单击 **“下一步”**。  
  
2.  选择你要收集的日志，如下所示：  
  
    1.  选中 **“服务器日志文件”** 复选框以收集服务器上的日志文件。  
  
    2.  默认选中 **“客户端计算机日志文件(此计算机)”** 复选框，指示日志收集器将从运行的网络计算机收集日志。 如果你只希望收集服务器日志，请清除 **“客户端计算机日志文件(此计算机)”** 复选框。  
  
    3.  单击“下一步” 。  
  
3.  当收到提示时，请键入服务器管理员的用户名和密码，然后单击 **“下一步”**。  
  
4.  键入或浏览到要保存日志文件的位置，然后单击 **“下一步”**。  
  
    > [!NOTE]
    >  你无需提供日志文件的文件名。 日志收集器通过连接的计算机名称和文件的时间戳来命名 zip 文件集合。  
  
5.  收集日志时将显示进度栏。  
  
6.  若要查看日志集文件的内容，请选中 **“打开保存日志的文件位置”** 复选框，然后单击 **“关闭”** 以关闭向导并打开日志集合文件。  
  
### <a name="running-the-log-collector-manually"></a>手动运行日志收集器  
 安装日志收集器后，将创建计划任务以运行该工具。 随后你可以从 **“计划任务管理器”** 运行日志收集器，而无需使用向导（如果启动向导时遇到问题）。  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>在服务器上手动运行日志收集器的步骤  
  
1.  直接或远程登录到服务器。  
  
2.  打开 **“任务计划程序”**。  
  
3.  在 **“任务计划程序库”** 的根目录中，浏览到名为 **“LogCollector”** 的计划任务。  
  
4.  右键单击 **“LogCollector”**，然后单击 **“运行”**。 日志收集器会将日志放置在服务器上，在默认文件夹 **\\ \\< 服务器名\>\Logs**。 如果您没有写入权限的文件夹或文件夹不存在，则日志将放置 **< temp\>** 子目录。  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>在网络计算机上手动运行日志收集器的步骤  
  
1.  直接或远程登录到网络计算机。  
  
2.  打开 **“任务计划程序”**。  
  
3.  在 **“任务计划程序库”** 的根目录中，浏览到名为 **“LogCollector”** 的计划任务。  
  
4.  右键单击 **“LogCollector”**，然后单击 **“运行”**。 日志收集器会将日志中的放置 **< temp\>** 网络计算机上的文件夹。
