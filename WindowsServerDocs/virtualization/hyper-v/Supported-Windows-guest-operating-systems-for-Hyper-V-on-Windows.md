---
title: 支持的 Windows 来宾操作系统为 Windows Server 上的 HYPER-V
description: 列出了支持作为来宾虚拟机中使用的 Windows 操作系统。 此外为以前版本的 HYPER-V 提供的相似的文章的链接。
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
ms.openlocfilehash: 5f0e91f3202f09d340154b49408c56752a9de577
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874208"
---
# <a name="supported-windows-guest-operating-systems-for-hyper-v-on-windows-server"></a>支持的 Windows 来宾操作系统为 Windows Server 上的 HYPER-V

>适用于：Windows Server 2016 中，Windows Server 2019

HYPER-V 支持几个版本的 Windows Server、 Windows、 和 Linux 分发版，若要在虚拟机，为来宾操作系统中运行。 这篇文章介绍支持的 Windows Server 和 Windows 来宾操作系统。 有关 Linux 和 FreeBSD 分发版，请参阅[Windows 上的 HYPER-V 的支持的 Linux 和 FreeBSD 虚拟机](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)。  
    
某些操作系统具有内置的集成服务。 其他需要你安装或升级集成服务作为单独的步骤，在设置虚拟机中的操作系统。 有关详细信息，请参阅以下各节和[Integration Services](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/integration-services)。  
  
## <a name="supported-windows-server-guest-operating-systems"></a>支持的 Windows Server 来宾操作系统  

以下是为 Windows Server 2016 和 Windows Server 2019 中的 HYPER-V 支持作为来宾操作系统版本的 Windows Server。 
  
|来宾操作系统（服务器）|虚拟处理器的最大数量|集成服务|说明|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows Server 2019 |240，第 2 代;<br>对于第 1 代为 64|内置|| 
|Windows Server 2016 |240，第 2 代;<br>对于第 1 代为 64|内置|| 
|Windows Server 2012 R2 |64|内置||  
|Windows Server 2012 |64|内置||  
|带有 Service Pack 1 (SP 1) 的 Windows Server 2008 R2|64|设置来宾操作系统后，请安装所有关键的 Windows 更新。|Datacenter、Enterprise、Standard 和 Web 版本。|
|Windows Server 2008 with Service Pack 2 (SP2)|8|设置来宾操作系统后，请安装所有关键的 Windows 更新。|Datacenter、Enterprise、Standard 和 Web 版本（32 位和 64 位）。|  
  
## <a name="supported-windows-client-guest-operating-systems"></a>支持的 Windows 客户端来宾操作系统  

以下是为 Windows Server 2016 和 Windows Server 2019 中的 HYPER-V 支持作为来宾操作系统版本的 Windows 客户端。
  
|来宾操作系统（客户端）|虚拟处理器的最大数量|集成服务|说明|  
|-------------------------------------|----------------------------------------|------------------------|---------|  
|Windows 10|32|内置||  
|Windows 8.1|32|内置||  
|带有 Service Pack 1 (SP 1) 的 Windows 7|4|设置来宾操作系统后，请升级集成服务。|旗舰版、企业版和专业版版本（32 位和 64 位）。|  
  
## <a name="guest-operating-system-support-on-other-versions-of-windows"></a>其他版本的 Windows 上的来宾操作系统支持  

下表提供有关其他版本的 Windows 上的 HYPER-V 支持的来宾操作系统的信息链接。  
  
|主机操作系统|主题|  
|-------------------------|---------|  
|Windows 10|[客户端 HYPER-V 的 Windows 10 中受支持的来宾操作系统](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)|  
|Windows Server 2012 R2 和 Windows 8.1|-   [支持的 Windows 来宾操作系统为 Windows Server 2012 R2 和 Windows 8.1 中的 Hyper V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792027(v=ws.11))<br />-   [Linux 和 FreeBSD 虚拟机上的 HYPER-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)|  
|Windows Server 2012 和 Windows 8|[支持的 Windows 来宾操作系统为 Windows Server 2012 和 Windows 8 中的 Hyper V](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn792028(v=ws.11))|  
|Windows Server 2008 和 Windows Server 2008 R2|[关于虚拟机和来宾操作系统](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc794868(v=ws.10))|  
  
## <a name="how-microsoft-provides-support-for-guest-operating-systems"></a>Microsoft 如何提供支持的来宾操作系统  

Microsoft 按如下方式为来宾操作系统提供支持：  
  
-   Microsoft 为在 Microsoft 操作系统和集成服务中找到的问题提供支持。  
  
-   对于经操作系统供应商认证可以在 Hyper-V 上运行的其他操作系统中发现的问题，应由该供应商提供支持。  
  
-   对于在其他操作系统中发现的问题，Microsoft 会将问题提交到多供应商支持社区 [TSANet](https://www.tsanet.org/)。  
  
## <a name="see-also"></a>请参阅  
  
-   [Linux 和 FreeBSD 虚拟机上的 HYPER-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
  
-   [客户端 HYPER-V 的 Windows 10 中受支持的来宾操作系统](https://docs.microsoft.com/virtualization/hyper-v-on-windows/about/supported-guest-os)  
  



