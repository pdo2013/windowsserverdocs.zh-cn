---
title: 身份验证问题疑难解答
description: 本主题是指南使用 Windows Server 2016 中的 "使用 OTP 身份验证部署远程访问" 指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 71307757-f8f4-4f82-b8b3-ffd4fd8c5d6d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73fe8458910cbe7dfaf000a6546bcba9263a9683
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404308"
---
# <a name="troubleshooting-authentication-issues"></a>身份验证问题疑难解答

>适用于：Windows Server（半年频道）、Windows Server 2016

本主题包含与使用 OTP 身份验证尝试连接到 DirectAccess 时用户可能遇到的问题相关的问题的疑难解答信息。 DirectAccerss OTP 相关事件记录在客户端计算机上的 "**应用程序和服务日志/Microsoft/Windows/OtpCredentialProvider**" 下事件查看器。 请确保在排查 DirectAccess OTP 问题时启用此日志。  
  
## <a name="failed-to-access-the-ca-that-issues-otp-certificates"></a>未能访问颁发 OTP 证书的 CA  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 用户 <username> 的 OTP 证书注册在 CA 服务器上失败 < CA_name >，请求失败，可能的失败原因：无法解析 CA 服务器名称，无法通过第一个 DirectAccess 隧道访问 CA 服务器，或者无法建立与 CA 服务器的连接。  
  
**原因**  
  
用户提供了一个有效的一次性密码，并且 DirectAccess 服务器已对证书请求进行签名;但是，客户端计算机无法联系颁发 OTP 证书的 CA 来完成注册过程。  
  
**解决方案**  
  
在 DirectAccess 服务器上运行以下 Windows PowerShell 命令：  
  
1.  获取已配置的 OTP 颁发 Ca 列表，并检查 "CAServer" 的值： `Get-DAOtpAuthentication`  
  
2.  请确保将 Ca 配置为管理服务器： `Get-DAMgmtServer -Type All`  
  
3.  请确保客户端计算机已建立基础结构隧道：在 "高级安全 Windows 防火墙" 控制台中，展开 "**监视"/"安全关联**"，单击 "**主模式**"，并确保 IPsec 安全关联与 DirectAccess 的正确远程地址一起显示configuration.  
  
## <a name="directaccess-server-connectivity-issues"></a>DirectAccess 服务器连接问题  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）  
  
以下错误之一：  
  
-   使用基路径 < OTP_authentication_path > 和端口 < OTP_authentication_port > 无法建立与远程访问服务器 < DirectAccess_server_hostname > 的连接。 错误代码： < internal_error_code >。  
  
-   使用基本路径 < OTP_authentication_path > 和端口 < OTP_authentication_port >，无法将用户凭据发送到远程访问服务器 < DirectAccess_server_hostname >。 错误代码： < internal_error_code >。  
  
-   使用基路径 < OTP_authentication_path > 和端口 < OTP_authentication_port >，未从远程访问服务器 < > DirectAccess_server_hostname 收到响应。 错误代码： < internal_error_code >。  
  
**原因**  
  
由于网络问题或 DirectAccess 服务器上配置错误的 IIS 服务器，客户端计算机无法通过 Internet 访问 DirectAccess 服务器。  
  
**解决方案**  
  
请确保客户端计算机上的 Internet 连接正在工作，并确保 DirectAccess 服务正在运行，并且可通过 Internet 进行访问。  
  
## <a name="failed-to-enroll-for-the-directaccess-otp-logon-certificate"></a>未能注册 DirectAccess OTP 登录证书  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 来自 CA < CA_name > 的证书注册失败。 请求未按 OTP 签名证书所需的方式进行签名，或者用户没有注册的权限。  
  
**原因**  
  
用户提供的一次性密码是正确的，但证书颁发机构（CA）拒绝颁发 OTP 登录证书。 证书请求可能未正确地使用正确的 EKU （OTP 注册机构应用程序策略）进行签名，或者用户没有对 DA OTP 模板的 "注册" 权限。  
  
**解决方案**  
  
请确保 DirectAccess OTP 用户有权注册 DirectAccess OTP 登录证书，并确保已将正确的 "应用程序策略" 包含在 DA OTP 注册机构签名模板中。 另外，请确保远程访问服务器上的 DirectAccess 注册机构证书有效。 请参阅3.2 计划 OTP 证书模板和3.3 规划注册机构证书。  
  
## <a name="missing-or-invalid-computer-account-certificate"></a>计算机帐户证书缺失或无效  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。  无法完成 OTP 身份验证，因为在本地计算机证书存储中找不到 OTP 所需的计算机证书。  
  
**原因**  
  
