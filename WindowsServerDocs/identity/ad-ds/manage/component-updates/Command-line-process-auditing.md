---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: "命令行进程审核"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 61e7a5ca2b9c00c9976e6032bb10adb1974020b0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="command-line-process-auditing"></a>命令行进程审核

>适用于：Windows Server 2016，Windows Server 2012 R2

**作者**：与 Windows 组索旋转器高级支持升级工程师  
  
> [!NOTE]  
> 内容由 Microsoft 客户支持工程师和适用于经验丰富的管理员系统师寻找更深入 technical 说明功能和 Windows Server 2012 R2 的解决方案不是通常提供 TechNet 上的主题。 但是，它不经历了同一编辑通行证，因此一些语言可能看起来比通常 TechNet 上找到内容小于优化。  
  
## <a name="overview"></a>概述  
  
-   预现有过程创建审核事件 ID 4688 现在会将包括审核命令行进程信息。  
  
-   它还将 Applocker 事件日志中记录 SHA1 2 月哈希可执行文件  
  
    -   应用和服务 Logs\ Microsoft \Windows\AppLocker  
  
-   您启用了通过 GPO，但默认处于禁用状态  
  
    -   "创建事件的过程中包括命令行"  
  
![命令行审核](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**确定 SEQ 图 \\\ * 阿拉伯语 16 事件 4688**  
  
查看更新的事件 ID 4688 引用 _Ref366427278 \h 图 16 中。  在这之前更新的信息的任何**过程命令行**获取登录。  由于此更多日志记录我们现在可以看到不仅 wscript.exe 过程开始使用，但它也是用于执行 VB 脚本。  
  
## <a name="configuration"></a>配置  
若要查看此次更新的效果，你将需要启用两种策略设置。  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>你必须启用以查看事件 ID 4688 的审核创建的进程审核。  
若要启用创建审核进程策略，编辑以下组策略：  
  
**策略位置：**计算机配置 > 策略 > Windows 设置 > 安全设置 > 高级审核配置 > 详细跟踪  
  
**策略名称：**审核创建进程  
  
**在受支持：** Windows 7 及更高  
  
**描述/帮助：**  
  
此安全策略设置用于确定是否操作系统生成进程创建（启动）时的审核事件和程序或创建它的用户的名称。  
  
这些审核事件可帮助您了解如何使用计算机并跟踪用户活动。  
  
事件音量：介于低等和中等，具体取决于系统使用情况  
  
**默认设置：**不配置  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>以查看事件 ID 4688 的附件，你必须启用新的策略设置：在创建事件的过程中包含命令行  
**表用 \\\ * 阿拉伯语 19 命令行过程策略设置**  
  
|策略配置|详细信息|  
|------------------------|-----------|  
|**路径**|管理 Templates\System\Audit 创建的进程|  
|**设置**|**在创建事件的过程中包含命令行**|  
|**默认设置**|不已配置（未启用）|  
|**在受支持：**|?|  
|**描述**|此策略设置用于确定在安全审核事件时创建了一个新的过程中记录的哪些信息。<br /><br />此设置仅适用于创建审核进程启用了。 如果你启用此设置，每个进程的命令行信息将登录作为创建审核进程事件 4688 的一部分安全事件日志中纯文本，"新过程已创建，"工作站和该策略设置应用上的服务器上。<br /><br />如果你禁用或该设置不配置，则进程的命令行信息将不包括在审核创建的进程事件中。<br /><br />无法配置默认设置：<br /><br />注意：任何拥有访问权限的用户，若要阅读安全事件将可以成功阅读的任何命令行参数启用此策略设置时创建过程。 命令行参数可能包含敏感或个人信息，例如密码或用户数据。|  
  
![命令行审核](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
当你使用的高级审核策略配置设置时，你需要以确认这些设置不会覆盖基本审核策略设置。  覆盖设置后，将记录事件 4719。  
  
![命令行审核](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
下面的过程中显示如何防止冲突通过阻止任何基本审核策略设置的应用。  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>若要确保不会覆盖高级审查策略配置设置  
![命令行审核](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  打开组策略管理控制台  
  
2.  默认域策略右键单击，然后单击编辑。  
  
3.  双击计算机配置、双击策略，，然后双击 Windows 设置。  
  
4.  双击安全设置，双击本地策略，然后单击安全选项。  
  
5.  双击审核：强制审核策略子设置 (Windows Vista 或更高版本) 来替代审核策略类别设置，然后单击定义此策略设置。  
  
6.  单击启用，然后单击确定。  
  
## <a name="additional-resources"></a>更多资源  
[创建审核进程](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[高级安全审核策略 Step-by-Step 指南](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker：常见问题](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>请尝试：探索命令行进程审核  
  
1.  启用**创建审核进程**事件，并确保不会覆盖提前审核策略配置  
  
2.  创建的脚本，将生成感兴趣的某些事件，并执行脚本。  观察发生的事件。  用于在课中生成该事件的脚本如下所示：  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  启用命令行进程审核  
  
4.  执行之前的同一个脚本和观察发生的事件  
  


