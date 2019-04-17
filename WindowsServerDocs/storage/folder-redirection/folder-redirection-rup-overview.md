---
title: 文件夹重定向、 脱机文件和漫游用户配置文件概述 （英文)
description: 文件夹重定向、 脱机文件和漫游用户配置文件的技术概述。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 4e3cf32cd718b906f16fc09901284d8520177df8
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233493"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>文件夹重定向、 脱机文件和漫游用户配置文件概述 （英文)

>适用于： Windows 10、 Windows 8、 Windows 8.1、 Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2016

本主题讨论文件夹重定向、 脱机文件 （客户端缓存或 CSC） 和漫游用户配置文件 （有时称为 RUP） 技术，包括 what's new 和在哪里可以找到其他信息。

## <a name="technology-description"></a>技术说明

结合使用“文件夹重定向”和“脱机文件”可将本地文件夹（例如“文档文件夹”）重定向到网络位置，同时在本地缓存内容，可提高速度和可用性。 使用“漫游用户配置文件”，将用户配置文件重定向到网络位置。 使用称为 Intellimirror 这些功能。

- **文件夹重定向**使用户和管理员以手动方式或通过使用组策略已知文件夹的路径重定向到新位置。 新位置可以是本地计算机上的文件夹或文件共享上的目录。 如同它仍存在的本地驱动器上，用户将交互重定向文件夹中的文件。 例如，您可以将文档文件夹，通常会存储在本地驱动器，重定向到网络位置。 文件夹中的文件是有效的用户从网络上的任何计算机。
- **脱机文件**使网络文件可供用户，即使网络连接到服务器不可用或速度缓慢。 联机工作，文件访问性能时的网络和服务器的速度。 脱机工作时，从本地访问速度脱机文件文件夹检索文件。 计算机切换到脱机模式时：
  - 已启用**始终脱机**模式
  - 服务器不可用
  - 网络连接较慢比可配置的阈值
  - 用户手动切换到脱机模式下通过使用 Windows 资源管理器中的**脱机工作**按钮
- **漫游用户配置文件**将重定向用户配置文件到文件共享，以便用户收到的相同的操作系统和多台计算机上的应用程序设置。 当用户登录到计算机上使用的帐户的设置与配置文件路径为文件共享时，用户的配置文件下载到本地计算机并合并与本地配置文件 （如果存在）。 当用户注销注销计算机时，包括任何更改，其配置文件的本地副本与配置文件的服务器副本合并。 通常，网络管理员启用漫游用户配置文件在域帐户。

## <a name="practical-applications"></a>实际应用程序

集中管理的用户数据和设置的存储并向用户提供脱机或网络或服务器停机时访问其数据的能力，管理员可以使用文件夹重定向、 脱机文件和漫游用户配置文件。 一些特定应用程序包括：

- 集中管理任务，如使用基于服务器的备份工具来备份用户文件夹和设置的客户端计算机中的数据。
- 使用户能够继续访问网络文件，即使没有网络或服务器中断。
- 优化的带宽使用情况和增强的可访问文件和文件夹由位于企业服务器场外承载的分支机构中的用户体验。
- 使移动用户能够脱机或低速网络中工作时，访问网络文件。

## <a name="new-and-changed-functionality"></a>新增功能和更改的功能

下表介绍一些文件夹重定向、 脱机文件和漫游用户配置文件的此版本中提供的主要变化。

|特性/功能|新功能或更新的功能？|描述|
|---|---|---|
|始终脱机模式|新增|通过始终脱机工作，通过高速网络连接进行连接，即使提供更快地访问文件和较低带宽使用率。|
|成本感知同步|新增|帮助用户避免同步时使用具有使用率限制的按流量计费的连接或其他提供程序的网络上漫游时从较高的数据使用情况成本。|
|主计算机支持|新增|可以用来限制文件夹重定向、 漫游用户配置文件，或为用户的主计算机同时使用。|

## <a name="always-offline-mode"></a>始终脱机模式

