---
title: 用于 Windows Server 2016 的服务器角色升级和迁移矩阵
description: 显示可以升级或迁移到 Windows Server 2016 的服务器角色。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/05/2016
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7e031a64-b1e6-4cf6-994a-e7c575835f6a
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: 9f8310baf659810d5d51587bafcc868c59ace61a
ms.sourcegitcommit: 5b05bae2f47ef5d9f6940e13a2e8097f311206de
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/19/2018
ms.locfileid: "2304423"
---
# <a name="server-role-upgrade-and-migration-matrix-for-windows-server-2016"></a>用于 Windows Server 2016 的服务器角色升级和迁移矩阵

>适用于：Windows Server 2016

本页中的网格介绍专用于移动到 Windows Server 2016 的服务器角色升级和迁移选项。 有关单个角色的迁移指南，请访问[在 Windows Server 中迁移角色和功能](https://docs.microsoft.com/windows-server/get-started/migrate-roles-and-features)。 有关安装和升级的详细信息，请参阅 [Windows Server Installation, Upgrade, and Migration](https://docs.microsoft.com/windows-server/get-started/installation-and-upgrade)（Windows Server 安装、升级和迁移）。

|服务器角色|是否可从 Windows Server 2012 R2 升级？|是否可从 Windows Server 2012 升级？|是否支持迁移？|是否无需停机就可完成迁移？|  
|-------------------|----------|--------------|--------------|----------|  
|Active Directory 证书服务| 是|    是|    是|    否|
|Active Directory 域服务|  是|    是|    是|    是|
|Active Directory 联合身份验证服务|  否| 否| 是|    否（需要将新节点添加到场中）|
|Active Directory 轻型目录服务|   是|    是|    是|    是|
|Active Directory 权限管理服务|   是|    是|    是|    否|
|DHCP 服务器|   是|    是|    是|    是|
|DNS 服务器|    是|    是|    是|    否|
|故障转移群集|是，[群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade)过程包括节点暂停排出、逐出、升级到 Windows Server 2016 以及重新加入原始群集。 是，当群集删除服务器以进行升级，并随后再将服务器添加到不同的群集时。|而非服务器是群集的一部分时。 是，当群集删除服务器以进行升级，并随后再将服务器添加到不同的群集时。  |是|否，对于 Windows Server 2012 故障转移群集而言。 是，对于具有 Hyper-V VM 的 Windows Server 2012 R2 故障转移群集或运行横向扩展文件服务器角色的 Windows Server 2012 R2 故障转移群集。 请参阅[群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade)。|
|文件和存储服务| 是|    是|    因子功能而不同|  否|
|Hyper-V| 是的。 （当主机是群集的一部分，且使用群集操作系统滚动升级过程，该过程包括节点暂停排出、收回、升级到 Windows Server 2016 并重新加入原始群集。）|  否|   是|  否，对于 Windows Server 2012 故障转移群集而言。 是，对于具有 Hyper-V VM 的 Windows Server 2012 R2 故障转移群集或运行横向扩展文件服务器角色的 Windows Server 2012 R2 故障转移群集。 请参阅[群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade)。| 
|打印和传真服务|    否| 否| 是 (Printbrm.exe)| 否|
|远程桌面服务|   是，对于所有子角色，但不支持混合模式场|   是，对于所有子角色，但不支持混合模式场|   是|    否|
|Web 服务器 (IIS)|  是|    是|    是|    否|
|Windows Server Essentials 体验|  是|    N/A - 新功能|  是|    否|
|Windows Server Update Services|    是|    是|    是|    否|
|工作文件夹|  是|    是|    是|    是，使用[群集操作系统滚动升级](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade)从 WS 2012 R2 群集进行操作时。|

