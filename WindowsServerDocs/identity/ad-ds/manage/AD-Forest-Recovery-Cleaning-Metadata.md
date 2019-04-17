---
title: "广告森林恢复-清洁已删除 dc 的元数据中"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: e7543381-4081-407f-adad-a9de792c6616
ms.technology: identity-adfs
ms.openlocfilehash: 3027c59b58801b44d20127e6bcf62dd7319708bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---cleaning-metadata-of-removed-writable-domain-controllers"></a>广告森林恢复-清理删除可写的域控制器的元数据中 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
 
 清除元数据中删除将识别到复制系统 DC 的 Active Directory 数据。  
  
 使用下面的过程的计划将重新添加到网络由重新安装广告 DS 的 Dc 删除 DC 对象。  
  
 如果你使用的版本的 Active Directory 用户和计算机或 Active Directory 的站点和服务，包含远程服务器管理工具 (RSAT)，当您删除 DC 对象元数据清理将自动执行。  
  

## <a name="deleting-a-domain-controller-using-active-directory-users-and-computers"></a>删除 Active Directory 用户和计算机使用域控制器  
 当你使用该版本的 Active Directory 用户和计算机或 Active Directory 管理中心远程服务器管理工具 (RSAT) 中时，元数据进行清理自动删除 DC 对象时。 服务器对象并计算机对象也会自动删除。  
  
 或者，你还可以使用 Active Directory 的站点和服务中 RSAT 删除 DC 对象。 如果你使用 Active Directory 的站点和服务，您必须之前，你可以删除 DC 对象删除对象关联的服务器和各自的。  
  
 若要下载 RSAT:  

-   [远程服务器管理工具适用于 Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)
  
-   [远程服务器管理工具为 Windows 8](https://www.microsoft.com/download/details.aspx?id=28972)  

-   [远程服务器管理工具适用于 Windows 7 Service pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=7887)  
  
-   [Microsoft 远程服务器管理工具适用于 Windows Vista](https://www.microsoft.com/download/details.aspx?id=21090)  
  
 以下过程均将一致的 Dc 运行的任一 Windows Server 2016、2012 年、2008 R2 或 2008 年。 元数据清理操作目标直流可以运行任何版本的 Windows Server。  
  
### <a name="to-delete-a-domain-controller-object-using-active-directory-users-and-computers-in-rsat"></a>若要删除 RSAT 在使用 Active Directory 用户和计算机域控制器对象  
  
1.  单击**开始**，单击**管理工具**，然后单击**Active Directory 用户和计算机**。  
2.  控制台树中，双击域容器，然后双击**域控制器**部门 (OU)。  
3.  在详细信息窗格中，右键单击你想要删除，然后单击 DC**删除**。 
![删除](media/AD-Forest-Recovery-Cleaning-Metadata/delete1.png) 
4.  单击**是**确认删除。 选择**此域控制器永久离线，并且不会再可以使用 Active Directory 域服务安装向导 (DCPROMO) 将其降级**复选框，然后单击**删除**。  
5.  如果 DC 全球目录服务器，请单击**是**确认删除。  
  
## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
  
