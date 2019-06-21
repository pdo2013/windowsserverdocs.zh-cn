---
title: 身份验证问题疑难解答
description: 本主题是指南部署带有 OTP 身份验证，Windows Server 2016 中的远程访问的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 08dd6822cc30135506d82041cfbeab0bc1a058ab
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282357"
---
# <a name="troubleshooting-authentication-issues"></a>身份验证问题疑难解答

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题包含用户可能尝试连接到 DirectAccess 使用 OTP 身份验证时与问题相关的问题的疑难解答信息。 DirectAccerss OTP 相关的事件记录在事件查看器中的客户端计算机上**应用程序和服务日志/Microsoft/Windows/OtpCredentialProvider**。 请确保 DirectAccess OTP 的问题进行故障排除时，启用此日志。  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>无法访问颁发 OTP 证书的 CA  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 OTP 的用户的证书注册<username>CA 服务器 < CA_name > 上失败，请求失败，则可能失败的原因：无法解析 CA 服务器名、 不能通过第一个 DirectAccess 隧道访问 CA 服务器或无法建立与 CA 服务器的连接。  
  
**原因**  
  
用户提供有效的一次性密码和 DirectAccess 服务器签名证书请求;但是，客户端计算机无法联系 CA 颁发 OTP 证书完成注册过程。  
  
**解决方案**  
  
在 DirectAccess 服务器上，运行以下 Windows PowerShell 命令：  
  
1.  获取配置 OTP 发证 Ca 的列表，检查 CAServer 的值： `Get-DAOtpAuthentication`  
  
2.  请确保，将 Ca 配置为管理服务器： `Get-DAMgmtServer -Type All`  
  
3.  请确保客户端计算机已建立了基础结构隧道：在 Windows 防火墙高级安全控制台中，展开**监视/安全关联**，单击**主模式**，并确保用正确的远程出现的 IPsec 安全关联DirectAccess 配置的的地址。  
  
## <a name="directaccess-server-connectivity-issues"></a>DirectAccess 服务器连接问题  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）  
  
下列错误之一：  
  
-   无法连接到远程访问服务器 < DirectAccess_server_hostname > 使用基路径 < OTP_authentication_path > 和 < OTP_authentication_port > 端口。 错误代码： < internal_error_code >。  
  
-   用户凭据无法发送到远程访问服务器 < DirectAccess_server_hostname > 使用基路径 < OTP_authentication_path > 和 < OTP_authentication_port > 端口。 错误代码： < internal_error_code >。  
  
-   从远程访问服务器 < DirectAccess_server_hostname > 使用基路径 < OTP_authentication_path > 和 < OTP_authentication_port > 端口未收到响应。 错误代码： < internal_error_code >。  
  
**原因**  
  
客户端计算机无法访问 Internet，由于任一网络问题或到 DirectAccess 服务器上的错误配置 IIS 服务器上的 DirectAccess 服务器。  
  
**解决方案**  
  
请确保的客户端计算机上的 Internet 连接正常工作，并确保 DirectAccess 服务正在运行并且可访问 Internet 上。  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>未能完成对 DirectAccess OTP 登录证书注册  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 从 CA < CA_name > 证书注册失败。 请求未按预期的 OTP 签名证书，签名或用户没有注册的权限。  
  
**原因**  
  
由用户提供的一次性密码不正确，但拒绝颁发证书颁发机构 (CA) 颁发 OTP 登录证书。 证书请求可能不会正确登录与正确的 EKU （OTP 注册机构应用程序策略），或者用户没有 DA OTP 模板的"注册"权限。  
  
**解决方案**  
  
请确保 DirectAccess OTP 的用户有权注册 DirectAccess OTP 登录证书和正确的"应用程序策略"纳入 DA OTP 注册机构签名模板。 此外请确保远程访问服务器上的 DirectAccess 注册颁发机构证书有效。 请参阅 3.2 计划 OTP 证书模板和 3.3 计划注册证书颁发机构证书。  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>缺失或无效的计算机帐户证书  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。  无法完成 OTP 身份验证，因为在本地计算机证书存储中找不到所需的 OTP 的计算机证书。  
  
**原因**  
  
DirectAccess OTP 身份验证需要客户端计算机证书来建立与 DirectAccess 服务器; SSL 连接但是，客户端计算机证书未找到或无效，例如，如果证书已过期。  
  
