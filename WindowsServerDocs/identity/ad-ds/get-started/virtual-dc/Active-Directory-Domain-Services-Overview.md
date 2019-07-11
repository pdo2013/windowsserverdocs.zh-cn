---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: Active Directory 域服务概述
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ed8a22881cd20633e6fcd61b146f3b0aad7a757b
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792283"
---
# <a name="active-directory-domain-services-overview"></a>Active Directory 域服务概述

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


目录是将对象的信息存储在网络的层次结构。 目录服务，如 Active Directory 域服务 (AD DS) 提供了用于存储目录数据并使此数据可供网络用户和管理员的方法。 例如，AD DS 存储有关用户帐户，如名称、 密码、 电话号码等信息，并使同一网络上的其他已授权的用户访问此信息。

Active Directory 将对象的信息存储在网络上，并使此信息来查找和使用的管理员和用户可以更轻松。 Active Directory 使用的结构化的数据存储的基础逻辑、 分层组织的目录信息。

此数据存储区，也称为目录，包含有关 Active Directory 对象的信息。 通常，这些对象包括共享的资源，如服务器、 卷、 打印机和网络用户和计算机帐户。 有关 Active Directory 数据存储区的详细信息，请参阅[Directory 数据存储区](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx)。

安全性通过登录身份验证以及对目录中的对象的访问控制与 Active Directory 集成。 单点网络登录，管理员可以管理目录数据和在其网络中，整个组织和授权的网络用户可以访问网络上任意位置的资源。 基于策略的管理简化了即使最复杂的网络的管理。 有关 Active Directory 安全性的详细信息，请参阅[安全性概述](../../plan/security-best-practices/best-practices-for-securing-active-directory.md)。

Active Directory 还包括：
* 一组规则**架构**、 定义对象的类和属性中包含该目录、 约束和限制这些对象的实例以及其名称的格式。 有关架构的详细信息，请参阅架构。


* 一个**全局编录**的包含目录中的每个对象有关的信息。 这允许用户和管理员可以找到目录信息而不考虑在目录中的哪个域实际包含的数据。 有关全局编录的详细信息，请参阅全局编录的角色。


* 一个**查询和索引机制**，以便可以发布和发现的网络用户或应用程序对象和其属性。 有关查询目录的详细信息，请参阅查找目录信息。


* 一个**复制服务**，通过网络分发目录数据。 在域中的所有域控制器参与复制，并包含其域的所有目录信息的完整副本。 对目录数据的任何更改均复制到域中的所有域控制器。 有关 Active Directory 复制的详细信息，请参阅复制概述。

## <a name="understanding-active-directory"></a>了解 Active Directory
 本部分提供了链接到 Active Directory 的核心概念：
 
* [Active Directory 结构和存储技术](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [域控制器角色](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Active Directory 架构](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771796(v=ws.10))
* [了解信任](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771568(v=ws.10)) 
* [Active Directory 复制技术](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Active Directory 搜索和发布技术](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* [与 DNS 和组策略进行互操作](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197486(v=ws.10))
* [了解架构](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Active Directory 概念的详细列表，请参阅[了解 Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx)。 


