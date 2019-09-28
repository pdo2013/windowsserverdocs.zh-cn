---
title: 管理 NPS 模板
description: 本主题提供有关如何为 Windows Server 2016 中的网络策略服务器创建、应用、导出和导入 NPS 模板的说明。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5ac733c11b277f09e64779c33d3392303fc34d98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396156"
---
# <a name="manage-nps-templates"></a>管理 NPS 模板

>适用于：Windows Server（半年频道）、Windows Server 2016

你可以使用网络策略服务器 \(NPS @ no__t 模板来创建配置元素，例如远程身份验证拨入用户服务 \(RADIUS @ no__t-3 客户端或共享机密，你可以在本地 NPS 上重复使用和导出以便在其他NPSs. 

模板管理在 NPS 控制台中提供了一个节点，你可以在其中创建、修改、删除、复制和查看 NPS 模板的使用。 NPS 模板旨在减少在一台或多台服务器上配置 NPS 所需的时间和成本。

以下 NPS 模板类型可用于模板管理中的配置。

- **共享机密**。 此模板类型使你能够指定共享机密，你可以在配置 RADIUS 客户端和服务器时，通过在 NPS 控制台中的适当位置选择模板来重复使用共享机密。 

- **RADIUS 客户端**。 此模板类型使你可以配置 RADIUS 客户端设置，你可以通过在 NPS 控制台中的相应位置选择该模板来重新使用这些设置。

- **远程 RADIUS 服务器**。 此模板使你可以配置远程 RADIUS 服务器设置，你可以通过在 NPS 控制台中的相应位置选择该模板来重新使用这些设置。 

- **IP 筛选器**。 使用此模板，你可以创建 Internet 协议版本4（IPv4）和 Internet 协议版本 6 \(IPv6 @ no__t 筛选器，这些筛选器可重复使用 \(by 在 NPS 控制台 @ no__t 中的适当位置选择模板。配置网络策略。

## <a name="create-an-nps-template"></a>创建 NPS 模板

配置模板不同于直接配置 NPS。 创建模板不会影响 NPS 的功能。 仅当你在 NPS 控制台中的适当位置选择了模板，并且应用模板影响 NPS 功能时才会出现此情况。 

例如，如果在 NPS 控制台中的 " **Radius 客户端和服务器**" 下配置 radius 客户端，则需要更改 nps 配置并执行将 nps 配置为与网络访问服务器之一通信的步骤。 @no__t 0The 下一步是将网络访问服务器 \(NAS @ no__t 配置为与 NPS 通信。 @no__t 

但是，如果你在 NPS 控制台中的 "**模板管理**" 下配置新的**radius 客户端**模板，而不是在**RADIUS 客户端和服务器**下创建新的 radius 客户端，则已创建模板，但尚未更改NPS 功能。 若要更改 NPS 功能，你必须从 NPS 控制台中正确的位置应用该模板。

以下过程提供了有关如何创建新模板的说明。

**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。

### <a name="to-create-an-nps-template"></a>创建 NPS 模板


1. 在 NPS 上服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 此时将打开 NPS 控制台。 

2. 在 NPS 控制台中，展开 "**模板管理**"，右键单击模板类型，如 " **RADIUS 客户端**"，然后单击 "**新建**"。

3. 此时将打开一个新的模板属性对话框，您可以使用该对话框来配置模板。

## <a name="apply-an-nps-template"></a>应用 NPS 模板

您可以使用在 "**模板管理**" 中创建的模板，方法是导航到 NPS 控制台中可应用模板的位置。 例如，如果要将共享机密模板应用到 RADIUS 客户端配置，可以使用以下过程。

**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。

### <a name="to-apply-an-nps-template"></a>应用 NPS 模板

1. 在 NPS 上服务器管理器中，单击 "**工具**"，然后单击 "**网络策略服务器**"。 此时将打开 NPS 控制台。

2. 在 NPS 控制台中，展开 " **Radius 客户端和服务器**"，然后展开 " **radius 客户端**"。

3.In **RADIUS 客户端**，在详细信息窗格中，右键单击要向其应用 NPS 模板的 RADIUS 客户端，然后单击 "**属性**"。

4. 在 RADIUS 客户端的 "属性" 对话框中的 "**选择现有共享机密" 模板**中，从模板列表中选择要应用的模板。

## <a name="export-or-import-nps-templates"></a>导出或导入 NPS 模板

你可以导出模板以便在其他 NPSs 上使用，也可以将模板导入**模板管理**，以便在本地计算机上使用。 

**Administrators**中的成员身份或同等身份是完成此过程所需的最低要求。

### <a name="to-export-or-import-nps-templates"></a>导出或导入 NPS 模板

1. 若要导出 NPS 模板，请在 NPS 控制台中右键单击 "**模板管理**"，然后单击 "将**模板导出到文件**"。

2. 若要导入 NPS 模板，请在 NPS 控制台中右键单击 "**模板管理**"，然后单击 "**从计算机导入模板**" 或 "**从文件导入模板**"。


