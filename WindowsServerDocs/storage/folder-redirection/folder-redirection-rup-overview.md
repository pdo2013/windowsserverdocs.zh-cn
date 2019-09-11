---
title: 文件夹重定向、脱机文件和漫游用户配置文件概述
description: 文件夹重定向、脱机文件和漫游用户配置文件技术的概述。
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 04/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d6c980cd9e7d96261ffe467f4d9da627e3c50102
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867243"
---
# <a name="folder-redirection-offline-files-and-roaming-user-profiles-overview"></a>文件夹重定向、脱机文件和漫游用户配置文件概述

>适用于：Windows 10、Windows 8、Windows 8.1、Windows Server 2019、Windows Server 2016、Windows Server 2012、Windows Server 2012 R2

本主题讨论文件夹重定向、脱机文件（客户端缓存或 CSC）以及漫游用户配置文件（有时称为 RUP）技术，包括新增功能和查找其他信息的位置。

## <a name="technology-description"></a>技术描述

结合使用“文件夹重定向”和“脱机文件”可将本地文件夹（例如“文档文件夹”）重定向到网络位置，同时在本地缓存内容，可提高速度和可用性。 使用“漫游用户配置文件”，将用户配置文件重定向到网络位置。 所用的这些功能可视为 Intellimirror。

- **文件夹重定向** 使用户和管理员能够手动或通过使用组策略将已知文件夹的路径重定向到新位置。 新位置可以是本地计算机上的文件夹，也可以是文件共享上的目录。 用户可与重定向的文件夹中的文件交互，犹如它依然存在于本地驱动器上。 例如，你可将文档文件夹（通常存储在本地驱动器上）重定向到网络位置。 这样，网络中任何计算机的用户就都可以使用该文件夹中的文件。
- **脱机文件使用户**可以使用网络文件，即使到服务器的网络连接不可用或速度慢也是如此。  联机工作时，文件访问性能取决于网络和服务器的速度。 当脱机工作时，可按本地访问速度从脱机文件文件夹中检索文件。 在以下情况下，计算机切换到“脱机”模式：
  - "**始终脱机**" 模式已启用
  - 服务器不可用
  - 网络连接比可配置的阈值更加缓慢。
  - 用户使用 Windows Explorer 中的 **“脱机工作”** 按钮，手动切换到脱机模式。
- **漫游用户配置文件** 将用户配置文件重定向到文件共享，以便用户在多台计算机上接收相同的操作系统和应用程序设置。 当用户使用设置为文件共享作为配置文件路径的帐户登录计算机时，用户的配置文件将下载到本地计算机，并与本地配置文件（如果有）合并。 当用户注销计算机时，其配置文件的本地副本（包括任何更改）将与配置文件的服务器副本合并。 通常，网络管理员会在域帐户上启用漫游用户配置文件。

## <a name="practical-applications"></a>实际应用程序

管理员可使用“文件夹重定向”、“脱机文件”和“漫游用户配置文件”集中存储用户数据和设置，以便用户能够在脱机或网络或服务器不可用的情况下访问其数据。 一些特定的应用包括：

- 为管理任务将客户端计算机中的数据集中在一起，例如使用基于服务器的备份工具，备份用户文件夹和设置。
- 可让用户继续访问网络文件，即使网络或服务器不可用。
- 优化带宽使用率，并改善分支机构中的用户访问非现场公司服务器托管的文件和文件夹的体验。
- 可让移动用户在脱机工作或通过速度较慢的网络工作时访问网络文件。

## <a name="new-and-changed-functionality"></a>新增功能和更改的功能

下表描述了“文件夹重定向”、“脱机文件”和在本版本中可用的“漫游用户配置文件”中的一些主要更改。

| 特性/功能 | 新功能或更新的功能？ | 描述 |
| --- | --- | --- |
| “始终脱机”模式 | 新增 | 通过始终脱机工作的方式，更快速地访问文件并降低带宽使用率，即使连接高速网络。 |
| 感知成本的同步 | 新增 | 当使用具有使用限制的按流量计费的连接或在其他提供程序的网络上漫游时，帮助用户避免同步的数据使用成本高。 |
| 主计算机支持 | 新增 | 使你能够限制仅使用文件夹重定向、漫游用户配置文件，或者将两者同时限制为用户的主要计算机。 |

## <a name="always-offline-mode"></a>“始终脱机”模式

从 Windows 8 和 Windows Server 2012 开始，管理员可以将脱机文件用户的体验配置为始终脱机工作，即使这些用户通过高速网络连接进行连接也是如此。 默认情况下，Windows 通过在后台每小时同步一次来更新脱机文件缓存中的文件。

