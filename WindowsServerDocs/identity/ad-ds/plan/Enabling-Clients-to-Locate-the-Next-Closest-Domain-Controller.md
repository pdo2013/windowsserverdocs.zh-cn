---
ms.assetid: 7dd905ea-4235-4519-8400-31b4fa0ed1bf
title: 启用客户端查找下一个最近的域控制器
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7550bdcea4e7b06d31463744bfdc3319c012c62c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880358"
---
# <a name="enabling-clients-to-locate-the-next-closest-domain-controller"></a>启用客户端查找下一个最近的域控制器

>适用于：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012

如果必须运行 Windows Server 2008 或更高版本的域控制器，你可以使它的客户端计算机运行 Windows Vista 或更高版本或 Windows Server 2008 或更高版本，从而更高效地查找域控制器**尝试下一步最近的站点**组策略设置。 此设置可改进通过帮助简化网络流量，尤其是在具有许多分支机构和站点的大型企业中的域控制器定位程序 （DC 定位器）。

此新设置可能会影响如何配置站点链接成本，因为它会影响域控制器位于的顺序。 对于有多个中心站点和分支机构的企业，通过确保，客户端故障转移到下一个最近的中心站点时在最近的中心站点找不到域控制器可显著减少网络上的 Active Directory 通信。

常规最佳做法是，应简化您的站点拓扑和站点链接成本如果启用了以尽可能**尝试下一个最近的站点**设置。 在企业中具有多个中心站点，这可以简化任何计划，以便为处理在一个站点中的客户端需要故障转移到另一个站点中的域控制器的情况。

默认情况下**尝试下一个最近的站点**设置未启用。 当未启用该设置时，DC 定位程序将使用以下算法来查找域控制器：

- 尝试在同一站点中查找域控制器。
- 如果没有域控制器在同一站点中不可用，请尝试在域中查找任何域控制器。

> [!NOTE]
> 这是 DC 定位程序在以前版本的 Active Directory 中使用相同的算法。 有关详细信息，请参阅文章[方式对 Active Directory 工作原理的 DNS 支持](https://go.microsoft.com/fwlink/?LinkId=108587)。

如果启用**尝试下一个最近的站点**设置，DC 定位程序则使用以下算法来查找域控制器：

- 尝试在同一站点中查找域控制器。
- 如果没有域控制器在同一站点中不可用，请尝试在下一个最近的站点中查找域控制器。 站点是更接近是否比另一个站点与成本更高版本站点链接成本较低站点链接。
- 如果在下一个最近的站点中没有域控制器不可用，请尝试在域中查找任何域控制器。

**尝试下一个最近的站点**设置使用自动站点覆盖的协调工作。 例如，如果下一个最近的站点有没有域控制器，DC 定位程序会尝试查找该站点执行自动站点覆盖的域控制器。

默认情况下，DC 定位器不考虑任何包含只读域控制器 (RODC)，确定在下一个最近的站点的站点。 此外，当客户端从运行版本早于 Windows Server 2008 的域控制器获取响应，DC 定位程序行为是相同时再设置未启用。

例如，假定站点拓扑具有四个网站与下图中的站点链接值。 在此示例中，所有域控制器都都运行 Windows Server 2008 或更高版本的可写域控制器。

![启用客户端查找 dc](media/Enabling-Clients-to-Locate-the-Next-Closest-Domain-Controller/beff4087-fb2a-463b-96ac-d440a9e29b75.gif)

当**尝试下一个最近的站点**如果 Site_B 中的客户端计算机尝试查找域控制器，将在此示例中，启用组策略设置，它首先尝试在其自己 Site_B 中查找域控制器。 如果没有在 Site_B 中可用，它尝试在 Site_A 中查找域控制器。

如果未启用该设置，客户端将尝试如果没有域控制器位于 Site_B Site_A、 Site_C 或 Site_D 中查找域控制器。

> [!NOTE]
> **尝试下一个最近的站点**设置使用自动站点覆盖的协调工作。 例如，如果下一个最近的站点有没有域控制器，DC 定位程序会尝试查找该站点执行自动站点覆盖的域控制器。

若要将应用**尝试下一个最近的站点**设置，可以创建组策略对象 (GPO) 并将其链接到相应的对象，为你的组织，也是可以修改默认域策略，使其影响运行 Windows 的所有客户端Vista 或更高版本和 Windows Server 2008 或更高版本的域中。 有关如何设置的详细信息**尝试下一个最近的站点**设置，请参见[启用客户端在下一个最近的站点中查找域控制器](https://technet.microsoft.com/library/cc772592.aspx)。
