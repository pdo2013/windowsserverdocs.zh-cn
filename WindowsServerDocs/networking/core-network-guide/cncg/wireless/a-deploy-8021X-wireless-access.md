---
title: 部署密码基于的 802.1 X 身份验证的无线接入
description: 本主题介绍 Windows Server 2016 网络指南"部署密码基于 802.1 X 身份验证无线的访问"部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ff06ba23-9c0f-49ec-8f7b-611cf8d73a1b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0ded8a273a9ad464b44fa7245db58d0fd05f06a2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-password-based-8021x-authenticated-wireless-access"></a>部署 Password\ 基于的 802.1 X 身份验证的无线接入

>适用于：Windows Server（半年通道），Windows Server 2016

这是 Windows Server 助手指南&reg;2016 Core 网络指南。 Core 网络指南提供规划和部署组件所需的完全正常网络域和新 Active Directory® 新建森林中的说明进行操作。

本指南介绍了如何版本核心网络时，通过提供有关如何部署电气和电子工程师 \(IEEE\) 802.1X\ 的说明进行操作-通过 IEEE 802.11 使用保护可扩展身份验证协议-Microsoft 挑战握手验证协议版本 2 的无线接入身份验证 \ (PEAP\ MS\ CHAP v2\)。

因为 PEAP\ MS\ CHAP v2 所需的用户身份验证过程中，提供基于 password\ 的凭据，而不是一个证书，则通常变得轻松又成本较低比 EAP\ TLS 或 PEAP\ TLS 部署。

>[!NOTE]
>本指南中 IEEE 802.1 X 身份验证的无线访问与 PEAP\ MS\ CHAP v2 缩写"无线接入"和"WiFi 访问"。

## <a name="about-this-guide"></a>有关此指南
本指南，预备，如下所述的指南结合提供有关如何部署以下 WiFi 访问基础结构的说明进行操作。  

- 一个或多个 802.1X\-支持 802.11 的无线接入点 \(APs\)。

- Active Directory 域服务 \(AD DS\) 用户和计算机。

- 组策略管理。

- 一个或多个网络策略服务器 \(NPS\) 服务器。

- 运行 NPS 计算机的服务器证书。

- 无线运行 Windows 的客户端计算机&reg;10、 Windows 8.1 或 Windows 8。

### <a name="dependencies-for-this-guide"></a>本指南中的相关性

成功部署经过身份验证的无线使用本指南，你必须对所有必需的技术部署网络域环境。 您还必须服务器证书部署到你的身份验证 NPS 服务器。

以下部分提供了指向向你显示如何部署这些技术的文档。

#### <a name="network-and-domain-environment-dependencies"></a>网络和域环境相关性

本指南专为已遵循的说明进行操作，Windows Server 2016 中的网络和系统管理员**Core 网络指南**部署 core 网络，或者对于之前已部署核心网络，其中包括广告 DS 中包含的核心技术域名系统 \(DNS\)、 动态主机配置协议 \(DHCP\)、 TCP\/IP、 NPS，以及 Windows Internet 名称服务 \(WINS\)。  

Core 网络指南可以在以下位置：

- Windows Server 2016 [Core 网络指南](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)适用于 Windows Server 2016 Technical 库。 

- [Core 网络指南](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)也会显示在 TechNet 库中，在 Word 格式在[https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683)。

#### <a name="server-certificate-dependencies"></a>服务器证书依赖项
有两个可用选项具有用于使用 802.1 X 身份验证的服务器证书的注册身份验证服务器使用 Active Directory 证书服务 \(AD CS\) 部署自己公钥基础结构或使用 server 证书的注册的公用证书颁发机构 \(CA\)。

##### <a name="ad-cs"></a>广告客户服务
部署经过身份验证的无线网络和系统管理员必须按照 Windows Server 2016 Core 网络助手指南中的说明**802.1 X 有线和无线部署部署服务器证书**。 本指南介绍了如何部署和使用广告客户服务向计算机运行 NPS 自动注册 server 证书。

本指南可以在以下位置。