### <a name="what-value-does-always-offline-mode-add"></a>总是脱机模式添加什么值？

“始终脱机”模式具有以下优点：

- 用户可更快地访问重定向文件夹（例如“文档文件夹”）中的文件。
- 减少网络带宽，从而降低昂贵的 WAN 连接或按流量计费的连接（4G 移动网络）的费用。

### <a name="how-has-always-offline-mode-changed-things"></a>"始终脱机" 模式有哪些更改？

在 Windows 8、Windows Server 2012 之前，用户将根据网络可用性和状况在联机和脱机模式之间转换，即使已启用慢速链接模式（也称为慢速连接模式）并将其设置为1毫秒延迟阈值。

对于 "始终脱机" 模式，当配置 "**配置慢速链接模式**组策略" 设置，并且 "**延迟**阈值" 参数设置为1毫秒时，计算机永远不会转换到联机模式。 默认情况下，每隔 120 分钟在后台对更改进行一次同步，但使用 **Configure Background Sync** 组策略设置可配置同步。

有关详细信息，请参阅[启用“始终脱机”模式，加快文件访问速度](enable-always-offline.md)。

## <a name="cost-aware-synchronization"></a>感知成本的同步

使用有成本的同步，当用户使用按流量计费的网络连接（例如4G 移动网络），并且订阅服务器的带宽限制接近或超出其带宽限制时，Windows 将禁用后台同步，或在另一个提供商的网络上漫游。

> [!NOTE]
> 按流量计费的网络连接通常具有往返网络延迟，这比 Windows 8、Windows Server 2019、Windows Server 2016 和 Windows Server 中的转换为脱机（慢速连接）模式的默认35毫秒延迟值慢2012。 因此，这些连接通常自动转换为“脱机”（慢速连接）模式。

### <a name="what-value-does-cost-aware-synchronization-add"></a>计算成本的同步的值是什么？

在使用具有使用限制的按流量计费的连接或在其他提供商的网络上漫游时，可帮助用户避免意外的高数据使用成本。

### <a name="how-has-cost-aware-synchronization-changed-things"></a>如何更改成本识别的同步？

在 Windows 8 和 Windows Server 2012 之前，如果用户希望在按流量计费的网络连接上使用脱机文件，则必须使用移动网络提供商提供的工具来跟踪数据的使用情况。 然后用户应手动切换到“脱机”模式，以便在漫游时，接近其带宽限制或超过限制。

使用经济的同步，Windows 会在按流量计费的连接时自动跟踪漫游和带宽使用限制。 当用户漫游时，接近他们的带宽限制或超过限制，则 Windows 切换为“脱机”模式，并阻止所有同步操作。 用户依然可手动启动同步，管理员可为特定用户（例如主管人员）覆盖感知成本的同步。

有关详细信息，请参阅 [Enable Background File Synchronization on Metered Networks](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127408(v%3dws.11))。

## <a name="primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>适用于文件夹重定向和漫游用户配置文件的主计算机

你现在可以为每个域用户指定一组称为 "主计算机" 的计算机，使你能够控制哪些计算机使用文件夹重定向、漫游用户配置文件或同时使用这两者。 指定主计算机可简单高效地将用户数据和设置与特定计算机或设备相关联，从而让管理员更加轻松地监视、改进数据安全性，以及帮助保护用户配置文件免遭损坏。

### <a name="what-value-do-primary-computers-add"></a>主计算机添加什么值？

为用户指定主计算机有以下四大优点：

- 管理员可以指定用户能够用来访问其重定向的数据和设置的计算机。 例如，管理员可以选择在用户的桌面和便携式计算机之间漫游用户数据和设置，并在用户登录到任何其他计算机（例如会议室计算机）时不漫游该信息。
- 指定主计算机可降低将残余个人或公司数据保留在用户登录的计算机上的安全和隐私风险。 例如，登录到员工的计算机进行临时访问的一般经理不会留下任何个人或公司数据。
- 主计算机可让管理员减少配置不正确或损坏配置文件的风险，从而避免在不同配置的系统之间漫游，如在基于 x86 和基于 x64 的计算机之间。
- 用户首次登录非主计算机（例如服务器）所需的时间更快，因为不会下载用户的漫游用户配置文件和/或重定向的文件夹。 还减少注销时间，因为对用户配置文件的更改无需上传到文件共享。

### <a name="how-have-primary-computers-changed-things"></a>主计算机有哪些更改？

