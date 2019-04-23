---
title: AD 林恢复-配置 DNS 服务器服务
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b2c37428a0fb685e6a7fa4875366f3cd13401bd9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842958"
---
# <a name="ad-forest-recovery---configuring-the-dns-server-service"></a>AD 林恢复-配置 DNS 服务器服务

>适用于：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

如果在从备份还原的 DC 上未安装 DNS 服务器角色，必须安装并配置 DNS 服务器。 

## <a name="install-and-configure-the-dns-server-service"></a>安装和配置 DNS 服务器服务

还原完成后未运行的 DNS 服务器为每个已还原 dc 中完成此步骤。 

> [!NOTE]
> 如果您从备份中还原 DC 运行 Windows Server 2008 R2，必须将 DC 到隔离的网络连接才能安装 DNS 服务器。 然后对相互共享、 独立的网络连接的每个已还原的 DNS 服务器。 运行 repadmin /replsum 以验证复制之间的已还原的 DNS 服务器正常运行。 验证复制后，你可以连接到生产网络已还原域控制器，如果已安装 DNS 服务器角色，可以应用修补程序，从而能够启动，同时在服务器未连接到任何网络的 DNS 服务器。 您应在自动的生成过程集成到操作系统安装映像，修补程序。 有关修补程序的详细信息，请参阅[一文 975654](https://go.microsoft.com/fwlink/?LinkId=184691)在 Microsoft 知识库文章 (https://go.microsoft.com/fwlink/?LinkId=184691)。 

完成以下安装和配置步骤。

### <a name="to-install-and-the-dns-server-service-using-server-manager"></a>安装和使用服务器管理器的 DNS 服务器服务  

1. 打开服务器管理器，然后单击**添加角色和功能**。 
2. 在“添加角色向导”中，如果出现“开始之前”页，请单击“下一步”。 
3. 上**安装类型**屏幕上，选择**基于角色或基于功能的安装**然后单击**下一步**。
4. 上**服务器选择**屏幕选择服务器，然后单击**下一步**。
5. 上**服务器角色**屏幕上，选择**DNS 服务器**，如果系统提示您单击**添加功能**然后单击**下一步**。
6. 上**功能**屏幕单击**下一步**。
7. 在读取信息**DNS 服务器**页上，然后依次**下一步**。
   ![DNS 服务器](media/AD-Forest-Recovery-Configure-DNS/dns1.png)  
8. 上**确认**页上，验证 DNS 服务器角色将安装，然后单击**安装**。 

### <a name="to-configure-the-dns-server-service"></a>若要配置 DNS 服务器服务

1. 打开服务器管理器中，单击**工具**然后单击**DNS**。
   ![DNS 服务器](media/AD-Forest-Recovery-Configure-DNS/dns2.png)
2. 严重故障之前的 DNS 服务器上创建了托管的相同 DNS 域名的 DNS 区域。 有关详细信息，请参阅添加正向查找区域 ([https://go.microsoft.com/fwlink/?LinkId=74574](https://go.microsoft.com/fwlink/?LinkId=74574))。
3. 配置 DNS 数据，如其关键工作不正常之前已存在。 例如：  

   - 配置存储在 AD DS 的 DNS 区域。 有关详细信息，请参阅更改区域类型 ([https://go.microsoft.com/fwlink/?LinkId=74579](https://go.microsoft.com/fwlink/?LinkId=74579))。
   - 配置域控制器定位程序 （DC 定位器） 资源记录，以允许安全动态更新具有权威的 DNS 区域。 有关详细信息，请参阅允许仅安全动态更新 ([https://go.microsoft.com/fwlink/?LinkId=74580](https://go.microsoft.com/fwlink/?LinkId=74580))。

4. 请确保父 DNS 区域包含委派的资源记录 （名称服务器 (NS) 和粘附主机 (A) 资源记录） 此 DNS 服务器托管子区域。 有关详细信息，请参阅创建区域委派 ([https://go.microsoft.com/fwlink/?LinkId=74562](https://go.microsoft.com/fwlink/?LinkId=74562))。
5. 配置 DNS 后，可以加快 NETLOGON 记录的注册。

   > [!NOTE]
   > 安全动态更新仅适用的全局编录服务器不可用。 

   在命令提示符下，键入以下命令，然后按 Enter：  

   **net stop netlogon**  

6. 键入以下命令，然后按 Enter：  

   **net start netlogon**  

   ![DNS 服务器](media/AD-Forest-Recovery-Configure-DNS/dns3.png)  

## <a name="next-steps"></a>后续步骤

- [AD 林恢复指南](AD-Forest-Recovery-Guide.md)
- [AD 林恢复的过程](AD-Forest-Recovery-Procedures.md)
