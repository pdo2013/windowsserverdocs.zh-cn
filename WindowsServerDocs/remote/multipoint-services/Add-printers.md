---
title: 添加打印机
description: 添加你的 MultiPoint 服务用户的打印机。
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1f6d3ca-8caf-4aa0-b0ea-93cdfd3f3f37
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: cce64f8cfbfb9d3b9984a0732d76418eea9dc9bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846058"
---
# <a name="add-printers"></a>添加打印机
使用本主题中的过程以向 MultiPoint 服务系统上的所有用户提供本地打印机。  
  
> [!NOTE]  
> 如果与 MultiPoint 服务使用域帐户，用户可以从其工作站使用任何网络打印机。  
  
1.  将打印机连接到 Multipoint server。  
  
2.  配置为共享打印机的打印机：  
  
    1.  以管理员身份登录到 MultiPoint Server 计算机。  
  
    2.  从“开始”  屏幕打开“控制面板” 。  
  
    3.  在控制面板中，单击**硬件**，然后单击**设备和打印机**。  
  
    4.  下**打印机和传真**，右键单击打印机，然后单击**打印机属性**。  
  
    5.  单击**共享**选项卡。  
  
    6.  单击**共享此打印机**，指定打印机的共享名称，然后单击**确定**。  
  
登录到已连接到 Multipoint 服务计算机的任何工作站用户将能够查看和使用打印机。 