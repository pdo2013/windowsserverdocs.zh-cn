---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Windows 时间服务技术参考
description: W32Time 服务提供网络而无需大量的配置的计算机的时钟同步。 W32Time 服务来说是必需的 Kerberos V5 身份验证成功的操作，则因此，对 AD DS 基于身份验证。
author: shortpatti
ms.author: dacuo
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 904a53797d22d2e06e0cd2f7572b99fc386dae16
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845118"
---
# <a name="windows-time-service-technical-reference"></a>Windows 时间服务技术参考
>适用于：Windows Server 2016 中，Windows Server 2012 R2，Windows Server 2012 中，Windows 10 或更高版本

W32Time 服务提供网络而无需大量的配置的计算机的时钟同步。 W32Time 服务来说是必需的 Kerberos V5 身份验证成功的操作，则因此，对 AD DS 基于身份验证。 任何 Kerberos 感知的应用程序，包括大多数安全服务，依赖于身份验证请求中包含的计算机之间的时间同步。 AD DS 域控制器也必须具有同步时钟，以帮助确保精确的数据复制。

> [!NOTE]  
> 在 Windows Server 2003 和 Microsoft Windows 2000 Server 中，目录服务名为 Active Directory 目录服务。 在 Windows Server 2008 R2 和 Windows Server 2008 中，目录服务名为 Active Directory 域服务 (AD DS)。 本主题的其余部分引用了 AD DS，但信息也适用于 Windows Server 2016 中的 Active Directory 域服务。

W32Time 服务的实现位于一个名为 W32Time.dll，在默认情况下安装的动态链接库 **%Systemroot%\System32**。 W32Time.dll 最初为 Windows 2000 Server 以支持一种规范中所需同步的网络上的时钟的 Kerberos V5 身份验证协议开发。 从 Windows Server 2003 开始，W32Time.dll 提供通过 Windows Server 2000 操作系统中网络时钟同步提高的准确性。 此外，在 Windows Server 2003 W32Time.dll 支持各种硬件设备和网络时间协议使用时间提供程序。

虽然最初设计用于提供进行 Kerberos 身份验证的时钟同步，但许多当前应用程序使用时间戳来确保事务一致性，记录的时间的重要事件和其他业务关键型，时间敏感型信息。  这些应用程序受益于提供的 Windows 时间服务的计算机之间的时间同步。

## <a name="importance-of-time-protocols"></a>时间协议的重要性
时间协议来交换时间信息，然后使用该信息以使其时钟同步的两台计算机之间进行通信。 使用 Windows 时间服务时间协议，客户端从服务器请求时间的信息，并使其根据收到的信息的时钟同步。
  
Windows 时间服务使用 NTP 来帮助跨网络同步时间。 NTP 是一个 Internet 时间协议，包括同步时钟所需的专业算法。 NTP 是更准确的时间协议比简单网络时间协议 (SNTP)，可在某些版本的 Windows;但是，W32Time 继续支持 SNTP 若要启用与运行 SNTP 基于时服务，例如 Windows 2000 的计算机的向后兼容性。
<!-- maybe this should be its own topic under the Tech Ref section -->
## <a name="where-to-find-windows-time-service-configuration-related-information"></a>在哪里可以找到 Windows 时间服务与配置相关信息  
本指南 does**不**讨论配置 Windows 时间服务。 有几个不同 Microsoft TechNet 上和 Microsoft 知识库中执行操作说明主题配置 Windows 时间服务的过程。 如果您需要配置信息，以下主题有助于你找到相应的信息。  
<!-- should this be an if/then table -->
-   若要配置林根主域控制器 (PDC) 仿真器的 Windows 时间服务，请参阅：  
  
    -   [在目录林根域中的 PDC 仿真器上配置 Windows 时间服务](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29) 
  
    -   [配置林的时间源](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794823%28v%3dws.10%29) 
  
    -   Microsoft 知识库文章 816042，[如何在 Windows Server 中配置权威时间服务器](https://go.microsoft.com/fwlink/?LinkID=60402)，其中介绍了运行 Windows Server 2008 R2，Windows Server 2008 中，Windows Server 的计算机的配置设置2003、 和 Windows Server 2003 R2。  
  
-   若要在任何域成员客户端或服务器或未配置为目录林根 PDC 仿真器的甚至域控制器上配置 Windows 时间服务，请参阅[配置客户端计算机进行自动域时间同步](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29).  
  
    > [!WARNING]  
    > 某些应用程序可能需要他们的计算机能够具有高准确性时服务。 如果是这样，您可以选择配置手动时间源，但请注意，Windows 时间服务不旨在作为非常准确的时间源。 确保您已了解支持限制为高准确性时环境中 Microsoft 知识库文章所述文章 939322，[支持边界来配置 Windows 时间服务对于高准确性环境](support-boundary.md).  
  
-   若要配置 Windows 时间服务，任何基于 Windows 的客户端或服务器计算机上，配置为工作组成员而不是域成员查看[配置为所选客户端计算机手动时间源](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816656%28v%3dws.10%29)。  
  
-   若要在运行虚拟环境的主机计算机上配置 Windows 时间服务，请参阅 Microsoft 知识库文章 816042，[如何在 Windows Server 中配置权威时间服务器](https://go.microsoft.com/fwlink/?LinkID=60402)。 如果您正在使用非 Microsoft 虚拟化产品，请务必参考针对该产品的供应商的文档。  
  
-   若要在虚拟机中运行的域控制器上配置 Windows 时间服务，建议您部分禁用主机系统和来宾操作系统用作域控制器之间的时间同步。 这使来宾域控制器同步时间对于域层次结构，但防止它存在时间倾斜如果从已保存状态还原。 有关详细信息，请参阅 Microsoft 知识库文章 976924，[接收 Windows 时间服务事件 Id 24、 29%和 38 HYPER-V 的基于 Windows Server 2008 的主机服务器运行的虚拟化的域控制器上](https://go.microsoft.com/fwlink/?LinkID=192236)并[的虚拟化的域控制器部署考虑事项](https://go.microsoft.com/fwlink/?LinkID=192235)。  
  
-   若要在虚拟计算机中还运行在林根 PDC 仿真器作为域控制器上配置 Windows 时间服务，按照物理计算机的相同的说明中所述[上配置 Windows 时间服务目录林根域中的 PDC 仿真器](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191%28v=ws.10%29)。  
  
-   若要运行的虚拟计算机的成员服务器上配置 Windows 时间服务，请使用域时间层次结构中所述[配置自动域时间同步的客户端计算机](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc816884%28v%3dws.10%29)。


> [!IMPORTANT]  
> 在 Windows Server 2016 之前 W32Time 服务未旨在满足时间敏感的应用程序的需求。  但是，更新到 Windows Server 2016 现在，您可以实现用于 1 毫秒的解决方案在你的域中的准确性。  有关详细信息，请参阅[Windows 2016 精确时间](accurate-time.md)并[支持边界来配置 Windows 时间服务对于高准确性环境](support-boundary.md)有关详细信息。

## <a name="related-topics"></a>相关主题
- [Windows 2016 准确的时间](accurate-time.md)
- [适用于 Windows Server 2016 的时间准确性改进](windows-server-2016-improvements.md)  
- [Windows 时间服务的工作原理](How-the-Windows-Time-Service-Works.md)  
- [Windows 时间服务工具和设置](Windows-Time-Service-Tools-and-Settings.md)  
- [支持边界来配置高准确性环境的 Windows 时间服务](support-boundary.md)
- [Microsoft 知识库文章 902229](https://go.microsoft.com/fwlink/?LinkId=186066)