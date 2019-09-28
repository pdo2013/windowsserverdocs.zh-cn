---
title: 发行说明 - Windows Server 版本 1709 中的重要问题
description: 总结了需要解决方法的重要问题，以避免故障、挂起、安装失败、数据丢失。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 04/23/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 7d5f899964414c10350cc22a594a959c940a1514
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71391518"
---
# <a name="release-notes-important-issues-in-windows-server-version-1709"></a>发行说明:Windows Server 版本 1709 中的重要问题

>适用于：Windows Server 半年频道

这些发行说明汇总了 Windows Server&reg; 操作系统中最关键的问题，包括避免或解决问题的方法（如果已知）。 有关此版本中按设计的更改、新功能和修补程序的信息，请参阅 [Windows Server 版本 1709 中的新增功能](whats-new-in-windows-server-1709.md)和特定功能团队提供的通知。 除非另有指定，否则，每个所报告的问题均适用于 Windows Server 2016 的所有版本和安装选项。  

本文档将持续更新。 发现需要解决的严重问题，以及有可用的新的解决方法和修补程序时，它们将被添加到此文档中。  
  
## <a name="storage-spaces-direct"></a>存储空间直通
[comment]: # (ID：未知；提交者：stevenek；状态：已签收)  
Windows Server 版本 1709 中不包含存储空间直通。 如果你在运行 Windows Server 版本 1709 的服务器上调用 *Enable-ClusterStorageSpacesDirect* 或其别名 *Enable-ClusterS2D*，则你会收到错误以及“不支持请求的操作”消息。

另外，它也不受支持，因此无法将运行 Windows Server 版本 1709 的服务器引入到 Windows Server 2016 存储空间直通部署中。

Windows Server 版本模型将提供新的选项，从而与类似版本保持一致，并且将为用于 [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) 和 [Office 365 专业增强版](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US)的模型提供维护。 半年频道版本为希望进行快节奏转变的客户提供了新功能，并且每年发布两次新版本，分别在春季和秋季。

Windows Server 半年频道注重于可从更快的创新中受益的容器和应用方案，有关更多信息，请参阅[此博客](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update)。 寻求存储空间直通等基础结构角色的客户应该使用长期服务频道版本，例如 Windows Server 2016（现已推出）和 [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview)（今年晚些时候推出）。 我们正在致力于为超融合基础设施构建最佳平台，并会根据客户的反馈继续开发新的功能和改进现有功能。 

存储空间直通已引入到了 Windows Server 2016 中，并且是我们的超融合平台的基础。 我们对客户积极采用 Microsoft 超融合平台一直都感到非常兴奋，并将致力于为客户提供服务。

我们一直在听取客户的反馈并努力为我们的超融合平台提供[下一组创新](https://blogs.technet.microsoft.com/windowsserver/2017/09/07/sneak-peek-2-windows-server-version-1709-hyper-converged-infrastructure/)。 如今，[Windows 预览体验成员](https://insider.windows.com/for-business/)版本中提供了这些功能，我们希望你试用它们并共享你的反馈。 对于寻求验证过的超融合解决方案的客户，我们建议加入 [Windows Server 软件定义](http://microsoft.com/wssd)计划。
