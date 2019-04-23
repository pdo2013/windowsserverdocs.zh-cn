---
title: 清除利用率数据
description: 本主题是 Windows Server 2016 中的 IP 地址管理 (IPAM) 管理指南的一部分。
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
ms.openlocfilehash: 7b471417d4c44c22f115443f1f2dcca6f351e6f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827938"
---
# <a name="purge-utilization-data"></a>清除利用率数据

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用本主题，了解如何从 IPAM 数据库中删除利用率数据。  

您必须是属于**IPAM 管理员**，在本地计算机**管理员**组或同等身份才能执行此过程。

## <a name="to-purge-the-ipam-database"></a>若要清除 IPAM 数据库  
1. 打开服务器管理器，然后浏览到 IPAM 客户端接口。
2. 浏览到以下位置之一：**IP 地址块**， **IP 地址清单**，或**IP 地址范围组**。  
3. 单击**任务**，然后单击**清除利用率数据**。 **清除利用率数据**对话框随即打开。
4. 在**清除所有的使用率数据或之前**，单击**选择一个日期**。
5. 选择你想要删除所有数据库记录和之前的日期的日期。
6. 单击 **“确定”**。 IPAM 中删除具有指定的所有记录。
