---
title: 始终启用 VPN 疑难解答
description: 本主题提供有关在 Windows Server 2016 中验证和排查 Always On VPN 部署问题的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 60873c8bbf71ad5afa58bd9e19b1a3fd650bc65f
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871349"
---
# <a name="troubleshoot-always-on-vpn"></a>始终启用 VPN 疑难解答 

>适用于：Windows Server (半年频道), Windows Server 2016, Windows Server 2012 R2, Windows 10

如果 Always On VPN 安装程序未能将客户端连接到内部网络，原因可能是无效的 VPN 证书、NPS 策略不正确，或者是客户端部署脚本或路由和远程访问中的问题。 对 VPN 连接进行故障排除和测试的第一步是了解 Always On VPN 基础结构的核心组件。 

可以通过多种方式对连接问题进行故障排除。 对于客户端问题和一般故障排除，客户端计算机上的应用程序日志非常有用。 对于特定于身份验证的问题，NPS 服务器上的 NPS 日志可帮助你确定问题的根源。

## <a name="error-codes"></a>错误代码

### <a name="error-code-800"></a>错误代码：800

- **错误说明。** 未建立远程连接，因为尝试的 VPN 隧道失败。 可能无法访问 VPN 服务器。 如果此连接尝试使用 L2TP/IPsec 隧道，则 IPsec 协商所需的安全参数可能配置不正确。

- **可能的原因。** 如果 VPN 隧道类型为**自动**，并且所有 vpn 隧道的连接尝试失败，则会出现此错误。

- **可能的解决方案：**

    - 如果知道要将哪个隧道用于部署，请在 VPN 客户端将 VPN 类型设置为该特定隧道类型。

    - 通过建立具有特定隧道类型的 VPN 连接，连接仍然会失败，但会导致更多的特定于隧道的错误（例如，"已阻止 PPTP 的 PPTP"）。

    - 当无法访问 VPN 服务器或隧道连接失败时，也会出现此错误。

- **确保：**

    - IKE 端口（UDP 端口500和4500）不会被阻止。

    - IKE 的正确证书同时存在于客户端和服务器上。

### <a name="error-code-809"></a>错误代码：809

- **错误说明。**  由于远程服务器不响应，因此无法建立计算机与 VPN 服务器之间的网络连接。 这可能是因为你的计算机与远程服务器之间的网络设备（例如防火墙、NAT、路由器）之一未配置为允许 VPN 连接。 请与您的管理员或服务提供商联系，以确定可能导致问题的设备。

- **可能的原因。** 此错误是由 VPN 服务器或防火墙上被阻止的 UDP 500 或4500端口引起的。

- **可能的解决方案。** 确保在客户端与 RRAS 服务器之间的所有防火墙之间允许 UDP 端口500和4500。

### <a name="error-code-812"></a>错误代码：812

- **错误说明。** 无法连接到 Always On 的 VPN。 由于 RAS/VPN 服务器上配置的策略，连接被阻止。 具体而言，服务器用于验证用户名和密码的身份验证方法可能与连接配置文件中配置的身份验证方法不匹配。 请与 RAS 服务器的管理员联系，并通知此错误。

- **可能的原因：**

    - 此错误的典型原因是 NPS 指定了客户端无法满足的身份验证条件。 例如，NPS 可能指定使用证书来保护 PEAP 连接，但客户端正在尝试使用 Eap-mschapv2。

    - 当基于 RRAS 的 VPN 服务器身份验证协议设置与 VPN 客户端计算机的身份验证协议设置不匹配时，会将事件日志20276记录到事件查看器中。

- **可能的解决方案。** 确保你的客户端配置与 NPS 服务器上指定的条件相匹配。

### <a name="error-code-13806"></a>错误代码：13806

- **错误说明。** IKE 未能找到有效的计算机证书。 请与网络安全管理员联系，以在适当的证书存储中安装有效的证书。

- **可能的原因。** 如果 VPN 服务器上不存在计算机证书或根计算机证书，则通常会出现此错误。

- **可能的解决方案。** 确保在客户端计算机和 VPN 服务器上都安装了此部署中概述的证书。

### <a name="error-code-13801"></a>错误代码：13801

- **错误说明。** IKE 身份验证凭据不可接受。

- **可能的原因。** 此错误通常发生在下列情况之一：

    - 在 RAS 服务器上用于 IKEv2 验证的计算机证书在 "**增强型密钥用法**" 下不进行**服务器身份验证**。

    - RAS 服务器上的计算机证书已过期。

    - 用于验证 RAS 服务器证书的根证书不存在于客户端计算机上。

    - 客户端计算机上使用的 VPN 服务器名称与服务器证书的**subjectName**不匹配。

