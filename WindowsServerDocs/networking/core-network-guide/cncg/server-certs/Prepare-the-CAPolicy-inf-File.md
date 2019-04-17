---
title: 准备 CAPolicy.inf 文件
description: CAPolicy.inf 包含用于安装 Active Directory 认证服务（广告客户服务）时，或时续订 CA 的各种设置证书。
manager: alanth
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9618d4abe512b487f4f22ffde85a052c1c52ef22
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/05/2018
---
# <a name="capolicyinf-syntax"></a>CAPolicy.inf 语法
>   适用于：Windows Server（半年通道），Windows Server 2016

CAPolicy.inf 是定义扩展、约束和应用与根 CA 证书颁发的根所有证书的其他配置设置配置文件。 在安装例程根 CA 开始之前主服务器上必须安装 CAPolicy.inf 文件。 当进行修改根 CA 安全限制，请必须续订根证书，并续订进程开始之前必须服务器上安装更新后的 CAPolicy.inf 文件。

CAPolicy.inf 是：

-   创建和手动由管理员

-   根和附属 CA 证书的创建过程中使用

-   定义上签名 CA 您登录并发出证书 (不 CA 允许请求时)

创建 CAPolicy.inf 文件后，你必须将其复制到**%系统根 %**服务器安装 ADCS 或续订 CA 证书之前的文件夹。

CAPolicy.inf 使得可能指定和配置各种 CA 特性和选项。 以下部分介绍所有选项可创建符合你的特定需求的.inf 文件。

## <a name="capolicyinf-file-structure"></a>CAPolicy.inf 文件结构

使用以下术语描述.inf 文件结构：

-   _部分_–是涵盖键的逻辑组中的文件的区域。 部分中.inf 文件的名称显示在支架中进行标识。 许多，而不是所有部分用于配置证书扩展。

-   _键_-是一项的名称和出现等于号左侧。

-   _值_-是的参数和等于号右侧显示。

在以下示例中，**版本**是部分中，**签名**键，并**"\ $ Windows NT \ $"**的值。

示例：

```PowerShell
[Version]                     #section
Signature="$Windows NT$"      #key=value
```

###  <a name="version"></a>版本

将该文件识别为.inf 文件。 版本是唯一要求部分和必须 CAPolicy.inf 文件的开头。

###  <a name="policystatementextension"></a>PolicyStatementExtension

列出已由组织机构的策略和是可选的还是强制它们。 多个策略用逗号分隔。 名称具有特定的部署，或检查存在这些策略的自定义应用程序针对上下文中的含义。

对于每个定义的策略，必须定义特定策略设置的部分。 对于每个策略中，您需要提供用户定义的对象标识符 (OID)，并任一你希望文本显示为策略声明或策略声明的 URL 链接。 以下形式出现：HTTP、FTP 或 LDAP URL 可以是 URL。

如果你打算策略声明中有描述性短信，然后 CAPolicy.inf 接下来的三行如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
Notice=”Legal policy statement text”
```

如果你要使用 URL 承载 CA 策略隐私声明，然后下一步三行改用如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.2
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
```

另外：

-   受支持多个 URL，请注意的键。

-   受支持的相同的策略部分中的通知和 URL 键。

-   使用空间的 Url 或包含空格短信，必须用名言。 这适用于**URL**键，则无论是否为显示在其中的部分。

多个通知和策略节中的 Url 的示例如下所示：

```
[InternalPolicy]
OID=1.1.1.1.1.1.1
URL=http://pki.wingtiptoys.com/policies/legalpolicy.asp
URL=ftp://ftp.wingtiptoys.com/pki/policies/legalpolicy.asp
Notice=”Legal policy statement text”
```

### <a name="crldistributionpoint"></a>CRLDistributionPoint

你可以在 CAPolicy.inf CA 根证书指定 CRL Distribution 点 (Cdp)。 在安装 CA 之后可以配置 CA 包含在每个证书颁发 CDP Url。 在此部分中的 CAPolicy.inf 文件指定的 Url 包含在根证书本身。

```
[CRLDistributionPoint]
URL=http://pki.wingtiptoys.com/cdp/WingtipToysRootCA.crl
```

有关此部分中的一些其他信息：

-   多个 Url 受支持。

-   HTTP、FTP 和 LDAP Url 受支持。 HTTPS Url 均不支持。

-   如果要设置根 CA 或续订根仅使用本部分。 附属 CA CDP 扩展取决于 CA 它附属 CA 证书的问题。

-   使用空间的 Url 必须包围引号。

-   如果没有 Url 指定–，即如果**[CRLDistributionPoint]**–部分存在于该文件，但是否为空 CRL Distribution 点扩展省略从根证书。 设置根 CA 时，这是通常最好。 Windows 不执行吊销根加拿大证书，以便 CDP 扩展是多余在根加拿大证书检查。

### <a name="authorityinformationaccess"></a>AuthorityInformationAccess

你可以指定 CAPolicy.inf CA 根证书颁发机构信息接入点。

```
[AuthorityInformationAccess]
URL=http://pki.wingtiptoys.com/Public/myCA.crt
```

在颁发机构信息一些其他说明来访问部分：

-   多个 Url 受支持。

-   HTTP、FTP、LDAP 和文件 Url 受支持。 HTTPS Url 均不支持。

-   如果要设置根加拿大或续订根仅使用本部分。 附属 CA AIA 扩展取决于 CA 其发出附属 CA 证书。

-   使用空间的 Url 必须包围引号。

-   如果没有 Url 指定–，即如果**[AuthorityInformationAccess]**–部分存在于该文件，但是否为空 CRL Distribution 点扩展省略从根证书。 同样，会首选就根 CA 证书而言设置没有颁发机构高于根需要通过其证书链接引用的 CA 原样。

