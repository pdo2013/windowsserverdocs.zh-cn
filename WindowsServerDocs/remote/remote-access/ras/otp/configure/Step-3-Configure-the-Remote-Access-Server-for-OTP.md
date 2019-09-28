---
title: 步骤3为 OTP 配置远程访问服务器
description: 本主题是指南使用 Windows Server 2016 中的 "使用 OTP 身份验证部署远程访问" 指南的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: df1e87f2-6a0f-433b-8e42-816ae75395f9
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 41cc5cc2df5ac9709818536df8fff098d2a0c297
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404337"
---
# <a name="step-3-configure-the-remote-access-server-for-otp"></a>步骤3为 OTP 配置远程访问服务器

>适用于：Windows Server（半年频道）、Windows Server 2016

为 RADIUS 服务器配置软件分发令牌后，通信端口会打开，已创建共享机密，与 Active Directory 相对应的用户帐户已在 RADIUS 服务器上创建，并且远程访问服务器具有配置为 RADIUS 身份验证代理，则需要配置远程访问服务器以支持 OTP。  
  
|任务|描述|  
|----|--------|  
|[3.1 从 OTP 身份验证中免除用户（可选）](#BKMK_Exempt)|如果将从具有 OTP 身份验证的 DirectAccess 中免除特定用户，请遵循以下预备步骤。|  
|[3.2 将远程访问服务器配置为支持 OTP](#BKMK_Config)|在远程访问服务器上，更新远程访问配置以支持 OTP 双重身份验证。|  
|[3.3 用于附加授权的智能卡](#BKMK_Smartcard)|有关智能卡使用的其他信息。|  
  
> [!NOTE]  
> 此主题将介绍一些 Windows PowerShell cmdlet 示例，你可以使用它们来自动执行所述的一些步骤。 有关详细信息，请参阅 [使用 cmdlet](https://go.microsoft.com/fwlink/p/?linkid=230693)。  
  
## <a name="BKMK_Exempt"></a>3.1 从 OTP 身份验证中免除用户（可选）  
如果要免除特定用户的 OTP 身份验证，则必须在远程访问配置之前执行以下步骤：  
  
> [!NOTE]  
> 配置 OTP 例外组时，必须等待域之间的复制完成。  
  
#### <a name="create-user-exemption-security-group"></a>创建用户例外安全组  
  
1.  在 Active Directory 中创建一个用于 OTP OTP 例外的安全组。  
  
2.  将从 OTP 身份验证免除的所有用户添加到安全组。  
  
    > [!NOTE]  
    > 请确保在 OTP 例外安全组中仅包括用户帐户，而不包括计算机帐户。  
  
## <a name="BKMK_Config"></a>3.2 将远程访问服务器配置为支持 OTP  
若要配置远程访问以将双因素身份验证和 OTP 用于 RADIUS 服务器，并将证书部署用于前面的部分，请执行以下步骤：  
  
#### <a name="configure-remote-access-for-otp"></a>为 OTP 配置远程访问  
  
1.  打开 "**远程访问管理**"，然后单击 "**配置**"。  
  
2.  在 " **DirectAccess 设置**" 窗口中的 "**步骤 2-远程访问服务器**" 下，单击 "**编辑**"。  
  
3.  单击 "**下一步**" 三次，并在 "**身份验证**" 部分选择 "**双重身份验证**" 和 "**使用 OTP**"，并确保选中 "**使用计算机证书**"。  
  
    > [!NOTE]  
    > 在远程访问服务器上启用 OTP 后，如果通过取消选择**使用 otp**来禁用 otp，则会在服务器上卸载 ISAPI 和 CGI 扩展。  
  
4.  如果需要 Windows 7 支持，请选中 "**使 windows 7 客户端计算机能够通过 DirectAccess 进行连接**" 复选框。 注意:如 "规划" 一节中所述，Windows 7 客户端必须安装了 DCA 2.0 以支持具有 OTP 的 DirectAccess。  
  
5.  单击“下一步”。  
  
6.  在 " **OTP RADIUS 服务器**" 部分中，双击 "空白**服务器名称**" 字段。  
  
7.  在 "**添加 Radius 服务器**" 对话框中，在 "**服务器名称**" 字段中键入 RADIUS 服务器的名称。 单击 "**共享机密**" 字段旁边的 "**更改**"，然后键入在**新密钥**中配置 RADIUS 服务器时使用的相同密码，并**确认新的机密**字段。 单击 **"确定"** 两次，然后单击 "**下一步**"。  
  
    > [!NOTE]  
    > 如果 RADIUS 服务器所在的域不同于远程访问服务器，则 "**服务器名称**" 字段必须指定 RADIUS 服务器的 FQDN。  
  
8.  在 " **OTP Ca 服务器**" 部分中，选择要用于注册 OTP 客户端身份验证证书的 CA 服务器，然后单击 "**添加**"。 单击“下一步”。  
  
9. 在 " **OTP 证书模板**" 部分中，单击 "**浏览**" 选择用于注册用于 OTP 身份验证的证书的证书模板。  
  
    > [!NOTE]  
    > 企业 CA 颁发的 OTP 证书的证书模板必须配置为不包含 "在颁发的证书中不包含吊销信息" 选项。 如果在创建证书模板期间选择了此选项，则 OTP 客户端计算机将无法正常登录。  
  
    单击 "**浏览**" 选择用于注册远程访问服务器用于签署 OTP 证书注册请求的证书的证书模板。 单击 **“确定”** 。 单击“下一步”。  
  
10. 如果需要使用 OTP 的豁免特定用户，请在 " **Otp 免除**" 部分中选择 "**不要求指定安全组中的用户使用双因素身份验证进行身份验证**"。 单击 "**安全组**"，然后选择为 OTP 例外创建的安全组。  
  
11. 在 "**远程访问服务器设置**" 页上，单击 "**完成**"。  
  
12. 在 " **DirectAccess 设置**" 窗口中的 "**步骤 3-基础结构服务器**" 下，单击 "**编辑**"。  
  
13. 单击 "**下一步**" 两次，然后在 "**管理**" 部分中双击 "**管理服务器**" 字段。  
  
14. 输入配置为颁发 OTP 证书的 CA 服务器的**计算机名称**或**地址**，然后单击 **"确定"** 。  
  
15. 在**远程访问设置**窗口中，单击 "**完成**"。  
  
16. 单击 " **DirectAccess 专家向导**" 上的 "**完成**"。  
  
17. 在 "**远程访问查看**" 对话框中，单击 "**应用**"，等待 DirectAccess 策略更新，然后单击 "**关闭**"。  
  
18. 在 "**开始**" 屏幕上，键入 "**powershell**"，右键单击 " **Powershell**"，单击 "**高级**"，然后单击 "以**管理员身份运行**"。 如果出现了“用户帐户控制”对话框，请确认其所显示的操作是你要采取的操作，然后单击“是”。  
  
19. 在 Windows PowerShell 窗口中，键入**gpupdate/force** ，然后按 enter。  
  
使用 PowerShell 命令配置 OTP 远程访问：  
  
@no__t 0Windows PowerShell](../../../../media/Step-3-Configure-the-Remote-Access-Server-for-OTP/PowerShellLogoSmall.gif)**Windows powershell 等效命令**  
  
下面一个或多个 Windows PowerShell cmdlet 执行的功能与前面的过程相同。 在同一行输入每个 cmdlet（即使此处可能因格式限制而出现多行换行）。  
  
若要配置远程访问以在当前使用计算机证书身份验证的部署上使用双因素身份验证，请执行以下操作：  
  
```  
Set-DAServer -UserAuthentication TwoFactor  
```  
  
若要将远程访问配置为使用 OTP 身份验证，请使用以下设置：  
  
-   名为 OTP.corp.contoso.com 的 OTP 服务器。  
  
-   名为 APP1 的 CA 服务器。 com\corp-APP1-CA1。  
  
-   名为 DAOTPLogon 的证书模板，用于注册为 OTP 身份验证颁发的证书。  
  
-   名为 DAOTPRA 的证书模板，用于注册由远程访问服务器用于签署 OTP 证书注册请求的注册机构证书。  
  
```  
Enable-DAOtpAuthentication -CertificateTemplateName 'DAOTPLogon' -SigningCertificateTemplateName 'DAOTPRA' -CAServer @('APP1.corp.contoso.com\corp-APP1-CA1') -RadiusServer OTP.corp.contoso.com -SharedSecret Abcd123$  
```  
  
执行 PowerShell 命令后，完成前一过程中的步骤12-19，将远程访问服务器配置为支持 OTP。  
  
> [!NOTE]  
> 请确保在添加入口点之前验证是否已在远程访问服务器上应用了 OTP 设置。  
  
## <a name="BKMK_Smartcard"></a>3.3 用于附加授权的智能卡  
在 "远程访问设置向导" 中步骤2的 "身份验证" 页上，你可以要求使用智能卡访问内部网络。 如果选择此选项，则远程访问设置向导将为 DirectAccess 服务器上的 intranet 隧道配置 IPsec 连接安全规则，以要求使用智能卡进行隧道模式授权。 隧道模式授权允许您指定只有经过授权的计算机或用户可以建立入站隧道。  
  
若要将智能卡用于 intranet 隧道的 IPsec 隧道模式授权，必须使用智能卡基础结构部署公钥基础结构（PKI）。  
  
由于 DirectAccess 客户端使用智能卡访问 intranet，因此还可以使用身份验证机制保证（一项 Windows Server 2008 R2 的一项功能），根据是否要控制对资源（如文件、文件夹和打印机）的访问用户使用基于智能卡的证书登录。 身份验证机制保证需要 Windows Server 2008 R2 的域功能级别。  
  
### <a name="allowing-access-for-users-with-unusable-smart-cards"></a>允许具有不可用智能卡的用户的访问权限  
若要为具有不可用智能卡的用户提供临时访问权限，请执行以下操作：  
  
1.  创建 Active Directory 安全组，以包含暂时不能使用其智能卡的用户的用户帐户。  
  
2.  对于 DirectAccess 服务器组策略对象，配置 IPsec 隧道授权的全局 IPsec 设置，并将 Active Directory 安全组添加到授权用户的列表中。  
  
若要向不能使用其智能卡的用户授予访问权限，请暂时将其用户帐户添加到 Active Directory 安全组。 当智能卡可用时，从组中删除该用户帐户。  
  
### <a name="under-the-covers-smart-card-authorization"></a>以下内容：智能卡授权  
智能卡授权工作方式为：对基于 Kerberos 的特定安全标识符（SID）启用 DirectAccess 服务器的 intranet 隧道连接安全规则的隧道模式授权。 对于智能卡身份验证，这是众所周知的 SID （S-1-5-65-1），它映射到基于智能卡的登录。 此 SID 存在于 DirectAccess 客户端的 Kerberos 令牌中，在 "全局 IPsec 隧道模式授权" 设置中配置为 "此组织证书"。  
  
当你在 DirectAccess 安装向导的步骤2中启用智能卡授权时，DirectAccess 安装向导将使用此 SID 为 DirectAccess 服务器组策略对象配置全局 IPsec 隧道模式授权设置。 若要在 DirectAccess 服务器组策略对象的 "具有高级安全性的 Windows 防火墙" 管理单元中查看此配置，请执行以下操作：  
  
1.  右键单击 "高级安全 Windows 防火墙"，然后单击 "属性"。  
  
2.  在 "IPsec 设置" 选项卡上的 "IPsec 隧道授权" 中，单击 "自定义"。  
  
3.  单击 "用户" 选项卡。你应会看到作为授权用户的 "NT AUTHORITY\This 组织证书"。  
  