- Windows Server 2016 Core 网络助手指南[802.1 X 有线和无线部署部署服务器证书](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/cncg/server-certs/deploy-server-certificates-for-802.1x-wired-and-wireless-deployments?f=255&MSPPError=-2147217396)HTML 技术库中的格式。 

##### <a name="public-ca"></a>公共 CA
你可以购买从公开 CA，VeriSign，如服务器证书客户端计算机已经信任。 

客户端计算机信任加利福尼亚信任根证书颁发机构证书应用商店中安装后 CA 证书。 默认情况下运行 Windows 的计算机有多个公共 CA 证书安装在其信任根证书颁发机构证书存储。  

建议你查看的每个此部署方案中采用的技术设计和部署的指南。 这些指南可以帮助你确定是否此部署方案提供服务和你的组织的网络所需的配置。

### <a name="requirements"></a>要求
以下是通过使用本指南中记录的方案部署无线接入基础结构要求：

- 部署之前这种情况下，你必须先购买 802.1X\-可在站点上所需的位置无线涵盖的支持的无线接入点。 计划部分本指南有助于确定必须支持你的接入点的功能。

- Active Directory 域服务 \(AD DS\) 安装一些其他的所需的网络技术，当根据 Windows Server 2016 Core 网络指南中的说明进行操作。  

- 部署广告客户服务，并于 NPS 服务器注册服务器证书。 本指南中使用 PEAP\ MS\ CHAP v2 certificate\ 基于身份验证方法部署时，这些证书是必需的。

- 你的成员是组织的熟悉受你的无线接入点和客户端计算机和在网络上的设备上安装无线网卡 IEEE 802.11 标准。 例如，你的组织中某个人是熟悉射频类型、 802.11 无线验证 \ （WPA2 或 WPA\） 和编码器 \(AES or TKIP\)。

## <a name="what-this-guide-does-not-provide"></a>本指南不提供的内容  
下面是一些本指南不提供的项目：

### <a name="comprehensive-guidance-for-selecting-8021x-capable-wireless-access-points"></a>选择 802.1X\ 全面指导-支持的无线接入点

因为品牌及型号的 802.1X\ 之间有很大不同的支持的无线接入点，本指南不提供有关的详细的信息：  

- 确定哪些品牌或模型的无线接入点最适合你的需求。

- 无线网络上的接入点物理部署。

- 对于无线虚拟本地区域网络 \(VLANs\) 高级如的无线接入点配置。

- 如何配置无线接入点 vendor\ 特定属性 NPS 中的说明进行操作。

此外，术语和设置的名称无线的接入点品牌及型号，而异，并且可能不匹配本指南中使用的设置通用名称。 有关无线接入点配置详细信息，你必须查看由你的无线接入点的制造商提供的产品文档。

### <a name="instructions-for-deploying-nps-server-certificates"></a>有关部署 NPS 服务器证书说明
  
有两个的替代项，用于部署 NPS server 证书。 本指南不会提供完整指导，以帮助您确定哪个替代满足你的需求。 一般情况下，但是，你面对的选择是：

- 购买公共 CA，如 VeriSign，已信任 Windows\ 基于客户端的证书。 此选项特别推荐的较小的网络。

- 通过使用广告客户服务部署公钥基础结构 \(PKI\) 网络上。 提供前面提到的部署指南中这推荐的大多数网络，以及有关如何部署服务器证书广告客户使用的说明进行操作。

### <a name="nps-network-policies-and-other-nps-settings"></a>NPS 网络策略和其他 NPS 设置

在运行时所做的配置设置除外**配置 802.1 X**向导中，述本指南，本指南不提供手动配置 NPS 条件、 限制或其他 NPS 设置的详细的信息。

### <a name="dhcp"></a>DHCP

此部署指南不提供有关设计，或为无线 Lan 部署 DHCP 子网信息。

## <a name="technology-overviews"></a>技术概述
以下是用于部署的无线接入技术概述：

