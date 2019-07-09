---
title: 发行说明 - Windows Server 2019 中的重要问题
description: 总结了需要解决方法的重要问题，以避免故障、挂起、安装失败和数据丢失
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
ms.assetid: 134aab85-664f-4d55-87ef-9e5fd098371f
author: jasongerend
ms.author: jgerend
manager: jasgroce
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 515255c301d343aa1b83bcfb506f2e3baa6ca969
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810737"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>发行说明 - Windows Server 2019 中的重要问题

>适用于：Windows Server 2019

这些发行说明汇总了 Windows Server 2019 操作系统中最关键的问题，包括避免或解决问题的方法（如果已知）。 有关此版本中按设计的更改、新功能和修补程序的信息，请参阅 [Windows Server 2019 中的新增功能](whats-new-19.md)和特定功能团队提供的通知。 除非另有指定，否则，每个所报告的问题均适用于 Windows Server 2019 的所有版本和安装选项。  

本文档将持续更新。 发现需要解决的严重问题，以及有可用的新的解决方法和修补程序时，它们将被添加到此文档中。  

## <a name="release-notes"></a>发行说明

Windows Server 2019 中存在以下已知问题。

| Title         | 描述                            |
| -----         | -----------                            |
| 服务器安装过程中的安装选项菜单已截断德语文本 | 当从德语服务器媒体运行安装程序时，在名为“选择要安装的操作系统”的操作系统选择窗口中，桌面体验安装选项的描述将在句末出现缺失和不正确的字符。 下面是应显示的完整德语文本。<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>这只会影响在公开可用的 Windows Server 2019、Windows Server 版本 1809 和 Microsoft HYPER-V Server 2019 中发布的德语媒体。|
| 在安装 Windows Server 版本 1809 时，Windows Server 品牌图像不正确 | 在 Windows Server 版本 1809 安装体验期间，一些初始屏幕上的背景图像显示 &quot;Windows Server 2019&quot;。  与 Windows Server 版本 1709 和 1803 一样，应简单地显示为 &quot;Windows Server&quot;。  对该产品的其他任何方面没有其他影响，对 Windows Server 2019 产品也没有影响。  在安装 Windows Server 版本 1809 时，该问题仅限于这一个映像，只对访问批量许可服务中心的批量许可客户可用。<br/> |

### <a name="copyright"></a>版权

本文档按“原样”提供。 本文档中表达的信息和视图（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。  

本文档未向你提供针对任何 Microsoft 产品的任何知识产权的任何法律权限。 你可以复制和使用本文档作为内部参考之用。

&copy; 2019 Microsoft Corporation。 保留所有权利。  

Microsoft、Active Directory、Hyper-V、Windows 和 Windows Server 是 Microsoft Corporation 在美国和/或其他国家/地区的注册商标或商标。  

本产品包含图形过滤器软件；该软件在一定程度上是以独立 JPEG 小组所做工作为基础的。  


1.0  
