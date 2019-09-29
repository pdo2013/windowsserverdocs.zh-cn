---
title: 设置更新同步
description: Windows Server Update Service （WSUS）主题-如何设置和配置更新同步
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4559016388f9b0d765c8e4d76f76fa7ef0a7f0f0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361597"
---
# <a name="setting-up-update-synchronizations"></a>设置更新同步

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在同步期间，WSUS 服务器将从更新源下载更新（更新元数据和文件）。 它还会下载新的产品分类和类别（如果有）。 当 WSUS 服务器首次同步时，它将下载你在配置同步选项时指定的所有更新。 第一次同步后，WSUS 服务器仅下载更新源中的更新，以及现有更新的元数据中的修订和更新过期。

当 WSUS 服务器首次下载更新时，可能需要较长时间。 如果要设置多台 WSUS 服务器，可以通过下载一个 WSUS 服务器上的所有更新，然后将更新复制到其他 WSUS 服务器的内容目录中，加速到一定范围的过程。

可以将内容从一个 WSUS 服务器的内容目录复制到另一个 WSUS 服务器的内容目录。 内容目录的位置是在运行 WSUS 后安装过程时指定的。 可以使用 wsusutil.exe 工具将更新元数据从一个 WSUS 服务器导出到一个文件中。 然后，可以将该文件导入到其他 WSUS 服务器。

## <a name="setting-up-update-synchronizations"></a>设置更新同步
"**选项**" 页是 Wsus 管理控制台中的中心访问点，用于自定义 wsus 服务器同步更新的方式。 你可以指定自动同步哪些更新、服务器获取更新的位置、连接设置和同步计划。 你还可以使用 "**选项**" 页中的配置向导随时配置或重新配置 WSUS 服务器。

### <a name="synchronizing-update-by-product-and-classification"></a>按产品和分类同步更新
WSUS 服务器根据你指定的产品或产品系列（例如，Windows 或 Windows Server 2008 Datacenter edition）和分类（例如，关键更新或安全更新）来下载更新。 第一次同步时，WSUS 服务器将下载您指定的类别中的所有可用更新。 在后续同步中，WSUS 服务器仅下载最新的更新（或对 WSUS 服务器上已有的更新的更新）。

可以在 "**产品和分类**" 下的 "**选项**" 页上指定更新产品和分类。 产品在层次结构中列出，按产品系列分组。 如果选择 "Windows"，则会自动选择属于该产品层次结构的每个产品。 通过选中 "父项" 复选框，可以选择其下的所有项以及所有将来的版本。 选中子复选框不会选中父复选框。 "产品" 的默认设置为 "所有 Windows 产品"，分类的默认设置为 "关键" 和 "安全更新"。

