---
title: "在服务器上设置 WinSAT 分数"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 911dc494-0f8f-4723-93d6-2106f914b906
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 77866acccac13ac48da8779700c8654f2c7f3277
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="set-the-winsat-score-on-the-server"></a>在服务器上设置 WinSAT 分数

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

你应该运行的 Windows Server Essentials 操作系统优化视频的流分辨率的服务器设置 WinSAT CPU 分数。 通过创建并安装.xml 文件包含 WinSAT 分数信息来执行此操作。  
  
## <a name="obtain-the-winsat-cpu-score"></a>获取 WinSAT CPU 分数  
 称为发现 WinSAT CPU 分数并将该信息放入操作系统阅读 WinServerSAT.xml 文件的 WinServerSAT.exe OPK 系统将提供程序。  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>若要获取 WinSAT CPU 分数  
  
1.  将复制 Resources\WinServerSAT\\ * ADK 媒体中到参考计算机。  
  
2.  在参考计算机上，打开提升了权限的 Command Prompt 窗口。  
  
3.  如果 %ProgramFiles%\Windows Server\Bin\OEM 文件夹不存在，键入以下命令，然后按 Enter。  
  
     **mkdir"%ProgramFiles%\Windows Server\Bin\OEM"**  
  
4.  键入以下命令，然后按 Enter。  
  
     **WinServerSAT.exe"%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
 下面的示例显示了 XML 创建 WinServerSAT.xml 文件的内容。  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 其中*WinSAT_Score*将替换为服务器发现的值。  
  
> [!IMPORTANT]
>  你必须 WinServerSAT.exe、 winsat.prx、 winsat.wmv 和 WinSATEncode.wmv 文件从计算机中删除参考捕获映像之前。
