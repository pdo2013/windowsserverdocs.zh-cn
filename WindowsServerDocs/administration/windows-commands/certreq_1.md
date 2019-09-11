---
title: certreq
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7a04e51f-f395-4bff-b57a-0e9efcadf973
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 19b4750b627a86a724b2a0f58ed7f9bde5ea1613
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867104"
---
# <a name="certreq"></a>certreq



Certreq 可用于从证书颁发机构（CA）请求证书、从 CA 获取对以前请求的响应、从 .inf 文件创建新请求、接受并安装对请求的响应、构造交叉认证或来自现有 CA 证书或请求的限定部属请求，以及对交叉证书或合格的部属请求进行签名。

> [!WARNING]
> - 早期版本的 certreq 可能不提供本文档中所述的所有选项。 可以通过运行 "语法表示法" 部分中所示的命令，查看特定版本的 certreq 提供的所有选项。
> - Certreq 在 CEP/CES 环境中时不支持基于密钥证明模板创建新的证书请求

## <a name="BKMK_Contents"></a>目录

本文的主要部分如下所示：
1.  [谓词](#BKMK_Verbs)
2.  [语法表示法](#BKMK_notation)
3.  [选项](#BKMK_Options)
4.  [Formats](#BKMK_Formats)
5.  [其他 certreq 示例](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>谓词

下表描述了可与 certreq 命令一起使用的谓词

|开关|描述|
|------|-----------|
|-提交|向 CA 提交请求。 有关详细信息，请参阅[Certreq](#BKMK_Submit)。|
|-检索*RequestID*|检索对来自 CA 的以前请求的响应。 有关详细信息，请参阅[Certreq-检索](#BKMK_Retrieve)。|
|-New|基于 .inf 文件创建新的请求。 有关详细信息，请参阅[Certreq](#BKMK_New)。|
|-Accept|接受并安装对证书请求的响应。 有关详细信息，请参阅[Certreq](#BKMK_accept)。|
|-Policy|为请求设置策略。 有关详细信息，请参阅[Certreq](#BKMK_policy)。|
|-Sign|对交叉证书或合格的部属请求进行签名。 有关详细信息，请参阅[Certreq](#BKMK_sign)。|
|-注册|注册或续订证书。 有关详细信息，请参阅[Certreq](#BKMK_enroll)。|
|-?|显示 certreq 语法、选项和说明的列表。|
|谓词 >-？  *\<*|显示指定谓词的帮助。|
|-v-？|显示 certreq 语法、选项和说明的详细列表。|

返回[内容](#BKMK_Contents)

## <a name="BKMK_notation"></a>语法表示法

-   有关基本的命令行语法, 请运行`certreq -?`
-   有关将 certutil 与特定谓词一起使用的语法，请运行**certreq**  *\<verb >* **-？**
-   若要将所有 certutil 语法发送到文本文件, 请运行以下命令:  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

下表描述了用于指示命令行语法的表示法。

|图解|描述|
|--------|-----------|
|不带方括号或大括号的文本|必须按如下所示键入项|
|\<尖括号内的文本 >|必须为其提供值的占位符|
|[方括号内的文本]|可选项|
|{大括号内的文本}|一组必需的项;选择一个|
|竖线（&#124;）|互斥项的分隔符;选择一个|
|省略号 (...)|可以重复的项|

返回[内容](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq-提交

这是默认的 certreq 参数，如果未在命令行提示符处显式指定选项，certreq 将尝试向 CA 提交证书申请。
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
使用– submit 选项时，必须指定证书请求文件。 如果省略此参数，则会显示一个公共的文件打开窗口，您可以在其中选择适当的证书请求文件。

您可以使用这些示例作为构建证书提交请求的起点：

若要提交简单证书请求，请使用下面的示例：
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
若要通过指定 SAN 属性请求证书，请参阅 Microsoft 知识库文章931351中的详细步骤[如何将使用者可选名称添加到安全 LDAP 证书](https://support.microsoft.com/kb/931351)中的 "如何使用 Certreq 实用程序创建和提交包含 SAN 的证书请求 "部分。

返回[内容](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq-检索

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   如果未指定 CAComputerName 或 CAName in-config CAComputerName\CANamea 对话框，将显示所有可用 Ca 的列表。
-   如果使用-config-而不是-config CAComputerName\CAName，则使用默认 CA 处理操作。
-   在 CA 实际发出证书后，可以使用 certreq 检索*RequestID*来检索证书。 *RequestID*PKC 可以是具有0x 前缀的十进制或十六进制，它可以是不带0x 前缀的证书序列号。 你还可以使用它来检索 CA 颁发的任何证书，包括吊销或过期证书，而不考虑证书的请求是否曾处于挂起状态。
-   如果向 CA 提交请求，则 CA 的策略模块可能会使请求处于挂起状态，并将*RequestID*返回给 Certreq 调用方以进行显示。 最终，CA 管理员将颁发证书或拒绝请求。

下面的命令检索证书 id 20，并创建证书文件（.cer）：
```
certreq -retrieve 20 MyCertificate.cer 
```
返回[内容](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq-新建

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
由于 INF 文件允许指定一组丰富的参数和选项，因此很难定义管理员应为所有目的使用的默认模板。 因此，本部分介绍了所有选项，使您能够创建可满足您特定需求的 INF 文件。 以下关键字用于描述 INF 文件的结构。
1.  *部分*是 INF 文件中的一个区域，其中包含密钥的逻辑组。 部分始终出现在 INF 文件的方括号中。
2.  *键*是位于等号左侧的参数。
3.  *值*是位于等号右侧的参数。

例如，最小 INF 文件类似于以下内容：
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
下面是可能会添加到 INF 文件中的部分可能的部分：

**[NewRequest]**

对于充当新证书请求模板的 INF 文件，此部分是必需的。 本节需要至少一个具有值的键。

|Key|定义|ReplTest1|示例|
|---|----------|-----|-------|
|使用者|几个应用程序依赖证书中的主题信息。 因此，建议指定此密钥的值。 如果未在此处设置主题，则建议将使用者名称作为使用者备用名称证书扩展的一部分包含在内。|相对可分辨名称字符串值|Subject = "CN = computer1" Subject = "CN = John Smith，CN = Users，DC = Contoso，DC = com"|
|Exportable|如果将此属性设置为 "TRUE"，则可以连同证书一起导出私钥。 若要确保安全级别较高，私钥应不可导出;但是，在某些情况下，如果多台计算机或用户必须共享相同的私钥，则可能需要使私钥可导出。|true、false|可导出 = TRUE。 CNG 密钥可以区分此和纯文本导出。 CAPI1 密钥不能。|
|ExportableEncrypted|指定私钥是否应设置为可导出。|true、false|ExportableEncrypted = true</br>提示：并非所有公共密钥大小和算法都适用于所有哈希算法。 Tamehe 指定的 CSP 还必须支持指定的哈希算法。 若要查看受支持的哈希算法的列表，可以运行命令<code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|要用于此请求的哈希算法。|Sha256、sha384、sha512、sha1、md5、md4、md2|HashAlgorithm = sha1。 若要查看支持的哈希算法的列表，请使用： &#124; certutil- &#124; oid 1 Findstr pwszCNGAlgid findstr/v CryptOIDInfo|
|KeyAlgorithm|服务提供程序将用于生成公钥和私钥对的算法。|RSA，DH，DSA，ECDH_P256，ECDH_P521，ECDSA_P256，ECDSA_P384，ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|不建议为生成新密钥材料的新请求设置此参数。 密钥容器由系统自动生成和维护。 对于应使用现有密钥材料的请求，可以将此值设置为现有密钥的密钥容器名称。 使用 certutil – key 命令显示计算机上下文的可用密钥容器的列表。 对当前用户的上下文使用 certutil – key – user 命令。|随机字符串值</br>提示：你应在具有空格或特殊字符的任何 INF 密钥值两侧使用双引号，以避免潜在的 INF 分析问题。|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|定义公钥和私钥的长度。 密钥长度会影响证书的安全级别。 较大的密钥长度通常提供较高的安全级别;但是，某些应用程序可能有关于密钥长度的限制。|加密服务提供程序支持的任何有效密钥长度。|KeyLength = 2048|
|KeySpec|确定密钥是否可用于签名，适用于 Exchange （加密），或同时用于两者。|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|密钥用法|定义证书密钥的用途。|CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 （128）</br>提示：显示的值是每个位定义的十六进制值（十进制）。 还可以使用旧语法：设置了多个位的单个十六进制值，而不是符号表示形式。 例如，密钥用法 = 0xa0。</br>CERT_NON_REPUDIATION_KEY_USAGE--40 （64）</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 （32）</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 （16）</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 （32768）|密钥用法 = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>提示：多个值使用竖线（&#124;）符号分隔符。 请确保在使用多个值时使用双引号，以避免出现 INF 分析问题。|
|KeyUsageProperty|检索一个值，该值标识可以对其使用私钥的特定目的。|NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--ffffff （16777215）|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|当你需要创建由计算机拥有的证书而不是用户时，此密钥非常重要。 生成的密钥材料将保留在已创建请求的安全主体（用户或计算机帐户）的安全上下文中。 当管理员代表计算机创建证书申请时，必须在计算机的安全上下文中创建密钥材料，而不是管理员的安全上下文。 否则，计算机无法访问其私钥，因为它将位于管理员的安全上下文中。|true、false|MachineKeySet = true</br>提示：默认值为 false。|
|NotBefore|指定日期或日期和时间，在该日期和时间之前无法发出请求。 NotBefore 可与 ValidityPeriod 和 ValidityPeriodUnits 一起使用。|日期或日期和时间|NotBefore = "7/24/2012 10:31 AM"</br>提示：NotBefore 和 NotAfter 仅适用于 RequestType = cert。日期分析尝试区分区域设置。使用月份名称将消除歧义，并应在每个区域设置中使用。|
|NotAfter|指定日期或日期和时间，在此时间之后无法发出请求。 NotAfter 不能与 ValidityPeriod 或 ValidityPeriodUnits 一起使用。|日期或日期和时间|NotAfter = "9/23/2014 10:31 AM"</br>提示：NotBefore 和 NotAfter 仅适用于 RequestType = cert。日期分析尝试区分区域设置。使用月份名称将消除歧义，并应在每个区域设置中使用。|
|PrivateKeyArchive|仅当相应的 RequestType 设置为 "CMC" 时，PrivateKeyArchive 设置才起作用，因为只有证书管理消息 over CMS （CMC）请求格式才允许安全地将请求者的私钥传输到 CA 进行密钥存档。|true、false|PrivateKeyArchive = True|
|EncryptionAlgorithm|要使用的加密算法。|可能的选项会有所不同，具体取决于操作系统版本和已安装的加密提供程序集。 若要查看可用算法的列表，请运行命令<code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code> ，使用的指定 CSP 还必须支持指定的对称加密算法和长度。|EncryptionAlgorithm = 3des|
|EncryptionLength|要使用的加密算法的长度。|指定的 EncryptionAlgorithm 允许的任何长度。|EncryptionLength = 128|
|ProviderName|提供程序名称是 CSP 的显示名称。|如果你不知道所使用的 CSP 的提供程序名称，请从命令行运行 certutil – csplist。 命令将显示本地系统上可用的所有 Csp 的名称|ProviderName = "Microsoft RSA SChannel 加密提供程序"|
|ProviderType|提供程序类型用于根据特定算法功能（如 "RSA Full"）选择特定提供程序。|如果你不知道所使用的 CSP 的提供程序类型，请从命令行提示符处运行 certutil – csplist。 此命令会显示本地系统上所有可用 Csp 的提供程序类型。|ProviderType = 1|
|RenewalCert|如果需要续订在生成证书请求的系统上存在的证书，则必须将其证书哈希指定为此密钥的值。|在其中创建证书请求的计算机上可用的任何证书的证书哈希。 如果你不知道证书哈希，请使用 "证书" MMC 管理单元，并查看应续订的证书。 打开证书属性，并查看证书的 "指纹" 属性。 证书续订要求使用 PKCS # 7 或 CMC 请求格式。|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>注意:这样，就可以代表其他用户请求注册请求。该请求还必须使用注册代理证书进行签名，否则 CA 将拒绝该请求。 使用-cert 选项来指定注册代理证书。|如果 RequestType 设置为 PKCS # 7 或 CMC，则可以为证书请求指定请求者名称。 如果 RequestType 设置为 PKCS # 10，此密钥将被忽略。 Requestername 只能设置为请求的一部分。 不能操作挂起请求中的 Requestername。|用户|Requestername = "Contoso\BSmith"|
|RequestType|确定用于生成和发送证书请求的标准。|PKCS10--1</br>PKCS7--2</br>CMC-3</br>Cert--4</br>SCEP--fd00 （64768）</br>提示：此选项表示自签名或自行颁发的证书。 它不会生成请求，而是生成新证书，然后安装证书。自签名为默认值。使用– cert 选项指定一个签名证书，以创建自签名证书（不是自签名证书）。|RequestType = CMC|
|SecurityDescriptor</br>提示：这仅适用于计算机上下文非智能卡密钥。|包含与安全对象关联的安全信息。 对于大多数安全对象，可以在创建对象的函数调用中指定对象的安全描述符。|基于[安全描述符定义语言](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx)的字符串。|SecurityDescriptor = "D:P （A;;GA;;;SY）（A;;GA;;;BA） "|
|内容： alternatesignaturealgorithm|指定并检索一个布尔值，该值指示 PKCS # 10 请求或证书签名的签名算法对象标识符（OID）是离散的还是组合的。|true、false|内容: alternatesignaturealgorithm = false</br>提示：对于 RSA 签名，false 表示 Pkcs1 1.5。 True 表示2.1 签名。|
|静默|默认情况下，此选项允许 CSP 访问交互式用户桌面并向用户请求诸如智能卡 PIN 之类的信息。 如果将此项设置为 TRUE，则 CSP 不得与桌面交互，将阻止向用户显示任何用户界面。|true、false|缄默 = true|
|SMIME|如果将此参数设置为 TRUE，则会将具有对象标识符值 "1.2.840.113549.1.9.15" 的扩展添加到请求中。 对象标识符的数量取决于安装的操作系统版本和 CSP 功能，这是指安全多用途 Internet 邮件扩展（S/MIME）应用程序（如 Outlook）可以使用的对称加密算法。|true、false|SMIME = true|
|UseExistingKeySet|此参数用于指定在生成证书请求时应使用现有的密钥对。 如果将此项设置为 TRUE，则还必须为 RenewalCert 项或 KeyContainer 名称指定值。 你不能设置可导出的密钥，因为你无法更改现有密钥的属性。 在这种情况下，生成证书请求时不会生成密钥材料。|true、false|UseExistingKeySet = true|
|KeyProtection|指定一个值，该值指示在使用之前私钥的保护方式。|XCN_NCRYPT_UI_NO_PROTCTION_FLAG-0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG--1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|指定一个布尔值，该值指示是否在请求中包含默认扩展和属性。 默认值由它们的对象标识符（Oid）表示。|true、false|SuppressDefaults = true|
|FriendlyName|新证书的友好名称。|文本|FriendlyName = "Server1"|
|ValidityPeriodUnits</br>注意:仅当请求类型 = cert 时才使用此类型。|指定要用于 ValidityPeriod 的单位数。|Numeric|ValidityPeriodUnits = 3|
|ValidityPeriod</br>注意:仅当请求类型 = cert 时才使用此类型。|VValidityPeriod 必须是美国英语复数时间段。|年、月、周、天、小时、分钟、秒|ValidityPeriod = 年|

返回[内容](#BKMK_Contents)

**扩展名**

本部分是可选的。


|  扩展 OID   | 定义 | ReplTest1 |                                                                                                                                                                                                                                                                                                                                                                                                                      示例                                                                                                                                                                                                                                                                                                                                                                                                                       |
|------------------|------------|-------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    2.5.29.17     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                2.5.29.17 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "UPN =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "EMail =User@Domain.com&"                                                                                                                                                                                                                                                                                                                                                                                                        |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                        *continue* = "DNS = &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                               *continue* = "DIRECTORYNAME = CN = NAME，DC = DOMAIN，DC = com &"                                                                                                                                                                                                                                                                                                                                                                                               |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                             *continue* = "URL =<http://host.domain.com/default.html&>"                                                                                                                                                                                                                                                                                                                                                                                              |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                         *continue* = "IPAddress = 10.0.0.1 &"                                                                                                                                                                                                                                                                                                                                                                                                         |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                       *continue* = "RegisteredId = 1.2.3.4.5 &"                                                                                                                                                                                                                                                                                                                                                                                                       |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                      *continue* = "1.2.3.4.6.1 = {Utf8} String &"                                                                                                                                                                                                                                                                                                                                                                                                      |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                  *continue* = "1.2.3.4.6.2 = {八进制} AAECAwQFBgc = &"                                                                                                                                                                                                                                                                                                                                                                                                   |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.2.3.4.6.2 = {八进制} {hex} 00 01 02 03 04 05 06 07 &"                                                                                                                                                                                                                                                                                                                                                                                           |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                 *continue* = "1.2.3.4.6.3 = {Asn} BAgAAQIDBAUGBw = = &"                                                                                                                                                                                                                                                                                                                                                                                                  |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                           *continue* = "1.2.3.4.6.3 = {hex} 04 08 00 01 02 03 04 05 06 07"                                                                                                                                                                                                                                                                                                                                                                                            |
|    2.5.29.37     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 2.5.29.37 = "{text}"                                                                                                                                                                                                                                                                                                                                                                                                                 |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                            *continue* = "1.3.6.1.5.5.7"。                                                                                                                                                                                                                                                                                                                                                                                                            |
|    *仍*    |            |       |                                                                                                                                                                                                                                                                                                                                                                                                          *continue* = "1.3.6.1.5.5.7.3.1"                                                                                                                                                                                                                                                                                                                                                                                                          |
|    2.5.29.19     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                              "{text} ca = 0pathlength = 3"                                                                                                                                                                                                                                                                                                                                                                                                              |
|     关键     |            |       |                                                                                                                                                                                                                                                                                                                                                                                                                 严重 = 2.5.29.19                                                                                                                                                                                                                                                                                                                                                                                                                 |
|     KeySpec      |            |       |                                                                                                                                                                                                                                                                                                                                                                                             AT_NONE-0</br>AT_SIGNATURE--2</br>AT_KEYEXCHANGE--1                                                                                                                                                                                                                                                                                                                                                                                             |
|   RequestType    |            |       |                                                                                                                                                                                                                                                                                                                                                                                   PKCS10--1</br>PKCS7--2</br>CMC-3</br>Cert--4</br>SCEP--fd00 （64768）                                                                                                                                                                                                                                                                                                                                                                                   |
|     密钥用法     |            |       |                                                                                                                                                                                                       CERT_DIGITAL_SIGNATURE_KEY_USAGE--80 （128）</br>CERT_NON_REPUDIATION_KEY_USAGE--40 （64）</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE--20 （32）</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE--10 （16）</br>CERT_KEY_AGREEMENT_KEY_USAGE--8</br>CERT_KEY_CERT_SIGN_KEY_USAGE--4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE--2</br>CERT_CRL_SIGN_KEY_USAGE--2</br>CERT_ENCIPHER_ONLY_KEY_USAGE--1</br>CERT_DECIPHER_ONLY_KEY_USAGE--8000 （32768）                                                                                                                                                                                                       |
| KeyUsageProperty |            |       |                                                                                                                                                                                                                                                                                                                                            NCRYPT_ALLOW_DECRYPT_FLAG--1</br>NCRYPT_ALLOW_SIGNING_FLAG--2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG--4</br>NCRYPT_ALLOW_ALL_USAGES--ffffff （16777215）                                                                                                                                                                                                                                                                                                                                             |
|  KeyProtection   |            |       |                                                                                                                                                                                                                                                                                                                                                                NCRYPT_UI_NO_PROTECTION_FLAG-0</br>NCRYPT_UI_PROTECT_KEY_FLAG--1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG--2                                                                                                                                                                                                                                                                                                                                                                 |
| SubjectNameFlags |  模板  |       |                                                                           CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME--40000000 （1073741824）</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH--80000000 （2147483648）</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN--10000000 （268435456）</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL--20000000 （536870912）</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME--8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID--1000000 （16777216）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS--8000000 （134217728）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS--400000 （4194304）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL--4000000 （67108864）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN--800000 （8388608）</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN--2000000 （33554432）                                                                            |
|  X500NameFlags   |            |       | CERT_NAME_STR_NONE-0</br>CERT_OID_NAME_STR--2</br>CERT_X500_NAME_STR--3</br>CERT_NAME_STR_SEMICOLON_FLAG--40000000 （1073741824）</br>CERT_NAME_STR_NO_PLUS_FLAG--20000000 （536870912）</br>CERT_NAME_STR_NO_QUOTING_FLAG--10000000 （268435456）</br>CERT_NAME_STR_CRLF_FLAG--8000000 （134217728）</br>CERT_NAME_STR_COMMA_FLAG--4000000 （67108864）</br>CERT_NAME_STR_REVERSE_FLAG--2000000 （33554432）</br>CERT_NAME_STR_FORWARD_FLAG--1000000 （16777216）</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG--10000 （65536）</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG--20000 （131072）</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG--40000 （262144）</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG--80000 （524288）</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG--100000 （1048576）</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG--200000 （2097152） |

返回[内容](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags 允许 INF 文件根据当前用户或当前计算机属性指定 certreq 自动填充哪些 Subject 和 SubjectAltName 扩展字段：DNS 名称、UPN 等。 使用文本 "template" 意味着改为使用模板名称标志。 这允许在多个上下文中使用单个 INF 文件，以生成具有特定于上下文的主题信息的请求。
>
> X500NameFlags 指定当使用者 INF 密钥值转换为 CertStrToName API 时，要直接传递到 API 的标志。

若要使用 certreq 请求证书，请使用以下示例中的步骤：

> [!WARNING]
> 本主题的内容基于 Windows Server 2008 AD CS 的默认设置;例如，将密钥长度设置为2048，选择 Microsoft 软件密钥存储提供程序作为 CSP，并使用安全哈希算法1（SHA1）。 根据公司的安全策略要求评估这些选择。

若要创建策略文件（.inf），请将下面的示例复制并保存到记事本，并另存为 RequestConfig：
```
[NewRequest] 
Subject = "CN=<FQDN of computer you are creating the certificate>" 
Exportable = TRUE 
KeyLength = 2048 
KeySpec = 1 
KeyUsage = 0xf0 
MachineKeySet = TRUE 
[RequestAttributes]
CertificateTemplate="WebServer"
[Extensions] 
OID = 1.3.6.1.5.5.7.3.1 
OID = 1.3.6.1.5.5.7.3.2  
```
在请求证书的计算机上，键入以下命令：
```
CertReq –New RequestConfig.inf CertRequest.req 
```
下面的示例演示如何实现 Oid 的 [String] 部分语法，以及其他难以解释数据的语法。 用于 EKU 扩展的新 {text} 语法示例，它使用以逗号分隔的 Oid 列表：
```
[Version]
Signature="$Windows NT$

[Strings]
szOID_ENHANCED_KEY_USAGE = "2.5.29.37"
szOID_PKIX_KP_SERVER_AUTH = "1.3.6.1.5.5.7.3.1"
szOID_PKIX_KP_CLIENT_AUTH = "1.3.6.1.5.5.7.3.2"

[NewRequest]
Subject = "CN=TestSelfSignedCert"
Requesttype = Cert

[Extensions]
%szOID_ENHANCED_KEY_USAGE%="{text}%szOID_PKIX_KP_SERVER_AUTH%,"
_continue_ = "%szOID_PKIX_KP_CLIENT_AUTH%"
```
返回[内容](#BKMK_Contents)

## <a name="BKMK_accept"></a>Certreq-accept

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
– Accept 参数使用已颁发的证书链接以前生成的私钥，并从请求证书的系统中删除挂起的证书请求（如果存在匹配的请求）。

可以使用此示例手动接受证书：
```
certreq -accept certnew.cer 
```

> [!WARNING]
> -Accept 谓词、-user 和– machine 选项指示要安装的证书是否应安装在用户或计算机上下文中。 如果在与要安装的公钥相匹配的上下文中存在未完成的请求，则不需要这些选项。 如果没有未完成的请求，则必须指定其中一个。

返回[内容](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq-策略

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   定义限定的部属时应用于 CA 证书的约束的配置文件称为 "策略 .inf"。
-   可以在[附录 A 规划和实现交叉证书和合格的部属](https://technet.microsoft.com/library/cc738878(WS.10).aspx)白皮书中找到策略 .inf 文件的示例。
-   如果在不使用任何其他参数的情况下键入 certreq-policy，将打开一个对话框窗口，以便可以选择请求的文件（请求、cmc、txt、der、cer 或 crt）。 选择所请求的文件并单击 "打开" 按钮后，将打开另一个对话框窗口，以便选择 INF 文件。

可以使用此示例生成交叉证书请求：
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 
```
返回[内容](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq-sign

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   如果在不使用任何其他参数的情况下键入 certreq，它将打开一个对话框窗口，以便你可以选择请求的文件（请求、cmc、txt、der、cer 或 crt）。
-   对合格的部属请求进行签名可能需要企业管理员凭据。 这是为合格的部属颁发签名证书的最佳做法。
-   用于对合格的部属请求进行签名的证书是使用合格的部属模板创建的。 企业管理员将必须为请求签名，或者授予对将对证书进行签名的个人的用户权限。
-   当你对 CMC 请求进行签名时，你可能需要对此请求具有多个人员签名，具体取决于与合格的部属关联的保障级别。
-   如果正在安装的合格从属 CA 的父 CA 处于脱机状态，则必须从脱机父 ca 获取合格从属 CA 的 CA 证书。 如果父 CA 处于联机状态，请在证书服务安装向导中指定合格从属 CA 的 CA 证书。

下面的命令序列将演示如何创建新的证书请求，对其进行签名并提交：
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 
```
返回[内容](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq-注册

注册到证书
```
certreq –enroll [Options] TemplateName
```
续订现有证书
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
只能续订时间有效的证书。 无法续订过期的证书，必须将其替换为新证书。

下面是使用其序列号续订证书的示例：
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
下面是一个示例，说明如何通过使用星号（*）来注册名为 Web 服务器的证书模板，以便通过 U/I 选择策略服务器：
```
certreq -enroll –machine –policyserver * "WebServer"
```
返回[内容](#BKMK_Contents)

## <a name="BKMK_Options"></a>选项

|选项|描述|
|-------|-----------|
|-any|强制 ICertRequest：： Submit 确定编码类型。|
|-attrib \<AttributeString >|指定名称和值字符串对，并以冒号分隔。</br>用 \n 分隔名称和值字符串对（例如，Name1： Value1\nName2： Value2）。|
|-二进制|将输出文件的格式设置为二进制格式，而不是 base64 编码格式。|
|-PolicyServer  *\<PolicyServer >*|"ldap：  *\<路径 >* "</br>插入运行证书注册策略 Web 服务的计算机的 URI 或唯一 ID。</br>若要指定要通过浏览使用请求文件，只需对 *\<policyserver >* 使用减号（-）。|
|-config \<ConfigString >|使用在配置字符串中指定的 CA 来处理操作，该 CAHostName\CAName。 对于 https 连接，请指定注册服务器 URI。 对于本地计算机存储区 CA，使用减号（-）符号。|
|-Anonymous|将匿名凭据用于证书注册 Web 服务。|
|-Kerberos|将 Kerberos （域）凭据用于证书注册 Web 服务。|
|-ClientCertificate  *\<ClientCertId >*|可以将 *\<ClientCertID >* 替换为证书指纹、CN、EKU、模板、电子邮件、UPN 和新的 name = value 语法。|
|-用户名 *\<用户名 >*|用于证书注册 Web 服务。 可以将 *\<UserName >* 替换为 SAM 名称或域 \ 用户 此选项与-p 选项一起使用。|
|-p  *\<密码 >*|用于证书注册 Web 服务。 用实际用户的密码替换 *\<密码 >* 。 此选项与-UserName 选项一起使用。|
|-user|为新的证书申请配置-user 上下文，或指定用于接受证书的上下文。 如果在 INF 或模板中未指定任何内容，则这是默认上下文。|
|-计算机|配置新的证书申请或为计算机上下文的证书指定上下文。 对于新请求，它必须与 MachineKeyset INF 密钥和模板上下文一致。 如果未指定此选项，并且模板未设置上下文，则默认为用户上下文。|
|-crl|将输出中的证书吊销列表（Crl）包含到由 CertChainFileOut 指定的 base64 编码的 PKCS #7 文件或 RequestFileOut 指定的 base64 编码文件中。|
|-rpc|指示 Active Directory 证书服务（AD CS）使用远程过程调用（RPC）服务器连接而不是分布式 COM。|
|-AdminForceMachine|使用密钥服务或模拟从本地系统上下文提交请求。 要求调用此选项的用户是本地管理员的成员。|
|-RenewOnBehalfOf|代表签名证书中标识的使用者提交续订。 这会在调用[ICertRequest：： Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)时设置 CR_IN_ROBO|
|-f|强制覆盖现有文件。 这也会绕过缓存模板和策略。|
|-q|使用静默模式;禁止显示所有交互式提示。|
|-Unicode|将标准输出重定向到另一个命令（在从 Windows PowerShell 中调用®脚本时）时，写入 Unicode 输出。|
|-UnicodeText|将 base64 文本编码的数据 blob 写入文件时，发送 Unicode 输出。|

返回[内容](#BKMK_Contents)

## <a name="BKMK_Formats"></a>Formats

|Formats|描述|
|-------|-----------|
|RequestFileIn|Base64 编码的或二进制输入文件名：PKCS #10 证书请求、CMS 证书请求、PKCS #7 证书续订请求、要交叉认证的 x.509 证书或 Ssh-keygen 标记格式的证书请求。|
|RequestFileOut|Base64 编码的输出文件名|
|CertFileOut|Base64 编码的 X-509 文件名。|
|PKCS10FileOut|仅用于[Certreq](#BKMK_policy)谓词。 Base64 编码的 PKCS10 输出文件名。|
|CertChainFileOut|Base64 编码的 PKCS #7 文件名。|
|FullResponseFileOut|Base64 编码的完整响应文件名。|
|PolicyFileIn|仅用于[Certreq](#BKMK_policy)谓词。 INF 文件，其中包含用于限定请求的扩展的文本表示形式。|

## <a name="BKMK_Examples"></a>其他 certreq 示例

以下文章包含 certreq 用法的示例：
-   [如何使用自定义使用者可选名称请求证书](https://technet.microsoft.com/library/ff625722.aspx)
-   [测试实验室指南：部署 AD CS 双层 PKI 层次结构](https://technet.microsoft.com/library/hh831348.aspx)
-   [附录3：Certreq 语法](https://technet.microsoft.com/library/cc736326.aspx)
-   [如何手动创建 web 服务器 SSL 证书](http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [使用 Windows Server 2008 CA 请求 AMT 设置证书](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [System Center Operations Manager 代理的证书注册](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [AD CS 循序渐进指南：双层 PKI 层次结构部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [如何使用第三方证书颁发机构启用 LDAP over SSL](https://support.microsoft.com/kb/321051)

返回[内容](#BKMK_Contents)
