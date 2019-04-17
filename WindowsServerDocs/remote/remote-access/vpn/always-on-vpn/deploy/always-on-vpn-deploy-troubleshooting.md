---
title: 始终启用 VPN 疑难解答
description: 本主题提供有关验证和疑难解答始终启用 VPN 部署 Windows Server 2016 中的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067047"
---
# 始终启用 VPN 疑难解答 

>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

如果始终启用 VPN 安装程序无法连接到内部网络的客户端，原因可能是无效的 VPN 证书、 不正确 NPS 策略或客户端部署脚本或者在路由和远程访问的问题。 疑难解答和测试你的 VPN 连接的第一步了解始终启用 VPN 基础结构的核心组件。 

你可以解决以下几种方式连接问题。 对于客户端问题和常规故障排除，客户端计算机上的应用程序日志是非常重要。 对于特定于身份验证的问题，NPS 服务器上的 NPS 日志可以帮助你确定该问题的来源。


## 错误代码


### 错误代码： 800

-   **错误描述。** 由于尝试的 VPN 隧道无法不进行远程连接。 VPN 服务器可能不可访问。 如果此连接尝试使用 L2TP/IPsec 隧道，安全参数被必需的 IPsec 协商可能未正确配置。


-   **可能的原因。** **自动**VPN 隧道类型并为所有 VPN 隧道连接尝试失败时，会发生此错误。

-   **可能的解决方案：**

    -   如果你知道要用于部署的隧道，VPN 客户端中将 VPN 类型设置为该特定隧道类型。

    -   通过使与特定隧道类型的 VPN 连接，仍然会失败连接，但它将导致更具体隧道的错误 (例如，"GRE 阻止 PPTP")。

    -   无法访问 VPN 服务器或隧道连接失败时，也会发生此错误。

-   **确保：**

    -   IKE 端口 （UDP ports500 和 4500） 不会被阻止。

    -   IKE 的正确证书存在客户端和服务器上。

### 错误代码： 809

-   **错误描述。**  因为未响应远程服务器无法建立你的计算机和 VPN 服务器之间的网络连接。 这可能是因为网络设备之一 \ (例如，防火墙，NAT，routers\) 计算机和远程服务器之间未配置为允许 VPN 连接。 请联系你的管理员，或者你的服务提供商，以确定哪些设备可能导致该问题。

-   **可能的原因。** 通过阻止的 UDP 500 或 VPN 服务器或防火墙上的 4500 端口被导致此错误。

-   **可能的解决方案。** 请确保通过客户端和 RRAS 服务器之间的所有防火墙允许 UDP ports500 和 4500。 
    
    
### 错误代码： 812

-   **错误描述。** 无法连接到始终启用 VPN。 由于 RAS/VPN 服务器上配置一个策略阻止连接。 具体来说，身份验证方法服务器用于验证你的用户名和密码可能与连接配置文件中配置的身份验证方法不匹配。 请联系 RAS 服务器的管理员，他或她此错误的通知。

-   **可能的原因：**

    -  此错误的典型原因是 NPS 已指定客户端无法满足身份验证条件。 例如，NPS 可以指定使用的证书来保护 PEAP 连接，但客户端尝试使用 Eap-mschapv2。

    -  基于 RRAS 的 VPN 服务器身份验证协议设置与 VPN 客户端计算机都不匹配时，会将事件日志 20276 记录到事件查看器中。

-   **可能的解决方案。** 确保你的客户端配置匹配 NPS 服务器指定的条件。


### 错误代码： 13806

-   **错误描述。** IKE failed to find 有效的计算机证书。 与有关在适当的证书存储中安装一个有效证书，网络安全管理员联系。

-   **可能的原因。** 当没有计算机证书时，通常会发生此错误或根计算机证书存在 VPN 服务器上。

-   **可能的解决方案。** 确保客户端计算机和 VPN 服务器上安装了此部署中概述的证书。

### 错误代码： 13801

-   **错误描述。** 接受 IKE 身份验证凭据。

