---
title: 始终启用 VPN 疑难解答
description: 本主题提供有关验证和 Windows Server 2016 中的 Always On VPN 部署疑难解答的说明。
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839088"
---
# <a name="troubleshoot-always-on-vpn"></a>始终启用 VPN 疑难解答 

>适用于：Windows Server （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows 10

如果 Always On VPN 安装程序无法将客户端连接到内部网络，则原因可能是 VPN 证书无效、 不正确的 NPS 策略或使用客户端部署脚本或路由和远程访问的问题。 故障排除和测试你的 VPN 连接的第一步了解 Always On VPN 基础结构的核心组件。 

可以通过多种方法中的连接问题进行故障排除。 有关客户端的问题和常规故障排除，客户端计算机上的应用程序日志是无价之宝。 有关特定于身份验证的问题，NPS 服务器上的 NPS 日志可以帮助您确定问题根源。


## <a name="error-codes"></a>错误代码


### <a name="error-code-800"></a>错误代码：800

-   **错误说明。** 未创建远程连接，因为尝试的 VPN 隧道失败。 VPN 服务器可能无法访问。 如果此连接尝试使用 L2TP/IPsec 隧道，用于 IPsec 协商可能未正确配置所需的安全参数。


-   **可能的原因。** VPN 隧道类型时，会发生此错误**自动**的所有 VPN 隧道连接尝试失败。

-   **可能的解决方案：**

    -   如果您知道要用于你的部署的隧道，设置的 VPN 类型为该特定的隧道类型在 VPN 客户端。

    -   通过使与特定的隧道类型的 VPN 连接，仍然会失败的连接，但它会导致更多特定于隧道的错误 (例如，"GRE 阻止 PPTP")。

    -   无法连接到 VPN 服务器或隧道连接失败时，也会发生此错误。

-   **确保：**

    -   IKE 端口 （UDP 端口 500 和 4500） 不会受到阻止。

    -   对于 IKE 正确证书存在客户端和服务器上。

### <a name="error-code-809"></a>错误代码：809

-   **错误说明。**  无法建立您的计算机和 VPN 服务器之间的网络连接，因为远程服务器未响应。 这可能是因为其中一个网络设备\(例如，防火墙、 NAT，路由器\)您的计算机和远程服务器之间未配置为允许 VPN 连接。 请联系管理员或服务提供商，以确定哪些设备可能会导致问题。

-   **可能的原因。** 已阻止的 UDP 500 或 VPN 服务器或防火墙上的最多返回 4500 端口被导致此错误。

-   **可能的解决方案。** 确保通过客户端和 RRAS 服务器之间的所有防火墙允许的 UDP 端口 500 和 4500。 
    
    
### <a name="error-code-812"></a>错误代码：812

-   **错误说明。** 无法连接到 Always On VPN。 由于 RAS/VPN 服务器上配置的策略，连接被阻止。 具体而言，身份验证方法使用的服务器以验证你的用户名和密码可能与在连接配置文件配置的身份验证方法不匹配。 请联系 RAS 服务器的管理员并通知用户此错误。

-   **可能的原因：**

    -  此错误的常见的原因是 NPS 已指定客户端不能满足身份验证条件。 例如，NPS 可以指定使用证书来保护 PEAP 连接，但客户端尝试使用 EAP-MSCHAPv2。

    -  当基于 RRAS 的 VPN 服务器身份验证协议设置不匹配的 VPN 客户端计算机时，事件日志 20276 记录到事件查看器。

-   **可能的解决方案。** 请确保你的客户端配置的 NPS 服务器指定的条件相匹配。


### <a name="error-code-13806"></a>错误代码：13806

-   **错误说明。** IKE 未能找到有效计算机证书。 请与网络安全管理员在相应的证书存储中安装有效的证书。

-   **可能的原因。** VPN 服务器上存在该错误通常发生在任何计算机证书或根计算机证书。

-   **可能的解决方案。** 请确保客户端计算机和 VPN 服务器上安装了此部署中所述的证书。

### <a name="error-code-13801"></a>错误代码：13801

-   **错误说明。** IKE 身份验证的凭据是不可接受。

-   **可能的原因。** 此错误通常发生在以下情况之一：

    -   计算机证书用于 IKEv2 验证 RAS 服务器上没有**服务器身份验证**下**增强型密钥用法**。

    -   RAS 服务器上的计算机证书已过期。

    -   要验证 RAS 服务器证书的根证书不存在客户端计算机上。

    -   使用客户端计算机上的 VPN 服务器名称不匹配**subjectName**的服务器证书。

