---
title: Windows Server 上的 Hyper-v 支持的 Windows 来宾操作系统
description: 列出了在虚拟机中用作来宾的支持的 Windows 操作系统。 还提供指向以前版本的 Hyper-v 的类似文章的链接。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06b35897-2192-48b7-8c2d-125c520b0786
author: lizap
ms.author: elizapo
ms.date: 01/08/2019
ms.openlocfilehash: b24c67de90f8773eec69f10381bd9ce1e121853e
ms.sourcegitcommit: b68ff64ecd87959cd2acde4a47506a01035b542a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/06/2019
ms.locfileid: "68830881"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>Windows Server 上的 Hyper-v 支持的 Windows 来宾操作系统

>适用于：Windows Server 2016、Windows Server 2019

Hyper-v 支持将多个版本的 Windows Server、Windows 和 Linux 分发版作为来宾操作系统在虚拟机中运行。 本文介绍了支持的 Windows Server 和 Windows 来宾操作系统。 对于 Linux 和 FreeBSD 分发版, 请参阅[Windows 上的 Hyper-v 支持的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。  
    
某些操作系统内置了 integration services。 其他一些要求在虚拟机中设置操作系统后, 将 integration services 作为一个单独的步骤安装或升级。 有关详细信息, 请参阅以下部分和[Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services)。  
  
## <a name="supported-windows-server-guest-operating-systems"></a>受支持的 Windows Server 来宾操作系统  

以下是 Windows server 2016 和 Windows Server 2019 中支持作为 Hyper-v 的来宾操作系统的 Windows Server 版本。 
  
|来宾操作系统（服务器）|虚拟处理器的最大数量|集成服务|说明|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows Server 版本 1903 |第2代为 240;<br>第1代64|内置||
|Windows Server 版本 1809 |第2代为 240;<br>第1代64|内置|| 
|Windows Server 2019 |第2代为 240;<br>第1代64|内置||
|Windows Server 版本 1803 |第2代为 240;<br>第1代64|内置|| 
|Windows Server 2016 |第2代为 240;<br>第1代64|内置|| 
|Windows Server 2012 R2 |64|内置||  
|Windows Server 2012 |64|内置||  
|带有 Service Pack 1 (SP 1) 的 Windows Server 2008 R2|64|在设置来宾操作系统后安装所有关键 Windows 更新。|Datacenter、Enterprise、Standard 和 Web 版本。|
|Windows Server 2008 with Service Pack 2 (SP2)|8|在设置来宾操作系统后安装所有关键 Windows 更新。|Datacenter、Enterprise、Standard 和 Web 版本（32 位和 64 位）。|  
  
## <a name="supported-windows-client-guest-operating-systems"></a>支持的 Windows 客户端来宾操作系统  

以下是在 Windows Server 2016 和 Windows Server 2019 中作为 Hyper-v 的来宾操作系统支持的 Windows 客户端版本。
  
|来宾操作系统（客户端）|虚拟处理器的最大数量|集成服务|说明|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows 10|32|内置||  
|Windows 8.1|32|内置||  
|带有 Service Pack 1 (SP 1) 的 Windows 7|4|在设置来宾操作系统后, 升级 integration services。|旗舰版、企业版和专业版版本（32 位和 64 位）。|  
  
## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>其他版本的 Windows 上的来宾操作系统支持  

下表提供了一些链接, 这些链接指向有关 Windows 其他版本上的 Hyper-v 支持的来宾操作系统的信息。  
  
|主机操作系统|主题|  
|-------------------------|---------|  
|Windows 10|[Windows 10 中的客户端 Hyper-v 支持的来宾操作系统](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|  
|Windows Server 2012 R2 和 Windows 8.1|-   [Windows Server 2012 R2 和 Windows 8.1 中的 Hyper-v 支持的 Windows 来宾操作系统](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Hyper-v 上的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|  
|Windows Server 2012 和 Windows 8|[Windows Server 2012 和 Windows 8 中的 Hyper-v 支持的 Windows 来宾操作系统](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|  
|Windows Server 2008 和 Windows Server 2008 R2|[关于虚拟机和来宾操作系统](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|  
  
## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Microsoft 如何为来宾操作系统提供支持  

Microsoft 按如下方式为来宾操作系统提供支持：  
  
-   Microsoft 为在 Microsoft 操作系统和集成服务中找到的问题提供支持。  
  
-   对于经操作系统供应商认证可以在 Hyper-V 上运行的其他操作系统中发现的问题，应由该供应商提供支持。  
  
-   对于在其他操作系统中发现的问题，Microsoft 会将问题提交到多供应商支持社区 [TSANet](https://www.tsanet.org/)。  
  
## <a name="see-also"></a>请参阅  
  
-   [Hyper-v 上的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
  
-   [Windows 10 中的客户端 Hyper-v 支持的来宾操作系统](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)  
  