### <a name="ieee-8021x"></a>IEEE 802.1 X
IEEE 802.1 X 标准定义用于提供身份验证的网络访问权限以太网网络 port\ 基于网络访问控制。 此 port\ 基于网络访问控制使用物理特征交换 LAN 基础结构进行身份验证到 LAN 端口连接的设备。 验证过程失败时，可以被拒绝访问该端口。 虽然此标准专为有线的以太网网络，但它已修改，可使用 802.11 无线 Lan。

### <a name="8021x-capable-wireless-access-points-aps"></a>802.1X\ 的支持的无线接入点 \(APs\)
此方案需要一个或多个 802.1X\ 部署-支持远程身份验证 Dial\-In 用户服务 \(RADIUS\) 协议与兼容的无线接入点。

802.1 X 和部署在 RADIUS 基础结构 NPS 服务器，如 RADIUS 服务器时 RADIUS\ 兼容接入点称为*RADIUS 客户端*。

### <a name="wireless-clients"></a>无线客户端
本指南提供了全面的配置的详细信息来提供 802.1 X 身份验证的 domain\ 成员用户使用运行 Windows 10、 Windows 8.1 和 Windows 8 的客户端无线连接到该网络的访问。 必须到域加入计算机，以便成功建立身份验证的访问权限。

>[!NOTE]
>你还可以使用无线客户端与运行 Windows Server 2016、 Windows Server 2012 R2 和 Windows Server 2012 的计算机。

### <a name="support-for-ieee-80211-standards"></a>IEEE 802.11 标准的支持
受支持的 Windows 和 Windows Server 操作系统提供 802.11 无线网络 built\ 在支持。 在以下操作系统，已安装的 802.11 无线网络适配器显示为在网络和共享中心中的无线网络连接。 

虽然 built\ 在支持 802.11 无线网络，但无线组件的 Windows 是取决于以下：

- 无线网络适配器的功能。 无线 LAN 或你需要的无线安全标准，必须支持安装无线网络适配器。 例如，如果无线网络适配器不支持 Wi\ Fi 保护访问 \(WPA\)，不能启用，或配置 WPA 安全选项。

- 无线网络适配器驱动程序的功能。 允许你为配置无线网络的选项，无线网络适配器驱动程序必须支持报告的所有其功能到 Windows。 验证你的无线网络适配器驱动程序面向你的操作系统功能。 此外请确保驱动程序处于最新版本，检查 Microsoft 更新或无线网络适配器供应商的网站。

下表显示了传输速率和常见 IEEE 802.11 无线标准的频率。

|标准|频率|比特率传输|使用情况|
|-------------|---------------|--------------------------|---------|  
|802.11|S\ Band 行业、 科学型和医疗 \(ISM\) 频率范围 \ (2.4 到 2.5 GHz\)|2 兆位 / 第二个 \(Mbps\)|过时。 不常用。|  
|802.11 b|S\ Band ISM|11 Mbps|常用。|  
|802.11 a|C\ Band ISM \ (5.725 到 5.875 GHz\)|54 Mbps|不常用的截止到支出和有限的范围。|  
|802.11 g|S\ Band ISM|54 Mbps|广泛使用。 802.11 g 设备是与 802.11 b 兼容设备。|  
|802.11 n \2.4 和 5.0 g h z|C\ Band 和 S\ Band ISM|250 Mbps|基于 pre\ 批准 IEEE 802.11 n 标准设备成为 2007 年 8 月可用。 许多 802.11 n 设备是与 802.11 a、 兼容 b 和 g 设备。|
|802.11ac |5 GHz |6.93 Gbps |802.11ac，于 2014 年，通过 IEEE 批准更可缩放和 802.11 n、 超过更快，并在接入点和无线客户端支持它进行部署。|

### <a name="wireless-network-security-methods"></a>无线网络安全方法
**无线网络安全措施**是非正式进行分组无线身份验证的 \ （有时称为无线 security\） 和无线安全加密。 若要防止未经授权的用户的无线网络的访问，并保护无线传输成对使用无线身份验证和加密。 

