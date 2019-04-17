---
title: "运行 Windows Server Essentials 日志收集"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="run-the-windows-server-essentials-log-collector"></a>运行 Windows Server Essentials 日志收集
你可以在网络从服务器或计算机中运行 Windows Server Essentials 日志收集。 如果在运行从服务器日志收集，你可以仅从服务器收集日志。 如果你从网络计算机运行收集日志，你可以选择从服务器，除了对该计算机日志收集日志。  
  
 你必须相应的管理权限才能运行日志收集。 如果你收集日志文件服务器，你必须服务器管理员;如果收集日志网络计算机上的文件，，必须对该计算机的客户端管理员。  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>若要通过向导服务器上运行日志收集  
  
1.  在**开始**页面的服务器上，单击**Windows Server Essentials 日志收集**。  
  
    > [!NOTE]
    >  -   如果日志收集程序未显示在**开始**页上，浏览到**%system%\Program 文件 (x86) \ Windows Server Essentials 日志收集**，然后双击**LogCollector**。  
    > -   如果你未登录到使用管理权限服务器，日志收集会提示你输入你的凭据。  
  
2.  当系统提示您收集的日志文件的保存位置时，你可以选择默认位置，**\\\ < ServerName\ > \logs**，或指定其他位置。 若要接受默认位置，请单击**下一步**。 若要更改的位置，请单击**浏览**，导航到你要在其中保存日志文件的文件夹，然后单击**保存**。  
  
    > [!NOTE]
    >  你不需要提供用于日志文件的文件名。 在日志器通过连接的计算机名称和文件的时间戳命名 zip 文件集锦。  
  
3.  进度栏时正在收集日志显示。  
  
4.  若要查看日志集锦文件的内容，请选择**打开文件位置保存日志**复选框，然后单击**关闭**以关闭向导并打开集锦日志文件。  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>若要使用该向导通过网络计算机上运行日志收集  
  
1.  浏览到**%system%\Program 文件 (x86) \ Windows Server Essentials 日志收集**，然后双击该文件**LogCollector.exe**。  
  
    > [!NOTE]
    >  如果你未登录到具有管理员权限的网络计算机，输入你的用户名和密码提示时，，然后单击**下一步**。  
  
2.  选择你想要收集，如下所示的日志：  
  
    1.  选择**服务器日志文件**收集日志文件服务器上的复选框。  
  
    2.  **客户端计算机日志文件（这台计算机）**默认情况下，用于指示日志回收将从运行的网络计算机中收集日志选中复选框。 如果你只希望收集服务器日志，则清除**客户端计算机日志文件（这台计算机）**复选框。  
  
    3.  单击**下一步**。  
  
3.  出现提示时，键入服务器的管理员用户名和密码，然后单击**下一步**。  
  
4.  键入或浏览你想要保存日志文件中，然后单击到的位置**下一步**。  
  
    > [!NOTE]
    >  你不需要提供用于日志文件的文件名。 在日志器通过连接的计算机名称和文件的时间戳命名 zip 文件集锦。  
  
5.  进度栏时正在收集日志显示。  
  
6.  若要查看日志集锦文件的内容，请选择**打开文件位置保存日志**复选框，然后单击**关闭**以关闭向导并打开集锦日志文件。  
  
### <a name="running-the-log-collector-manually"></a>手动运行日志收集  
 日志回收安装后，会创建了计划的任务运行该工具。 随后，你可以运行从日志收集**计划任务管理器**而无需使用该向导中，如果有启动该向导的问题。  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>若要手动服务器上运行日志收集  
  
1.  登录直接或远程服务器。  
  
2.  打开**任务计划程序**。  
  
3.  根处**任务计划程序库**，浏览到名为的计划任务**LogCollector**。  
  
4.  右键单击**LogCollector**，然后单击**运行**。 日志回收服务器，在默认文件夹中的地方日志**\\\ < ServerName\ > \Logs**。 如果您没有该文件夹中，写入权限或文件夹不存在，日志放置在**< temp\ >**子目录。  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>若要手动网络计算机上运行日志收集  
  
1.  登录直接或远程计算机的网络。  
  
2.  打开**任务计划程序**。  
  
3.  根处**任务计划程序库**，浏览到名为的计划任务**LogCollector**。  
  
4.  右键单击**LogCollector**，然后单击**运行**。 日志收集的位置中的日志**< temp\ >**网络计算机上的文件夹。
