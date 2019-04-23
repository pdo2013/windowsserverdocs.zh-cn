---
ms.assetid: cd70b0aa-0a67-4966-a041-4dd3f302c98b
title: 创建 DNS 基础结构设计
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 080c36f8410be4d6b1933c74730e2b55ce8d0a0b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856128"
---
# <a name="creating-a-dns-infrastructure-design"></a>创建 DNS 基础结构设计

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建 Active Directory 林和域设计后，必须设计域名系统 (DNS) 基础结构以支持 Active Directory 逻辑结构。 DNS 使用户能够使用很容易记住要连接到计算机和 IP 网络上的其他资源的友好名称。 Windows Server 2008 中 active Directory 域服务 (AD DS) 需要 DNS。  
  
设计 DNS 以支持 AD DS 的过程根据你的组织是否有现有的 DNS 服务器服务，或者要部署新的 DNS 服务器服务：  
  
- 如果已有现有的 DNS 基础结构，必须将 Active Directory 命名空间集成到该环境。 有关详细信息，请参阅[到现有的 DNS 基础结构的集成 AD DS](../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)。  
- 如果在位置没有 DNS 基础结构，必须设计和部署新的 DNS 基础结构来支持 AD DS。 有关详细信息，请参阅[部署域名系统 (DNS)](https://go.microsoft.com/fwlink/?LinkId=93656)。  
  
如果你的组织具有现成 DNS 基础结构，必须确保您了解您的 DNS 基础结构如何与 Active Directory 命名空间进行交互。 有关可帮助您记录您现有的 DNS 基础结构设计的工作表，下载 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 从[作业辅助工具为 Windows Server 2003 部署工具包](https://go.microsoft.com/fwlink/?LinkID=102558)和打开"DNS 清单"(DSSLOGI_8.doc)。  
  
> [!NOTE]  
> 除了 IP 版本 4 (IPv4) 地址，Windows Server 还支持 IP 版本 6 (IPv6) 地址。 工作表以帮助你记录当前的 DNS 结构的递归名称解析方法时列出的 IPv6 地址，请参阅[附录 a:DNS 清单](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)。
  
设计 DNS 基础结构以支持 AD DS 之前，将很有帮助，若要了解 DNS 层次结构、 DNS 名称解析过程中，及如何 DNS 支持 AD DS。 有关 DNS 层次结构和名称解析过程的详细信息，请参阅 DNS 技术参考 ([https://go.microsoft.com/fwlink/?LinkID=48145](https://go.microsoft.com/fwlink/?LinkID=48145))。 有关 DNS 如何支持 AD DS 的详细信息，请参阅 Active Directory 技术参考 DNS 支持 ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147))。  
  
## <a name="in-this-section"></a>本节内容  

- [查看 DNS 概念](../../ad-ds/plan/Reviewing-DNS-Concepts.md)  
- [DNS 和 AD DS](../../ad-ds/plan/DNS-and-AD-DS.md)  
- [为 AD DS 所有者角色分配 DNS](../../ad-ds/deploy/Assigning-the-DNS-for-AD-DS-Owner-Role.md)  
- [将 AD DS 集成到现有的 DNS 基础结构](../../ad-ds/plan/../../ad-ds/plan/Integrating-AD-DS-into-an-Existing-DNS-Infrastructure.md)  
