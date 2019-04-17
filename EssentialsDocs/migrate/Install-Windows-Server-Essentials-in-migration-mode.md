---
title: "安装 Windows Server Essentials 中迁移模式 1"
description: "介绍了如何使用 Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fd7196ac-cfa6-46a5-ba77-6962b47a825e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 808a4b1e120fa559d603b34ad006b18de6b94378
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="install-windows-server-essentials-in-migration-mode1"></a>安装 Windows Server Essentials 中迁移模式 1

>适用于：Windows Server 2016 Essentials，Windows Server 2012 R2 Essentials 中，Windows Server 2012 Essentials

对您的网络中运行的 Windows Server Essentials 只有一个服务器，并且该服务器必须是网络域控制器。  
  
 迁移模式中一起安装时，Windows Server Essentials 安装向导将执行以下任务：  
  
1.  安装并配置 Windows Server Essentials 服务器软件目标服务器上。  
  
2.  更新到最新版本的域的方案。  
  
3.  向现有域加入目标服务器。 源服务器和目的地服务器可以都为相同的域的成员之前完成迁移的过程。 迁移完毕后，你必须 21 天内从网络删除源服务器。  
  
    > [!WARNING]
    >  一条错误消息被添加事件日志每一天期间 21 天的宽限期直到源服务器删除你的网络。 文本消息显示为，"FSMO 角色检查检测到条件您不符合许可策略的环境中。 按住管理服务器必须主域控制器和域名主机 Active Directory 角色。 请现在向管理服务器移动 Active Directory 角色。 此服务器将自动关闭如果 21 天从首先检测到这种情况的时间内不更正该问题。" 21 日宽限期到期后，源服务器将关闭。  
  
4.  将操作 （也称为灵活单个主操作或 FSMO） 母版角色从源服务器传输到目的地的服务器。 操作主角色都是标准数据传输和更新方法不足时将使用专门的域控制器任务。 域控制器目标服务器时，它必须存储主机角色操作。  
  
5.  为全球的目录服务器配置目标服务器。 全球目录服务器是管理分布式的数据存储库域控制器。 它包含可搜索、 部分表示的每个每林中域的 Active Directory 对象。  
  
6.  与该站点许可服务器配置目标服务器。  
  
##  <a name="BKMK_Install"></a>在目标服务器上安装 Windows Server Essentials  
 若要安装和配置 Windows Server Essentials 迁移型目标服务器上，执行以下步骤。  
  
#### <a name="to-install-windows-server-essentials-on-the-destination-server"></a>若要安装目标服务器上的 Windows Server Essentials  
  
1.  打开目的地 Server 和 Windows Server Essentials DVD1 插入 DVD 驱动器。 如果你看到一条消息，将询问你是否想要从 CD 或 DVD 启动，请按任意键，若要执行此操作。  
  
    > [!NOTE]
    >  如果目标服务器支持从 U 盘启动，你可以使用**Windows 7 USB/DVD 下载工具**从 Windows Server Essentials ISO 文件创建可启动 u 盘。 使用 U 盘可以明显加快在安装过程因为 flash 驱动器读取数据不是 DVD-ROM 驱动器一样更快。 创建可启动 U 盘后，你可以将 answer 文件添加到盘。 你可以[下载 Windows 7 USB/DVD 下载工具](https://go.microsoft.com/fwlink/p/?LinkId=248282)免费在 Microsoft 官方商城的网站。  
  
    > [!NOTE]
    >  如果目标服务器无法从 DVD 启动，重启计算机并检查以确保在 BIOS 设置**DVD-ROM**首次启动序列中列出。 有关如何更改 BIOS 设置启动序列的详细信息，请参阅硬件制造商提供的文档。  
  
2.  单击**全新安装**。  
  
3.  如果你有未显示在列表中内部硬盘驱动器上，单击**加载驱动程序**并在继续之前安装必需的驱动程序。  
  
4.  选中复选框，用于验证的所有文件和文件夹主硬盘将被删除，然后单击**安装**。  
  
5.  在**选择服务器安装模式**页上，单击**Server 迁移**，然后提供所需的迁移信息。  
  
6.  当**服务器成功迁移**消息出现时，单击**关闭**。  
  
 安装完成后，你将自动登录使用管理员用户帐户和迁移 answer 文件中提供，你的密码。  
  
> [!NOTE]
>  若要安装 Windows Server Essentials 时，请解锁桌面，使用内置的管理员帐户并将密码的空白。  
  
##  <a name="BKMK_VerifyTheHealthOfDC"></a>验证域控制器的运行状况  
 迁移前，你应确保的域控制器和 Windows Server Essentials 网络正常运行。  
  
 下表列出了这些工具，可用于在诊断问题对目的地服务器和网络，并在域中：  
  
|工具|描述|  
|----------|-----------------|  
|Netdiag|帮助隔离网络和连接性问题。 有关详细信息并下载，请参阅[Netdiag](https://go.microsoft.com/fwlink/?LinkId=217388)。|  
|Dcdiag.exe|分析域控制器林或企业版中的状态，并报告问题，以帮助你诊断问题。 有关详细信息并下载，请参阅[Dcdiag](https://go.microsoft.com/fwlink/?LinkId=217389)。|  
|Repadmin.exe|帮助你诊断复制域控制器之间的问题。 此工具需要参数命令行运行。 有关详细信息并下载，请参阅[Repadmin](https://go.microsoft.com/fwlink/?LinkId=217387)。|  
  
 你应该更正所有这些工具报告才能继续迁移的问题。  
  
> [!NOTE]
>  如果你计划迁移至另一台本地 Exchange 服务器的电子邮件，请参阅[与 Windows Server Essentials 集成 On 本地 Exchange Server](../manage/Integrate-an-On-Premises-Exchange-Server-with-Windows-Server-Essentials.md)有关如何设置你的本地 Exchange 服务器的信息。