配置时无线安全设置无线网络策略的组策略中，有多个组合，可供选择。 但是，对于 802.1 X 身份验证的无线部署受支持 WPA2\ 企业版、 WPA\ 的企业版和使用 802.1 X 身份验证标准打开。

>[!NOTE]
>配置无线网络策略，时，你必须选择**WPA2\ 企业**， **WPA\ 企业**，或**打开使用 802.1 X**才能获得对设置所需的 802.1 X 身份验证无线部署访问权限。  

#### <a name="wireless-authentication"></a>无线身份验证
本指南建议以下无线身份验证标准使用 802.1 X 身份验证无线部署。  

**Wi\ Fi 受保护访问 – 企业 \(WPA\-Enterprise\)** WPA 是由遵守 802.11 无线安全性协议 WiFi 联盟开发临时标准。 WPA 协议，所开发大量前面有线等效保密 \(WEP\) 协议中所发现的严重的漏洞的响应。

WPA\ 企业通过 WEP 通过提供改进了的安全性：  

1. 需要使用 802.1 X EAP 框架基础结构，以确保集中相互身份验证和动态的密钥管理作为的身份验证  

2. 增强消息完整性检查 \(MIC\)，以免的标题和负载完整性检查的值的 \(ICV\)  

3. 实现阻止重播攻击帧计数器  

**Wi\ Fi 受保护的访问 2-企业 \(WPA2\-Enterprise\)**等 WPA\ 企业标准、 WPA2\ 企业使用 802.1 X 并 EAP 框架。 WPA2\ 企业多用户和大型托管的网络提供增强数据保护。 WPA2\ 企业是一个强大的协议，专门用于阻止通过验证身份验证的服务器通过网络用户的未经授权的网络访问权限。  

#### <a name="wireless-security-encryption"></a>无线安全加密
无线安全加密用于保护无线客户端与无线接入点之间发送无线传输。 无线安全加密配合使用与所选的网络安全身份验证方法。 默认情况下，计算机运行的 Windows 10、 Windows 8.1 和 Windows 8 支持两个加密标准：

1. **临时键完整性协议**\(TKIP\) 是最初旨在提供更安全的无线加密比无法提供本质上是弱有线等效保密 \(WEP\) 协议的较旧的加密协议。 通过 IEEE 802.11 i 设计 TKIP 任务组和而无需旧的硬件更换替换 WEP Wi\ Fi 联盟。 TKIP 是一套算法封装 WEP 负载中，并允许用户的传统 WiFi 设备升级到 TKIP 而不必更换了硬件。 WEP，如 TKIP 为基础使用 RC4 流式传输加密算法。 新的协议，但是，进行每个数据包加密使用独特的密钥和的键可以使用 wep 比更强大。 虽然 TKIP 很有用升级旨在使用仅 WEP 的较旧的设备上的安全，它不能解决所有面临无线 Lan、 的安全问题，并在大多数情况下并不功能充分强大的敏感政府或公司的数据传输保护。  

2. **高级加密标准**\(AES\) 是商业和政府数据加密的首选的加密协议。 AES 提供无线传输安全于 TKIP 或 WEP 更大程度。 与 TKIP 和 WEP，不同 AES 要求支持 AES 标准的无线硬件。 AES 是 symmetric\ 键加密标准使用 AES\ 128、 AES\ 192 和 AES\ 256 三个块密码。

在 Windows Server 2016，以下 AES\ 基于无线加密是方法可用于在属性无线配置文件的配置时选择 WPA2\-企业版建议的身份验证方法。

1. **AES\ CCMP**。 应对模式密码块链连接功能消息身份验证代码协议 \(CCMP\) 实现 802.11 i 标准和专为更高版本的安全加密比 WEP，通过提供使用 128 位 AES 加密密钥。
2. **AES\ GCMP**。 Galois 计数器模式协议 \(GCMP\) 受 802.11ac、 效率高于 AES\ CCMP 并提供更好的性能，用于无线客户端。 GCMP 使用 256 一点 AES 加密密钥。

> [!IMPORTANT]
> 有线等效保密 \(WEP\) 是原始无线安全标准用于加密网络通信。 不应部署 WEP 网络上由于 well\ 已知这种过期形式的安全漏洞。