从 Windows 8 和 Windows Server 2012 开始，管理员可以配置脱机文件始终脱机工作，即使它们连接时通过高速网络连接的用户的体验。 Windows 更新默认情况下在后台，每小时同步脱机文件缓存中的文件。

### <a name="what-value-does-always-offline-mode-add"></a>哪些值始终脱机模式下添加呢？

始终脱机模式具有以下优点：

- 用户体验更快地访问重定向的文件夹，例如文档文件夹中的文件。
- 减少了网络带宽，从而会降低成本昂贵的 WAN 连接或按流量计费的连接，如 4 G 移动网络上。

### <a name="how-has-always-offline-mode-changed-things"></a>如何始终脱机模式已更改内容

Windows 8，Windows Server 2012 之前，用户将转换之间的联机和脱机模式，具体取决于网络的可用性和条件，即使已启用慢速链接模式 （也称为低速连接模式） 并将其设置为 1 毫秒延迟阈值。

始终脱机模式下，计算机永远不会转换为联机模式时配置的**配置慢速链接模式**组策略设置和**延迟**阈值参数设置为 1 毫秒。 更改同步在后台每个 120 分钟，默认情况下，但使用**配置背景同步**组策略设置同步进行配置。

有关详细信息，请参阅[启用始终脱机模式下更快地提供对文件的访问](enable-always-offline.md)。

## <a name="cost-aware-synchronization"></a>成本感知同步

使用成本感知同步时，Windows 禁用背景同步时使用按流量计费的网络连接，如 4 G 的移动网络和订阅者是 near 或超出其带宽限制，或其他提供程序的网络上漫游用户。

>[!NOTE]
>按流量计费的网络连接通常具有比转换到 Windows 8、 Windows Server 2012 和 Windows Server 2016 中脱机 （低速连接） 模式的默认 35 毫秒延迟值的要慢往返网络延迟。 因此，这些连接通常转换为脱机 （低速连接） 模式自动。

### <a name="what-value-does-cost-aware-synchronization-add"></a>成本感知同步添加哪些值？

成本感知同步可帮助用户避免意外较高的数据使用情况成本时使用具有使用率限制的按流量计费的连接或漫游其他提供程序的网络上。

### <a name="how-has-cost-aware-synchronization-changed-things"></a>如何成本感知同步已更改内容

Windows 8 和 Windows Server 2012 之前, 想要使用脱机在按流量计费的网络连接上的文件时最小化费用的用户必须通过使用从的移动网络提供程序的工具来跟踪其数据使用情况。 然后用户手动无法切换到脱机模式时它们所漫游、 接近其带宽限制，或超出其限制。

与成本感知同步，Windows 会自动跟踪在按流量计费的连接上的漫游和带宽使用率限制。 漫游用户，接近其带宽限制，或超出其限制时, Windows 切换到脱机模式，并阻止所有同步。 用户可以仍手动启动同步，以及管理员可以重写为特定用户，如 executives 成本感知同步。

有关详细信息，请参阅[计量网络上启用后台文件的同步](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11))。

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>文件夹重定向和漫游用户配置文件的主计算机

现在，您可以指定一组称为主计算机，使您能够的控制哪些计算机使用文件夹重定向、 漫游用户配置文件，或同时每个域用户的计算机。 指定主计算机是将用户数据和设置特定的计算机或设备，与相关联的简单且强大方法简化管理员监督、 提高数据安全性，并帮助防止用户配置文件损坏。

### <a name="what-value-do-primary-computers-add"></a>主计算机添加哪些值？

有四个指定的用户的主计算机的主要好处：

- 管理员可以指定哪些用户可以使用访问其重定向的数据和设置的计算机。 例如，管理员可以选择漫游用户的桌面和便携式计算机之间的用户数据和设置和该用户在登录到任何其他计算机，如会议室内计算机时不漫游的信息。
- 指定主计算机减少了安全和隐私的剩余的个人或公司数据保留在用户已登录的计算机的风险。 例如，登录到临时访问的员工的计算机的常规经理不遗留任何个人或公司的数据。
- 主计算机启用管理员联系以降低的配置不当风险或否则损坏的配置文件，可能会导致漫游之间不同系统，如之间配置基于 x86 和基于 x64 的计算机。
- 用户的漫游用户配置文件和/或重定向的文件夹将不会下载在于，可以更快的用户的首次登录的非主计算机上，如服务器，所需的时间量。 注销时间也将减少，因为不需要更改的用户配置文件上载到文件共享。

