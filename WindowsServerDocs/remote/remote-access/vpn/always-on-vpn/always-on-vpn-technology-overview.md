---
title: 始终启用 VPN 技术概述
description: '此页面 provies 简要概述了与详细文档链接的始终启用 VPN 技术。 '
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: fd3f7c6ca8555e270aabf04bbee6800ed284080c
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031321"
---
# 始终启用 VPN 技术概述
>适用于： Windows Server （半年频道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows 10

& #171; [**上一步：** 了解有关始终启用 VPN 增强功能](always-on-vpn-enhancements.md)<br>
& #187; [**下一步：** 了解相关始终启用 VPN 的高级功能的信息](deploy/always-on-vpn-adv-options.md)

此部署，你必须安装新的远程访问服务器正在运行 Windows Server 2016 中，以及修改现有部署基础结构的一部分。

下图显示了部署始终启用 VPN 所需的基础结构。

![始终启用 VPN 基础结构](../../../media/Always-On-Vpn/Ao-Vpn-Overview.jpg)

下图所示的连接过程包括以下步骤：

1. Windows 10 VPN 客户端使用的公用 DNS 服务器，为 VPN 网关 IP 地址执行名称解析查询。

2. 使用 DNS 返回的 IP 地址，VPN 客户端发送到 VPN 网关连接请求。

3. VPN 网关也被配置为远程身份验证拨入用户服务 \(RADIUS\) 客户端;VPN RADIUS 客户端将连接请求发送到组织/公司 NPS 服务器进行连接请求处理。

4. NPS 服务器处理连接请求，包括执行授权和身份验证，并确定是否要允许或拒绝连接请求。

5. NPS 服务器转发到 VPN 网关访问接受或拒绝访问的响应。

6. 启动或终止连接根据 VPN 服务器从 NPS 服务器接收响应。

在上图中所示的每个基础结构组件的详细信息，请参阅以下部分。

>[!NOTE]
>如果你已经有一些网络上部署这些技术，可用于说明此部署指南中执行的技术的其他配置为此部署目的。

## 域名系统 (DNS)

内部和外部域名系统 (DNS) 区域是必需的这假定内部区域是外部的区域 （例如，corp.contoso.com 和 contoso.com） 委派的子域。

了解更多有关[域名系统 (DNS)](../../../../networking/dns/dns-top.md)或[核心网络指南](../../../../networking/core-network-guide/core-network-guide.md)。




>[!NOTE] 
>其他 DNS 设计，如拆分式 DNS （使用相同的域名内部和外部在单独的 DNS 区域） 或不相关的内部和外部域 （例如，contoso.local 和 contoso.com） 也是可行。 有关部署拆分式 DNS 的详细信息，请参阅[使用拆分/大脑 DNS 部署的 DNS 策略](../../../../networking/dns/deploy/split-brain-DNS-deployment.md)。

## 防火墙

请确保你防火墙允许通信所需的 VPN 和 RADIUS 通信正常。

有关详细信息，请参阅[配置 RADIUS 通信的防火墙](../../../../networking/technologies/nps/nps-firewalls-configure.md)。

## 为 RAS 网关 VPN 服务器的远程访问

在 Windows Server 2016 中，远程访问服务器角色旨在执行以及路由器和远程访问服务器;因此，它支持丰富的功能。 有关此部署指南，你需要只有一小部分的这些功能： 对 IKEv2 VPN 连接和 LAN 路由的支持。

IKEv2 VPN 隧道协议的注释 7296 Internet 工程任务组请求中所述。 IKEv2 的主要优势在于它能够规避基础网络连接中断。 例如，如果连接是暂时丢失或用户将从一个网络客户端计算机移到另一台，IKEv2 自动还原 VPN 连接时重新建立网络连接 — 所有无需用户干预。

