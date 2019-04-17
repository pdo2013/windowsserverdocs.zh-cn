---
title: NPS 模板
description: 本主题提供 Windows Server 2016 中的网络策略 Server 模板的概述。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2835959b8c076ef7b6aeb1fca31a62717ef95037
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="nps-templates"></a>NPS 模板

>适用于：Windows Server（半年通道），Windows Server 2016

网络策略服务器 \(NPS\) 模板允许您创建配置元素，如远程身份验证拨入用户服务 \(RADIUS\) 客户端或共享的密码，可以在当地 NPS 服务器和使用其他 NPS 服务器上的导出重复使用。

NPS 模板旨在减少的时间和配置 NPS 一个或多个服务器上所花费的费用。 以下 NPS 模板类型是可供配置模板管理中：

- 共享的机密
- RADIUS 客户端
- 远程 RADIUS 服务器
- IP 筛选器
- 更新服务器组

配置模板是不同于直接配置 NPS 服务器。 创建模板不会影响 NPS 服务器的功能。 它是只有当你选择模板中 NPS 控制台模板影响 NPS server 功能适当位置。 

例如，如果你在 NPS RADIUS 客户端和服务器下控制台配置 RADIUS 客户端，您改变 NPS 服务器配置并花配置 NPS 与你的网络访问权限服务器 \(NAS's\) 之一进行通信的一步。 \ (下一步是配置 NAS 与 NPS。通信 \) 但是，如果你在下 NPS 控制台上配置了新 RADIUS 客户端模板**模板管理**而不是创建新 RADIUS 客户端下的**RADIUS 客户端和服务器**、你已创建模板，但没有尚未改变 NPS 服务器功能。 若要更改 NPS 服务器功能，必须从正确的位置中 NPS 主机选择模板。

## <a name="creating-templates"></a>创建模板

若要创建模板，打开 NPS 控制台、右键单击模板类型，例如**IP Filters**，然后单击**新建**。 打开一个新的模板属性对话框，以便你可以将配置你模板。

## <a name="using-templates-locally"></a>使用模板本地

你可以使用你已创建中模板**模板管理**通过导航到 NPS 主机中的某个位置，可以应用模板。 例如，如果创建新的共享机密模板你想要在应用于 RADIUS 客户端配置，**RADIUS 客户端和服务器**和**RADIUS 客户端**，打开 RADIUS 客户端属性。 在**选择现有的共享机密模板**，选择你之前创建了从列表中提供的模板模板。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。