- **可能的解决方案。** 验证服务器证书是否在 "**增强型密钥用法**" 下包括**服务器身份验证**。 验证服务器证书是否仍然有效。 验证 RRAS 服务器上的 "**受信任的根证书颁发机构**" 下列出了所使用的 CA。 验证 VPN 客户端是否使用 vpn 服务器的证书上提供的 VPN 服务器的 FQDN 进行连接。

### <a name="error-code-0x80070040"></a>错误代码：0x80070040

- **错误说明。** 服务器证书不会将**服务器身份验证**作为其证书使用条目之一。

- **可能的原因。** 如果 RAS 服务器上未安装服务器身份验证证书，则可能出现此错误。

- **可能的解决方案。** 请确保 RAS 服务器用于**IKEv2**的计算机证书具有**服务器身份验证**作为证书使用条目之一。

### <a name="error-code-0x800b0109"></a>错误代码：0x800B0109

通常，VPN 客户端计算机联接到基于 Active Directory 的域。 如果使用域凭据登录到 VPN 服务器，则该证书会自动安装在 "受信任的根证书颁发机构" 存储中。 但是，如果计算机未加入域，或者使用备用证书链，则可能会遇到此问题。

- **错误说明。** 已处理证书链，但在信任提供程序不信任的根证书中终止。

- **可能的原因。** 如果未在客户端计算机上的受信任根证书颁发机构存储中安装合适的受信任根 CA 证书，则可能出现此错误。

- **可能的解决方案。** 请确保在 "受信任的根证书颁发机构" 存储中的客户端计算机上安装了根证书。

## <a name="logs"></a>日志

### <a name="application-logs"></a>应用程序日志

客户端计算机上的应用程序日志记录 VPN 连接事件的大多数较高级别的详细信息。

从源 RasClient 查找事件。 所有错误消息都将返回消息末尾的错误代码。 下面详细介绍了一些更常见的错误代码，但[路由和远程访问错误代码](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx)中提供了完整列表。

## <a name="nps-logs"></a>NPS 日志

NPS 创建并存储 NPS 记帐日志。 默认情况下，这些文件存储在名为\\的\\文件\\中的% SYSTEMROOT%System32 日志文件中，其中*xxxx*是创建文件的日期。

默认情况下，这些日志采用逗号分隔值格式，但不包括标题行。 标题行为：

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

如果将此标题行粘贴为日志文件的第一行，然后将该文件导入 Microsoft Excel，则这些列将正确标记。