**解决方案**  
  
请确保计算机证书存在且有效：  
  
1.  客户端计算机上，在 MMC 证书控制台的本地计算机帐户，打开**Personal/Certificates**。  
  
2.  请确保有证书颁发相匹配的计算机名称，然后双击该证书。  
  
3.  上**证书**对话框中，在**证书路径**选项卡上，在**证书状态**，请确保它显示"此证书是确定。"  
  
如果找不到有效的证书，删除无效的证书 （如果存在） 并重新注册计算机证书通过任一运行`gpupdate /Force`从提升的命令提示符或重新启动客户端计算机。  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>缺少颁发 OTP 证书的 CA  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 无法完成 OTP 身份验证，因为 DA 服务器未返回的颁发 CA 的地址。  
  
**原因**  
  
或者，不存在 Ca 颁发 OTP 证书配置，或者颁发 OTP 证书已配置 Ca 的所有已无响应。  
  
**解决方案**  
  
1.  使用以下命令获取的 Ca 颁发 OTP 证书 （CA 名称所示 CAServer） 的列表： `Get-DAOtpAuthentication`。  
  
2.  如果没有 Ca 配置：  
  
    1.  使用这两个命令`Set-DAOtpAuthentication`或远程访问管理控制台，以颁发 DirectAccess OTP 登录证书将 Ca 配置。  
  
    2.  应用新配置并强制客户端通过运行刷新 DirectAccess GPO 设置`gpupdate /Force`从提升的命令提示符或重新启动客户端计算机。  
  
3.  如果没有配置的 Ca，请确保它们联机并且当前响应注册请求。  
  
## <a name="misconfigured-directaccess-server-address"></a>错误配置的 DirectAccess 服务器地址  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 OTP 身份验证无法按预期完成。 无法确定的名称或地址的远程访问服务器。  错误代码： < error_code >。 由服务器管理员应验证 DirectAccess 设置。  
  
**原因**  
  
未正确配置 DirectAccess 服务器的地址。  
  
**解决方案**  
  
检查已配置的 DirectAccess 服务器地址使用`Get-DirectAccess`和更正地址，如果配置不正确。  
  
请确保最新的设置部署在客户端计算机上通过运行`gpupdate /force`从提升的命令提示符或重新启动客户端计算机。  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>未能生成 OTP 登录证书请求  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 无法初始化 OTP 身份验证的证书请求。 不能生成的私钥，或用户<username>无法访问域控制器上的证书模板 < OTP_template_name >。  
  
**原因**  
  
有两个可能的原因，此错误：  
  
-   用户没有读取 OTP 登录模板的权限。  
  
-   用户的计算机因网络问题而无法访问域控制器。  
  
**解决方案**  
  
-   查看 OTP 登录模板上设置的权限，请确保为 DirectAccess OTP 预配的所有用户具有都读取权限。  
  
-   请确保域控制器配置为管理服务器和客户端计算机可以通过基础结构隧道访问域控制器。 请参阅 3.2 计划 OTP 证书模板。  
  
## <a name="no-connection-to-the-domain-controller"></a>未连接到域控制器  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 无法建立与域控制器进行 OTP 身份验证的连接。 错误代码： < error_code >。  
  
**原因**  
  
有两个可能的原因，此错误：  
  
-   用户的计算机可以没有网络连接。  
  
-   域控制器无法访问通过基础结构隧道。  
  
**解决方案**  
  
-   请确保域控制器被配置为管理服务器通过从 PowerShell 提示符下运行以下命令： `Get-DAMgmtServer -Type All`。  
  
-   请确保客户端计算机都可以通过基础结构隧道连接域控制器。  
  
## <a name="otp-provider-requires-challengeresponse"></a>OTP 提供程序要求质询/响应  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 用户的远程访问服务器 (< DirectAccess_server_name >) 的 OTP 身份验证 (<username>) 要求用户质询。  
  
**原因**  
  
使用的 OTP 提供程序要求用户提供的 RADIUS 质询/响应交换，Windows Server 2012 DirectAccess OTP 不支持的窗体中的其他凭据。  
  
**解决方案**  
  
配置为不需要的任何方案中的质询/响应的 OTP 提供程序。  
  
