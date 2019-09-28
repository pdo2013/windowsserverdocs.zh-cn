---
title: 服务器证书部署规划
description: 本主题是指导 802.1 X 有线和无线部署的 "部署服务器证书" 的一部分
manager: brianlic
ms.topic: article
ms.assetid: 7eb746e0-1046-4123-b532-77d5683ded44
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ec5bc315381f85434753f9becc94409a74271b7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356104"
---
# <a name="server-certificate-deployment-planning"></a>服务器证书部署规划

>适用于：Windows Server（半年频道）、Windows Server 2016

部署服务器证书之前，必须计划以下各项：  
  
-   [规划基本服务器配置](#bkmk_basic)  
  
-   [规划域访问](#bkmk_domain)  
  
-   [规划 Web 服务器上的虚拟目录的位置和名称](#bkmk_virtual)  
  
-   [规划 Web 服务器的 DNS 别名（CNAME）记录](#bkmk_cname)  
  
-   [规划 Capolicy.inf 的配置](#bkmk_capolicy)  
  
-   [在 CA1 上规划 CDP 和 AIA 扩展的配置](#bkmk_cdp)  
  
-   [规划 CA 和 Web 服务器之间的复制操作](#bkmk_copy)  
  
-   [在 CA 上规划服务器证书模板的配置](#bkmk_template)  
  
## <a name="bkmk_basic"></a>规划基本服务器配置  
在你打算用作证书颁发机构和 Web 服务器的计算机上安装 Windows Server 2016 后，你必须重命名计算机并为本地计算机分配和配置静态 IP 地址。  
  
有关详细信息，请参阅 Windows Server 2016 [Core 网络指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_domain"></a>规划域访问  
若要登录到域，计算机必须是域成员计算机，并且必须在登录尝试之前 AD DS 中创建用户帐户。 此外，本指南中的大多数过程都要求用户帐户是 Active Directory 用户和计算机中的 Enterprise Admins 或 Domain Admins 组的成员，因此你必须使用具有相应组成员身份的帐户登录到 CA。  
  
有关详细信息，请参阅 Windows Server 2016 [Core 网络指南](../../../core-network-guide/Core-Network-Guide.md)。  
  
## <a name="bkmk_virtual"></a>规划 Web 服务器上的虚拟目录的位置和名称  
若要向其他计算机提供对 CRL 和 CA 证书的访问权限，必须将这些项存储在 Web 服务器上的虚拟目录中。 本指南中的虚拟目录位于 Web 服务器 WEB1 上。 此文件夹位于 "C：" 驱动器上，名为 "pki"。 你可以在 Web 服务器上找到适合你的部署的任何文件夹位置上的虚拟目录。  
  
## <a name="bkmk_cname"></a>规划 Web 服务器的 DNS 别名（CNAME）记录  
别名（CNAME）资源记录有时也称为规范名称资源记录。 利用这些记录，你可以使用多个名称来指向单个主机，这样就可以轻松地在同一台计算机上同时承载文件传输协议（FTP）服务器和 Web 服务器等操作。 例如，已知服务器名称（ftp、www）使用别名（CNAME）资源记录进行注册，这些记录映射到承载这些服务的服务器计算机的域名系统（DNS）主机名（例如 WEB1）。  
  
本指南提供有关配置 Web 服务器以承载证书颁发机构（CA）的证书吊销列表（CRL）的说明。 由于你可能还希望将 Web 服务器用于其他目的（例如）来托管 FTP 或网站，因此最好在 DNS 中为 Web 服务器创建别名资源记录。 在本指南中，CNAME 记录名为 "pki"，但你可以选择适合你的部署的名称。  
  
## <a name="bkmk_capolicy"></a>规划 Capolicy.inf 的配置  
安装 AD CS 之前，必须在 CA 上配置 Capolicy.inf，并将其信息用于部署。 Capolicy.inf 文件包含以下信息：  
  
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
您必须为此文件规划以下各项：  
  
-   **URL**。 示例 Capolicy.inf 文件的 URL 值为 **https://pki.corp.contoso.com/pki/cps.txt** 。 这是因为本指南中的 Web 服务器名为 WEB1，并且具有 pki 的 DNS CNAME 资源记录。 Web 服务器还联接到 corp.contoso.com 域。 此外，在 Web 服务器上有一个名为 "pki" 的虚拟目录，其中存储了证书吊销列表。 确保你为 Capolicy.inf 文件中的 URL 提供的值指向域中 Web 服务器上的虚拟目录。  
  
-   **RenewalKeyLength**。 Windows Server 2012 中 AD CS 的默认续订密钥长度为2048。 你选择的密钥长度应尽可能长，同时仍提供与你打算使用的应用程序的兼容性。  
  
-   **RenewalValidityPeriodUnits**。 示例 Capolicy.inf 文件的 RenewalValidityPeriodUnits 值为5年。 这是因为 CA 预期的生命周期大约为10年。 RenewalValidityPeriodUnits 的值应反映 CA 的总体有效期或要为其提供注册的最高年数。  
  
-   **CRLPeriodUnits**。 示例 Capolicy.inf 文件的 CRLPeriodUnits 值为1。 这是因为本指南中的证书吊销列表的示例刷新间隔为1周。 在使用此设置指定的时间间隔值上，必须将 CA 上的 CRL 发布到存储 CRL 的 Web 服务器虚拟目录，并为在身份验证过程中的计算机提供对其的访问权限。  
  
-   **内容: alternatesignaturealgorithm**。 此 Capolicy.inf 通过实现备用签名格式实现了改进的安全机制。 如果你仍然具有需要此 CA 颁发的证书的 Windows XP 客户端，则不应实现此设置。  
  
如果你不打算稍后将任何从属 Ca 添加到你的公钥基础结构中，并且想要阻止添加任何从属 Ca，则可以将 PathLength 密钥添加到值为0的 Capolicy.inf 文件中。 若要添加此项，请将以下代码复制并粘贴到文件中：  
  
```  
[BasicConstraintsExtension]  
PathLength=0  
Critical=Yes  
```  
  
> [!IMPORTANT]  
> 建议您不要更改 Capolicy.inf 文件中的任何其他设置，除非您有具体的原因。  
  
## <a name="bkmk_cdp"></a>在 CA1 上规划 CDP 和 AIA 扩展的配置  
当你在 CA1 上配置证书吊销列表（CRL）分发点（CDP）和颁发机构信息访问（AIA）设置时，需要你的 Web 服务器名称和域名。 还需要在存储证书吊销列表（CRL）和证书颁发机构证书的 Web 服务器上创建的虚拟目录的名称。  
  
在此部署步骤中必须输入的 CDP 位置的格式为：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl.`  
      
例如，如果你的 Web 服务器名为 WEB1，并且你的 Web 服务器的 DNS 别名 CNAME 记录为 "pki"，则你的域为 corp.contoso.com，并且虚拟目录命名为 "pki"，CDP 位置为：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`  
      
必须输入的 AIA 位置格式如下：  
      
    `http:\/\/*DNSAlias\(CNAME\)RecordName*.*Domain*.com\/*VirtualDirectoryName*\/<ServerDNSName>\_<CaName><CertificateName>.crt.`  
      
例如，如果你的 Web 服务器名为 WEB1，并且你的 Web 服务器的 DNS 别名 CNAME 记录为 "pki"，则你的域为 corp.contoso.com，并且虚拟目录命名为 "pki"，AIA 位置为：  
      
    `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`  
      
## <a name="bkmk_copy"></a>规划 CA 和 Web 服务器之间的复制操作  
若要将 CRL 和 CA 证书从 CA 发布到 Web 服务器虚拟目录，可以在 CA 上配置 CDP 和 AIA 位置之后运行 certutil-CRL 命令。 在运行此命令之前，请确保在 "CA 属性**扩展**" 选项卡上配置正确路径，然后再按照本指南中的说明操作。 此外，若要将企业 CA 证书复制到 Web 服务器，则必须已在 Web 服务器上创建了虚拟目录，并将该文件夹配置为共享文件夹。  
  
## <a name="bkmk_template"></a>在 CA 上规划服务器证书模板的配置  
若要部署自动注册的服务器证书，你必须复制名为**RAS 和 IAS 服务器**的证书模板。 默认情况下，此副本为**RAS 和 IAS 服务器的命名副本**。 如果要重命名此模板副本，请规划要在此部署步骤中使用的名称。  
  
> [!NOTE]  
> 本指南中的最后三个部署部分-这允许你配置服务器证书自动注册、刷新服务器上的组策略，并验证服务器是否从 CA 接收到有效的服务器证书-无需额外规划逐步.  
  


