---
title: "自定义服务器备份"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 19b2559c-6090-45af-9a08-2eefc28473c8
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d18dca276bccdf672664a5a3c2bd28e0221fff94
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="customize-server-backup"></a>自定义服务器备份

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>默认情况下服务器备份关闭  
 你可以选择默认情况下关闭服务器备份。 你需要设置的值**HKEY_LOCAL_MACHINE\Software\ Microsoft \ Windows Server \ServerBackup\ProviderDisabled**为 1 以启用该选项。  
  
 此项设置，当通过仪表板或启动栏不会公开服务器备份用户界面。 这允许你使用第三方服务器备份的应用程序。  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>若要添加 ServerBackup\ProviderDisabled？注册表项，并设置为 1 的值  
  
1.  在服务器上，单击**开始**，单击**运行**，类型**regedit**中**打开**文本框中，然后单击**确定**。  
  
2.  在导航窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，展开**Windows Server**，然后展开**服务器备份**。  
  
3.  右键单击**服务器备份**，单击**新建**，然后单击**DWARD 值**。  
  
4.  对于名称、输入**ProviderDisabled**。  
  
5.  右键单击该名称、选择**修改**，输入**1**的值数据，然后单击**确定**。  
  
## <a name="turn-on-server-backup"></a>打开 Server 备份  
 你可以打开服务器备份如果振动已关闭通过创建**ProviderDisabled**注册表项（如上文所述）。  
  
 你需要删除项**HKEY_LOCAL_MACHINE\Software\ Microsoft \ Windows Server \ServerBackup\ProviderDisabled**以启用默认服务器备份、更改 Windows Server 服务器备份服务服务开始键入和重新启动的服务器。  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>若要删除 ServerBackup\ProviderDisabled？注册表项  
  
1.  在服务器上，将鼠标移动到屏幕的右上角，然后单击**搜索**。  
  
2.  在搜索框中，键入**regedit**，然后单击**Regedit**应用程序。  
  
3.  在导航窗格中，展开**HKEY_LOCAL_MACHINE**，展开**软件**，展开**Microsoft**，展开**Windows Server**，然后展开**服务器备份**。  
  
4.  右键单击**ProviderDisabled**，然后单击**删除**。  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>更改 Windows Server 服务器备份服务开始类型  
  
1.  在服务器上，将鼠标移动到屏幕的右上角，然后单击**搜索**。  
  
2.  在搜索框中，键入**services.msc**，然后单击**服务**应用程序。  
  
3.  在服务窗格中，右键单击**Windows Server 服务器备份服务**，然后单击**属性**。  
  
4.  在**常规**选项卡上，选择**自动**为**启动类型**。  
  
5.  单击**确定**关闭对话框。  
  
#### <a name="restart-the-server"></a>重新启动服务器  
  
1.  在服务器上，将鼠标移动到屏幕的右上角单击**设置**，单击**电源**，然后单击重启。