-   **可能的原因。** 在以下情况之一，通常会发生此错误：

    -   用于 IKEv2 验证 RAS 服务器上的计算机证书不具有下**增强型密钥用法**的**服务器身份验证**。

    -   RAS 服务器上的计算机证书已过期。

    -   若要验证 RAS 服务器证书的根证书不存在的客户端计算机上。

    -   在客户端计算机上使用的 VPN 服务器名称不匹配的服务器证书**subjectName** 。

-   **可能的解决方案。** 验证服务器证书包含在**增强型密钥用法**下的**服务器身份验证**。 验证服务器证书仍然有效。 验证 CA 使用**受信任的根证书颁发机构**下面列出的 RRAS 服务器上。 验证 VPN 客户端连接的 VPN 服务器证书上呈现通过 VPN 服务器的 FQDN。


### 错误代码： 0x80070040

-   **错误描述。** 服务器证书作为其证书的使用情况项之一没有**服务器身份验证**。

-   **可能的原因。** 如果没有服务器身份验证证书 RAS 服务器上安装，则可能发生此错误。

-   **可能的解决方案。** 请确保为**IKEv2**使用 RAS 服务器的计算机证书作为证书的使用情况项之一具有**服务器身份验证**。

### 错误代码： 0x800B0109

通常情况下，VPN 客户端计算机已加入到基于 Active Directory 域。 如果你使用的域凭据登录到 VPN 服务器上，此证书自动安装在受信任的根证书颁发机构存储。 但是，如果计算机未加入域，或者如果你使用备用证书链，你可能会遇到此问题。

-   **错误描述。** 证书链处理，但在信任提供程序不信任的根证书终止。

-   **可能的原因。** 如果在受信任的根证书颁发机构未安装相应受信任的根 CA 证书，则可能发生此错误存储在客户端计算机上。

-   **可能的解决方案。** 请确保在受信任的根证书颁发机构存储中的客户端计算机上安装的根证书。

## 日志

### 应用程序日志

客户端计算机上的应用程序日志记录大部分 VPN 连接事件的更高级别的详细信息。

从源 RasClient 查找的事件。 所有错误消息都返回末尾的消息的错误代码。 在下面，详细介绍一些更常见的错误代码，但的完整列表位于[路由和远程访问错误代码](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx)。

## NPS 日志
NPS 创建和存储 NPS 记帐日志。 默认情况下，这些存储在文件在名为中 %SYSTEMROOT%\\System32\\Logfiles\\*XXXX。* txt，其中*XXXX*是日期创建该文件。

默认情况下，这些日志以逗号分隔值格式，但它们不包含标题行。 标题行是：

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

如果作为日志文件的第一行粘贴此标题行，然后将该文件导入 Microsoft Excel，将正确标记为列。

