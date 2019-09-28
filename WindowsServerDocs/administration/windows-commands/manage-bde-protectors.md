---
title: manage-bde 保护程序
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f9b22c5-cc93-45df-9165-bedee94998da
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/06/2018
ms.openlocfilehash: 7b25f6fe3c8a067d843fc12e9c1d1955a9606c09
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373939"
---
# <a name="manage-bde-protectors"></a>manage-bde：保护程序

>适用于：Windows Server（半年频道）、Windows Server 2016

管理用于 BitLocker 加密密钥的保护方法。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。
## <a name="syntax"></a>语法
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Parameters

|   参数   |                                                                                                                                                                                           描述                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     -get      |                                                                                                                                            显示在驱动器上启用的所有密钥保护方法，并提供其类型和标识符（ID）。                                                                                                                                             |
|     -添加      |                                                                                                                                   添加使用其他[add 参数](manage-bde-protectors.md#BKMK_addprotectors)指定的密钥保护方法。                                                                                                                                    |
|    -delete    | 删除 BitLocker 使用的密钥保护方法。 除非使用可选[-delete 参数](manage-bde-protectors.md#BKMK_deleteprotectors)指定要删除的保护程序，否则将从驱动器中删除所有密钥保护程序。 删除驱动器上的最后一个保护程序时，将禁用驱动器的 BitLocker 保护，以确保不会无意中丢失对数据的访问权限。 |
|   -disable    |                      禁用保护，使任何人都可以通过使加密密钥在驱动器上不受保护的加密密钥来访问加密的数据。 不会删除任何密钥保护程序。 将在下次启动 Windows 时恢复保护，除非使用可选[的-disable 参数](manage-bde-protectors.md#BKMK_disableprot)指定重启计数。                       |
|    -enable    |                                                                                                                             通过从驱动器中删除不安全的加密密钥来启用保护。 将强制实施驱动器上的所有已配置的密钥保护程序。                                                                                                                             |
|   -adbackup   |                                                                          备份指定的驱动器的所有恢复信息 Active Directory 域服务（AD DS）。 若要仅备份一个恢复密钥以便 AD DS，请附加 **-id**参数，并指定要备份的特定恢复密钥的 id。                                                                           |
|  -aadbackup   |                                                                            备份指定到 Azure Active Directory （Azure Ad）的驱动器的所有恢复信息。 若要仅备份一个恢复密钥以便 Azure AD，请附加 **-id**参数，并指定要备份的特定恢复密钥的 id。                                                                             |
|    <Drive>    |                                                                                                                                                                          表示驱动器号后跟一个冒号。                                                                                                                                                                          |
| -computername |                                                                                                              指定 manage-bde.exe 将用于修改另一台计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。                                                                                                              |
|    <Name>     |                                                                                                                 表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。                                                                                                                  |
|   -? 或 /?    |                                                                                                                                                                            在命令提示符下显示 brief help。                                                                                                                                                                            |
|  -help 或-h  |                                                                                                                                                                          在命令提示符下显示完整的帮助。                                                                                                                                                                           |

### <a name="BKMK_addprotectors"></a>-添加语法和参数
```
manage-bde  protectors  add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

|          参数           |                                                                                                                                                                                   描述                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           <Drive>            |                                                                                                                                                                 表示驱动器号后跟一个冒号。                                                                                                                                                                  |
|      -ms-fve-recoverypassword       |                                                                                                                                    添加数字密码保护程序。 你还可以使用 **-rp**作为此命令的缩写形式。                                                                                                                                     |
|     <NumericalPassword>      |                                                                                                                                                                        表示恢复密码。                                                                                                                                                                        |
|         -recoverykey         |                                                                                                                                添加用于恢复的外部密钥保护程序。 你还可以使用 **-rk**作为此命令的缩写形式。                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                               表示恢复密钥的目录路径。                                                                                                                                                                |
|         -启动          |                                                                                                                                 添加用于启动的外部密钥保护程序。 你还可以使用 **-sk**作为此命令的缩写形式。                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                                表示启动密钥的目录路径。                                                                                                                                                                |
|         -证书         |                                                                                                                               为数据驱动器添加公钥保护程序。 你还可以使用 **-cert**作为此命令的缩写形式。                                                                                                                               |
|             -cf              |                                                                                                                                              指定将用于提供公钥证书的证书文件。                                                                                                                                              |
|   <pathToCertificateFile>    |                                                                                                                                                             表示证书文件的目录路径。                                                                                                                                                              |
|             -ct              |                                                                                                                                           指定将使用证书指纹来标识公钥证书                                                                                                                                           |
|   <CertificateThumbprint>    |                                                       指定要使用的证书的指纹属性的值。 例如，证书指纹值 "a9 09 50 2d d8 2a e4 14 33 e6 f8 38 86 b0 0d 42 77 a3 2a 7b" 应指定为 "a909502dd82ae41433e6f83886b00d4277a32a7b"。                                                        |
|          -tpmandpin          |                                                                                           为操作系统驱动器添加受信任的平台模块（TPM）和个人标识号（PIN）保护程序。 你还可以使用 **-tp**作为此命令的缩写形式。                                                                                           |
|      -tpmandstartupkey       |                                                                                                                    添加操作系统驱动器的 TPM 和启动密钥保护程序。 你还可以使用 **-tsk**作为此命令的缩写形式。                                                                                                                    |
|   -tpmandpinandstartupkey    |                                                                                                                为操作系统驱动器添加 TPM、PIN 和启动密钥保护程序。 你还可以使用 **-tpsk**作为此命令的缩写形式。                                                                                                                 |
|          -password           |                                                                                                                              添加数据驱动器的密码密钥保护程序。 你还可以使用 **-pw**作为此命令的缩写形式。                                                                                                                              |
|      -adaccountorgroup       | 为卷添加基于安全标识符（SID）的标识保护程序。  你还可以使用 **-sid**作为此命令的缩写形式。 **重要提示：** 默认情况下，不能使用 WMI 或 manage-bde 远程添加 ADAccountOrGroup 保护程序。  如果你的部署需要能够远程添加此保护程序，则必须启用约束委派。 |
|        -computername         |                                                                                                       指定正在使用 manage-bde 修改其他计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。                                                                                                       |
|            <Name>            |                                                                                                         表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。                                                                                                         |

### <a name="BKMK_deleteprotectors"></a>-delete 语法和 parameters
```
manage-bde  protectors  delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|       参数        |                                                                              描述                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        <Drive>         |                                                             表示驱动器号后跟一个冒号。                                                             |
|         -type          |                               标识要删除的密钥保护程序。 你还可以使用 **-t**作为此命令的缩写形式。                               |
|    ms-fve-recoverypassword    |                                                 指定应删除任何恢复密码密钥保护程序。                                                 |
|      externalkey       |                                        指定应删除与驱动器关联的任何外部密钥保护程序。                                         |
|      证书       |                                       指定应删除与驱动器关联的任何证书密钥保护程序。                                       |
|          tpm           |                                        指定应删除与驱动器关联的任何仅 TPM 密钥保护程序。                                         |
|    tpmandstartupkey    |                                指定应删除与驱动器关联的任何基于 TPM 和启动密钥保护程序的密钥保护程序。                                |
|       tpmandpin        |                                    指定应删除与驱动器关联的任何基于 TPM 和 PIN 的密钥保护程序。                                    |
| tpmandpinandstartupkey |                             指定应删除与驱动器关联的任何基于 TPM、PIN 和启动密钥保护程序的密钥保护程序。                             |
|        password        |                                        指定应删除与驱动器关联的任何密码密钥保护程序。                                         |
|        标识        |                                        指定应删除与驱动器关联的任何标识密钥保护程序。                                         |
|          -id           |                使用密钥标识符标识要删除的密钥保护程序。 此参数是 **-type**参数的替代选项。                 |
|    <KeyProtectorID>    |        标识要删除的驱动器上的单个密钥保护程序。 可以通过使用**manage-bde-保护程序-get**命令显示密钥保护程序 id。         |
|     -computername      | 指定 manage-bde.exe 将用于修改另一台计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。 |
|         <Name>         |    表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。     |
|        -? 或 /?        |                                                               在命令提示符下显示 brief help。                                                               |
|      -help 或-h       |                                                             在命令提示符下显示完整的帮助。                                                              |

### <a name="BKMK_disableprot"></a>-禁用语法和参数
```
manage-bde  protectors  disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|   参数   |                                                                                                                                                                                                                   描述                                                                                                                                                                                                                    |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Drive>    |                                                                                                                                                                                                  表示驱动器号后跟一个冒号。                                                                                                                                                                                                  |
|  RebootCount  | 从 Windows 8 开始，指定对操作系统卷的保护已挂起，并将在重新启动 Windows 后恢复 RebootCount 参数中指定的次数。 指定0则无限期挂起保护。 如果未指定此参数，则重新启动 Windows 时，BitLocker 保护将自动恢复。 还可以使用 **-rc**作为此命令的缩写形式。 |
| -computername |                                                                                                                                      指定 manage-bde.exe 将用于修改另一台计算机上的 BitLocker 保护。 你还可以使用 **-cn**作为此命令的缩写形式。                                                                                                                                      |
|    <Name>     |                                                                                                                                         表示要修改 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。                                                                                                                                          |
|   -? 或 /?    |                                                                                                                                                                                                    在命令提示符下显示 brief help。                                                                                                                                                                                                    |
|  -help 或-h  |                                                                                                                                                                                                  在命令提示符下显示完整的帮助。                                                                                                                                                                                                   |

## <a name="BKMK_Examples"></a>示例
以下示例演示了如何使用 **-保护程序**命令将证书文件标识的证书密钥保护程序添加到驱动器 E。
```
manage-bde  protectors  add E: -certificate  cf "c:\File Folder\Filename.cer"
```
下面的示例演示如何使用 **-保护程序**命令将由域和用户名标识的**adaccountorgroup**密钥保护程序添加到驱动器 E。
```
manage-bde  protectors  add E: -sid DOMAIN\user
```
以下示例说明了如何使用**保护**程序命令禁用保护，直到计算机重新启动3次。
```
manage-bde  protectors  disable C: -rc 3
```
下面的示例演示如何使用 **-保护程序**命令删除驱动器 C 上的所有基于 TPM 和启动密钥的保护程序。
```
manage-bde  protectors  delete C: -type tpmandstartupkey
```
下面的示例演示如何使用 **-保护程序**命令将驱动器 C 的所有恢复信息备份到 AD DS。
```
manage-bde  protectors  adbackup C:
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