为仅将私人用户数据下载到主计算机中，当用户登录计算机时，文件夹重定向和漫游用户配置文件技术可执行以下逻辑检查：

1. Windows 操作系统检查新的组策略设置（仅**在主计算机上下载漫游配置文件**并在**主计算机上重定向文件夹**）以确定 Active 中的**计算机**上的属性Directory 域服务（AD DS）应影响漫游用户的配置文件或应用文件夹重定向的决定。
2. 如果策略设置启用主计算机支持，则 Windows 验证 AD DS 架构是否支持 **ms-DS-Primary-Computer** 属性。 如果支持，则 Windows 按照如下方式确定是否将用户登录的计算机指定为用户的主计算机：
    1. 如果计算机是用户的主计算机之一，则 Windows 将应用漫游用户配置文件和文件夹重定向设置。
    2. 如果计算机不是用户的主计算机之一，则 Windows 将加载用户的缓存本地配置文件（如果存在）或创建新的本地配置文件。 Windows 还根据之前应用的组策略设置（保留在本地“文件夹重定向”配置中）指定的删除操作删除任何现有的重定向文件夹。

有关详细信息，请参阅[为主计算机部署文件夹重定向和漫游用户配置文件](deploy-primary-computers.md)

## <a name="hardware-requirements"></a>硬件要求

“文件夹重定向”、“脱机文件”和“漫游用户配置文件”需要基于 x64 或 x86 的计算机，基于 ARM (WOA) 的计算机上的 Windows 并不支持它们。

## <a name="software-requirements"></a>软件要求

为指定主计算机，你的环境必须符合以下要求：

- 必须更新 Active Directory 域服务（AD DS）架构以包括 Windows Server 2012 架构和条件（安装 Windows Server 2012 或更高版本的域控制器会自动更新架构）。 有关升级 AD DS 架构的详细信息，请参阅[将域控制器升级到 Windows Server 2016](../../identity/ad-ds/deploy/upgrade-domain-controllers.md)。
- 客户端计算机必须运行 Windows 10、Windows 8.1、Windows 8、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012，并加入所管理的 Active Directory 域。

## <a name="more-information"></a>详细信息

有关其他相关信息，请参阅以下资源。

| 内容类型 | 参考资料 |
| --- | --- |
| 产品评估 | [支持具有可靠文件服务和存储的信息工作者](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831495(v%3dws.11)>)<br>[脱机文件中的新增功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff183315(v=ws.10)>)（Windows 7 和 Windows Server 2008 R2）<br>[适用于 Windows Vista 的脱机文件中的新增功能](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc749449(v=ws.10)>)<br>对[Windows Vista 中脱机文件的更改](<https://technet.microsoft.com/library/2007.11.offline.aspx>)（TechNet 杂志） |
| 部署 | [部署文件夹重定向、脱机文件和漫游用户配置文件](deploy-folder-redirection.md)<br>[实现最终用户数据集中解决方案：文件夹重定向和脱机文件技术验证和部署](http://download.microsoft.com/download/3/0/1/3019A3DA-2F41-4F2D-BBC9-A6D24C4C68C4/Implementing%20an%20End-User%20Data%20Centralization%20Solution.docx)<br>[管理漫游用户数据部署指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-vista/cc766489(v=ws.10)>)<br>[为 Windows 7 计算机配置新增脱机文件功能循序渐进指南](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff633429(v=ws.10)>)<br>[使用文件夹重定向](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753996(v=ws.11)>)<br>[实现文件夹重定向](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc737434(v=ws.10)>)（Windows Server 2003） |
| 工具和设置 | [MSDN 上的脱机文件](https://msdn.microsoft.com/library/cc296092.aspx)<br>[组策略引用脱机文件](https://msdn.microsoft.com/library/ms878937.aspx)（Windows 2000） |
| 社区资源 | [文件服务和存储论坛](https://social.technet.microsoft.com/forums/windowsserver/home?forum=winserverfiles)<br>[您好，脚本专家！如何在 Windows 中使用脱机文件功能？](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>)<br>[您好，脚本专家！如何启用和禁用脱机文件？](<https://blogs.technet.microsoft.com/heyscriptingguy/2009/06/02/hey-scripting-guy-how-can-i-enable-and-disable-offline-files/>) |
| 相关技术|[Windows Server 中的标识和访问](../../identity/identity-and-access.md)<br>[Windows Server 中的存储](../storage.md)<br>[远程访问和服务器管理](../../remote/index.md) |