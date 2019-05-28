---
title: 管理 WSUS 客户端计算机和 WSUS 计算机组
description: Windows Server Update Service (WSUS) 主题-如何管理客户端计算机和组
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 4ede63ab08d204c29555b28ae3a73795291c321c
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222494"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>管理 WSUS 客户端计算机和 WSUS 计算机组

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

计算机节点是用于管理 WSUS 客户端计算机和设备在 WSUS 管理控制台中的中心接入点。 在此节点可以找到已设置的不同组 （加上的默认组中，未分配的计算机）。

## <a name="managing-client-computers"></a>管理客户端计算机
选择一个中的计算机组**计算机**节点下的**选项**该组，以显示详细信息窗格中会导致计算机。 如果向计算机分配到多个组，则会在这两个组的列表中。 如果在列表中选择一台计算机，可以看到其属性，其中包括有关计算机的更新，例如安装的状态或特定计算机的更新的检测状态的常规详细信息。 您可以按状态筛选给定的计算机组下的计算机的列表。 默认值将显示唯一计算机的所需更新或其已安装失败;但是，可以显示筛选的任何状态。 单击**刷新**后更改状态筛选器。

此外可以管理计算机页面，其中包括创建组并向它们分配的计算机上的计算机组。 有关管理计算机组的详细信息，请参阅管理下一步的本指南中，部分和部分中的计算机组[1.5。计划 WSUS 计算机组](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups)在步骤 1 中：为你的 WSUS 部署指南的 WSUS 部署做好准备。

> [!NOTE]
> 必须先配置客户端计算机联系 WSUS 服务器，才能从该服务器管理。 执行此任务，直到你的 WSUS 服务器将无法识别客户端计算机和它们将不会显示在计算机页面上的列表。 有关设置客户端计算机的详细信息，请参阅[1.5。计划 WSUS 计算机组](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups)的步骤 1:为你的 WSUS 部署做好准备和步骤 3:配置 WSUS，WSUS 部署指南中。

## <a name="controlling-when-wsus-client-computers-install-updates"></a>控制 WSUS 客户端计算机时安装更新
WSUS 客户端计算机安装更新时，有两种方法可用于控制：

-   截止时间审批：安装更新时，严格强制实施截止时间

-   WSUS 组策略：Windows 更新代理扫描以及安装更新时，组策略控制

    有关详细信息，请参阅：[步骤 5：配置组策略设置为自动更新](../deploy/4-configure-group-policy-settings-for-automatic-updates.md)，WSUS 部署指南中。

## <a name="managing-computer-groups"></a>管理计算机组
WSUS 可让你将各组客户端计算机作为更新目标，从而确保特定计算机总是在最方便的时候获得适当的更新。 例如，如果同一部门（例如会计组）中的所有计算机具有特定配置，你可为该组建立一个计算机组，并确定它们的计算机需要哪些更新以及何时安装这些更新，然后使用 WSUS 报告评估团队更新。

始终将计算机分配到**的所有计算机**组，然后继续被分配给**未分配的计算机**组，直到将其分配给另一个组。 计算机可以属于多个组。

可以按层次结构建立计算机组（例如，在 Accounting 组下的 Payroll 组和 Accounts Payable 组）。 到位置较低，以及更高版本组本身，将自动部署为更高版本组批准的更新。 因此，如果你为 Accounting 组批准 Update1，更新将部署到 Accounting 组中的所有计算机、 Payroll 组中的所有计算机以及 Accounts Payable 组中的所有计算机。

因为可将计算机分配给多个组，所以可为同一台计算机多次批准单一更新。 但是，更新仅被部署一次，且 WSUS 服务器将解决任何冲突。 若要继续上述示例中，如果计算机 a 被分配到工资和 Accounts Payable 组，并为这两个组批准 Update1，它将被部署一次。

可以使用“指向服务器端”或“指向客户端”这两种方法之一将计算机分配到计算机组。 使用服务器端目标，则手动移动一个或多个客户端计算机到一台计算机组一次。 使用客户端目标，使用组策略或编辑客户端计算机上的注册表设置，以使那些计算机可以自动将其添加到以前创建的计算机组。 此过程可以编写脚本并立即部署到多台计算机。 必须指定目标所使用的方法将在 WSUS 服务器上通过选择上的两个选项之一**计算机**一部分**选项**页。

> [!NOTE]
> 如果 WSUS 服务器在副本模式下运行，则不能在该服务器上创建计算机组。 必须是 WSUS 服务器层次结构的根 WSUS 服务器上创建副本服务器的客户端所需的所有计算机组。 有关副本模式的详细信息，请参阅[运行 WSUS 副本模式](running-wsus-replica-mode.md); 有关服务器端和客户端目标设定的详细信息，请参阅部分[1.5。计划 WSUS 计算机组](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups)的步骤 1:为你的 WSUS 部署做好准备 WSUS 部署指南中。