DirectAccess OTP 身份验证需要客户端计算机证书才能与 DirectAccess 服务器建立 SSL 连接;但是，如果证书已过期，则找不到客户端计算机证书或该证书无效。  
  
**解决方案**  
  
请确保计算机证书存在并且有效：  
  
1.  在客户端计算机上的 MMC 证书控制台中，为本地计算机帐户打开 "**个人/证书**"。  
  
2.  请确保颁发的证书与计算机名称匹配，并双击该证书。  
  
3.  在 "**证书" 对话框的**"证书**状态**" 下的 "证书 **" 选项卡**中，确保 "证书状态" 显示为 "正常"。  
  
如果找不到有效的证书，则删除无效的证书（如果存在），然后通过从提升的命令提示符或重启客户端计算机上运行 @no__t 来重新注册计算机证书。  
  
## <a name="missing-ca-that-issues-otp-certificates"></a>缺少颁发 OTP 证书的 CA  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 无法完成 OTP 身份验证，因为 DA 服务器未返回发证 CA 的地址。  
  
**原因**  
  
没有颁发 OTP 证书的 Ca，或者所有颁发 OTP 证书的已配置的 Ca 都没有响应。  
  
**解决方案**  
  
1.  使用以下命令获取颁发 OTP 证书的 Ca 列表（CA 名称显示在 CAServer 中）： `Get-DAOtpAuthentication`。  
  
2.  如果未配置任何 Ca：  
  
    1.  使用命令 `Set-DAOtpAuthentication` 或远程访问管理控制台来配置颁发 DirectAccess OTP 登录证书的 Ca。  
  
    2.  应用新配置，并强制客户端通过从提升的命令提示符或重新启动客户端计算机上运行 @no__t 来刷新 DirectAccess GPO 设置。  
  
3.  如果配置了 Ca，请确保它们处于联机状态并响应注册请求。  
  
## <a name="misconfigured-directaccess-server-address"></a>DirectAccess 服务器地址配置错误  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 OTP 身份验证无法按预期完成。 无法确定远程访问服务器的名称或地址。  错误代码： < error_code >。 DirectAccess 设置应由服务器管理员验证。  
  
**原因**  
  
DirectAccess 服务器的地址配置不正确。  
  
**解决方案**  
  
使用 `Get-DirectAccess` 检查配置的 DirectAccess 服务器地址，并在地址配置错误时更正地址。  
  
请确保在客户端计算机上部署最新的设置，方法是在提升的命令提示符下运行 `gpupdate /force` 或重新启动客户端计算机。  
  
## <a name="failed-to-generate-the-otp-logon-certificate-request"></a>未能生成 OTP 登录证书请求  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 无法初始化 OTP 身份验证的证书请求。 无法生成私钥，或者用户 <username> 无法访问域控制器上的证书模板 < OTP_template_name >。  
  
**原因**  
  
导致此错误的可能原因有两个：  
  
-   用户没有读取 OTP 登录模板的权限。  
  
-   由于网络问题，用户的计算机无法访问域控制器。  
  
**解决方案**  
  
-   查看 OTP 登录模板上的权限设置，并确保为 DirectAccess OTP 预配的所有用户都具有 "读取" 权限。  
  
-   请确保域控制器已配置为管理服务器，并且客户端计算机可以通过基础结构隧道访问域控制器。 请参阅3.2 计划 OTP 证书模板。  
  
## <a name="no-connection-to-the-domain-controller"></a>没有到域控制器的连接  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 无法建立与用于 OTP 身份验证的域控制器的连接。 错误代码： < error_code >。  
  
**原因**  
  
导致此错误的可能原因有两个：  
  
-   用户的计算机没有网络连接。  
  
-   不能通过基础结构隧道访问域控制器。  
  
**解决方案**  
  
-   通过在 PowerShell 提示符下运行以下命令，确保域控制器配置为管理服务器： `Get-DAMgmtServer -Type All`。  
  
-   确保客户端计算机可以通过基础结构隧道访问域控制器。  
  
## <a name="otp-provider-requires-challengeresponse"></a>OTP 提供程序需要质询/响应  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 使用远程访问服务器的 OTP 身份验证（< DirectAccess_server_name >）为用户（<username>）要求用户提供质询。  
  
**原因**  
  
使用的 OTP 提供程序要求用户以 RADIUS 质询/响应交换的形式提供其他凭据，而 Windows Server 2012 DirectAccess OTP 不支持此方法。  
  
**解决方案**  
  
在任何情况下，将 OTP 提供程序配置为不要求质询/响应。  
  
## <a name="incorrect-otp-logon-template-used"></a>使用了错误的 OTP 登录模板  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 用户 <username> 请求证书的 CA 模板未配置为颁发 OTP 证书。  
  
