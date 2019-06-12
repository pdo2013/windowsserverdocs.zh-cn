---
title: 在服务器上设置 WinSAT 分数
description: 介绍如何使用 Windows Server Essentials
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
ms.openlocfilehash: 4e5ce037c7a8c802419cd980fc0272c4f687c6a6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433449"
---
# <a name="set-the-winsat-score-on-the-server"></a>在服务器上设置 WinSAT 分数

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

应运行 Windows Server Essentials 操作系统以优化视频流分辨率为服务器设置 WinSAT CPU 分数。 此操作可通过创建和安装包含 WinSAT 分数信息的 .xml 文件来完成。  
  
## <a name="obtain-the-winsat-cpu-score"></a>获取 WinSAT CPU 分数  
 OPK 中提供了一个名为 WinServerSAT.exe 的程序，该程序用于发现 WinSAT CPU 分数并将该信息放入 WinServerSAT.xml 文件中供操作系统读取。  
  
#### <a name="to-obtain-the-winsat-cpu-score"></a>获取 WinSAT CPU 分数  
  
1. 复制 Resources\WinServerSAT\\* 在引用计算机的 ADK 媒体中。  
  
2. 在引用计算机中，打开提升的命令提示符窗口。  
  
3. 如果 %ProgramFiles%\Windows Server\Bin\OEM 文件夹不存在，则输入以下命令，然后按 Enter 键。  
  
    **mkdir "%ProgramFiles%\Windows Server\Bin\OEM"**  
  
4. 输入以下命令，然后按 Enter 键。  
  
    **WinServerSAT.exe "%ProgramFiles%\Windows Server\Bin\OEM\WinServerSAT.xml"**  
  
   以下示例显示了创建的 WinServerSAT.xml 文件的 XML 内容。  
  
```  
  
<?xml version="1.0" encoding="UTF-16"?>  
<WinSAT>  
   <CpuScore description="WinSAT CPU score">WinSAT_Score</CpuScore>  
</WinSAT>  
```  
  
 其中， *WinSAT_Score* 将替换为在服务器上发现的值。  
  
> [!IMPORTANT]
>  捕获映像之前，必须从引用计算机中删除 WinServerSAT.exe、winsat.prx、winsat.wmv 和 WinSATEncode.wmv 文件。
