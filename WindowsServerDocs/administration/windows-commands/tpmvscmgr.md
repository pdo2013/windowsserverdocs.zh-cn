---
title: tpmvscmgr
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8b2c8ff4-5c5d-446d-99e7-4daa1b36a163
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f6cfc15b7c089c9b6ad4d9a267b951373e63a63a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888588"
---
# <a name="tpmvscmgr"></a>tpmvscmgr



Tpmvscmgr 命令行工具允许具有管理凭据的用户创建和删除的计算机上 TPM 虚拟智能卡。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

### <a name="parameters-for-create-command"></a>创建命令的参数

Create 命令设置新用户的系统上的虚拟智能卡。 如果删除操作是必需的它返回新创建的卡，以供日后参考的实例 ID。 实例 ID 的格式**ROOT\SMARTCARDREADER\000n**其中**n**从 0 开始，每次创建新的虚拟智能卡时增加 1。

|参数|描述|
|---------|-----------|
|/name|必需。 指示新的虚拟智能卡的名称。|
|/AdminKey|指示可用于重置卡的 PIN，如果用户忘记了 PIN 的所需的管理员密钥。</br>**默认**指定 010203040506070801020304050607080102030405060708 的默认值。</br>**提示**会提示用户输入管理员密钥的值。</br>**随机**卡不会返回给用户的管理员密钥的随机设置会导致。 这将创建的卡可能不能管理通过使用智能卡管理工具。 当使用随机生成，管理员密钥必须输入为 48 个十六进制字符。|
|/PIN|指示所需的用户的 PIN 值。</br>**默认**指定默认的 12345678 旋转中心点。</br>**提示**会提示用户输入的 PIN，在命令行。 PIN 必须至少 8 个字符，并且它可以包含数字、 字符和特殊字符。|
|/PUK|指示所需的 PIN 解锁密钥 (PUK) 值。 PUK 值必须为至少 8 个字符，并且可以包含数字、 字符和特殊字符。 如果省略该参数，则没有 PUK 的情况下创建卡片。</br>**默认**指定默认的 12345678 PUK。</br>**提示**向用户提示输入 PUK 在命令行。|
|/generate|在所需的虚拟智能卡对函数的存储中生成文件。 如果生成 / 省略参数，它相当于创建卡而无需此文件系统。 无文件系统的卡仅智能卡管理系统等 Microsoft Configuration Manager 管理。|
|/machine|可以指定远程计算机可以在其创建虚拟智能卡的名称。 这可在域环境中，并且它依赖于 DCOM。 若要成功地在不同计算机上创建虚拟智能卡命令，运行此命令的用户必须是远程计算机上的本地管理员组中的成员。|
|/?|此命令将显示的帮助。|

### <a name="parameters-for-destroy-command"></a>Destroy 命令的参数

Destroy 命令从用户的计算机上安全地删除虚拟智能卡。

> [!WARNING]
> 删除虚拟智能卡时，便无法恢复。

|参数|描述|
|---------|-----------|
|/instance|指定要删除虚拟智能卡的实例 ID。 实例 Id 作为输出由生成 Tpmvscmgr.exe 时创建卡。 /Instance 参数是 Destroy 命令的必填的字段。|
|/?|此命令将显示的帮助。|

## <a name="remarks"></a>备注

中的成员身份**管理员**组 （或等效） 在目标计算机上是最低要求运行此命令的所有参数。

对于字母数字输入，允许完整 127 ASCII 字符集。

## <a name="BKMK_Examples"></a>示例

下面的命令演示如何创建虚拟智能卡，可以通过从另一台计算机启动智能卡管理工具更高版本管理。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey DEFAULT /PIN PROMPT
```
或者，而不是使用默认管理员密钥，可以在命令行创建非管理员密钥。 下面的命令演示如何创建非管理员密钥。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey PROMPT /PIN PROMPT
```
以下命令将创建非托管的虚拟智能卡，可用于注册证书。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey RANDOM /PIN PROMPT /generate
```
以下命令将创建具有随机的管理员密钥虚拟智能卡。 创建 cardis 后将自动丢弃该密钥。 这意味着，如果用户忘记 PIN 或想要更改 PIN 时，用户需要删除卡，然后重新创建。 若要删除卡，用户可以运行以下命令。
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
其中\<实例 ID > 打印该值，在屏幕上时该用户创建卡片。 具体而言，对于创建第一张卡片，实例 ID 是 ROOT\SMARTCARDREADER\0000。

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)