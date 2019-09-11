---
title: 准备 CAPolicy.inf 文件
description: Capolicy.inf 包含在安装 Active Directory 认证服务（AD CS）或续订 CA 证书时使用的各种设置。
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fb2e25dcd27ed3046eeeb444a9f167ccff6e1dd3
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868957"
---
# <a name="capolicyinf-syntax"></a>Capolicy.inf 语法
>   适用于：Windows Server（半年频道）、Windows Server 2016

Capolicy.inf 是一个配置文件，用于定义应用于根 CA 证书和根 CA 颁发的所有证书的扩展、约束和其他配置设置。 在根 CA 的安装例程开始之前，必须在主机服务器上安装 Capolicy.inf 文件。 当修改根 CA 上的安全限制时，必须续订根证书，并在续订过程开始之前在服务器上安装更新的 Capolicy.inf 文件。

Capolicy.inf 是：

-   由管理员手动创建和定义

-   在创建根 CA 和从属 CA 证书期间使用

-   在签名和颁发证书的签名 CA 上定义（不是向其授予请求的 CA）

创建 Capolicy.inf 文件后，必须将其复制到服务器的 **% systemroot%** 文件夹中，然后再安装 ADCS 或续订 CA 证书。

使用 Capolicy.inf 可以指定和配置各种 CA 属性和选项。 以下部分介绍了用于创建为特定需求定制的 .inf 文件的所有选项。

## <a name="capolicyinf-file-structure"></a>Capolicy.inf 文件结构

以下术语用于描述 .inf 文件结构：

-   _部分_–是涵盖逻辑键组的文件区域。 .Inf 文件中的部分名称通过出现在括号中来标识。 许多（但不是全部）部分用于配置证书扩展。

-   _Key_ –是一项的名称，并显示在等号的左侧。

-   _值_–参数，并显示在等号的右侧。

在下面的示例中， **[版本]** 为部分，**签名**是密钥，而 **"\$Windows NT\$"** 是值。

例如：

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

将文件标识为 .inf 文件。 Version 是唯一必需的部分，必须位于 Capolicy.inf 文件的开头。

###  <a name="policystatementextension"></a>PolicyStatementExtension

列出组织定义的策略，以及这些策略是可选的还是必需的。 多个策略用逗号分隔。 名称在特定部署的上下文中有意义，或与检查是否存在这些策略的自定义应用程序相关。

对于每个已定义的策略，必须有一个定义该特定策略设置的部分。 对于每个策略，需要提供用户定义的对象标识符（OID）以及要作为策略声明显示的文本，或者提供指向 policy 语句的 URL 指针。 URL 可以是 HTTP、FTP 或 LDAP URL 的形式。

如果你将在 policy 语句中使用说明性文本，则 Capolicy.inf 的后三行如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

如果要使用 URL 来托管 CA 策略声明，接下来的三行应如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

此外：

-   支持多个 URL 和通知密钥。

-   支持在同一策略部分中声明和 URL 密钥。

-   带有空格或带空格的文本的 Url 必须用引号括起来。 无论**URL**密钥出现在哪一节，都是如此。

策略部分中的多个通知和 Url 的示例如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

可以在 Capolicy.inf 中为根 CA 证书指定 CRL 分发点（Cdp）。  安装 CA 后，可以配置 CA 在每个颁发的证书中包含的 CDP Url。 根 CA 证书显示在 Capolicy.inf 文件的此部分中指定的 Url。 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

本节中的一些其他信息：
-   支持
    - HTTP 
    - 文件 Url
    - LDAP Url 
    - 多个 Url
   
    >[!IMPORTANT]
    >不支持 HTTPS Url。

-   引号必须用空格将 Url 引起来。

-   如果未指定 Url （即，如果文件中存在 **[CRLDistributionPoint]** 部分但为空），则会从根 CA 证书中省略颁发机构信息访问扩展。 在设置根 CA 时，这通常更可取。 Windows 不对根 CA 证书执行吊销检查，因此 CDP 扩展在根 CA 证书中是多余的。

-    CA 可以发布到文件 UNC，例如，表示客户端通过 HTTP 检索到的网站文件夹的共享。

-   仅在设置根 CA 或续订根 CA 证书时才使用此部分。 CA 确定从属 CA CDP 扩展。
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

你可以为根 CA 证书指定 Capolicy.inf 中的颁发机构信息访问点。

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

授权信息访问部分中的一些其他说明：

-   支持多个 Url。

-   支持 HTTP、FTP、LDAP 和文件 Url。 不支持 HTTPS Url。

-   仅在设置根 CA 或续订根 CA 证书时才使用此部分。 从属 CA AIA 扩展由颁发了从属 CA 证书的 CA 决定。

-   带有空格的 Url 必须用引号括起来。

-   如果未指定 Url （即，如果文件中存在 **[AuthorityInformationAccess]** 部分但为空），则会从根 CA 证书中省略 CRL 分发点扩展。 同样，在根 CA 证书的情况下，这是首选的设置，因为没有颁发机构比需要通过指向其证书的链接引用的根 CA 更高。

### <a name="certsrv_server"></a>certsrv_Server

Capolicy.inf 的另一个可选部分为 [certsrv_server]，用于指定续订或安装的 CA 的续订密钥长度、续订有效期和证书吊销列表（CRL）有效期。 此部分中没有任何项是必需的。 其中许多设置的默认值都足以满足大多数需求，只需从 Capolicy.inf 文件中省略即可。 或者，在安装 CA 后，其中的许多设置都可以更改。

