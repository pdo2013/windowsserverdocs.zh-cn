---
title: 步骤3规划 OTP 证书部署
description: 本主题是指南使用 Windows Server 2016 中的 "使用 OTP 身份验证部署远程访问" 指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8406286599e5b03173ce1b5d6c34c35245a9094
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366949"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>步骤3规划 OTP 证书部署

>适用于：Windows Server（半年频道）、Windows Server 2016

在规划 RADIUS 服务器后，你必须规划证书颁发机构（CA）要求，包括将颁发一次性密码（OTP）证书的 CA、OTP 证书模板和远程使用的注册机构证书。访问服务器来对所有 DirectAccess 客户端 OTP 证书请求进行签名。 这些证书的使用方式如下：  
  
1.  DirectAccess 客户端请求 OTP 证书，远程访问服务器接收该请求。  
  
2.  远程访问服务器验证 OTP 凭据，如果它们有效，服务器将充当注册机构，并使用简短的签名证书对 OTP 证书注册请求进行签名。  
  
3.  远程访问服务器将签名证书注册请求发送回 DirectAccess 客户端  
  
4.  然后，客户端使用服务器签名的证书注册请求从 CA 注册 OTP 证书。  
  
5.  CA 将验证凭据和请求。  
  
|任务|描述|  
|----|--------|  
|[3.1 计划 OTP CA](#bkmk_3_1_CA)|规划证书颁发机构（CA），以用于为 OTP 身份验证的 DirectAccess 客户端颁发证书。|  
|[3.2 计划 OTP 证书模板](#bkmk_3_2_OTP_Cert)|规划 OTP 证书模板。|
|[3.3 规划注册机构证书](#bkmk_33RACert)|规划注册机构证书以签署所有 OTP 身份验证证书申请。|

## <a name="bkmk_3_1_CA"></a>3.1 计划 OTP CA  
若要使用一次性密码身份验证（OTP）部署 DirectAccess，需要使用内部 CA 向 DirectAccess 客户端计算机颁发 OTP 身份验证证书。 出于此目的，你可以使用用于颁发证书的相同内部 CA，以便进行常规 IPsec 计算机身份验证。  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3.2 计划 OTP 证书模板  
每个 DirectAccess 客户端都需要一个 OTP 身份验证证书才能获得对内部网络的访问权限。 必须在内部 CA 上为 OTP 证书配置模板。 配置 OTP 证书模板时，请注意以下事项：  
  
-   所有需要执行 OTP 身份验证的用户都必须具有此模板的 "读取" 和 "注册" 权限。  
  
-   使用者名称应从 Active Directory 信息生成，以确保使用者名称与 OTP 用户名匹配，而不是执行证书请求的远程访问服务器的名称。 "使用者名称" 必须为完全可分辨名称格式，"使用者备用名称" 必须为 UPN 格式。 这可以确保已注册的 OTP 证书对智能卡 Kerberos 身份验证有效。  
  
-   证书的预期用途必须是智能卡登录  
  
-   颁发必须需要一个授权签名。 签名必须在 "注册机构签名证书" 模板中设置预定义的 "DirectAccess OTP 应用程序策略"。  
  
-   有效期应设置为一小时。  
  
    > [!NOTE]  
    > 在 CA 服务器是 Windows Server 2003 计算机的情况下，必须在另一台计算机上配置模板。 这是因为，在运行 2008/Vista 之前的 Windows 版本时，不能以小时为单位设置**有效期**。 如果用于配置模板的计算机未安装证书服务角色，或者它是一台客户端计算机，则可能需要安装 "证书模板" 管理单元。 有关此主题的详细信息，请单击[此处](https://technet.microsoft.com/library/cc732445.aspx)。  
  
-   续订期应设置为0。  
  
-   可有可无不应将证书和请求存储在 CA 数据库中。  
  
-   必须正确设置证书增强型密钥用法参数，如下所示：  
  
    -   对于 "DirectAccess 注册签名证书" 模板，请使用密钥1.3.6.1.4.1.311.81.1.1。  
  
    -   对于 OTP 身份验证证书模板，请使用密钥1.3.6.1.4.1.311.20.2.2 密钥。  
  
## <a name="bkmk_33RACert"></a>3.3 规划注册机构证书  
当 DirectAccess 客户端请求 OTP 证书时，远程访问服务器将接收来自客户端的请求。 远程访问服务器使用注册机构证书对来自客户端的所有 OTP 证书请求进行签名。 仅当远程访问服务器上的注册机构证书对请求进行签名时，CA 才会颁发证书。 证书必须由内部 CA 颁发，证书不能是自签名证书。 它不必由颁发 OTP 证书的 CA 颁发，但颁发 OTP 证书的 CA 必须信任颁发注册机构签名证书的 CA。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 4：为远程访问服务器计划 OTP @ no__t-0  
  