### <a name="how-have-primary-computers-changed-things"></a>如何主计算机已更改内容

若要限制到主计算机上下载专用用户数据，文件夹重定向和漫游用户配置文件技术时，执行以下逻辑检查一个用户登录到计算机上：

1. Windows 操作系统检查的新组策略设置 （**仅在主计算机上下载漫游的配置文件**和**仅主计算机上的重定向文件夹**） 以确定是否**msDS 主计算机**中活动的属性Directory 域服务 (AD DS) 应影响漫游用户的配置文件或应用文件夹重定向的决策。
2. 如果策略设置使主计算机支持，Windows 验证的 AD DS 架构支持**msDS 主计算机**属性。 如果是这样，确定 Windows，如果用户登录到的计算机被指定为用户的主计算机，如下所示：
    1. 如果计算机是用户的主计算机之一，Windows 应用漫游用户配置文件和文件夹重定向设置。
    2. 计算机不是一个用户的主计算机上，如果 Windows 加载用户的缓存的本地配置文件，如果存在此参数，或创建一个新的本地配置文件。 Windows 还会删除任何现有的重定向的文件夹根据已由以前应用组策略设置，将保留在本地文件夹重定向配置中指定的删除操作。

有关详细信息，请参阅[部署文件夹重定向和漫游用户配置文件的主计算机](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>硬件要求

文件夹重定向、 脱机文件和漫游用户配置文件需要基于 x64 的计算机或基于 x86 的计算机，并且它们不支持 Windows 基于 ARM 工单协议的计算机上。

## <a name="software-requirements"></a>软件要求

若要指定主计算机，您的环境必须满足以下要求：

- 必须 Active Directory 域服务 (AD DS) 架构更新以包括 Windows Server 2012 架构和条件 （自动安装的 Windows Server 2012 或更高版本的域控制器更新架构）。 有关升级的 AD DS 架构的详细信息，请参阅[将域控制器升级到 Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md)。
- 客户端计算机必须运行 Windows 10、 Windows 8.1、 Windows 8、 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012 和加入正在管理的 Active Directory 域。

## <a name="more-information"></a>详细信息

有关其他相关信息，请参阅以下资源。

|内容类型|引用|
|---|---|
|产品评估|[支持与可靠文件服务和存储的信息工作者](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[什么是脱机文件中](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>)（Windows 7 和 Windows Server 2008 R2）<br>[What's New 脱机文件适用于 Windows Vista 中](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>[在 Windows Vista 中的脱机文件的更改](<https://technet.microsoft.com/library/2007.11.offline.aspx>)（TechNet 杂志）|
|部署|[部署文件夹重定向、 脱机文件和漫游用户配置文件](deploy-folder-redirection.md)<br>[正在实施最终用户数据集中化解决方案： 文件夹重定向和脱机文件技术验证和部署](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[管理漫游用户数据部署指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[脱机新配置文件功能为 Windows 7 计算机分步指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[使用文件夹重定向](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[实现文件夹重定向](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>)(在 Windows Server 2003)|
|工具和设置|[MSDN 上的脱机文件](https://msdn.microsoft.com/library/cc296092.aspx)<br>[脱机文件组策略参考 （英文）](https://msdn.microsoft.com/library/ms878937.aspx)(Windows 2000)|
|社区资源|[文件服务和存储论坛](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[嗨，脚本专家 ！ 如何可以使用 Windows 中的脱机文件功能？](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[嗨，脚本专家 ！ 如何启用和禁用脱机文件？](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)|
相关技术|[标识和 Windows Server 中的访问](../../identity/identity-and-access.md)<br>[Windows Server 中的存储](../storage.md)<br>[远程访问和服务器管理](../../remote/index.md)|