NPS 日志有助于诊断与策略相关的问题。 有关 NPS 日志的详细信息，请参阅[解释 Nps 数据库格式日志文件](https://technet.microsoft.com/library/cc771748.aspx)。

## <a name="vpn_profileps1-script-issues"></a>VPN_Profile 脚本问题

手动运行 VPN_ Profile. ps1 脚本时，最常见的问题包括：

- 是否使用远程连接工具？  请确保不要使用 RDP 或其他远程连接方法，因为它烂摊子了用户登录检测。

- 用户是否为该本地计算机的管理员？  请确保在运行 VPN_Profile 脚本时，用户具有管理员权限。

- 是否启用了额外的 PowerShell 安全功能？ 请确保 PowerShell 执行策略未阻止脚本。 在运行脚本之前，您可以考虑关闭受约束的语言模式（如果启用）。 脚本成功完成后，可以激活受约束的语言模式。

## <a name="always-on-vpn-client-connection-issues"></a>Always On VPN 客户端连接问题

较小的错误配置可能会导致客户端连接失败，并可能会很难找到原因。  在建立连接之前，Always On VPN 客户端会经历几个步骤。 解决客户端连接问题时，请完成以下过程：

1. 模板计算机是否已外部连接？ **Whatismyip**扫描应显示不属于你的公共 IP 地址。

2. 是否可以将远程访问/vpn 服务器名称解析为 IP 地址？ 在 "控制面板" 的 **"**  > **网络**和**Internet** > **网络连接**" 中，打开 VPN 配置文件的属性。 "**常规**" 选项卡中的值应可通过 DNS 公开解析。

3. 是否可以通过外部网络访问 VPN 服务器？ 考虑将 Internet 控制消息协议（ICMP）打开到外部接口，并从远程客户端对名称进行 ping 操作。 Ping 成功后，可以删除 ICMP 允许规则。

4. VPN 服务器上的内部和外部 Nic 是否配置正确？ 它们是在不同的子网中吗？ 外部 NIC 是否连接到防火墙上的正确接口？

5. 是否从客户端向 VPN 服务器的外部接口打开 UDP 500 和4500端口？ 检查客户端防火墙、服务器防火墙和任何硬件防火墙。 IPSEC 使用 UDP 端口500，因此请确保未在任何位置禁用或阻止 IPEC。

6. 证书验证是否失败？ 验证 NPS 服务器是否具有可为 IKE 请求提供服务的服务器身份验证证书。 请确保已将正确的 VPN 服务器 IP 指定为 NPS 客户端。 确保你正在使用 PEAP 进行身份验证，并且受保护的 EAP 属性应该只允许使用证书进行身份验证。 可以检查 NPS 事件日志中的身份验证失败。 有关更多详细信息，请参阅[安装和配置 NPS 服务器](vpn-deploy-nps.md)

7. 你是否正在连接，但没有 Internet/本地网络访问权限？ 检查 DHCP/VPN 服务器 IP 池是否存在配置问题。

8. 你是否正在连接并且具有有效的内部 IP，但无权访问本地资源？  验证客户端是否知道如何访问这些资源。 可以使用 VPN 服务器路由请求。

## <a name="azure-ad-conditional-access-connection-issues"></a>Azure AD 条件访问连接问题

### <a name="oops---you-cant-get-to-this-yet"></a>糟糕-你仍无法访问

- **错误说明。** 如果不满足条件访问策略，则会阻止 VPN 连接，但会在用户选择**X**之后连接以关闭消息。  选择 **"确定"** 将导致另一个身份验证尝试，该尝试以其他 "糟糕" 消息结束。 这些事件记录在客户端的 AAD 操作事件日志中。

- **可能的原因**

  - 用户在其个人证书存储区中具有有效的客户端身份验证证书，该证书不是由 Azure AD 颁发的。

  - VPN 配置文件\<TLSExtensions\>部分缺失 **\<或不包含 EKUName\>AAD 条件\<访问\>/EKUName EKUOID\<\>1.3.6.1.4.1.311.87 </EKUOID\>\>\<\>EKUName > AAD 条件访问 </EKUName EKUOID 1.3.6.1.4.1.311.87 </EKUOID\> \<** 条目。 EKUName > 和\<EKUOID > 条目告诉 vpn 客户端在将证书传递到 vpn 服务器时要从用户的证书存储中检索的证书。 \< 如果不这样做，VPN 客户端将使用用户证书存储区中任何有效的客户端身份验证证书，并且身份验证成功。 

  - RADIUS 服务器（NPS）未配置为仅接受包含**AAD 条件访问**OID 的客户端证书。

- **可能的解决方案。** 若要对此循环进行转义，请执行以下操作：

  1. 在 Windows PowerShell 中，运行**get-wmiobject** cmdlet 以转储 VPN 配置文件配置。 
  2. 验证 "  **\<TLSExtensions >** "、  **\<"EKUName >** " 和 **\<"EKUOID" >** 部分是否存在，并显示正确的名称和 OID。
      
      ```powershell
      PS C:\> Get-WmiObject -Class MDM_VPNv2_01 -Namespace root\cimv2\mdm\dmmap

      __GENUS                 : 2
      __CLASS                 : MDM_VPNv2_01
      __SUPERCLASS            :
      __DYNASTY               : MDM_VPNv2_01
      __RELPATH               : MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VPNv2"
      __PROPERTY_COUNT        : 10
      __DERIVATION            : {}
      __SERVER                : DERS2
      __NAMESPACE             : root\cimv2\mdm\dmmap
      __PATH                  : \\DERS2\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VP
                                  Nv2"
      AlwaysOn                :
      ByPassForLocal          :
      DnsSuffix               :
      EdpModeId               :
      InstanceID              : AlwaysOnVPN
      LockDown                :
      ParentID                : ./Vendor/MSFT/VPNv2
      ProfileXML              : <VPNProfile><RememberCredentials>false</RememberCredentials><DeviceCompliance><Enabled>true</
                                  Enabled><Sso><Enabled>true</Enabled></Sso></DeviceCompliance><NativeProfile><Servers>derras2.
                                  corp.deverett.info;derras2.corp.deverett.info</Servers><RoutingPolicyType>ForceTunnel</Routin
                                  gPolicyType><NativeProtocolType>Ikev2</NativeProtocolType><Authentication><UserMethod>Eap</Us
                                  erMethod><MachineMethod>Eap</MachineMethod><Eap><Configuration><EapHostConfig
                                  xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config
                                  xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.
                                  com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.mic
                                  rosoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptFor
                                  ServerValidation>true</DisableUserPromptForServerValidation><ServerNames></ServerNames></Serv
                                  erValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Ea
                                  p xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type>
                                  <EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><Credenti
                                  alsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore
                                  ></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUse
                                  rPromptForServerValidation><ServerNames></ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b
                                  1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>fal
                                  se</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/E
                                  apTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://ww
                                  w.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName><TLSExtens
                                  ions
                                  xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xml
                                  ns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><
                                  EKUName>AAD Conditional
                                  Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList
                                  Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></Client
                                  AuthEKUList></FilteringInfo></TLSExtensions></EapType></Eap><EnableQuarantineChecks>false</En
                                  ableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><Perfo
                                  rmServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2"
                                  >false</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisionin
                                  g/MsPeapConnectionPropertiesV2">false</AcceptServerName></PeapExtensions></EapType></Eap></Co
                                  nfig></EapHostConfig></Configuration></Eap></Authentication></NativeProfile></VPNProfile>
      RememberCredentials     : False
      TrustedNetworkDetection :
      PSComputerName          : DERS2
      ```

  3. 若要确定用户的证书存储中是否存在有效证书，请运行**Certutil**命令：

     ```powershell
     C:\>certutil -store -user My

      My "Personal"
      ================ Certificate 0 ================
      Serial Number: 32000000265259d0069fa6f205000000000026
      Issuer: CN=corp-DEDC0-CA, DC=corp, DC=deverett, DC=info
       NotBefore: 12/8/2017 8:07 PM
       NotAfter: 12/8/2018 8:07 PM
      Subject: E=winfed@deverett.info, CN=WinFed, OU=Users, OU=Corp, DC=corp, DC=deverett, DC=info
      Certificate Template Name (Certificate Type): User
      Non-root Certificate
      Template: User
      Cert Hash(sha1): a50337ab015d5612b7dc4c1e759d201e74cc2a93
        Key Container = a890fd7fbbfc072f8fe045e680c501cf_5834bfa9-1c4a-44a8-a128-c2267f712336
        Simple container name: te-User-c7bcc4bd-0498-4411-af44-da2257f54387
        Provider = Microsoft Enhanced Cryptographic Provider v1.0
      Encryption test passed
        
      ================ Certificate 1 ================
      Serial Number: 367fbdd7e6e4103dec9b91f93959ac56
      Issuer: CN=Microsoft VPN root CA gen 1
       NotBefore: 12/8/2017 6:24 PM
       NotAfter: 12/8/2017 7:29 PM
      Subject: CN=WinFed@deverett.info
      Non-root Certificate
      Cert Hash(sha1): 37378a1b06dcef1b4d4753f7d21e4f20b18fbfec
        Key Container = 31685cae-af6f-48fb-ac37-845c69b4c097
        Unique container name: bf4097e20d4480b8d6ebc139c9360f02_5834bfa9-1c4a-44a8-a128-c2267f712336
        Provider = Microsoft Software Key Storage Provider
      Private key is NOT exportable
      Encryption test passed
     ```
     >[!NOTE]
     >如果用户的个人存储区中存在来自颁发者**CN = MICROSOFT VPN 根 CA 第1代**的证书，但用户通过选择**X**来获取访问权限，则关闭该消息，收集 CAPI2 事件日志以验证用于身份验证的证书是不是从 Microsoft VPN 根 CA 颁发的有效客户端身份验证证书。

  4. 如果用户的个人存储区中存在有效的客户端身份验证证书，则在用户选择**X**并且 **\<TLSExtensions >** 、  **\<EKUName >** 和**EKUOID\<>** 部分存在并且包含正确的信息。
   
     出现一条错误消息，显示 "找不到可用于可扩展身份验证协议的证书"。

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>无法从 "VPN 连接" 边栏选项卡删除证书

- **错误说明。** 无法删除 VPN 连接边栏选项卡上的证书。

- **可能的原因。** 证书设置**为主**证书。

- **可能的解决方案。**

    1. 在 "VPN 连接" 边栏选项卡中，选择证书。
    2. 在 "**主**" 下，选择 "**否**"，然后选择 "**保存**"。
    3. 在 "VPN 连接" 边栏选项卡中，再次选择证书。
    4. 选择 "**删除**"。