示例如下所示：

```
[certsrv_server]
RenewalKeyLength=2048
RenewalValidityPeriod=Years
RenewalValidityPeriodUnits=5
CRLPeriod=Days
CRLPeriodUnits=2
CRLDeltaPeriod=Hours
CRLDeltaPeriodUnits=4
ClockSkewMinutes=20
LoadDefaultTemplates=True
AlternateSignatureAlgorithm=0
ForceUTF8=0
EnableKeyCounting=0
```

**RenewalKeyLength**仅为续订设置密钥大小。 这仅在 CA 证书续订期间生成新的密钥对时使用。 安装 CA 时，将设置初始 CA 证书的密钥大小。

使用新密钥对续订 CA 证书时，密钥长度可以增加或减少。 例如，如果将根 CA 密钥大小设置为4096字节或更高，然后发现你的 Java 应用或网络设备只能支持2048字节的密钥大小。 无论是增加还是减少大小，都必须重新颁发该 CA 颁发的所有证书。

续订旧的根 CA 证书时， **RenewalValidityPeriod**和**RenewalValidityPeriodUnits**会建立新的根 ca 证书的生存期。 它仅适用于根 CA。 从属 CA 的证书生存期由其上级决定。 RenewalValidityPeriod 可具有以下值：小时、日、周、月和年。

**CRLPeriod**和**CRLPERIODUNITS**为基本 CRL 建立有效期。 **CRLPeriod**可具有以下值：小时、日、周、月和年。

**CRLDeltaPeriod**和**CRLDELTAPERIODUNITS**建立增量 CRL 的有效期。 **CRLDeltaPeriod**可具有以下值：小时、日、周、月和年。

每个设置都可以在安装 CA 后进行配置：

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

请记得重新启动 Active Directory 证书服务，以使更改生效。

**LoadDefaultTemplates**仅适用于企业 CA 的安装过程。 此设置为 "True" 或 "False" （或 "1" 或 "0"），指示是否将 CA 配置为具有任何默认模板。

在 CA 的默认安装中，默认证书模板的子集将添加到 "证书颁发机构" 管理单元的 "证书模板" 文件夹中。 这意味着，一旦 AD CS 服务在安装角色后开始，具有足够权限的用户或计算机即可立即注册证书。

在安装 CA 后，你可能不希望立即颁发任何证书，因此你可以使用 LoadDefaultTemplates 设置来阻止将默认模板添加到企业 CA。 如果未在 CA 上配置模板，则它不会颁发证书。

**内容: alternatesignaturealgorithm**将 ca 配置为支持 ca 证书\#和证书请求的 PKCS 1 v 2.1 签名格式。 当在根 CA 上设置为1时，CA 证书将包含 PKCS\#1 2.1 版签名格式。 如果在从属 CA 上设置，从属 CA 将创建包含 PKCS\#1 v 2.1 签名格式的证书请求。

**ForceUTF8**将 Subject 和颁发者可分辨名称中的相对可分辨名称（RDNs）的默认编码更改为 utf-8。 只有那些支持 UTF-8 的 RDNs （例如，由 RFC 定义为目录字符串类型的那些）受影响。 例如，域组件（DC）的 RDN 支持编码为 IA5 或 UTF-8，而国家 RDN （C）仅支持编码为可打印字符串。 因此，ForceUTF8 指令会影响 DC RDN，但不会影响 C RDN。

**EnableKeyCounting**将 ca 配置为每次使用 ca 的签名密钥时递增计数器。 请勿启用此设置，除非你具有支持密钥计数的硬件安全模块（HSM）和关联的加密服务提供程序（CSP）。 Microsoft 强 CSP 和 Microsoft 软件密钥存储提供程序（KSP）都不支持密钥计数。


## <a name="create-the-capolicyinf-file"></a>创建 Capolicy.inf 文件

在安装 AD CS 之前，请为你的部署配置具有特定设置的 Capolicy.inf 文件。

**先决条件：** 您必须是 Administrators 组的成员。

1. 在计划安装 AD CS 的计算机上，打开 Windows PowerShell，键入**notepad c:\CAPolicy.inf** ，然后按 enter。

2. 系统提示创建新文件时，单击 **“是”** 。

3. 输入以下内容作为文件的内容：
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
   [CRLDistributionPoint]  
   [AuthorityInformationAccess]
   ```
4. 单击 "**文件**"，然后单击 "**另存为**"。

5. 导航到% systemroot% 文件夹。

6. 确保以下信息：

   -   将“文件名”设置为“CAPolicy.inf”

   -   **“保存类型”** 设置为“CAPolicy.inf” **“所有文件”**

   -   **编码**是 **ANSI**

7. 单击“保存”。

8. 提示你覆盖文件时，单击 **“是”** 。

   ![保存为 Capolicy.inf 文件的位置](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

   > [!CAUTION]
   >   确保以 inf 扩展名保存 CAPolicy.inf。 如果不在文件名末尾特意键入“.inf”并按描述选择各个选项，文件将另存为文本文件，并在 CA 安装期间不得使用。

9. 关闭记事本。

> [!IMPORTANT]
>   在 Capolicy.inf 中，可以看到有一行指定 URL https://pki.corp.contoso.com/pki/cps.txt 。 CAPolicy.inf 的“内部策略”部分只会作为你会如何指定证书实行声明 (CPS) 的位置的示例出现。 在本指南中，不会指示您创建证书实践语句（CPS）。
