---
title: certreq
description: 'Windows 命令主题 * * *- '
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
ms.openlocfilehash: 9e48682cee40c00e187ca674bd8019b136a2c3f4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818138"
---
# <a name="certreq"></a>certreq



Certreq 可用于从证书颁发机构 (CA) 申请证书检索到上一个请求从 CA 时，若要从.inf 文件，以接受和安装的响应的请求构造交叉证书创建新的请求的响应或合格的部属请求从现有的 CA 证书或请求，并签署交叉证书或合格的从属请求。

> [!WARNING]
> - 早期版本的 certreq 可能无法提供所有本文档中描述的选项。 您可以查看所有 certreq 的特定版本提供了通过运行语法表示法部分中所示的命令的选项。
> - Certreq 不支持创建新的证书请求基于 CEP/CES 环境中的密钥证明模板

## <a name="BKMK_Contents"></a>内容

在本文中的主要部分如下所示：
1.  [Verbs](#BKMK_Verbs)
2.  [语法表示法](#BKMK_notation)
3.  [选项](#BKMK_Options)
4.  [格式](#BKMK_Formats)
5.  [其他 certreq 示例](#BKMK_Examples)

## <a name="BKMK_Verbs"></a>谓词

那里下表介绍可用于 certreq 命令的谓词

|开关|描述|
|------|-----------|
|-Submit|将请求提交到 CA。 有关详细信息，请参阅[Certreq-提交](#BKMK_Submit)。|
|-检索*RequestID*|检索来自 CA 的上一个请求的响应。 有关详细信息，请参阅[Certreq-检索](#BKMK_Retrieve)。|
|-New|从.inf 文件创建新的请求。 有关详细信息，请参阅[Certreq-新](#BKMK_New)。|
|-Accept|接受并安装证书请求的响应。 有关详细信息，请参阅[Certreq-接受](#BKMK_accept)。|
|-Policy|设置请求的策略。 有关详细信息，请参阅[Certreq-策略](#BKMK_policy)。|
|-Sign|交叉证书或合格的部属申请上签名。 有关详细信息，请参阅[Certreq-登录](#BKMK_sign)。|
|-Enroll|有关注册或续订证书。 有关详细信息，请参阅[Certreq-注册](#BKMK_enroll)。|
|-?|显示 certreq 语法、 选项和说明的列表。|
|*\<verb>* -?|显示指定的谓词的帮助。|
|-v -?|显示 certreq 语法、 选项和说明的详细列表。|

返回到[内容](#BKMK_Contents)

## <a name="BKMK_notation"></a>语法表示法

-   有关基本命令行语法，运行 `certreq -?`
-   有关与特定谓词使用 certutil 语法，运行**certreq** *\<谓词 >* **-？**
-   若要将所有的 certutil 语法发送到文本文件中，运行以下命令：  
    -   `certreq -v -? > certreqhelp.txt`
    -   `notepad certreqhelp.txt`

下表介绍用来指示命令行语法表示法。

|表示法|描述|
|--------|-----------|
|不带括号或大括号的文本|必须键入如下所示的项|
|\<在尖括号内的文本 >|必须为其提供一个值的占位符|
|[方括号内的文本]|可选项|
|{大括号内的文本}|组的所需的项目;选择一个|
|垂直条 (&#124;)|互斥项; 分隔符选择一个|
|省略号 （...）|可以重复的项|

返回到[内容](#BKMK_Contents)

## <a name="BKMK_Submit"></a>Certreq-提交

这是默认 certreq.exe 参数，如果在命令行提示符下显式不指定任何选项，certreq.exe 尝试提交到 CA 的证书请求。
```
CertReq [-Submit] [Options] [RequestFileIn [CertFileOut [CertChainFileOut [FullResponseFileOut]]]]
```
使用时，必须指定证书请求文件 – 提交选项。 如果省略此参数，则将显示一个常见的文件打开窗口，您可以在其中选择相应的证书请求文件。

可以使用这些示例作为起点，构建在证书提交请求：

若要提交简单的证书申请，请使用下面的示例：
```
certreq –submit certRequest.req certnew.cer certnew.pfx
```
若要通过指定的 SAN 属性请求证书，请参阅 Microsoft 知识库文章 931351 中的详细的步骤[如何向安全 LDAP 证书添加使用者备用名称](https://support.microsoft.com/kb/931351)中"如何使用 Certreq.exe 实用程序创建和提交包含 SAN 的证书请求"一节。

返回到[内容](#BKMK_Contents)

## <a name="BKMK_Retrieve"></a>Certreq-检索

```
certreq -retrieve [Options] RequestId [CertFileOut [CertChainFileOut [FullResponseFileOut]]]
```
-   如果不在-config CAComputerName\CANamea 对话框中指定 CAComputerName 或 CAName 出现并显示可用的所有 Ca 的列表。
-   如果你使用-config-而不是-config CAComputerName\CAName，使用默认 CA 处理该操作。
-   可以使用 certreq-检索*RequestID* CA 已实际发出后检索证书。 *RequestID*PKC 可以是十进制或十六进制使用 0x 前缀、 它可以是没有任何 0x 前缀证书序列号。 此外可以使用它来检索曾经颁发 ca，包括已吊销或过期的证书，而不考虑证书的请求是否曾处于挂起状态的任何证书。
-   如果提交请求到 CA，CA 的策略模块可能保留在挂起状态并返回请求*RequestID*到用于显示的 Certreq 调用方。 最终，CA 的管理员将颁发证书或拒绝该请求。

以下命令检索证书 id 20，并创建证书文件 (.cer):
```
certreq -retrieve 20 MyCertificate.cer 

```
返回到[内容](#BKMK_Contents)

## <a name="BKMK_New"></a>Certreq-新

```
certreq -new [Options] [PolicyFileIn [RequestFileOut]]
```
因为 INF 文件允许一组丰富的参数和选项，以指定在很难定义管理员应使用的所有目的的默认模板。 因此，本部分介绍所有选项，以使您能够创建特定需求来定制的 INF 文件。 以下关键字用于描述 INF 文件结构。
1.  一个*部分*是包含密钥的逻辑组的 INF 文件中的某个区域。 始终 INF 文件中的括号内，将显示一个部分。
2.  一个*密钥*是等号左侧的参数。
3.  一个*值*是等号右侧的参数。

例如，最小的 INF 文件将类似以下：
```
[NewRequest] 
; At least one value must be set in this section 
Subject = "CN=W2K8-BO-DC.contoso2.com"
```
以下是一些可能会添加到 INF 文件，可能部分：

**[NewRequest]**

本部分是必需的它就像新的证书请求的模板的 INF 文件。 本部分中需要至少一个密钥值。

|键|定义|ReplTest1|示例|
|---|----------|-----|-------|
|主题|多个应用程序依赖于证书的使用者信息。 因此，建议指定此密钥的值。 如果使用者未在此处设置，则建议使用者名称是作为使用者可选名称证书扩展的一部分。|相对可分辨名称字符串值|Subject = "CN=computer1.contoso.com" Subject="CN=John Smith,CN=Users,DC=Contoso,DC=com"|
|Exportable|如果此属性设置为 TRUE，可以与证书一起导出的私钥。 若要确保高级别的安全性，私钥不应为可导出;但是，在某些情况下，它可能需要使私钥可导出，如果多台计算机或用户必须共享相同的私钥。|true、false|可导出 = TRUE。 CNG 密钥可区分和纯文本可导出。 CAPI1 密钥不能。|
|ExportableEncrypted|指定是否应设置私钥可导出。|true、false|ExportableEncrypted = true</br>提示：并非所有公共密钥大小和算法将使用所有哈希算法。 Tamehe 指定 CSP 还必须支持指定的哈希算法。 若要查看支持的哈希算法的列表，可以运行命令 <code>certutil -oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo</code>|
|HashAlgorithm|要用于此请求的哈希算法。|Sha256、 sha384、 sha512、 sha1、 md5、 md4、 md2|HashAlgorithm = sha1。 若要查看使用支持的哈希算法列表： certutil-oid 1 &#124; findstr pwszCNGAlgid &#124; findstr /v CryptOIDInfo|
|KeyAlgorithm|服务提供程序将用于生成公钥和私钥对该算法。|RSA, DH, DSA, ECDH_P256, ECDH_P521, ECDSA_P256, ECDSA_P384, ECDSA_P521|KeyAlgorithm = RSA|
|KeyContainer|建议不要设置此参数为新请求其中生成新密钥材料。 密钥容器自动生成并由系统维护。 对于其中应使用现有的密钥材料的请求，此值可以设置为现有密钥的密钥容器名称。 使用 certutil – 密钥命令来显示有关计算机上下文的可用密钥容器的列表。 使用 certutil – 密钥-当前用户的上下文的用户命令。|随机字符串值</br>提示：应使用双引号括住任何 INF 密钥值，该值包含空格或特殊字符以避免潜在 INF 分析问题。|KeyContainer = {C347BD28-7F69-4090-AA16-BC58CF4D749C}|
|KeyLength|定义的公共和私有密钥的长度。 密钥长度会影响该证书的安全级别。 更高版本的密钥长度通常会提供较高的安全级别;但是，某些应用程序可能具有有关密钥的长度限制。|支持加密服务提供程序的任何有效长度。|KeyLength = 2048|
|KeySpec|确定签名、 Exchange （加密），或两个可以使用该密钥。|AT_NONE, AT_SIGNATURE, AT_KEYEXCHANGE|KeySpec = AT_KEYEXCHANGE|
|密钥用法|定义证书密钥应使用什么来。|CERT_DIGITAL_SIGNATURE_KEY_USAGE-80 (128)</br>提示：显示的值是有关每个定义的位的十六进制 （十进制） 值。 此外可以使用较旧的语法： 具有多个单个的十六进制值的位集，而不是符号的表示形式。 例如，密钥用法 = 0xa0。</br>CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE -- 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE -- 1</br>CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)|KeyUsage = "CERT_DIGITAL_SIGNATURE_KEY_USAGE &#124; CERT_KEY_ENCIPHERMENT_KEY_USAGE"</br>提示：多个值使用一个管道 (&#124;) 符号分隔符。 请确保使用多个值以避免 INF 分析问题时，使用双引号。|
|KeyUsageProperty|检索一个值，标识可以为其使用的专用密钥的特定用途。|NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)|KeyUsageProperty = "NCRYPT_ALLOW_DECRYPT_FLAG &#124; NCRYPT_ALLOW_SIGNING_FLAG"|
|MachineKeySet|当您需要创建所拥有的计算机而非用户的证书时，此密钥非常重要。 生成的密钥材料维护的安全上下文中的安全主体 （用户或计算机帐户） 已创建了请求。 当管理员创建代表计算机的证书请求时，必须在计算机的安全上下文和非管理员的安全上下文中创建的密钥材料。 否则，计算机无法访问其私钥，因为它将是管理员的安全上下文中。|true、false|MachineKeySet = true</br>提示：默认值为 false。|
|NotBefore|指定日期或日期和时间不能在其之前发出请求。 NotBefore 可以用于 ValidityPeriod 和 ValidityPeriodUnits。|日期或日期和时间|NotBefore ="2012 年 7 月 24 日上午 10:31"</br>提示：NotBefore 和 NotAfter 是针对 RequestType = 仅证书。日期分析会尝试进行区分区域设置的。使用的月份名称将消除歧义，即可在每个区域设置中正常工作。|
|NotAfter|指定日期或日期和时间之后不能发出请求。 NotAfter 不能用于 ValidityPeriod 或 ValidityPeriodUnits。|日期或日期和时间|NotAfter ="2014 年 9 月 23 日上午 10:31"</br>提示：NotBefore 和 NotAfter 是针对 RequestType = 仅证书。日期分析会尝试进行区分区域设置的。使用的月份名称将消除歧义，即可在每个区域设置中正常工作。|
|PrivateKeyArchive|PrivateKeyArchive 设置仅适用于相应的 RequestType 设置为"CMC"，因为只有证书管理消息 CMS (CMC) 请求格式通过允许安全地将请求者的私钥传输给 CA 进行密钥存档。|true、false|PrivateKeyArchive = True|
|EncryptionAlgorithm|要使用的加密算法。|可能的选项会有所不同，具体取决于操作系统版本和已安装的加密提供程序集。 若要查看可用的算法的列表，请运行命令<code>certutil -oid 2 &#124; findstr pwszCNGAlgid</code>指定使用的 CSP 还必须支持指定的对称加密算法和长度。|EncryptionAlgorithm = 3des|
|EncryptionLength|要使用的加密算法的长度。|允许指定 EncryptionAlgorithm 任何长度。|EncryptionLength = 128|
|ProviderName|提供程序名称是 CSP 的显示名称...|如果不知道正在使用的 CSP 的提供程序名称，请从命令行运行 certutil – csplist。 该命令将显示在本地系统上可用的所有 csp 名称|ProviderName = "Microsoft RSA SChannel Cryptographic Provider"|
|ProviderType|提供程序类型用于选择特定的提供程序根据特定的算法功能，如"RSA Full"。|如果不知道正在使用的 CSP 的提供程序类型，请从命令行提示符运行 certutil – csplist。 该命令将显示所有的 Csp 而言是本地系统上可用的提供程序类型。|ProviderType = 1|
|RenewalCert|如果需要续订证书请求生成的位置在系统存在的证书，则必须为此键的值指定其证书哈希。|可在其中创建证书请求的计算机上任何证书的证书哈希。 如果不知道证书哈希，使用证书 Mmc 管理单元，并查看应续订的证书。 打开证书属性，请参阅该证书的"指纹"属性。 证书续订需要 PKCS #7 或 CMC 请求格式。|RenewalCert = 4EDF274BD2919C6E9EC6A522F0F3B153E9B1582D|
|RequesterName</br>注意：这使得请求代表另一个用户请求进行注册。此外必须使用 Enrollment Agent 证书签名请求或 CA 会拒绝该请求。 使用-证书选项指定注册代理证书。|如果 RequestType 设置为 PKCS #7 或 CMC，可以为证书申请指定请求者名称。 如果 RequestType 设置为 PKCS #10，将忽略此密钥。 Requestername 只能设置为请求的一部分。 不能操作 Requestername 中挂起的请求。|域 \ 用户|Requestername = "Contoso\BSmith"|
|RequestType|确定用于生成和发送证书请求的标准。|PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>证书-4</br>SCEP -- fd00 (64768)</br>提示：此选项指示自签名或自行颁发的证书。 它不会生成一个请求，但而不是新的证书，然后安装该证书。自签名是默认值。使用指定的签名证书创建自行颁发的证书不是自签名的 – 证书选项。|RequestType = CMC|
|SecurityDescriptor</br>提示：这是仅适用于计算机上下文非智能卡密钥。|包含与安全对象关联的安全信息。 最安全的对象，可以在创建对象的函数调用中指定对象的安全描述符。|字符串根据[安全描述符定义语言](https://msdn.microsoft.com/library/aa379567(v=vs.85).aspx)。|SecurityDescriptor = "D:P(A;;GA;;;SY)(A;;GA;;;BA)"|
|AlternateSignatureAlgorithm|指定和检索一个布尔值，指示 PKCS #10 请求或证书签名的签名算法对象标识符 (OID) 是离散的还是组合。|true、false|内容： AlternateSignatureAlgorithm = false</br>提示：对于 RSA 签名，false 表示 Pkcs1 v1.5。 True 表示 v2.1 签名。|
|静默|默认情况下，此选项允许用户从 CSP 访问交互式用户桌面和请求信息，如智能卡 PIN。 如果此项设置为 TRUE，CSP 必须与桌面进行交互，并将阻止向用户显示任何用户界面。|true、false|无提示 = true|
|SMIME|如果此参数设置为 TRUE，则将使用对象标识符值 1.2.840.113549.1.9.15 扩展添加到请求。 对象标识符的数量取决于在安装操作系统版本和 CSP 功能，后者是指可能由安全多用途 Internet 邮件扩展 (S/MIME) 应用程序，如 Outlook 的对称加密算法。|true、false|SMIME = true|
|UseExistingKeySet|此参数用于指定应在生成证书请求过程中使用的现有密钥对。 如果此项设置为 TRUE，还必须指定 RenewalCert 密钥或 KeyContainer 名称的值。 不，你必须设置可导出的密钥，因为不能更改现有键的属性。 在这种情况下，生成证书请求时生成没有密钥材料。|true、false|UseExistingKeySet = true|
|KeyProtection|指定一个值，指示如何在使用前保护私钥。|XCN_NCRYPT_UI_NO_PROTCTION_FLAG -- 0</br>XCN_NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>XCN_NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|KeyProtection = NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG|
|SuppressDefaults|指定一个布尔值，该值指示是否请求中包含的默认扩展插件和属性。 默认值由其对象标识符 (Oid) 表示。|true、false|SuppressDefaults = true|
|FriendlyName|新的证书的友好名称。|Text|FriendlyName = "Server1"|
|ValidityPeriodUnits</br>注意：仅使用此请求的类型时 = 证书。|指定的与 ValidityPeriod 一起使用的单位数。|数字|ValidityPeriodUnits = 3|
|ValidityPeriod</br>注意：仅使用此请求的类型时 = 证书。|VValidityPeriod 必须为美国英语复数时间段。|年、 月、 周、 天、 小时数，分钟秒|ValidityPeriod = 年|

返回到[内容](#BKMK_Contents)

**[扩展]**

本部分是可选的。

|扩展 OID|定义|值|示例|
|-------------|----------|-----|-------|
|2.5.29.17|||2.5.29.17 = "{text}"|
|_continue_|||_continue_ = "UPN=User@Domain.com&"|
|_continue_|||_continue_ = "EMail=User@Domain.com&"|
|_continue_|||_continue_ = "DNS=host.domain.com&"|
|_continue_|||_continue_ = "DirectoryName=CN=Name,DC=Domain,DC=com&"|
|_continue_|||_continue_ = "URL=http://host.domain.com/default.html&"|
|_continue_|||_continue_ = "IPAddress=10.0.0.1&"|
|_continue_|||_continue_ = "RegisteredId=1.2.3.4.5&"|
|_continue_|||_continue_ = "1.2.3.4.6.1={utf8}String&"|
|_continue_|||_continue_ = "1.2.3.4.6.2={octet}AAECAwQFBgc=&"|
|_continue_|||_continue_ = "1.2.3.4.6.2={octet}{hex}00 01 02 03 04 05 06 07&"|
|_continue_|||_continue_ = "1.2.3.4.6.3={asn}BAgAAQIDBAUGBw==&"|
|_continue_|||_continue_ = "1.2.3.4.6.3={hex}04 08 00 01 02 03 04 05 06 07"|
|2.5.29.37|||2.5.29.37="{text}"|
|_continue_|||_continue_ = "1.3.6.1.5.5.7.|
|_continue_|||_continue_ = "1.3.6.1.5.5.7.3.1"|
|2.5.29.19|||"{text}ca=0pathlength=3"|
|严重|||Critical=2.5.29.19|
|KeySpec|||AT_NONE -- 0</br>AT_SIGNATURE-2</br>AT_KEYEXCHANGE -- 1|
|RequestType|||PKCS10 -- 1</br>PKCS7 -- 2</br>CMC -- 3</br>证书-4</br>SCEP -- fd00 (64768)|
|密钥用法|||CERT_DIGITAL_SIGNATURE_KEY_USAGE-80 (128)</br>CERT_NON_REPUDIATION_KEY_USAGE -- 40 (64)</br>CERT_KEY_ENCIPHERMENT_KEY_USAGE -- 20 (32)</br>CERT_DATA_ENCIPHERMENT_KEY_USAGE -- 10 (16)</br>CERT_KEY_AGREEMENT_KEY_USAGE -- 8</br>CERT_KEY_CERT_SIGN_KEY_USAGE -- 4</br>CERT_OFFLINE_CRL_SIGN_KEY_USAGE -- 2</br>CERT_CRL_SIGN_KEY_USAGE -- 2</br>CERT_ENCIPHER_ONLY_KEY_USAGE -- 1</br>CERT_DECIPHER_ONLY_KEY_USAGE -- 8000 (32768)|
|KeyUsageProperty|||NCRYPT_ALLOW_DECRYPT_FLAG -- 1</br>NCRYPT_ALLOW_SIGNING_FLAG -- 2</br>NCRYPT_ALLOW_KEY_AGREEMENT_FLAG -- 4</br>NCRYPT_ALLOW_ALL_USAGES -- ffffff (16777215)|
|KeyProtection|||NCRYPT_UI_NO_PROTECTION_FLAG -- 0</br>NCRYPT_UI_PROTECT_KEY_FLAG -- 1</br>NCRYPT_UI_FORCE_HIGH_PROTECTION_FLAG -- 2|
|SubjectNameFlags|模板||CT_FLAG_SUBJECT_REQUIRE_COMMON_NAME-40000000 (1073741824)</br>CT_FLAG_SUBJECT_REQUIRE_DIRECTORY_PATH -- 80000000 (2147483648)</br>CT_FLAG_SUBJECT_REQUIRE_DNS_AS_CN -- 10000000 (268435456)</br>CT_FLAG_SUBJECT_REQUIRE_EMAIL -- 20000000 (536870912)</br>CT_FLAG_OLD_CERT_SUPPLIES_SUBJECT_AND_ALT_NAME-8</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DIRECTORY_GUID -- 1000000 (16777216)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DNS -- 8000000 (134217728)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_DOMAIN_DNS -- 400000 (4194304)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_EMAIL -- 4000000 (67108864)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_SPN -- 800000 (8388608)</br>CT_FLAG_SUBJECT_ALT_REQUIRE_UPN -- 2000000 (33554432)|
|X500NameFlags|||CERT_NAME_STR_NONE -- 0</br>CERT_OID_NAME_STR -- 2</br>CERT_X500_NAME_STR -- 3</br>CERT_NAME_STR_SEMICOLON_FLAG -- 40000000 (1073741824)</br>CERT_NAME_STR_NO_PLUS_FLAG -- 20000000 (536870912)</br>CERT_NAME_STR_NO_QUOTING_FLAG-10000000 (268435456)</br>CERT_NAME_STR_CRLF_FLAG -- 8000000 (134217728)</br>CERT_NAME_STR_COMMA_FLAG -- 4000000 (67108864)</br>CERT_NAME_STR_REVERSE_FLAG -- 2000000 (33554432)</br>CERT_NAME_STR_FORWARD_FLAG-1000000 (16777216)</br>CERT_NAME_STR_DISABLE_IE4_UTF8_FLAG -- 10000 (65536)</br>CERT_NAME_STR_ENABLE_T61_UNICODE_FLAG -- 20000 (131072)</br>CERT_NAME_STR_ENABLE_UTF8_UNICODE_FLAG -- 40000 (262144)</br>CERT_NAME_STR_FORCE_UTF8_DIR_STR_FLAG -- 80000 (524288)</br>CERT_NAME_STR_DISABLE_UTF8_DIR_STR_FLAG -- 100000 (1048576)</br>CERT_NAME_STR_ENABLE_PUNYCODE_FLAG -- 200000 (2097152)|

返回到[内容](#BKMK_Contents)

> [!NOTE]
> SubjectNameFlags 允许 INF 文件，以指定哪些使用者和 SubjectAltName 扩展字段应该是由 certreq 基于当前用户或当前计算机属性自动填充：DNS 名称、 UPN 和等等。 使用文本"模板"意味着改为使用的模板名称标志。 这允许单个的 INF 文件以使用多个上下文中生成具有特定于上下文的使用者信息的请求。
>
> X500NameFlags 指定要被直接传递到 CertStrToName API 时的使用者 INF 密钥值转换为 ASN.1 的标志编码可分辨名称。

若要申请证书基于使用 certreq-新使用下面的示例中的步骤：

> [!WARNING]
> 此主题的内容基于 Windows Server 2008 AD CS; 的默认设置例如，密钥长度设置为 2048，选择 Microsoft Software Key Storage Provider 作为 CSP，并使用安全哈希算法 1 (SHA1)。 根据公司的安全策略的要求这些选择的计算结果。

若要创建的策略文件 (.inf) 复制和在记事本中保存下面的示例并将另存为 RequestConfig.inf:
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
申请证书的计算机上键入以下命令：
```
CertReq –New RequestConfig.inf CertRequest.req 

```
下面的示例演示如何实现 [Strings] 部分语法 Oid 和其他难以解释的数据。 EKU 扩展，它使用 Oid 的以逗号分隔列表的新 {text} 语法示例：
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
返回到[内容](#BKMK_Contents)

## <a name="BKMK_accept"></a>Certreq -accept

```
CertReq -accept [Options] [CertChainFileIn | FullResponseFileIn | CertFileIn]
```
– 接受参数链接以前生成的私钥与已颁发的证书和从证书 （如果没有匹配的请求） 的请求所在的系统中删除挂起的证书申请。

用于手动接受证书，可以使用以下示例：
```
certreq -accept certnew.cer 

```

> [!WARNING]
> -接受谓词，则-用户和 – 计算机选项指示是否应在用户或计算机上下文中安装正在安装证书。 如果在与要安装的公钥匹配任一上下文中没有未完成请求，则不需要这些选项。 如果存在任何未完成请求，则其中一个必须指定。

返回到[内容](#BKMK_Contents)

## <a name="BKMK_policy"></a>Certreq-策略

```
certreq -policy [-attrib AttributeString] [-binary] [-cert CertID] [RequestFileIn [PolicyFileIn [RequestFileOut [PKCS10FileOut]]]]
```
-   定义的约束的定义合格的部属时应用于 CA 证书配置文件称为 Policy.inf...
-   您可以找到 Policy.inf 文件中的示例[附录规划和实现交叉证书和合格的部属](https://technet.microsoft.com/library/cc738878(WS.10).aspx)白皮书。
-   如果键入 certreq-而无需任何附加参数的策略会打开一个对话框窗口这样您就可以选择请求文件 （请求、 cmc、 txt、 der、 cer 或 crt）。 一旦您选择请求的文件，并单击打开按钮，以便选择 INF 文件将打开另一个对话框窗口。

此示例可用于生成交叉证书请求：
```
certreq -policy Certsrv.req Policy.inf newcertsrv.req 

```
返回到[内容](#BKMK_Contents)

## <a name="BKMK_sign"></a>Certreq-登录

```
certreq -sign [Options] [RequestFileIn [RequestFileOut]]
```
-   如果键入 certreq-登录而无需任何附加参数它将打开一个对话框窗口，这样您就可以选择请求的文件 （请求、 cmc、 txt、 der、 cer 或 crt）。
-   签名合格的部属请求可能需要企业管理员凭据。 这是用于颁发签名证书用于合格的部属最佳做法。
-   使用合格的部属模板来创建用于合格的部属请求签名的证书。 企业管理员将需要登录请求或授予用户权限将证书进行签名的个人。
-   当签名 CMC 请求时，可能需要具有多个人员登录此请求，具体取决于与合格的部属相关联的保证级别。
-   如果合格的从属 CA 安装父 CA 处于脱机状态，必须从脱机父为合格的从属 CA 获取 CA 证书。 如果父 CA 处于联机状态，指定 CA 证书的合格的从属 CA 证书服务安装向导运行期间。

下面的命令系列将演示如何创建新的证书请求、 对其进行签名并将其提交：
```
certreq -new policyfile.inf MyRequest.req
certreq -sign MyRequest.req MyRequest_Sign.req
certreq -submit MyRequest_Sign.req MyRequest_cert.cer 

```
返回到[内容](#BKMK_Contents)

## <a name="BKMK_enroll"></a>Certreq-注册

若要注册到的证书
```
certreq –enroll [Options] TemplateName
```
若要续订现有证书
```
certreq –enroll –cert CertId [Options] Renew [ReuseKeys]
```
仅可以续订证书的有效时间。 已过期的证书无法续订，必须使用新证书替换。

下面是续订证书，证书使用其序列号的示例：
```
certreq –enroll -machine –cert "61 2d 3c fe 00 00 00 00 00 05" Renew
```
此处注册到证书模板的示例通过使用星号 （*） 来选择 U/实现： 通过策略服务器调用 web 服务器
```
certreq -enroll –machine –policyserver * "WebServer"
```
返回到[内容](#BKMK_Contents)

## <a name="BKMK_Options"></a>选项

|选项|描述|
|-------|-----------|
|-任何|强制 ICertRequest::Submit 来确定编码类型。|
|-attrib \<AttributeString>|指定名称和值字符串对，用冒号分隔。</br>单独的名称和值字符串对使用 \n (例如，Name1:Value1\nName2:Value2)。|
|-二进制|格式为而不是 base64 编码的二进制输出文件。|
|-PolicyServer *\<PolicyServer>*|"ldap: *\<路径 >*"</br>插入的 URI 或运行证书注册策略 Web 服务的计算机的唯一 ID。</br>若要指定想要通过浏览使用请求文件，只需使用相减 （-） 符号*\<服务器 >*。|
|-config \<ConfigString>|通过使用配置字符串，它是 CAHostName\CAName 中指定 CA 处理该操作。 用于 https 连接，指定注册服务器 URI。 用于在本地计算机存储区 CA，请使用相减 （-） 登录。|
|-Anonymous|对证书注册 Web 服务使用匿名凭据。|
|-Kerberos|证书注册 Web 服务使用 Kerberos （域） 凭据。|
|-ClientCertificate *\<ClientCertId>*|您可以替换 *\<ClientCertID >* 与证书指纹、 CN、 EKU、 模板、 电子邮件、 UPN 和新名称 = 值语法。|
|-UserName *\<用户名 >*|与证书注册 Web 服务一起使用。 你可以替换*\<用户名 >* SAM 名称或域 \ 用户。 此选项适用于与-p 选项一起使用。|
|-p *\<密码 >*|与证书注册 Web 服务一起使用。 Substitute *\<密码 >* 实际用户的密码。 此选项适用于与-UserName 选项一起使用。|
|-用户|配置新的证书请求的用户上下文或指定的上下文证书验收。 这是默认上下文中，如果未指定任何 INF 或模板中。|
|-计算机|配置新的证书请求，或指定的上下文计算机上下文证书验收。 为新请求，它必须与 MachineKeyset INF 密钥和模板上下文一致。 如果未指定此选项，并且该模板不会设置上下文，默认值是用户上下文。|
|-crl|包括证书吊销列表 (Crl) 输出到指定 CertChainFileOut 的 base64 编码 PKCS #7 文件或指定 RequestFileOut 的 base64 编码文件中。|
|-rpc|指示 Active Directory 证书服务 (AD CS) 若要使用远程过程调用 (RPC) 服务器连接，而不是分布式 com。|
|-AdminForceMachine|使用密钥服务或模拟提交本地系统上下文中的请求。 需要调用此选项的用户是本地管理员的成员。|
|-RenewOnBehalfOf|代表使用者标识签名证书中提交了续订。 这将调用时设置 CR_IN_ROBO [ICertRequest::Submit](https://technet.microsoft.com/subscriptions/aa385040.aspx)|
|-f|强制覆盖现有文件。 这也绕过缓存模板和策略。|
|-q|使用无提示模式;取消所有交互式提示。|
|-Unicode|Unicode 输出时写入标准输出重定向或通过管道传递给另一个命令，可帮助从 Windows PowerShell® 脚本调用时）。|
|-UnicodeText|将 Unicode 输出发送时编写的 base64 文本编码的文件的数据 blob。|

返回到[内容](#BKMK_Contents)

## <a name="BKMK_Formats"></a>格式

|格式|描述|
|-------|-----------|
|RequestFileIn|Base64 编码或二进制输入的文件名称：PKCS #10 证书请求、 CMS 证书申请，PKCS #7 证书续订请求、 要交叉验证的 X.509 证书或 KeyGen 标记格式证书请求。|
|RequestFileOut|Base64 编码的输出文件名称|
|CertFileOut|Base64 编码的 x-509 文件名。|
|PKCS10FileOut|以用于[Certreq-策略](#BKMK_policy)仅谓词。 Base64 编码 PKCS10 输出文件名称。|
|CertChainFileOut|Base64 编码 PKCS #7 文件的名称。|
|FullResponseFileOut|Base64 编码的完整响应文件的名称。|
|PolicyFileIn|以用于[Certreq-策略](#BKMK_policy)仅谓词。 包含用来限定请求的扩展插件的文本表示形式的 INF 文件。|

## <a name="BKMK_Examples"></a>其他 certreq 示例

以下文章包含 certreq 用法的示例：
-   [如何使用自定义使用者可选名称的申请证书](https://technet.microsoft.com/library/ff625722.aspx)
-   [测试实验室指南：部署 AD CS 两层 PKI 层次结构](https://technet.microsoft.com/library/hh831348.aspx)
-   [附录 3:Certreq.exe 语法](https://technet.microsoft.com/library/cc736326.aspx)
-   [如何手动创建 web 服务器 SSL 证书](http://blogs.technet.com/b/pki/archive/2009/08/05/how-to-create-a-web-server-ssl-certificate-manually.aspx)
-   [请求 AMT 设置证书使用的是 Windows Server 2008 CA](https://social.technet.microsoft.com/wiki/contents/articles/request-an-amt-provisioning-certificate-using-a-windows-server-2008-ca.aspx)
-   [System Center Operations Manager 代理的证书注册](https://social.technet.microsoft.com/wiki/contents/articles/certificate-enrollment-for-system-center-operations-manager-agent.aspx)
-   [AD CS 迁移分步指南：两层 PKI 层次结构部署](https://social.technet.microsoft.com/wiki/contents/articles/15037.ad-cs-step-by-step-guide-two-tier-pki-hierarchy-deployment.aspx)
-   [How to enable LDAP over SSL 的第三方证书颁发机构](https://support.microsoft.com/kb/321051)

返回到[内容](#BKMK_Contents)
