---
title: NPS 模板
description: 本主题概述了 Windows Server 2016 中的网络策略服务器模板。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0647dbf0f99a01e32ba68475b439501e2dbeebfe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823308"
---
# <a name="nps-templates"></a>NPS 模板

>适用于：Windows 服务器 （半年频道），Windows Server 2016

网络策略服务器\(NPS\)模板可用于创建配置元素，如远程身份验证拨入用户服务\(RADIUS\)客户端或共享的机密，可以重复使用的本地NPS 和其他 NPSs 上使用的导出。

NPS 模板旨在减少的时间和一个或多个服务器上的 NPS 配置所需的成本。 以下 NPS 模板类型是可用于在模板管理的配置：

- 共享的机密
- RADIUS 客户端
- 远程 RADIUS 服务器
- IP 筛选器
- 修正服务器组

配置模板是不同于直接配置 NPS。 创建模板不会影响 NPS 的功能。 它是仅当你选择的模板中，模板会影响 NPS 功能在 NPS 控制台中的适当位置。 

例如，如果在 NPS 控制台中下 RADIUS 客户端和服务器配置的 RADIUS 客户端，在您更改 NPS 配置并花中配置 NPS 与其中一个网络访问服务器进行通信的一步\(NAS 的\). \(下一步是配置 NAS 与 NPS 进行通信。\)但是，如果你将新的 RADIUS 客户端模板配置在 NPS 控制台中下**模板管理**而不是创建新的 RADIUS 客户端下**RADIUS 客户端和服务器**，已创建模板，但您没有改变 NPS 功能尚未。 若要更改 NPS 功能，必须从正确位置在 NPS 控制台中选择模板。

## <a name="creating-templates"></a>创建模板

若要创建一个模板，请打开 NPS 控制台，右键单击一种模板类型，例如**IP 筛选器**，然后单击**新建**。 新的模板属性对话框随即打开，可用于配置你的模板。

## <a name="using-templates-locally"></a>在本地使用模板

可以使用中已创建的模板**模板管理**通过导航到在 NPS 控制台中可以应用模板的位置。 例如，如果创建新的共享机密模板要应用到 RADIUS 客户端配置，在**RADIUS 客户端和服务器**并**RADIUS 客户端**，打开 RADIUS 客户端属性。 在中**选择一个现有的共享机密模板**，选择以前创建的可用模板列表中的模板。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。
