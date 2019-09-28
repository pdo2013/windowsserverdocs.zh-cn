---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: 保护 Active Directory 的最佳方案
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0d6f4abbf5dd071a2e229acbda2057c1f81851e6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408622"
---
# <a name="best-practices-for-securing-active-directory"></a>保护 Active Directory 的最佳方案

>适用于：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本文档提供实践者的视角，并包含一套实用的技术，可帮助 IT 管理人员保护企业 Active Directory 环境。 Active Directory 在 IT 基础结构中扮演关键角色，并确保不同网络资源在全球相互连接的环境中的协调性和安全性。 所讨论的方法很大程度上取决于 Microsoft 信息安全和风险管理（ISRM）组织的经验，它负责保护 Microsoft IT 及其他 Microsoft 业务部门的资产，并向你提供选择的 Microsoft 全球500客户数。  
  
-   [执行摘要](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [简介](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [危及系统安全的途径](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [凭据容易被盗的帐户](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [降低 Active Directory 攻击面](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [实现最小特权管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [实现安全管理主机](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [保护域控制器免遭攻击](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [监视 Active Directory 以获得损害迹象](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [审核策略建议](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [规划折衷](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [维护更安全的环境](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [附录](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [附录 B：Active Directory 中有权限的帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [附录 C：Active Directory 中受保护的帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [附录 D：保护 Active Directory 中的内置 Administrator 帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [附录 E：保护 Active Directory 中的 Enterprise Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [附录 F：保护 Active Directory 中的 Domain Admins 组](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [附录 G：保护 Active Directory 中的 Administrators 组](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [附录 H：保护本地管理员帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [附录 I：为 Active Directory 中受保护的帐户和组创建管理帐户](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [附录 L：要监视的事件](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [附录 M：文档链接和建议的读物](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


