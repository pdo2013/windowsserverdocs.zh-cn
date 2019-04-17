---
title: "将媒体流式传输设置的更改"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dec690d2-f80c-4b09-99d6-3bba41331972
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d34d60e792fcda4d71a4509fe3d1b95fc6e74d0e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="change-media-streaming-settings"></a>将媒体流式传输设置的更改

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

多个选项都可供你更改流式传输设置的媒体。 有以下选项：  
  
-   [隐藏远程媒体流式传输外接程序](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [设置媒体库名称](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [将视频流式传输的质量设置](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [可编程启用或禁用媒体流式传输](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a>隐藏远程媒体流式传输外接程序  
 你可以隐藏流式传输外接程序中通过的注册表中添加项远程媒体。  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>若要隐藏远程媒体流式传输外接程序  
  
1.  在服务器上，将鼠标移动到屏幕的右上角，然后单击**搜索**。  
  
2.  在**搜索**框中，键入**regedit**，然后单击**Regedit**应用程序。  
  
3.  在左侧窗格中，展开至以下注册表项：  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\ Microsoft \ Windows Server \RemoteAccess\DisabledAddIns**  
  
4.  右键单击**DisabledAddIns**，指向**新建**，然后单击**DWORD 值**。  
  
5.  对于新值，键入**497796c6-9cc7-43be-aa26-4e6b5695370d**，这是标识符流式传输外接程序、远程媒体，然后按**Enter**。  
  
6.  右键单击值，然后单击**修改**。  
  
7.  键入**1**的值数据，然后单击**确定**。  
  
##  <a name="BKMK_LibraryName"></a>设置媒体库名称  
 你可以使用 Windows Server 解决方案 SDK 中类设置媒体库的名称。 若要设置媒体库的名称，你可以使用**SetMediaLibraryName**付款方式**MediaStreamingManager**中类**Microsoft.WindowsServerSolutions.MediaStreaming**命名空间。 下面的示例显示了如何设置媒体库的名称：  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 有关详细信息，请参阅[Windows Server 解决方案 SDK](https://go.microsoft.com/fwlink/?LinkID=248648)。  
  
##  <a name="BKMK_StreamingQuality"></a>将视频流式传输的质量设置  
 你设置的视频流通过获取 WinSAT CPU 分数创建并安装.xml 文件包含 WinSAT 分数信息的质量。 如果设置视频质量的用户界面向客户将不会显示在初始配置运行之前已安装.xml 文件包含 WinSAT 分数信息。 有关详细信息，请参阅[服务器上设置 WinSAT 分数](Set-the-WinSAT-Score-on-the-Server.md)。  
  
##  <a name="BKMK_Program"></a>可编程启用或禁用媒体流式传输  
 你可以使用 Windows Server 解决方案 SDK 中类可编程启用或禁用媒体流式传输。 若要启用或禁用媒体流式传输时，你可以使用**SetMediaStreamingEnabled**付款方式**MediaStreamingManager**中类**Microsoft.WindowsServerSolutions.MediaStreaming**命名空间。 下面的示例代码显示启用媒体流式传输的方法：  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 下面的示例代码显示禁用媒体流式传输的方法：  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>请参阅  
 [创建和自定义映像](Creating-and-Customizing-the-Image.md)   
 [其他自定义设置](Additional-Customizations.md)   
 [准备部署该映像](Preparing-the-Image-for-Deployment.md)   
 [测试客户体验](Testing-the-Customer-Experience.md)