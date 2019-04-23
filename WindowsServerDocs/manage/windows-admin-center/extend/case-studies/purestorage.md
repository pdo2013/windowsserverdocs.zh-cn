---
title: Windows Admin Center SDK 案例研究-纯粹的存储
description: Windows Admin Center SDK 案例研究-纯粹的存储
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 25018474fd22d05804ecc7faafbd633fbb4db269
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849918"
---
# <a name="pure-storage-extension"></a>纯存储扩展

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>提供 Windows Admin Center 的端到端数组管理 

[纯存储](https://www.purestorage.com/)提供企业，提供以数据为中心的体系结构，以提升你的业务的竞争优势的所有闪存数据存储解决方案。  纯代码是针对 Microsoft Windows Server 认证的 Microsoft 金牌合作伙伴和开发针对 Azure 等关键 Microsoft 解决方案的技术集成的 HYPER-V、 SQL Server、 System Center、 Windows PowerShell 和 Windows SMB。 纯最近宣布支持 Windows Admin Center 提供纯 FlashArray 产品的单一窗格视图的最新版本的扩展技术预览。  来自此扩展插件，使用户能够从一种工具执行监视任务、 查看实时性能指标，以及管理存储卷和发起程序。

在早期，当 Windows Admin Center 称为"项目 Honolulu"，纯代码看到能够为客户提供的值和合作伙伴从单一 Windows Admin Center 提供的控制台管理多个纯存储 FlashArrays 的能力。

纯开始研究使用"项目 Honolulu"用例时他们立即意识到提供 Windows Admin Center 和 FlashArray 之间的统一的管理体验的可能性。 纯密切协作，与 Windows Admin Center 工程团队，这可以帮助定义功能的实现详细信息。 纯也是能够在 Windows Admin Center 的早期阶段提供反馈，并为 Microsoft 团队做出贡献。 

![纯存储扩展](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"我们已集成模仿我们 FlashArray web 界面，可启用从 Windows Admin Center 中的直接管理功能集。我们的客户和合作伙伴将受益于单一管理与无需使用两个不同的管理工具的平台。除了单点管理优势的客户将能够根据上下文管理连接到 FlashArray 的 Windows 服务器。"</cite>
>
> -Barkz、 技术总监 Microsoft 解决方案和集成，纯粹的存储

在纯存储解决方案扩展中包含的功能包括：
- 连接到多个 FlashArrays。
- 查看 FlashArray 详细信息，包括 IOPs、 带宽、 延迟、 减少数据和空间管理。 这些是你从 FlashArray 管理 GUI 中获取所有相同详细信息。
- 查看用于为 Windows Server 主机和群集共享卷 (Csv) 启用共享的卷访问权限的配置的主机组。
- 查看主机-的所有连接信息可包括主机名，iSCSI 限定名称 (Iqn) 和全球通用名称 (Wwn)。
- 管理卷-这包括能够创建和销毁的卷。 卷被销毁后它将要被放置在销毁项存储桶中，您需要 Eradicate 从主 FlashArray 管理 GUI。
- 管理发起程序 — 这是最有趣的功能提供了上下文到由 Windows Admin Center 部署的单个服务器之一。 如果多路径 IO (MPIO) 是已安装/配置和创建/装载新卷，您可以查看对单独的 Windows 服务器，检查已连接的磁盘 （卷）。

一个[演示视频](https://youtu.be/IFAeCAd6V2g)已创建，显示所有纯存储解决方案扩展提供的功能。 

以下屏幕截图说明如何查看哪些磁盘 （卷） 连接到特定的 Windows Server 主机。 除了查看连接详细信息，我们检查是否已配置多路径 IO。

![纯存储扩展](../../media/extend-case-study-purestorage/purestorage-2.png)

除了查看磁盘，可以创建新卷，并立即而无需使用 Windows 磁盘管理工具安装到主机中。

![纯存储扩展](../../media/extend-case-study-purestorage/purestorage-3.png)

由于发布我们 Technical Preview 中，到目前为止收集的客户反馈一直非常积极，还为我们提供深入了解不同的功能来添加在将来的版本。 

更多资源：
- [纯存储扩展公告博客文章](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/us/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)播客