### <a name="active-directory-doman-services-ad-ds"></a>Active Directory 域服务 \(AD DS\)
广告 DS 提供的分布式的数据库存储，并管理 directory\ 启用应用程序从网络资源和 application\ 特定的数据的信息。 管理员可以使用广告 DS 组织的网络、 用户、 计算机和其他设备，如元素到分层包容结构。 分层包容结构每个域中包括 Active Directory 林林中域和单位 \(OUs\)。 广告 DS 运行的服务器称为*域控制器*。  

广告 DS 包含计算机帐户的用户帐户，用户凭据进行身份验证和评估无线连接的授权 IEEE 802.1 X 和 PEAP\ MS\ CHAP v2 所需的帐户属性。

### <a name="active-directory-users-and-computers"></a>Active Directory 的用户和计算机
Active Directory 用户和计算机是广告 DS 包含帐户表示物理实体，例如计算机、 用户，或者安全组的一个组成部分。 一个*安全组*用户或计算机管理员可以作为一个整体进行管理的帐户的集锦。 属于特定组的用户和计算机的帐户被称为*组成员*。  

### <a name="group-policy-management"></a>组策略管理
组策略管理使您能够基于 directory\ 更改和配置管理用户和计算机的设置，包括安全和用户的信息。 使用组策略定义组的用户和计算机配置。 使用组策略，你可以指定的注册表项、 安全性、 安装软件、 脚本、 文件夹重定向、 远程安装服务和维护 Internet Explorer 的设置。 组策略中包含你创建的设置的组策略对象 \(GPO\)。 通过将 GPO 与所选的 Active Directory 系统容器相关联，站点、 域和华丽绚烂，你可以将 GPO 的设置应用到的用户和这些 Active Directory 容器中的计算机。 若要在企业中管理组策略对象，你可以使用组策略编辑器中管理 Microsoft 管理控制台 \(MMC\)。  

本指南提供有关如何指定设置无线网络中的详细的说明了 \ (IEEE 802.11\) 组策略管理策略扩展。 无线网络 \ (IEEE 802.11\) 策略为必要连接配置 domain\ 成员无线客户端计算机和无线设置 802.1 X 身份验证的无线接入。

### <a name="server-certificates"></a>服务器证书
此部署方案需要为每个 NPS 服务器执行 802.1 X 身份验证的服务器证书。  

服务器证书是数字文档常用进行身份验证和开放网络上的安全信息。 证书牢固将公钥绑定到负有相应的专用键。 证书由颁发进行数字签名，他们可以发出的用户、 一台电脑或服务。  

证书颁发机构 \(CA\) 是负责建立和保证真实性的属于主题公钥实体 \ （通常是用户或 computers\） 或其他 Ca。 证书颁发机构的活动，可能包括将公钥绑定到特征通过管理证书序列号以及撤销证书的签名证书的名称。  

Active Directory 证书服务 \(AD CS\) 是为 CA 网络问题证书服务器角色。 广告客户服务证书基础结构，也称为*公钥基础结构 \(PKI\)*，可自定义为提供服务适用于企业的证书颁发和管理。

### <a name="eap-peap-and-peap-ms-chap-v2"></a>EAP、 PEAP 和 PEAP\ MS\ CHAP v2
可扩展身份验证协议 \(EAP\) 扩展协议 Point\ to\ 点的任意长度交换 \(PPP\) 通过允许使用凭据和信息的额外的身份验证方法。 使用 EAP 身份验证，这两个网络访问客户端和验证器 \ （如 NPS server\) 必须支持 EAP 类型都相同成功进行身份验证的发生。 Windows Server 2016 包含一个 EAP 基础结构，支持两个 EAP 类型，并将 EAP 消息传递到 NPS 服务器的能力。 你可以通过使用 EAP，支持额外的身份验证方案，称为*EAP 类型*。 受 Windows Server 2016 的 EAP 类型是：  

- 传输层安全性 \(TLS\)

