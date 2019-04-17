---
title: Windows Admin Center 概述
description: 了解如何使用 Windows Admin Center (Project Honolulu) 管理 Windows Server
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: high
ms.prod: windows-server-threshold
ms.openlocfilehash: d941e9884dced40ce750645b662d8df73503bc41
ms.sourcegitcommit: 475292afc919c6d17569f05007a97bc6b92dd225
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2019
ms.locfileid: "9267773"
---
# Windows Admin Center

>适用于：Windows Admin Center、Windows Admin Center 预览版

欢迎使用 Windows Admin Center！

**Windows Admin Center**（代号为 **Project Honolulu**）是 Windows Server“内部”管理工具的演进版；它是单一虚拟管理平台，整合了有关本地和远程服务器管理的各个方面。 作为一种本地部署、基于浏览器的管理体验，不需要 Internet 连接和 Azure。 利用 Windows Admin Center，你可以全面控制部署的各个方面，包括未连接到 Internet 的专用网络。

## 简介

>[!VIDEO https://www.youtube.com/embed/PcQj6ZklmK0]

![Windows Admin Center 信息图](media/WAC1809Poster_thumb.PNG)

[下载 PDF](https://github.com/MicrosoftDocs/windowsserverdocs/raw/master/WindowsServerDocs/manage/windows-admin-center/media/WindowsAdminCenter1809Poster.pdf)

## 快速入门

你可以在几分钟内在你的环境中启动并运行 Windows Admin Center：

1. [下载](https://aka.ms/windowsadmincenter)
2. [安装](deploy/install.md)
3. [入门](use/get-started.md)

## 内容概览

<table>
    <tr></tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>了解</h3>
            <ul>
            <li><a href="understand/what-is.md">什么是 Windows Admin Center？</a>
            <li><a href="understand/faq.md">常见问题解答</a>
            <li><a href="understand/case-studies.md">案例研究</a>
            <li><a href="understand/related-management.md">相关管理产品</a>
            <li><a href="understand/videos.md">视频</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>计划</h3>
            <ul>
            <li><a href="plan/installation-options.md">哪种安装类型适合你？</a>
            <li><a href="plan/user-access-options.md">用户访问选项</a>
            <li><a href="plan/azure-integration-options.md">存在哪些 Azure 集成选项？</a>
            <br>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>部署</h3>
            <ul>
            <li><a href="deploy/prepare-environment.md">准备你的环境</a>
            <li><a href="deploy/install.md">安装 Windows Admin Center</a>
            <li><a href="deploy/high-availability.md">实现高可用性</a>
         </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>配置</h3>
            <ul>
            <li><a href="configure/settings.md">Windows Admin Center 设置</a>
            <li><a href="configure/user-access-control.md">用户访问控制和权限</a>
            <li><a href="configure/using-extensions.md">扩展</a>
            <li><a href="configure/azure-integration.md">与 Azure 集成</a>
            <li><a href="configure/manage-azure-vms.md">通过 Windows Admin Center 管理 Azure 虚拟机</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td style="vertical-align: top;">
            <h3>用途</h3>
            <ul>
            <li><a href="use/get-started.md">启动和添加连接</a>
            <li><a href="use/manage-servers.md">管理服务器</a>
            <li><a href="use/manage-hyper-converged.md">管理超级集成基础架构</a>
            <li><a href="use/manage-failover-clusters.md">管理故障转移群集</a>
            <li><a href="use/manage-virtual-machines.md">管理虚拟机</a>
            <li><a href="use/azure-services.md">利用 Azure 服务</a>
            <li><a href="use/troubleshooting.md">常见疑难解答步骤</a>
            <li><a href="use/logging.md">日志记录</a>
            <li><a href="use/known-issues.md">已知问题</a>
            </ul>
        </td>
        <td style="vertical-align: top;">
            <h3>扩展</h3>
            <ul>
            <li><a href="extend/extensibility-overview.md">扩展概述</a>
            <li><a href="extend/understand-extensions.md">了解扩展</a>
            <li><a href="extend/developing-extensions.md">开发扩展</a>
            <li><a href="extend/publish-extensions.md">指南</a>
            <li><a href="extend/publish-extensions.md">发布扩展</a>
            </ul>
        </td>
    </tr>

</table>

## 版本历史

了解我们发布的最新功能：

- 版本 [1903](https://aka.ms/wac1903) 可提供来自 Azure Monitor 的电子邮件通知、支持从 Active Directory 添加服务器或电脑连接，还具备用于管理 Active Directory、DHCP 和 DNS 的新工具。
- 版本 [1902] (https://aka.ms/wac1902)将共享的连接列表和改进添加到了软件定义的网络 (SDN) 管理，包括管理 ACL 的新 SDN 工具、网关连接和逻辑网络。
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

## 不断更新

<a target="_blank" class="mscom-link twitter-follow-link" title="在 Twitter 上关注我们" aria-label="Follow us on Twitter" data-info="Twitter" href="https://twitter.com/servermgmt"><picture><source srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR" media="(min-width:0)"><img srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR" alt="Follow us on Twitter" src="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOolR"></picture></a>
 | 
<a target="_blank" class="mscom-link blogs-follow-link" title="阅读我们的博客" aria-label="Visit our Blogs" data-info="Blogs" href="https://blogs.technet.microsoft.com/servermanagement/"><picture><source srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw" media="(min-width:0)"><img srcset="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw" alt="Follow us on Blogs" src="//img-prod-cms-rt-microsoft-com.akamaized.net/cms/api/am/imageFileData/REOtyw"></picture></a>
