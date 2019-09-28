---
title: certutil
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c264ccf0-ba1e-412b-9dd3-d77dd9345ad9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45c9946cc53fe3a901c3f6ee53f082a5b3d086c0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379648"
---
# <a name="certutil"></a>certutil

Certutil 是作为证书服务的一部分安装的命令行程序。 可以使用 Certutil 转储并显示证书颁发机构 (CA) 配置信息、配置证书服务、备份和还原 CA 组件以及验证证书、密钥对和证书链。

如果在没有其他参数的情况下在证书颁发机构上运行 certutil, 则会显示当前的证书颁发机构配置。 在非证书颁发机构上运行 cerutil 时, 该命令默认为运行 certutil [-dump](#-dump)谓词。

> [!WARNING]
> 更早版本的 certutil 可能不提供本文档中所述的所有选项。 可以通过运行[语法表示法](#syntax-notations)部分中显示的命令来查看特定版本的 certutil 提供的所有选项。

## <a name="menu"></a>菜单

本文档的主要部分包括:

- [谓词](#verbs)
- [语法表示法](#syntax-notations)
- [选项](#options)
- [其他 certutil 示例](#additional-certutil-examples)

## <a name="verbs"></a>谓词

下表描述了可与 certutil 命令一起使用的谓词。

|谓词|描述|
|-----|-----------|
|[-dump](#-dump)|转储配置信息或文件|
|[-asn](#-asn)|分析 ASN 1 文件|
|[-decodehex](#-decodehex)|解码十六进制编码的文件|
|[-解码](#-decode)|对 Base64 编码的文件进行解码|
|[-编码](#-encode)|将文件编码为 Base64|
|[-deny](#-deny)|拒绝挂起的证书申请|
|[-重新提交](#-resubmit)|重新提交挂起的证书申请|
|[-setattributes](#-setattributes)|设置挂起证书请求的属性|
|[-setextension](#-setextension)|设置挂起证书请求的扩展|
|[-revoke](#-revoke)|吊销证书|
|[-isvalid](#-isvalid)|显示当前证书的处置|
|[-getconfig](#-getconfig)|获取默认配置字符串|
|[-ping](#-ping)|尝试联系 Active Directory 证书服务请求接口|
|-pingadmin|尝试联系 Active Directory 的证书服务管理界面|
|[-CAInfo](#-cainfo)|显示有关证书颁发机构的信息|
|[-ca. cert](#-cacert)|检索证书颁发机构的证书|
|[-ca。](#-cachain)|检索证书颁发机构的证书链|
|[-GetCRL](#-getcrl)|获取证书吊销列表 (CRL)|
|[-CRL](#-crl)|发布新证书吊销列表（Crl） [或仅限增量 Crl]|
|[-shutdown](#-shutdown)|关闭 Active Directory 证书服务|
|[-installCert](#-installcert)|安装证书颁发机构证书|
|[-renewCert](#-renewcert)|续订证书颁发机构证书|
|[-架构](#-schema)|转储证书的架构|
|[-view](#-view)|转储证书视图|
|[-db](#-db)|转储原始数据库|
|[-deleterow](#-deleterow)|从服务器数据库中删除行|
|[-备份](#-backup)|备份 Active Directory 证书服务|
|[-backupDB](#-backupdb)|备份 Active Directory 证书服务数据库|
|[-backupKey](#-backupkey)|备份 Active Directory 证书服务证书和私钥|
|[-restore](#-restore)|还原 Active Directory 证书服务|
|[-restoreDB](#-restoredb)|还原 Active Directory 证书服务数据库|
|[-restoreKey](#-restorekey)|还原 Active Directory 证书服务证书和私钥|
|[-importPFX](#-importpfx)|导入证书和私钥|
|[-dynamicfilelist](#-dynamicfilelist)|显示动态文件列表|
|[-databaselocations](#-databaselocations)|显示数据库位置|
|[-hashfile](#-hashfile)|生成和显示文件的加密哈希|
|[-存储](#-store)|转储证书存储|
|[-addstore](#-addstore)|将证书添加到应用商店|
|[-delstore](#-delstore)|从应用商店中删除证书|
|[-verifystore](#-verifystore)|验证存储中的证书|
|[-repairstore](#-repairstore)|修复密钥关联或更新证书属性或密钥安全描述符|
|[-viewstore](#-viewstore)|转储证书存储区|
|[-viewdelstore](#-viewdelstore)|从应用商店中删除证书|
|[-dsPublish](#-dspublish)|将证书或证书吊销列表 (CRL) 发布到 Active Directory|
|[-ADTemplate](#-adtemplate)|显示 AD 模板|
|[-Template](#-template)|显示证书模板|
|[-TemplateCAs](#-templatecas)|显示证书模板的证书颁发机构 (Ca)|
|[-Catemplates.txt](#-catemplates)|显示 CA 的模板|
|[-SetCASites](#-setcasites)|管理 Ca 的站点名称|
|[-enrollmentServerURL](#-enrollmentserverurl)|显示、添加或删除与 CA 关联的注册服务器 Url|
|[-ADCA](#-adca)|显示 AD Ca|
|[-CA](#-ca)|显示注册策略 Ca|
|[-Policy](#-policy)|显示注册策略|
|[-PolicyCache](#-policycache)|显示或删除注册策略缓存条目|
|[-CredStore](#-credstore)|显示、添加或删除凭据存储区项|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|安装默认证书模板|
|[-URLCache](#-urlcache)|显示或删除 URL 缓存条目|
|[-脉冲](#-pulse)|脉冲自动注册事件|
|[-MachineInfo](#-machineinfo)|显示有关 Active Directory 计算机对象的信息|
|[-DCInfo](#-dcinfo)|显示有关域控制器的信息|
|[-EntInfo](#-entinfo)|显示有关企业 CA 的信息|
|[-TCAInfo](#-tcainfo)|显示有关 CA 的信息|
|[-SCInfo](#-scinfo)|显示有关智能卡的信息|
|[-SCRoots](#-scroots)|管理智能卡根证书|
|[-verifykeys](#-verifykeys)|验证公钥或私钥集|
|[-验证](#-verify)|验证证书、证书吊销列表 (CRL) 或证书链|
|[-verifyCTL](#-verifyctl)|验证 AuthRoot 或不允许的证书 CTL|
|[-sign](#-sign)|重新签署证书吊销列表 (CRL) 或证书|
|[-vroot](#-vroot)|创建或删除 web 虚拟根和文件共享|
|[-vocsproot](#-vocsproot)|创建或删除 OCSP web 代理的 web 虚拟根|
|[-addEnrollmentServer](#-addenrollmentserver)|添加注册服务器应用程序|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|删除注册服务器应用程序|
|[-addPolicyServer](#-addpolicyserver)|添加策略服务器应用程序|
|[-deletePolicyServer](#-deletepolicyserver)|删除策略服务器应用程序|
|[-oid](#-oid)|显示对象标识符或设置显示名称|
|[-错误](#-error)|显示与错误代码关联的消息文本|
|[-getreg](#-getreg)|显示注册表值|
|[-setreg](#-setreg)|设置注册表值|
|[-delreg](#-delreg)|删除注册表值|
|[-ImportKMS](#-importkms)|将用户密钥和证书导入到服务器数据库以进行密钥存档|
|[-ImportCert](#-importcert)|将证书文件导入到数据库中|
|[-GetKey](#-getkey)|检索存档的私钥恢复 blob|
|[-RecoverKey](#-recoverkey)|恢复存档的私钥|
|[-MergePFX](#-mergepfx)|合并 PFX 文件|
|[-ConvertEPF](#-convertepf)|将 PFX 文件转换为 EPF 文件|
|-?|显示谓词的列表|
|-谓词 >-？  *\<*|显示指定谓词的帮助。|
|-? -v|显示动词和的完整列表|

返回[菜单](#menu)

## <a name="syntax-notations"></a>语法表示法

- 有关基本的命令行语法, 请运行`certutil -?`
- 有关将 certutil 与特定谓词一起使用的语法, 请运行**certutil**  *\<verb >* **-？**
- 若要将所有 certutil 语法发送到文本文件, 请运行以下命令:  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

下表描述了用于指示命令行语法的表示法。


|            图解             |                  描述                  |
|---------------------------------|-----------------------------------------------|
| 不带方括号或大括号的文本 |         必须按如下所示键入项          |
|  \<尖括号内的文本 >  | 必须为其提供值的占位符 |
|  [方括号内的文本]  |                可选项                 |
|      {大括号内的文本}       |       一组必需的项;选择一个       |
|         竖线 (          |                       )                       |
|          省略号 (...)           |          可以重复的项           |

返回[菜单](#menu)

## <a name="-dump"></a>-dump

CertUtil [Options] [-dump]

CertUtil [Options] [-dump] 文件

转储配置信息或文件

[-f][-无声][-split][-p Password][-t Timeout]

返回[菜单](#menu)

## <a name="-asn"></a>-asn

CertUtil [Options]-asn 文件 [type]

分析 ASN 1 文件

类型: 数值 dm-crypt\_字符串\_ \*解码类型

返回[菜单](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [Options]-decodehex InFile OutFile [type]

类型: numeric dm-crypt\_字符串\_ \*编码类型

[-f]

返回[菜单](#menu)

## <a name="-decode"></a>-解码

CertUtil [Options]-解码 InFile OutFile

解码 Base64 编码的文件

[-f]

返回[菜单](#menu)

## <a name="-encode"></a>-编码

CertUtil [Options]-编码 InFile OutFile

将文件编码为 Base64

[-f][-UnicodeText]

返回[菜单](#menu)

## <a name="-deny"></a>-deny

CertUtil [Options]-deny RequestId

拒绝挂起的请求

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-resubmit"></a>-重新提交

CertUtil [Options]-重新提交 RequestId

重新提交挂起的请求

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-setattributes"></a>-setattributes

CertUtil [Options]-setattributes RequestId AttributeString

设置挂起请求的属性

RequestId--挂起请求的数字请求 Id

AttributeString-请求属性名称和值对

- 名称和值以冒号分隔。
- 多个名称、值对由换行符分隔。
- 示例: "CertificateTemplate:User\nEMail:User@Domain.com"
- 每个 "\n" 序列都转换为换行符。

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-setextension"></a>-setextension

CertUtil [Options]-setextension RequestId ExtensionName Flags {Long |日期 |String |\@InFile}

设置挂起请求的扩展

RequestId-请求的请求 Id

ExtensionName--extension 的 ObjectId 字符串

标志--建议使用。  1使扩展成为关键扩展, 2 禁用它, 3 同时执行这两项。

如果最后一个参数是数值, 则将其视为一个长整型值。

如果可以将其分析为日期, 则将其作为日期获取。

如果以 "\@" 开头, 则标记的其余部分是包含二进制数据或 ascii 文本十六进制转储的文件名。

其他任何内容都作为字符串使用。

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-revoke"></a>-revoke

CertUtil [Options]-revoke SerialNumber [Reason]

吊销证书

SerialNumber要吊销的证书序列号的逗号分隔列表

原因: 数值或符号吊销原因

- 0CRL_REASON_UNSPECIFIED:未指定 (默认值)
- 1：CRL_REASON_KEY_COMPROMISE:密钥泄漏
- 2：CRL_REASON_CA_COMPROMISE:CA 泄露
- 3：CRL_REASON_AFFILIATION_CHANGED:附属关系改变
- 4:30CRL_REASON_SUPERSEDED:被取代
- 5:40CRL_REASON_CESSATION_OF_OPERATION:操作的哈
- 共CRL_REASON_CERTIFICATE_HOLD:证书保留
- 8CRL_REASON_REMOVE_FROM_CRL:从 CRL 中删除
- -1：解除吊销解除吊销

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-isvalid"></a>-isvalid

CertUtil [Options]-isvalid SerialNumber |CertHash

显示当前证书处置

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-getconfig"></a>-getconfig

CertUtil [Options]-getconfig

获取默认配置字符串

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-ping"></a>-ping

CertUtil [Options]-ping [MaxSecondsToWait |CAMachineList]

Ping Active Directory 证书服务请求接口

CAMachineList--逗号分隔的 CA 计算机名称列表

1. 对于单台计算机, 使用终止逗号
2. 显示每个 CA 计算机的站点成本

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-cainfo"></a>-CAInfo

CertUtil [Options]-CAInfo [InfoName [Index |ErrorCode]]

显示 CA 信息

InfoName-表示要显示的 CA 属性 (见下文)。 对于所有\*属性都使用 ""。

Index--可选的从零开始的属性索引

ErrorCode--数字错误代码

[-f][-split][-config Machine\CAName]

InfoName 参数语法:

- 文件文件版本
- 产品产品版本
- exitcount:退出模块计数
- exit [Index]：退出模块说明
- 政策策略模块说明
- 路径名CA 名称
- sanitizedname:净化的 CA 名称
- dsname净化的 CA 短名称 (DS 名称)
- 共享文件夹共享文件夹
- error1 错误代码:错误消息文本
- error2 错误代码:错误消息文本和错误代码
- 类别CA 类型
- 信息CA 信息
- 上层父 CA
- certcount:CA 证书计数
- xchgcount:CA 交换证书计数
- kracount:KRA 证书计数
- kraused:KRA cert 使用计数
- propidmax:最大 CA PropId
- certstate [Index]：CA 证书
- certversion [Index]：CA 证书版本
- certstatuscode [Index]：CA 证书验证状态
- crlstate [Index]：CRL
- krastate [Index]：KRA 证书
- crossstate + [Index]：前向交叉证书
- crossstate-[Index]：后向交叉证书
- cert [Index]：CA 证书
- certchain [Index]：CA 证书链
- certcrlchain [Index]：带有 Crl 的 CA 证书链
- xchg [Index]：CA 交换证书
- xchgchain [Index]：CA 交换证书链
- xchgcrlchain [Index]：带有 Crl 的 CA 交换证书链
- kra [Index]：KRA 证书
- 叉 + [Index]：前向交叉证书
- 交叉 [索引]：后向交叉证书
- CRL [Index]：基本 CRL
- deltacrl [Index]：增量 CRL
- crlstatus [Index]：CRL 发布状态
- deltacrlstatus [Index]：增量 CRL 发布状态
- dnsDNS 名称
- 职位角色分隔
- 广告高级服务器版
- 模板模板
- csp [Index]：OCSP Url
- aia [Index]：AIA Url
- cdp [Index]：CDP Url
- localenameCA 区域设置名称
- subjecttemplateoids:主题模板 Oid

返回[菜单](#menu)

## <a name="-cacert"></a>-ca. cert

CertUtil [Options]-ca. cert OutCACertFile [Index]

检索 CA 的证书

OutCACertFile: 输出文件

编入CA 证书续订索引 (默认为最新)

[-f][-split][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-cachain"></a>-ca。

CertUtil [Options]-ca OutCACertChainFile [Index]

检索 CA 的证书链

OutCACertChainFile: 输出文件

编入CA 证书续订索引 (默认为最新)

[-f][-split][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [Options]-GetCRL OutFile [Index] [delta]

获取 CRL

编入CRL 索引或密钥索引 (对于最新密钥, 默认为 CRL)

delta: 增量 CRL (默认为基本 CRL)

[-f][-split][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-crl"></a>-CRL

CertUtil [Options]-CRL [dd： hh | 重新发布] [delta]

发布新的 Crl [仅限增量 Crl]

dd: hh-新 CRL 有效期 (以天和小时为单位)

重新发布--重新发布最近的 Crl

增量--仅增量 Crl (默认为基本和增量 Crl)

[-split][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-shutdown"></a>-shutdown

CertUtil [Options]-shutdown

关闭 Active Directory 证书服务

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [Options]-installCert [CACertFile]

安装证书颁发机构证书

[-f][-无声][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [Options]-renewCert [ReuseKeys] [Machine\ParentCAName]

续订证书颁发机构证书

使用-f 忽略未完成的续订请求, 并生成新的请求。

[-f][-无声][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-schema"></a>-架构

CertUtil [Options]-schema [Ext |Attrib |CRL

转储证书架构

默认为请求和证书表

宋体扩展表

恶性属性表

CRLCRL 表

[-split][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-view"></a>-view

CertUtil [Options]-view [Queue |日志 |LogFail |已撤消 |Ext |Attrib |CRL] [csv]

转储证书视图

队列：请求队列

日志：颁发或吊销的证书, 以及失败的请求

LogFail:失败的请求

吊销吊销的证书

宋体扩展表

恶性属性表

CRLCRL 表

.csv作为逗号分隔值输出

显示所有条目的 StatusCode 列:-out StatusCode

显示最后一个条目的所有列:-restrict "RequestId = = $"

显示三个请求的 requestid 和处置:-restrict "requestid > = 37, RequestId\<40"-out "requestid, 处置"

若要显示所有基本 Crl 的行 Id 和 CRL 号, 请执行以下操作:-restrict "CRLMinBase = 0"-out "CRLRowId, CRLNumber" CRL

显示基本 CRL 号 3:-v-restrict "CRLMinBase = 0, CRLNumber = 3"-out "CRLRawCRL" CRL

显示整个 CRL 表:CRL

使用 "Date [+ |-dd： hh]" 作为日期限制

使用 "now + dd: hh" 表示相对于当前时间的日期

[-无声][-split][-config Machine\CAName][-restrict RestrictionList][-out ColumnList]

返回[菜单](#menu)

## <a name="-db"></a>-db

CertUtil [Options]-db

转储原始数据库

[-config Machine\CAName][-restrict RestrictionList][-out ColumnList]

返回[菜单](#menu)

## <a name="-deleterow"></a>-deleterow

CertUtil [Options]-deleterow RowId |Date [Request |证书 |Ext |Attrib |CRL

删除服务器数据库行

需要失败和挂起的请求 (提交日期)

Cert:过期和吊销的证书 (过期日期)

宋体扩展表

恶性属性表

CRLCRL 表 (到期日期)

若要删除失败和由2001年1月22日提交的挂起请求:1/22/2001 请求

若要删除所有已在2001年1月22日过期的证书:1/22/2001 证书

若要删除证书行, 请查看 RequestId 37 的属性和扩展:37

删除由2001年1月22日过期的 Crl:1/22/2001 CRL

[-f][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-backup"></a>-备份

CertUtil [Options]-backup BackupDirectory [增量] [KeepLog]

备份 Active Directory 证书服务

BackupDirectory: 用于存储备份数据的目录

增量: 仅执行增量备份 (默认为完整备份)

KeepLog: 保留数据库日志文件 (默认为截断日志文件)

[-f][-config Machine\CAName][-p Password]

返回[菜单](#menu)

## <a name="-backupdb"></a>-backupDB

CertUtil [Options]-backupDB BackupDirectory [增量] [KeepLog]

备份 Active Directory 证书服务数据库

BackupDirectory: 用于存储备份数据库文件的目录

增量: 仅执行增量备份 (默认为完整备份)

KeepLog: 保留数据库日志文件 (默认为截断日志文件)

[-f][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [Options]-backupKey BackupDirectory

备份 Active Directory 证书服务证书和私钥

BackupDirectory: 用于存储备份的 PFX 文件的目录

[-f][-config Machine\CAName][-p Password][-t Timeout]

返回[菜单](#menu)

## <a name="-restore"></a>-restore

CertUtil [选项]-restore BackupDirectory

还原 Active Directory 证书服务

BackupDirectory: 包含要还原的数据的目录

[-f][-config Machine\CAName][-p Password]

返回[菜单](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [Options]-restoreDB BackupDirectory

还原 Active Directory 证书服务数据库

BackupDirectory: 包含要还原的数据库文件的目录

[-f][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [Options]-restoreKey BackupDirectory |PFXFile

还原 Active Directory 证书服务证书和私钥

BackupDirectory: 包含要还原的 PFX 文件的目录

PFXFile:要还原的 PFX 文件

[-f][-config Machine\CAName][-p Password]

返回[菜单](#menu)

## <a name="-importpfx"></a>-importPFX

CertUtil [Options]-importPFX [CertificateStoreName] PFXFile [修饰符]

导入证书和私钥

CertificateStoreName:证书存储区名称。  请参阅[-store](#-store)。

PFXFile:要导入的 PFX 文件

组成以下一项或多项的逗号分隔列表:

1. AT_SIGNATURE:将 KeySpec 更改为签名
2. AT_KEYEXCHANGE:将 KeySpec 更改为密钥交换
3. NoExport:将私钥设为不可导出
4. NoCert:不导入证书
5. NoChain:不要导入证书链
6. NoRoot:不导入根证书
7. 确保用密码保护密钥
8. NoProtect:不通过密码保护密钥

默认为 "个人计算机存储"。

[-f][-user][-p Password][-csp 提供程序]

返回[菜单](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [Options]-dynamicfilelist

显示动态文件列表

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [Options]-databaselocations

显示数据库位置

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [Options]-hashfile InFile [HashAlgorithm]

生成和显示文件的加密哈希

返回[菜单](#menu)

## <a name="-store"></a>-存储

CertUtil [Options]-store [CertificateStoreName [证书 id [OutputFile]]]

转储证书存储

CertificateStoreName:证书存储区名称。 例如：

- "My"、"CA" (默认)、"Root"、
- "ldap:///CN=Certification 机关, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？" objectClass = 证书颁发机构 "(查看根证书)
- "ldap:///CN=CAName,CN=Certification 机关, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？" objectClass = 证书颁发机构 "(修改根证书)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ certificateRevocationList？ base？ objectClass = cRLDistributionPoint" (查看 Crl)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = 证书颁发机构" (企业 CA 证书)
- ldap(AD 计算机对象证书)
- -用户 ldap:(AD 用户对象证书)

证书 id证书或 CRL 匹配令牌。  这可以是序列号、SHA-1 证书、CRL、CTL 或公钥哈希、数字证书索引 (0、1等等)、数字 CRL 索引 (.0、.1 等等)、数字 CTL 索引 (.)0、。1等)、公钥、签名或扩展 ObjectId、证书使用者公用名、电子邮件地址、UPN 或 DNS 名称、密钥容器名称或 CSP 名称、模板名称或 ObjectId、EKU 或应用程序策略 ObjectId 或 CRL 颁发者公用名。 其中许多项可能会导致多个匹配项。

OutputFile: 保存匹配证书的文件

使用-user 访问用户存储而不是计算机存储。

使用-enterprise 来访问计算机企业应用商店。

使用-service 访问计算机服务存储。

使用-microsoft-windows-grouppolicy 访问计算机组策略存储。

例如：

- -enterprise NTAuth
- -enterprise Root 37
- -user My 26e0aaaf000000000004
- CA 11

[-f][-enterprise][-user][-Microsoft-windows-grouppolicy][-无声][-split][-dc DCName]

返回[菜单](#menu)

## <a name="-addstore"></a>-addstore

CertUtil [Options]-addstore CertificateStoreName InFile

将证书添加到存储

CertificateStoreName:证书存储区名称。  请参阅[-store](#-store)。

InFile要添加到存储区中的证书或 CRL 文件。

[-f][-enterprise][-user][-Microsoft-windows-grouppolicy][-dc DCName]

返回[菜单](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [Options]-delstore CertificateStoreName 证书 id

从存储中删除证书

CertificateStoreName:证书存储区名称。  请参阅[-store](#-store)。

证书 id证书或 CRL 匹配令牌。  请参阅[-store](#-store)。

[-enterprise][-user][-Microsoft-windows-grouppolicy][-dc DCName]

返回[菜单](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [Options]-verifystore CertificateStoreName [证书 id]

验证存储中的证书

CertificateStoreName:证书存储区名称。  请参阅[-store](#-store)。

证书 id证书或 CRL 匹配令牌。  请参阅[-store](#-store)。

[-enterprise][-user][-Microsoft-windows-grouppolicy][-无声][-split][-dc DCName][-t Timeout]

返回[菜单](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [Options]-repairstore CertificateStoreName CertIdList [PropertyInfFile |SDDLSecurityDescriptor]

修复密钥关联或更新证书属性或密钥安全描述符

CertificateStoreName:证书存储区名称。  请参阅[-store](#-store)。

CertIdList: 以逗号分隔的证书或 CRL 匹配令牌列表。 请参阅[-store](#-store)证书 id description。

PropertyInfFile-包含外部属性的 INF 文件:

```
[Properties]
     19 = Empty ; Add archived property, OR:
     19 =       ; Remove archived property

     11 = "{text}Friendly Name" ; Add friendly name property

     127 = "{hex}" ; Add custom hexadecimal property
         _continue_ = "00 01 02 03 04 05 06 07 08 09 0a 0b 0c 0d 0e 0f"
         _continue_ = "10 11 12 13 14 15 16 17 18 19 1a 1b 1c 1d 1e 1f"

     2 = "{text}" ; Add Key Provider Information property
       _continue_ = "Container=Container Name&"
       _continue_ = "Provider=Microsoft Strong Cryptographic Provider&"
       _continue_ = "ProviderType=1&"
       _continue_ = "Flags=0&"
       _continue_ = "KeySpec=2"

     9 = "{text}" ; Add Enhanced Key Usage property
       _continue_ = "1.3.6.1.5.5.7.3.2,"
       _continue_ = "1.3.6.1.5.5.7.3.1,"
```

[-f][-enterprise][-user][-Microsoft-windows-grouppolicy][-无声][-split][-csp 提供程序]

返回[菜单](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [Options]-viewstore [CertificateStoreName [证书 id [OutputFile]]]

转储证书存储

CertificateStoreName:证书存储区名称。 例如：

- "My"、"CA" (默认)、"Root"、
- "ldap:///CN=Certification 机关, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？" objectClass = 证书颁发机构 "(查看根证书)
- "ldap:///CN=CAName,CN=Certification 机关, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？" objectClass = 证书颁发机构 "(修改根证书)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ certificateRevocationList？ base？ objectClass = cRLDistributionPoint" (查看 Crl)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = 证书颁发机构" (企业 CA 证书)
- ldap(AD 计算机对象证书)
- -用户 ldap:(AD 用户对象证书)

证书 id证书或 CRL 匹配令牌。 这可以是序列号、SHA-1 证书、CRL、CTL 或公钥哈希、数字证书索引 (0、1等等)、数字 CRL 索引 (.0、.1 等等)、数字 CTL 索引 (.)0、。1等)、公钥、签名或扩展 ObjectId、证书使用者公用名、电子邮件地址、UPN 或 DNS 名称、密钥容器名称或 CSP 名称、模板名称或 ObjectId、EKU 或应用程序策略 ObjectId 或 CRL 颁发者公用名。 其中许多项可能会导致多个匹配项。

OutputFile: 保存匹配证书的文件

使用-user 访问用户存储而不是计算机存储。

使用-enterprise 来访问计算机企业应用商店。

使用-service 访问计算机服务存储。

使用-microsoft-windows-grouppolicy 访问计算机组策略存储。

例如：

1. -enterprise NTAuth
2. -enterprise Root 37
3. -user My 26e0aaaf000000000004
4. CA 11

[-f][-enterprise][-user][-Microsoft-windows-grouppolicy][-dc DCName]

返回[菜单](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [Options]-viewdelstore [CertificateStoreName [证书 id [OutputFile]]]

从存储中删除证书

CertificateStoreName:证书存储区名称。 例如：

- "My"、"CA" (默认)、"Root"、
- "ldap:///CN=Certification 机关, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？" objectClass = 证书颁发机构 "(查看根证书)
- "ldap:///CN=CAName,CN=Certification 机关, CN = Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？" objectClass = 证书颁发机构 "(修改根证书)
- "ldap:///CN=CAName,CN=MachineName,CN=CDP,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ certificateRevocationList？ base？ objectClass = cRLDistributionPoint" (查看 Crl)
- "ldap:///CN=NTAuthCertificates,CN=Public Key Services, CN = Services, CN = Configuration, DC = cpandl, DC = com？ cACertificate？ base？ objectClass = 证书颁发机构" (企业 CA 证书)
- ldap(AD 计算机对象证书)
- -用户 ldap:(AD 用户对象证书)

证书 id证书或 CRL 匹配令牌。 这可以是序列号、SHA-1 证书、CRL、CTL 或公钥哈希、数字证书索引 (0、1等等)、数字 CRL 索引 (.0、.1 等等)、数字 CTL 索引 (.)0、。1等)、公钥、签名或扩展 ObjectId、证书使用者公用名、电子邮件地址、UPN 或 DNS 名称、密钥容器名称或 CSP 名称、模板名称或 ObjectId、EKU 或应用程序策略 ObjectId 或 CRL 颁发者公用名。 其中许多项可能会导致多个匹配项。

OutputFile: 保存匹配证书的文件

使用-user 访问用户存储而不是计算机存储。

使用-enterprise 来访问计算机企业应用商店。

使用-service 访问计算机服务存储。

使用-microsoft-windows-grouppolicy 访问计算机组策略存储。

例如：

1. -enterprise NTAuth
2. -enterprise Root 37
3. -user My 26e0aaaf000000000004
4. CA 11

[-f][-enterprise][-user][-Microsoft-windows-grouppolicy][-dc DCName]

返回[菜单](#menu)

## <a name="-dspublish"></a>-dsPublish

CertUtil [Options]-dsPublish CertFile [NTAuthCA |Rootca.cer |SubCA |CrossCA |KRA |用户 |设备

CertUtil [Options]-dsPublish CRLFile [DSCDPContainer [DSCDPCN]]

将证书或 CRL 发布到 Active Directory

CertFile: 要发布的证书文件

NTAuthCA:将证书发布到 DS 企业应用商店

Rootca.cer将证书发布到受 DS 信任的根存储

SubCA将 CA 证书发布到 DS CA 对象

CrossCA:将交叉证书发布到 DS CA 对象

KRA将证书发布到 DS 密钥恢复代理对象

用户：将证书发布到用户 DS 对象

设备将证书发布到计算机 DS 对象

CRLFile:要发布的 CRL 文件

DSCDPContainer:DS CDP 容器 CN, 通常是 CA 计算机名称

DSCDPCN:DS CDP 对象 CN, 通常基于净化的 CA 短名称和密钥索引

使用-f 创建 DS 对象。

[-f][-user][-dc DCName]

返回[菜单](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [Options]-ADTemplate [Template]

显示 AD 模板

[-f][-user][-[-mt][-dc DCName]

## <a name="-template"></a>-Template

CertUtil [Options]-Template [模板]

显示注册策略模板

[-f][-user][-无声][-PolicyServer URLOrId][-Anonymous][-Kerberos][-ClientCertificate ClientCertId][-UserName UserName][-p Password]

返回[菜单](#menu)

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [Options]-TemplateCAs 模板

显示模板的 Ca

[-f][-user][-dc DCName]

返回[菜单](#menu)

## <a name="-catemplates"></a>-Catemplates.txt

CertUtil [Options]-Catemplates.txt [Template]

显示 CA 的模板

[-f][-user][-[-mt][-config Machine\CAName][-dc DCName]

返回[菜单](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [Options]-SetCASites [set] [SiteName]

CertUtil [Options]-SetCASites verify [SiteName]

CertUtil [Options]-SetCASites delete

设置、验证或删除 CA 站点名称

- 使用-config 选项来针对单个 CA (默认值为 "所有 Ca")
- 仅当面向单个 CA 时才允许*SiteName*
- 使用-f 替代指定*SiteName*的验证错误
- 使用-f 删除所有 CA 站点名称

[-f][-config Machine\CAName][-dc DCName]

> [!NOTE]
> 有关为 Active Directory 域服务 (AD DS) 站点感知配置 Ca 的详细信息, 请参阅[AD DS AD CS 和 PKI 客户端的站点感知](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx)。

返回[菜单](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [Options]-enrollmentServerURL [URL AuthenticationType [Priority] [修饰符]]

CertUtil [Options]-enrollmentServerURL URL 删除

显示、添加或删除与 CA 关联的注册服务器 Url

AuthenticationType添加 URL 时指定下列客户端身份验证方法之一

1. V5使用 Kerberos SSL 凭据
2. 用户名使用命名帐户作为 SSL 凭据
3. ClientCertificate使用 x.509 证书 SSL 凭据
4. Anonymous：使用匿名 SSL 凭据

删除: 删除与 CA 关联的指定 URL

优先级: 如果在添加 URL 时未指定, 则默认为 "1"

修饰符--以下一个或多个的逗号分隔列表:

1. AllowRenewalsOnly:仅可通过此 URL 将续订请求提交到此 CA
2. AllowKeyBasedRenewal:允许使用在 AD 中没有关联帐户的证书。 这仅适用于 ClientCertificate 和 AllowRenewalsOnly 模式

[-config Machine\CAName][-dc DCName]

返回[菜单](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [Options]-ADCA [CAName]

显示 AD Ca

[-f][-split][-dc DCName]

返回[菜单](#menu)

## <a name="-ca"></a>-CA

CertUtil [Options]-CA [CAName |TemplateName

显示注册策略 Ca

[-f][-user][-无声][-split][-PolicyServer URLOrId][-Anonymous][-Kerberos][-ClientCertificate ClientCertId][-UserName UserName][-p Password]

返回[菜单](#menu)

## <a name="-policy"></a>-Policy

显示注册策略

[-f][-user][-无声][-split][-PolicyServer URLOrId][-Anonymous][-Kerberos][-ClientCertificate ClientCertId][-UserName UserName][-p Password]

返回[菜单](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [Options]-PolicyCache [delete]

显示或删除注册策略缓存条目

删除: 删除策略服务器缓存条目

-f: 使用-f 删除所有缓存条目

[-f][-user][-PolicyServer URLOrId]

返回[菜单](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [Options]-CredStore [URL]

CertUtil [Options]-CredStore URL 添加

CertUtil [Options]-CredStore URL 删除

显示、添加或删除凭据存储区项

URL: 目标 URL。  使用\*匹配所有条目。 使用 https://machine\ * 来匹配 URL 前缀。

添加: 添加凭据存储项。 还必须指定 SSL 凭据。

删除: 删除凭据存储项

-f: 使用-f 覆盖项或删除多个项。

[-f][-user][-无声][-Anonymous][-Kerberos][-ClientCertificate ClientCertId][-UserName UserName][-p Password]

返回[菜单](#menu)

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [Options]-InstallDefaultTemplates

安装默认证书模板

[-dc DCName]

返回[菜单](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [Options]-URLCache [URL |CRL |\* [删除]]

显示或删除 URL 缓存条目

URL: 缓存的 URL

CRL: 仅操作所有缓存的 CRL Url

\*: 操作所有缓存的 Url

删除: 从当前用户的本地缓存中删除相关的 Url

使用-f 强制提取特定 URL 并更新缓存。

[-f][-split]

返回[菜单](#menu)

## <a name="-pulse"></a>-脉冲

CertUtil [Options]-脉冲

脉冲自动注册事件

[-user]

返回[菜单](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [Options]-MachineInfo DomainName\MachineName $

显示 Active Directory 计算机对象信息

返回[菜单](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [Options]-DCInfo [域] [验证 |DeleteBad |DeleteAll

显示域控制器信息

默认值是在不验证的情况下显示 DC 证书

[-f][-user][-urlfetch][-dc DCName][-t Timeout]

> [!TIP]
> 指定 Active Directory 域服务（AD DS）域 **[域]** 并指定在 Windows Server 2012 中添加域控制器（ **-dc**）的功能。 若要成功运行此命令, 你必须使用属于**Domain admins**或**Enterprise admins**成员的帐户。 此命令的行为修改如下:</br>> 1。  如果未指定域并且未指定特定的域控制器, 则此选项将返回要从默认域控制器处理的域控制器的列表。</br>> 2。  如果未指定域, 但指定了域控制器, 则会生成指定域控制器上的证书报表。</br>> 3。  如果指定了域, 但未指定域控制器, 则会在列表中的每个域控制器的证书上生成域控制器的列表。</br>> 4。  如果指定了域和域控制器, 则会从目标域控制器生成域控制器的列表。 还会生成列表中每个域控制器的证书报表。

例如, 假设有一个名为 CPANDL 的域, 其中包含名为 CPANDL-DC1 的域控制器。 可以运行以下命令, 从 CPANDL-DC1: certutil-dc CPANDL-dcinfo CPANDL 检索域控制器及其证书的列表。

返回[菜单](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [Options]-EntInfo DomainName\MachineName $

[-f][-user]

返回[菜单](#menu)

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [Options]-TCAInfo [DomainDN |-]

显示 CA 信息

[-f][-enterprise][-user][-urlfetch][-dc DCName][-t Timeout]

返回[菜单](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [Options]-SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

显示智能卡信息

CRYPT_DELETEKEYSET:删除智能卡上的所有密钥

[-无声][-split][-urlfetch][-t Timeout]

返回[菜单](#menu)

## <a name="-scroots"></a>-SCRoots

CertUtil [Options]-SCRoots update [+] [InputRootFile] [ReaderName]

CertUtil [Options]-SCRoots save \@OutputRootFile [ReaderName]

CertUtil [Options]-SCRoots 视图 [InputRootFile |ReaderName]

CertUtil [Options]-SCRoots delete [ReaderName]

管理智能卡根证书

[-f][-split][-p Password]

返回[菜单](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [Options]-verifykeys [Cspparameters.keycontainername CACertFile]

验证公钥/私钥集

Cspparameters.keycontainername: 要验证的密钥的密钥容器名称。 默认为计算机密钥。  使用用户的用户密钥。

CACertFile: 签名或加密证书文件

如果未指定任何参数, 则将根据其私钥验证每个签名 CA 证书。

只能对本地 CA 或本地密钥执行此操作。

[-f][-user][-无声][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-verify"></a>-验证

CertUtil [Options]-verify CertFile [ApplicationPolicyList |-[IssuancePolicyList]]

CertUtil [Options]-verify CertFile [CACertFile [CrossedCACertFile]]

CertUtil [Options]-验证 CRLFile CACertFile [IssuedCertFile]

CertUtil [Options]-验证 CRLFile CACertFile [DeltaCRLFile]

验证证书、CRL 或链

CertFile要验证的证书

ApplicationPolicyList: 可选的以逗号分隔的所需应用程序策略 ObjectIds 列表

IssuancePolicyList: 可选的以逗号分隔的所需颁发策略 ObjectIds 列表

CACertFile: 要对其进行验证的可选颁发 CA 证书

CrossedCACertFile: 可选证书由 CertFile 交叉认证

CRLFile:要验证的 CRL

IssuedCertFile: CRLFile 涵盖的可选颁发证书

DeltaCRLFile: 可选的增量 CRL

如果指定了 ApplicationPolicyList, 则链生成限制为指定应用程序策略的有效链。

如果指定了 IssuancePolicyList, 则链生成限制为指定的发布策略的有效链。

如果指定了 CACertFile, 则会对照 CertFile 或 CRLFile 来验证 CACertFile 中的字段。

如果未指定 CACertFile, 则使用 CertFile 生成并验证完整的链。

如果同时指定了 CACertFile 和 CrossedCACertFile, 则会对照 CertFile 验证 CACertFile 和 CrossedCACertFile 中的字段。

如果指定了 IssuedCertFile, 则 IssuedCertFile 中的字段将根据 CRLFile 进行验证。

如果指定了 DeltaCRLFile, 则 DeltaCRLFile 中的字段将根据 CRLFile 进行验证。

[-f][-enterprise][-user][-无声][-split][-urlfetch][-t Timeout]

返回[菜单](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [Options]-verifyCTL CTLObject [CertDir] [CertFile]

验证 AuthRoot 或不允许的证书 CTL

CTLObject:标识要验证的 CTL:

- AuthRootWU: 从 URL 缓存读取 AuthRoot CAB 和匹配的证书。 改为使用-f 从 Windows 更新下载。
- DisallowedWU: 读取不允许的证书 CAB, 并从 URL 缓存禁用证书存储区文件。  改为使用-f 从 Windows 更新下载。
- AuthRoot: 读取注册表缓存的 AuthRoot CTL。  使用 with-f 和不受信任的 CertFile 来强制更新注册表缓存的 AuthRoot 和不允许的证书 Ctl。
- 不允许: 读取注册表缓存不允许的证书 CTL。 -f 与 AuthRoot 的行为相同。
- CTLFileName: file 或 http: CTL 或 CAB 的路径

CertDir: 包含与 CTL 条目匹配的证书的文件夹。 Http: 文件夹路径必须以路径分隔符结尾。 如果未使用 AuthRoot 指定文件夹, 则将在多个位置中搜索匹配的证书: 本地证书存储、crypt32.dll 资源和本地 URL 缓存。 必要时, 请使用-f 从 Windows 更新下载。 否则, 默认为与 CTLObject 相同的文件夹或网站。

CertFile: 包含要验证的证书的文件。 证书将与 CTL 条目匹配, 并显示匹配结果。 禁止显示大多数默认输出。

[-f][-user][-split]

返回[菜单](#menu)

## <a name="-sign"></a>-sign

CertUtil [Options]-sign InFileList |SerialNumber |CRL OutFileList [开始日期 + dd： hh] [+ SerialNumberList |-SerialNumberList |-ObjectIdList | \@ExtensionFile]

CertUtil [Options]-sign InFileList |SerialNumber |CRL OutFileList [#HashAlgorithm] [+ 内容: alternatesignaturealgorithm |-内容: alternatesignaturealgorithm]

重新签署 CRL 或证书

InFileList: 逗号分隔的证书或 CRL 文件列表, 用于修改和重新签名

SerialNumber要创建的证书的序列号。 有效期和其他选项不得存在。

CRL创建一个空 CRL。 有效期和其他选项不得存在。

OutFileList: 以逗号分隔的已修改证书或 CRL 输出文件的列表。 文件数量必须与 InFileList 匹配。

开始日期 + dd: hh: 新的有效期: 可选的日期加上;可选日期和小时有效期;如果同时指定两者, 则使用加号 (+) 分隔符。 使用 "now [+ dd： hh]" 从当前时间开始。 使用 "从不" 无到期日期 (仅适用于 Crl)。

SerialNumberList: 要添加或删除的以逗号分隔的序列号列表

ObjectIdList: 要删除的以逗号分隔的扩展 ObjectId 列表

\@ExtensionFile:INF 文件包含要更新或删除的扩展:

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm前面带有 # 号的哈希算法的名称

内容: alternatesignaturealgorithm: 备用签名算法说明符

减号将导致序列号和扩展被删除。 加号将导致序列号添加到 CRL。 从 CRL 中删除项时, 列表可能同时包含序列号和 ObjectIds。 内容: alternatesignaturealgorithm 之前的减号导致使用旧签名格式。 内容: alternatesignaturealgorithm 之前的加号会导致使用 alternature 签名格式。 如果未指定内容: alternatesignaturealgorithm, 则使用证书或 CRL 中的签名格式。

[-nullsign][-f][-无声][-Cert 证书 id]

返回[菜单](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [Options]-vroot [delete]

创建/删除 web 虚拟根和文件共享

返回[菜单](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [Options]-vocsproot [delete]

为 OCSP web 代理创建/删除 web 虚拟根

返回[菜单](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [Options]-addEnrollmentServer Kerberos |用户名 |ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

添加注册服务器应用程序

为指定的 CA 添加注册服务器应用程序和应用程序池 (如有必要)。 此命令不安装二进制文件或包。 与客户端连接到证书注册服务器的下列身份验证方法之一。

- V5使用 Kerberos SSL 凭据
- 用户名使用命名帐户作为 SSL 凭据
- ClientCertificate使用 x.509 证书 SSL 凭据
- AllowRenewalsOnly:仅可通过此 URL 将续订请求提交到此 CA
- AllowKeyBasedRenewal--允许使用在 AD 中没有关联帐户的证书。 这仅适用于 ClientCertificate 和 AllowRenewalsOnly 模式。

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [Options]-deleteEnrollmentServer Kerberos |用户名 |ClientCertificate

删除注册服务器应用程序

为指定的 CA 删除注册服务器应用程序和应用程序池 (如有必要)。 此命令不删除二进制文件或包。 与客户端连接到证书注册服务器的下列身份验证方法之一。

1. V5使用 Kerberos SSL 凭据
2. 用户名使用命名帐户作为 SSL 凭据
3. ClientCertificate使用 x.509 证书 SSL 凭据

[-config Machine\CAName]

返回[菜单](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [Options]-addPolicyServer Kerberos |用户名 |ClientCertificate [KeyBasedRenewal]

添加策略服务器应用程序

如有必要, 请添加策略服务器应用程序和应用程序池。 此命令不安装二进制文件或包。 与客户端连接到证书策略服务器的下列身份验证方法之一:

- V5使用 Kerberos SSL 凭据
- 用户名使用命名帐户作为 SSL 凭据
- ClientCertificate使用 x.509 证书 SSL 凭据
- KeyBasedRenewal:仅将包含 KeyBasedRenewal 模板的策略返回到客户端。 此标志仅适用于 UserName 和 ClientCertificate authentication。

返回[菜单](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [Options]-deletePolicyServer Kerberos |用户名 |ClientCertificate [KeyBasedRenewal]

删除策略服务器应用程序

如有必要, 请删除策略服务器应用程序和应用程序池。 此命令不删除二进制文件或包。 与客户端连接到证书策略服务器的下列身份验证方法之一:

1. V5使用 Kerberos SSL 凭据
2. 用户名使用命名帐户作为 SSL 凭据
3. ClientCertificate使用 x.509 证书 SSL 凭据
4. KeyBasedRenewal:KeyBasedRenewal 策略服务器

返回[菜单](#menu)

## <a name="-oid"></a>-oid

CertUtil [Options]-oid ObjectId [DisplayName | delete [LanguageId [Type]]]

CertUtil [Options]-oid GroupId

CertUtil [Options]-oid AlgId |AlgorithmName [GroupId]

显示 ObjectId 或设置显示名称

- ObjectId--要显示或添加显示名称的 ObjectId
- GroupId--要枚举的 ObjectIds 的十进制 GroupId 编号
- AlgId--要查找的 ObjectId 的十六进制 AlgId
- AlgorithmName-要查找的 ObjectId 的算法名称
- DisplayName-要存储在 DS 中的显示名称
- 删除--删除显示名称
- LanguageId--Language Id (默认为当前值:1033)
- Type-要创建的 DS 对象类型:1用于模板 (默认值), 2 表示颁发策略, 3 表示应用程序策略
- 使用-f 创建 DS 对象。

[-f]

返回[菜单](#menu)

## <a name="-error"></a>-错误

CertUtil [Options]-错误 ErrorCode

显示错误代码消息文本

返回[菜单](#menu)

## <a name="-getreg"></a>-getreg

CertUtil [Options]-getreg [{ca | 还原 | 策略 | 退出 | 模板 | 注册 | 链 |PolicyServers} \[ProgId @ no__t-1] [RegistryValueName]

显示注册表值

认证使用 CA 的注册表项

restore使用 CA 的还原注册表项

政策使用策略模块的注册表项

离开使用第一个退出模块的注册表项

模版使用模板注册表项 (使用用户模板的用户)

参加使用注册注册表项 (将用户用于用户上下文)

scsi使用链配置注册表项

PolicyServers:使用策略服务器注册表项

ProgId使用策略或退出模块的 ProgId (注册表子项名称)

RegistryValueName: 注册表值名称 (使用 "名称\*" 与前缀匹配)

值: 新的数字、字符串或日期注册表值或文件名。 如果数字值以 "+" 或 "-" 开头, 则在现有注册表值中设置或清除在新值中指定的位。

如果字符串值以 "+" 或 "-" 开头，并且现有值为 REG_MULTI_SZ 值，则会将该字符串添加到现有的注册表值或从中删除。 若要强制创建 REG_MULTI_SZ 值，请将 "\n" 添加到字符串值的末尾。

如果值以 "\@" 开头, 则该值的其余部分是包含二进制值的十六进制文本表示形式的文件的名称。 如果未引用有效的文件，则会将其视为 [Date] [+ |-] [dd： hh]--可选的日期加上或减去可选的日和小时。 如果同时指定两者, 则使用加号 (+) 或减号 (-) 分隔符。 使用 "now + dd: hh" 表示相对于当前时间的日期。

使用 "chain\ChainCacheResyncFiletime \@now" 有效地刷新缓存的 crl。

[-f][-user][-Microsoft-windows-grouppolicy][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-setreg"></a>-setreg

CertUtil [Options]-setreg [{ca | 还原 | 策略 | 退出 | 模板 | 注册 | 链 |PolicyServers} \[ProgId @ no__t-1] RegistryValueName Value

设置注册表值

认证使用 CA 的注册表项

restore使用 CA 的还原注册表项

政策使用策略模块的注册表项

离开使用第一个退出模块的注册表项

模版使用模板注册表项 (使用用户模板的用户)

参加使用注册注册表项 (将用户用于用户上下文)

scsi使用链配置注册表项

PolicyServers:使用策略服务器注册表项

ProgId使用策略或退出模块的 ProgId (注册表子项名称)

RegistryValueName: 注册表值名称 (使用 "名称\*" 与前缀匹配)

值: 新的数字、字符串或日期注册表值或文件名。 如果数字值以 "+" 或 "-" 开头, 则在现有注册表值中设置或清除在新值中指定的位。

如果字符串值以 "+" 或 "-" 开头，并且现有值为 REG_MULTI_SZ 值，则会将该字符串添加到现有的注册表值或从中删除。 若要强制创建 REG_MULTI_SZ 值，请将 "\n" 添加到字符串值的末尾。

如果值以 "\@" 开头, 则该值的其余部分是包含二进制值的十六进制文本表示形式的文件的名称。 如果未引用有效的文件，则会将其视为 [Date] [+ |-] [dd： hh]--可选的日期加上或减去可选的日和小时。 如果同时指定两者, 则使用加号 (+) 或减号 (-) 分隔符。 使用 "now + dd: hh" 表示相对于当前时间的日期。

使用 "chain\ChainCacheResyncFiletime \@now" 有效地刷新缓存的 crl。

[-f][-user][-Microsoft-windows-grouppolicy][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-delreg"></a>-delreg

CertUtil [Options]-delreg [{ca | 还原 | 策略 | 退出 | 模板 | 注册 | 链 |PolicyServers} \[ProgId @ no__t-1] [RegistryValueName]

删除注册表值

认证使用 CA 的注册表项

restore使用 CA 的还原注册表项

政策使用策略模块的注册表项

离开使用第一个退出模块的注册表项

模版使用模板注册表项 (使用用户模板的用户)

参加使用注册注册表项 (将用户用于用户上下文)

scsi使用链配置注册表项

PolicyServers:使用策略服务器注册表项

ProgId使用策略或退出模块的 ProgId (注册表子项名称)

RegistryValueName: 注册表值名称 (使用 "名称\*" 与前缀匹配)

值: 新的数字、字符串或日期注册表值或文件名。 如果数字值以 "+" 或 "-" 开头, 则在现有注册表值中设置或清除在新值中指定的位。

如果字符串值以 "+" 或 "-" 开头，并且现有值为 REG_MULTI_SZ 值，则会将该字符串添加到现有的注册表值或从中删除。 若要强制创建 REG_MULTI_SZ 值，请将 "\n" 添加到字符串值的末尾。

如果值以 "\@" 开头, 则该值的其余部分是包含二进制值的十六进制文本表示形式的文件的名称。 如果未引用有效的文件，则会将其视为 [Date] [+ |-] [dd： hh]--可选的日期加上或减去可选的日和小时。 如果同时指定两者, 则使用加号 (+) 或减号 (-) 分隔符。 使用 "now + dd: hh" 表示相对于当前时间的日期。

使用 "chain\ChainCacheResyncFiletime \@now" 有效地刷新缓存的 crl。

[-f][-user][-Microsoft-windows-grouppolicy][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [Options]-ImportKMS UserKeyAndCertFile [证书 id]

将用户密钥和证书导入到服务器数据库以进行密钥存档

UserKeyAndCertFile-包含要存档的用户私钥和证书的数据文件。  这可以是以下任一项:

- Exchange 密钥管理服务器 (KMS) 导出文件
- PFX 文件

证书 idKMS 导出文件解密证书匹配标记。  请参阅[-store](#-store)。

使用-f 导入不由 CA 颁发的证书。

[-f][-无声][-split][-config Machine\CAName][-p Password][-symkeyalg SymmetricKeyAlgorithm [，KeyLength]]

返回[菜单](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [Options]-ImportCert Certfile [ExistingRow]

将证书文件导入到数据库中

使用 ExistingRow 导入证书, 以代替对同一密钥的挂起的请求。

使用-f 导入不由 CA 颁发的证书。

还可能需要将 CA 配置为支持外接证书导入： certutil-setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f][-config Machine\CAName]

返回[菜单](#menu)

## <a name="-getkey"></a>-GetKey

CertUtil [Options]-GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [Options]-GetKey SearchToken script OutputScriptFile

CertUtil [Options]-GetKey SearchToken 检索 |恢复 OutputFileBaseName

检索存档的私钥恢复 blob、生成恢复脚本或恢复存档的密钥

脚本: 生成用于检索和恢复密钥的脚本 (如果找到多个匹配的恢复候选项, 则为默认行为; 如果未指定输出文件, 则为默认行为)。

检索: 检索一个或多个密钥恢复 Blob (如果只找到一个匹配的恢复候选项, 则检索默认行为; 如果指定了输出文件)

recover: 在一个步骤中检索和恢复私钥 (需要密钥恢复代理证书和私钥)

SearchToken:用于选择要恢复的密钥和证书。

可以是以下任一项:

1. 证书公用名
2. 证书序列号
3. 证书 SHA-1 哈希 (指纹)
4. 证书 KeyId SHA-1 哈希 (使用者密钥标识符)
5. 申请人姓名 (域 \ 用户)
6. UPN (用户\@域)

RecoveryBlobOutFile: 输出文件包含一个或多个密钥恢复代理证书, 它仍加密为一个证书链和一个关联的私钥。

OutputScriptFile: 输出文件, 其中包含用于检索和恢复私钥的批处理脚本。

OutputFileBaseName: 输出文件基名称。 对于检索, 将截断任何扩展, 并为每个密钥恢复 blob 追加特定于证书的字符串和扩展名。  每个文件都包含一个证书链和一个关联的私钥, 仍加密为一个或多个密钥恢复代理证书。 对于恢复, 将截断任何扩展, 并追加 p12 扩展名。  包含已恢复的证书链和关联的私钥, 作为 PFX 文件存储。

[-f][-UnicodeText][-无声][-config Machine\CAName][-p Password][-ProtectTo SAMNameAndSIDList][-csp 提供程序]

返回[菜单](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [Options]-RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

恢复存档的私钥

[-f][-user][-无声][-split][-p Password][-ProtectTo SAMNameAndSIDList][-csp 提供程序][-t Timeout]

返回[菜单](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [Options]-MergePFX PFXInFileList PFXOutFile [ExtendedProperties]

PFXInFileList:逗号分隔的 PFX 输入文件列表

PFXOutFile:PFX 输出文件

ExtendedProperties包括扩展属性

在命令行中指定的密码是以逗号分隔的密码列表。  如果指定了多个密码, 则将最后一个密码用于输出文件。  如果只提供了一个密码或最后一个密码为 "\*", 则系统会提示用户输入输出文件密码。

[-f][-user][-split][-p Password][-ProtectTo SAMNameAndSIDList][-csp 提供程序]

返回[菜单](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [Options]-ConvertEPF PFXInFileList EPFOutFile [cast | cast] [V3CACertId] [，Salt]

将 PFX 文件转换为 EPF 文件

PFXInFileList:逗号分隔的 PFX 输入文件列表

EPF:EPF 输出文件

计使用强制转换64加密

cast-:使用强制转换64加密 (导出)

V3CACertId:V3 CA 证书匹配标记。  请参阅[-store](#-store)证书 id description。

SaltEPF 输出文件 salt 字符串

在命令行中指定的密码是以逗号分隔的密码列表。 如果指定了多个密码, 则将最后一个密码用于输出文件。  如果只提供了一个密码或最后一个密码为 "\*", 则系统会提示用户输入输出文件密码。

[-f][-无声][-split][-dc DCName][-p Password][-csp 提供程序]

返回[菜单](#menu)

## <a name="options"></a>选项

本部分定义可通过命令指定的选项。

|选项|描述|
|-------|-----------|
|-nullsign|使用数据哈希作为签名|
|-f|强制覆盖|
|-enterprise|使用本地计算机企业注册表证书存储|
|-user|使用 HKEY_CURRENT_USER 密钥或证书存储|
|-Microsoft-windows-grouppolicy|使用组策略证书存储|
|-未|显示用户模板|
|-mt|显示计算机模板|
|-Unicode|以 Unicode 编写重定向的输出|
|-UnicodeText|用 Unicode 写入输出文件|
|-gmt|将时间显示为 GMT|
|-秒|显示时间 (以秒和毫秒为单位)|
|-无提示|使用无提示标志获取 dm-crypt 上下文|
|-split|拆分嵌入的 node.js 元素, 并保存到文件|
|-v|详细操作|
|-privatekey.ppk|显示密码和私钥数据|
|-pin PIN|智能卡 PIN|
|-urlfetch|检索并验证 AIA 证书和 CDP Crl|
|-config Machine\CAName|CA 和计算机名字符串|
|-PolicyServer URLOrId|策略服务器 URL 或 Id。对于选择 U/I, 请使用-PolicyServer。 对于所有策略服务器, 请使用-PolicyServer\*|
|-Anonymous|使用匿名 SSL 凭据|
|-Kerberos|使用 Kerberos SSL 凭据|
|-ClientCertificate ClientCertId|使用 x.509 证书 SSL 凭据。 对于选择 U/I, 请使用-clientCertificate。|
|-用户名用户名|使用命名帐户作为 SSL 凭据。 对于选择 U/I, 请使用-UserName。|
|-Cert 证书 id|签名证书|
|-dc DCName|面向特定域控制器|
|-限制 RestrictionList|逗号分隔的限制列表。 每个限制都包含列名称、关系运算符和常量整数、字符串或日期。 一个列名前面可能有一个加号或减号, 用来指示排序顺序。 例如：</br>"RequestId = 47"</br>"+ RequesterName > = a, RequesterName < b"</br>"-RequesterName > 域, 处置 = 21"|
|-out ColumnList|逗号分隔的列列表|
|-p 密码|密码|
|-ProtectTo SAMNameAndSIDList|逗号分隔 SAM 名称/SID 列表|
|-csp 提供程序|提供程序|
|-t 超时|URL 提取超时值 (毫秒)|
|-symkeyalg SymmetricKeyAlgorithm [，KeyLength]|具有可选密钥长度的对称密钥算法的名称, 示例:AES、128或3DES|

返回[菜单](#menu)

## <a name="additional-certutil-examples"></a>其他 certutil 示例

有关如何使用此命令的一些示例, 请参阅

1. [用于从命令行管理 Active Directory 证书服务 (AD CS) 的 Certutil 示例](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [用于管理证书的 Certutil 任务](https://technet.microsoft.com/library/cc772898.aspx)
3. [使用 CertUtil 命令行工具演练进行二进制请求导出](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [根 CA 证书续订](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [Certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

返回[菜单](#menu)