NPS 日志可能有助于诊断与策略相关的问题。 有关 NPS 日志的详细信息，请参阅[解释 NPS 数据库格式日志文件](https://technet.microsoft.com/library/cc771748.aspx)。

## VPN_Profile.ps1 脚本问题
手动运行 VPN_ Profile.ps1 脚本时，最常见的问题，包括：

- 使用远程连接工具？  请确保不使用 RDP 或其他远程连接方法，因为它与用户登录检测这将搞。

- 是用户的本地计算机的管理员？  请确保同时运行 VPN_Profile.ps1 脚本的用户具有管理员权限。

- 你是否拥有其他 PowerShell 安全功能已启用？ 请确保 PowerShell 执行策略不会阻止该脚本。 你可以考虑关闭约束的语言模式下，如果已启用，然后再运行脚本。 该脚本成功完成后，你可以激活约束的语言模式。

## 始终启用 VPN 客户端连接问题
较小的错误配置可能会导致客户端连接失败，并可以难以查找原因。  始终启用 VPN 客户端之前建立连接经历几个步骤。 当客户端连接问题进行疑难解答，转的清除使用以下过程：


1. 模板计算机外部是否已连接？ **Whatismyip**扫描应显示不属于你的公共 IP 地址。

2. 可以将每个 VPN 远程访问服务器名称解析为 IP 地址？ 在**控制面板**中 \> **网络**和**Internet** \> **网络连接**，打开你的 VPN 配置文件的属性。 **常规**选项卡中的值应公开通过 DNS 解析。

3. 你可以从外部网络访问 VPN 服务器？ 请考虑到外部接口打开 Internet 控制消息协议 (ICMP) 和 ping 远程客户端中的名称。 执行 ping 操作成功后，你可以删除 ICMP 允许规则。

4. 你是否拥有正确配置 VPN 服务器上的内部和外部 Nic？ 它们在不同的子网中？ 外部 NIC 未连接到正确的接口防火墙上？

5. UDP 500 和 4500 端口是从客户端上打开到 VPN 服务器的外部接口的吗？ 检查客户端防火墙、 服务器防火墙和任何硬件防火墙。 IPSEC 使用 UDP 端口 500，因此请确保没有 IPEC 禁用或阻止任意位置。

7. 证书验证失败？ 验证 NPS 服务器有可以 IKE 请求提供服务的服务器身份验证证书。 请确保你拥有正确的 VPN 服务器 IP 指定作为 NPS 客户端。 请确保你正在通过 PEAP 中, 进行身份验证，并受保护的 EAP 属性应只允许使用证书身份验证。 你可以检查 NPS 事件日志中的身份验证失败。 有关更多详细信息，请参阅[安装和配置 NPS 服务器](vpn-deploy-nps.md)

8. 连接却不具有 Internet/本地网络访问？ 检查你的配置问题的 DHCP/VPN 服务器 IP 池。

9.  你的连接并拥有没有本地资源的访问权限的有效内部 IP 但？  验证客户端知道如何获取对这些资源。 你可以使用请求路由到 VPN 服务器。


## Azure AD 的条件访问连接问题

### 天哪-你无法获得这尚未

-   **错误描述。** 条件访问策略不满足，则阻止 VPN 连接，但之后连接时用户单击**X**以关闭该消息。  单击**确定**将导致另一个身份验证请求，在另一个_Oops_消息结束。 客户端的 AAD 操作事件日志中记录这些事件。 

-   **可能的原因。** 

    - 用户具有的不通过 Azure AD 颁发的有效的客户端身份验证证书在他们的个人证书存储。

    - VPN 配置文件 \ < TLSExtensions\ > 部分为缺少或不包含**\ < EKUName\ > AAD 条件 Access\ < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ > \ < EKUName > AAD 条件访问< / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ >** 条目。 \ < EKUName > 和 \ < EKUOID > 条目告诉 VPN 客户端要传递给 VPN 服务器的证书时从用户的证书存储中检索的证书。 无需此，VPN 客户端使用用户的证书存储中的任何有效的客户端身份验证证书是和身份验证成功。 

    - RADIUS 服务器 (NPS) 尚未配置为仅接受包含**AAD 条件访问**OID 的客户端证书。

-   **可能的解决方案。** 转义此循环中，执行以下操作：

    1. Windows PowerShell 中运行的**Get-wmiobject** cmdlet 转储 VPN 配置文件配置。 
    2. 确认**\ < TLSExtensions >**， **\ < EKUName >**，并**\ < EKUOID >** 部分存在并显示正确的名称和 OID。 
        ```
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

    3. 若要确定是否存在有效证书用户的证书存储中，运行**Certutil**命令：

       ```
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
       >如果从颁发者证书**CN = Microsoft VPN 根 CA 第 1 代**显示在用户的个人存储，但通过单击**X**以关闭 Oops 消息、 收集 CAPI2 事件日志中获得访问权限的用户，以验证是否已用于进行身份验证的证书不从 Microsoft VPN 根 CA 颁发的有效客户端身份验证证书。

   4. 如果用户的个人存储中存在一个有效的客户端身份验证证书，连接将失败 （如它应该），用户单击**X**之后，如果**\ < TLSExtensions >**， **\ < EKUName >**，并**\ < EKUOID >** 部分存在，并且包含正确的信息。<p>_证书无法找到可可扩展身份验证协议与。_ 出现错误消息。

### 无法从 VPN 连接边栏选项卡中删除证书

-   **错误描述。** VPN 连接边栏选项卡上的证书不能删除。

-   **可能的原因。** 证书设置为**主要**。

-   **可能的解决方案。** 

    1. VPN 连接的边栏选项卡，选择证书。
    2. 在**主**，选择**否**，然后单击**保存**。
    3. VPN 连接的边栏选项卡，请再次选择的证书。
    4. 单击**删除**。


---