通过使用 RAS 网关，你可以部署进行最终用户提供远程访问你的组织的网络和资源的 VPN 连接。 部署始终启用 VPN 维护客户端和组织的网络之间的持续性连接，每当远程计算机连接到 Internet。 使用 RAS 网关，你也可以之间主要 office 和分支机构，例如创建不同位置，两个服务器之间的站点到站点 VPN 连接并使用网络地址转换 \(NAT\) 以便网络内的用户可以访问外部资源，如 Internet。 此外，RAS 网关支持边界网关协议 (BGP)，可提供动态路由服务，当你远程机构位置还可以支持 BGP 的边缘网关。

你可以通过使用 Windows PowerShell 命令和远程访问 Microsoft 管理控制台 (MMC) 管理远程访问服务 (RAS) 网关。



## 网络策略服务器 (NPS)

NPS，你可以创建并实施组织级网络访问策略连接请求身份验证和授权。 为远程身份验证拨入用户服务 (RADIUS) 服务器使用 NPS 时，你将网络访问服务器，如 VPN 服务器配置为 NPS 中 RADIUS 客户端。

你还配置 NPS 用于授权连接请求的网络策略和配置 RADIUS 记帐 NPS 将记帐信息日志文件在本地硬盘上或 Microsoft SQL Server 数据库中。

有关详细信息，请参阅[网络策略服务器 (NPS)](../../../../networking/technologies/nps/nps-top.md)。


## Active Directory 证书服务

证书颁发机构 (CA) 服务器是运行 Active Directory 证书服务的证书颁发机构。 VPN 配置要求基于 Active Directory 的公钥基础结构 (PKI)。

组织可以使用 AD CS 通过将用户、 设备或服务的身份绑定到相应的公钥增强安全性。 AD CS 还包括允许你管理证书注册和各种可扩展的环境中吊销的功能。 有关详细信息，请参阅[Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)和[公共密钥基础结构设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)。

在部署完成，将在 CA 上配置以下证书模板。

-   用户身份验证证书模板

-   VPN 服务器身份验证证书模板

-   NPS 服务器身份验证证书模板

### 证书模板

通过允许你为选定的任务预配置的颁发证书，证书模板可以大大简化管理证书颁发机构 (CA) 的任务。 证书模板 mmc 管理单元可以执行以下任务。

-   查看每个证书模板的属性。

-   复制和修改证书模板。

-   控制哪些用户和计算机可以读取模板并注册证书。

-   执行与证书模板有关其他管理任务。

证书模板是不可或缺的组成部分企业证书颁发机构 (CA)。 它们是环境中，这是一组规则和证书注册、 使用和管理的格式的证书策略的重要元素。

