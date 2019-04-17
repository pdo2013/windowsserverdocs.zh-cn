---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: "Active Directory 集成 DNS 区域"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 11940ddda320ea36bf8439afed91fcf6803f2fee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-integrated-dns-zones"></a>Active Directory 集成 DNS 区域

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

域名系统 (DNS) 服务器域控制器上运行 Active Directory 域服务 (广告 DS) 中，可以存储它们的区域。 这种方式，不需要配置单独 DNS 复制拓扑使用普通 DNS 区域传输因为区域中的所有数据将自动都复制通过 Active Directory 复制。 这简化部署 DNS 的过程，并提供以下优势：  
  
-   对 DNS 复制创建多个母版。 因此中运行 DNS 服务器服务, 任何域控制器可以更新写入 Active Directory 集成 DNS 区域它们是权威的域名。 不需要单独的 DNS 区域传输拓扑。  
  
-   支持的安全的动态更新。 安全的动态更新允许管理员可以控制哪些计算机更新哪些名称和防止未经授权的计算机覆盖 DNS 中的现有的名称。  
  
在 Windows Server 2008 的 active Directory 集成 DNS 将区域数据存储在应用程序目录分区。 （没有行为更改的 Active Directory 基于 Windows Server 2003 DNS 集成从。）广告 DS 安装期间创建 DNS 特定应用程序的以下目录分区：  
  
-   森林范围的应用程序目录分区，称为 ForestDnsZones  
  
-   为树林中的每个域域范围的应用程序 directory 分区名为 DomainDnsZones  
  
有关广告 DS 如何存储在应用程序分区中的 DNS 信息的详细信息，请参阅[DNS 技术参考](https://go.microsoft.com/fwlink/?LinkId=106636)。  
  
> [!NOTE]  
> 我们建议您安装 DNS，当你在运行 Active Directory 域服务安装向导 (Dcpromo.exe)。 如果你执行此操作时，该向导将自动创建 DNS 区域委派。 有关详细信息，请参阅[部署 Windows Server 2008 森林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  


