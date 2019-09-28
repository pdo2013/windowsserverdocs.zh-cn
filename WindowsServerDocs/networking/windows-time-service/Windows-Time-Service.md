---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 时间服务技术参考
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 31c7c53a5dd28813076fcaa745093050808b5755
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405229"
---
# <a name="windows-time-service"></a>Windows 时间服务

>适用于：Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows 10 或更高版本


**本指南中**  
  
* 在何处查找 Windows 时间服务配置信息  
* 什么是 Windows 时间服务？  
* 时间协议的重要性  
* Windows 时间服务的工作原理   
* Windows 时间服务工具和设置  
  
> [!NOTE]  
> 在 Windows Server 2003 和 Microsoft Windows 2000 服务器中，目录服务被命名为 Active Directory directory 服务。 在 Windows Server 2008 R2 和 Windows Server 2008 中，目录服务被命名为 Active Directory 域服务（AD DS）。 本主题的其余部分是指 AD DS，但该信息也适用于 Windows Server 2016 中 Active Directory 域服务。  
  
Windows 时间服务（也称为 W32Time）同步在 AD DS 域中运行的所有计算机的日期和时间。 对于许多 Windows 服务和业务线应用程序的正确操作，时间同步至关重要。 Windows 时间服务使用网络时间协议（NTP）来同步网络上的计算机时钟，以便可以将准确的时钟值或时间戳分配给网络验证和资源访问请求。 该服务集成了 NTP 和时间提供程序，使其成为企业管理员的可靠且可伸缩的时间服务。
  
> [!IMPORTANT]  
> 在 Windows Server 2016 之前，W32Time 服务并未设计为满足区分时间的应用程序需要。  但是，Windows Server 2016 的更新现在允许你在域中实施1ms 准确性解决方案。  有关详细信息，请参阅[windows 2016 准确时间](accurate-time.md)和[支持边界，为高准确性环境配置 windows 时间服务](support-boundary.md)。  
  
## <a name="BKMK_Config"></a>在何处查找 Windows 时间服务配置信息  
本指南**不**讨论如何配置 Windows 时间服务。 Microsoft TechNet 和 Microsoft 知识库中提供了几个不同的主题，其中介绍了配置 Windows 时间服务的过程。 如果需要配置信息，请遵循以下主题来帮助你找到相应的信息。  
  
-   若要为林根主域控制器（PDC）仿真器配置 Windows 时间服务，请参阅：  
  
    -   [在林根域中的 PDC 模拟器上配置 Windows 时间服务](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [为林配置时间源](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Microsoft 知识库文章816042，[如何在 Windows server 中配置权威时间服务器](https://go.microsoft.com/fwlink/?LinkID=60402)，描述运行 Windows Server 2008 R2、windows server 2008、windows server 2003 和 windows 的计算机的配置设置Server 2003 R2。  
  
-   若要在任何域成员客户端或服务器上配置 Windows 时间服务，甚至在未配置为林根 PDC 模拟器的域控制器上配置 Windows 时间服务，请参阅为[自动域时间同步配置客户端计算机](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。  
  
    > [!WARNING]  
    > 某些应用程序可能要求其计算机具有高准确性的时间服务。 如果是这种情况，您可以选择配置手动时间源，但需注意的是，Windows 时间服务不是设计为非常准确的时间源。 确保你已了解 Microsoft 知识库文章939322中所述的对高准确性时间环境的支持限制，并[支持边界为高准确性环境配置 Windows 时间服务](support-boundary.md)。  
  
-   若要在配置为工作组成员而不是域成员的任何基于 Windows 的客户端或服务器计算机上配置 Windows 时间服务，请参阅为[所选客户端计算机配置手动时间源](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29)。  
  
-   若要在运行虚拟环境的主计算机上配置 Windows 时间服务，请参阅 Microsoft 知识库文章816042：[如何在 Windows server 中配置权威时间服务器](https://go.microsoft.com/fwlink/?LinkID=60402)。 如果你使用非 Microsoft 虚拟化产品，请务必查阅该产品的供应商文档。  
  
-   若要在虚拟机中运行的域控制器上配置 Windows 时间服务，建议您部分禁用充当域控制器的主机系统和来宾操作系统之间的时间同步。 这会使来宾域控制器为域层次结构同步时间，但如果从已保存状态还原，则保护其不会发生时间偏差。 有关详细信息，请参阅 Microsoft 知识库文章976924：在使用 Hyper-v 和部署的[基于 Windows Server 2008 的主机服务器上运行的虚拟化域控制器上收到 Windows 时间服务事件 id 24、29和 38](https://go.microsoft.com/fwlink/?LinkID=192236) [虚拟化域控制器的注意事项](https://go.microsoft.com/fwlink/?LinkID=192235)。  
  
-   若要在充当同时在虚拟机中运行的林根 PDC 模拟器的域控制器上配置 Windows 时间服务，请按照在[PDC 上配置 Windows 时间服务中所述，为物理计算机执行相同的说明目录林根级域中的模拟器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。  
  
-   若要在作为虚拟计算机运行的成员服务器上配置 Windows 时间服务，请使用中所述的域时间层次结构（为[自动域时间同步配置客户端计算机](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)）。  
  
## <a name="BKMK_WTS"></a>什么是 Windows 时间服务？  
Windows 时间服务（W32Time）为计算机提供网络时钟同步，而无需进行大量配置。  
  
Windows 时间服务对于成功操作 Kerberos 版本5身份验证至关重要，因此用于基于 AD DS 的身份验证。 任何 Kerberos 感知应用程序（包括大多数安全服务）都依赖于加入身份验证请求的计算机之间的时间同步。 AD DS 域控制器还必须具有同步的时钟，以帮助确保准确的数据复制。  
  
Windows 时间服务在名为 W32Time 的动态链接库中实现。 默认情况下，在操作系统设置和安装过程中，在 **%Systemroot%\System32**文件夹中安装 W32Time。  
  
W32Time 最初是为 Windows 2000 服务器开发的，以便支持 Kerberos V5 身份验证协议的规范，该协议要求在网络上进行时钟同步。 从 Windows Server 2003 开始，W32Time 提供对 Windows 2000 服务器操作系统的网络时钟同步提供的更高准确性，此外，还支持通过时间使用各种硬件设备和网络时间协议接口. 尽管最初旨在为 Kerberos 身份验证提供时钟同步，但很多当前应用程序使用时间戳来确保事务一致性，记录重要事件的时间，以及其他业务关键、时间敏感的信息. 这些应用程序从 Windows 时间服务提供的计算机之间的时间同步中获益。  
  
## <a name="BKMK_TimeProtocols"></a>时间协议的重要性  
时间协议在两台计算机之间进行通信以交换时间信息，然后使用该信息同步其时钟。 使用 Windows 时间服务时间协议，客户端从服务器请求时间信息，并根据收到的信息同步其时钟。  
  
Windows 时间服务使用 NTP 来帮助在网络之间同步时间。 NTP 是一种 Internet 时间协议，其中包括同步时钟所需的专业算法。 NTP 是比在某些版本的 Windows 中使用的简单网络时间协议（SNTP）更准确的时间协议;但是，W32Time 继续支持 SNTP，以便能够与运行基于 SNTP 的时间服务（例如 Windows 2000）的计算机向后兼容。  
  
## <a name="see-also"></a>请参阅  
[Windows 时间服务的工作原理](How-the-Windows-Time-Service-Works.md)  
[Windows 时间服务工具和设置](Windows-Time-Service-Tools-and-Settings.md)  
[Microsoft 知识库文章902229](https://go.microsoft.com/fwlink/?LinkId=186066)