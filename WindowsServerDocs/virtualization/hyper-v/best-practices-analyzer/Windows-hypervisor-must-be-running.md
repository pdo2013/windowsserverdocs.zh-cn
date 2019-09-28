---
title: Windows 虚拟机监控程序必须运行
description: 提供有关如何解决此最佳做法分析器规则报告的问题的说明。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: 51f863425bd1107894fb5e4d44ed7c742a806394
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393043"
---
# <a name="windows-hypervisor-must-be-running"></a>Windows 虚拟机监控程序必须运行

>适用于：Windows Server 2016
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|先决条件|  
  
在以下部分中，"斜体" 指示在此问题的最佳做法分析器工具中出现的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*Windows 虚拟机监控程序未运行。*  
  
## <a name="impact"></a>影响  
  
*在运行 Windows 虚拟机监控程序之前，无法启动虚拟机。*  
  
## <a name="resolution"></a>分辨率  
  
@no__t 0Check Windows Server 目录，以查看此服务器是否有资格运行 Hyper-v。接下来，请确保已为硬件辅助虚拟化和硬件强制的数据执行保护启用了 BIOS。然后，检查 Hyper-v 虚拟机监控程序事件日志。 *  
  
若要检查目录，请参阅[Windows Server 目录](https://go.microsoft.com/fwlink/?LinkId=111228)（ https://go.microsoft.com/fwlink/?LinkId=111228) 。  
  
> [!CAUTION]  
> 更改计算机的系统 BIOS 中的某些参数可能会导致计算机停止加载操作系统，或使硬件设备（如硬盘驱动器）不可用。 请始终查阅计算机的用户手册，以确定配置系统 BIOS 的正确方法。 此外，跟踪你修改的参数及其原始值始终是一个好主意，以便以后可以根据需要还原它们。 如果在系统 BIOS 中更改参数后遇到问题，请尝试加载默认设置（通常在 BIOS 配置实用工具中提供选项），或与计算机制造商联系以获得帮助。  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>验证 BIOS 或 UEFI 中的虚拟化支持  
  
1.  重新启动计算机，并通过配置工具访问 BIOS 或 UEFI。 当计算机执行引导过程时，通常可以使用此工具的访问权限。 打开大多数计算机后，会立即出现一条消息，其中列出了用于打开配置工具的键或键组合。  
  
2.  找到虚拟化和硬件强制执行的数据执行保护（DEP）的设置，并验证它们是否已打开。 下面是这些设置在配置工具中的常见菜单位置，并举例说明了它们的命名：  
  
    -   虚拟化支持：  
  
        -   通常适用于主要处理器或性能的设置。 有时它位于安全设置下。  
  
        -   查找包含 "虚拟化" 或 "虚拟化技术" 的参数名称。  
  
    -   硬件强制 DEP：  
  
        -   通常在 "安全" 或 "内存" 设置下提供。  
  
        -   查找包含 "执行"、"执行" 或 "预防" 的参数名称。  
  
3.  如有必要，请按照配置工具中的说明启用设置。 保存更改并退出。  
  
4.  如果进行了任何更改，请关闭电源，然后再重新打开。  
  
    > [!IMPORTANT]  
    > 我们建议你关闭电源，然后再重新打开（有时称为电源周期），因为在这种情况下，这些更改不会应用到某些计算机上。  
  
接下来，请检查 Hyper-v 虚拟机监控程序事件日志。 如果出现问题，还需要检查系统日志。  
  
#### <a name="to-check-the-event-logs"></a>检查事件日志  
  
1.  打开事件查看器。 依次单击 "**开始**"、"**管理工具**"，然后单击 "**事件查看器**"。  
  
2.  打开 Hyper-v 虚拟机监控程序事件日志。 在导航窗格中，展开 "**应用程序和服务日志**"  >> **Microsoft** >> **Windows** >> **hyper-v 虚拟机监控程序**，然后单击 "**操作**"。  
  
3.  如果 Windows 虚拟机监控程序正在运行，则无需执行其他操作。 如果 Windows 虚拟机监控程序未运行，请执行以下操作：  
  
4.  打开系统日志。 （在导航窗格中，展开 " **Windows 日志**"，然后选择 "**系统**"。）  
  
5.  使用筛选器查找 Hyper-v 虚拟机监控程序事件：   
    1. 在 "**操作**" 窗格中，单击 "**筛选当前日志**"。 对于**事件源**，请指定 "Hyper-v-虚拟机监控程序"。   
    2. 查找报告问题的事件。 例如，事件 ID 41 表明 BIOS 配置存在问题："Hyper-v 启动失败;在 BIOS 中，.VMX 不存在或未启用。 "  
  
### <a name="see-also"></a>请参阅  
有关使用 Windows 10 上的 Hyper-v 的详细信息，包括如何检查您的计算机是否可以运行 Hyper-v 的详细信息，请参阅[windows 10 Hyper-v 系统要求](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility)。 


