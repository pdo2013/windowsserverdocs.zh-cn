---
title: 删除存储空间直通中的服务器
ms.assetid: 9d8499a7-1307-473d-9f00-8a051164fad2
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
description: 如何在 Windows Server 中删除存储空间直通群集中的服务器。
ms.date: 2/5/2017
ms.localizationpriority: medium
ms.openlocfilehash: 9fcb67b3c5fbcff0ca2a48ee9a1d2e109af3e9a8
ms.sourcegitcommit: bb626d8626ef47426b781925ea588755dbe2e403
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7871600"
---
# 删除存储空间直通中的服务器

>适用于： Windows Server 2019、 Windows Server 2016

本主题介绍如何使用 PowerShell 删除[存储空间直通](storage-spaces-direct-overview.md)中的服务器。

## 删除服务器但保留其驱动器

如果你打算不久将服务器添加回群集，或者如果你打算通过将其移到另一个服务器来保留其驱动器，则可以从群集中删除服务器，但*不*从存储池中删除其驱动器。 如果你使用故障转移群集管理器删除服务器，那么这是默认行为。

使用 PowerShell 中的 [Remove-ClusterNode](https://technet.microsoft.com/library/hh847251.aspx) cmdlet：

```PowerShell
Remove-ClusterNode <Name>
```

此 cmdlet 会快速成功而不考虑任何容量注意事项，因为存储池会“记住”缺少的驱动器，并期待它们复原。 数据不会从缺少的驱动器中移走。 虽然缺少驱动器，但其 **OperationalStatus** 将显示为“通信中断”，并且你的卷将显示为“不完整”。

当驱动器复原时，系统会自动检测它们并将其与池重新关联，即使它们现在在新服务器中也不例外。

   >[!WARNING]
   > 不要将具有池数据的驱动器从一个服务器分发到多个其他服务器。 例如，如果一个具有十个驱动器的服务器发生故障（例如，因为其主板或启动驱动器发生故障），那么你**可以**将所有十个驱动器移到一个新服务器中，但是**不可以**将其中每个驱动器分别移到不同的其他服务器中。

## 删除服务器及其驱动器

如果你想从群集中永久删除服务器（有时称为缩容），则可以从群集中删除该服务器*并*从存储池中删除其驱动器。

将 **Remove-ClusterNode** cmdlet 与可选的 **-CleanUpDisks** 标志配合使用：

```PowerShell
Remove-ClusterNode <Name> -CleanUpDisks
```

此 cmdlet 可能会运行很长时间（有时会运行许多个小时），因为 Windows 必须将存储在该服务器上的所有数据移到群集中的其他服务器。 完成此操作后，将从存储池中永久删除驱动器，并会将受影响的卷恢复为正常运行状态。

### 要求

若要永久缩容（删除服务器*及*其驱动器），群集必须满足以下两个要求。 否则，**Remove-ClusterNode -CleanUpDisks** cmdlet 将在开始执行任何数据移动之前立即返回错误以最大程度减少中断。

#### 足够的容量

首先，你必须具有足够的存储容量以容纳所有卷在其余服务器中。

例如，如果你安装了四个服务器，每个服务器都有 10 个 1 TB 的驱动器，那么你拥有 40 TB 的总物理存储容量。 删除一个服务器及其所有驱动器后，你将剩下 30 TB 的容量。 如果卷的占用空间合计超过了 30 TB，则其余服务器将无法容纳它们，因此 cmdlet 将返回错误，并且不会移动任何数据。

#### 足够的故障域

其次，你必须具有足够的故障域（通常是服务器）来提供卷复原。

例如，如果你的卷在服务器级别使用三向镜像来进行复原，则它们无法完全正常运行，除非你至少具有三个服务器。 如果你刚好安装了 3 个服务器，然后尝试删除一个服务器及其所有驱动器，则 cmdlet 将返回一个错误并且不会移动任何数据。

下表显示了每种复原类型所需的最小故障域数。

|    复原          |    所需的最小故障域数   |
|------------------------|-------------------------------------|
|    双向镜像      |    2                                |
|    三向镜像    |    3                                |
|    双奇偶校验         |    4                                |

   >[!NOTE]
   > 可以简单地使用较少的服务器，例如在故障或维护期间。 但是，为了使卷恢复到完全正常运行状态，你必须安装上面列出的最小数量的服务器。

## 另请参阅

- [存储空间直通概述](storage-spaces-direct-overview.md)
