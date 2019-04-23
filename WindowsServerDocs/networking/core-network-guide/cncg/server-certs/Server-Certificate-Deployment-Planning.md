---
title: 服务器证书部署规划
description: 本主题是指南为 802.1x 有线和无线部署部署服务器证书的一部分
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0d14ed33c9bf389433f59774c04ff15e37256cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839528"
---
# <a name="server-certificate-deployment-planning"></a>服务器证书部署规划

>适用于：Windows 服务器 （半年频道），Windows Server 2016

在部署服务器证书之前，必须规划以下各项：  
  
-   [规划基本服务器配置](#bkmk_basic)  
  
-   [规划域访问](#bkmk_domain)  
  
-   [在 Web 服务器上计划的位置和虚拟目录的名称](#bkmk_virtual)  
  
-   [计划你的 Web 服务器的 DNS 别名 (CNAME) 记录](#bkmk_cname)  
  
-   [规划配置 CAPolicy.inf](#bkmk_capolicy)  
  
-   [计划 CA1 CDP 和 AIA 扩展的配置](#bkmk_cdp)  
  
-   [规划 CA 和 Web 服务器之间复制操作](#bkmk_copy)  
  
-   [在 CA 上计划服务器证书模板的配置](#bkmk_template)  
  
## <a name="bkmk_basic"></a>规划基本服务器配置  
想要用作证书颁发机构和 Web 服务器的计算机上安装 Windows Server 2016 后，必须重命名计算机和分配并配置本地计算机的静态 IP 地址。  
  
有关详细信息，请参阅 Windows Server 2016[核心网络指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_domain"></a>规划域访问  
若要登录到域，计算机必须是域成员计算机，必须在之前尝试登录 AD DS 中创建的用户帐户。 此外，本指南中的大多数过程需要的用户帐户是 Active Directory 用户和计算机中的 Enterprise Admins 或 Domain Admins 组的成员，因此您必须登录到 CA 与具有适当的组成员身份的帐户。  
  
有关详细信息，请参阅 Windows Server 2016[核心网络指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_virtual"></a>在 Web 服务器上计划的位置和虚拟目录的名称  
若要提供对 CRL 和 CA 证书到其他计算机访问，您必须在 Web 服务器上虚拟目录中存储这些项。 在本指南中，虚拟目录位于 WEB1 在 Web 服务器上。 此文件夹位于"c:"驱动器上和名为"pki"。 在 Web 服务器上适用于你的部署任意文件夹位置，可以找到您的虚拟目录。  
  
## <a name="bkmk_cname"></a>计划你的 Web 服务器的 DNS 别名 (CNAME) 记录  
别名 (CNAME) 资源记录有时也称为规范名称资源记录。 借助这些记录，可以使用多个名称指向一台主机，从而轻松实现以下功能： 为主机文件传输协议 (FTP) 服务器和在同一台计算机上的 Web 服务器。 例如，众所周知的服务器名称 （ftp、 www） 进行注册这些服务使用映射到域名系统 (DNS) 主机名，例如 WEB1，为服务器计算机承载的别名 (CNAME) 资源记录。  
  
本指南提供了说明如何配置你的 Web 服务器以承载证书颁发机构 (CA) 证书吊销列表 (CRL)。 您可能还想要使用你的 Web 服务器出于其他目的，如托管 FTP 或 Web 站点，因为它是别名资源记录在 DNS 中创建你的 Web 服务器的一个好办法。 本指南中的 CNAME 记录名为"pki，"但您可以选择适合你的部署的名称。  
  
## <a name="bkmk_capolicy"></a>规划配置 CAPolicy.inf  
在安装 AD CS 之前，必须使用适合于你的部署的信息在 CA 上配置 CAPolicy.inf。 CAPolicy.inf 文件包含以下信息：  
  
```  
[Version]  
Signature="$Windows NT$"  
[PolicyStatementExtension]  
Policies=InternalPolicy  
[InternalPolicy]  
OID=1.2.3.4.1455.67.89.5  
Notice="Legal Policy Statement"  
URL=https://pki.corp.contoso.com/pki/cps.txt  
[Certsrv_Server]  
RenewalKeyLength=2048  
RenewalValidityPeriod=Years  
RenewalValidityPeriodUnits=5  
CRLPeriod=weeks  
CRLPeriodUnits=1  
LoadDefaultTemplates=0  
AlternateSignatureAlgorithm=1  
```  
必须计划此文件的以下各项：  
  
-   **URL**。 示例 CAPolicy.inf 文件的 URL 值为**https://pki.corp.contoso.com/pki/cps.txt**。 这是因为本指南中的 Web 服务器名为 WEB1，并且 pki 的 DNS CNAME 资源记录。 Web 服务器也加入到 corp.contoso.com 域中。 此外，没有名为"pki"证书吊销列表的存储位置的 Web 服务器上的虚拟目录。 请确保 url 中提供虚拟目录在 CAPolicy.inf 文件指向你的域中的 Web 服务器的值。  
  
-   **RenewalKeyLength**。 为 Windows Server 2012 中的 AD CS 的默认续订密钥长度为 2048年。 同时仍提供与想要使用的应用程序兼容性，您选择的密钥长度应尽可能长时间。  
  
-   **RenewalValidityPeriodUnits**。 示例 CAPolicy.inf 文件拥有 RenewalValidityPeriodUnits 值为 5 年。 这是因为 CA 的预期生命周期是大约十年。 RenewalValidityPeriodUnits 的值应反映 CA 或年你想要提供注册的最大数目的整体的有效期。  
  
-   **CRLPeriodUnits**。 示例 CAPolicy.inf 文件拥有 CRLPeriodUnits 值为 1。 这是因为本指南中的证书吊销列表的示例中刷新间隔是 1 周。 在使用此设置指定的时间间隔值，必须将 CA 上的 CRL 发布到 Web 服务器虚拟目录存储 CRL 的位置和提供的身份验证过程中的计算机访问它。  
  
-   **内容： AlternateSignatureAlgorithm**。 此 CAPolicy.inf 通过实现备用签名格式实现了改进的安全机制。 如果仍有需要来自此 CA 的证书的 Windows XP 客户端，不应实现此设置。  
  
如果您不计划在更高版本时，将任何从属 Ca 添加到你的公钥基础结构和你想要阻止添加任何从属 Ca，可以将 PathLength 密钥添加到 CAPolicy.inf 文件，值为 0。 若要添加此项，复制并粘贴到文件的以下代码：  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> 不建议您更改 CAPolicy.inf 文件中的任何其他设置，除非有特定原因需要执行此操作。  
  
## <a name="bkmk_cdp"></a>计划 CA1 CDP 和 AIA 扩展的配置  
CA1 上配置证书吊销列表 (CRL) 分发点 (CDP) 和颁发机构信息访问 (AIA) 设置时，需要你的 Web 服务器和你的域名的名称。 您还需要 Web 服务器证书吊销列表 (CRL) 和证书颁发机构证书的存储位置创建的虚拟目录的名称。  
  
必须在此部署步骤中输入的 CDP 位置采用格式：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
例如，如果你的 Web 服务器为 WEB1，且你的 DNS 别名的 CNAME 记录的 Web 服务器的"pki"，你的域为 corp.contoso.com，和虚拟目录名为 pki，CDP 位置是：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
必须输入的 AIA 位置采用格式：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
例如，如果你的 Web 服务器为 WEB1，且你的 DNS 别名的 CNAME 记录的 Web 服务器的"pki"，你的域为 corp.contoso.com，和虚拟目录名为 pki、 AIA 位置是：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>规划 CA 和 Web 服务器之间复制操作  
若要发布的 CRL 和 CA 证书从 CA 访问 Web 服务器虚拟目录，可以在 CA 上配置的 CDP 和 AIA 位置后运行 certutil-crl 命令。 请确保您 CA 属性上配置正确的路径**扩展**选项卡，然后再运行此命令使用本指南中的说明。 此外，企业 CA 证书复制到 Web 服务器，您必须已创建的虚拟目录的 Web 服务器上并配置为共享文件夹的文件夹。  
  
## <a name="bkmk_template"></a>在 CA 上计划服务器证书模板的配置  
若要部署自动注册的服务器证书，必须将复制证书模板名为**RAS 和 IAS 服务器**。 默认情况下，名为此副本**复制的 RAS 和 IAS 服务器**。 如果你想要重命名此模板副本，规划你想要在此步骤中部署过程中使用的名称。  
  
> [!NOTE]  
> 本指南-它允许您配置服务器证书自动注册、 组策略刷新在服务器上，并验证服务器已从 CA 接收有效的服务器证书的最后三个部署部分不需要额外的规划步骤。  
  


