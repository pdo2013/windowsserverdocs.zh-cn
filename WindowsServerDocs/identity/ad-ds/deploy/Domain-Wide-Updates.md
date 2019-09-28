---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Active Directory 域范围的架构更新
description: 升级域控制器时由 adprep/domainprep 执行的全域架构更新
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 28bb22c0f2dc70e899fe5ddfe232eacedfd400bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390758"
---
# <a name="domain-wide-schema-updates"></a>域范围的架构更新

>适用于：Windows Server

你可以查看以下更改集，以帮助了解并准备 Windows Server 中的 adprep/domainprep 执行的架构更新。

从 Windows Server 2012 开始，在 AD DS 安装过程中，将根据需要自动运行 Adprep 命令。 它们还可以在 AD DS 安装之前单独运行。 有关详细信息，请参阅[运行 Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx)。

有关如何解释访问控制项（ACE）字符串的详细信息，请参阅[ACE 字符串](https://msdn.microsoft.com/library/aa374928(VS.85).aspx)。 有关如何解释安全 ID （SID）字符串的详细信息，请参阅[SID 字符串](https://msdn.microsoft.com/library/aa379602(VS.85).aspx)。

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server （半年频道）：全域性更新

完成 Windows Server 2016 （操作89）中的 "**域**完成" 执行的操作之后，"Cn = 都，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain" 对象的 "**修订版本**" 属性设置为**16**.

|操作编号和 GUID|描述|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 89**： {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|删除向企业密钥管理员授予完全控制权限的 ACE，并添加 ACE 授予企业密钥管理员只需完全控制 msdsKeyCredentialLink 属性。|Delete （A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企业密钥管理员） <br /> <br />添加（OA;CIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;企业密钥管理员）|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016：全域性更新

完成 Windows Server 2016 （操作82-88）中的 "**域**完成" 执行的操作之后，"Cn = 都，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain" 对象的 "**修订版本**" 属性设置为**15。** .

|操作编号和 GUID|描述|特性|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 82**： {83C53DA7-427E-47A4-A07A-A324598B88F7}|在域的根目录创建 CN = Keys 容器|-objectClass：容器<br />2008密钥凭据对象的默认容器<br />ShowInAdvancedViewOnlyTRUE|的CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;中文<br />的CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;D的<br />的CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;SY<br />的CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;D2-D<br />的CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;ED-24000B|
|**操作 83**： {C81FC9CC-0130-4FD1-B272-634D74818133}|添加完全控制允许 "domain\Key Admins" 和 "rootdomain\Enterprise Key Admins" 的 "CN = Keys" 容器的 ace。|不可用|的CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;密钥管理员）<br />的CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企业密钥管理员）|
|**操作 84**： {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|修改 otherWellKnownObjects 属性，使其指向 CN = Keys 容器。|- otherWellKnownObjects:B:32：683A24E2E8164BD3AF86AC3C2CF3F981： CN = Keys，% ws|不可用|
|**操作 85**： {e6d5fd00-385d-4e65-b02d-9da3493ed850}|修改域 NC，以允许 "domain\Key Admins" 和 "rootdomain\Enterprise Key Admins" 修改 KeyCredentialLink 特性。 |不可用|OACIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;密钥管理员）<br />OACIRPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;;根域中的企业密钥管理员，但在非根域中，产生了具有不可解析-527 SID 的虚假域相对 ACE）|
|**操作 86**： {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|向 creator 所有者和自助授予 DS 验证的写入计算机车载|不可用|OACIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11d0-a285-00aa003049e2; PS）<br />OACIIO; SW; 9b026da6-0d3c-465c-8bee-5199d7165cba; bf967a86-0de6-11d0-a285-00aa003049e2; CO）|
|**操作 87**： {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|删除 ACE "向不正确的域相对企业密钥管理员组授予完全控制"，并向企业密钥管理员组添加 ACE "授予完全控制权限"。 |不可用|Delete （A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企业密钥管理员）<br /> <br />Add （A;CIRPWPCRLCLOCCDCRCWDWOSDDTSW;;;企业密钥管理员）|
|**操作 88**： {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|在域 NC 对象上添加 "ExpirePasswordsOnSmartCardOnlyAccounts" 并将默认值设置为 FALSE|不可用|不可用|

仅在 Windows Server 2016 域控制器升级后创建企业密钥管理员和密钥管理员组，并接管 PDC 仿真器 FSMO 角色。

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2：全域性更新

尽管 Windows Server 2012 **R2 中的**"完成" 操作不会执行任何操作，但在命令完成后，Cn = 都，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 对象的**修订**属性设置为**10。** .

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012：全域性更新

在 Windows Server 2012 中的 "**域**完成" 执行的操作（操作78、79、80和81）完成后，Cn = 都，Cn = DOMAINUPDATES，Cn = SYSTEM，DC = ForestRootDomain 对象的**修订**属性为设置为**9**。

|操作编号和 GUID|描述|特性|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 78**： {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|创建新的对象 CN = 域分区中的 TPM 设备。|对象类： Mstpm-ownerinformation-InformationObjectsContainer|不可用|
|**操作 79**： {54afcfb9-637a-4251-9f47-4d50e7021211}|创建了 TPM 服务的访问控制项。|不可用|OACIIO;WP; ea1b7b93-5e48-46d5-bc6c-4df4fda78a35; bf967a86-0de6-11d0-a285-00aa003049e2; PS）|
|**操作 80**： {f4728883-84dd-483c-9897-274f2ebcf11e}|向**可克隆域控制器**组授予 "克隆 DC" 的扩展权限|不可用|（OA;;CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;;*域 SID*-522）|
|**操作 81**： {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|在所有对象上向主体自助授予对等的、允许的、其他身份操作。|不可用|OACIOI;RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;PS|
