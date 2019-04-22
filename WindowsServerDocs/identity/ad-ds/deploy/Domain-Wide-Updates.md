---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Active Directory 全域性架构更新
description: 升级域控制器时，adprep /domainprep 所执行的全域性架构更新
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 20598431b9c2376051247fd7ba14e33af00968cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812318"
---
# <a name="domain-wide-schema-updates"></a>全域性架构更新

>适用于：Windows Server

可以查看以下一系列更改，以帮助理解及准备对 Windows Server 中的 adprep /domainprep 执行架构更新。

从 Windows Server 2012 开始，将根据需要在 AD DS 安装过程中自动运行 Adprep 命令。 它们也可以运行单独安装 AD DS 之前。 有关详细信息，请参阅[运行 Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx)。

有关如何解释访问控制项 (ACE) 字符串的详细信息，请参阅[ACE 字符串](https://msdn.microsoft.com/library/aa374928(VS.85).aspx)。 有关如何解释安全 ID (SID) 字符串的详细信息，请参阅[SID 字符串](https://msdn.microsoft.com/library/aa379602(VS.85).aspx)。

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server （半年频道）：域范围更新

执行的操作后， **domainprep**在 Windows Server 2016 （操作 89） 完成时，**修订**属性 CN = ActiveDirectoryUpdate，CN = DomainUpdates，CN = System，DC =ForestRootDomain 对象设置为**16**。

|操作数量和 GUID|描述|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|删除 ACE 授予完全控制对企业密钥管理权限并添加 ACE 授予企业密钥管理员对只是 msdsKeyCredentialLink 属性的完全控制。|删除 (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企业密钥管理） <br /> <br />添加 (OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;企业密钥管理）|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016:域范围更新

执行的操作后， **domainprep**在 Windows Server 2016 （操作 82 88） 完成时，**修订**属性 CN = ActiveDirectoryUpdate，CN = DomainUpdates，CN = System，DC = 的 ForestRootDomain 对象设置为**15**。

|操作数量和 GUID|描述|特性|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|创建 CN = 根域的密钥容器|-objectClass： 容器<br />-说明：密钥凭据对象的默认容器<br />-ShowInAdvancedViewOnly:TRUE|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;EA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;SY)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;DD)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;;ED)|
|**操作 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|添加完全控制允许 ace 到 CN ="domain\Key 管理员"的密钥容器和"rootdomain\Enterprise 密钥管理"。|不可用|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;密钥管理员）<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企业密钥管理）|
|**操作 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|修改 otherWellKnownObjects 属性以指向 CN = 密钥容器。|-otherWellKnownObjects:B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN=Keys,%ws|不可用|
|**操作 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|修改域 NC 为允许"domain\Key 管理员"和"rootdomain\Enterprise 密钥管理"来修改 Msds-keycredentiallink 属性。 |不可用|(OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;密钥管理员）<br />(OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;企业密钥管理在根域，但非根域中导致虚假的域相对 ACE 具有不可解析的-527 SID）|
|**操作 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|向创建者所有者和自助授予 DS-验证-写入-计算机汽车|不可用|(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;CO)|
|**操作 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|删除 ACE 授予完全控制权限到不正确的域相对企业密钥管理组，并添加到企业密钥管理组授予完全控制的 ACE。 |不可用|删除 (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企业密钥管理）<br /> <br />添加 (A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企业密钥管理）|
|**操作 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|添加域 NC 对象上的"msDS ExpirePasswordsOnSmartCardOnlyAccounts"并将默认值设置为 FALSE|不可用|不可用|

Windows Server 2016 域控制器被提升，并将接管 PDC 模拟器 FSMO 角色后，仅创建企业密钥管理和密钥管理组。

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2：域范围更新

尽管任何操作都通过不执行**domainprep**在 Windows Server 2012 R2，完成该命令后,**修订**CN 属性 = ActiveDirectoryUpdate，CN = DomainUpdates，CN = System，DC = 的 ForestRootDomain 对象设置为**10**。

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012:域范围更新

执行的操作后， **domainprep**在 Windows Server 2012 (operations 78、 79、 80 以及 81) 完成时，**修订**属性 CN = ActiveDirectoryUpdate，CN = DomainUpdates，CN = System，DC = 的 ForestRootDomain 对象设置为**9**。

|操作数量和 GUID|描述|特性|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|创建新对象 CN = 域分区中的 TPM 设备。|对象类： msTPM InformationObjectsContainer|不可用|
|**操作 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|创建的 TPM 服务的访问控制项。|不可用|(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**操作 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|授予"克隆 DC"从右到扩展**Cloneable Domain Controllers**组|不可用|(OA;CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;*域 SID*-522)|
|**操作 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|授予 ms-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity 主体自身上的所有对象。|不可用|(OA;CIOI;RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;;PS)|
