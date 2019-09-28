---
ms.assetid: 04b63d9f-e924-4146-9b1d-785ed8b4239c
title: 规划与 AD FS 1.x 的互操作性
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e9f72bd83c90a804749329521a72e3232589c735
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407967"
---
# <a name="planning-for-interoperability-with-ad-fs-1x"></a>规划与 AD FS 1.x 的互操作性

Active Directory 联合身份验证服务 @no__t 0AD FS @ no__t; 运行 Windows Server®2012的联合服务器可以与带有 Windows Server 2003 R2 @ no__t 的 AD FS 1.0 2installed @no__t 和联合身份验证服务 1.1 AD FS-4installed 互操作Windows Server 2008 或 Windows Server 2008 R2 @ no__t-5 联合身份验证服务。 支持以下任何互操作性组合：  

-   任何 AD FS 1。*x*联合身份验证服务可以发送可由 Windows Server 2012 中的 AD FS 联合身份验证服务使用的声明。 有关详细信息，请参阅 [Checklist：将 AD FS 配置为使用 AD FS 1.x @ no__t 中的声明。  

-   Windows Server 2012 中的任何 AD FS 联合身份验证服务都可以发送 AD FS 1。*x*@no__t-可由 AD FS 1 使用的1compatible 声明。*x*联合身份验证服务。 有关详细信息，请参阅 [Checklist：配置 AD FS 以将声明发送到 AD FS 1.x 联合身份验证服务 @ no__t。  

-   Windows Server 2012 中的任何 AD FS 联合身份验证服务都可以发送 AD FS 1。*x*@no__t 1compatible 声明，可供运行 AD FS 1 的一台或多台 Web 服务器使用。*x*声明 @ no__t-3aware Web 代理。 有关详细信息，请参阅 [Checklist：配置 AD FS 以将声明发送到 AD FS 1.x 声明感知 Web 代理 @ no__t-0。  

> [!NOTE]  
> AD FS 不支持 AD FS 1 或与之进行互操作。基于*x* Windows NT 令牌的 Web 代理。  

AD FS 1。*x*@no__t 声明是可以由 Windows Server 2012 中的 AD FS 联合身份验证服务发送并由 AD FS 1 理解的声明。*x*联合身份验证服务。 AD FS 1。*x*联合身份验证服务可以使用 AD FS 联合身份验证服务发送的声明，必须发送名称标识符 @no__t 1ID @ no__t 声明类型。  

## <a name="understanding-the-name-id-claim-type"></a>了解名称 ID 声明类型  
名称 ID 声明类型等效于 AD FS 1.*x* 使用的标识声明类型。 每当要与 AD FS 1.*x*进行互操作时，都必须使用该类型。 名称 ID 声明类型启用 AD FS 1。*x*联合身份验证服务或 AD FS 1。*x*宣称 @ no__t-2aware Web Agent 使用 Windows Server 2012 发送的 AD FS 声明，前提是这些声明是使用下表中的名称 ID 格式之一发送的。  


|      名称 ID 格式       |               对应 URI                |
|---------------------------|------------------------------------------------|
| AD FS 1.*x* 电子邮件地址 | http://schemas.xmlsoap.org/claims/EmailAddress |
|   AD FS 1.*x* 电子邮件 UPN   |     http://schemas.xmlsoap.org/claims/UPN      |
|        公用名        |  http://schemas.xmlsoap.org/claims/CommonName  |
|           Group           |    http://schemas.xmlsoap.org/claims/Group     |

必须仅发送一个采用合适格式的名称 ID 声明。 满足该条件时，还可能会发送许多其他声明（假设它们符合表中描述的限制）。  

> [!NOTE]  
> AD FS 1。*x*联合身份验证服务只能解释以统一资源标识符开头的传入声明类型，\(URI @ no__t-2 的 @no__t。  

## <a name="see-also"></a>请参阅
[Windows Server 2012 中的 AD FS 设计指南](AD-FS-Design-Guide-in-Windows-Server-2012.md)
