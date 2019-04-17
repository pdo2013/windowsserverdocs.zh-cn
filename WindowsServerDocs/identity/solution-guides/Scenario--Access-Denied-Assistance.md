---
ms.assetid: aae907eb-11cf-4a87-a046-8680872ed0b1
title: "方案访问拒绝协助"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d6b8d8a094aa86fd5be78d2eae13bae50df3fd79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/03/2017
---
# <a name="scenario-access-denied-assistance"></a>方案：访问拒绝协助

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

在尝试访问共享的文件和文件夹文件服务器，在他们没有权限时，用户将获取拒绝访问消息。 管理员通常没有相应的上下文进行疑难解答访问问题，从而使其更难以解决该问题。  
  
## <a name="scenario-description"></a>方案说明  
获取帮助是 Windows Server 2012，该法案通过以下方式相关的问题进行疑难解答中的新增功能访问拒绝要访问的文件和文件夹：  
  
-   **自行协助。** 如果用户可以确定该问题并修复此问题，以便他们可以请求访问权限，业务侵犯较低，，以及在中心访问策略需要安装任何特殊的异常。 访问拒绝协助提供文件服务器管理员，可以使用特定于企业的信息进行自定义拒绝访问消息。 例如，管理员可以设置该邮件，以便用户可以不涉及文件服务器管理员数据所有者从请求访问权限。  
  
-   **通过数据所有者协助。** 你可以定义 distribution 列表的共享文件夹，并对其进行配置，以便文件夹所有者接收电子邮件通知时，用户需要访问。 如果数据所有者不知道如何帮助的用户获取访问权限，的所有者可以将此信息转发到文件服务器管理员。  
  
-   **文件服务器管理员的帮助。** 当用户无法解决问题，并且不能帮助数据负责人，这种类型的协助才可用。  Windows Server 2012 提供位置文件服务器管理员可以对文件或文件夹查看用户有效的权限，以便更轻松访问的问题进行疑难解答用户界面。  
  
在 Windows Server 2012 的访问拒绝协助提供文件服务器管理员相关的访问权限的详细信息，以便他们可以确定该问题并相应的工具，以便他们可以进行配置更改，以满足访问请求。 例如，用户可能会使用他们当前不可以访问文件的访问以下过程：  
  
-   在用户尝试阅读 \\\financeshares 共享文件夹中的文件，但服务器显示拒绝访问消息。  
  
-    Windows Server 2012 显示为用户提供一个选项来申请协助访问拒绝协助信息。  
  
-   如果用户请求访问资源，由服务器将发送到文件夹所有者信息的访问权限请求使用电子邮件。  
  
你可以找到规划信息来配置中的访问拒绝协助[计划 Access-Denied 协助](assetId:///b169f0a4-8b97-4da8-ae4a-c8f1986d19e1)。  
  
你可以找到有关配置中的访问拒绝协助步骤[Deploy Access-Denied 协助 #40; 演示步骤 & #41;](Deploy-Access-Denied-Assistance--Demonstration-Steps-.md).  
  
## <a name="in-this-scenario"></a>在此情况下  
此项 scenario 是动态访问控制方案的一部分。 有关动态访问控制其他信息，请参阅：  
  
-   [动态访问控制：方案概述](Dynamic-Access-Control--Scenario-Overview.md)  
  
## <a name="practical-applications"></a>实际应用  
在 Windows Server 2012 访问拒绝获取帮助是会影响动态访问控制通过使用户可以请求访问直接从访问拒绝邮件共享的文件和文件夹。  
  
## <a name="BKMK_NEW"></a>此项 scenario 中所含功能  
下表列出了这种情况的一部分的功能，并介绍了如何支持。  
  
|功能|它如何支持此方案|  
|-----------|---------------------------------|  
|[文件服务器资源管理器概述](https://technet.microsoft.com/library/hh831701.aspx)|可以通过使用文件服务器上的文件服务器资源管理器控制台配置访问拒绝协助。|  
|[文件和存储服务概述](https://technet.microsoft.com/library/hh831487.aspx)|文件服务器资源管理器文件和存储服务的角色服务，并且组成的一组用于管理你的网络上的文件服务器的功能。|  
  


