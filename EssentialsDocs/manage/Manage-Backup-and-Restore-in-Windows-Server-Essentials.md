---
title: 管理 Windows Server Essentials 中的备份和还原
description: 介绍如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41000915-f6ff-4dbb-b7be-629ef36386d4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6f6f0d27472664cd1cc538897d3d525fad506282
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828518"
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>管理 Windows Server Essentials 中的备份和还原

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 Windows Server Essentials 提供了可靠的方法来执行你的服务器的常规备份和网络计算机的备份。 发生数据丢失时，你可以从服务器上的成功备份中恢复数据，而无需还原整个计算机。 如有必要，你可以对网络中的服务器或客户端计算机执行完整的系统还原。 下表描述了对你可用的不同备份选项及它们的优点。  
  
|备份功能|描述|优点|  
|--------------------|-----------------|----------------|  
|服务器备份|备份运行 Windows Server Essentials 的服务器。 将数据备份到外部 USB 驱动器。<br /><br /> 有关详细信息，请参阅[管理服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md)并[还原或修复服务器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md)。|-可以还原文件和你的服务器上的文件夹。<br /><br /> -可以执行你的服务器的完整系统还原。|  
|Client Computer Backup|备份网络中的客户端计算机。 在运行 Windows Server Essentials 的服务器上备份数据。<br /><br /> 有关详细信息，请参阅[管理客户端备份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)并[通过现有的客户端计算机备份还原完整系统](Restore-a-full-system-from-an-existing-client-computer-backup.md)。|-可以还原文件和文件夹从你的服务器。<br /><br /> -可以执行客户端计算机的完整系统还原。|  
| Microsoft Azure 备份|在服务器上执行文件或文件夹的联机备份。 使用 Azure 备份来备份服务器数据，当信息进行加密使用你的密码，然后将其上载到 Internet 上的安全数据中心。<br /><br /> 有关详细信息，请参阅[管理联机备份](Manage-Online-Backup-in-Windows-Server-Essentials.md)。|-可以还原文件和文件夹从你的服务器。<br /><br /> -使用增量备份时，只有文件更改传输到云。<br /><br /> -备份存储在 Microsoft Azure 中并且是现场，减少了保护现场备份媒体的需要。|  
  
## <a name="see-also"></a>请参阅  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
