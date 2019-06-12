---
title: certutil
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 7a602472d30f19cb2d4a802423635e5788e78a43
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434550"
---
# <a name="certutil"></a>certutil

Certutil.exe 是一个命令行程序，证书服务的一部分安装。 可以使用 Certutil.exe 来转储并显示证书颁发机构 (CA) 配置信息，请配置证书服务备份和还原 CA 组件，并验证证书、 密钥对和证书链。

在不使用其他参数的证书颁发机构上运行 certutil 时，它显示当前的证书颁发机构配置。 Cerutil 非证书颁发机构上运行时，该命令默认为运行 certutil [-转储](#-dump)谓词。

> [!WARNING]
> Certutil 的早期版本可能不会提供所有本文档中描述的选项。 您可以查看所有 certutil 的特定版本提供了通过运行中所示的命令的选项[语法表示法](#syntax-notations)部分。

## <a name="menu"></a>菜单

下面是本文档中的主要部分：

- [Verbs](#verbs)
- [语法表示法](#syntax-notations)
- [选项](#options)
- [其他 certutil 示例](#additional-certutil-examples)

## <a name="verbs"></a>谓词

下表介绍可以使用 certutil 命令使用的谓词。

|谓词|描述|
|-----|-----------|
|[-dump](#-dump)|转储的配置信息或文件|
|[-asn](#-asn)|分析 ASN.1 文件|
|[-decodehex](#-decodehex)|解码十六进制编码的文件|
|[-decode](#-decode)|解码的 Base64 编码文件|
|[-encode](#-encode)|文件编码为 Base64|
|[-deny](#-deny)|拒绝挂起的证书申请|
|[-resubmit](#-resubmit)|重新提交挂起的证书申请|
|[-setattributes](#-setattributes)|为挂起的证书申请设置属性|
|[-setextension](#-setextension)|设置挂起的证书请求的扩展|
|[-revoke](#-revoke)|吊销证书|
|[-isvalid](#-isvalid)|显示当前证书的处理设置|
|[-getconfig](#-getconfig)|获取默认配置字符串|
|[-ping](#-ping)|尝试联系 Active Directory 证书服务请求的接口|
|-pingadmin|尝试联系 Active Directory 证书服务管理界面|
|[-CAInfo](#-cainfo)|显示有关证书颁发机构的信息|
|[-ca.cert](#-cacert)|检索证书颁发机构的证书|
|[-ca.chain](#-cachain)|检索证书颁发机构的证书链|
|[-GetCRL](#-getcrl)|获取证书吊销列表 (CRL)|
|[-CRL](#-crl)|发布新的证书吊销列表 (Crl) [或仅增量 Crl]|
|[-shutdown](#-shutdown)|关闭 Active Directory 证书服务|
|[-installCert](#-installcert)|安装证书颁发机构证书|
|[-renewCert](#-renewcert)|续订证书颁发机构证书|
|[-schema](#-schema)|转储证书架构|
|[-view](#-view)|转储证书视图|
|[-db](#-db)|转储原始数据库|
|[-deleterow](#-deleterow)|从服务器数据库中删除行|
|[-backup](#-backup)|备份 Active Directory 证书服务|
|[-backupDB](#-backupdb)|备份 Active Directory 证书服务数据库|
|[-backupKey](#-backupkey)|备份 Active Directory 证书服务证书和私钥|
|[-restore](#-restore)|还原 Active Directory 证书服务|
|[-restoreDB](#-restoredb)|还原 Active Directory 证书服务数据库|
|[-restoreKey](#-restorekey)|还原 Active Directory 证书服务证书和私钥|
|[-importPFX](#-importpfx)|导入证书和私钥|
|[-dynamicfilelist](#-dynamicfilelist)|显示动态文件列表|
|[-databaselocations](#-databaselocations)|显示数据库位置|
|[-hashfile](#-hashfile)|生成并通过 file 显示加密哈希|
|[-store](#-store)|转储的证书存储区|
|[-addstore](#-addstore)|将证书添加到应用商店|
|[-delstore](#-delstore)|从存储中删除证书|
|[-verifystore](#-verifystore)|验证存储中的证书|
|[-repairstore](#-repairstore)|修复键关联或更新证书属性或重要的安全描述符|
|[-viewstore](#-viewstore)|转储的证书存储|
|[-viewdelstore](#-viewdelstore)|从存储中删除证书|
|[-dsPublish](#-dspublish)|将证书或证书吊销列表 (CRL) 发布到 Active Directory|
|[-ADTemplate](#-adtemplate)|显示 AD 模板|
|[-Template](#-template)|显示证书模板|
|[-TemplateCAs](#-templatecas)|显示证书模板的证书颁发机构 (Ca)|
|[-CATemplates](#-catemplates)|CA 的显示模板|
|[-SetCASites](#-setcasites)|管理 Ca 的站点名称|
|[-enrollmentServerURL](#-enrollmentserverurl)|显示、 添加或删除与 CA 相关联的注册服务器 Url|
|[-ADCA](#-adca)|显示 AD Ca|
|[-CA](#-ca)|显示注册策略 Ca|
|[-Policy](#-policy)|显示注册策略|
|[-PolicyCache](#-policycache)|显示或删除注册策略缓存条目|
|[-CredStore](#-credstore)|显示、 添加或删除凭据存储区条目|
|[-InstallDefaultTemplates](#-installdefaulttemplates)|安装默认的证书模板|
|[-URLCache](#-urlcache)|显示或删除 URL 缓存条目|
|[-pulse](#-pulse)|脉冲自动注册事件|
|[-MachineInfo](#-machineinfo)|显示有关 Active Directory 计算机对象的信息|
|[-DCInfo](#-dcinfo)|显示有关域控制器的信息|
|[-EntInfo](#-entinfo)|显示有关企业 CA 的信息|
|[-TCAInfo](#-tcainfo)|显示关于 CA 的信息|
|[-SCInfo](#-scinfo)|显示有关智能卡信息|
|[-SCRoots](#-scroots)|管理智能卡的根证书|
|[-verifykeys](#-verifykeys)|验证公共或专用的密钥集|
|[-verify](#-verify)|验证证书、 证书吊销列表 (CRL) 或证书链|
|[-verifyCTL](#-verifyctl)|验证 AuthRoot 或不允许的证书的 CTL|
|[-sign](#-sign)|重新签名的证书吊销列表 (CRL) 或证书|
|[-vroot](#-vroot)|创建或删除 web 虚拟根和文件共享|
|[-vocsproot](#-vocsproot)|创建或删除 web 虚拟根 OCSP web 代理|
|[-addEnrollmentServer](#-addenrollmentserver)|添加注册服务器应用程序|
|[-deleteEnrollmentServer](#-deleteenrollmentserver)|删除注册服务器应用程序|
|[-addPolicyServer](#-addpolicyserver)|添加策略服务器应用程序|
|[-deletePolicyServer](#-deletepolicyserver)|删除策略服务器应用程序|
|[-oid](#-oid)|显示的对象标识符或设置显示名称|
|[-error](#-error)|显示与错误代码关联的消息文本|
|[-getreg](#-getreg)|显示的注册表值|
|[-setreg](#-setreg)|设置注册表值|
|[-delreg](#-delreg)|删除注册表值|
|[-ImportKMS](#-importkms)|用户密钥和证书导入密钥存档服务器数据库|
|[-ImportCert](#-importcert)|证书文件导入数据库|
|[-GetKey](#-getkey)|检索已存档私钥恢复 blob|
|[-RecoverKey](#-recoverkey)|恢复已存档的私钥|
|[-MergePFX](#-mergepfx)|合并 PFX 文件|
|[-ConvertEPF](#-convertepf)|将 PFX 文件转换为 EPF 文件|
|-?|显示谓词列表|
|- *\<verb>* -?|显示指定的谓词的帮助。|
|-? -v|显示谓词的完整列表和|

返回到[菜单](#menu)

## <a name="syntax-notations"></a>语法表示法

- 有关基本命令行语法，运行 `certutil -?`
- 有关与特定谓词使用 certutil 语法，运行**certutil** *\<谓词 >* **-？**
- 若要将所有的 certutil 语法发送到文本文件中，运行以下命令：  
  - `certutil -v -? > certutilhelp.txt`
  - `notepad certutilhelp.txt`

下表介绍用来指示命令行语法表示法。


|            表示法             |                  描述                  |
|---------------------------------|-----------------------------------------------|
| 不带括号或大括号的文本 |         必须键入如下所示的项          |
|  \<在尖括号内的文本 >  | 必须为其提供一个值的占位符 |
|  [方括号内的文本]  |                可选项                 |
|      {大括号内的文本}       |       组的所需的项目;选择一个       |
|         竖线 （          |                       )                       |
|          省略号 （...）           |          可以重复的项           |

返回到[菜单](#menu)

## <a name="-dump"></a>-dump

CertUtil [Options] [-dump]

CertUtil [选项] [-转储] 文件

转储的配置信息或文件

[-f] [-silent] [-split] [-p Password] [-t Timeout]

返回到[菜单](#menu)

## <a name="-asn"></a>-asn

CertUtil [选项]-asn 文件 [类型]

分析 ASN.1 文件

类型： 数值 CRYPT\_字符串\_\*解码类型

返回到[菜单](#menu)

## <a name="-decodehex"></a>-decodehex

CertUtil [选项] decodehex InFile OutFile [类型]

类型： 数值 CRYPT\_字符串\_\*编码类型

[-f]

返回到[菜单](#menu)

## <a name="-decode"></a>-decode

CertUtil [Options] -decode InFile OutFile

解码 Base64 编码的文件

[-f]

返回到[菜单](#menu)

## <a name="-encode"></a>-encode

CertUtil [Options] -encode InFile OutFile

文件编码为 Base64

[-f] [-UnicodeText]

返回到[菜单](#menu)

## <a name="-deny"></a>-拒绝

CertUtil [选项]-拒绝请求 Id

拒绝挂起的请求

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-resubmit"></a>-resubmit

CertUtil [选项]-重新提交请求 Id

重新提交挂起的请求

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-setattributes"></a>-setattributes

CertUtil [Options] -setattributes RequestId AttributeString

为挂起的请求的设置属性

RequestId-数字请求 Id 的挂起的请求

AttributeString-请求特性名称和值对

- 名称和值是分号分隔。
- 多个名称值对在不同的行。
- 示例:"CertificateTemplate:User\nEMail:User@Domain.com"
- 每个"\n"序列转换为换行分隔符。

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-setextension"></a>-setextension

CertUtil [选项] setextension RequestId ExtensionName 标记 {长 |日期 |字符串 |@InFile}

为挂起的请求集扩展

挂起的请求的请求 Id-数字的请求 Id

该扩展的字符串，ExtensionName-ObjectId

建议标志--0。  1 标识扩展是关键，2 禁用它，3 两者都执行。

如果最后一个参数是数值，它被视为长时间。

如果它可以解析为日期，则它会为日期。

如果它以 @，该令牌的其余部分由包含二进制数据或 ascii 文本十六进制转储的文件名。

任何其他内容被视为字符串。

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-revoke"></a>-revoke

CertUtil [Options] -revoke SerialNumber [Reason]

吊销证书

序列号：若要吊销的证书序列号的逗号分隔列表

原因： 数字或符号吊销原因

- 0:CRL_REASON_UNSPECIFIED:未指定 （默认值）
- 1：CRL_REASON_KEY_COMPROMISE:密钥泄漏
- 2：CRL_REASON_CA_COMPROMISE:CA 泄漏
- 3：CRL_REASON_AFFILIATION_CHANGED:从属关系已改变
- 4:CRL_REASON_SUPERSEDED:被取代
- 5:CRL_REASON_CESSATION_OF_OPERATION:停止操作
- 6:CRL_REASON_CERTIFICATE_HOLD:证书挂起
- 8:CRL_REASON_REMOVE_FROM_CRL:从 CRL 中删除
- -1：解除吊销：解除吊销

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-isvalid"></a>-isvalid

CertUtil [Options] -isvalid SerialNumber | CertHash

显示当前证书部署

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-getconfig"></a>-getconfig

CertUtil [Options] -getconfig

获取默认配置字符串

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-ping"></a>-ping

CertUtil [Options] -ping [MaxSecondsToWait | CAMachineList]

Ping Active Directory 证书服务请求的接口

CAMachineList-以逗号分隔的 CA 计算机名称列表

1. 对于单台计算机，请使用终止逗号
2. 显示每个 CA 计算机的站点成本

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-cainfo"></a>-CAInfo

CertUtil [Options] -CAInfo [InfoName [Index | ErrorCode]]

显示 CA 信息

InfoName-指示 CA 属性来显示 （见下文）。 使用"\*"为所有属性。

索引--从零开始的可选属性索引

错误代码-数字错误代码

[-f] [-split] [-config Machine\CAName]

InfoName 参数语法：

- 文件：文件版本
- 产品：产品版本
- exitcount:退出模块计数
- 退出 [Index]:退出模块说明
- 策略：策略模块说明
- 名称：CA 名称
- sanitizedname:净化的 CA 名称
- dsname:净化的 CA 短名称 （DS 名称）
- 共享文件夹：共享的文件夹
- error1 错误代码：错误消息文本
- error2 错误代码：错误消息文本和错误代码
- 类型：CA 类型
- 信息：CA 信息
- 父级：父 CA
- certcount:CA 证书计数
- xchgcount:CA 交换证书计数
- kracount:KRA 证书计数
- kraused:KRA 证书使用计数
- propidmax:Maximum CA PropId
- certstate [Index]:CA 证书
- certversion [Index]:CA 证书版本
- certstatuscode [Index]:CA 证书验证状态
- crlstate [Index]:CRL
- krastate [Index]:KRA 证书
- crossstate + [Index]:前向交叉证书
- crossstate-[Index]:后向交叉证书
- 证书 [Index]:CA 证书
- certchain [Index]:CA 证书链
- certcrlchain [Index]:通过 Crl 的 CA 证书链
- xchg [Index]:CA 交换证书
- xchgchain [Index]:CA 交换证书链
- xchgcrlchain [Index]:通过 Crl 的 CA exchange 证书链
- kra [Index]:KRA 证书
- 跨 + [Index]:前向交叉证书
- 跨-[Index]:后向交叉证书
- CRL [Index]:基本 CRL
- deltacrl [Index]:Delta CRL
- crlstatus [Index]:CRL 发布状态
- deltacrlstatus [Index]:增量 CRL 发布状态
- dns:DNS 名称
- 角色：角色分隔
- 广告：高级服务器版
- 模板：模板
- csp [Index]:OCSP Url
- aia [Index]:AIA Url
- cdp [Index]:CDP Url
- localename:CA 的区域设置名称
- subjecttemplateoids:使用者模板 Oid

返回到[菜单](#menu)

## <a name="-cacert"></a>-ca.cert

CertUtil [Options] -ca.cert OutCACertFile [Index]

检索 CA 的证书

OutCACertFile： 输出文件

索引：CA 证书续订索引 （默认为最新）

[-f] [-split] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-cachain"></a>-ca.chain

CertUtil [Options] -ca.chain OutCACertChainFile [Index]

检索 CA 的证书链

OutCACertChainFile： 输出文件

索引：CA 证书续订索引 （默认为最新）

[-f] [-split] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-getcrl"></a>-GetCRL

CertUtil [Options] -GetCRL OutFile [Index] [delta]

获取 CRL

索引：CRL 索引或键索引 （最新密钥 CRL 的默认值）

增量： 增量 CRL （默认值为基本 CRL）

[-f] [-split] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-crl"></a>-CRL

CertUtil [Options] -CRL [dd:hh | republish] [delta]

发布新 Crl [或仅将增量 Crl]

dd:hh-新的 CRL 有效期内，小时和天数

重新发布-重新发布最新 Crl

增量-仅增量 Crl （默认值为基准 crl 和增量 Crl）

[-split] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-shutdown"></a>-shutdown

CertUtil [Options] -shutdown

关闭 Active Directory 证书服务

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-installcert"></a>-installCert

CertUtil [Options] -installCert [CACertFile]

安装证书颁发机构证书

[-f] [-silent] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-renewcert"></a>-renewCert

CertUtil [Options] -renewCert [ReuseKeys] [Machine\ParentCAName]

续订证书颁发机构证书

使用-f 要忽略的未完成的续订请求，并生成新的请求。

[-f] [-silent] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-schema"></a>-schema

CertUtil [选项]-架构 [Ext |Attrib |CRL]

转储证书架构

默认值为请求和证书表

Ext:扩展表

Attrib:属性表

CRL:CRL 表

[-split] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-view"></a>-view

CertUtil [选项] 的视图 [队列 |日志 |LogFail |撤消 |Ext |Attrib |CRL] [csv]

转储证书视图

队列：请求队列

日志：颁发或吊销证书，以及失败的请求

LogFail:失败的请求

吊销：吊销的证书

Ext:扩展表

Attrib:属性表

CRL:CRL 表

csv:输出如逗号分隔的值

若要显示所有条目的 StatusCode 列:-out StatusCode

若要显示所有列的最后一项:-限制"请求 Id = = $"

若要显示的三个请求 RequestId 和处置:-限制"请求 Id > = 37，RequestId\<40"-out"请求 Id，处理设置"

若要显示所有基 Crl 的行 Id 和 CRL 编号:-限制"CRLMinBase = 0"-out"CRLRowId，CRLNumber"CRL

To display Base CRL Number 3: -v -restrict "CRLMinBase=0,CRLNumber=3" -out "CRLRawCRL" CRL

若要显示整个 CRL 表：CRL

使用"日期 [+ |-dd:hh]"的日期限制

使用"现在 + dd:hh"相对于当前时间的日期

[-无提示][-拆分][-config Machine\CAName][-限制 RestrictionList][-out ColumnList]

返回到[菜单](#menu)

## <a name="-db"></a>-db

CertUtil [Options] -db

转储原始数据库

[-config Machine\CAName][-限制 RestrictionList][-out ColumnList]

返回到[菜单](#menu)

## <a name="-deleterow"></a>-deleterow

CertUtil [选项]-deleterow RowId |日期 [请求 |Cert |Ext |Attrib |CRL]

删除服务器数据库行

请求：失败和挂起的请求 （提交日期）

Cert:过期和吊销的证书 （到期日期）

Ext:扩展表

Attrib:属性表

CRL:CRL 表 （超过到期日期）

若要删除挂起的请求的失败和提交在 2001 年 1 月 22 日：1/22/2001 请求

若要删除过期的所有证书通过 2001 年 1 月 22 日：1/22/2001 证书

若要删除的 RequestId 37 证书行、 属性和扩展：37

若要删除过期的 Crl 的 2001 年 1 月 22 日：2001 年 1 月 22 日 CRL

[-f] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-backup"></a>-backup

CertUtil [选项]-备份 BackupDirectory [增量] [KeepLog]

备份 Active Directory 证书服务

BackupDirectory： 目录来存储备份的数据

增量： 执行增量备份仅 （默认值为完整备份）

KeepLog： 保留数据库日志文件 （默认值为截断日志文件）

[-f] [-config Machine\CAName] [-p Password]

返回到[菜单](#menu)

## <a name="-backupdb"></a>-backupDB

CertUtil [Options] -backupDB BackupDirectory [Incremental] [KeepLog]

备份 Active Directory 证书服务数据库

BackupDirectory： 目录来存储备份的数据库文件

增量： 执行增量备份仅 （默认值为完整备份）

KeepLog： 保留数据库日志文件 （默认值为截断日志文件）

[-f] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-backupkey"></a>-backupKey

CertUtil [Options] -backupKey BackupDirectory

备份 Active Directory 证书服务证书和私钥

BackupDirectory： 目录来存储备份的 PFX 文件

[-f] [-config Machine\CAName] [-p Password] [-t Timeout]

返回到[菜单](#menu)

## <a name="-restore"></a>-restore

CertUtil [Options] -restore BackupDirectory

还原 Active Directory 证书服务

BackupDirectory： 目录，其中包含要还原的数据

[-f] [-config Machine\CAName] [-p Password]

返回到[菜单](#menu)

## <a name="-restoredb"></a>-restoreDB

CertUtil [Options] -restoreDB BackupDirectory

还原 Active Directory 证书服务数据库

BackupDirectory： 目录，其中包含要还原的数据库文件

[-f] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-restorekey"></a>-restoreKey

CertUtil [Options] -restoreKey BackupDirectory | PFXFile

还原 Active Directory 证书服务证书和私钥

BackupDirectory： 目录，其中包含要还原的 PFX 文件

PFXFile:若要还原的 PFX 文件

[-f] [-config Machine\CAName] [-p Password]

返回到[菜单](#menu)

## <a name="-importpfx"></a>-importPFX

CertUtil [Options] -importPFX [CertificateStoreName] PFXFile [Modifiers]

导入证书和私钥

CertificateStoreName:证书存储区名称。  请参阅[-存储](#-store)。

PFXFile:要导入 PFX 文件

修饰符：一个或多个以下的以逗号分隔列表：

1. AT_SIGNATURE:更改签名到 KeySpec
2. AT_KEYEXCHANGE:更改为密钥交换 KeySpec
3. NoExport:使非可导出私钥
4. NoCert:不导入证书
5. NoChain:不导入的证书链
6. NoRoot:不导入根证书
7. 保护：保护使用密码的密钥
8. NoProtect:执行不密码保护密钥

默认值为个人计算机存储。

[-f] [-user] [-p Password] [-csp Provider]

返回到[菜单](#menu)

## <a name="-dynamicfilelist"></a>-dynamicfilelist

CertUtil [Options] -dynamicfilelist

显示动态文件列表

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-databaselocations"></a>-databaselocations

CertUtil [Options] -databaselocations

显示数据库位置

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-hashfile"></a>-hashfile

CertUtil [Options] -hashfile InFile [HashAlgorithm]

生成并通过 file 显示加密哈希

返回到[菜单](#menu)

## <a name="-store"></a>-store

CertUtil [Options] -store [CertificateStoreName [CertId [OutputFile]]]

转储证书存储区

CertificateStoreName:证书存储区名称。 示例：

- "我的"，"CA"（默认值）、"Root"，
- "ldap: / / CN = 证书颁发机构，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 一个？ objectClass = 证书颁发机构"（查看根证书）
- "ldap: / / CN = CAName，CN = 证书颁发机构，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基？ objectClass = 证书颁发机构"（修改根证书）
- "ldap: / / CN = CAName，CN = MachineName，CN = CDP，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ certificateRevocationList？ 基？ objectClass = cRLDistributionPoint"(视图 Crl)
- "ldap: / / CN = NTAuthCertificates，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基？ objectClass = 证书颁发机构"（企业 CA 证书）
- ldap:（AD 计算机对象证书）
- -用户 ldap:（AD 用户对象证书）

CertId:证书或 CRL 匹配令牌。  这可以是序列号、 sha-1 证书、 CRL、 CTL 或公共密钥哈希，数值 cert 索引 （0，1，依此类推），数字的 CRL 索引 (。 0、.1、 等等)，数字 CTL 索引 (...0...1，依次类推） 的公共密钥、 签名或扩展 ObjectId，证书使用者公用名，电子邮件地址的 UPN 或 DNS 名称、 密钥容器名称或 CSP 名称、 模板名称或 ObjectId、 EKU 或应用程序策略 ObjectId 或 CRL 颁发者公用名。 其中许多可能会导致多个匹配项。

要保存匹配证书的输出文件： 文件

使用-用户访问而不是计算机存储区的用户存储。

使用-企业来访问计算机企业存储。

使用-服务能够访问计算机服务存储区。

使用的组策略来访问计算机组策略存储区。

示例：

- -企业 NTAuth
- -企业根 37
- -用户我 26e0aaaf000000000004
- CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-dc DCName]

返回到[菜单](#menu)

## <a name="-addstore"></a>-addstore

CertUtil [Options] -addstore CertificateStoreName InFile

将证书添加到存储

CertificateStoreName:证书存储区名称。  请参阅[-存储](#-store)。

InFile:若要添加到存储的证书或 CRL 文件。

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

返回到[菜单](#menu)

## <a name="-delstore"></a>-delstore

CertUtil [Options] -delstore CertificateStoreName CertId

从存储中删除证书

CertificateStoreName:证书存储区名称。  请参阅[-存储](#-store)。

CertId:证书或 CRL 匹配令牌。  请参阅[-存储](#-store)。

[-enterprise] [-user] [-GroupPolicy] [-dc DCName]

返回到[菜单](#menu)

## <a name="-verifystore"></a>-verifystore

CertUtil [Options] -verifystore CertificateStoreName [CertId]

验证证书存储区中

CertificateStoreName:证书存储区名称。  请参阅[-存储](#-store)。

CertId:证书或 CRL 匹配令牌。  请参阅[-存储](#-store)。

[-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-dc DCName] [-t Timeout]

返回到[菜单](#menu)

## <a name="-repairstore"></a>-repairstore

CertUtil [Options] -repairstore CertificateStoreName CertIdList [PropertyInfFile | SDDLSecurityDescriptor]

修复键关联或更新证书属性或密钥安全描述符

CertificateStoreName:证书存储区名称。  请参阅[-存储](#-store)。

证书或 CRL 匹配令牌 CertIdList： 以逗号分隔列表。 请参阅[-存储](#-store)CertId 说明。

包含外部属性 PropertyInfFile-INF 文件：

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

[-f] [-enterprise] [-user] [-GroupPolicy] [-silent] [-split] [-csp Provider]

返回到[菜单](#menu)

## <a name="-viewstore"></a>-viewstore

CertUtil [Options] -viewstore [CertificateStoreName [CertId [OutputFile]]]

转储证书存储区

CertificateStoreName:证书存储区名称。 示例：

- "我的"，"CA"（默认值）、"Root"，
- "ldap: / / CN = 证书颁发机构，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 一个？ objectClass = 证书颁发机构"（查看根证书）
- "ldap: / / CN = CAName，CN = 证书颁发机构，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基？ objectClass = 证书颁发机构"（修改根证书）
- "ldap: / / CN = CAName，CN = MachineName，CN = CDP，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ certificateRevocationList？ 基？ objectClass = cRLDistributionPoint"(视图 Crl)
- "ldap: / / CN = NTAuthCertificates，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基？ objectClass = 证书颁发机构"（企业 CA 证书）
- ldap:（AD 计算机对象证书）
- -用户 ldap:（AD 用户对象证书）

CertId:证书或 CRL 匹配令牌。 这可以是序列号、 sha-1 证书、 CRL、 CTL 或公共密钥哈希，数值 cert 索引 （0，1，依此类推），数字的 CRL 索引 (。 0、.1、 等等)，数字 CTL 索引 (...0...1，依次类推） 的公共密钥、 签名或扩展 ObjectId，证书使用者公用名，电子邮件地址的 UPN 或 DNS 名称、 密钥容器名称或 CSP 名称、 模板名称或 ObjectId、 EKU 或应用程序策略 ObjectId 或 CRL 颁发者公用名。 其中许多可能会导致多个匹配项。

要保存匹配证书的输出文件： 文件

使用-用户访问而不是计算机存储区的用户存储。

使用-企业来访问计算机企业存储。

使用-服务能够访问计算机服务存储区。

使用的组策略来访问计算机组策略存储区。

示例：

1. -企业 NTAuth
2. -企业根 37
3. -用户我 26e0aaaf000000000004
4. CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

返回到[菜单](#menu)

## <a name="-viewdelstore"></a>-viewdelstore

CertUtil [Options] -viewdelstore [CertificateStoreName [CertId [OutputFile]]]

从存储中删除证书

CertificateStoreName:证书存储区名称。 示例：

- "我的"，"CA"（默认值）、"Root"，
- "ldap: / / CN = 证书颁发机构，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 一个？ objectClass = 证书颁发机构"（查看根证书）
- "ldap: / / CN = CAName，CN = 证书颁发机构，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基？ objectClass = 证书颁发机构"（修改根证书）
- "ldap: / / CN = CAName，CN = MachineName，CN = CDP，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ certificateRevocationList？ 基？ objectClass = cRLDistributionPoint"(视图 Crl)
- "ldap: / / CN = NTAuthCertificates，CN = Public Key Services，CN = Services，CN = Configuration，DC = cpandl，DC = com？ cACertificate？ 基？ objectClass = 证书颁发机构"（企业 CA 证书）
- ldap:（AD 计算机对象证书）
- -用户 ldap:（AD 用户对象证书）

CertId:证书或 CRL 匹配令牌。 这可以是序列号、 sha-1 证书、 CRL、 CTL 或公共密钥哈希，数值 cert 索引 （0，1，依此类推），数字的 CRL 索引 (。 0、.1、 等等)，数字 CTL 索引 (...0...1，依次类推） 的公共密钥、 签名或扩展 ObjectId，证书使用者公用名，电子邮件地址的 UPN 或 DNS 名称、 密钥容器名称或 CSP 名称、 模板名称或 ObjectId、 EKU 或应用程序策略 ObjectId 或 CRL 颁发者公用名。 其中许多可能会导致多个匹配项。

要保存匹配证书的输出文件： 文件

使用-用户访问而不是计算机存储区的用户存储。

使用-企业来访问计算机企业存储。

使用-服务能够访问计算机服务存储区。

使用的组策略来访问计算机组策略存储区。

示例：

1. -企业 NTAuth
2. -企业根 37
3. -用户我 26e0aaaf000000000004
4. CA .11

[-f] [-enterprise] [-user] [-GroupPolicy] [-dc DCName]

返回到[菜单](#menu)

## <a name="-dspublish"></a>-dsPublish

[选项] CertUtil-dsPublish CertFile [NTAuthCA |RootCA |SubCA |CrossCA |KRA |用户 |计算机]

CertUtil [Options] -dsPublish CRLFile [DSCDPContainer [DSCDPCN]]

将证书或 CRL 发布到 Active Directory

CertFile： 要发布的证书文件

NTAuthCA:将证书发布到 DS 企业存储

RootCA:将证书发布到 DS 受信任的根存储区

SubCA:将 CA 证书发布到 DS CA 对象

CrossCA:跨 DS CA 对象向证书发布

KRA:将证书发布到 DS Key Recovery Agent 对象

用户：将证书发布到用户 DS 对象

计算机：将证书发布到机 DS 对象

CRLFile:若要发布的 CRL 文件

DSCDPContainer:DS CDP 容器 CN，通常是 CA 计算机名称

DSCDPCN:DS CDP 对象 CN，通常基于净化的 CA 短名称和密钥索引

使用-f 创建 DS 对象。

[-f] [-user] [-dc DCName]

返回到[菜单](#menu)

## <a name="-adtemplate"></a>-ADTemplate

CertUtil [Options] -ADTemplate [Template]

显示 AD 模板

[-f] [-user] [-ut] [-mt] [-dc DCName]

## <a name="-template"></a>-Template

CertUtil [Options] -Template [Template]

显示注册策略模板

[-f] [-user] [-silent] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

返回到[菜单](#menu)

## <a name="-templatecas"></a>-TemplateCAs

CertUtil [Options] -TemplateCAs Template

显示 Ca 模板

[-f] [-user] [-dc DCName]

返回到[菜单](#menu)

## <a name="-catemplates"></a>-CATemplates

CertUtil [Options] -CATemplates [Template]

CA 的显示模板

[-f] [-user] [-ut] [-mt] [-config Machine\CAName] [-dc DCName]

返回到[菜单](#menu)

## <a name="-setcasites"></a>-SetCASites

CertUtil [Options] -SetCASites [set] [SiteName]

CertUtil [Options] -SetCASites verify [SiteName]

CertUtil [Options] -SetCASites delete

设置、 验证或删除 CA 站点名称

- -Config 选项用于针对单个 CA （默认值为所有 Ca）
- *SiteName*仅当针对单个 CA 时，才允许
- -F 用于重写为指定的验证错误*SiteName*
- 使用-f 中删除所有 CA 站点名称

[-f] [-config Machine\CAName] [-dc DCName]

> [!NOTE]
> 配置 Ca 的 Active Directory 域服务 (AD DS) 站点感知的详细信息，请参阅[适用于 AD CS 和 PKI 客户端的 AD DS 站点感知](https://social.technet.microsoft.com/wiki/contents/articles/14106.ad-ds-site-awareness-for-ad-cs-and-pki-clients.aspx)。

返回到[菜单](#menu)

## <a name="-enrollmentserverurl"></a>-enrollmentServerURL

CertUtil [Options] -enrollmentServerURL [URL AuthenticationType [Priority] [Modifiers]]

CertUtil [Options] -enrollmentServerURL URL delete

显示、 添加或删除与 CA 相关联的注册服务器 Url

AuthenticationType:添加 URL 时指定一个以下客户端身份验证方法

1. Kerberos:使用 Kerberos SSL 凭据
2. 用户名：指定的帐户用于 SSL 凭据
3. ClientCertificate:使用 X.509 证书的 SSL 凭据
4. Anonymous：使用匿名的 SSL 凭据

删除： 删除指定的 URL 与 CA 相关联

如果未指定添加 URL 时，优先级： 默认为"1"

修饰符-以逗号分隔的一个或多个以下的列表：

1. AllowRenewalsOnly:可以向此 CA 通过此 URL 提交仅续订请求
2. AllowKeyBasedRenewal:允许使用在 AD 中具有关联的帐户的证书。 这适用于仅使用 ClientCertificate 和 AllowRenewalsOnly 模式

[-config Machine\CAName][-dc DCName]

返回到[菜单](#menu)

## <a name="-adca"></a>-ADCA

CertUtil [Options] -ADCA [CAName]

显示 AD Ca

[-f] [-split] [-dc DCName]

返回到[菜单](#menu)

## <a name="-ca"></a>-CA

CertUtil [Options] -CA [CAName | TemplateName]

显示注册策略 Ca

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

返回到[菜单](#menu)

## <a name="-policy"></a>-Policy

显示注册策略

[-f] [-user] [-silent] [-split] [-PolicyServer URLOrId] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

返回到[菜单](#menu)

## <a name="-policycache"></a>-PolicyCache

CertUtil [Options] -PolicyCache [delete]

显示或删除注册策略缓存条目

删除： 删除策略服务器的缓存条目

-f： 使用-f 中删除所有缓存条目

[-f] [-user] [-PolicyServer URLOrId]

返回到[菜单](#menu)

## <a name="-credstore"></a>-CredStore

CertUtil [Options] -CredStore [URL]

CertUtil [Options] -CredStore URL add

CertUtil [Options] -CredStore URL delete

显示、 添加或删除凭据存储区条目

URL： 目标 URL。  使用\*以匹配的所有条目。 使用 https://machine\* 若要匹配的 URL 前缀。

添加： 将凭据存储区条目添加。 此外必须指定 SSL 凭据。

删除： 删除凭据存储区条目

-f： 使用-f 来覆盖一个条目，或若要删除多个条目。

[-f] [-user] [-silent] [-Anonymous] [-Kerberos] [-ClientCertificate ClientCertId] [-UserName UserName] [-p Password]

返回到[菜单](#menu)

## <a name="-installdefaulttemplates"></a>-InstallDefaultTemplates

CertUtil [Options] -InstallDefaultTemplates

安装默认的证书模板

[-dc DCName]

返回到[菜单](#menu)

## <a name="-urlcache"></a>-URLCache

CertUtil [Options] -URLCache [URL | CRL | \* [delete]]

显示或删除 URL 缓存条目

URL： 缓存的 URL

对所有缓存 CRL 的 Url 仅 CRL:

\*： 对所有缓存的 Url

删除： 从当前用户的本地缓存中删除相关 Url

使用-f 强制提取特定的 URL，并更新缓存。

[-f] [-split]

返回到[菜单](#menu)

## <a name="-pulse"></a>-pulse

CertUtil [Options] -pulse

暂停自动注册事件

[-用户]

返回到[菜单](#menu)

## <a name="-machineinfo"></a>-MachineInfo

CertUtil [Options] -MachineInfo DomainName\MachineName$

显示 Active Directory 计算机对象信息

返回到[菜单](#menu)

## <a name="-dcinfo"></a>-DCInfo

CertUtil [Options] -DCInfo [Domain] [Verify | DeleteBad | DeleteAll]

显示域控制器的信息

默认为显示 DC 而无需验证证书

[-f] [-user] [-urlfetch] [-dc DCName] [-t Timeout]

> [!TIP]
> 指定的 Active Directory 域服务 (AD DS) 域的功能 **[Domain]** ，并指定域控制器 (**dc**) 已添加 Windows Server 2012 中。 若要成功运行该命令，必须使用该帐户是属于**Domain Admins**或**Enterprise Admins**。 此命令的行为的修改是按如下所示：</br>> 1.  如果未指定域且未指定特定的域控制器，此选项将返回域控制器来处理从默认域控制器的列表。</br>> 2.  如果未指定域，但指定的域控制器，生成指定的域控制器上的证书的报告。</br>> 3.  如果指定了某个域，但未指定域控制器，以及有关每个域控制器列表中的证书的报告生成的域控制器列表。</br>> 4.  如果指定了域和域控制器，从目标的域控制器生成的域控制器列表。 此外会生成每个域控制器列表中的证书的报告。

例如，假定存在名为 CPANDL DC1 的域控制器创建名为 CPANDL 域。 您可以运行以下命令检索域控制器和其证书的列表，从 CPANDL DC1: certutil-dc cpandl dc1-dcinfo cpandl

返回到[菜单](#menu)

## <a name="-entinfo"></a>-EntInfo

CertUtil [Options] -EntInfo DomainName\MachineName$

[-f] [-user]

返回到[菜单](#menu)

## <a name="-tcainfo"></a>-TCAInfo

CertUtil [Options] -TCAInfo [DomainDN | -]

显示 CA 信息

[-f] [-enterprise] [-user] [-urlfetch] [-dc DCName] [-t Timeout]

返回到[菜单](#menu)

## <a name="-scinfo"></a>-SCInfo

CertUtil [Options] -SCInfo [ReaderName [CRYPT_DELETEKEYSET]]

显示智能卡信息

CRYPT_DELETEKEYSET:删除智能卡上的所有密钥

[-silent] [-split] [-urlfetch] [-t Timeout]

返回到[菜单](#menu)

## <a name="-scroots"></a>-SCRoots

CertUtil [Options] -SCRoots update [+][InputRootFile] [ReaderName]

CertUtil [Options] -SCRoots save @OutputRootFile [ReaderName]

CertUtil [Options] -SCRoots view [InputRootFile | ReaderName]

CertUtil [Options] -SCRoots delete [ReaderName]

管理智能卡的根证书

[-f] [-split] [-p Password]

返回到[菜单](#menu)

## <a name="-verifykeys"></a>-verifykeys

CertUtil [Options] -verifykeys [KeyContainerName CACertFile]

验证公钥/私钥的密钥集

若要验证的密钥的 KeyContainerName： 密钥容器名称。 默认值为计算机密钥。  使用-用户的用户密钥。

CACertFile： 签名或加密的证书文件

如果未不指定任何参数，根据其私钥验证每个签名的 CA 证书。

仅针对本地 CA 或本地密钥执行此操作。

[-f]。[-用户][-无提示][-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-verify"></a>-verify

CertUtil [Options] -verify CertFile [ApplicationPolicyList | - [IssuancePolicyList]]

CertUtil [Options] -verify CertFile [CACertFile [CrossedCACertFile]]

CertUtil [Options] -verify CRLFile CACertFile [IssuedCertFile]

CertUtil [Options] -verify CRLFile CACertFile [DeltaCRLFile]

验证证书、 CRL 或链

CertFile:若要验证的证书

所需的应用程序策略 ObjectIds ApplicationPolicyList： 可选的以逗号分隔列表

所需的颁发策略 ObjectIds IssuancePolicyList： 可选的以逗号分隔列表

CACertFile： 可选发证 CA 证书，以便验证

CrossedCACertFile： 可选证书交叉验证 CertFile

CRLFile:若要验证的 CRL

涵盖的 CRLFile IssuedCertFile： 可选已颁发的证书

DeltaCRLFile： 可选增量 crl 的步骤

如果指定 ApplicationPolicyList，链构建被限制到指定的应用程序策略对有效的链。

如果指定 IssuancePolicyList，链生成仅限于对指定的颁发策略有效链。

如果指定 CACertFile，CACertFile 中的字段来验证对 CertFile 或 CRLFile。

如果未指定 CACertFile，CertFile 用于构建和验证完整链。

如果同时指定 CACertFile 和 CrossedCACertFile，CACertFile 和 CrossedCACertFile 中的字段来验证对 CertFile。

如果指定 IssuedCertFile，IssuedCertFile 中的字段来验证对 CRLFile。

如果指定 DeltaCRLFile，DeltaCRLFile 中的字段来验证对 CRLFile。

[-f] [-enterprise] [-user] [-silent] [-split] [-urlfetch] [-t Timeout]

返回到[菜单](#menu)

## <a name="-verifyctl"></a>-verifyCTL

CertUtil [Options] -verifyCTL CTLObject [CertDir] [CertFile]

验证 AuthRoot 或不允许的证书的 CTL

CTLObject:标识 CTL 验证：

- AuthRootWU： 从 URL 缓存读取 AuthRoot CAB 和匹配的证书。 使用-f 要改为从 Windows Update 下载。
- DisallowedWU： 读取不允许使用证书 CAB 和不允许从 URL 缓存的证书存储区文件。  使用-f 要改为从 Windows Update 下载。
- AuthRoot： 读取的注册表缓存 AuthRoot CTL。  使用-f 和不信任，若要强制更新注册表 CertFile 缓存 AuthRoot 和不允许使用证书的 Ctl。
- 不允许： 读取的注册表缓存不允许使用证书 CTL。 -f 与 AuthRoot 一样具有相同的行为。
- CTLFileName： 文件或 http: CTL 或 CAB 路径

CertDir： 此文件夹包含证书的 CTL 的匹配项。 Http： 文件夹路径必须以路径分隔符结尾。 如果未使用 AuthRoot 或不允许指定一个文件夹，将多个位置搜索匹配的证书： 本地证书存储、 crypt32.dll 资源和本地的 URL 缓存。 -F 用于从 Windows 更新在必要时才下载。 否则默认为 CTLObject 为相同的文件夹或 web 站点。

CertFile： 包含要验证的证书文件。 证书将与 CTL 条目匹配，并且匹配显示的结果。 禁止显示的默认输出的大部分。

[-f] [-user] [-split]

返回到[菜单](#menu)

## <a name="-sign"></a>-登录

CertUtil [Options] -sign InFileList|SerialNumber|CRL OutFileList [StartDate+dd:hh] [+SerialNumberList | -SerialNumberList | -ObjectIdList | @ExtensionFile]

CertUtil [Options] -sign InFileList|SerialNumber|CRL OutFileList [#HashAlgorithm] [+AlternateSignatureAlgorithm | -AlternateSignatureAlgorithm]

重新签名的 CRL 或证书

若要修改并重新签名的证书或 CRL 文件的 InFileList： 以逗号分隔列表

序列号：若要创建的证书序列号。 有效期和其他选项不会显示。

CRL:创建空的 CRL。 有效期和其他选项不会显示。

已修改的证书或 CRL 输出文件的 OutFileList： 以逗号分隔列表。 文件数必须匹配 InFileList。

StartDate + dd:hh： 新的有效期： 可选的日期加上;可选的天数和小时有效期;如果同时指定两者，使用加号 （+） 分隔符。 使用"现在 [+ dd:hh]"在当前时间开始。 使用"从不"（适用于仅 Crl) 有无过期日期。

SerialNumberList： 以逗号分隔的序列号列表，以添加或删除

ObjectIdList： 以逗号分隔扩展 ObjectId 列表删除

@ExtensionFile: INF 文件包含要更新或删除扩展：

```
[Extensions]
     2.5.29.31 = ; Remove CRL Distribution Points extension
     2.5.29.15 = "{hex}" ; Update Key Usage extension
     _continue_="03 02 01 86"
```

HashAlgorithm:以 # 符号开头的哈希算法的名称

内容： AlternateSignatureAlgorithm： 备用签名算法说明符

负号会导致序列号和扩展被删除。 正号会导致序列号添加到 CRL。 当从 CRL 中删除项，列表可能包含序列号和 ObjectIds。 内容： AlternateSignatureAlgorithm 前的减号将导致要使用的旧签名格式。 加号之前内容： AlternateSignatureAlgorithm 导致 alternature 签名格式，才能使用。 如果未指定内容： AlternateSignatureAlgorithm 则使用证书或 CRL 的签名格式。

[-nullsign] [-f] [-silent] [-Cert CertId]

返回到[菜单](#menu)

## <a name="-vroot"></a>-vroot

CertUtil [Options] -vroot [delete]

创建/删除 web 虚拟根和文件共享

返回到[菜单](#menu)

## <a name="-vocsproot"></a>-vocsproot

CertUtil [Options] -vocsproot [delete]

创建/删除的虚拟根 web OCSP web 代理

返回到[菜单](#menu)

## <a name="-addenrollmentserver"></a>-addEnrollmentServer

CertUtil [Options] -addEnrollmentServer Kerberos | UserName | ClientCertificate [AllowRenewalsOnly] [AllowKeyBasedRenewal]

添加注册服务器应用程序

添加注册服务器应用程序和应用程序池，如有必要，为指定的 CA。 此命令不会安装二进制文件或包。 与客户端连接到证书注册服务器的以下身份验证方法之一。

- Kerberos:使用 Kerberos SSL 凭据
- 用户名：指定的帐户用于 SSL 凭据
- ClientCertificate:使用 X.509 证书的 SSL 凭据
- AllowRenewalsOnly:可以向此 CA 通过此 URL 提交仅续订请求
- AllowKeyBasedRenewal-允许使用在 AD 中具有关联的帐户的证书。 这仅使用 ClientCertificate 和 AllowRenewalsOnly 模式适用。

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-deleteenrollmentserver"></a>-deleteEnrollmentServer

CertUtil [Options] -deleteEnrollmentServer Kerberos | UserName | ClientCertificate

删除注册服务器应用程序

删除注册服务器应用程序和应用程序池，如有必要，为指定的 CA。 此命令不会删除二进制文件或包。 与客户端连接到证书注册服务器的以下身份验证方法之一。

1. Kerberos:使用 Kerberos SSL 凭据
2. 用户名：指定的帐户用于 SSL 凭据
3. ClientCertificate:使用 X.509 证书的 SSL 凭据

[-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-addpolicyserver"></a>-addPolicyServer

CertUtil [Options] -addPolicyServer Kerberos | UserName | ClientCertificate [KeyBasedRenewal]

添加策略服务器应用程序

如有必要，将添加一个策略服务器应用程序和应用程序池。 此命令不会安装二进制文件或包。 与客户端连接到证书策略服务器的以下身份验证方法之一：

- Kerberos:使用 Kerberos SSL 凭据
- 用户名：指定的帐户用于 SSL 凭据
- ClientCertificate:使用 X.509 证书的 SSL 凭据
- KeyBasedRenewal:包含 KeyBasedRenewal 模板的策略返回到客户端。 此标志仅适用于用户名和 ClientCertificate 身份验证。

返回到[菜单](#menu)

## <a name="-deletepolicyserver"></a>-deletePolicyServer

CertUtil [Options] -deletePolicyServer Kerberos | UserName | ClientCertificate [KeyBasedRenewal]

删除策略服务器应用程序

如有必要，删除策略服务器应用程序和应用程序池。 此命令不会删除二进制文件或包。 与客户端连接到证书策略服务器的以下身份验证方法之一：

1. Kerberos:使用 Kerberos SSL 凭据
2. 用户名：指定的帐户用于 SSL 凭据
3. ClientCertificate:使用 X.509 证书的 SSL 凭据
4. KeyBasedRenewal:KeyBasedRenewal 策略服务器

返回到[菜单](#menu)

## <a name="-oid"></a>-oid

CertUtil [Options] -oid ObjectId [DisplayName | delete [LanguageId [Type]]]

CertUtil [Options] -oid GroupId

CertUtil [Options] -oid AlgId | AlgorithmName [GroupId]

显示 ObjectId 或设置显示名称

- ObjectId-ObjectId 来显示或添加显示名称
- 有关 ObjectIds 枚举 GroupId-GroupId 小数
- 要查找 ObjectId 为 AlgId-十六进制 AlgId
- 要查找 ObjectId AlgorithmName-算法名称
- DisplayName-显示名称将存储在 DS
- 删除-删除显示名称
- LanguageId-语言 Id (到当前的默认值：1033)
- 类型-DS 对象类型来创建：模板 （默认值），2 表示颁发策略，为应用程序策略的 3 1
- 使用-f 创建 DS 对象。

[-f]

返回到[菜单](#menu)

## <a name="-error"></a>-error

CertUtil [Options] -error ErrorCode

显示错误代码的消息文本

返回到[菜单](#menu)

## <a name="-getreg"></a>-getreg

CertUtil [Options] -getreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]][RegistryValueName]

显示注册表值

ca:使用 CA 的注册表项

还原：使用 CA 还原注册表项

策略：使用策略模块注册表项

退出：使用第一次退出模块的注册表项

模板：使用模板的注册表项 （使用-用户模板的用户）

注册：使用注册的注册表项 （使用-用户上下文的用户）

链：使用链 configuration 注册表项

PolicyServers:使用策略服务器注册表项

ProgId:使用策略或退出模块的 ProgId （注册表子项名称）

RegistryValueName： 注册表值名称 (使用"名称\*"为前缀匹配项)

值： 新的数字、 字符串或日期的注册表值或文件名。 如果数字值开头，则"+"或"-"，设置或清除现有的注册表值中的新值中指定的位。

如果字符串值开头，则"+"或"-"，和现有的值是 REG_MULTI_SZ 值、 添加或删除现有的注册表值从字符串。 若要强制创建 REG_MULTI_SZ 值，请添加到末尾的字符串值"\n"。

如果值以"@"，值的其余部分是包含二进制值的十六进制文本表示形式的文件的名称。 如果它未引用有效的文件，而是解析为 [Date] [+ |-] [dd:hh]-一个可选的日期加上或减去可选天数和时数。 如果同时指定两者，使用加号 （+） 或减号 （-） 分隔符。 使用"现在 + dd:hh"相对于当前时间的日期。

使用"chain\ChainCacheResyncFiletime @now"若要有效地刷新缓存的 Crl。

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-setreg"></a>-setreg

CertUtil [Options] -setreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]]RegistryValueName Value

设置注册表值

ca:使用 CA 的注册表项

还原：使用 CA 还原注册表项

策略：使用策略模块注册表项

退出：使用第一次退出模块的注册表项

模板：使用模板的注册表项 （使用-用户模板的用户）

注册：使用注册的注册表项 （使用-用户上下文的用户）

链：使用链 configuration 注册表项

PolicyServers:使用策略服务器注册表项

ProgId:使用策略或退出模块的 ProgId （注册表子项名称）

RegistryValueName： 注册表值名称 (使用"名称\*"为前缀匹配项)

值： 新的数字、 字符串或日期的注册表值或文件名。 如果数字值开头，则"+"或"-"，设置或清除现有的注册表值中的新值中指定的位。

如果字符串值开头，则"+"或"-"，和现有的值是 REG_MULTI_SZ 值、 添加或删除现有的注册表值从字符串。 若要强制创建 REG_MULTI_SZ 值，请添加到末尾的字符串值"\n"。

如果值以"@"，值的其余部分是包含二进制值的十六进制文本表示形式的文件的名称。 如果它未引用有效的文件，而是解析为 [Date] [+ |-] [dd:hh]-一个可选的日期加上或减去可选天数和时数。 如果同时指定两者，使用加号 （+） 或减号 （-） 分隔符。 使用"现在 + dd:hh"相对于当前时间的日期。

使用"chain\ChainCacheResyncFiletime @now"若要有效地刷新缓存的 Crl。

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-delreg"></a>-delreg

CertUtil [Options] -delreg [{ca|restore|policy|exit|template|enroll|chain|PolicyServers}\[ProgId\]][RegistryValueName]

删除注册表值

ca:使用 CA 的注册表项

还原：使用 CA 还原注册表项

策略：使用策略模块注册表项

退出：使用第一次退出模块的注册表项

模板：使用模板的注册表项 （使用-用户模板的用户）

注册：使用注册的注册表项 （使用-用户上下文的用户）

链：使用链 configuration 注册表项

PolicyServers:使用策略服务器注册表项

ProgId:使用策略或退出模块的 ProgId （注册表子项名称）

RegistryValueName： 注册表值名称 (使用"名称\*"为前缀匹配项)

值： 新的数字、 字符串或日期的注册表值或文件名。 如果数字值开头，则"+"或"-"，设置或清除现有的注册表值中的新值中指定的位。

如果字符串值开头，则"+"或"-"，和现有的值是 REG_MULTI_SZ 值、 添加或删除现有的注册表值从字符串。 若要强制创建 REG_MULTI_SZ 值，请添加到末尾的字符串值"\n"。

如果值以"@"，值的其余部分是包含二进制值的十六进制文本表示形式的文件的名称。 如果它未引用有效的文件，而是解析为 [Date] [+ |-] [dd:hh]-一个可选的日期加上或减去可选天数和时数。 如果同时指定两者，使用加号 （+） 或减号 （-） 分隔符。 使用"现在 + dd:hh"相对于当前时间的日期。

使用"chain\ChainCacheResyncFiletime @now"若要有效地刷新缓存的 Crl。

[-f] [-user] [-GroupPolicy] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-importkms"></a>-ImportKMS

CertUtil [Options] -ImportKMS UserKeyAndCertFile [CertId]

用户密钥和证书导入到 server 数据库中的密钥存档

UserKeyAndCertFile-数据文件包含用户的私钥和证书以进行存档。  这可以是以下任一项：

- Exchange 密钥管理服务器 (KMS) 导出文件
- PFX 文件

CertId:KMS 将导出文件解密证书匹配令牌。  请参阅[-存储](#-store)。

使用-f 导入不由 CA 颁发的证书。

[-f] [-silent] [-split] [-config Machine\CAName] [-p Password] [-symkeyalg SymmetricKeyAlgorithm[,KeyLength]]

返回到[菜单](#menu)

## <a name="-importcert"></a>-ImportCert

CertUtil [Options] -ImportCert Certfile [ExistingRow]

证书文件导入数据库

使用 ExistingRow 导入证书代替相同键的挂起请求。

使用-f 导入不由 CA 颁发的证书。

CA 还可能需要将配置为支持外部证书导入： certutil-setreg ca\KRAFlags + KRAF_ENABLEFOREIGN

[-f] [-config Machine\CAName]

返回到[菜单](#menu)

## <a name="-getkey"></a>-GetKey

CertUtil [Options] -GetKey SearchToken [RecoveryBlobOutFile]

CertUtil [选项] GetKey SearchToken 脚本 OutputScriptFile

CertUtil [选项] GetKey SearchToken 检索 |恢复 OutputFileBaseName

检索已存档私钥的恢复点、 生成恢复脚本，或恢复存档的密钥

脚本： 生成一个脚本来检索和恢复的密钥 （默认行为或如果找到多个匹配的恢复候选项，如果输出文件不是指定）。

检索： 检索一个或多个密钥恢复 Blob （如果找到一个匹配的恢复候选项，并且如果指定输出文件的默认行为）

恢复： 检索并恢复在 （需要 Key Recovery Agent 证书和私钥） 的一个步骤中的专用密钥

SearchToken:用于选择的密钥和证书才会得到恢复。

可以是以下任一项：

1. 证书公用名
2. 证书序列号
3. 证书的 sha-1 哈希 （指纹）
4. 证书 KeyId sha-1 哈希 （使用者密钥标识符）
5. 请求者名称 （域 \ 用户）
6. UPN (user@domain)

包含证书链和关联的私钥，仍加密到一个或多个 Key Recovery Agent 证书 RecoveryBlobOutFile： 输出文件。

包含批处理脚本来检索并恢复私钥 OutputScriptFile： 输出文件。

OutputFileBaseName： 输出文件基名称。 用于检索、 截断任何扩展插件和特定于证书的字符串和.rec 扩展名追加的每个密钥恢复 blob。  每个文件包含证书链和关联的私钥，仍加密到一个或多个 Key Recovery Agent 证书。 要进行恢复，任何扩展被截断并追加.p12 扩展。  包含已恢复的证书链和关联的私钥，以 PFX 文件形式存储。

[-f] [-UnicodeText] [-silent] [-config Machine\CAName] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider]

返回到[菜单](#menu)

## <a name="-recoverkey"></a>-RecoverKey

CertUtil [Options] -RecoverKey RecoveryBlobInFile [PFXOutFile [RecipientIndex]]

恢复已存档的私钥

[-f] [-user] [-silent] [-split] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider] [-t Timeout]

返回到[菜单](#menu)

## <a name="-mergepfx"></a>-MergePFX

CertUtil [Options] -MergePFX PFXInFileList PFXOutFile [ExtendedProperties]

PFXInFileList:以逗号分隔的 PFX 输入的文件列表

PFXOutFile:输出的 PFX 文件

扩展属性：包括扩展的属性

在命令行上指定的密码是以逗号分隔的密码列表。  如果指定多个密码，则最后一次密码用于输出文件。  如果只提供一个密码或最后一次密码是"\*"，将提示用户输入输出文件的密码。

[-f] [-user] [-split] [-p Password] [-ProtectTo SAMNameAndSIDList] [-csp Provider]

返回到[菜单](#menu)

## <a name="-convertepf"></a>-ConvertEPF

CertUtil [Options] -ConvertEPF PFXInFileList EPFOutFile [cast | cast-] [V3CACertId][,Salt]

将 PFX 文件转换为 EPF 文件

PFXInFileList:以逗号分隔的 PFX 输入的文件列表

EPF:EPF 输出文件

强制转换：使用强制转换 64 加密

强制转换的：使用强制转换 64 加密 （导出）

V3CACertId:V3 CA 证书匹配标记。  请参阅[-存储](#-store)CertId 说明。

Salt:EPF 输出文件 salt 字符串

在命令行上指定的密码是以逗号分隔的密码列表。 如果指定多个密码，则最后一次密码用于输出文件。  如果只提供一个密码或最后一次密码是"\*"，将提示用户输入输出文件的密码。

[-f] [-silent] [-split] [-dc DCName] [-p Password] [-csp Provider]

返回到[菜单](#menu)

## <a name="options"></a>选项

本部分定义可以使用该命令指定的选项。

|选项|描述|
|-------|-----------|
|-nullsign|将用作签名的数据的哈希|
|-f|强制覆盖|
|-enterprise|使用本地计算机企业注册表证书存储|
|-用户|使用 HKEY_CURRENT_USER 密钥或证书存储区|
|-GroupPolicy|使用组策略的证书存储区|
|-ut|显示用户模板|
|-mt|显示计算机模板|
|-Unicode|以 unicode 格式写入重定向的输出|
|-UnicodeText|以 unicode 格式写入输出文件|
|-gmt|显示时间为 GMT|
|-seconds|以秒和毫秒为单位显示时间|
|-无提示|使用无提示标志来获取加密上下文|
|-split|拆分嵌入的 ASN.1 元素，并将保存到文件|
|-v|详细操作|
|-privatekey|显示密码和专用密钥数据|
|-将固定 PIN|智能卡 PIN|
|-urlfetch|检索和验证的证书都 AIA 和 CDP Crl|
|-config Machine\CAName|CA 和计算机名称字符串|
|-PolicyServer URLOrId|策略服务器 URL 或 id。用于选择 U /，我使用的服务器。 对于所有策略服务器，请使用-服务器 \*|
|-Anonymous|使用匿名的 SSL 凭据|
|-Kerberos|使用 Kerberos SSL 凭据|
|-ClientCertificate ClientCertId|使用 X.509 证书的 SSL 凭据。 用于选择 U / 我使用 clientCertificate。|
|-UserName 用户名|使用指定的帐户输入 SSL 凭据。 用于选择 U /，我使用的用户名。|
|-证书 CertId|签名证书|
|dc DCName|特定的域控制器为目标|
|-限制 RestrictionList|以逗号分隔的限制列表。 每个限制包含列名称、 关系运算符和常量整数、 字符串或日期。 加号或减号来指示排序顺序，之前可能有一个列名称。 示例：</br>"请求 Id = 47"</br>"+ RequesterName > = a，RequesterName < b"</br>"-RequesterName > 域、 处置 = 21"|
|-out ColumnList|以逗号分隔的列列表|
|-p 密码|密码|
|-ProtectTo SAMNameAndSIDList|以逗号分隔的 SAM 名称/SID 列表|
|-csp 提供程序|提供程序|
|-t 超时|URL 提取超时以毫秒为单位|
|-symkeyalg SymmetricKeyAlgorithm[,KeyLength]|可选的密钥长度，对称密钥算法的名称示例：AES 128 或 3DES|

返回到[菜单](#menu)

## <a name="additional-certutil-examples"></a>其他 certutil 示例

有关如何使用此命令的一些示例，请参阅

1. [从命令行管理 Active Directory 证书服务 (AD CS) 的 Certutil 示例](https://social.technet.microsoft.com/wiki/contents/articles/3063.certutil-examples-for-managing-active-directory-certificate-services-ad-cs-from-the-command-line.aspx)
2. [管理证书的 Certutil 任务](https://technet.microsoft.com/library/cc772898.aspx)
3. [使用 CertUtil.exe 命令行工具演练二进制请求导出](https://social.technet.microsoft.com/wiki/contents/articles/7573.active-directory-certificate-services-pki-key-archival-and-management.aspx)
4. [根 CA 证书续订](https://social.technet.microsoft.com/wiki/contents/articles/2016.root-ca-certificate-renewal.aspx)
5. [certutil](https://msdn.microsoft.com/subscriptions/cc773087.aspx)

返回到[菜单](#menu)
