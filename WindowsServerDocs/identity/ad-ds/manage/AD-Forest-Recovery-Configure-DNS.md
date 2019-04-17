---
title: "广告森林恢复-配置 DNS 服务器服务"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f24570965fd8b3f3e050779c42758865cbee2728
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>广告森林恢复-配置 DNS 服务器服务 

>适用于：Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2
 
如果 DNS 服务器角色从备份中还原直流上未安装，你必须安装并配置 DNS 服务器。  
  

## <a name="install-and-configure-the-dns-server-service"></a>安装和配置 DNS 服务器服务  
对于每个还原 DC 还原完成后不运行作为 DNS 服务器完成此步骤。  
  
> [!NOTE]
>  如果你从备份中还原直流运行的 Windows Server 2008 R2，必须才能安装 DNS 服务器 DC 连接到隔离的网络。 然后连接还原 DNS 服务器的每个独立的相互共享网络。 运行 repadmin /replsum 验证该复制之间还原 DNS 服务器时是否可正常运行。 验证复制后，你可以连接到生产网络还原的 Dc，如果已安装了 DNS 服务器角色，可以将应用，它使 DNS 服务器以启动该服务器未连接到任何网络时修补程序。 应你的自动的版本过程将到操作系统安装图像的修补程序。 有关修补程序的详细信息，请参阅[文章 975654](https://go.microsoft.com/fwlink/?LinkId=184691) Microsoft 知识库 (https://go.microsoft.com/fwlink/?LinkId=184691) 中。 

完成安装和配置执行以下步骤。
  
### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>安装和使用服务器管理器 DNS 服务器服务  
  
1.  打开服务器管理器，然后单击**添加角色和功能**。  
2.  在添加角色向导中，如果**开始之前**页面显示时，单击**下一步**。  
3.  上**安装类型**屏幕选择**角色基于或者基于安装功能**单击**下一步**。
4.  在**选择服务器**选择服务器屏幕，然后单击**下一步**。
5.  在**服务器角色**屏幕选择**DNS 服务器**，如果系统提示您单击**添加功能**单击**下一步**。
6.  在**功能**屏幕单击**下一步**。
7.  阅读上的信息**DNS 服务器**页，，然后单击**下一步**。
![DNS 服务器](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8.  在**确认**页上，检查 DNS 服务器角色将安装，然后单击**安装**。  
  
     
### <a name="to-configure-the-dns-server-service"></a>若要配置 DNS 服务器服务 
1.  打开服务器管理器中，单击**工具**单击**DNS**。
![DNS 服务器](media/AD-Forest-Recovery-Configure-DNS/dns2.png)    
2.  关键故障之前 DNS 服务器上创建了托管相同 DNS 域名称 DNS 区域。 有关详细信息，请参阅添加反向查找区域 ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574))。  
3.  配置它前关键故障不存在 DNS 数据。 例如：  
  
    -   配置 DNS 存储在广告 DS 的区域。 有关详细信息，请参阅更改区域类型 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579))。  
  
    -   配置 DNS 域控制器（DC 定位器）定位器资源记录，以允许安全的动态更新的授权的区域。 有关详细信息，请参阅允许仅安全的动态更新 ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580))。  
  
4. 确保家长 DNS 区域包含委派资源记录（恕服务器 (NS)，并将结合主机 () 资源记录）此 DNS 服务器上托管孩子区域。 有关详细信息，请参阅创建区域委派 ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562))。  
5. 配置 DNS 后，你可以加快 NETLOGON 记录的注册。  
  
    > [!NOTE]
    >  安全的动态更新仅适用时全球目录服务器可用。  
  
     在命令提示符下，键入以下命令，，然后按 ENTER:  
  
     **网络停止 netlogon**  
  
6. 键入以下命令，并按 enter 键：  
  
     **网络开始 netlogon**  

![DNS 服务器](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>后续步骤

- [广告林恢复指南](AD-Forest-Recovery-Guide.md)
- [广告森林恢复-过程](AD-Forest-Recovery-Procedures.md)
