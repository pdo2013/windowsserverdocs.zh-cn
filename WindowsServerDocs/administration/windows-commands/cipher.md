---
title: cipher
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 78ef795e-0f87-4acd-8d15-192c972c0f41
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d801d6e6286e97319766c879f7289f6191cc7101
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434332"
---
# <a name="cipher"></a>cipher



显示或更改 NTFS 卷上的目录和文件的加密。 如果使用不带参数，**密码**显示当前目录和其包含的所有文件的加密状态。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
cipher [/e | /d | /c] [/s:<Directory>] [/b] [/h] [PathName [...]]
cipher /k
cipher /r:<FileName> [/smartcard]
cipher /u [/n]
cipher /w:<Directory>
cipher /x[:efsfile] [FileName]
cipher /y
cipher /adduser [/certhash:<Hash> | /certfile:<FileName>] [/s:Directory] [/b] [/h] [PathName [...]]
cipher /removeuser /certhash:<Hash> [/s:<Directory>] [/b] [/h] [<PathName> [...]]
cipher /rekey [PathName [...]]
```

## <a name="parameters"></a>Parameters

|          Parameters           |                                                                                                                                                   描述                                                                                                                                                    |
|-------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|              /b               |                                                                                                    如果遇到错误时，中止。 默认情况下**密码**继续运行，即使遇到错误。                                                                                                    |
|              /c               |                                                                                                                                   在加密的文件上显示的信息。                                                                                                                                    |
|              /d               |                                                                                                                                   解密指定的文件或目录。                                                                                                                                   |
|              /e               |                                                                                          对指定的文件或目录进行加密。 目录被标记，以便以后添加的文件将被加密。                                                                                           |
|              /h               |                                                                                                     显示文件包含隐藏或系统属性。 默认情况下，这些文件未加密或解密。                                                                                                     |
|              /k               |                                                                            创建一个新的证书和密钥用于加密文件系统 (EFS) 文件。 如果 **/k**指定参数时，将忽略所有其他参数。                                                                            |
|  /r:\<FileName> [/smartcard]  |   生成一个 EFS 恢复代理密钥和证书，然后将其写入到.pfx 文件 （包含证书和私钥） 和.cer 文件 （包含仅的证书）。 如果 **/smartcard**指定，则它将恢复密钥和证书写入到智能卡，并不生成任何.pfx 文件。   |
|        /s:\<目录 >        |                                                                                                               执行指定的操作的所有子目录中指定*Directory*。                                                                                                               |
|            /u [/n]            |  查找本地驱动器上的所有加密的文件。 如果用于 **/n**参数，进行任何更新。 如果使用不带 **/n**， **/u**比较用户的文件加密密钥或恢复代理的密钥为当前的文件，并更新它们，如果已更改。 此参数仅适用于 **/n**。  |
|        /w:\<目录 >        | 从整个卷上的未使用的可用磁盘空间中删除数据。 如果您使用 **/w**参数，将忽略所有其他参数。 指定的目录可以是位于本地卷中的任何位置。 如果它是装入点，或指向的目录中另一个卷，将数据上卷会被删除。 |
|  /x[:efsfile] [\<FileName>]   |                                 将备份 EFS 证书和密钥到指定的文件名。 如果用于 **: efsfile**， **/x**备份用于加密该文件的用户的证书。 否则，用户的当前 EFS 证书和密钥备份。                                 |
|              /y               |                                                                                                                      在本地计算机上显示你当前的 EFS 证书的缩略图。                                                                                                                      |
|  /adduser [/ certhash:\<哈希 >  |                                                                                                                                              /certfile:<FileName>]                                                                                                                                               |
|            /rekey             |                                                                                                                 更新要使用当前配置的 EFS 密钥的指定加密的文件。                                                                                                                 |
| /removeuser /certhash:\<哈希 > |                                                                                       从指定文件中删除用户。 *哈希*供 **/certhash**必须是要删除的证书的 SHA1 哈希。                                                                                       |
|              /?               |                                                                                                                                       在命令提示符下显示帮助。                                                                                                                                       |

## <a name="remarks"></a>备注

-   如果未加密的父目录，它被修改时，加密的文件无法成为对解密。 因此，在加密文件时，您还应加密的父目录。
-   管理员可以将.cer 文件的内容添加到 EFS 恢复策略，若要创建用户，恢复代理，然后导入.pfx 文件以恢复单个文件。
-   可以使用多个目录的名称和通配符。
-   您必须将多个参数之间的空格。

## <a name="BKMK_examples"></a>示例

若要显示当前目录中的每个文件和子目录的加密状态，请键入：
```
cipher
```
加密的文件和目录标有**E**。使用标记为未加密的文件和目录**U**。例如，下面的输出指示当前目录及其所有内容是当前未加密：
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```
若要启用对上一示例中使用的专用目录进行加密，请键入：
```
cipher /e private
```
将显示以下输出：
```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```
**密码**命令将显示以下输出：
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```
请注意，标记的专用目录进行加密。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)