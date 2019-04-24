---
title: 部署带有 OTP 身份验证的远程访问
description: 本主题是指南部署带有 OTP 身份验证，Windows Server 2016 中的远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecb4503c6f8cbc08f2175a33a41929491e4a2b7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811988"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>部署带有 OTP 身份验证的远程访问

>适用于：Windows 服务器 （半年频道），Windows Server 2016

 Windows Server 2016 和 Windows Server 2012 将 DirectAccess 以及路由和远程访问服务\(RRAS\) VPN 到单个远程访问角色。   

## <a name="BKMK_OVER"></a>应用场景说明  
在此方案中远程访问启用了 DirectAccess 服务器配置为使用两个的 DirectAccess 客户端用户进行身份验证\-身份一次性密码\(OTP\)身份验证，除了标准活动目录的凭据。  
  
## <a name="prerequisites"></a>先决条件  
在开始部署此方案之前，请查看此列表以了解重要要求：  
  
-   [部署单个 DirectAccess 服务器使用高级设置](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)部署 OTP 之前，必须部署。  
  
-   Windows 7 客户端必须使用 DCA 2.0 以支持 OTP。  
  
-   OTP 不支持更改 PIN 更改。  
  
-   必须部署公钥基础结构。  
  
    有关详细信息，请参阅：[测试实验室指南微型模块：适用于 Windows Server 2012 的基本 PKI。](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   不支持 DirectAccess 管理控制台或 Windows PowerShell cmdlet 之外更改策略。  
  
## <a name="in-this-scenario"></a>本方案内容  
OTP 身份验证方案包含许多步骤：  
  
1.  [部署单个 DirectAccess 服务器使用高级设置](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。 配置 OTP 之前，必须部署单个远程访问服务器。 计划和部署单台服务器包括设计和配置网络拓扑、计划和部署证书、设置 DNS 和 Active Directory、配置远程访问服务器设置、部署 DirectAccess 客户端以及准备 Intranet 服务器。  
  
2.  [规划使用 OTP 身份验证的远程访问](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication)。 除了所需的一台服务器的规划，OTP 还要求计划 Microsoft 证书颁发机构\(CA\) OTP; 和 RADIUS 证书模板和\-启用 OTP 服务器。 计划可能还包括必需的安全组，以免除特定用户接受强\(OTP 或智能卡\)身份验证。 有关在多中配置 OTP 的信息\-林的环境，请参阅[配置多林部署](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md)。  
  
3.  [带有 OTP 身份验证配置 DirectAccess](/configure/Configure-RA-with-OTP-Authentication.md)。 OTP 部署由多个配置步骤，包括为 OTP 身份验证，配置 OTP 服务器，在远程访问服务器上，配置 OTP 设置以及更新 DirectAccess 客户端设置准备基础结构组成。  
  
4.  [Troubleshoot an OTP Deployment]((/troubleshoot/Troubleshoot-an-OTP-Deployment.md)。 此故障排除部分介绍了大量部署带有 OTP 身份验证的远程访问时可能出现的最常见错误。  
  
## <a name="BKMK_APP"></a>实际应用程序  
增加安全使用 OTP，提高 DirectAccess 部署的安全性。 用户需要 OTP 凭据才能访问内部网络。 用户提供 OTP 凭据通过网络连接的 Windows 10 或 Windows 8 客户端计算机上，或通过使用 DirectAccess 连接助手中可用的工作区连接\(DCA\)上运行的客户端计算机Windows 7。 OTP 身份验证过程包括以下步骤：  
  
1.  DirectAccess 客户端输入域凭据来访问 DirectAccess 基础结构服务器\(通过基础结构隧道\)。  如果由于特定的 IKE 故障导致内部网络的连接不可用，则客户端计算机上的工作区连接将通知用户需要输入凭据。 客户端计算机上运行的 Windows 7，pop\-向上请求会出现智能卡凭据。  
  
2.  在输入 OTP 凭据后，它们通过 SSL 发送到远程访问服务器，一小段连同\-术语智能卡登录证书。  
  
3.  远程访问服务器启动了使用 RADIUS 的 OTP 凭据验证\-基于 OTP 服务器。  
  
4.  如果通过验证，远程访问服务器会使用其注册机构证书对证书请求签名，并将其发送回 DirectAccess 客户端计算机  
  
5.  DirectAccess 客户端计算机将签名的证书请求转发到 CA 并存储注册的证书以供 Kerberos SSP\/接入点。  
  
6.  使用本证书，客户端计算机能够以透明方式执行标准的智能卡 Kerberos 身份验证。  
  
## <a name="BKMK_NEW"></a>在此方案中包括角色和功能  
下表列出了本方案所需的角色和功能：  
  
|角色\/功能|如何支持本方案|  
|---------|-----------------|  
|*远程访问管理角色*|该角色可使用服务器管理器控制台加以安装和卸载。 此角色包括 DirectAccess，以前是 Windows Server 2008 R2，以及路由和远程访问服务以前是网络策略下的角色服务和访问服务中的功能\(NPAS\)服务器角色。 远程访问角色由以下两个组件组成：<br /><br />1.DirectAccess 以及路由和远程访问服务\(RRAS\) VPN DirectAccess 和 VPN 由远程访问管理控制台中一起管理。<br />2.在传统路由和远程访问控制台中管理 RRAS 路由 — RRAS 路由功能。<br /><br />远程访问角色取决于以下服务器功能：<br /><br />-Internet Information Services \(IIS\) Web 服务器-此功能是需要配置网络位置服务器、 使用 OTP 身份验证，并配置默认 web 探测。<br />的远程访问服务器上的本地帐户的 Windows 内部 Database-Used。|  
|远程访问管理工具功能|此功能的安装如下所述：<br /><br />-如果远程访问角色安装，并且支持远程管理控制台用户界面它是默认情况下，远程访问服务器上安装。<br />-它可以在不运行远程访问服务器角色的服务器上选择性地安装。 在这种情况下，它可用于远程管理运行 DirectAccess 和 VPN 的远程访问计算机。<br /><br />远程访问管理工具功能包括以下各项：<br /><br />-远程访问 GUI 和命令行工具<br />的 Windows PowerShell 远程访问模块<br /><br />依赖项包括：<br /><br />组策略管理控制台<br />-RAS 连接管理器管理工具包\(CMAK\)<br />-   Windows PowerShell 3.0<br />图形管理工具和基础结构|  
  
## <a name="BKMK_HARD"></a>硬件要求  
本方案的硬件要求包括以下各项：  
  
-   满足 Windows Server 2016 或 Windows Server 2012 的硬件要求的计算机。  
  
-   为测试该方案，至少一台计算机运行 Windows 10、 Windows 8 或 Windows 7 配置为 DirectAccess 客户端是必需的。  
  
-   通过 RADIUS 支持 PAP 的 OTP 服务器。  
  
-   OTP 硬件或软件令牌。  
  
## <a name="BKMK_SOFT"></a>软件要求  
存在许多对本方案的要求。  
  
1.  对单台服务器部署的软件要求。 有关详细信息，请参阅[部署单个 DirectAccess 服务器使用高级设置](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)。  
  
2.  除了单个服务器的软件要求有大量的 OTP\-特定要求：  
  
    1.  IPsec 身份验证中的 CA 必须使用 IPsec 部署 DirectAccess OTP 部署计算机由 CA 颁发的证书。 OTP 部署不支持将远程访问服务器用作 Kerberos 代理的 IPsec 身份验证。 需要内部证书颁发机构。  
  
    2.  OTP 身份验证的 Microsoft 企业 CA 的 CA\(运行 Windows 2003 服务器上或更高版本\)颁发 OTP 客户端证书所需。 可采纳颁发用于 IPsec 身份验证的证书的相同证书颁发机构。 必须可通过第一个基础结构隧道访问 CA 服务器。  
  
    3.  安全组-若要不对用户进行强身份验证，包含这些用户的 Active Directory 安全组是必需的。  
  
    4.  客户端\-端的要求-适用于 Windows 10 和 Windows 8 客户端计算机，网络连接助手\(NCA\)服务用于检测是否需要输入 OTP 凭据。 如果它们是 DirectAccess 媒体管理器会提示输入凭据。  NCA 包含在操作系统，并且没有安装或部署是必需的。 对于 Windows 7 客户端计算机，DirectAccess 连接助手\(DCA\) 2.0 是必需的。 可从 [Microsoft 下载中心](https://www.microsoft.com/download/details.aspx?id=29039)下载该服务。  
  
    5.  请注意以下事项：  
  
        1.  可以并行使用智能卡和受信任的平台模块使用 OTP 身份验证\(TPM\)\-基于身份验证。 在远程访问管理控制台中启用 OTP 身份验证的同时还将启用智能卡身份验证。  
  
        2.  过程中指定的安全远程访问配置用户组可以是从两个中免除\-因素身份验证，并因此验证用户名\/仅密码。  
  
        3.  不支持 OTP 新的 PIN 和下一个令牌代码模式  
  
        4.  在远程访问多站点部署中，OTP 设置是通用的，并是所有入口点的标识。 如果为 OTP 配置多台 RADIUS 或颁发证书机构服务器，则由每台远程访问服务器根据可用性和可伸缩性对这些服务器进行排序。  
  
        5.  在远程访问多中配置 OTP 时\-林环境中，OTP Ca 只能来自资源林，并且应跨林信任配置证书注册。 有关详细信息，请参阅[AD CS:与 Windows Server 2008 R2 的跨林证书注册](https://technet.microsoft.com/library/ff955842.aspx)。  
  
        6.  使用 KEY FOB OTP 令牌的用户应插入 PIN 并按代码当中\(无需任何逗号\)DirectAccess OTP 对话中。 使用 PIN PAD OTP 的用户应仅在对话的令牌代码当中插入 PIN。  
  
        7.  启用 WEBDAV 时不应启用 OTP。  
  
## <a name="KnownIssues"></a>已知的问题  
下面是配置 OTP 方案时的已知问题：  
  
-   远程访问使用探测机制来验证到 RADIUS 连接\-基于 OTP 服务器。 在某些情况下，这可能会导致在 OTP 服务器上引发错误。 为了避免此问题，请在 OTP 服务器上执行以下操作：  
  
    -   创建一个用户帐户，该帐户与在远程访问服务器上为探测机制配置的用户名和密码匹配。 用户名不应定义 Active Directory 用户。  
  
        默认情况下，远程访问服务器上的用户名为 DAProbeUser，密码为 DAProbePass。 可使用远程访问服务器上注册表中的以下值来修改这些默认设置：  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\RadiusProbeUser  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\ RadiusProbePass  
  
-   如果更改已配置且正在运行的 DirectAccess 部署中的 IPsec 根证书，OTP 将停止工作。 若要解决此问题，在每台 DirectAccess 服务器，在 Windows PowerShell 提示符处，运行命令： `iisreset`  
  
