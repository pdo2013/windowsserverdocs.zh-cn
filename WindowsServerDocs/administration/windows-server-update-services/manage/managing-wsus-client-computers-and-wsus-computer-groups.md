---
title: 管理 WSUS 客户端计算机和 WSUS 计算机组
description: Windows Server Update Service （WSUS）主题-如何管理客户端计算机和组
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 454fa385dc9fb91218ad836d4ad34e92e9644dac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361626"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>管理 WSUS 客户端计算机和 WSUS 计算机组

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

在 WSUS 管理控制台中，"计算机" 节点是用于管理 WSUS 客户端计算机和设备的中心访问点。 在此节点下，您可以找到设置的不同组（以及默认组 "未分配的计算机"）。

## <a name="managing-client-computers"></a>管理客户端计算机
在 "**选项**" 下的 "**计算机**" 节点中选择一个计算机组将导致该组中的计算机显示在 "详细信息" 窗格中。 如果计算机被分配到多个组，它将出现在两个组的列表中。 如果在列表中选择一台计算机，则可以看到其属性，其中包括有关计算机的常规详细信息以及该计算机的更新状态，例如特定计算机的更新的安装或检测状态。 您可以按状态筛选给定计算机组下的计算机列表。 默认情况下，仅显示需要更新或安装失败的计算机;不过，你可以按任何状态筛选显示。 更改状态筛选器后，单击 "**刷新**"。

你还可以在 "计算机" 页上管理计算机组，其中包括创建组并向其分配计算机。 有关管理计算机组的详细信息，请参阅本指南下一部分的管理计算机组和 [1.5 部分。在步骤1中计划 WSUS 计算机组 @ no__t-0：准备 wsus 部署指南的 WSUS 部署。

> [!NOTE]
> 必须先将客户端计算机配置为联系 WSUS 服务器，然后才能从该服务器管理它们。 在您执行此任务之前，WSUS 服务器将无法识别您的客户端计算机，它们将不会显示在 "计算机" 页上的列表中。 有关设置客户端计算机的详细信息，请参阅 [1.5。在步骤1中计划 WSUS 计算机组 @ no__t-0：准备 WSUS 部署和步骤3：在 WSUS 部署指南中配置 WSUS。

## <a name="controlling-when-wsus-client-computers-install-updates"></a>控制 WSUS 客户端计算机安装更新的时间
可以通过两种方法来控制 WSUS 客户端计算机安装更新的时间：

-   按截止时间审批：安装更新时严格强制执行的截止时间

-   WSUS 组策略：组策略控制 Windows 更新代理扫描和安装更新的时间

    有关详细信息, 请参阅:[步骤 5：在 WSUS 部署指南中配置自动更新 @ no__t-0 的组策略设置。

## <a name="managing-computer-groups"></a>管理计算机组
WSUS 可让你将各组客户端计算机作为更新目标，从而确保特定计算机总是在最方便的时候获得适当的更新。 例如，如果同一部门（例如会计组）中的所有计算机具有特定配置，你可为该组建立一个计算机组，并确定它们的计算机需要哪些更新以及何时安装这些更新，然后使用 WSUS 报告评估团队更新。

始终将计算机分配给 "**所有计算机**" 组，并将其分配给 "**未分配的计算机**" 组，直到你将它们分配给其他组。 计算机可以属于多个组。

可以按层次结构建立计算机组（例如，在 Accounting 组下的 Payroll 组和 Accounts Payable 组）。 为较高的组批准的更新将自动部署到较低的组以及更高的组本身。 因此，如果您为会计组批准 Update1，更新将部署到会计组中的所有计算机、该工资组中的所有计算机以及 "应付帐款" 组中的所有计算机。

因为可将计算机分配给多个组，所以可为同一台计算机多次批准单一更新。 但是，更新仅被部署一次，且 WSUS 服务器将解决任何冲突。 若要继续执行上面的示例，如果将计算机分配给工资单和应付帐款组，同时为这两个组批准了 Update1，则它将仅部署一次。

可以使用“指向服务器端”或“指向客户端”这两种方法之一将计算机分配到计算机组。 通过服务器端目标，您可以将一台或多台客户端计算机手动移动到一个计算机组。 使用客户端目标设定，你可以在客户端计算机上使用组策略或编辑注册表设置，使那些计算机可以自动将其添加到之前创建的计算机组中。 可以同时编写脚本并将其部署到多台计算机。 你必须在 "**选项**" 页的 "**计算机**" 部分中选择两个选项之一，以指定将在 WSUS 服务器上使用的目标方法。

> [!NOTE]
> 如果 WSUS 服务器在副本模式下运行，则不能在该服务器上创建计算机组。 必须在作为 WSUS 服务器层次结构的根的 WSUS 服务器上创建副本服务器的客户端所需的所有计算机组。 有关副本模式的详细信息，请参阅[运行 WSUS 副本模式](running-wsus-replica-mode.md)和有关服务器端和客户端目标的详细信息，请参阅 [1.5 部分。在步骤1中计划 WSUS 计算机组 @ no__t-0：在 WSUS 部署指南中准备 WSUS 部署。