如果 WSUS 服务器在副本模式下运行，则将无法执行此任务。 有关副本模式的详细信息，请参阅[运行 WSUS 副本模式](running-wsus-replica-mode.md)和 [Step 1：准备 WSUS 部署 @ no__t-0。

##### <a name="to-specify-update-products-and-classifications-for-synchronization"></a>为同步指定更新产品和分类

1.  在 WSUS 管理控制台中，单击 "**选项**" 节点。

2.  单击 "**产品和分类**"，然后单击 "**产品**" 选项卡。

3.  选中要用 WSUS 更新的产品或产品系列的复选框，然后单击 **"确定"** 。

4.  在 "**分类**" 选项卡上，选中你希望 WSUS 服务器同步的更新分类的复选框，然后单击 **"确定"** 。

> [!NOTE]
> 可以采用相同的方式删除产品或分类。 WSUS 服务器将停止为已清除的产品同步新更新。 但是，在清除之前为这些产品同步的更新将保留在 WSUS 服务器上，并将列为 "可用"。
> 
> 要删除这些产品，请拒绝更新，如[更新操作](updates-operations.md)中所述，然后使用["服务器清理向导](the-server-cleanup-wizard.md)" 删除它们。

### <a name="synchronizing-updates-by-language"></a>按语言同步更新
WSUS 服务器将根据你指定的语言下载更新。 你可以同步其可用的所有语言的更新，也可以指定一小部分语言。 如果你有 WSUS 服务器的层次结构，并且需要下载不同语言的更新，请确保已在上游服务器上指定了所有必需的语言。 在下游服务器上，可以指定在上游服务器上指定的语言子集。

### <a name="synchronizing-updates-from-the-microsoft-update-catalog"></a>正在从 Microsoft 更新目录同步更新
有关从 Microsoft 更新目录站点同步更新的详细信息，请参阅：[WSUS 和目录站点](wsus-and-the-catalog-site.md)。

## <a name="configuring-proxy-server-settings"></a>配置代理服务器设置
你可以将 WSUS 服务器配置为在与上游服务器或 Microsoft 更新同步期间使用代理服务器。 仅当 WSUS 服务器运行同步时，此设置才适用。 默认情况下，WSUS 服务器将尝试直接连接到上游服务器或 Microsoft 更新。

#### <a name="to-specify-a-proxy-server-for-synchronization"></a>指定代理服务器进行同步

1.  在 WSUS 管理控制台中，单击 "**选项**"，然后单击 "**更新源和代理服务器**"。

2.  在 "**代理服务器**" 选项卡上，选中 "**同步时使用代理服务器**" 复选框，然后键入代理服务器的服务器名称和端口号。

    > [!NOTE]
    > 使用代理服务器配置为使用的相同端口号配置 WSUS。

    -   如果要使用特定用户凭据连接到代理服务器，请选中 "**使用用户凭据连接到代理服务器"** 复选框，然后在对应的框中输入用户的用户名、域和密码。

    -   如果要为连接到代理服务器的用户启用基本身份验证，请选中 "**允许基本身份验证（以明文形式发送密码）** " 复选框。

3.  单击 **“确定”** 。

    > [!NOTE]
    > 由于 WSUS 会启动其所有网络流量，因此不需要在直接连接到 Microsoft update 的 WSUS 服务器上配置 Windows 防火墙。

## <a name="configuring-the-update-source"></a>配置更新源
更新源是 WSUS 服务器从中获取更新和更新元数据的位置。 你可以指定更新源应为 Microsoft 更新或其他 WSUS 服务器（充当更新源的 WSUS 服务器是上游服务器，并且你的服务器是下游服务器）。

用于自定义 WSUS 服务器如何与更新源同步的选项包括：

-   可以指定用于同步的自定义端口。 有关配置端口的信息，请参阅 [Step 3：在 WSUS 部署指南中配置 WSUS @ no__t。

-   可以使用安全套接字层（SSL）来保护 WSUS 服务器之间更新信息的同步。 有关使用 SSL 的详细信息，请参阅 "3.5" 部分。 安全套接字层协议 "为 @no__t 的安全 WSUS-0Step 3：在 WSUS 部署指南中配置 WSUS @ no__t。

## <a name="synchronizing-manually-or-automatically"></a>手动或自动同步
你可以手动同步 WSUS 服务器或指定它自动同步的时间。

#### <a name="to-manually-synchronize-the-wsus-server"></a>手动同步 WSUS 服务器

1.  在 WSUS 管理控制台中，单击 "**选项**"，然后单击 "**同步计划**"。

2.  单击 "**手动同步**"，然后单击 **"确定"** 。

#### <a name="to-set-up-an-automatic-synchronization-schedule"></a>设置自动同步计划

1.  在 WSUS 管理控制台中，单击 "**选项**"，然后单击 "**同步计划**"。

2.  单击 "**自动同步**"。

3.  对于 "**第一次同步**"，请选择每天要开始同步的时间。

4.  对于 "**每天同步**次数"，请选择每天要执行的同步次数。 例如，如果想要从凌晨3:00 开始进行四次同步，则同步将在上午3:00、9:00 A.M.、下午 3:00 9:00 和下午进行。 每天。 （请注意，会将随机时间偏移量添加到计划的同步时间，以将服务器连接的空间排除在 Microsoft 更新。）

5.  单击 **“确定”** 。

#### <a name="to-synchronize-your-wsus-server-immediately"></a>立即同步 WSUS 服务器

1.  在 WSUS 管理控制台中，选择 "顶层服务器" 节点。

2.  在**概述**窗格中的 "**同步状态**" 下，单击 "**立即同步**"。

> [!NOTE]
> 同步由下游服务器启动。
