---
ms.assetid: c8597cc8-bdcb-4e59-a09e-128ef5ebeaf8
title: 命令行进程审核
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 66ae6992775319cf614b0cb4c21f864150746687
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859548"
---
# <a name="command-line-process-auditing"></a>命令行进程审核

>适用于：Windows Server 2016, Windows Server 2012 R2

**作者**:与 Windows 组的 Justin Turner，高级支持呈报工程师  
  
> [!NOTE]  
> 本内容由 Microsoft 客户支持工程师编写，适用于正在查找比 TechNet 主题通常提供的内容更深入的有关 Windows Server 2012 R2 中的功能和解决方案的技术说明的有经验管理员和系统架构师。 但是，它未经过相同的编辑审批，因此某些语言可能看起来不如通常在 TechNet 上找到的内容那么精练。  
  
## <a name="overview"></a>概述  
  
-   预先存在的进程创建审核事件 ID 4688 现在将包括命令行进程审核的信息。  
  
-   它还将在 Applocker 事件日志中记录的可执行文件的 SHA1/2 哈希  
  
    -   应用程序和服务 Logs\Microsoft\Windows\AppLocker  
  
-   启用通过 GPO，但默认处于禁用状态  
  
    -   "进程创建事件中包括命令行"  
  
![命令行审核](media/Command-line-process-auditing/GTR_ADDS_Event4688.gif)  
  
**图 SEQ Figure \\ \*阿拉伯语 16 事件 4688**  
  
查看已更新的事件 ID 4688 REF _Ref366427278 \h 图 16 中。  在此之前未更新的信息**进程命令行**获取记录。  由于此附加的日志记录我们现在可以看到，不仅 wscript.exe 进程已启动，但这也是用于执行 VB 脚本。  
  
## <a name="configuration"></a>配置  
若要查看此更新的影响，您将需要启用两个策略设置。  
  
### <a name="you-must-have-audit-process-creation-auditing-enabled-to-see-event-id-4688"></a>您必须具有启用若要查看事件 ID 4688 审核进程创建审核。  
若要启用审核进程创建策略，请编辑以下组策略：  
  
**策略位置：** 计算机配置 > 策略 > Windows 设置 > 安全设置 > 高级审核配置 > 详细的跟踪  
  
**策略名称：** 审核进程创建  
  
**支持：** Windows 7 及更高版本  
  
**说明/帮助：**  
  
此安全策略设置确定操作系统是否生成审核事件时将创建一个进程 （开始） 和程序或创建它的用户的名称。  
  
这些审核事件可以帮助您了解计算机的使用情况并跟踪用户活动。  
  
事件量：从低到中等，具体取决于系统使用情况  
  
**默认值：** 未配置  
  
### <a name="in-order-to-see-the-additions-to-event-id-4688-you-must-enable-the-new-policy-setting-include-command-line-in-process-creation-events"></a>若要查看对事件 ID 4688 新增的内容，必须启用新的策略设置：进程创建事件中包括命令行  
**表 SEQ 表\\\*阿拉伯语 19 命令行进程策略设置**  
  
|策略配置|详细信息|  
|------------------------|-----------|  
|**Path**|管理 Templates\System\Audit 进程创建|  
|**设置**|**进程创建事件中包括命令行**|  
|**默认设置**|配置 （未启用）|  
|**支持：**|?|  
|**说明**|此策略设置确定哪些信息记录在安全审核事件时创建一个新进程。<br /><br />此设置仅适用时启用了审核进程创建策略。 如果启用此策略设置的命令行信息的每个进程将以纯文本格式作为审核进程创建事件 4688 的一部分在安全事件日志中记录，"新进程创建后，"上的工作站和服务器上此策略应用设置。<br /><br />如果禁用或未配置此策略设置，将不会在审核进程创建事件中包含进程的命令行信息。<br /><br />Default：未配置<br /><br />注意：启用此策略设置后，具有访问权限的任何用户读取安全事件将能够成功读取的任何命令行参数创建过程。 命令行参数可以包含如密码或用户数据的敏感或私人信息。|  
  
![命令行审核](media/Command-line-process-auditing/GTR_ADDS_IncludeCLISetting.gif)  
  
当使用“高级审核策略配置”设置时，您需要确认这些设置不会被基本审核策略设置覆盖。  当设置被覆盖，将记录事件 4719。  
  
![命令行审核](media/Command-line-process-auditing/GTR_ADDS_Event4719.gif)  
  
以下过程显示了如何通过阻止应用任何基本审核策略设置来避免冲突。  
  
### <a name="to-ensure-that-advanced-audit-policy-configuration-settings-are-not-overwritten"></a>确保“高级审核策略配置”设置不被覆盖  
![命令行审核](media/Command-line-process-auditing/GTR_ADDS_AdvAuditPolicy.gif)  
  
1.  打开组策略管理控制台  
  
2.  右键单击默认域策略，然后单击编辑。  
  
3.  双击计算机配置中，双击策略，然后双击 Windows 设置。  
  
4.  双击安全设置，双击本地策略，然后单击安全选项。  
  
5.  双击审核：强制审核策略子类别设置 (Windows Vista 或更高版本) 以替代审核策略类别设置，然后单击定义此策略设置。  
  
6.  单击启用，并单击确定。  
  
## <a name="additional-resources"></a>其他资源  
[审核进程创建](https://technet.microsoft.com/library/dd941613(v=WS.10).aspx)  
  
[高级安全审核策略循序渐进指南](https://technet.microsoft.com/library/dd408940(v=WS.10).aspx)  
  
[AppLocker:方面的常见问题](https://technet.microsoft.com/library/ee619725(v=ws.10).aspx)  
  
## <a name="try-this-explore-command-line-process-auditing"></a>请尝试此操作：探索命令行过程审核  
  
1.  启用**审核进程创建**事件，并确保不覆盖高级审核策略配置  
  
2.  创建一个脚本，并将生成感兴趣的某些事件，执行该脚本。  观察的事件。  用于在课中生成事件的脚本如下所示：  
  
    ```  
    mkdir c:\systemfiles\temp\commandandcontrol\zone\fifthward  
    copy \\192.168.1.254\c$\hidden c:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward  
    start C:\systemfiles\temp\hidden\commandandcontrol\zone\fifthward\ntuserrights.vbs  
    del c:\systemfiles\temp\*.* /Q  
    ```  
  
3.  启用命令行进程审核  
  
4.  执行之前的同一个脚本，并观察的事件  
  


