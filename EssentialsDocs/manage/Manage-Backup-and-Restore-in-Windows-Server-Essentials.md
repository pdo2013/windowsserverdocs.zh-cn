---
title: "管理备份和还原在 Windows Server Essentials"
description: "介绍了如何使用 Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="manage-backup-and-restore-in-windows-server-essentials"></a>管理备份和还原在 Windows Server Essentials

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials
 
 Windows Server 软件包提供可靠的各种方式来执行服务器的定期备份和网络计算机的备份。 如果数据丢失，你可以数据从备份中还原成功服务器上而不是将整个计算机还原。 如有必要，你可以执行网络中的全系统还原到服务器或的客户端计算机。 下表介绍了以及他们优点会向你提供的不同备份选项。  
  
|备份的功能|描述|优点|  
|--------------------|-----------------|----------------|  
|服务器备份|运行 Windows Server Essentials 服务器备份。 数据被备份至外部 USB 驱动器。<br /><br /> 有关详细信息，请参阅[管理服务器备份](Manage-Server-Backup-in-Windows-Server-Essentials.md)和[还原或修复服务器](Restore-or-repair-your-server-running-Windows-Server-Essentials.md)。|-可以还原的文件和文件夹服务器上。<br /><br /> -可以执行服务器的全系统的还原。|  
|客户端计算机备份|客户端计算机在网络备份。 在服务器上运行的 Windows Server Essentials 备份数据。<br /><br /> 有关详细信息，请参阅[管理客户端备份](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md)和[从现有的客户端计算机备份中还原完整的系统](Restore-a-full-system-from-an-existing-client-computer-backup.md)。|-可以还原的文件和文件夹服务器。<br /><br /> -可以执行客户端计算机的全系统的还原。|  
| MicrosoftAzure 备份|在服务器上执行的联机备份文件或文件夹。 当你使用 Azure 备份来备份 server 的数据时的信息进行加密通过使用你的密码之前将其上载到 Internet 安全数据中心。<br /><br /> 有关详细信息，请参阅[管理 Online Backup](Manage-Online-Backup-in-Windows-Server-Essentials.md)。|-可以还原的文件和文件夹服务器。<br /><br /> -使用增量备份，仅对文件进行更改转移到云中。<br /><br /> -备份存储在 Microsoft Azure，并且现场减少地保护和保护现场备份的媒体。|  
  
## <a name="see-also"></a>请参阅  
  
-   [管理 Windows Server Essentials](Manage-Windows-Server-Essentials.md)  
  
-   [使用 Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)