-   **可能的解决方案。** 验证服务器证书，包括**服务器身份验证**下**增强型密钥用法**。 验证服务器证书仍然有效。 验证 CA 使用下列出**受信任的根证书颁发机构**RRAS 服务器上。 验证 VPN 客户端连接上的 VPN 服务器的证书提供通过 VPN 服务器的 FQDN。


### <a name="error-code-0x80070040"></a>错误代码：0x80070040

-   **错误说明。** 服务器证书不具有**服务器身份验证**作为其证书的使用情况条目之一。

-   **可能的原因。** 如果没有服务器身份验证证书安装在 RAS 服务器上，可能会发生此错误。

-   **可能的解决方案。** 请确保计算机证书的 RAS 服务器都使用为**IKEv2**已**服务器身份验证**作为证书使用情况条目之一。

### <a name="error-code-0x800b0109"></a>错误代码：0x800B0109

通常情况下，VPN 客户端计算机加入到基于 Active Directory 域。 如果使用域凭据登录到 VPN 服务器上，此证书自动安装在受信任的根证书颁发机构存储。 但是，如果未将计算机加入到域或使用备用证书链，可能会遇到此问题。

-   **错误说明。** 处理证书链，但是在信任提供程序不信任的根证书中终止。

-   **可能的原因。** 如果相应的受信任的根 CA 证书未安装在受信任的根证书颁发机构中可能出现此错误将存储在客户端计算机上。

-   **可能的解决方案。** 请确保在受信任的根证书颁发机构存储区中的客户端计算机上安装了根证书。

## <a name="logs"></a>日志

### <a name="application-logs"></a>应用程序日志

客户端计算机上的应用程序日志记录大多数 VPN 连接事件的更高级别的详细信息。

从源 RasClient 查看的事件。 所有错误消息都返回的消息末尾处的错误代码。 下面，详细介绍一些更常见的错误代码，但中提供了完整列表[路由和远程访问错误代码](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx)。

## <a name="nps-logs"></a>NPS 日志
NPS 创建，并将 NPS 记帐日志。 默认情况下，它们存储在 %SYSTEMROOT%\\System32\\Logfiles\\在文件中名为*XXXX。* txt，其中*XXXX*是该文件的创建的日期。

默认情况下，这些日志是以逗号分隔值格式，但它们不包括标题行。 标题行是：

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

如果将为日志文件的第一行中粘贴此标题行，然后将文件导入 Microsoft Excel，将正确地标记为列。

