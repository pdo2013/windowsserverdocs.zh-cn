---
title: 通过 MultiPoint Services 存储文件
description: 了解有关 MultiPoint Services 中的文件存储
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: b432ca793b156997761f9fadab7340c394e3b553
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817338"
---
# <a name="storing-files-with-multipoint-services"></a>通过 MultiPoint Services 存储文件
MultiPoint 服务支持通过以下方式存储用户文件：  
  
-   **在硬盘驱动器上的操作系统分区。** 默认情况下，MultiPoint 服务将存储在与操作系统的硬盘驱动器上的用户文件。  
  
-   **在硬盘驱动器上的单独的分区。** 当首次设置 MultiPoint 服务系统时，你可以*分区*硬盘驱动器。 也就是说，可以配置在驱动器的部分，以便该方法的功能，就好像单独的驱动器。 这使得更轻松地还原或操作系统升级而不会影响用户文件。 有关详细信息，请参阅[创建分区或逻辑驱动器](https://go.microsoft.com/fwlink/?LinkId=182618)Windows Server 技术库中。  
  
-   **其他内部或外部硬盘驱动器上。** 可以将其他内部或外部硬盘驱动器附加到 MultiPoint 服务，用于保存和备份数据。  
  
-   **在共享的网络文件夹。** 若要使用户文件从任何工作站，可以在网络上创建共享的文件夹。 这要求另一台计算机或服务器以及运行 MultiPoint 服务的计算机。 这是用于存储文件，如果没有可用的文件服务器的建议的方法。  
  
    对于小型的 2-3 计算机无可用的文件服务器与运行 MultiPoint 服务系统，其中一台 MultiPoint 服务计算机可以充当文件服务器的所有 MultiPoint 服务计算机。 然后会在 MultiPoint 服务充当文件服务器上创建所有用户的用户的帐户。  
  
