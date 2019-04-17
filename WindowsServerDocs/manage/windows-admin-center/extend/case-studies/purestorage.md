---
title: Windows Admin Center SDK 案例研究-纯存储
description: Windows Admin Center SDK 案例研究-纯存储
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: ebeec824f802f020d0ece17524ba43b1baeba893
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2019
ms.locfileid: "8995301"
---
# 纯存储扩展

## 提供有关 Windows Admin Center 的端到端阵列管理 

[纯存储](https://www.purestorage.com/)提供了企业版提供以数据为中心的体系结构，可加速的竞争优势业务发展的全闪存数据存储解决方案。  纯是适用于 Microsoft Windows Server，认证的 Microsoft 金牌合作伙伴和开发技术集成的密钥的 Microsoft 解决方案，如 Azure、 HYPER-V、 SQL Server、 System Center、 Windows PowerShell 和 Windows SMB。 纯最近发布扩展支持最新版本的 Windows Admin Center 提供单窗格概况纯 FlashArray 产品的技术的预览。  此扩展，用户能够从一种工具进行监视任务、 查看实时性能指标和管理存储卷和启动器。

在早期，当 Windows Admin Center 被称为"Project Honolulu"，纯看到能够为客户提供的值，并且合作伙伴能够管理多个纯存储 FlashArrays 单一虚拟管理平台，Windows Admin Center 提供。

纯启动研究与"Project Honolulu"的用例时它们立即意识到提供统一的管理体验 Windows Admin Center 和 FlashArray 之间的可能性。 纯密切合作与 Windows Admin Center 工程团队进行，帮助定义的功能的实现详细信息。 纯也是能够在 Windows Admin Center 的早期阶段提供反馈，并使 Microsoft 团队的贡献。 

![纯存储扩展](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"我们已经集成模拟我们 FlashArray web 界面，可以直接从 Windows Admin Center 内的管理功能集。 客户和合作伙伴将受益于单一的与无需使用两种不同的管理工具。 除了单点管理权益客户将能够上下文管理 Windows 服务器连接到 FlashArray。"</cite>
>
> -Barkz、 技术主管 Microsoft 解决方案和集成，纯存储

纯的存储解决方案扩展中包含的功能包括：
- 连接到多个 FlashArrays。
- 查看 FlashArray 详细信息，包括 IOPs、 带宽、 延迟、 数据减少和空间管理。 这些是你获得 FlashArray 管理 GUI 的所有相同详细信息。
- 查看用于启用 Windows Server 主机和群集共享卷 (Csv) 共享的卷的访问权限配置的主机组。
- 查看主机-所有连接信息都可以包括主机名，iSCSI 限定名称 (Iqn) 和全球通用名称 （全球通用名称）。
- 管理卷 — 这包括创建和销毁卷的能力。 后销毁卷将放置在已销毁项目存储段，你将从主 FlashArray 管理 GUI 需要 Eradicate。
- 管理启动器 — 这是一个最感兴趣的功能向由 Windows Admin Center 部署的各个服务器提供上下文。 多路径 IO (MPIO) 是否安装/配置和创建/安装新卷，你可以查看已连接的磁盘 （卷） 到单独的 Windows 服务器，检查。

已创建显示所有纯的存储解决方案扩展提供的功能的[视频演示](https://youtu.be/IFAeCAd6V2g)。 

以下屏幕截图演示了查看哪些磁盘 （卷） 连接到特定的 Windows Server 主机。 除了查看连接详细信息，我们会检查是否配置多路径 IO。

![纯存储扩展](../../media/extend-case-study-purestorage/purestorage-2.png)

除了查看磁盘，可以创建新的卷的大小和立即装载到主机而无需使用 Windows 磁盘管理工具。

![纯存储扩展](../../media/extend-case-study-purestorage/purestorage-3.png)

由于释放我们 Technical Preview，收集到目前为止的客户反馈得到了非常积极和还提供了我们要添加在将来版本的不同功能的见解。 

其他资源：
- [纯存储扩展公告博客文章](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)播客