## <a name="incorrect-otp-logon-template-used"></a>使用不正确 OTP 登录模板  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 中的用户的 CA 模板<username>请求证书未配置为颁发 OTP 证书。  
  
**原因**  
  
DirectAccess OTP 登录模板已更换，客户端计算机尝试使用较旧的模板进行身份验证。  
  
**解决方案**  
  
请确保客户端计算机使用最新的 OTP 配置通过执行以下项之一：  
  
-   通过从提升的命令提示符运行以下命令强制组策略更新： `gpupdate /Force`。  
  
-   重新启动客户端计算机。  
  
## <a name="missing-otp-signing-certificate"></a>缺少签名证书的 OTP  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）。 找不到 OTP 签名证书。 不能签名 OTP 证书注册请求。  
  
**原因**  
  
DirectAccess OTP 的远程访问服务器; 找不到签名证书因此，不能进行用户证书申请签名由远程访问服务器。 未签名的证书或签名证书已过期，不续订。  
  
**解决方案**  
  
远程访问服务器上执行这些步骤。  
  
1.  通过运行 PowerShell cmdlet 检查配置的 OTP 签名证书模板名称`Get-DAOtpAuthentication`并检查的值`SigningCertificateTemplateName`。  
  
2.  使用证书 MMC 管理单元中，以确保计算机上存在有效的证书从该模板注册。  
  
3.  如果不存在任何此类证书，删除过期的证书 （如果存在） 和注册基于此模板的新证书。  
  
创建进行签名的 OTP 证书模板看到 3.3 计划注册证书颁发机构证书。  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>缺少或不正确 UPN/DN 的用户  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端事件日志）  
  
下列错误之一：  
  
-   用户<username>不能使用 OTP 进行身份验证。 请确保 UPN Active Directory 中的用户名称定义。 错误代码： < error_code >。  
  
-   用户<username>不能使用 OTP 进行身份验证。 请确保 DN 定义 Active Directory 中的用户名。 错误代码： < error_code >。  
  
**收到错误**（服务器事件日志）  
  
用户名称<username>指定 OTP 身份验证不存在。  
  
**原因**  
  
用户不具有用户主体名称 (UPN) 或可分辨名称 (DN) 属性正确设置用户帐户中，这些属性都是必需的 DirectAccess OTP 正常工作。  
  
**解决方案**  
  
在域控制器上使用 Active Directory 用户和计算机控制台验证这些属性已正确设置为身份验证的用户。  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>OTP 证书不受信任可以进行登录  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**原因**  
  
颁发 OTP 证书的 CA 不在企业 NTAuth 存储中;因此，已注册的证书不能用于登录。 这可能尚未建立跨域信任的 CA 的多域和多林环境中。  
  
**解决方案**  
  
请确保企业 NTAuth 的证书存储到的用户尝试进行身份验证的域中安装颁发 OTP 证书的 CA 层次结构的根证书。  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows 无法验证用户凭据  
**方案**。 用户将失败并出现错误使用 OTP 进行身份验证："由于内部错误失败身份验证"  
  
**收到错误**（客户端计算机）。 Windows 已验证你的凭据时，出现了问题。 重试，或你的管理员寻求帮助。  
  
**原因**  
  
DirectAccess OTP 登录证书不包括 CRL 时，则 Kerberos 身份验证协议无效。 DirectAccess OTP 登录证书不包括 CRL，因为任一：  
  
-   DirectAccess OTP 登录模板已配置了选项**不包含在颁发的证书的吊销信息**。  
  
-   CA 配置为不将 Crl 发布。  
  
**解决方案**  
  
1.  若要确认在远程访问管理控制台中，此错误的原因**步骤 2 远程访问服务器**，单击**编辑**，然后在**远程访问服务器设置**向导中，单击**OTP 证书模板**。 记下用于进行 OTP 身份验证颁发的证书注册的证书模板。 打开证书颁发机构控制台中的，在左窗格中，单击**证书模板**，双击要查看证书模板属性的 OTP 登录证书。  
  
    若要解决此问题，配置 OTP 登录证书的证书，并且没有选择**不包含在颁发的证书的吊销信息**上的复选框**Server**模板的选项卡属性对话框。  
  
2.  在 CA 服务器上，打开证书颁发机构 MMC，右键单击颁发 CA，然后单击**属性**。 上**扩展**选项卡上确保正确配置了 CRL 发布。  
  


