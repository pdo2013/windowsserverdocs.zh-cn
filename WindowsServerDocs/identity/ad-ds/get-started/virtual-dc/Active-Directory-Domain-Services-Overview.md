---
ms.assetid: f052dfcd-dace-4485-8d0a-cc7df5cf3751
title: "Active Directory 域名服务概述"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b502017315d2b8b6b3d6ddfdad40c6d0f913e6c3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="active-directory-domain-services-overview"></a>Active Directory 域名服务概述

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012


目录是会对象的信息存储在网络的分层结构。 目录服务，如 Active Directory 域服务 (广告 DS)，提供目录数据存储和网络的用户和管理员推出此类数据的方法。 例如，广告 DS 存储有关用户帐户，如姓名、 密码、 电话号码和等等的信息，并使相同网络上其他授权的用户可以访问此信息。

Active Directory 存储在网络上的对象的信息，并使此信息的管理员和用户地查找和使用易于。 Active Directory 用作结构化的数据官方商城基础目录信息的逻辑，分层组织。

此数据官方商城，也称为目录包含 Active Directory 对象的信息。 这些对象通常包括如服务器、 卷、 打印机和网络的用户和计算机帐户共享的资源。 有关 Active Directory 数据应用商店的详细信息，请参阅[目录数据存储](https://technet.microsoft.com/library/cc736627(v=ws.10).aspx)。

通过登录身份验证和目录中的对象访问控制与 Active Directory 集成安全。 一次网络登录，使用管理员目录数据和组织整个其网络，可以管理，并授权的网络用户可以访问该网络上的任意位置的资源。 基于策略管理简化了甚至最复杂的网络的管理。 有关 Active Directory 安全性的详细信息，请参阅安全概述。

Active Directory 还包括：
* 一组规则，**架构**、 定义对象类和中包含属性目录、 约束并限制这些对象，实例和其名称的格式。 有关模式的详细信息，请参阅方案。


* 一个**全球目录**，其中包含有关目录中的每个对象的信息。 这使用户和管理员查找目录信息无论目录中的哪个域实际上包含的数据。 全球目录有关详细信息，请参阅全球目录中的角色。


* 一个**查询和索引机制**，以便可以发布并找到网络用户或应用程序对象及其属性。 有关查询到的目录的详细信息，请参阅查找目录信息。


* 一个**复制服务**，通过网络分发目录数据。 在某个域中的所有域控制器参与复制，并包含完整的所有其域目录信息副本。 对目录数据的任何更改被复制到域中的所有域控制器。 有关 Active Directory 复制的详细信息，请参阅复制概述。

## <a name="understanding-active-directory"></a>了解 Active Directory
 此部分中提供了指向核心 Active Directory 概念：
 
* [Active Directory 结构和技术存储](https://technet.microsoft.com/library/cc759186(v=ws.10).aspx)
* [域控制器角色](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* Active Directory 方案 
* [了解信任](https://technet.microsoft.com/library/cc771294(v=ws.10).aspx) 
* [Active Directory 复制技术](https://technet.microsoft.com/library/cc786438(v=ws.10).aspx) 
* [Active Directory 搜索和发布技术](https://technet.microsoft.com/library/cc775686(v=ws.10).aspx) 
* 与 DNS 和组策略进行交互 
* [了解方案](https://technet.microsoft.com/library/cc759402(v=ws.10).aspx) 

Active Directory 概念的详细列表，请参阅[了解 Active Directory](https://technet.microsoft.com/library/cc781408(v=ws.10).aspx)。 


