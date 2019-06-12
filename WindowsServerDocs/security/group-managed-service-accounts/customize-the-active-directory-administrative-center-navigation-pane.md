---
title: 自定义 Active Directory 管理中心导航窗格
ms.prod: windows-server-threshold
description: Windows Server 安全
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: a4cab0246226cf22a1b7212b832a902783952407
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446502"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>自定义 Active Directory 管理中心导航窗格

>适用于：Windows 服务器 （半年频道），Windows Server 2016

  使用树视图中，这是类似于 Active Directory 用户和计算机控制台树中，或使用列表视图，您可以浏览 Active Directory 管理中心导航窗格。

 无论使用树视图或列表视图，您可以自定义 Active Directory 管理中心导航窗格中随时通过从本地域或任何外部域中添加各种容器\(，即不是本地域的域具有与本地域建立信任关系\)到导航窗格中，作为单独的节点。 自定义 Active Directory 管理中心导航窗格可以更快地访问 Active Directory 对象。 有关详细信息，请参阅[在 Active Directory 管理中心内管理不同域](manage-different-domains-in-active-directory-administrative-center.md)。

 此外，若要进一步自定义导航窗格，可以重命名或删除这些手动添加的导航窗格节点，创建这些节点的副本，或者在导航窗格中向上或向下移动它们。

> [!NOTE]
>  无法自定义默认的本地域节点。

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>若要自定义 Active Directory 管理中心导航窗格

1. 在 Active Directory 管理中心导航窗格中，右键\-单击你想要修改的节点。 可以修改节点的位置和名称，还可以创建节点的副本。

2. 单击以下命令之一：

   -   **重命名**

   -   **创建重复的节点**

   -   **删除**

   -   **上移**

   -   **向下移动**

   通过使用列表视图，您可以利用的最近使用过\(MRU\)列表。 访问此导航节点中的至少一个容器时，MRU 列表自动显示在导航节点下。 此外可以通过展开 Active Directory 管理中心窗口顶部的痕迹导航栏来查看当前的 MRU 列表。 MRU 列表始终包含在某个特定导航节点中访问的最后三个容器。 在每次选择特定容器时，该容器会被添加至 MRU 列表的顶部，并从 MRU 列表中删除最后一个容器。

  

