---
title: 通过 MultiPoint Services 存储文件
description: 了解 MultiPoint Services 中的文件存储
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c9eb0461-3846-4ddc-97ff-de10f03f30cf
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: bf31f5c582cffb5b38cff8cb15fcfdb4b3f76386
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389296"
---
# <a name="storing-files-with-multipoint-services"></a>通过 MultiPoint Services 存储文件
MultiPoint 服务支持通过下列方式存储用户文件：  
  
-   **在硬盘驱动器的操作系统分区上。** 默认情况下，MultiPoint 服务使用操作系统将用户文件存储在硬盘驱动器上。  
  
-   **位于硬盘驱动器的单独分区中。** 首次设置 MultiPoint 服务系统时，可以对硬盘驱动器进行*分区*。 也就是说，你可以配置驱动器的一部分，使其像是一个单独的驱动器。 这样就可以更轻松地还原或升级操作系统，而不会影响用户文件。 有关详细信息，请参阅 Windows Server 技术库中的[创建分区或逻辑驱动器](https://go.microsoft.com/fwlink/?LinkId=182618)。  
  
-   **在其他内部或外部硬盘驱动器上。** 可以将额外的内部或外部硬盘驱动器附加到 MultiPoint 服务，以便保存和备份数据。  
  
-   **在共享的网络文件夹中。** 要使用户文件在任何工作站上都可用，可以在网络上创建一个共享文件夹。 这除了需要另一台计算机或服务器以外，还需要运行 MultiPoint 服务的计算机。 如果文件服务器可用，则建议使用此方法存储文件。  
  
    对于运行无可用文件服务器的多个2-3 计算机的小型系统，其中一个 MultiPoint 服务计算机可以充当所有 MultiPoint 服务计算机的文件服务器。 然后，你将为作为文件服务器的 MultiPoint 服务上的所有用户创建用户帐户。  
  
