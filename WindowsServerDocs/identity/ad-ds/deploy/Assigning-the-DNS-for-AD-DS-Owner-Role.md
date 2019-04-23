---
ms.assetid: 4163cf03-3bff-426c-9844-4cc2d7897d52
title: 为 AD DS 所有者角色分配 DNS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: dde9ed6035b30ba5b5b96b7132d25a1f8a1c0b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884018"
---
# <a name="assigning-the-dns-for-ad-ds-owner-role"></a>为 AD DS 所有者角色分配 DNS

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

林所有者将分配域名系统 (DNS) 林的 Active Directory 域服务 (AD DS) 所有者。 AD DS 所有者的林 DNS 是人员 （或用户组） 由谁来负责监督 DNS 部署 AD DS 基础结构和确保 （如有必要） 与正确的 Internet 颁发机构注册域名称。  
  
AD DS 所有者的 DNS 负责在林中的 AD DS 设计的 DNS。 如果你的组织当前运行的 DNS 服务器服务，现有的 DNS 服务器服务的 DNS 设计器适用于 AD DS 所有者委派到域控制器上运行的 DNS 服务器的林根 DNS 名称的 DNS。  
  
在林中的 AD DS 所有者的 DNS 还与动态主机配置协议 (DHCP) 组和组织的 DNS 组维护联系人，并协调与这些组的计划在林中每个域的单个 DNS 所有者 （如果有）。 林 DNS 所有者可确保 DHCP 和 DNS 组所涉及的 AD DS 设计过程在 DNS 中，以便每个组已意识到 DNS 的设计规划，并且可以提前提供输入。  
  


