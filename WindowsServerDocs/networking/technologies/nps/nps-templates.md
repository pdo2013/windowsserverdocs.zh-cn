---
title: NPS 模板
description: 本主题提供 Windows Server 2016 中的网络策略服务器模板的概述。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bafff7a6a15312ab1bdca2e7b98307bc94731d3a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405318"
---
# <a name="nps-templates"></a>NPS 模板

>适用于：Windows Server（半年频道）、Windows Server 2016

网络策略服务器 \(NPS @ no__t 模板允许你创建配置元素，例如远程身份验证拨入用户服务 \(RADIUS @ no__t-3 客户端或共享机密，你可以在本地 NPS 上重复使用该配置元素，并在其他NPSs.

NPS 模板旨在减少在一台或多台服务器上配置 NPS 所需的时间和成本。 以下 NPS 模板类型可用于模板管理中的配置：

- 共享机密
- RADIUS 客户端
- 远程 RADIUS 服务器
- IP 筛选器
- 修正服务器组

配置模板不同于直接配置 NPS。 创建模板不会影响 NPS 的功能。 仅当在 NPS 控制台中的适当位置选择模板时，模板才会影响 NPS 功能。 

例如，如果在 NPS 控制台中的 "RADIUS 客户端和服务器" 下配置 RADIUS 客户端，则已更改 NPS 配置，并执行将 NPS 配置为与其中一个网络访问服务器（\(NAS's @ no__t）进行通信的步骤。 @no__t 0The 下一步是将 NAS 配置为与 NPS 通信。 \)但是，如果你在 NPS 控制台中的 "**模板管理**" 下配置新的 Radius 客户端模板，而不是在**RADIUS 客户端和服务器**下创建新的 radius 客户端，则已创建模板，但尚未更改 NPS功能。 若要更改 NPS 功能，你必须从 NPS 控制台中正确的位置选择模板。

## <a name="creating-templates"></a>创建模板

若要创建模板，请打开 NPS 控制台，右键单击模板类型（如**IP 筛选器**），然后单击 "**新建**"。 此时将打开一个新的模板属性对话框，可通过该对话框配置模板。

## <a name="using-templates-locally"></a>本地使用模板

通过导航到 NPS 控制台中可应用模板的位置，可以使用在 "**模板管理**" 中创建的模板。 例如，如果你要将新的共享机密模板应用到 RADIUS 客户端配置，请在**Radius 客户端和服务器**和**radius 客户端**中打开 radius 客户端属性。 在 "**选择现有的共享机密模板**" 中，从可用模板列表中选择先前创建的模板。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。
