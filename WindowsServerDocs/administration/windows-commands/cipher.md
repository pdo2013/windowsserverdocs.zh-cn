---
title: cipher
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 7ba6a54c275e1765bfdc31fe30d78fc6e3da6c05
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379385"
---
# <a name="cipher"></a>cipher



显示或更改 NTFS 卷上的目录和文件的加密。 如果在没有参数的情况下使用，则**cipher**将显示当前目录及其包含的所有文件的加密状态。

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
|              /b               |                                                                                                    如果遇到错误，则中止。 默认情况下，即使遇到错误，**密码**也会继续运行。                                                                                                    |
|              /c               |                                                                                                                                   显示有关加密文件的信息。                                                                                                                                    |
|              /d               |                                                                                                                                   解密指定的文件或目录。                                                                                                                                   |
|              /e               |                                                                                          加密指定的文件或目录。 将对目录进行标记，以便对以后添加的文件进行加密。                                                                                           |
|              /h               |                                                                                                     显示具有隐藏或系统属性的文件。 默认情况下，这些文件不会进行加密或解密。                                                                                                     |
|              遇到               |                                                                            创建新的证书和密钥以与加密文件系统（EFS）文件一起使用。 如果指定了 **/k**参数，将忽略所有其他参数。                                                                            |
|  /r： \<FileName > [/smartcard]  |   生成一个 EFS 恢复代理密钥和证书，然后将其写入到 .pfx 文件（包含证书和私钥）和 .cer 文件（只包含证书）。 如果指定了 **/smartcard** ，则它会将恢复密钥和证书写入智能卡，并且不生成 .pfx 文件。   |
|        /s： \<Directory >        |                                                                                                               在指定*目录*中的所有子目录上执行指定的操作。                                                                                                               |
|            /u [/n]            |  查找本地驱动器上的所有加密文件。 如果与 **/n**参数一起使用，则不会进行更新。 如果使用时没有 **/n**， **/u**会将用户的文件加密密钥或恢复代理的密钥与当前密钥进行比较，并在更改后对其进行更新。 此参数仅适用于 **/n**。  |
|        /w： \<Directory >        | 删除整个卷上的可用磁盘空间中的数据。 如果使用 **/w**参数，则将忽略所有其他参数。 指定的目录可以位于本地卷中的任意位置。 如果它是装入点或指向另一卷中的某个目录，则将删除该卷上的数据。 |
|  /x [： efsfile] [\<FileName >]   |                                 将 EFS 证书和密钥备份到指定的文件名。 如果与 **： efsfile**一起使用， **/x**将备份用于加密该文件的用户的证书。 否则，将备份用户的当前 EFS 证书和密钥。                                 |
|              /y               |                                                                                                                      显示本地计算机上的当前 EFS 证书缩略图。                                                                                                                      |
|  /adduser [/certhash： \<Hash >  |                                                                                                                                              /certfile： <FileName>]                                                                                                                                               |
|            /rekey             |                                                                                                                 更新指定的加密文件以使用当前配置的 EFS 密钥。                                                                                                                 |
| /removeuser/certhash： \<Hash > |                                                                                       从指定的文件中删除用户。 为 **/certhash**提供的*哈希*必须是要删除的证书的 SHA1 哈希。                                                                                       |
|              /?               |                                                                                                                                       在命令提示符下显示帮助。                                                                                                                                       |

## <a name="remarks"></a>备注

-   如果未加密父目录，则加密的文件在修改后可能会被解密。 因此，在对文件进行加密时，还应加密父目录。
-   管理员可以将 .cer 文件的内容添加到 EFS 恢复策略中，为用户创建恢复代理，然后导入该 .pfx 文件以恢复单个文件。
-   可以使用多个目录名称和通配符。
-   必须在多个参数之间输入空格。

## <a name="BKMK_examples"></a>示例

若要显示当前目录中每个文件和子目录的加密状态，请键入：
```
cipher
```
加密的文件和目录用**E**进行标记。未加密的文件和目录用**U**标记。例如，下面的输出指示当前目录及其所有内容当前未加密：
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
U Private
U hello.doc
U hello.txt
```
若要在前面的示例中使用的专用目录上启用加密，请键入：
```
cipher /e private
```
以下输出显示：
```
Encrypting files in C:\Users\MainUser\Documents\
Private             [OK]
1 file(s) [or directorie(s)] within 1 directorie(s) were encrypted.
```
**Cipher**命令显示以下输出：
```
Listing C:\Users\MainUser\Documents\
New files added to this directory will not be encrypted.
E Private
U hello.doc
U hello.txt
```
请注意，专用目录已标记为已加密。

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)