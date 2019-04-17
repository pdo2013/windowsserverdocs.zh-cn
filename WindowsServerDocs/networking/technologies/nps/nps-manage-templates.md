---
title: 管理模板 NPS
description: 本主题提供有关如何创建、应用、导出和导入 NPS 模板网络策略服务器，在 Windows Server 2016 中的说明进行操作。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a7f4c50d87c155c1adcd445eae8df23aab7730b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="manage-nps-templates"></a>管理模板 NPS

>适用于：Windows Server（半年通道），Windows Server 2016

你可以使用网络策略服务器 \(NPS\) 模板创建配置元素，如远程身份验证拨入用户服务 \(RADIUS\) 客户端或共享的密码，可以在当地 NPS 服务器和使用其他 NPS 服务器上的导出重复使用。 

管理模板提供在其中可以创建、修改、删除、复制，并查看 NPS 模板使用 NPS 控制台节点。 NPS 模板旨在减少的时间和配置 NPS 一个或多个服务器上所花费的费用。

以下 NPS 模板类型是可供配置中管理模板。

- **共享机密**。 此模板类型就可以让你指定共享的密码可重复使用（通过选择在相应的位置中 NPS 控制台模板）配置 RADIUS 客户端和服务器时。 

- **RADIUS 客户端**。 借助此类模板它使您可以配置 RADIUS 客户端设置，你可以通过在相应的位置中 NPS 控制台选择模板重复使用。

- **远程 RADIUS 服务器**。 此模板使您可以配置，你可以选择在 NPS 主机的相应位置的模板重用远程 RADIUS 服务器设置。 

- **IP Filters**。 此模板就可以让你创建 Internet 协议第四版 (IPv4) 和 Internet 协议版本 6 \(IPv6\) 筛选器，你可以重新使用该值 \（通过在相应的位置中 NPS console\ 选择模板）时，您将配置网络策略。

## <a name="create-an-nps-template"></a>创建 NPS 模板

配置模板是不同于直接配置 NPS 服务器。 创建模板不会影响 NPS 服务器的功能。 它是只有你选择在 NPS 主机的相应位置的模板和应用模板模板影响 NPS 服务器功能。 

例如，如果你在下 NPS 控制台配置 RADIUS 客户端**RADIUS 客户端和服务器**，你改变 NPS server 配置，并且在配置 NPS 与你的网络访问权限服务器之一进行通信的一步。 \ (配置网络的访问权限服务器下一步是与 NPS 进行通信的 \(NAS\)。\) 

但是，如果您将配置新**RADIUS 客户端**中 NPS 控制台下模板**管理模板**而不是创建新 RADIUS 客户端下的**RADIUS 客户端和服务器**、你已创建模板，但没有尚未改变 NPS 服务器功能。 若要更改 NPS 服务器的功能，必须从正确的位置中 NPS 控制台应用模板。

下面的过程中提供有关如何创建新的模板说明进行操作。

在会员**管理员**，或等效的最低要求完成此过程。

### <a name="to-create-an-nps-template"></a>若要创建 NPS 模板


1. 在 NPS 服务器，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 打开 NPS 主机。 

2. 在 NPS 控制台中，展开**模板管理**，如模板类型，请右键单击**RADIUS 客户端**，然后单击**新建**。

3. 打开一个新的模板属性对话框，你可以使用要配置你模板。

## <a name="apply-an-nps-template"></a>应用 NPS 模板

你可以使用你创建一个模板**模板管理**通过导航到 NPS 主机中的某个位置，你可以将应用模板。 例如，如果你想要共享机密模板时应遵守 RADIUS 客户端配置，你可以使用下面的过程。

在会员**管理员**，或等效的最低要求完成此过程。

### <a name="to-apply-an-nps-template"></a>若要将 NPS 模板应用

1. 在 NPS 服务器，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 打开 NPS 主机。

2. 在 NPS 控制台中，展开**RADIUS 客户端和服务器**，然后展开**RADIUS 客户端**。

3.在**RADIUS 客户端**，在详细信息窗格中，右键单击你想要将 NPS 模板中，应用，然后单击 RADIUS 客户端**属性**。

4. 在属性对话框中为 RADIUS 客户端，在**选择现有的共享机密模板**，选择你想要将应用从列表中提供的模板模板。

## <a name="export-or-import-nps-templates"></a>导出或导入 NPS 模板

你可以将导出模板其他 NPS 在服务器上，使用或可以导入到模板**模板管理**在本地计算机上使用。 

在会员**管理员**，或等效的最低要求完成此过程。

### <a name="to-export-or-import-nps-templates"></a>若要导入 NPS 模板或导出

1. 如果要导出 NPS 模板，在 NPS 控制台中，请右键单击**管理模板**，然后单击**导出到一个文件的模板**。

2. 若要导入 NPS 模板，在 NPS 控制台中，右键单击**管理模板**，然后单击**从计算机中导入模板**或**从一个文件导入模板**。


