---
title: 选择命名空间类型
description: 本文介绍如何选择命名空间类型。
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: becaab1b4c35492200ad3d75d5f31829d85e10f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402199"
---
# <a name="choose-a-namespace-type"></a>选择命名空间类型

> 适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012，Windows Server 2008 R2，Windows Server 2008

创建命名空间时，必须选择以下两种命名空间类型之一：独立命名空间或基于域的命名空间。 此外，如果选择基于域的命名空间，则必须选择命名空间模式：Windows 2000 服务器模式或 Windows Server 2008 模式。

## <a name="choosing-a-namespace-type"></a>选择命名空间类型

如果你的环境符合以下条件之一，请选择独立命名空间：

-   你的组织不使用 Active Directory 域服务（AD DS）。
-   你希望通过使用故障转移群集增加命名空间的可用性。
-   需要在域中创建一个具有超过5000个 DFS 文件夹的命名空间，该域不符合基于域的命名空间的要求（Windows Server 2008 模式），如本主题后面所述。

> [!NOTE]
> 若要检查命名空间的大小，请在 DFS 管理控制台树中，右键单击该命名空间，单击**属性**，然后在**命名空间属性**对话框中查看命名空间大小。 有关 DFS 命名空间可伸缩性的详细信息，请参阅 Microsoft 网站[文件服务](https://technet.microsoft.com/library/cc771548.aspx)。

如果你的环境符合以下条件之一，请选择基于域的命名空间：

-   你希望通过使用多个命名空间服务器来确保命名空间的可用性。
-   你希望对用户隐藏命名空间服务器的名称。 这样，便可简化替换命名空间服务器或将命名空间迁移到其他服务器的过程。

## <a name="choosing-a-domain-based-namespace-mode"></a>选择基于域的命名空间模式

如果选择基于域的命名空间，则必须选择是使用 Windows 2000 服务器模式还是 Windows Server 2008 模式。 Windows Server 2008 模式包括对基于访问权限的枚举的支持和增加的可伸缩性。 Windows 2000 Server 中引入的基于域的命名空间现在称为 "基于域的命名空间（Windows 2000 服务器模式）"。

若要使用 Windows Server 2008 模式，域和命名空间必须满足下列最低要求：

-   林使用 Windows Server 2003 或更高版本的林功能级别。
-   域使用 Windows Server 2008 或更高版本的域功能级别。
-   所有命名空间服务器正在运行 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 或 Windows Server 2008。

如果你的环境支持，请在创建新的基于域的命名空间时选择 Windows Server 2008 模式。 此模式提供附加功能和可伸缩性，还消除了从 Windows 2000 服务器模式迁移命名空间的可能需要。

有关将命名空间迁移到 Windows Server 2008 模式的信息，请参阅[将基于域的命名空间迁移到 Windows server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)。

如果你的环境在 Windows Server 2008 模式下不支持基于域的命名空间，请使用命名空间的现有 Windows 2000 服务器模式。

## <a name="comparing-namespace-types-and-modes"></a>比较命名空间类型和模式

每种命名空间类型和模式的特征都在下表中进行了说明。

|特征|独立命名空间|基于域的命名空间（Windows 2000 Server 模式） |基于域的命名空间（Windows Server 2008 模式） | 
|---|---|---|---|
|命名空间的路径|\\ @ no__t-1*ServerName\RootName* |\\ @ no__t-1*NetBIOSDomainName\RootName* <br />\\ @ no__t-1*DNSDomainName\RootName*|\\ @ no__t-1*NetBIOSDomainName\RootName* <br /> \\ @ no__t-1*DNSDomainName\RootName*|
|命名空间信息存储位置|命名空间服务器上的注册表和内存缓存中|每个命名空间服务器上的 AD DS 和内存缓存中|每个命名空间服务器上的 AD DS 和内存缓存中|
|命名空间大小建议|命名空间中可能有 5,000 多个包含目标的文件夹；建议的上限是 50,000 个包含目标的文件夹|在 AD DS 中，命名空间对象的大小应小于 5 兆字节 (MB)，才能保持与没有运行 Windows Server 2008 的域控制器的兼容性。 这表示，包含目标的文件夹数大约不超过 5,000 个。|命名空间中可能有 5,000 多个包含目标的文件夹；建议的上限是 50,000 个包含目标的文件夹 |
|最小 AD DS 林功能级别|不需要 AD DS|Windows 2000|Windows Server 2003|
|最小 AD DS 域功能级别|不需要 AD DS|Windows 2000 混合式|Windows Server 2008|
|支持的最小命名空间服务器数|Windows 2000 Server|Windows 2000 Server|Windows Server 2008|
|支持基于访问的枚举（如果启用）|是，需要 Windows Server 2008 命名空间服务器|否|是|
|支持的方法，可确保命名空间的可用性|在故障转移群集上创建独立命名空间。|使用多个命名空间服务器托管命名空间。 （命名空间服务器必须位于同一个域中。）|使用多个命名空间服务器托管命名空间。 （命名空间服务器必须位于同一个域中。）|
|支持使用 DFS 复制对文件夹目标进行复制|加入 AD DS 域时受支持|支持|支持|

## <a name="see-also"></a>请参阅

-   [部署 DFS 命名空间](deploying-dfs-namespaces.md)
-   [将基于域的命名空间迁移到 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


