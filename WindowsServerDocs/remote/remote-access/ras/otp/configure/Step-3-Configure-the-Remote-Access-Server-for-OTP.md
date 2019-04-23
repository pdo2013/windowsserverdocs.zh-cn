---
title: 步骤 3： 为 OTP 配置远程访问服务器
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
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5e192c062e8ecd18128109321058ddc58b6b1e4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839838"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>步骤 3： 为 OTP 配置远程访问服务器

>适用于：Windows 服务器 （半年频道），Windows Server 2016

RADIUS 服务器配置与软件分发令牌之后，通信的端口已打开、 已创建一个共享的机密，RADIUS 服务器上已创建与 Active Directory 对应的用户帐户和远程访问服务器具有已配置为 RADIUS 身份验证代理，则远程访问服务器必须配置为支持 OTP。  
  
|任务|描述|  
|----|--------|  
|[3.1 要不对用户进行 OTP 身份验证 （可选）](#BKMK_Exempt)|如果带有 OTP 身份验证，将从 DirectAccess 免除特定用户，然后执行以下基本步骤。|  
|[3.2 配置远程访问服务器以支持 OTP](#BKMK_Config)|远程访问服务器上更新远程访问配置以支持 OTP 双因素身份验证。|  
|[3.3 其他授权智能卡](#BKMK_Smartcard)|有关使用智能卡的其他信息。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_Exempt"></a>3.1 要不对用户进行 OTP 身份验证 （可选）  
如果特定的用户，则在 OTP 身份验证也如此，然后必须在远程访问配置之前执行以下步骤：  
  
> [!NOTE]  
> 你必须等待复制完成时配置的 OTP 免除组中的域之间。  
  
#### <a name="create-user-exemption-security-group"></a>创建用户例外安全组  
  
1.  为目的 OTP 免除 Active Directory 中创建安全组。  
  
2.  添加的所有用户免于 OTP 身份验证添加到安全组。  
  
    > [!NOTE]  
    > 请确保仅包含用户帐户和不是计算机帐户，OTP 免除安全组中。  
  
## <a name="BKMK_Config"></a>3.2 配置远程访问服务器以支持 OTP  
若要配置远程访问使用双因素身份验证和 OTP 的 RADIUS 服务器和前一节的证书部署，请使用以下步骤：  
  
#### <a name="configure-remote-access-for-otp"></a>为 OTP 配置远程访问  
  
1.  打开**远程访问管理**然后单击**配置**。  
  
2.  在中**DirectAccess 安装**窗口下**步骤 2-远程访问服务器**，单击**编辑**。  
  
3.  单击**下一步**三次，然后在**身份验证**部分，选择这两**双因素身份验证**并**使用 OTP**，并确保该**使用计算机证书**检查。  
  
    > [!NOTE]  
    > 如果取消选择禁用 OTP 的远程访问服务器上，启用 OTP 后**使用 OTP**，将在服务器上卸载 ISAPI 和 CGI 扩展。  
  
4.  如果需要 Windows 7 支持，则选择**使 Windows 7 客户端计算机能够通过 DirectAccess 进行连接**复选框。 注意：如在规划部分中所述，Windows 7 客户端必须安装以支持使用 OTP 的 DirectAccess 的 DCA 2.0。  
  
5.  单击“下一步” 。  
  
6.  在中**OTP RADIUS 服务器**部分中，双击空白**服务器名称**字段。  
  
7.  在中**添加 RADIUS 服务器**对话框中，键入 RADIUS 服务器的名称**服务器名称**字段。 单击**更改**旁边**共享机密**字段，并键入配置中的 RADIUS 服务器时使用的相同密码**新机密**和**确认新的机密**字段。 单击**确定**两次，然后单击**下一步**。  
  
    > [!NOTE]  
    > 如果 RADIUS 服务器位于域不同于远程访问服务器，则**服务器名称**字段必须指定 RADIUS 服务器的 FQDN。  
  
8.  在中**OTP CA 服务器**部分，选择 CA 服务器以用于注册 OTP 客户端身份验证证书，然后单击**添加**。 单击“下一步” 。  
  
9. 在中**OTP 证书模板**部分中，单击**浏览**选择用于进行 OTP 身份验证颁发的证书注册的证书模板。  
  
    > [!NOTE]  
    > 如果不使用"不包含吊销信息中颁发的证书"选项，必须配置 OTP 证书由企业 CA 颁发的证书模板。 如果在证书模板创建期间选择此选项，然后 OTP 客户端计算机将无法正确登录。  
  
    单击**浏览**选择用于注册远程访问服务器使用 OTP 证书注册请求进行签名的证书的证书模板。 单击 **“确定”**。 单击“下一步” 。  
  
10. 如果豁免某些特定用户使用 OTP 的 DirectAccess 的方式是必需的然后在**OTP 免除**部分中，选择**不要求使用双因素身份验证进行身份验证的指定的安全组中的用户**. 单击**安全组**，然后选择已创建的 OTP 免除安全组。  
  
11. 上**远程访问服务器设置**页上，单击**完成**。  
  
12. 在中**DirectAccess 安装**窗口下**第 3 步-基础结构服务器**，单击**编辑**。  
  
13. 单击**下一步**两次，然后在**管理**部分中双击**管理服务器**字段。  
  
14. 输入**计算机名**或**地址**的 CA 服务器的配置为颁发 OTP 证书，然后单击**确定**。  
  
15. 在中**远程访问设置**窗口中单击**完成**。  
  
16. 单击**完成**上**DirectAccess 专家向导**。  
  
17. 上**远程访问评审**对话框中，单击**应用**，等待 DirectAccess 策略更新，然后单击**关闭**。  
  
18. 上**启动**屏幕上，键入**powershell.exe**，右键单击**powershell**，单击**高级**，然后单击**以运行管理员**。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
19. 在 Windows PowerShell 窗口中，键入**gpupdate /force**然后按 ENTER。  
  
若要使用 PowerShell 命令的 OTP 配置远程访问：  
  
![Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**Windows PowerShell 等效命令**  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要配置上当前使用计算机证书身份验证的部署使用双因素身份验证的远程访问权限：  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
若要配置远程访问权限以使用 OTP 身份验证使用以下设置：  
  
-   名为 OTP.corp.contoso.com 的 OTP 服务器。  
  
-   CA 服务器名为 APP1.corp.contoso.com\corp-APP1-CA1。  
  
-   名为 DAOTPLogon 用于进行 OTP 身份验证颁发的证书注册的证书模板。  
  
-   名为 DAOTPRA 的证书模板用于注册远程访问服务器使用 OTP 证书注册请求进行签名的注册机构证书。  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
执行 PowerShell 命令在完成步骤 12-19 日之前的步骤来配置远程访问服务器以支持 OTP。  
  
> [!NOTE]  
> 请务必验证已应用的 OTP 设置远程访问服务器上之前添加一个入口点。  
  
## <a name="BKMK_Smartcard"></a>3.3 其他授权智能卡  
在步骤 2 远程访问设置向导中的身份验证页上，你可以要求访问内部网络将智能卡。 选中此选项后，远程访问设置向导配置 intranet 隧道的 IPsec 连接安全规则以要求使用智能卡进行隧道模式授权在 DirectAccess 服务器上。 隧道模式授权允许您指定的只有经过授权的计算机或用户可以建立入站的隧道。  
  
若要为 intranet 隧道 IPsec 隧道模式授权使用智能卡，您必须部署公钥基础结构 (PKI) 与智能卡基础结构。  
  
因为 DirectAccess 客户端的 intranet 访问使用智能卡，您还可以使用身份验证机制保证，Windows Server 2008 R2 的一项功能来控制访问资源，例如文件、 文件夹和打印机，根据是否在使用基于智能卡的证书登录的用户。 身份验证机制保证要求 Windows Server 2008 R2 的域功能级别。  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>无法使用智能卡具有的用户允许访问  
若要允许用户使用智能卡不可用的临时访问权限，请执行以下操作：  
  
1.  创建 Active Directory 安全组以包含暂时无法使用其智能卡的用户的用户帐户。  
  
2.  为 DirectAccess 服务器的组策略对象中，配置的 IPsec 隧道授权全局 IPsec 设置和 Active Directory 安全组添加到授权用户列表。  
  
不能使用其智能卡用户授予访问权限，暂时将其用户帐户添加到 Active Directory 安全组。 可使用智能卡时，请从组中删除用户帐户。  
  
### <a name="under-the-covers-smart-card-authorization"></a>在后台中：智能卡授权  
通过启用特定的基于 Kerberos 的安全标识符 (SID) 的 DirectAccess 服务器的隧道模式授权 intranet 隧道连接安全规则上的智能卡授权机制。 对于智能卡授权，这是已知 SID (S-1-5-65-1)，将映射到基于智能卡登录。 此 SID 是存在于 DirectAccess 客户端的 Kerberos 令牌和引用以作为"本组织证书"时配置为全局 IPsec 隧道模式授权设置。  
  
启用 DirectAccess 安装向导的第 2 步中的智能卡授权时，DirectAccess 安装向导将使用此 SID 的 DirectAccess 服务器的组策略对象配置 IPsec 隧道模式授权的全局设置。 若要查看此配置中具有高级安全管理单元中的 DirectAccess 服务器的组策略对象的 Windows 防火墙，请执行以下操作：  
  
1.  右键单击具有高级安全性的 Windows 防火墙，然后单击属性。  
  
2.  在 IPsec 设置选项卡上，在 IPsec 隧道授权中，单击自定义。  
  
3.  单击用户选项卡。为授权用户，应看到"NT AUTHORITY\This 组织证书"。  
  


