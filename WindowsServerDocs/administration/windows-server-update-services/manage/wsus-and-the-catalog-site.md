---
title: WSUS 和目录站点
description: Windows Server Update Service (WSUS) 主题-如何导入 WSUS 的修补程序，通过访问 Microsoft Update 目录站点
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 298df36b856cba97ec19126f77456785e5eb6f50
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810927"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS 和目录站点

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

目录站点是可从中导入的修补程序和硬件驱动程序的 Microsoft 位置。

## <a name="the-microsoft-update-catalog-site"></a>Microsoft Update 目录站点
为了修补程序导入 WSUS 时，你必须从 WSUS 计算机访问 Microsoft 更新目录网站。 指示为 WSUS 服务器已安装，在 WSUS 管理控制台的任何计算机可以用于从目录站点导入修补程序。 您必须将登录到计算机以管理员身份导入修补程序。

#### <a name="to-access-the-microsoft-update-catalog-site"></a>若要访问 Microsoft 更新目录站点

1.  在 WSUS 管理控制台中，选择顶部的服务器节点或**更新**，然后在**操作**窗格中，单击**导入更新**。 在 Microsoft Update 目录网站将打开一个浏览器窗口。

2.  若要访问此站点中的更新，必须安装 Microsoft 更新目录 activeX 控件。

3.  您可以浏览此站点的 Windows 修补程序和硬件驱动程序。 在您找到所的需，将它们添加到购物篮中。

4.  完成浏览后，转到购物篮，并单击导入到导入你的更新。 若要下载更新，而不将其导入，请清除**直接导入 Windows Server Update Services**复选框。

从 Microsoft 更新目录站点导入的已批准的更新下载下一次 WSUS 服务器同步。 它们不会下载从 Microsoft 更新目录站点导入的时间。

请注意，您必须访问 Microsoft 更新目录站点但 WSUS 控制台，以确保 WSUS 兼容的格式中，导入更新。 如果手动访问 Microsoft 更新目录网站，下载任何更新未导入到 WSUS 服务器，但改为下载个人身份 *。MSU 文件。 WSUS 当前不具有受支持的机制，用于导入中的文件\*。MSU 格式。

如果您运行的服务器清理向导，将设置为未批准或拒绝的更新从 Microsoft 更新目录中导入可能从 WSUS 服务器中删除。 如果删除，它们可以是从 Microsoft 更新目录重新导入。

> [!NOTE]
> 您可以删除从 Microsoft 更新目录中导入设置为未批准或已拒绝，通过运行的 WSUS 服务器清理向导的更新。 你可以重新导入已从 WSUS 系统通过 Microsoft 更新目录中以前删除的更新。

## <a name="restricting-access-to-hotfixes"></a>限制对修补程序的访问
WSUS 管理员可能会考虑将访问限制为它们已从 Microsoft 更新目录网站下载的修补程序。 为了使此限制，请执行以下步骤：

#### <a name="to-restrict-access-to-hotfixes"></a>若要限制对修补程序的访问

1.  启用 IIS 内容的虚拟根目录上的 Windows 身份验证。

    -   启动 IIS 管理器。

    -   导航到 WSUS 管理网站下的内容节点。

    -   上**内容主页**窗格中，双击中**身份验证**选项。

    -   选择**匿名身份验证**然后单击**禁用**中**操作**右侧窗格中的。

    -   选择**Windows 身份验证**然后单击**启用**中**操作**右侧窗格中的。

2.  创建需要该修补程序的计算机的 WSUS 目标组并将其添加到组。 有关计算机和组的详细信息，请参阅[管理 WSUS 客户端计算机和 WSUS 计算机组](managing-wsus-client-computers-and-wsus-computer-groups.md)在本指南中和部分[3.3。配置 WSUS 计算机组](../deploy/2-configure-wsus.md#23-configure-wsus-computer-groups)的步骤 3:配置 WSUS，WSUS 部署指南中。

3.  下载该修补程序的文件。

4.  设置这些文件的权限，以便只能机帐户的这些计算机可以读取它们。 您还需要允许网络服务帐户对文件的完全访问。

5.  批准在步骤 2 中创建的 WSUS 目标组的修补程序。

## <a name="importing-updates-in-different-languages"></a>导入不同语言的更新
Microsoft Update 目录网站包括支持多种语言的更新。 它非常**重要**以匹配由 WSUS 服务器与受这些更新的语言支持的语言。 如果 WSUS 服务器不支持为更新中包含的所有语言，更新将不会部署到客户端计算机。 同样，如果支持多种语言的更新已下载到 WSUS 服务器，但尚未部署到客户端计算机，并且管理员取消选择的语言之一包含更新，更新将不会部署到客户端。