NPS 日志可以帮助你诊断与策略相关的问题。 有关 NPS 日志的详细信息，请参阅[解释 NPS 数据库格式日志文件](https://technet.microsoft.com/library/cc771748.aspx)。

## <a name="vpnprofileps1-script-issues"></a>VPN_Profile.ps1 脚本问题
手动运行 VPN_ Profile.ps1 脚本时最常见的问题包括：

- 使用远程连接工具？  请确保不使用 RDP 或另一种远程连接方法，因为它都在困扰着与用户登录名检测。

- 是用户的本地计算机的管理员？  请确保运行 VPN_Profile.ps1 脚本时用户具有管理员权限。

- 你是否已启用的其他 PowerShell 安全功能？ 请确保 PowerShell 执行策略不会阻止该脚本。 您可以考虑关闭受限语言模式下，如果启用，然后再运行该脚本。 在脚本已成功完成后，可以激活受限语言模式。

## <a name="always-on-vpn-client-connection-issues"></a>Always On VPN 客户端连接问题
较小的配置不正确可能导致客户端连接失败，可能很难找到的原因。  Always On VPN 客户端在建立连接之前经历几个步骤。 当客户端连接问题故障排除，经历的消除使用以下过程：


1. 外部模板计算机已连接？ 一个**whatismyip**扫描应显示不属于您的公共 IP 地址。

2. 可以远程访问 VPN 服务器名称解析为 IP 地址？ 在中**Control Panel** \> **网络**并**Internet** \> **网络连接**，打开属性为 VPN 配置文件。 中的值**常规**选项卡将变为可公开解析通过 DNS。

3. 您可以从外部网络访问 VPN 服务器？ 请考虑打开到的外部接口的 Internet 控制消息协议 (ICMP) 和从远程客户端名称执行 ping 操作。 Ping 命令成功后，可以删除 ICMP 允许规则。

4. 您是否已正确配置 VPN 服务器上有内部和外部 Nic？ 它们位于不同子网？ 到防火墙上的正确的接口可以连接外部 NIC？

5. UDP 500 和 4500 端口是打开从客户端到 VPN 服务器的外部接口的？ 检查客户端防火墙、 服务器防火墙和硬件的任何防火墙。 IPSEC 使用 UDP 端口 500，因此请确保没有 IPEC 禁用或任意位置阻止的。

7. 证书验证失败？ 验证 NPS 服务器可以 IKE 请求提供服务的服务器身份验证证书。 请确保具有正确的 VPN 服务器 IP 作为 NPS 客户端指定。 请确保使用 PEAP，进行身份验证和受保护的 EAP 属性应仅允许使用证书的身份验证。 您可以检查 NPS 事件日志中的身份验证失败。 有关更多详细信息，请参阅[安装和配置 NPS 服务器](vpn-deploy-nps.md)

8. 是否正在连接，但不是具有 Internet/本地网络访问权限？ 检查配置问题在 DHCP/VPN 服务器 IP 池。

9.  将连接并具有有效内部 IP 但不是能访问本地资源？  验证客户端知道如何获取对这些资源。 可以使用将请求路由的 VPN 服务器。


## <a name="azure-ad-conditional-access-connection-issues"></a>Azure AD 条件性访问连接问题

### <a name="oops---you-cant-get-to-this-yet"></a>很抱歉-你无法执行此尚未

-   **错误说明。** 当条件性访问策略不满足，阻止 VPN 连接，但连接用户单击鼠标后**X**以关闭消息。  单击**确定**会导致另一个身份验证尝试，在另一个结束_糟糕_消息。 客户端的 AAD Operational 事件日志中记录这些事件。 

-   **可能的原因。** 

    - 用户具有有效的客户端身份验证证书在其个人证书存储区不由 Azure AD 颁发的。

    - VPN 配置文件\<TLSExtensions\>部分中为缺少或不包含 **\<EKUName\>AAD 条件性访问\</EKUName\> \<EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>\<EKUName > AAD 条件性访问 < / EKUName\>\<EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>** 条目。 \<EKUName > 和\<EKUOID > 条目告知 VPN 客户端要将证书传递到 VPN 服务器时从用户的证书存储中检索的证书。 如果没有，VPN 客户端使用任何有效的客户端身份验证证书出现在用户的证书存储和身份验证成功。 

    - RADIUS 服务器 (NPS) 未配置为仅接受包含的客户端证书**AAD 条件性访问**OID。

-   **可能的解决方案。** 要转义此循环，请执行以下操作：

    1. 在 Windows PowerShell 中，运行**Get-wmiobject** cmdlet 来转储 VPN 配置文件配置。 
    2. 确认 **\<TLSExtensions >**，  **\<EKUName >**，以及 **\<EKUOID >** 部分存在并且正确显示名称和 OID。 
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

    3. 若要确定用户的证书存储区中是否存在有效的证书，请运行**Certutil**命令：

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
       >如果提供的证书颁发者**CN = Microsoft VPN 根 CA 第 1 代**存在于用户的个人存储区，但用户通过单击获取访问权限**X**关闭糟糕消息，收集 CAPI2 事件日志，以验证用于进行身份验证的证书是不从 Microsoft VPN 根 CA 颁发的有效客户端身份验证证书。

   4. 如果用户的个人存储中存在有效的客户端身份验证证书，则连接将失败 （如应），用户单击鼠标后**X** ; 如果 **\<TLSExtensions >**， **\<EKUName >**，并 **\<EKUOID >** 部分存在并且包含正确的信息。<p>_证书未找到可用于可扩展身份验证协议。_ 将显示错误消息。

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>无法从 VPN 连接边栏选项卡中删除证书

-   **错误说明。** 无法删除 VPN 连接边栏选项卡上的证书。

-   **可能的原因。** 证书设置为**主**。

-   **可能的解决方案。** 

    1. 在 VPN 连接的边栏选项卡，选择证书。
    2. 下**主**，选择**否**然后单击**保存**。
    3. 在 VPN 连接的边栏选项卡，再次选择的证书。
    4. 单击“删除”。


---
