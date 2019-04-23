---
title: 设置更新同步
description: Windows Server Update Service (WSUS) 主题-如何设置和配置更新同步
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ddd5c395-451b-44a0-8e08-a05db26d2282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e381316372e68d2a43203b8fc90a243af5f40b02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869208"
---
# <a name="setting-up-update-synchronizations"></a>设置更新同步

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在同步期间，WSUS 服务器将从更新源下载的更新 （更新元数据和文件）。 它还下载新的产品分类和类别，如果有的话。 你的 WSUS 服务器同步时第一次，它将下载的所有更新的配置同步选项时指定。 第一个同步后，你的 WSUS 服务器仅下载更新从更新源，以及现有的更新和过期时间的元数据中的修订更新。

第一次的 WSUS 服务器下载更新可能需要长时间。 如果您设置了多个 WSUS 服务器，你可以加快该过程的在某种程度上通过下载一台 WSUS 服务器上的所有更新，然后将更新复制到其他 WSUS 服务器的内容目录。

可以将内容从一台 WSUS 服务器的内容目录复制到另一个。 在运行 WSUS post 安装过程时指定的内容目录的位置。 Wsusutil.exe 工具可用于从一台 WSUS 服务器的更新元数据导出到文件。 然后，可以将该文件导入其他 WSUS 服务器中。

## <a name="setting-up-update-synchronizations"></a>设置更新同步
**选项**页是在 WSUS 管理控制台中自定义你的 WSUS 服务器将更新的同步的中心访问点。 您可以指定哪些更新自动同步你的服务器位置获取更新、 连接设置和同步计划。 您还可以使用从配置向导**选项**页后，可以配置或随时重新配置你的 WSUS 服务器。

### <a name="synchronizing-update-by-product-and-classification"></a>同步更新按产品和分类
WSUS 服务器下载更新基于产品或产品系列 （例如，Windows 或 Windows Server 2008 中，数据中心版） 和你指定的分类 （例如，关键更新或安全更新）。 在首次同步 WSUS 服务器下载所有具有指定的类别，在可用的更新。 在后续同步中，在 WSUS 服务器下载仅最新的更新 （或对你的 WSUS 服务器上已经可用的更新的更改） 类别的您指定。

您可以在指定更新产品和分类**选项**页**产品和分类**。 层次结构中，按产品系列分组列出了产品。 如果选择 Windows，则自动选择归入该产品层次结构每个产品。 通过选择父复选框来选择在其下的所有项以及所有将来的版本。 选择子复选框将选择父复选框。 默认设置为产品是所有的 Windows 产品和分类的默认设置是关键和安全更新。

如果在副本模式下运行的 WSUS 服务器，你将不能执行此任务。 有关副本模式的详细信息，请参阅[运行 WSUS 副本模式](running-wsus-replica-mode.md)，和[步骤 1:为 WSUS 部署做好准备](../plan/plan-your-wsus-deployment.md)。

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>若要指定更新产品和分类进行同步

1.  在 WSUS 管理控制台中，单击**选项**节点。

2.  单击**产品和分类**，然后单击**产品**选项卡。

3.  选择的产品或产品系列你想要使用 WSUS，更新，然后单击复选框**确定**。

4.  上**分类**选项卡上，选中你想要同步，然后单击你的 WSUS 服务器的更新分类的复选框**确定**。

> [!NOTE]
> 可以相同方式删除产品或分类。 你的 WSUS 服务器将停止同步已清除的产品的新更新。 但是之前你清除了, 已同步对这些产品的更新将保留在你的 WSUS 服务器上，并且会列出为可用。
> 
> 若要删除的那些产品，拒绝更新，如中所述[更新操作](updates-operations.md)，然后使用[服务器清理向导](the-server-cleanup-wizard.md)将其删除。

### <a name="synchronizing-updates-by-language"></a>同步的语言的更新
你的 WSUS 服务器下载基于你指定的语言的更新。 在同步更新中的所有语言的可用，也可以指定语言的子集。 如果有层次结构的 WSUS 服务器，并且您需要下载不同语言的更新，请确保在上游服务器上指定了所有需要的语言。 在下游服务器可以指定上游服务器指定的语言的子集。

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>将更新从 Microsoft Update 目录同步
有关同步来自 Microsoft Update 目录网站的更新的详细信息，请参阅：[WSUS 和目录站点](wsus-and-the-catalog-site.md)。

### <a name="synchronizing-device-updates-by-inventory-inventory-based-synchronization"></a>同步设备更新的清单 （基于清单的同步）
某些产品类别和分类 （例如驱动程序） 包含非常大量的更新，因此不建议将同步到 WSUS 服务器这些整个类别。 执行此操作可能会导致性能问题和不间断的维护困难。 WSUS 库存系统从客户端设备收集非标识性信息，并使用该清单信息以从 Microsoft Update 检索刚好足够更新元数据。 此机制是大致等效于具有 WSUS 自动，搜索 Microsoft 更新目录导入仅适用于设备检测到更新管理的设备。

