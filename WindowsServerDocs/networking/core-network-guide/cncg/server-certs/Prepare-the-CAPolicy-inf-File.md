---
title: 准备 CAPolicy.inf 文件
description: CAPolicy.inf 包含安装 Active Directory 证书服务 (AD CS) 时或续订 CA 证书时使用的各种设置。
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 80c7155224502379e2e9618ceb38709c5051a6b7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857838"
---
# <a name="capolicyinf-syntax"></a>CAPolicy.inf 语法
>   适用于：Windows 服务器 （半年频道），Windows Server 2016

CAPolicy.inf 是定义扩展插件、 约束和其他配置设置应用于根 CA 证书和根 CA 颁发的所有证书配置文件。 必须安装例程的根 CA 开始进行之前主机服务器上安装的 CAPolicy.inf 文件。 当要修改的根 CA 的安全限制时，必须续订根证书和续订过程开始之前必须在服务器上安装更新的 CAPolicy.inf 文件。

CAPolicy.inf 是：

-   创建并由管理员手动定义

-   根和从属 CA 证书的创建过程中使用

-   在其中对签名并颁发的证书 (不允许请求时的 CA) 签名的 CA 上定义

创建 CAPolicy.inf 文件后，你必须将它复制到 **%systemroot%** 服务器之前安装 ADCS 或续订 CA 证书的文件夹。

CAPolicy.inf 使可能指定并配置各种 CA 属性和选项。 以下部分介绍为您创建特定需求来定制的.inf 文件的所有选项。

## <a name="capolicyinf-file-structure"></a>CAPolicy.inf 文件结构

使用以下术语来描述的.inf 文件结构：

-   _部分_– 是包含密钥的逻辑组的文件的区域。 .Inf 文件中的部分名称进行标识出现在括号中。 很多，但并非所有的部分用于配置证书扩展。

-   _密钥_– 是一个条目的名称，将显示在等号左侧。

-   _值_– 是参数，将出现在等号右侧。

在以下示例中， **[Version]** 是部分中，**签名**键，并 **"\$Windows NT\$"** 的值。

例如：

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>Version

标识为.inf 文件的文件。 版本是唯一所需部分并且必须在 CAPolicy.inf 文件开头。

###  <a name="policystatementextension"></a>PolicyStatementExtension

列出已由组织定义的策略以及它们是否可选或必需。 由逗号分隔多个策略。 名称的特定部署，或与检查存在这些策略的自定义应用程序相关的上下文中具有的含义。

对于每个定义的策略，必须定义为该特定策略设置的部分。 对于每个策略，你需要提供用户定义的对象标识符 (OID) 和文本想要显示为策略语句或策略语句的 URL 链接。 URL 可以是 HTTP、 FTP 或 LDAP URL 的形式。

如果要在策略语句中有描述性文本，然后 CAPolicy.inf 的接下来的三行如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