- Microsoft 挑战握手验证协议版本 2 \ (MS\ CHAP v2\)

>[!IMPORTANT]
>强大的 EAP 类型 \ （如这些根据 certificates\） 提供更好的安全性 brute\ 强制攻击字典攻击，密码猜测比 password\ 基于身份验证协议攻击 \ （如 CHAP 或 MS\ CHAP 版本 1 \)。

保护的 EAP \(PEAP\) 使用 TLS 创建加密的通道之间身份验证 PEAP 客户端，如无线的计算机，并且 PEAP authenticator，如 NPS 服务器或其他 RADIUS 服务器。 PEAP 不指定身份验证方法，但它提供其他 EAP 身份验证协议的其他安全 \ （如 EAP\ MS\ CHAP v2\)，可以通过提供 PEAP TLS 加密通道中运行。 PEAP 用作访问客户连接到你的组织的网络以下类型的网络访问权限服务器 \(NASs\) 经过身份验证方法：

- 802.1X\ 的支持的无线接入点

- 802.1X\-支持身份验证交换机

- 运行 Windows Server 2016 和远程访问服务 \(RAS\) 的计算机，配置为虚拟专用网络 \(VPN\) 服务器和 / 或 DirectAccess 服务器

- 运行 Windows Server 2016 和远程桌面服务计算机

PEAP\ MS\ CHAP v2 就能更易于，因为用户身份验证执行通过使用凭据 password\ 基于比 EAP\ TLS 部署 \ （用户名和 password\），而不是证书或智能卡。 仅 NPS 或其他 RADIUS 服务器需要具有证书。 NPS 服务器证书用于 NPS 服务器的身份验证过程证明 PEAP 客户端到其身份。

本指南提供来配置你的无线客户端和 NPS server\(s\) 使用 802.1 X 身份验证访问 PEAP\ MS\ CHAP v2 的说明进行操作。

### <a name="network-policy-server"></a>网络策略服务器
网络策略服务器 \(NPS\) 允许你集中配置和使用远程验证 Dial\-In 用户服务 \(RADIUS\) 服务器和 RADIUS 代理服务器管理网络策略。 当你部署 802.1 X 无线接入 NPS 是必需的。

在你 802.1 X 的无线接入点配置为在 NPS RADIUS 客户端上时，NPS 处理发送的接入点的连接请求。 在连接处理请求 NPS 执行身份验证和授权。 身份验证确定客户端其所出示有效的凭据。 如果 NPS 成功进行身份验证请求客户端，然后 NPS 确定是否客户端获得授权，以使请求的连接，并使或拒绝该连接。 这是更多详细介绍，如下所示：

#### <a name="authentication"></a>身份验证

成功相互 PEAP\ MS\ CHAP v2 身份验证有两个主要的部分：

1.  客户端进行 NPS 服务器身份验证。  在此阶段相互身份验证，NPS 服务器发送它服务器证书向客户端计算机，以便客户可以验证的证书 NPS 服务器的身份。 成功进行身份验证 NPS 服务器的客户端计算机必须信任发出 NPS 服务器证书 CA。 客户端信任此 CA 时客户端计算机上信任根证书颁发机构证书应用商店中为 CA 证书。

    如果部署专用 CA，CA 证书自动安装在信任根证书颁发机构证书官方商城的当前用户和本地计算机时组策略刷新域成员客户端计算机上。 如果你决定部署从公共 CA 服务器证书，请确保公共 CA 证书已在信任根证书颁发机构证书应用商店中。  

2.  NPS 服务器验证用户的身份。 客户端成功进行身份验证 NPS 服务器后，客户将发送到 NPS 服务器，验证用户的用户帐户数据库中 Active Directory 域服务 \(AD DS\) 防御的凭据用户基于 password\ 的凭据。

如果有效凭据，身份验证成功 NPS 服务器开始处理连接请求的授权阶段。 如果凭据不可有效，验证失败 NPS 发送拒绝访问消息和连接请求被拒绝。  

#### <a name="authorization"></a>授权