### <a name="certsrvserver"></a>certsrv_Server

另一个可选 CAPolicy.inf 部分介绍 [certsrv_server]，用于指定续订密钥长度、续订有效期内，以及证书吊销 (CRL) 列表有效期为是 CA 正在续订或安装。 在此部分中键都需要。 这些设置的许多有足够的大多数需求，并只是可以从 CAPolicy.inf 文件省略的默认值。 或者，这些设置的许多可以更改后在安装 CA。

一个示例如下所示：

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

**RenewalKeyLength**设置仅续订的关键大小。 在加拿大证书续订生成新的关键配对时才使用此选项。 安装后 CA 设置的初始 CA 证书关键大小。

当续订使用新的关键配对 CA 证书，关键长度可以或者增加或减少。 例如，如果你已经设置了根 4096 字节或更高版本，CA 关键大小，然后发现，你有 Java 应用或可能仅支持的 2048 字节密钥大小的网络设备。 无论你增加或减少大小，你必须重新由该 CA 颁发所有证书。

**RenewalValidityPeriod**和**RenewalValidityPeriodUnits**建立时续订旧根证书的整个使用期限内的新根证书。 它仅适用于根 CA。 证书整个使用期限内的附属 CA 由其卓越的决定。 RenewalValidityPeriod 有以下值：小时，日、周、月和年。

**CRLPeriod**和**CRLPeriodUnits**基本 crl 建立有效期。 **CRLPeriod**有以下值：小时，日、周、月和年。

**CRLDeltaPeriod**和**CRLDeltaPeriodUnits**建立增量 CRL 有效期。 **CRLDeltaPeriod**有以下值：小时，日、周、月和年。

CA 安装后，可配置每个设置：

```
Certutil -setreg CACRLPeriod Weeks
Certutil -setreg CACRLPeriodUnits 1
Certutil -setreg CACRLDeltaPeriod Days
Certutil -setreg CACRLDeltaPeriodUnits 1
```

请记住重新启动的 Active Directory 证书服务的任何更改生效。

**LoadDefaultTemplates**仅适用于企业 CA 安装期间。 此设置，或者真或 False（或 1 或 0），规定是否通过任何默认模板配置 CA。

在加拿大的默认安装的默认证书模板子集添加到中证书颁发机构贴靠证书模板文件夹中。 这意味着，一旦广告客户服务服务启动之后，在安装了角色用户或计算机具有足够的权限可以立即注册证书。

你不希望任何证书颁发立即后已安装了 CA，以便你可以使用 LoadDefaultTemplates 设置阻止默认模板添加到企业版 CA。 如果没有模板配置 CA 上，它可以发出没有证书。

**AlternateSignatureAlgorithm**配置 CA 支持 PKCS\ #1 2.1 版签名格式 CA 证书和证书请求。 当设置为在根 1 CA CA 证书包括 PKCS\ #1 2.1 版签名格式。 如果在从上设置 CA、附属 CA 将创建证书请求，包括 PKCS\ #1 2.1 版签名格式。

**ForceUTF8**更改默认值的主题和发行商中的相关辨别名服务器编码分辨接为的名称。 只有这些 RDNs 支持 utf-8，例如被定义为目录字符串类型 RFC，通过会受到影响。 例如，RDN 域组件 (DC) 的支持时 Country RDN (C) 仅支持编码作为可打印字符串 IA5 或 utf-8，编码。 ForceUTF8 指令因此将影响 DC RDN，但不是会影响 C RDN。

**EnableKeyCounting**配置 CA 增加计数器每次使用 CA 签名该键。 未启用此设置，除非你有硬件安全模块 (HSM) 和关联加密支持服务提供商 (CSP) 键计数。 不使用 Microsoft 强 CSP 或 Microsoft 软件键存储提供商 (KSP) 支持关键计数。


## <a name="create-the-capolicyinf-file"></a>创建 CAPolicy.inf 文件

安装广告客户服务之前，你配置 CAPolicy.inf 文件的特定设置为你部署。

**先决条件：**你必须是管理员组中的成员。

1.  打算安装广告的客户服务、打开 Windows PowerShell，计算机上键入**记事本 c:.inf**并按 ENTER。

2.  当提示你创建一个新文件，请单击**是**。

3.  输入以下命令以该文件的内容：
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
    [CRLDistributionPoint]  
    [AuthorityInformationAccess]
   ```
1.  单击**文件**，然后单击**另存为**。

2.  导航到 %系统根 %文件夹。

3.  确保：

    -   **文件名**设置为**CAPolicy.inf**

    -   **将另存为类型**设置为**所有文件**

    -   **编码**是**ANSI**

4.  单击**保存**。

5.  当提示你覆盖文件时，请单击**是**。

    ![将另存为 CAPolicy.inf 文件的位置](../../../media/Prepare-the-CAPolicy-inf-File/001-SaveCAPolicyORCA1.gif)

    >   [!CAUTION]  
    >   请确保保存 CAPolicy.inf 用 inf 扩展。 如果不专门键入**.inf**在的文件的名称和选择的选项，述结束时，文件将另存为文本文件和将不会在加拿大安装期间使用。

6.  关闭记事本。

>   [!IMPORTANT]  
>   在 CAPolicy.inf，您可以看到有是指定的 URL 行http://pki.corp.contoso.com/pki/cps.txt。 只需显示为指定证书练习声明 (CPS) 中的位置的方式的一个示例 CAPolicy.inf 内部策略部分。 本指南中，在你未被要求创建证书练习语句 (CPS)。
