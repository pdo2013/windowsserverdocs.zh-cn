---
title: 启用文件共享
description: 了解 MultiPoint Services 中的文件共享
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 508ad056-8e0c-4d59-a4fa-05775a54125d
author: evaseydl
ms.author: evas
manager: Scottman
ms.openlocfilehash: aff6501d129696fd78d58bdac52f5a4a9ef54c63
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395449"
---
# <a name="enable-file-sharing-in-multipoint-services"></a>在 MultiPoint 服务中启用文件共享
可以通过两种方式允许 MultiPoint 工作站上的用户共享文件：  
  
-   **如果网络中有文件服务器，** 建议您在文件服务器上创建一个共享文件夹。  
  
-   **如果你的小型网络为2-3 多点服务器，但没有专用的文件服务器，** 则其中一个 multipoint 服务器可充当所有其他运行 multipoint 服务的计算机的文件服务器。 在该服务器上创建一个共享文件夹，然后为该服务器上的所有用户创建本地用户帐户。 共享文件夹可以位于原始内部驱动器上，也可以将其他内部或外部驱动器连接到计算机。  
  