运行 NPS 服务器执行授权，如下所示：  

1. 限制在中的用户或计算机帐户 dial\ 中属性广告 DS NPS 检查。 每个用户和计算机的帐户中的 Active Directory 用户和计算机包含多个属性，包括上发现的那些项**Dial\ 中**选项卡。在此实验中，在**网络访问权限**、如果值为**允许访问**，授权的用户或计算机连接到该网络。 如果值为**拒绝访问**，用户或计算机未授权连接到该网络。 如果值为**控制 NPS 网络策略访问**、 NPS 评估配置的网络策略，以确定是否用户或计算机有权连接到该网络。 

2. 然后，NPS 处理自己的网络策略查找符合连接请求的策略。 如果找到匹配的策略，则 NPS 授权或拒绝根据该策略配置的连接。  

身份验证和授权是否成功，以及相应的网络策略授予访问权限，NPS 授予访问权限的网络，并且用户和计算机可以连接到网络资源他们拥有的权限。  

>[!NOTE]
>若要部署无线的访问权限，您必须配置 NPS 策略。 本指南介绍了如何使用**配置 802.1 X 向导**NPS 来为 802.1 X 身份验证的无线接入创建 NPS 策略中。  

### <a name="bootstrap-profiles"></a>引导配置文件
在 802.1X\-验证无线网络，无线客户必须提供安全进行 RADIUS 服务器身份验证，才能进行连接到该网络的凭据。 保护 eap \[PEAP\]\-Microsoft 挑战握手验证协议版本 2 \[MS\-CHAP v2\]，安全凭据是用户名和密码。 对于 EAP\ 传输层安全性 \[TLS\] PEAP\ TLS 安全凭据是证书，如客户端用户和计算机证书或智能卡或。

当连接到的网络配置为执行 PEAP\ MS\ CHAP v2、 PEAP\ TLS 或 EAP\ TLS 身份验证，默认情况下，Windows 无线客户端还必须验证由 RADIUS 服务器发送一个计算机证书。 身份验证的每个会话 RADIUS 服务器发送该计算机证书通常称为服务器证书。

在两种方式之一其服务器证书如上文所述，你都可以发出 RADIUS 服务器： 从商业 CA \ (VeriSign，Inc.，如 \)，或从你部署网络的专用 CA。 如果 RADIUS 服务器发送计算机证书颁发的商业 CA 已在客户端信任根证书颁发机构证书应用商店中安装了根证书，无线客户可以验证 RADIUS 服务器计算机证书，则无论是否无线客户端已加入域 Active Directory。 在此情况下无线客户可以连接到无线网络，然后你可以加入域的计算机。

>[!NOTE]
>可以禁用需要验证的服务器证书客户的行为，但服务器证书验证建议不要禁用生产环境中。

无线引导配置文件都是临时配置文件的方式使无线客户端用户连接到 802.1X\ 配置-经过身份验证的计算机已加入域和之间之前的无线网络 /，或者才能用户成功登录到域首次使用给定无线计算机。  本部分总结了尝试加入域，或者用户可以使用 domain\ 加入无线计算机首次登录到域加入无线计算机时遇到问题。

有关部署的 IT 管理员的用户无法物理将计算机连接到有线的以太网网络，若要将计算机连接到域，和计算机未安装必需的发出根中安装证书，CA 其**信任根证书颁发机构**证书应用商店中，您可以使用无线连接的临时配置文件，称为配置无线客户端*引导个人资料*连接到无线网络。

一个*引导个人资料*不再需要在验证 RADIUS 服务器计算机证书。 此临时配置，无线用户可以将计算机连接到域，届时无线网络 \ (IEEE 802.11\) 策略应用和计算机上自动安装相应根证书。

组策略应用时，在计算机; 上应用的一个或多个无线连接配置文件，强制相互身份验证的要求启动配置文件不再需要和被删除。 加入域的计算机并后重启计算机，用户可以使用无线连接到域登录。

有关使用这些技术的无线接入部署进程概述，请参阅[无线访问部署概述](b-wireless-access-deploy-overview.md)。
