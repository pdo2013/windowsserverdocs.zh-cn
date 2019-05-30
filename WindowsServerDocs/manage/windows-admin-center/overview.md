---
title: Windows Admin Center 概述
description: 了解如何使用 Windows Admin Center (Project Honolulu) 管理 Windows Server
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 04/12/2019
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: ee3c4ba5d6c3dc911ab318ade9a46b279317496f
ms.sourcegitcommit: 39ab8041d166e6817a95417d6aa30bc7abeeef54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/28/2019
ms.locfileid: "66260260"
---
# <a name="windows-admin-center"></a>Windows Admin Center

>适用于：Windows Admin Center，Windows Admin Center 预览版

**Windows Admin Center** (代码名为**项目 Honolulu**) 是一种演变，Windows Server 中框管理工具; 它是单一管理平台，包含本地和远程服务器管理的所有方面。 作为一种本地部署、基于浏览器的管理体验，不需要 Internet 连接和 Azure。 利用 Windows Admin Center，你可以全面控制部署的各个方面，包括未连接到 Internet 的专用网络。

## <a name="introduction"></a>简介

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Windows Admin Center 信息图](media/WAC1809Poster_thumb.PNG)

[下载 pdf 文件](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## <a name="quick-start"></a>快速入门

你可以在几分钟内在你的环境中启动并运行 Windows Admin Center：

1. [下载](https://aka.ms/windowsadmincenter)
2. [安装](deploy/install.md)
3. [开始行动](use/get-started.md)

## <a name="contents-at-a-glance"></a>一眼的内容

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>了解</h3>
            <ul>
            <li><a href="understand/what-is.md">什么是 Windows Admin Center？</a>
            <li><a href="understand/faq.md">常见问题</a>
            <li><a href="understand/case-studies.md">案例研究</a>
            <li><a href="understand/related-management.md">相关的管理产品</a>
            <li><a href="understand/videos.md">视频</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>规划</h3>
            <ul>
            <li><a href="plan/installation-options.md">哪种类型是安装的最适合你？</a>
            <li><a href="plan/user-access-options.md">用户访问选项</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>部署</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">准备环境</a>
            <li><a href="deploy/install.md">安装 Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">启用高可用性</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>配置</h3>
            <ul>
            <li><a href="configure/settings.md">Windows Admin Center 设置</a>
            <li><a href="configure/user-access-control.md">用户访问控制和权限</a>
            <li><a href="configure/using-extensions.md">扩展插件</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>将</h3>
            <ul>
            <li><a href="use/get-started.md">启动和添加连接</a>
            <li><a href="use/manage-servers.md">管理服务器</a>
            <li><a href="use/manage-hyper-converged.md">管理超聚合基础结构</a>
            <li><a href="use/manage-failover-clusters.md">管理故障转移群集</a>
            <li><a href="use/manage-virtual-machines.md">管理虚拟机</a>
            <li><a href="use/logging.md">日志记录</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>连接到 Azure</h3>
            <ul>
            <li><a href="azure/index.md">Azure 混合服务</a></li>
            <li><a href="azure/azure-integration.md">Windows Admin Center 连接到 Azure</a></li>
            <li><a href="azure/deploy-wac-in-azure.md">部署在 Azure 中 Windows Admin Center</a></li>
            <li><a href="azure/manage-azure-vms.md">通过 Windows Admin Center 管理 Azure 虚拟机</a></li>
            </ul>
        </td>
    </tr>
    <tr>
            <td style="vertical-align: top;">
            <h3>支持</h3>
            <ul>
            <li><a href="support/index.md">支持策略</a>
            <li><a href="support/troubleshooting.md">常见的疑难解答步骤</a>
            <li><a href="support/known-issues.md">已知问题</a>
            </ul>
        </td>
            <td style="vertical-align: top;">
            <h3>扩展</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">扩展概述</a>
            <li><a href="extend/understand-extensions.md">了解扩展</a>
            <li><a href="extend/developing-extensions.md">开发扩展</a>
            <li><a href="extend/publish-extensions.md">参考线</a>
            <li><a href="extend/publish-extensions.md">发布扩展</a>
            </ul>
        </td>
    </tr>

</table>

## <a name="release-history"></a>发布历史记录

了解我们发布的最新功能：

- 版本 1904.1-对维护进行更新，以提高稳定性的网关插件。
- 版本[1904年](https://aka.ms/wac1904)是最新 GA 版本引入了 Azure 混合服务工具，并将以前在预览版中供 GA 通道的功能。
- 版本[1903年](https://aka.ms/wac1903)通过 Azure Monitor，能够从 Active Directory 和新工具来管理 Active Directory、 DHCP 和 DNS 中添加服务器或电脑连接将电子邮件通知。
- 版本[1902年](https://aka.ms/wac1902)添加到软件定义网络 (SDN) 管理，包括新的 SDN 工具来管理 Acl、 网关连接和逻辑网络的共享的连接列表和改进。
- 版本 [1812](https://aka.ms/wac1812) 添加了深色主题（预览版）、电源配置设置、 BMC 信息以及 PowerShell 支持，以管理[扩展](./configure/using-extensions.md#manage-extensions-with-powershell)和[连接](./use/get-started.md#use-powershell-to-import-or-export-your-connections-with-tags)。
- 版本 [1809.5](https://aka.ms/wac1809.5) 是 GA 累积更新，其中包含整个平台的各种质量提升、功能改进和 Bug 修复，以及超融合基础设施管理解决方案中的若干新功能。
- 版本 [1809](https://cloudblogs.microsoft.com/windowsserver/2018/09/20/windows-admin-center-1809-and-sdk-now-generally-available/) 是以前的 GA 版本，在 GA 通道中提供之前的预览版功能。
- 版本 [1808](https://aka.ms/WACPreview1808-InsiderBlog) 添加了“已安装应用”工具、进行了大量的后台改进并增加了对预览版 SDK 的主要更新。
- 版本 [1807](https://aka.ms/WACPreview1807-InsiderBlog) 添加了简化的 Azure 连接体验、改进了虚拟机库存页面、增加了文件共享功能和 Azure 更新管理集成等。 
- 版本 [1806](https://aka.ms/WACPreview1806-InsiderBlog) 添加了显示 PowerShell 脚本、SDN 管理、2008 R2 连接、SDN、计划的任务和许多其他改进。
- 版本 1804.25 - 维护更新以为在完全脱机的环境中安装 Windows Admin Center 的用户提供支持。
- 版本 [1804](https://cloudblogs.microsoft.com/windowsserver/2018/04/12/announcing-windows-admin-center-our-reimagined-management-experience/) - Project Honolulu 成为了 Windows Admin Center，并添加了安全功能和基于角色的访问控制。 我们的第一个 GA 发行版。
- 版本 [1803](https://blogs.windows.com/windowsexperience/2018/03/13/announcing-project-honolulu-technical-preview-1803-and-rsat-insider-preview-for-windows-10) 添加了对 Azure AD 访问控制、详细日志记录、可调整的内容和一批工具改进的支持。
- 版本 [1802](https://blogs.windows.com/windowsexperience/2018/02/13/announcing-windows-server-insider-preview-build-17093-project-honolulu-technical-preview-1802) 添加了对辅助功能、本地化、高可用性部署、标记、Hyper-V 主机设置和网关身份验证的支持。
- 版本 [1712](https://blogs.windows.com/windowsexperience/2017/12/19/announcing-project-honolulu-technical-preview-1712-build-05002) 在工具中添加了更多的虚拟机功能和性能改进。
- 版本 [1711](https://cloudblogs.microsoft.com/windowsserver/2017/12/01/1711-update-to-project-honolulu-technical-preview-is-now-available/) 添加了备受期待的工具（远程桌面和 PowerShell）以及其他改进。
- 版本 [1709](https://cloudblogs.microsoft.com/windowsserver/2017/09/22/project-honolulu-technical-preview-is-now-available-for-download/) 已作为我们的第一个公共预览版启动。

## <a name="stay-updated"></a>保持更新状态

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR)[Twitter 上关注我们](https://twitter.com/servermgmt)

![ ](//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw)[阅读我们的博客](https://blogs.technet.microsoft.com/servermanagement/)
