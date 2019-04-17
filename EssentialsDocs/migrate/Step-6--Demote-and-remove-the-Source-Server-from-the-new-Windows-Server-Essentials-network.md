---
title: "第 6 步：将降级，从新的 Windows Server Essentials 网络删除源服务器"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 86244c66-2c5e-488d-adb8-112e1ca3e2e1
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 24a1f2da2333c7e6854e9efd9d996391d0fcb3b9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="step-6-demote-and-remove-the-source-server-from-the-new-windows-server-essentials-network"></a>第 6 步：将降级，从新的 Windows Server Essentials 网络删除源服务器

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

安装 Windows Server Essentials 完并完成迁移后，你必须执行以下任务：  
  
1.  [删除 Active Directory 证书服务](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_ADCS)  
  
2.  [断开连接直接连接到源代码服务器的打印机](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_PhysicallyDisconnect)  
  
3.  [降级源服务器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_DemoteTheSourceServer)  
  
4.  [删除和重新源服务器](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md#BKMK_RemoveTheSourceServer)  
  
##  <a name="BKMK_ADCS"></a>删除 Active Directory 证书服务  
 此过程已稍有不同，如果你有安装的单个服务器上的多个 Active Directory 证书服务（广告客户服务）角色服务。 若要卸载的广告的客户服务角色服务，并保留其他广告客户服务角色服务，可以使用下面的过程。  
  
 若要完成此过程，必须使用相同的权限安装证书颁发机构）的用户身份登录。 如果进行卸载企业 CA，在企业管理员或等值金额会员是的最低要求完成此过程。  
  
#### <a name="to-remove-ad-cs"></a>若要删除广告客户服务  
  
1.  域管理员身份登录到源代码服务器上。  
  
2.  单击**开始**，单击**管理工具**，然后单击**服务器管理器**。  
  
3.  单击**继续**中**用户帐户控制**对话框。  
  
4.  在**角色摘要**部分中，单击**删除角色**。  
  
5.  在删除角色向导中，单击**下一步**。  
  
6.  清除**Active Directory 证书服务**复选框，然后依次单击**下一步**。  
  
7.  在**确认删除选项**页上，检查信息，然后依次单击**删除**。  
  
    > [!NOTE]
    >  如果运行的 Internet 信息服务 (IIS)，提示你之前停止服务。 单击**确定**。  
  
    > [!NOTE]
    >  首先，你可能需要删除**证书颁发机构 Web 注册**，如果它已安装。  
  
8.  在删除角色向导完成之后，重新启动服务器卸载过程的完成。  
  
    > [!IMPORTANT]
    >  即使未提示你执行此操作，重新启动服务器。  
  
##  <a name="BKMK_PhysicallyDisconnect"></a>断开连接直接连接到源代码服务器的打印机  
 降级源服务器之前，断开物理连接任何直接连接到源代码服务器和共享通过源服务器的打印机。 请确保没有 Active Directory 对象保持直接连接到源代码服务器的打印机。 打印机可以然后直接连接到目标 Server 和 Windows Server Essentials 从共享。  
  
##  <a name="BKMK_DemoteTheSourceServer"></a>降级源服务器  
 降级源服务器的广告 DS 域控制器角色域成员服务器的作用之前，请确保组策略设置将应用于所有客户端计算机，如下面的过程中所述。  
  
> [!IMPORTANT]
>  组策略更改的更新的客户端计算机上时，源服务器和目的地服务器必须连接到该网络。  
  
#### <a name="to-force-a-group-policy-update-on-a-client-computer"></a>若要在客户端计算机上强制更新组策略  
  
1.  以管理员身份登录到 client 计算机。  
  
2.  以 administrator 身份打开一个 Command Prompt 窗口。  
  
3.  在命令提示符下，键入**gpupdate /force**，然后按 ENTER。  
  
4.  该过程可能需要注销并重新登录才能完成。 单击**是**以确认。  
  
 如果你从 Windows Server Essentials 或其以前的版本进行迁移，若要降级服务器，请参阅[删除 Active Directory 域服务](https://technet.microsoft.com/library/hh472163.aspx)。 添加源代码服务器以在工作组中的成员，并断开网络后，必须从广告 DS 目标服务器上将其删除。  
  
 如果你要从 Windows Server Essentials 迁移，使用服务器管理器中删除 Active Directory 域服务角色，因此降级使用下面的过程的源服务器上的域控制器：  
  
#### <a name="to-remove-the-source-server-from-active-directory"></a>若要删除 Active Directory 源服务器  
  
1.  在目标服务器上，打开**Active Directory 用户和计算机**。  
  
2.  在**Active Directory 用户和计算机**导航窗格中，展开域名，，然后展开**计算机**。  
  
3.  如果在列表中服务器仍然存在源服务器，右键单击源服务器名称、单击**删除**，然后单击**是**。  
  
4.  验证源服务器是否未列出，然后再关闭**Active Directory 用户和计算机**。  
  
##  <a name="BKMK_RemoveTheSourceServer"></a>删除和重新源服务器  
 关闭源服务器，并断开网络。 我们建议在未重新格式化至少一周源服务器，以确保所有必需的数据迁移到目的地的服务器。 验证具有迁移的所有数据后，你可以重装此服务器网络上为辅助服务器执行其他任务，如果需要。  
  
> [!NOTE]
>  你将降级并删除源服务器后，重新启动目标服务器。  
  
 降级源服务器后，它不在正常运行的状态。 如果你想要调整用途源服务器，最简单方法是将其重新格式化安装在服务器操作系统，然后将其设置为其他服务器的使用。  
  
## <a name="next-steps"></a>后续步骤  
 您可以降级并从新的 Windows Server Essentials 网络删除源服务器。 现在转到[第 7 步： 为 Windows Server Essentials 迁移执行后的迁移任务](Step-7--Perform-post-migration-tasks-for-the-Windows-Server-Essentials-migration.md)。  
  

若要查看所有步骤，请参阅[迁移到 Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md)。