**原因**  
  
已替换 DirectAccess OTP 登录模板，客户端计算机正在尝试使用较旧的模板进行身份验证。  
  
**解决方案**  
  
通过执行以下操作之一，确保客户端计算机使用最新的 OTP 配置：  
  
-   通过从提升的命令提示符运行以下命令来强制执行组策略更新： `gpupdate /Force`。  
  
-   重新启动客户端计算机。  
  
## <a name="missing-otp-signing-certificate"></a>缺少 OTP 签名证书  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）。 找不到 OTP 签名证书。 OTP 证书注册请求不能进行签名。  
  
**原因**  
  
在远程访问服务器上找不到 DirectAccess OTP 签名证书;因此，远程访问服务器无法对用户证书请求进行签名。 没有签名证书，或者签名证书已过期，未续订。  
  
**解决方案**  
  
在远程访问服务器上执行这些步骤。  
  
1.  运行 PowerShell cmdlet `Get-DAOtpAuthentication` 并检查 `SigningCertificateTemplateName` 的值，以检查配置的 OTP 签名证书模板名称。  
  
2.  使用 "证书" MMC 管理单元，确保计算机上存在从此模板注册的有效证书。  
  
3.  如果此类证书不存在，请删除过期的证书（如果存在），然后根据此模板注册新证书。  
  
若要创建 OTP 签名证书模板，请参阅3.3 计划注册机构证书。  
  
## <a name="missing-or-incorrect-upndn-for-the-user"></a>用户的 UPN/DN 缺失或不正确  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端事件日志）  
  
以下错误之一：  
  
-   无法通过 OTP 对用户 <username> 进行身份验证。 确保为 Active Directory 中的用户名定义了 UPN。 错误代码： < error_code >。  
  
-   无法通过 OTP 对用户 <username> 进行身份验证。 确保为 Active Directory 中的用户名定义 DN。 错误代码： < error_code >。  
  
**收到错误**（服务器事件日志）  
  
为 OTP 身份验证指定的用户名 <username> 不存在。  
  
**原因**  
  
用户未在用户帐户中正确设置用户主体名称（UPN）或可分辨名称（DN）属性，这些属性是正常运行 DirectAccess OTP 所必需的。  
  
**解决方案**  
  
使用域控制器上的 "Active Directory 用户和计算机" 控制台来验证是否为身份验证用户正确设置了这两个属性。  
  
## <a name="otp-certificate-is-not-trusted-for-login"></a>OTP 证书不受信任，无法登录  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**原因**  
  
颁发 OTP 证书的 CA 不在 enterprise NTAuth store 中;因此，注册的证书不能用于登录。 这可能发生在未建立跨域 CA 信任的多域和多林环境中。  
  
**解决方案**  
  
确保颁发 OTP 证书的 CA 层次结构的根证书安装在用户尝试对其进行身份验证的域的 enterprise NTAuth 证书存储中。  
  
## <a name="windows-could-not-verify-user-credentials"></a>Windows 无法验证用户凭据  
**方案**。 用户无法使用 OTP 进行身份验证，错误如下："由于内部错误，身份验证失败"  
  
**收到错误**（客户端计算机）。 Windows 验证你的凭据时出现问题。 请重试，或向管理员寻求帮助。  
  
**原因**  
  
当 DirectAccess OTP 登录证书不包含 CRL 时，Kerberos 身份验证协议不起作用。 DirectAccess OTP 登录证书不包括 CRL，原因如下：  
  
-   DirectAccess OTP 登录模板的配置选项**不包括颁发的证书中的吊销信息**。  
  
-   CA 配置为不发布 Crl。  
  
**解决方案**  
  
1.  若要确认此错误的原因，请在远程访问管理控制台的 "**步骤2远程访问服务器**" 中，单击 "**编辑**"，然后在 "**远程访问服务器安装**向导" 中，单击 " **OTP 证书模板**"。 记下用于注册用于 OTP 身份验证的证书的证书模板。 打开 "证书颁发机构" 控制台，在左窗格中单击 "**证书模板**"，然后双击 "OTP 登录证书" 查看证书模板属性。  
  
    若要解决此问题，请为 OTP 登录证书配置一个证书，并不要在 "模板属性" 对话框的 "**服务器**" 选项卡上选中 "**不在颁发的证书中包含吊销信息**" 复选框。  
  
2.  在 CA 服务器上，打开 "证书颁发机构" MMC，右键单击发证 CA，然后单击 "**属性**"。 在 "**扩展**" 选项卡上，确保已正确配置 CRL 发布。  
  


