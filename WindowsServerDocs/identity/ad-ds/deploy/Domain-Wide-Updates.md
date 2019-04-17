---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: "Active Directory 整个域架构更新"
description: "由 adprep 整个域架构更新 /domainprep 当提升域控制器"
author: MicrosoftGuyJFlo
ms.author: joflore
manager: femila
ms.date: 07/14/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 59efb7c854b7f3693776bf9a1a371f98c2206d60
ms.sourcegitcommit: 79c1359232bece2e5c3ee5507f1e4bf19b44f487
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/19/2018
---
# <a name="domain-wide-schema-updates"></a>全域架构更新

>适用于：Windows Server 2016，Windows Server 2012 R2、Windows Server 2012

你可以查看更改，以帮助您了解和由 adprep 方案更新准备下列一组 /domainprep 在 Windows Server。 

开始在 Windows Server 2012，自动广告 DS 安装期间需要运行 Adprep 命令。 它们也可以运行单独提前广告 DS 安装。 有关详细信息，请参阅[运行 Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx)。

有关如何解释访问控制 (ACE) 条目字符串的详细信息，请参阅[ACE 字符串](https://msdn.microsoft.com/library/aa374928(VS.85).aspx)。 有关如何解释安全标识符字符串的详细信息，请参阅[SID 字符串](https://msdn.microsoft.com/library/aa379602(VS.85).aspx)。

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server（半年通道）：域范围更新

只需执行的操作后**域**Windows Server 2016（操作 89）完成之后，**修订**CN 的特性 =ActiveDirectoryUpdate、CN = DomainUpdates，CN = 系统特区 = 的 ForestRootDomain 对象设置为**16**。

|操作号和 GUID|描述|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|删除完全控制授予企业键管理员 A 并添加 A 授予企业键管理员完全控制只需 msdsKeyCredentialLink 属性。|删除。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企业关键管理员） <br /> <br />添加 (OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;企业关键管理员）|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016：域范围更新

只需执行的操作后**域**Windows Server 2016（操作 82 88）完成之后，**修订**CN 的特性 = ActiveDirectoryUpdate，CN = DomainUpdates，CN = 系统特区 = 的 ForestRootDomain 对象设置为**15**。

|操作号和 GUID|描述|属性|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 82**: {83C53DA7-427E-47A4-A07A-A324598B88F7}|创建 CN = 根部域键容器|-对象类：容器<br />-描述：默认容器关键凭据对象<br />-ShowInAdvancedViewOnly：如此|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;EA)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;; D)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;SY)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;; DD)<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;ED)|
|**操作 83**: {C81FC9CC-0130-4FD1-B272-634D74818133}|添加完全控制允许到 CN ace = 键用于"domain\Key 管理员"和"rootdomain\Enterprise 管理员密钥"。|不适用|(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;关键管理员）<br />(A;CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企业关键管理员）|
|**操作 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|修改 otherWellKnownObjects 特性指向 CN = 键容器。|-otherWellKnownObjects: B:32:683A24E2E8164BD3AF86AC3C2CF3F981:CN = 键，%ws|不适用|
|**操作 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|修改域 NC 允许"domain\Key 管理员"和"rootdomain\Enterprise 键管理员"修改 msds KeyCredentialLink 属性。 |不适用|(OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;关键管理员）<br />(OA;CI;RPWP; 5b47d60f-6090-40b2-9f37-2a4de88f3063;企业键管理员根域中，但在非根域导致与非解析-527 SID 虚假域相对 ACE）|
|**操作 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|创建者所有者和自行授予 DS 验证-写入-计算机汽车|不适用|(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;PS)<br />(OA;CIIO;SW;9b026da6-0d3c-465c-8bee-5199d7165cba;bf967a86-0de6-11d0-a285-00aa003049e2;CO)|
|**操作 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|删除 A 完全控制授予错误域相对企业键管理员组中，并添加完全控制授予企业键管理员组 A。 |不适用|删除。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企业关键管理员）<br /> <br />添加。CI;RPWPCRLCLOCCDCRCWDWOSDDTSW;;企业关键管理员）|
|**操作 88**: {434bb40d-dbc9-4fe7-81d4-d57229f7b080}|添加"msDS ExpirePasswordsOnSmartCardOnlyAccounts"上的域 NC 对象和设置为默认值|不适用|不适用|

Windows Server 2016 域控制器提升和接管 PDC 仿真器 FSMO 角色后仅创建企业键管理员和键管理员的组。

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012R2：域范围更新

尽管不会执行操作的**域**Windows Server 2012 R2、命令完成后，在**修订**CN 的特性 = ActiveDirectoryUpdate，CN = DomainUpdates，CN = 系统特区 = 的 ForestRootDomain 对象设置为**10**。

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012：域范围更新

只需执行的操作后**域**Windows Server 2012（操作 78、79、80%和 81）完成之后，在**修订**CN 的特性 = ActiveDirectoryUpdate，CN = DomainUpdates，CN = 系统特区 = 的 ForestRootDomain 对象设置为**9**。

|操作号和 GUID|描述|属性|权限|
|------------------------------|---------------|--------------|---------------|
|**操作 78**: {c3c927a6-cc1d-47c0-966b-be8f9b63d991}|创建新的对象 CN = TPM 设备域分区中。|对象类：msTPM InformationObjectsContainer|不适用|
|**操作 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|创建 TPM 服务的访问权限控制项。|不适用|(OA;CIIO;WP;ea1b7b93-5e48-46d5-bc6c-4df4fda78a35;bf967a86-0de6-11d0-a285-00aa003049e2;PS)|
|**操作 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|授予扩展直接向"克隆 DC"**克隆域控制器**组|不适用|(OA;CR; 3e0f7e18-2c7a-4c10-ba82-4d926db99a3e;*域 SID*-522)|
|**操作 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|所有对象 MS-DS-Allowed-To-Act-On-Behalf-Of-Other-Identity 授予主体自身。|不适用|(OA;CIOI;RPWP; 3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;PS)|
