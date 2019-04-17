---
title: 清除利用率数据
description: 本主题介绍的 IP 地址管理 (IPAM) 管理指南中的 Windows Server 2016 的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 45cada9e-69b9-43df-b6f5-6d3942435809
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c41be119099aed4867df1bae1a55e2fbaa5c9064
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="purge-utilization-data"></a>清除利用率数据

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用本主题以了解如何从 IPAM 数据库中删除利用率数据。  

你必须成员**IPAM 管理员**，在本地计算机**管理员**组中或等效要执行此过程。

## <a name="to-purge-the-ipam-database"></a>若要清除 IPAM 数据库  
1. 打开服务器管理器，然后浏览到 IPAM 客户端界面。
2. 浏览到以下位置之一：**IP 地址阻止**，**IP 地址库存**，或**IP 地址范围组**。  
3. 单击**任务**，然后单击**清除利用率数据**。 **清除利用率数据**对话框中打开。
4. 在**清除所有利用率数据当日或之前**，单击**选择日期**。
5. 选择你想要删除所有数据库记录在和日期之前的日期。
6. 单击**确定**。 IPAM 删除了你所指定的全部记录。
