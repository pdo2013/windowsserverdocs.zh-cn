---
title: 必须运行 Windows 虚拟机监控程序
description: 提供说明来解决此最佳实践分析程序规则所报告的问题。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 501a9beb-c464-46c0-88c5-e3e7e3e70101
author: KBDAzure
ms.date: 10/03/2016
ms.openlocfilehash: f34e10918c60fb602c3a88ef3434cda619b11d8d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884138"
---
# <a name="windows-hypervisor-must-be-running"></a>必须运行 Windows 虚拟机监控程序

>适用于：Windows Server 2016
  
|属性|详细信息|  
|-|-|  
|**操作系统**|Windows Server 2016|  
|**产品/功能**|Hyper-V|  
|**Severity**|警告|  
|**类别**|先决条件|  
  
在以下部分中，斜体指示在此问题的最佳做法分析器工具中显示的 UI 文本。  
  
## <a name="issue"></a>问题  
  
*Windows 虚拟机监控程序未运行。*  
  
## <a name="impact"></a>影响  
  
*无法启动虚拟机，直到运行 Windows 虚拟机监控程序。*  
  
## <a name="resolution"></a>分辨率  
  
*检查以查看此服务器就可以运行 HYPER-V 的 Windows Server 目录。接下来，请确保 BIOS 启用硬件辅助虚拟化和硬件强制执行数据执行保护。然后，检查虚拟机管理程序 Hyper V 事件日志。*  
  
若要查看此目录，请参阅[Windows Server 目录](https://go.microsoft.com/fwlink/?LinkId=111228)(https://go.microsoft.com/fwlink/?LinkId=111228)。  
  
> [!CAUTION]  
> 更改计算机的系统 BIOS 中的某些参数可能会导致该计算机以停止加载操作系统，或它可以使硬件设备，如硬盘驱动器，不可用。 始终咨询以确定正确的方式来配置系统 BIOS 的计算机的用户手册。 此外，它始终是你修改和其原始值，以便您可以在以后还原如果需要它们的参数跟踪的一个好办法。 如果更改参数在系统 BIOS 中的后遇到问题，尝试加载默认设置 （一个选项，通常可在 BIOS 配置工具），或与计算机制造商联系以获得帮助。  
  
#### <a name="to-verify-virtualization-support-in-the-bios-or-uefi"></a>若要验证的 BIOS 或 UEFI 中的虚拟化支持  
  
1.  重新启动计算机并通过配置工具访问 BIOS 或 UEFI。 在计算机完成启动过程中出现时，此工具的访问权限通常是可用。 立即关闭大多数计算机上后，几秒钟，其中列出了密钥或密钥以按以打开配置工具的组合会显示一条消息。  
  
2.  查找虚拟化和硬件强制执行数据执行保护 (DEP) 设置并验证它们位于上。 以下是常用的菜单位置中的配置工具和示例的什么它们可能名为的这些设置：  
  
    -   虚拟化支持：  
  
        -   通常可在主处理器或性能的设置使用。 有时，在安全设置下。  
  
        -   查找包含"虚拟化"或"虚拟化技术"的参数名称。  
  
    -   硬件强制执行 DEP:  
  
        -   通常可在安全或内存设置使用。  
  
        -   查找包含"执行"、"执行"或"防护"的参数名称。  
  
3.  如有必要，打开设置配置工具中的说明。 保存所做的更改并退出。  
  
4.  如果进行了任何更改，关闭电源，然后返回到完成。  
  
    > [!IMPORTANT]  
    > 我们建议您关闭电源，然后再打开 （有时称为电源周期），因为所做的更改不会应用某些计算机上，直到发生这种情况。  
  
接下来，检查虚拟机管理程序 Hyper V 事件日志。 如果有问题，您还将检查系统日志。  
  
#### <a name="to-check-the-event-logs"></a>若要检查的事件日志  
  
1.  打开事件查看器。 单击**启动**，单击**管理工具**，然后单击**事件查看器**。  
  
2.  打开 Hyper V 的虚拟机监控程序事件日志。 在导航窗格中，展开**应用程序和服务日志** >> **Microsoft** >> **Windows**  >>  **Hyper-v-V-虚拟机监控程序**，然后单击**Operational**。  
  
3.  如果正在运行 Windows 虚拟机监控程序，则不需要任何进一步的操作。 如果没有运行 Windows 虚拟机监控程序，请执行此操作：  
  
4.  打开系统日志。 (在导航窗格中，展开**Windows 日志**，然后选择**系统**。)  
  
5.  使用筛选器来查找虚拟机管理程序 Hyper V 事件：   
    1. 在中**操作**窗格中，单击**筛选当前日志**。 有关**事件源**，指定"Hyper-v-V-虚拟机监控程序"。   
    2. 查看的报告问题的事件。 例如，事件 ID 41 表明 BIOS 配置出现问题："HYPER-V 启动失败;任一 VMX 不存在或未在 BIOS 中启用了。"  
  
### <a name="see-also"></a>请参阅  
有关在 Windows 10 中，包括如何检查您的计算机可以运行 HYPER-V，使用 HYPER-V 的详细信息请参阅[Windows 10 HYPER-V 系统要求](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_compatibility)。 


