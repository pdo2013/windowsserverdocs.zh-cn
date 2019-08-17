---
title: Windows 管理中心 SDK 案例研究-纯存储
description: Windows 管理中心 SDK 案例研究-纯存储
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 1/7/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: cfd9d31d388b9acb1a4a4fa40b3975b235a8634b
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546617"
---
# <a name="pure-storage-extension"></a>纯粹的存储扩展

## <a name="providing-end-to-end-array-management-for-windows-admin-center"></a>为 Windows 管理中心提供端到端阵列管理 

[纯粹的存储](https://www.purestorage.com/)提供企业的、所有闪存的数据存储解决方案, 可提供以数据为中心的体系结构, 从而加快你的业务的竞争优势。  纯粹的 Microsoft 金牌合作伙伴, 适用于 Microsoft Windows Server, 并且为 Azure、Hyper-v、SQL Server、System Center、Windows PowerShell 和 Windows SMB 等关键 Microsoft 解决方案开发技术集成。 刚发布了一项技术预览版, 它支持 Windows 管理中心中心的最新版本, 该版本提供了纯 FlashArray 产品的单窗格视图。  通过此扩展, 用户可以通过一个工具来执行监视任务、查看实时性能指标以及管理存储卷和发起方。

早于, 当 Windows 管理中心中心被称为 "项目 Honolulu" 时, 纯粹了解到能够为客户和合作伙伴提供从 Windows 管理中心提供的单个控制台管理多个纯存储 FlashArrays 的能力。

当使用 "Project Honolulu" 开始研究用例时, 它们会立即实现在 Windows 管理中心和 FlashArray 之间提供统一的管理体验。 与 Windows 管理中心工程团队纯粹密切合作, 这有助于定义功能的实现细节。 纯粹还能在 Windows 管理中心的初期阶段提供反馈, 并向 Microsoft 团队作出贡献。 

![纯粹的存储扩展](../../media/extend-case-study-purestorage/purestorage-1.png)

> <cite>"我们已集成了一个功能集, 用于模拟我们的 FlashArray web 界面, 以便能够在 Windows 管理中心内实现直接管理。我们的客户和合作伙伴将受益于单个窗格, 而不需要使用两个不同的管理工具。除单点管理权益外, 客户还可以根据上下文管理连接到 FlashArray 的 Windows 服务器。</cite>
>
> --Barkz, 技术总监 Microsoft 解决方案 & 集成, 纯存储

纯粹存储解决方案扩展中包含的功能包括:
- 正在连接到多个 FlashArrays。
- 查看 FlashArray 详细信息, 包括 IOPs、带宽、延迟、数据缩减和空间管理。 这些是从 FlashArray 管理 GUI 获取的所有相同的详细信息。
- 查看用于为 Windows Server 主机和群集共享卷 (Csv) 启用共享卷访问的已配置主机组。
- 查看主机-所有连接信息均可用, 包括主机名、iSCSI 限定名称 (主机 iqn) 和全球通用名称 (Wwn)。
- 管理卷—这包括创建和销毁卷的能力。 销毁卷后, 会将其放入 "已销毁的项目" 存储桶中, 需要从主 FlashArray 管理 GUI 中消除。
- 管理发起程序-这是一项最有趣的功能, 可向 Windows 管理中心部署所管理的各个服务器提供上下文。 你可以查看已连接的磁盘 (卷) 到各个 Windows 服务器, 检查是否已安装/配置多路径 IO (MPIO), 并创建/装载新卷。

已创建演示[视频](https://youtu.be/IFAeCAd6V2g), 其中显示了纯粹存储解决方案扩展提供的所有功能。 

以下屏幕截图说明了如何查看连接到特定 Windows Server 主机的磁盘 (卷)。 除了查看连接详细信息之外, 我们还会检查是否配置了多路径 IO。

![纯粹的存储扩展](../../media/extend-case-study-purestorage/purestorage-2.png)

除了查看磁盘以外, 还可以创建新卷并将其立即装入主机, 而无需使用 Windows 磁盘管理工具。

![纯粹的存储扩展](../../media/extend-case-study-purestorage/purestorage-3.png)

从我们的技术预览版开始, 到目前为止, 收集的客户反馈已非常积极, 而且还向我们介绍了在未来版本中添加的不同功能。 

其他资源:
- [纯粹的存储扩展通知博客文章](https://blog.purestorage.com/tech-preview-of-the-pure-storage-extension-for-windows-admin-center/)
- [PureReport](https://itunes.apple.com/podcast/windows-admin-center-extension-from-pure-storage/id1392639991?i=1000424316130&mt=2)播客
