---
title: WSUS 和目录站点
description: Windows Server Update Service （WSUS）主题-如何通过访问 Microsoft 更新目录站点将修补程序导入到 WSUS
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c30fad5f38a1b3088c6b4d12ac92d8b8a15aee83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361453"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS 和目录站点

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

目录站点是您可以从中导入修补程序和硬件驱动程序的 Microsoft 位置。

## <a name="the-microsoft-update-catalog-site"></a>Microsoft 更新目录站点
若要将修补程序导入到 WSUS，必须从 WSUS 计算机访问 Microsoft 更新目录站点。 安装 WSUS 管理控制台的任何计算机（无论是否为 WSUS 服务器）都可用于从目录站点导入修补程序。 您必须以管理员身份登录计算机才能导入修补程序。

#### <a name="to-access-the-microsoft-update-catalog-site"></a>访问 Microsoft 更新目录站点

1.  在 WSUS 管理控制台中，选择顶层服务器节点或**更新**，并在 "**操作**" 窗格中单击 "**导入更新**"。 此时会在 Microsoft 更新目录网站上打开一个浏览器窗口。

2.  若要访问此站点上的更新，必须安装 Microsoft 更新 Catalog activeX 控件。

3.  可在此站点上浏览 Windows 修补程序和硬件驱动程序。 找到所需的文件后，将其添加到购物篮中。

4.  完成浏览后，请转到购物篮，并单击 "导入" 以导入更新。 若要下载更新而不导入，请清除 "**直接导入 Windows Server Update Services** " 复选框。

下次 WSUS 服务器同步时，将下载从 Microsoft 更新目录站点导入的已批准更新。 在从 Microsoft 更新目录站点导入时，它们不会下载。

请注意，你必须通过 WSUS 控制台访问 Microsoft 更新目录站点，以确保以与 WSUS 兼容的格式导入更新。 如果手动访问 Microsoft 更新目录网站，则下载的任何更新都不会导入到 WSUS 服务器中，而是作为单个 * 下载。MSU 文件。 WSUS 当前不具有用于在 @no__t 中导入文件的支持机制。MSU 格式。

如果运行服务器清理向导，则可能会从 WSUS 服务器中删除从设置为 "未批准" 或 "已拒绝" Microsoft 更新目录导入的更新。 如果删除了这些项，则可以从 Microsoft 更新目录重新导入它们。

> [!NOTE]
> 通过运行 WSUS 服务器清理向导，你可以删除从 Microsoft 更新目录导入的更新，这些更新将被设置为 "未批准" 或 "已拒绝"。 你可以重新导入以前通过 Microsoft 更新目录从 WSUS 系统中删除的更新。

## <a name="restricting-access-to-hotfixes"></a>限制对修补程序的访问
WSUS 管理员可能会考虑将访问权限限制为从 Microsoft 更新目录站点下载的修补程序。 为了做出此限制，请遵循以下步骤：

#### <a name="to-restrict-access-to-hotfixes"></a>限制对修补程序的访问

1.  在 IIS 内容 vroot 上启用 Windows 身份验证。

    -   启动 IIS 管理器。

    -   导航到 WSUS 管理网站下的内容节点。

    -   在 "**内容" 主页**窗格中，双击 "**身份验证**" 选项。

    -   选择 "**匿名身份验证**"，然后在右侧的 "**操作**" 窗格中单击 "**禁用**"。

    -   选择 " **Windows 身份验证**"，然后在右侧的 "**操作**" 窗格中单击 "**启用**"。

2.  为需要修补程序的计算机创建 WSUS 目标组，并将其添加到组中。 有关计算机和组的详细信息，请参阅本指南中的[管理 Wsus 客户端计算机和 wsus 计算机组](managing-wsus-client-computers-and-wsus-computer-groups.md)和 [3.3 部分。配置 WSUS 计算机组 @ no__t-0，步骤3：在 WSUS 部署指南中配置 WSUS。

3.  下载修补程序的文件。

4.  设置这些文件的权限，以便只有这些计算机的计算机帐户可以读取这些文件。 你还需要允许网络服务帐户对文件具有完全访问权限。

5.  批准在步骤2中创建的 WSUS 目标组的修补程序。

## <a name="importing-updates-in-different-languages"></a>导入不同语言的更新
Microsoft 更新目录网站包含支持多种语言的更新。 **必须**将 WSUS 服务器支持的语言与这些更新支持的语言相匹配。 如果 WSUS 服务器不支持更新中包含的所有语言，则不会将更新部署到客户端计算机。 同样，如果已将支持多种语言的更新下载到 WSUS 服务器，但尚未部署到客户端计算机，并且管理员取消选择包含更新的其中一种语言，则不会将更新部署到客户端。