有关详细信息，请参阅[证书模板](https://technet.microsoft.com/library/cc730705.aspx)。

### 数字服务器证书

此部署指南提供了使用 Active Directory 证书服务 (AD CS) 注册和自动注册到远程访问和 NPS 基础结构服务器的证书的说明。 AD CS 允许你生成公钥基础结构 (PKI)，并为你的组织提供公钥加密、 数字证书和数字签名功能。

当你网络上的计算机之间进行身份验证使用数字服务器证书时，证书提供：

1.  通过加密机密性。

2.  通过数字签名的完整性。

3.  通过将证书密钥与计算机的网络上的计算机、 用户或设备帐户相关联的身份验证。

有关详细信息，请参阅[AD CS 分步指南： 两个层 PKI 层次结构部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)。

## Active Directory 域服务 (AD DS)

AD DS 提供一种分布式的数据库的存储，并从目录的应用程序管理网络资源和特定于应用程序的数据的信息。 管理员可以使用 AD DS 将元素的网络，如用户、 计算机和其他设备的组织到内嵌层次结构。 内嵌层次结构包括每个域中 Active Directory 林中的林和组织单位 (Ou) 中的域。 运行 AD DS 的服务器称为域控制器。

AD DS 包含用户帐户、 计算机帐户和帐户属性所需的受保护的可扩展身份验证协议 (PEAP) 进行身份验证用户凭据并评估为 VPN 连接请求的授权。 有关部署 AD DS 的信息，请参阅 Windows Server 2016[核心网络指南](../../../../networking/core-network-guide/Core-Network-Guide.md)。



在此部署中的步骤完成，将域控制器上配置以下各项。

-   启用组策略中的计算机和用户的证书自动注册

-   创建 VPN 用户组

-   创建 VPN 服务器组

-   创建 NPS 服务器组

### Active Directory 用户和计算机

Active Directory 用户和计算机是包含表示物理实体，例如计算机、 一个人或安全组的帐户的 AD DS 的组成部分。 安全组是用户或计算机帐户的管理员可以管理作为单个单元的集合。 属于特定组的用户和计算机帐户被称为组成员。

在 Active Directory 用户和计算机的用户帐户具有拨入的属性，NPS 评估授权过程中的除非用户帐户的**网络访问权限**属性设置为**NPS 网络策略控制访问**. 这是默认设置为所有用户帐户。 但是，在某些情况下，此设置可能会有不同的配置，会阻止用户使用 VPN 连接。 若要防止这种可能性，你可以配置 NPS 服务器忽略用户帐户拨入属性。

有关详细信息，请参阅[配置 NPS 忽略用户帐户拨入属性](../../../../networking/technologies/nps/nps-np-configure.md#configure-nps-to-ignore-user-account-dial-in-properties)。



### 组策略管理

组策略管理实现基于目录的更改和配置管理用户和计算机设置，包括安全和用户信息。 使用组策略来定义组的用户和计算机配置。

使用组策略，你可以指定注册表项、 安全、 软件安装、 脚本、 文件夹重定向、 远程安装服务和 Internet Explorer 维护设置。 组策略对象 (GPO) 中包含你创建的组策略设置。 通过将 GPO 与所选的 Active Directory 系统容器关联 — 站点、 域和 Ou — 可以将 GPO 的设置应用于的用户和计算机的这些 Active Directory 容器。 若要在企业内管理组策略对象，你可以使用组策略管理编辑器 Microsoft 管理控制台 (MMC)。


## Windows 10 VPN 客户端

除了服务器组件，请确保你配置为使用 VPN 客户端计算机运行 windows 10 周年更新 (version1607)。 Windows 10 VPN 客户端必须是域加入到 Active Directory 域。


Windows 10 VPN 客户端是高度可配置，并提供很多选项。 为了更好地说明这种情况下使用的特定功能，表 1 标识此部署引用的 VPN 功能类别和特定配置。 你将通过使用 VPNv2 配置服务提供程序 (CSP) 在此部署的后面部分所述来配置这些功能的个别设置。 

表 1.  VPN 功能和此部署中所述的配置

| **VPN 功能** | **部署方案配置**         |
|-----------------|-----------------------------------------------|
| 连接类型 | 本机 IKEv2                                  |
| 路由         | 拆分隧道                               |
| 名称解析 | 域名信息列表和 DNS 后缀   |
| 触发      | 始终启用和受信任的网络检测       |
| 身份验证  | PEAP TLS 与受 TPM 保护用户证书 |
---

>[!NOTE] 
>PEAP-TLS 和 TPM 分别为"受保护的可扩展身份验证协议与传输层安全"和"受信任的平台模块"。

### VPNv2 CSP 节点

在此部署中，你可以使用 ProfileXML VPNv2 CSP 节点创建 VPN 配置文件传递到 windows 10 客户端计算机的。 配置服务提供程序 (Csp) 是公开 Windows 客户端; 中的各种管理功能的接口从概念上来说，Csp 工作类似于组策略的工作原理。 每个云解决方案提供商有表示单个设置的配置节点。 此外如组策略设置，可以将绑定到注册表项、 文件、 权限等的云解决方案提供商设置。 你与如何使用组策略管理编辑器以配置组策略对象 (Gpo) 类似，配置通过使用 Microsoft Intune 等移动设备管理 (MDM) 解决方案 CSP 节点。 Intune 等 MDM 产品提供操作系统中配置云解决方案提供商的用户友好的配置选项。

![云解决方案提供商配置的移动设备管理](../../../media/Always-On-Vpn/Vpn-Mdm.jpg)

但是，不能配置某些云解决方案提供商节点直接通过类似 Intune 管理员控制台的用户界面 (UI)。 在这些情况下，你必须配置的开放移动联盟统一资源标识符 (OMA-URI) 设置手动。 通过使用 OMA 设备管理协议 (OMA-DM)，大多数现代 Apple、 Android 和 Windows 设备支持的通用设备管理规范配置 OMA Uri。 只要它们遵循 OMA DM 规范，所有 MDM 产品应以相同的方式都与这些操作系统中。

Windows 10 提供许多 Csp，但此部署侧重于使用 VPNv2 CSP 配置 VPN 客户端。 VPNv2 CSP 通过唯一的云解决方案提供商节点在 windows 10 中允许每个 VPN 配置文件设置的配置。 也包含在 VPNv2 CSP 是调用*ProfileXML*，这允许你配置的所有设置的节点在一个节点中而不是单独。 有关 ProfileXML 的详细信息，请参阅此部署更高版本中的"ProfileXML 概述"部分。 有关每个 VPNv2 CSP 节点的详细信息，请参阅[VPNv2 CSP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-csp)。



## 后续步骤

- [了解的一些高级始终启用 VPN 功能](deploy/always-on-vpn-adv-options.md)

- [开始菜单规划始终启用 VPN 部署](deploy/always-on-vpn-deploy-deployment.md)


---

## 相关主题
- [Microsoft 服务器软件支持 Microsoft Azure 虚拟机](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)： 本文将讨论如何在 Microsoft Azure 虚拟机环境 （基础结构作为-服务） 中运行 Microsoft 服务器软件的支持策略。

- [远程访问](../../Remote-Access.md)： 本主题提供 Windows Server 2016 中的远程访问服务器角色的概述。

- [Windows 10 VPN 技术指南](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide)： 本指南将指导你完成将使 Windows 10 客户端在企业 VPN 解决方案以及如何配置你的部署中的决策。 本指南引用 VPNv2 配置服务提供程序 (CSP)，并提供使用 Microsoft Intune 和用于 Windows10 的 VPN 配置文件模板的移动设备管理 (MDM) 配置说明。

- [核心网络指南](../../../../networking/core-network-guide/Core-Network-Guide.md)： 本指南提供说明了如何规划和部署完全正常运行的网络和一个新的林在新的 Active Directory 域所需的核心组件。

- [域名系统 (DNS)](../../../../networking/dns/dns-top.md): 本主题提供概述的域名系统 (DNS)。 Windows Server 2016 中 DNS 是你可以通过使用服务器管理器或 Windows PowerShell 命令来安装服务器角色。 如果你要安装新的 Active Directory 林和域，DNS 将自动安装 Active directory 作为的林和域的全局目录服务器。 

- [Active Directory 证书服务概述](https://technet.microsoft.com/library/hh831740.aspx)： 本文档提供在 Windows Server® 2012 Active Directory 证书服务 (AD CS) 的概述。 AD CS 是允许你生成公钥基础结构 (PKI)，并为你的组织提供公钥加密、 数字证书和数字签名功能的服务器角色。

- [公共密钥基础结构设计指南](https://social.technet.microsoft.com/wiki/contents/articles/2901.public-key-infrastructure-design-guidance.aspx)： 此 wiki 提供了设计指南公钥基础结构 (Pki)。 配置 PKI 和证书颁发机构 (CA) 层次结构之前，你应该知道你的组织的安全策略和证书的做法声明 (CPS)。

- [AD CS 分步指南： 两个层 PKI 层次结构部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)： 此分步指南介绍了设置基本配置 Active Directory® 证书服务 (AD CS) 在实验室环境中所需的步骤。 Windows Server® 2008 R2 中的 AD CS 提供用于创建和管理软件安全系统使用公钥技术中使用公钥证书的可自定义服务。

- [网络策略服务器 (NPS)](../../../../networking/technologies/nps/nps-top.md): 本主题提供 Windows Server 2016 中的网络策略服务器的概述。 通过网络策略服务器 (NPS)，你可以针对连接请求身份验证和授权创建并实施组织级网络访问策略。 

---