如果要使用的 URL 来承载 CA 策略语句，然后接下来的三行改为如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
```

此外：

-   支持多个 URL，请注意，密钥。

-   支持通知和 URL 相同的策略部分中的密钥。

-   带空格的 Url 或带有空格的文本必须用引号引起来。 这适用于**URL**键，而不考虑它在其中出现的部分。

多个通知和策略部分中的 Url 的示例所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=https://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

在 CAPolicy.inf 中的根 CA 证书，可以指定 CRL 分发点 (Cdp)。  安装 CA 之后, 您可以配置 CA 颁发的每个证书中包括 CDP Url。 根 CA 证书显示在 CAPolicy.inf 文件的此部分中指定的 Url。 

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

一些有关本部分中的其他信息：
-   支持：
    - HTTP 
    - 文件 Url
    - LDAP Url 
    - 多个 Url
   
    >[!IMPORTANT]
    >不支持 HTTPS Url。

-   引号必须用空格括起来的 Url。

-   如果没有指定了 Url – 也就是说，如果 **[CRLDistributionPoint]** 部分文件中存在但为空-颁发机构信息访问扩展省略的根 CA 证书。 设置根 CA 时，这是通常更可取。 Windows 不会执行吊销检查根 CA 证书，因此 CDP 扩展是多余的根 CA 证书中。

-    CA 可以将发布到文件的 UNC，例如，表示客户端通过 HTTP 将检索其中的网站文件夹的共享。

-   仅使用本部分中，如果您要设置的根 CA 或续订根 CA 证书。 CA 确定从属 CA CDP 扩展。
   

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

在 CAPolicy.inf 中的根 CA 证书，可以指定的颁发机构信息访问点。

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

颁发机构信息访问部分的一些其他注意事项：

-   支持多个 Url。

-   支持 HTTP、 FTP、 LDAP 和文件的 Url。 不支持 HTTPS Url。

-   如果您要设置根 CA，或续订根 CA 证书，仅使用此部分。 从属 CA AIA 扩展取决于 CA 颁发从属 CA 的证书。

-   带空格的 Url 必须用引号引起来。

-   如果没有指定了 Url – 也就是说，如果 **[AuthorityInformationAccess]** 部分文件中存在但为空-CRL 分发点扩展省略的根 CA 证书。 同样，这将是在根 CA 证书的情况下的首选的设置，因为高于根 CA，需要引用的链接到其证书没有颁发机构。

### <a name="certsrvserver"></a>certsrv_Server

CAPolicy.inf 的另一个可选部分是 [certsrv_server] 用于指定续订密钥长度、 续订有效期内和证书吊销列表 (CRL) 有效期的 ca，正在安装或续订。 在本部分中密钥都是必需的。 有许多这些设置的默认值足以满足大多数需求，只是可以省略了从 CAPolicy.inf 文件。 此外，许多这些设置可以更改安装 CA 之后。

一个示例所示：

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

**RenewalKeyLength**设置仅续订的密钥大小。 CA 证书续订期间生成新的密钥对时才使用此选项。 安装 CA 时设置初始的 CA 证书的密钥大小。

当续订 CA 证书与新的密钥对，密钥长度可增加或减少。 例如，如果已设置的根 CA 密钥大小为 4096 个字节或更高版本，并发现，则必须 Java 应用程序或仅支持 2048 个字节的密钥大小的网络设备。 是否增加或减小大小，必须重新颁发由该 CA 颁发的所有证书。

**RenewalValidityPeriod**并**RenewalValidityPeriodUnits**续订旧的根 CA 证书时建立新的根 CA 证书的生存期。 它仅适用于根 CA。 从属 CA 证书生存期由其上级确定。 RenewalValidityPeriod 可以具有以下值：小时、 天、 周、 月和年。

**CRLPeriod**并**CRLPeriodUnits**建立基本 crl 的有效期。 **CRLPeriod**可以具有以下值：小时、 天、 周、 月和年。

**CRLDeltaPeriod**并**CRLDeltaPeriodUnits**建立增量 CRL 的有效期。 **CRLDeltaPeriod**可以具有以下值：小时、 天、 周、 月和年。

安装 CA 后，可以配置每个设置：

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

请记得重新启动 Active Directory 证书服务的任何更改才会生效。

**LoadDefaultTemplates**仅企业 CA 安装的过程适用。 此设置是 True 或 False （或 1 或 0），指示是否 CA 配置有任何默认模板。

在默认安装中的 ca，默认证书模板的子集添加到证书颁发机构管理单元中的证书模板文件夹中。 这意味着，只要 AD CS 服务启动后安装了角色的用户或计算机具有足够的权限可以立即注册证书。

您可能不希望因此可以使用 LoadDefaultTemplates 设置来防止默认模板添加到企业 CA 颁发任何证书已安装 CA 之后立即。 如果没有在 CA 上配置模板，它可以颁发任何证书。

**内容： AlternateSignatureAlgorithm**配置 CA 以支持 PKCS\#1 V2.1 签名格式的 CA 证书和证书请求。 如果设置为 1 上的根 CA 的 CA 证书将包含 PKCS\#1 V2.1 签名格式。 当在从属 CA 上设置时，从属 CA 将创建证书申请包括 PKCS\#1 V2.1 签名格式。

**ForceUTF8**更改默认值编码使用者和颁发者中的相对可分辨名称 (Rdn) 的可分辨名称为 utf-8。 仅这些 Rdn 支持 utf-8，例如那些被定义为目录字符串类型由 rfc 验证，会受到影响。 例如，RDN 的域组件 (DC) 支持编码为 IA5 或 UTF-8，而 Country RDN (C) 只支持与可打印字符串的编码。 ForceUTF8 指令将因此会影响 DC RDN，但不是会影响 C RDN。

**EnableKeyCounting**配置 CA 以递增计数器，每次使用 CA 的签名密钥。 如果您没有硬件安全模块 (HSM) 和关联的加密服务提供程序 (CSP) 的支持密钥计数，则不要启用此设置。 不使用 Microsoft 强 CSP 或 Microsoft 软件密钥存储提供程序 (KSP) 的支持密钥计数。


## <a name="create-the-capolicyinf-file"></a>创建 CAPolicy.inf 文件

安装 AD CS 之前，您配置 CAPolicy.inf 文件使用特定的设置为你的部署。

**先决条件：** 必须是 Administrators 组的成员。

1.  在您打算安装 AD CS，打开 Windows PowerShell，在计算机上键入**记事本 c:\CAPolicy.inf**然后按 ENTER。

2.  系统提示创建新文件时，单击 **“是”**。

3.  输入以下内容作为文件的内容：
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
1.  单击**文件**，然后单击**另存为**。

2.  导航到 %systemroot%文件夹。

3.  确保以下信息：

    -   将“文件名”设置为“CAPolicy.inf”

    -   **“保存类型”** 设置为“CAPolicy.inf” **“所有文件”**

    -   **编码**是 **ANSI**

4.  单击“保存” 。

5.  提示你覆盖文件时，单击 **“是”**。

    ![CAPolicy.inf 文件另存为位置](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   确保以 inf 扩展名保存 CAPolicy.inf。 如果不在文件名末尾特意键入“.inf”并按描述选择各个选项，文件将另存为文本文件，并在 CA 安装期间不得使用。

6.  关闭记事本。

>   [!IMPORTANT]  
>   在 CAPolicy.inf 中，您可以看到有一行指定 URL https://pki.corp.contoso.com/pki/cps.txt。 CAPolicy.inf 的“内部策略”部分只会作为你会如何指定证书实行声明 (CPS) 的位置的示例出现。 在本指南中，您不被指示创建证书实行声明 (CPS)。
