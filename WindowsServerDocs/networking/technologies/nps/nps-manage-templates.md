---
title: 管理 NPS 模板
description: 本主题将说明了如何创建、 应用、 导出和导入 Windows Server 2016 中的网络策略服务器的 NPS 模板。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 170a0e86f7dcca77c6efe841318b522554f8e78e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845128"
---
# <a name="manage-nps-templates"></a>管理 NPS 模板

>适用于：Windows 服务器 （半年频道），Windows Server 2016

可以使用网络策略服务器\(NPS\)模板来创建配置元素，如远程身份验证拨入用户服务\(RADIUS\)客户端或共享的机密，可以重复使用的本地NPS 和其他 NPSs 上使用的导出。 

模板管理提供了在 NPS 控制台，其中您可以创建、 修改、 删除、 复制时，和查看使用 NPS 模板中的节点。 NPS 模板旨在减少的时间和一个或多个服务器上的 NPS 配置所需的成本。

以下 NPS 模板类型是可用于在模板管理的配置。

- **共享机密**。 此模板类型使你可以指定可以重复使用 （通过在相应的位置在 NPS 控制台中选择该模板） 的共享的机密配置 RADIUS 客户端和服务器时。 

- **RADIUS 客户端**。 此模板类型使你可以配置通过在相应的位置在 NPS 控制台中选择模板，可以重用的 RADIUS 客户端设置。

- **远程 RADIUS 服务器**。 此模板可使你可以配置通过在相应的位置在 NPS 控制台中选择模板，可以重用的远程 RADIUS 服务器设置。 

- **IP 筛选器**。 此模板使你可以创建 Internet 协议版本 4 (IPv4) 和 Internet 协议版本 6 \(IPv6\)可以重复使用的筛选器\(在 NPS 中的相应位置选择模板控制台\)时配置网络策略。

## <a name="create-an-nps-template"></a>创建 NPS 模板

配置模板是不同于直接配置 NPS。 创建模板不会影响 NPS 的功能。 它是仅当在 NPS 控制台中的相应位置中选择的模板和应用模板，模板会影响 NPS 功能。 

例如，如果在 NPS 控制台中下配置的 RADIUS 客户端**RADIUS 客户端和服务器**，更改将 NPS 配置并执行一个步骤中配置 NPS 与其中一个网络访问服务器进行通信。 \(下一步是配置网络访问服务器\(NAS\)与 NPS 进行通信。\) 

但是，如果你配置一个新**RADIUS 客户端**模板在 NPS 控制台中下**模板管理**而不是创建新的 RADIUS 客户端下**RADIUS 客户端和服务器**，创建一个模板，但不是具有尚未更改 NPS 功能。 若要更改 NPS 功能，必须从 NPS 控制台中的正确位置应用该模板。

以下过程提供了有关如何创建新的模板的说明。

中的成员身份**管理员**，或等效身份是完成此过程所需的最小。

### <a name="to-create-an-nps-template"></a>若要创建 NPS 模板


1. 在 NPS 中，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 将打开 NPS 控制台。 

2. 在 NPS 控制台中，展开**模板管理**，右键单击一种模板类型，例如**RADIUS 客户端**，然后单击**新建**。

3. 此时将打开新的模板属性对话框，可用来配置你的模板。

## <a name="apply-an-nps-template"></a>NPS 模板应用

可以使用模板中创建**模板管理**通过导航到在 NPS 控制台中可以应用模板的位置。 例如，如果你想要应用于 RADIUS 客户端配置的共享机密模板，您可以使用以下过程。

中的成员身份**管理员**，或等效身份是完成此过程所需的最小。

### <a name="to-apply-an-nps-template"></a>若要将 NPS 模板应用

1. 在 NPS 中，在服务器管理器中，单击**工具**，然后单击**网络策略服务器**。 将打开 NPS 控制台。

2. 在 NPS 控制台中，展开**RADIUS 客户端和服务器**，然后展开**RADIUS 客户端**。

3.在**RADIUS 客户端**，在详细信息窗格中，右键单击你想要将 NPS 模板中，应用，然后单击的 RADIUS 客户端**属性**。

4. 在属性对话框框中，为 RADIUS 客户端，在**选择一个现有的共享机密模板**，选择你想要从模板列表中应用的模板。

## <a name="export-or-import-nps-templates"></a>导出或导入 NPS 模板

可以导出模板用于其他 NPSs，或者可以导入到的模板**模板管理**在本地计算机上使用。 

中的成员身份**管理员**，或等效身份是完成此过程所需的最小。

### <a name="to-export-or-import-nps-templates"></a>若要导出或导入 NPS 模板

1. 若要导出 NPS 模板，在 NPS 控制台中，右键单击**模板管理**，然后单击**将模板导出到文件**。

2. 若要导入 NPS 模板，在 NPS 控制台中，右键单击**模板管理**，然后单击**从计算机导入模板**或**从文件导入模板**。


