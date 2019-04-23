---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Active Directory 集成 DNS 区域
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5e0830944ec5d91dace52db337e89211ee362b98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873248"
---
# <a name="active-directory-integrated-dns-zones"></a>Active Directory 集成 DNS 区域

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

在域控制器上运行的域系统 (DNS) 服务器可以在 Active Directory 域服务 (AD DS) 中存储其区域。 在这种方式，不需配置单独的 DNS 复制拓扑，使用普通的 DNS 区域传送，因为所有区域数据通过 Active Directory 复制会自动都复制。 这简化了部署 DNS 的过程，并提供以下优势：  
  
-   为 DNS 复制创建多个主节点。 因此，运行 DNS 服务器服务的域中的任何域控制器可以写入到它们是权威的域名的 Active Directory 集成的 DNS 区域的更新。 不需要单独的 DNS 区域传输拓扑。  
  
-   支持安全动态更新。 安全动态更新允许管理员控制哪些计算机更新哪些名称并防止未经授权的计算机覆盖在 DNS 中的现有名称。  
  
Windows Server 2008 中 active Directory 集成的 DNS 将区域数据存储在应用程序目录分区。 （有从基于 Windows Server 2003 的 DNS 与 Active Directory 集成的任何行为更改。）在 AD DS 安装过程中创建以下 DNS 特定于应用程序目录分区：  
  
-   林范围的应用程序目录分区，称为 ForestDnsZones  
  
-   为每个域、 林的域范围的应用程序目录分区名为 DomainDnsZones  
  
有关 AD DS 将 DNS 信息存储在应用程序分区的方式的详细信息，请参阅[DNS 技术参考](https://go.microsoft.com/fwlink/?LinkId=106636)。  
  
> [!NOTE]  
> 我们建议你在运行 Active Directory 域服务安装向导 (Dcpromo.exe) 时安装 DNS。 如果这样做，向导将自动创建 DNS 区域委派。 有关详细信息，请参阅[部署 Windows Server 2008 目录林根域](https://technet.microsoft.com/library/cc731174.aspx)。  
  


