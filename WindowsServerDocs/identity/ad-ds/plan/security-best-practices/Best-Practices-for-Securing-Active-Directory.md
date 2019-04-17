---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: "保护 Active Directory 的最佳实践"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ecef0c173677d379524189b1769d4721ab0774a8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="best-practices-for-securing-active-directory"></a>保护 Active Directory 的最佳实践

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

本文档提供执行者角度，并包含可行的方法来帮助保护企业 Active Directory 环境 IT 经理一组。 Active Directory IT 基础结构中, 扮演重要角色，并确保颜色协调和不同的网络资源，在全球、 互连环境中的安全。 介绍的方法很大程度上根据 Microsoft 的信息安全和风险管理 (ISRM) 组织的体验，负责保护 Microsoft IT 和其他 Microsoft Business 部门，除了告知 Microsoft 全球 500 客户所选的大量资产。  
  
-   [Foreword](https://technet.microsoft.com/library/dn487451.aspx)  
  
-   [确认](https://technet.microsoft.com/library/dn487445.aspx)  
  
-   [摘要](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [简介](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [危害的途径](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [吸引帐户凭据盗用](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [减少 Active Directory 攻击 Surface](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [实现最低权限管理模型](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [实现安全管理主机](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [保护免受攻击域控制器](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [危害的迹象监视 Active Directory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [建议审核策略](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [对于危害计划](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [维护更安全环境](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [附录](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [附录 b：特权的帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [附录 c：受保护的帐户和 Active Directory 中的组](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [附录 d：保护 Active Directory 中的内置管理员帐户](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [附录 e：保护企业管理员中的组 Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [附录 f：保护域的 Active Directory 的管理员组](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [附录 g：保护中 Active Directory 的管理员组](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [附录 h：保护措施的本地管理员帐户和组](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [附录 i：创建管理帐户中的 Active Directory 的受保护的帐户和组](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [附录 l：事件到监视器](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [附录 m：文档链接，建议阅读](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


