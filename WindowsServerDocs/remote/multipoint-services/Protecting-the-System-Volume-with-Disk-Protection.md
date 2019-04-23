---
title: 通过磁盘保护保护系统卷
description: 提供有关 MultiPoint 服务的磁盘保护的信息
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 18694665-ed65-4d84-8505-f460cf3df907
author: evaseydl
manager: scotman
ms.author: evas
ms.openlocfilehash: a663bb27a7480066c997844d68b33774f9032e92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855208"
---
# <a name="protecting-the-system-volume-with-disk-protection"></a>通过磁盘保护保护系统卷
MultiPoint 服务提供的选项要立即清除对每次启动计算机的系统卷的任何更改。 如果启用磁盘保护功能，对驱动器，如配置损坏或恶意软件，引入的任何修改将撤消下次重启计算机。 这是一种便捷功能，该管理员想要确保每次加载的已知"好"或"黄金"软件映像。 自动更新或软件修补可以计划，例如，午夜。 规划考虑因素是是否想要具有最终用户将无法进行更改，例如从 Internet 安装软件。 与启用此功能，如果你希望用户能够存储文件，文件共享将需要外部系统卷。  
  
