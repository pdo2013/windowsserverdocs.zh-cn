---
title: 发行说明-Windows Server 2019 中的重要问题
description: 总结了需要解决方法为了避免崩溃，还是正在挂起安装故障和数据丢失的重要问题
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
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810737"
---
# <a name="release-notes---important-issues-in-windows-server-2019"></a>发行说明-Windows Server 2019 中的重要问题

>适用于：Windows Server 2019

这些发行说明汇总了在 Windows Server 2019 操作系统系统中，包括避免或解决问题，方法，如果已知的最关键问题。 按设计更改、 新功能和修补程序在此版本中的信息，请参阅[What's New in Windows Server 2019](whats-new-19.md)和特定功能团队的通知。 除非另行指定，否则每个所报告的问题适用于所有版本和安装选项安装的 Windows Server 2019。  

本文档将持续更新。 发现需要解决的严重问题，以及有可用的新的解决方法和修补程序时，它们将被添加到此文档中。  

## <a name="release-notes"></a>发行说明

Windows Server 2019 中存在以下已知的问题。

| 标题         | 描述                            |
| -----         | -----------                            |
| 在服务器安装过程中的安装选项菜单已截断德语文本 | 桌面体验安装选项的说明在"选择你想要安装，操作系统"操作系统选择窗口标题为从正使用德语服务器媒体运行安装程序时将结束时具有缺少和不正确的字符句子。 下面是完整德语文本，它应显示。<br/>      <br/>`Durch diese Option wird die vollständige grafische Umgebung von Windows installiert, wodurch zusätzlicher Speicherplatz verbraucht wird. Sie kann hilfreich sein, wenn Sie den Windows-Desktop verwenden möchten oder über eine App verfügen, die die grafische Umgebung benötigt.` <br><br>这只会影响在公共 Windows Server 2019 可用性、 Windows Server、 版本 1809，和 Microsoft HYPER-V Server 2019 时释放的德语媒体。|
| 在 Windows Server 版本 1809年安装过程中不正确的 Windows Server 品牌图像 | Windows Server 版本 1809，在安装程序体验期间的背景图像上一些初始屏幕，显示&quot;Windows Server 2019&quot;。  正如使用 Windows Server，版本 1709年和 1803，这应只是说&quot;Windows Server&quot;。  有其他影响在产品中，任何其他位置，并且没有对 Windows Server 2019 产品没有任何影响。  在 Windows server，版本 1809，可仅用于访问批量许可服务中心的批量许可客户的安装过程，此问题仅限于此一个映像。<br/> |

### <a name="copyright"></a>版权

本文档按“原样”提供。 本文档中表达的信息和视图（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。  

本文档未向你提供针对任何 Microsoft 产品的任何知识产权的任何法律权限。 你可以复制和使用本文档作为内部参考之用。

&copy; 2019 Microsoft Corporation. 保留所有权利。  

Microsoft、Active Directory、Hyper-V、Windows 和 Windows Server 是 Microsoft Corporation 在美国和/或其他国家/地区的注册商标或商标。  

本产品包含图形过滤器软件；该软件在一定程度上是以独立 JPEG 小组所做工作为基础的。  


1.0  
