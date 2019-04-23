---
title: 自定义服务器备份
description: 介绍如何使用 Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838538"
---
# <a name="customize-server-backup"></a>自定义服务器备份

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

## <a name="turn-off-server-backup-by-default"></a>默认情况下关闭服务器备份  
 可以选择在默认情况下关闭服务器备份。 需要将 **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** 的值设置为 1 才能启用此选项。  
  
 当设置了此项时，不会通过仪表板或快速启动板公开服务器备份用户界面。 这使你可以将第三方应用程序用于服务器备份。  
  
#### <a name="to-add-serverbackupproviderdisabled-registry-key-and-set-the-value-to-1"></a>若要添加 ServerBackup\ProviderDisabled？注册表项和设置，则值为 1  
  
1.  在服务器上，依次单击 **“开始”**、**“运行”**，在 **“打开”** 文本框中输入 **regedit**，然后单击 **“确定”**。  
  
2.  在导航窗格中，依次展开 **“HKEY_LOCAL_MACHINE”**、**“SOFTWARE”**、**“Microsoft”**、**“Windows Server”** 和 **“ServerBackup”**。  
  
3.  右键单击 **“ServerBackup”**，单击 **“新建”**，然后单击 **“DWARD 值”**。  
  
4.  对于名称，请输入 **ProviderDisabled**。  
  
5.  右键单击该名称，选择 **“修改”**，输入 **1** 作为值数据，然后单击 **“确定”**。  
  
## <a name="turn-on-server-backup"></a>打开服务器备份  
 如果已通过创建 **ProviderDisabled** 注册表项（见本文档前面所述）关闭“服务器备份”，你可打开“服务器备份”。  
  
 需要删除项 **HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Server\ServerBackup\ProviderDisabled** 以启用默认服务器备份，更改 Windows Server Server Backup Service 的服务启动类型并重新启动服务器。  
  
#### <a name="to-delete-serverbackupproviderdisabled-registry-key"></a>若要删除 ServerBackup\ProviderDisabled？注册表项  
  
1.  在服务器上，将鼠标移动到屏幕右上角，然后单击 **“搜索”**。  
  
2.  在搜索框中，键入 **“regedit”**，然后单击 **“Regedit”** 应用程序。  
  
3.  在导航窗格中，依次展开 **“HKEY_LOCAL_MACHINE”**、**“SOFTWARE”**、**“Microsoft”**、**“Windows Server”** 和 **“ServerBackup”**。  
  
4.  右键单击 **“ProviderDisabled”**，然后单击 **“删除”**。  
  
#### <a name="change-the-start-type-of-windows-server-server-backup-service"></a>更改 Windows Server Server Backup Service 的启动类型  
  
1.  在服务器上，将鼠标移动到屏幕右上角，然后单击 **“搜索”**。  
  
2.  在“搜索框”中，输入 **services.msc**，然后单击 **“服务”** 应用程序。  
  
3.  在服务窗格中，右键单击 **“Windows Server Server Backup Service”**，然后单击 **“属性”**。  
  
4.  在 **“常规”** 选项卡上，选择 **“自动”** 作为 **“启动类型”**。  
  
5.  单击 **“确定”**，关闭对话框。  
  
#### <a name="restart-the-server"></a>重新启动服务器  
  
1.  在服务器上，将鼠标移动到屏幕右上角，然后依次单击 **“设置”**、**“电源”** 和“重新启动”。