启用此清单功能是唯一支持的方法，以获取特定设备固件和基于模型的服务集未发布到 Microsoft 更新目录。

审核和批准就像任何其他更新，以这种方式同步更新和也遵循相同的自动批准规则，取代和过期和任何其他行为关联与传统的更新。

WSUS 执行服务器端筛选，当客户端请求某些驱动程序和固件更新，包括已由清单导入的更新。 因此，客户端计算机或设备都将收到的元数据驱动程序的 detectoid 和仅适用于实际附加到该设备的设备驱动程序更新。 此行为最大程度减少客户端扫描时间并减少客户端和 WSUS 服务器之间传输的数据。

> [!NOTE]
> 启用基于清单的同步后，WSUS 会保留在每个设备的基础; 上的设备清单仅摘要汇总 （消除重复的 Id 列表） 不断发送到上游 WSUS 服务器。 上游 WSUS 服务器不会收到有关哪些设备与相关联的计算机，也不 WSUS 层次结构中存在多少个实例的给定设备的信息。 一般情况下，此摘要汇总不能用于标识或计数由 WSUS 管理网络上的设备。

## <a name="configuring-proxy-server-settings"></a>配置代理服务器设置
可以配置你的 WSUS 服务器与上游服务器或 Microsoft Update 的同步期间使用代理服务器。 仅当你的 WSUS 服务器运行同步时，将应用此设置。 默认情况下你的 WSUS 服务器将尝试直接连接到上游服务器或 Microsoft Update。

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>若要指定代理服务器进行同步

1.  在 WSUS 管理控制台中，单击**选项**，然后单击**更新源和代理服务器**。

2.  上**代理服务器**选项卡上，选择**同步时使用代理服务器**复选框，然后键入服务器名称和端口号的代理服务器。

    > [!NOTE]
    > 使用代理服务器配置为相同的端口号配置 WSUS 使用。

    -   如果你想要连接到特定用户凭据的代理服务器，请选择**使用用户凭据来连接到代理服务器**复选框，，然后在对应的框中输入用户名、 域和用户的密码.

    -   如果你想要启用基本身份验证连接到代理服务器，选择的用户**允许基本身份验证 （密码以明文形式发送）** 复选框。

3.  单击 **“确定”**。

    > [!NOTE]
    > 由于 WSUS 启动所有其网络流量，没有必要在直接连接到 Microsoft 更新的 WSUS 服务器上配置 Windows 防火墙。

## <a name="configuring-the-update-source"></a>配置更新源
更新源是你的 WSUS 服务器从其获取其更新的位置和更新元数据。 您可以指定更新源应为 Microsoft 更新或另一台 WSUS 服务器 （充当更新源是上游服务器，并且你的服务器是下游服务器的 WSUS 服务器）。

用于自定义如何将你的 WSUS 服务器同步的更新源的选项包括：

-   可以指定自定义端口进行同步。 有关配置的端口的信息，请参阅[步骤 3:将 WSUS 配置](../deploy/2-configure-wsus.md)WSUS 部署指南中。

-   您可以使用安全套接字层 (SSL) 安全同步的 WSUS 服务器之间的更新信息。 有关使用 SSL 的详细信息，请参阅部分"3.5。 安全 WSUS 使用安全套接字层协议"的[步骤 3:将 WSUS 配置](../deploy/2-configure-wsus.md)WSUS 部署指南中。

## <a name="synchronizing-manually-or-automatically"></a>手动或自动同步
可以手动同步你的 WSUS 服务器，也可以指定为其自动同步的时间。

#### <a name="to-manually-synchronize-the-wsus-server"></a>若要手动同步 WSUS 服务器

1.  在 WSUS 管理控制台中，单击**选项**，然后单击**同步计划**。

2.  单击**手动同步**，然后单击**确定**。

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>若要设置自动同步计划

1.  在 WSUS 管理控制台中，单击**选项**，然后单击**同步计划**。

2.  单击**自动同步**。

3.  有关**第一次同步**，选择要开始每日同步的时间。

4.  有关**每天同步次数**，选择想要执行每日同步数。 例如，如果您想同步四次在凌晨 3:00 天开始，则同步会发生在凌晨 3:00、 上午 9:00，下午 3:00 和下午 9:00 每一天。 （请注意，随机的时间偏移量将添加到计划的同步时间以隔开服务器连接到 Microsoft 更新。）

5.  单击 **“确定”**。

#### <a name="to-synchronize-your-wsus-server-immediately"></a>若要立即同步你的 WSUS 服务器

1.  在 WSUS 管理控制台中，选择上的服务器节点。

2.  在中**概述**窗格下**同步状态**，单击**立即同步**。

> [!NOTE]
> 下游服务器启动同步。
