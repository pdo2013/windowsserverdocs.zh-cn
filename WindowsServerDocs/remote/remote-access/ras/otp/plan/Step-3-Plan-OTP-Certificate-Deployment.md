---
title: 步骤 3 计划 OTP 证书部署
description: 本主题是指南部署带有 OTP 身份验证，Windows Server 2016 中的远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eca02eeb-d92d-463e-aae0-1f7038ba26fe
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb6be03ed5319a56f9507859e753c88e020ceea9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813868"
---
# <a name="step-3-plan-otp-certificate-deployment"></a>步骤 3 计划 OTP 证书部署

>适用于：Windows 服务器 （半年频道），Windows Server 2016

规划 RADIUS 服务器之后, 必须规划证书颁发机构 (CA) 要求，包括将颁发的一次性密码 (OTP) 证书，OTP 证书模板和使用远程的注册机构证书的 CA所有 DirectAccess 客户端 OTP 证书请求进行签名的访问服务器。 使用这些证书，如下所示：  
  
1.  DirectAccess 客户端请求一个 OTP 的证书，并远程访问服务器接收请求。  
  
2.  远程访问服务器将验证输入 OTP 凭据和它们是否有效，则服务器将充当注册机构并签署 OTP 证书注册请求使用的是生存期较短的签名证书。  
  
3.  远程访问服务器将已签名的证书注册请求发送回 DirectAccess 客户端  
  
4.  客户端注册中使用由服务器签名证书注册请求的 CA 的 OTP 证书。  
  
5.  CA 验证的凭据，并请求。  
  
|任务|描述|  
|----|--------|  
|[3.1 计划 OTP CA](#bkmk_3_1_CA)|计划使用证书颁发给 DirectAccess 客户端进行 OTP 身份验证证书颁发机构 (CA)。|  
|[3.2 计划 OTP 证书模板](#bkmk_3_2_OTP_Cert)|计划 OTP 证书模板。|
|[3.3 计划注册证书颁发机构证书](#bkmk_33RACert)|规划所有 OTP 身份验证证书请求进行签名的注册颁发机构证书。|

## <a name="bkmk_3_1_CA"></a>3.1 计划 OTP CA  
若要使用一次性密码身份验证 (OTP) 部署 DirectAccess，您需要内部 CA 向 DirectAccess 客户端计算机颁发 OTP 身份验证证书。 为此，可以使用相同的内部 CA 用于颁发证书的使用进行常规 IPsec 计算机身份验证。  
  
## <a name="bkmk_3_2_OTP_Cert"></a>3.2 计划 OTP 证书模板  
每个 DirectAccess 客户端才能访问内部网络需要 OTP 身份验证证书。 你必须将模板配置 OTP 证书在内部 CA 上。 配置 OTP 的证书模板时，请注意以下事项：  
  
-   所有用户都需要进行 OTP 身份验证必须都具有读取和注册权限才能使用此模板。  
  
-   应从 Active Directory 信息，以确保使用者名称匹配的 OTP 用户名，而不是执行证书请求的远程访问服务器的名称生成使用者名称。 使用者名称必须是完全可分辨的名称格式，并且使用者可选名称必须以 UPN 格式。 这可确保已注册的 OTP 证书可用于智能卡 Kerberos 身份验证。  
  
-   证书的预期的用途必须是智能卡登录  
  
-   颁发必须要求授权的签名。 签名必须与注册颁发机构签名的证书模板中设置预定义 DirectAccess OTP 应用程序策略配置。  
  
-   有效期限应设置为一小时。  
  
    > [!NOTE]  
    > 在其中 CA 服务器是 Windows Server 2003 的计算机，则必须在另一台计算机上配置了模板的情况下。 这是由于设置**有效期**以小时为单位，则无法运行 Windows 2008/Vista 之前的版本。 如果使用配置的模板的计算机不具有证书服务角色安装，或是客户端计算机，你可能需要安装证书模板管理单元。 此主题的详细信息，请单击[此处](https://technet.microsoft.com/library/cc732445.aspx)。  
  
-   续订期应设置为 0。  
  
-   （可选）证书和请求不应存储在 CA 数据库。  
  
-   证书增强型密钥用法参数必须设置正确，按如下所示：  
  
    -   对于 DirectAccess 注册签名证书模板使用密钥 1.3.6.1.4.1.311.81.1.1。  
  
    -   对于 OTP 身份验证证书模板使用密钥 1.3.6.1.4.1.311.20.2.2 密钥。  
  
## <a name="bkmk_33RACert"></a>3.3 计划注册证书颁发机构证书  
当 DirectAccess 客户端请求的 OTP 证书时，远程访问服务器将接收从客户端请求。 远程访问服务器对所有使用注册机构证书的客户端的 OTP 证书申请进行签名。 在 CA 颁发证书，仅当由远程访问服务器上的注册颁发机构证书签名请求。 必须由内部 CA 颁发证书，不能为自签名证书。 不需要由颁发 OTP 证书的 CA 颁发但颁发 OTP 证书的 CA 必须信任颁发注册颁发机构签名证书的 CA。  
  
## <a name="BKMK_Links"></a>另请参阅  
  
-   [步骤 4：OTP 规划远程访问服务器](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  


