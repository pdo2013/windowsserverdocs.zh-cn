---
title: 计划多林部署
description: 本主题是指南的 Windows Server 2016 中的多林环境中部署远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8acc260f-d6d1-4d32-9e3a-1fd0b2a71586
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a2f14fdb2fd3ab6f0a89c8d8c1a8853041dcba94
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281013"
---
# <a name="plan-a-multi-forest-deployment"></a>计划多林部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍在多林部署中配置远程访问所需的计划步骤。  
  
## <a name="prerequisites"></a>先决条件  
在开始部署此方案之前，请查看此列表以了解重要要求：  
  
-   需要双向信任。  
  
## <a name="plan-trust-between-forests"></a>计划林之间的信任  
如果你决定要启用对新林中资源的访问，允许来自新林的客户端使用 DirectAccess，或者将新林的远程访问服务器作为入口点添加到远程访问部署中，那么你必须确保在两个林之间配置完全信任（即双向可传递信任），请参阅 [信任类型](https://technet.microsoft.com/library/cc775736.aspx)。 若要多林部署允许管理员执行编辑新林中的 GPO、使用新林中的安全组作为客户端安全组、远程调用（WinRM、RPC）新林中的计算机以及对新林的远程客户端进行身份验证等操作，必须先配置林之间的完全信任。  
  
## <a name="plan-remote-access-administrator-permissions"></a>计划远程访问管理员权限  
当你配置远程访问时，它会在每个包含远程访问服务器或客户端的域中更新或创建 GPO。 在多林环境中，情况与单林环境相同，远程访问管理员必须拥有写入和修改 DirectAccess GPO 及其安全筛选器的权限和（可选）在所涉及的所有林中为 DirectAccess GPO 创建链接的权限。 不论远程访问管理员属于哪个林，都需要拥有这些权限。  
  
此外，远程访问管理员必须是所有远程访问服务器（包括新林中被添加作为原远程访问部署的入口点的远程访问服务器）的本地管理员。  
  
## <a name="ClientSG"></a>计划客户端安全组  
你至少必须在新林中为该林的 DirectAccess 客户端计算机创建一个安全组。 这是因为一个安全组不能包含多个林的帐户。  
  
> [!NOTE]  
> -   DirectAccess 需要至少一个 Windows 10&reg;或 Windows&reg; 8 客户端安全组的每个林。 但是，建议使用一个 Windows 10 或 Windows 8 客户端安全组为包含 Windows 10 或 Windows 8 客户端的每个域。  
> -   DirectAccess 多站点时，需要至少一个 Windows 7&reg;每个支持的 Windows 7 客户端计算机每个 DirectAccess 入口点的林的客户端安全组。 但是，建议要有为包含 Windows 7 客户端的每个域的每个入口点的单独 Windows 7 客户端安全组。  
>   
> 如果要在添加的域中的客户端计算机上应用 DirectAccess，则必须在这些域中创建客户端 GPO。 添加安全组会导致为新域写入新的客户端 GPO；因此，如果你将新域中的新安全组添加到 DirectAccess 客户端安全组列表中，客户端 GPO 会自动在新域上生成，并且新域的客户端计算机将通过该客户端 GPO 获取 DirectAccess 设置。  
>   
> 请注意，如果你将新域的客户端添加到已经是 DirectAccess 客户端安全组的现有安全组中，DirectAccess 将不会自动在新域上创建客户端 GPO。 新域中的客户端将不会接收 DirectAccess 设置，并且无法使用 DirectAcecss 进行连接。  
  
## <a name="plan-certification-authorities"></a>计划证书颁发机构  
如果 DirectAccess 部署配置为使用一次性密码 (OTP) 身份验证，则各个林具有相同的签名证书模板，但这些模板的 Oid 值不同。 这导致无法将林配置为一个单一配置单元。 若要解决此问题，并在多林环境中配置 OTP，请参阅主题中的"配置在多林部署中的 OTP"一节[配置多林部署](Configure-a-Multi-Forest-Deployment.md)。  
  
当使用 IPsec 计算机证书身份验证时，不管客户端和服务器计算机属于哪个林，它们都必须拥有同一根或中间证书颁发机构颁发的计算机证书。  
  
## <a name="plan-otp-exemptions"></a>计划 OTP 免除  
如果你使用的是 DirectAccess OTP 身份验证，请注意，OTP 免除安全组对于单个林的用户是有限制的。 这是因为一个安全组只能包含来自单个林的用户，并且只能配置一个这样的安全组。  
  


