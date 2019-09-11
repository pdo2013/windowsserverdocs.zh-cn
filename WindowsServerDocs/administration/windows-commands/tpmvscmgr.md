---
title: tpmvscmgr
description: '适用于 * * * * 的 Windows 命令主题 '
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
ms.openlocfilehash: 9b1d9b049615322bffc39b5b372ce145579b57b2
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70868767"
---
# <a name="tpmvscmgr"></a>tpmvscmgr



使用 Tpmvscmgr 命令行工具，具有管理凭据的用户可以在计算机上创建和删除 TPM 虚拟智能卡。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Tpmvscmgr create [/name] [/AdminKey DEFAULT | PROMPT | RANDOM] [/PIN DEFAULT | PROMPT] [/PUK DEFAULT | PROMPT] [/generate] [/machine] [/?]
```
```
Tpmvscmgr destroy [/instance <instance ID>] [/?]
```

### <a name="parameters-for-create-command"></a>Create 命令的参数

Create 命令在用户的系统上设置新的虚拟智能卡。 如果需要删除，它将返回新创建的卡片的实例 ID，以便以后引用。 实例 ID 的格式为**ROOT\SMARTCARDREADER\000n** ，其中**n**从0开始，每次创建新的虚拟智能卡时增加1。

|参数|描述|
|---------|-----------|
|/name|必需。 指示新虚拟智能卡的名称。|
|/AdminKey|指示在用户忘记密码时可用于重置卡的 PIN 的所需管理员密钥。</br>**默认值**指定010203040506070801020304050607080102030405060708的默认值。</br>**提示符**提示用户输入管理员密钥的值。</br>**随机**为不返回给用户的卡的管理员密钥产生随机设置。 这会创建可能无法通过使用智能卡管理工具进行管理的卡。 如果随机生成，则必须输入48的十六进制字符。|
|/PIN|指示所需的用户 PIN 值。</br>**默认值**指定12345678的默认 PIN。</br>**提示符**在命令行提示用户输入 PIN。 PIN 必须至少为8个字符，并且可以包含数字、字符和特殊字符。|
|/PUK|指示所需的 PIN 解锁密钥（PUK）值。 PUK 值必须至少为8个字符，并且可以包含数字、字符和特殊字符。 如果省略该参数，则会在不使用 PUK 的情况下创建卡。</br>**默认值**指定默认的 PUK 为12345678。</br>**提示符**提示用户在命令行中输入 PUK。|
|/generate|在存储中生成虚拟智能卡正常运行所必需的文件。 如果省略/generate 参数，则它等效于在没有此文件系统的情况下创建卡。 只有智能卡管理系统（如 Microsoft Configuration Manager）才能管理没有文件系统的卡。|
|/machine|允许您指定可在其上创建虚拟智能卡的远程计算机的名称。 这只能在域环境中使用，并且依赖于 DCOM。 要使该命令成功地在另一台计算机上创建虚拟智能卡，运行此命令的用户必须是远程计算机上的本地管理员组中的成员。|
|/?|显示此命令的帮助。|

### <a name="parameters-for-destroy-command"></a>用于销毁命令的参数

销毁命令从用户的计算机安全地删除虚拟智能卡。

> [!WARNING]
> 删除虚拟智能卡后，将无法恢复。

|参数|描述|
|---------|-----------|
|/instance|指定要删除的虚拟智能卡的实例 ID。 创建卡时，instanceID 作为 Tpmvscmgr 的输出生成。 /Instance 参数是销毁命令的必填字段。|
|/?|显示此命令的帮助。|

## <a name="remarks"></a>备注

在目标计算机上， **Administrators**组中的成员身份至少是运行此命令的所有参数所必需的。

对于字母数字输入，允许使用完整的127字符 ASCII 集。

## <a name="BKMK_Examples"></a>示例

以下命令显示了如何创建一个虚拟智能卡，以后可以通过从另一台计算机启动的智能卡管理工具管理该智能卡。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey DEFAULT /PIN PROMPT
```
或者，你可以在命令行中创建管理员密钥，而不是使用默认的管理员密钥。 以下命令显示了如何创建管理员密钥。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey PROMPT /PIN PROMPT
```
以下命令将创建可用于注册证书的非托管虚拟智能卡。
```
tpmvscmgr.exe create /name "VirtualSmartCardForCorpAccess" /AdminKey RANDOM /PIN PROMPT /generate
```
以下命令将创建具有随机管理员密钥的虚拟智能卡。 创建 cardis 后，该键会自动丢弃。 这意味着，如果用户忘记了 PIN，或想要更改 PIN，则用户需要删除卡并再次创建。 若要删除卡，用户可以运行以下命令。
```
tpmvscmgr.exe destroy /instance <instance ID> 
```
其中\<实例 ID > 是用户创建卡时在屏幕上打印的值。 具体而言，对于创建的第一个卡，实例 ID 为 ROOT\SMARTCARDREADER\0000。

#### <a name="additional-references"></a>其他参考

-   [命令行语法项](command-line-syntax-key.md)