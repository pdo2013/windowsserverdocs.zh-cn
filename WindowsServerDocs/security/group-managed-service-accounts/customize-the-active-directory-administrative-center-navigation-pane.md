---
title: "自定义 Active Directory 管理中心的导航窗格"
ms.prod: windows-server-threshold
description: "Windows Server 安全"
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: e7b1128d93912f724225905bedd38131f8aab0b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>自定义 Active Directory 管理中心的导航窗格

>适用于：Windows Server（半年通道），Windows Server 2016

  你可以通过 Active Directory 管理中心导航窗格中使用浏览树视图中，类似于 <DICT__Active Directory 用户和计算机>Active Directory 的用户和计算机 < 月 DICT__Active Directory 用户和计算机 >控制台树中，也可以通过列表视图。

 无论你使用的树视图或列表视图，你可以自定义你的 Active Directory 管理中心导航窗格中随时通过添加本地域或任何外域从各种容器 \ (即已建立的、与本地 domain\ 信任地域之外域）导航窗格作为独立节点。 自定义 Active Directory 管理中心导航窗格中，可提供更快地访问 Active Directory 对象。 有关详细信息，请参阅[管理 Active Directory 管理中心在不同的域](manage-different-domains-in-active-directory-administrative-center.md)。

 此外，若要进一步自导航窗格中，你可以重命名或删除这些手动添加的导航窗格节点、创建重复这些节点，或移动向上或向下导航窗格中。

> [!NOTE]
>  你无法自定义默认地域节点。

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>若要自定义 Active Directory 管理中心的导航窗格

1.  Active Directory 管理中心导航窗格中，right\ 单击你想要修改的节点。 你可以修改的位置或名称节点，也可以创建的副本。

2.  单击以下命令之一：

    -   **重命名**

    -   **创建重复节点**

    -   **删除**

    -   **向上移动**

    -   **向下移动**

 通过使用列表视图，你可以充分利用最近使用 \(MRU\) 列表。 当你访问此导航节点中的至少一个容器 MRU 列表自动显示下导航节点。 通过扩展痕迹导航栏 Active Directory 管理中心窗口顶部，您还可以查看当前 MRU 列表。 MRU 的列表始终包含你访问在某个特定导航节点中的最后三个容器。 每当你选择的特定容器此容器将添加到 MRU 列表顶部和 MRU 列表中的最后一个容器会删除。

  

