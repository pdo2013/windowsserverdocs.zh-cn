---
title: 服务器证书部署计划
description: 本主题介绍指南部署服务器证书 802.1 X 有线和无线部署部分
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eacfa404da91d14a7a7328646c2320be8220000c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="server-certificate-deployment-planning"></a>服务器证书部署计划

>适用于：Windows Server（半年通道），Windows Server 2016

部署服务器证书之前，你必须计划以下各项：  
  
-   [套餐基本服务器配置](#bkmk_basic)  
  
-   [套餐域访问](#bkmk_domain)  
  
-   [计划你的 Web 服务器上的位置和虚拟目录的名称](#bkmk_virtual)  
  
-   [计划 DNS 别名 (CNAME) 记录的你的 Web 服务器](#bkmk_cname)  
  
-   [计划 CAPolicy.inf 的配置](#bkmk_capolicy)  
  
-   [在 CA1 CDP 和 AIA 扩展的套餐配置](#bkmk_cdp)  
  
-   [计划 CA 和 Web 服务器之间复制操作](#bkmk_copy)  
  
-   [计划 CA 上的服务器证书模板配置](#bkmk_template)  
  
## <a name="bkmk_basic"></a>套餐基本服务器配置  
在你打算作为你的证书颁发机构和 Web 服务器利用这两台计算机上安装了 Windows Server 2016 后，你必须将计算机重命名和指定并配置本地计算机的静态 IP 地址。  
  
有关详细信息，请参阅 Windows Server 2016 [Core 网络指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_domain"></a>套餐域访问  
登录到域，该计算机必须域成员计算机，并且必须在之前的登录尝试广告 DS 创建用户帐户。 此外，大多数本指南中的步骤要求用户帐户的企业管理员或域管理组成员 Active Directory 用户和计算机，因此你必须登录到已相应的组成员的帐户 CA。  
  
有关详细信息，请参阅 Windows Server 2016 [Core 网络指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_virtual"></a>计划你的 Web 服务器上的位置和虚拟目录的名称  
要提供访问权限，CRL 和加拿大证书的其他计算机，你必须在你的 Web 服务器上虚拟目录中存储这些商品。 本指南中的虚拟目录位于 Web 服务器 WEB1。 该文件夹位于驱动器"C"及其名为"pki。" 适用于你的部署任何文件夹位置 Web 服务器上，你可以找到你虚拟的目录。  
  
## <a name="bkmk_cname"></a>计划 DNS 别名 (CNAME) 记录的你的 Web 服务器  
资源记录 (CNAME) 别名有时也称为资源的 canonical 名称记录。 借助这些记录，你可以使用多个名称指向一台主机，使其可用于轻松执行诸如主机文件传输协议 （） 目录和 Web 服务器同一台计算机上。 例如，使用这些服务的地图域名系统 (DNS) 主机为的名称，如 WEB1，服务器计算机该主机的别名 (CNAME) 资源记录注册普遍服务器名称 （设备，www）。  
  
本指南提供 Web 服务器配置说明主机你认证颁发机构证书吊销列表 (CRL)。 你可能还想要使用你的 Web 服务器用于其他用途，如主持 FTP 或网站，因为它是为你的 Web 服务器 DNS 创建别名资源记录一个好主意。 本指南中 CNAME 记录名为"pki"，但你可以选择适合你的部署的名称。  
  
## <a name="bkmk_capolicy"></a>计划 CAPolicy.inf 的配置  
安装广告客户服务之前，你必须上的信息是正确的部署 CA 配置 CAPolicy.inf。 CAPolicy.inf 文件包含以下信息：  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=http://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
你必须计划此文件以下各项：  
  
-   **URL**。 示例 CAPolicy.inf 文件的 URL 值为**http://pki.corp.contoso.com/pki/cps.txt**。 这是 pki 的因为本指南中的 Web 服务器 WEB1 名为，并且 DNS CNAME 资源记录。 Web 服务器还加入 corp.contoso.com 域。 此外，在名为"pki"存储证书吊销列表中的 Web 服务器没有虚拟目录。 请确保你提供 URL CAPolicy.inf 文件积分到虚拟目录中的 Web 服务器域中的值。  
  
-   **RenewalKeyLength**。 在 Windows Server 2012 的广告客户服务默认续订关键长度为 2048年。 选择你的主要长度应该时仍能提供与您想要使用的应用程序的兼容性尽可能长时间。  
  
-   **RenewalValidityPeriodUnits**。 示例 CAPolicy.inf 文件具有 5 年 RenewalValidityPeriodUnits 值。 这是因为 CA 的预期生命周期内大约 10 年。 RenewalValidityPeriodUnits 值应反映 CA 或你想要提供注册年数量最多的整体有效期。  
  
-   **CRLPeriodUnits**。 示例 CAPolicy.inf 文件包含 CRLPeriodUnits 值为 1。 这是因为本指南中的证书吊销列表示例刷新间隔 1 周。 该设置与你指定的时间间隔值，必须发布 CA 上的 CRL CRL 储存的 Web 服务器虚拟目录，并提供身份验证过程中的计算机的访问权限。  
  
-   **AlternateSignatureAlgorithm**。 此 CAPolicy.inf 通过实现备用签名格式实现改进了的安全性机制。 如果你仍然拥有的 Windows XP 客户需要从此 CA 证书，则不应实现此设置。  
  
如果你未计划在以后，添加到公钥基础结构的任何附属 Ca，并且你想要阻止的任何附属 Ca 加，你可以向值为 0 CAPolicy.inf 文件添加 PathLength 密钥。 若要添加该键，复制并粘贴到文件以下代码：  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> 不建议你更改 CAPolicy.inf 文件中的任何其他设置，除非你已执行此操作的特定的原因。  
  
## <a name="bkmk_cdp"></a>在 CA1 CDP 和 AIA 扩展的套餐配置  
在上 CA1 配置证书列表吊销 (CRL) Distribution 点 (CDP) 和颁发机构信息的访问权限 (AIA) 设置时，你将需要 Web 服务器和你的域名名称。 你还需要在你 Web 服务器上存储证书吊销列表 (CRL) 和证书颁发机构证书你创建虚拟目录的名称。  
  
你必须在这一步中部署过程中输入 CDP 位置具有格式：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
例如，如果 WEB1 名为你的 Web 服务器，并且你 DNS 别名 CNAME 记录的 Web 服务器"pki"，你域是 corp.contoso.com，并且你虚拟目录名为 pki，CDP 位置：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
必须输入 AIA 位置具有格式：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
例如，如果 WEB1 名为你的 Web 服务器，并且你 DNS 别名 CNAME 记录的 Web 服务器"pki"，你域是 corp.contoso.com，并且你虚拟目录名为 pki，AIA 位置：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>计划 CA 和 Web 服务器之间复制操作  
发布 CRL，CA 中的证书 CA 访问 Web 服务器虚拟目录，你可以运行 certutil-crl 命令后上 CA 配置 CDP 和 AIA 的位置。 确保你在 CA 属性配置正确路径**扩展**运行此命令使用本指南中的说明进行操作前选项卡。 此外，复制到 Web 服务器企业 CA 证书，你必须已创建的 Web 服务器上的虚拟目录并配置文件夹作为共享文件夹。  
  
## <a name="bkmk_template"></a>计划 CA 上的服务器证书模板配置  
若要部署自动注册服务器的证书，必须将复制证书模板名为**RAS 和 IAS 服务器**。 默认情况下，此副本名为**副本的 RAS 和 IAS 服务器**。 如果你想要重命名该模板副本，则计划你想要在这一步中部署过程中使用的名称。  
  
> [!NOTE]  
> 本指南-这允许你配置自动服务器证书的注册，刷新组策略，在服务器上，并验证的服务器，ca 收到服务器有效证书的最后三个部署部分不需要规划的额外步骤。  
  


