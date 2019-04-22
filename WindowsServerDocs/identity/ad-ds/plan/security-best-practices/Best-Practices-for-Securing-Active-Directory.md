---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: 保护 Active Directory 的最佳方案
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 972def668634e794908a3ff2933d038ae38be5d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817078"
---
# <a name="best-practices-for-securing-active-directory"></a>保护 Active Directory 的最佳方案

>适用于：Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

本文档提供实践者的角度来看，并包含一套实用的技术来帮助保护企业 Active Directory 环境的 IT 管理人员。 Active Directory 在 IT 基础结构中扮演关键角色，并确保不同网络资源在全球相互连接的环境中的协调性和安全性。 讨论的方法很大程度上基于 Microsoft 信息安全和风险管理 (ISRM) 组织的经验，负责保护 Microsoft IT 和其他 Microsoft 业务部门，除了为建议的资产所选的 Microsoft 全球 500 强客户数。  
  
-   [执行摘要](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [简介](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [危及系统安全的途径](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [凭据被盗的帐户](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [减少 Active Directory 攻击面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [实现最小特权的管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [实现安全管理主机](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [保护域控制器免受攻击](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [监视 Active Directory 中遭到破坏的迹象](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [审核策略建议](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [规划泄露](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [维护更安全的环境](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [附录](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [附录 b:权限的帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [附录 c:受保护的帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [附录 d:确保在 Active Directory 中的内置 Administrator 帐户的安全](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [附录 e:保护 Active Directory 中的 Enterprise Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [附录 f:保护 Active Directory 中的 Domain Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [附录 g:保护 Active Directory 中的管理员组](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [附录 h:保护本地管理员帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [附录 i:为受保护的帐户和组在 Active Directory 中的创建管理帐户](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [附录 l:监视的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [附录 m:文档链接和建议的读物](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


