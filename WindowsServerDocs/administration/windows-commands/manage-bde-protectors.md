---
title: 管理 bde 保护程序
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78c0b338aa34684cc3c5da5ae4493f3526537ee6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437506"
---
# <a name="manage-bde-protectors"></a>管理 bde： 保护程序

>适用于：Windows 服务器 （半年频道），Windows Server 2016

管理用于 BitLocker 加密密钥的保护方法。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。
## <a name="syntax"></a>语法
```
manage-bde -protectors [{-get|-add|-delete|-disable|-enable|-adbackup|-aadbackup}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```
### <a name="parameters"></a>Parameters

|   参数   |                                                                                                                                                                                           描述                                                                                                                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     -get      |                                                                                                                                            显示在驱动器上启用的所有密钥保护方法并提供它们的类型和标识符 (ID)。                                                                                                                                             |
|     -add      |                                                                                                                                   添加密钥保护方法操作，指定通过使用其他[将参数添加](manage-bde-protectors.md#BKMK_addprotectors)。                                                                                                                                    |
|    -delete    | 删除使用的 BitLocker 密钥保护方法。 将从驱动器删除所有密钥保护程序，除非可选[-删除参数](manage-bde-protectors.md#BKMK_deleteprotectors)用来指定要删除的保护程序。 删除的驱动器上的最后一个保护程序时，禁用驱动器的 BitLocker 保护以确保对数据的访问不是丢失无意中。 |
|   -disable    |                      禁用保护，这将允许任何人都可以访问加密的数据提供加密密钥进行不安全的驱动器上。 不删除任何密钥保护程序。 保护将继续下一次启动 Windows 时，除非可选[-禁用参数](manage-bde-protectors.md#BKMK_disableprot)用来指定重启计数。                       |
|    -enable    |                                                                                                                             通过从驱动器删除不安全的加密密钥启用保护。 将强制执行所有配置密钥保护程序的驱动器上。                                                                                                                             |
|   -adbackup   |                                                                          备份到 Active Directory 域服务 (AD DS) 中指定的驱动器的所有恢复信息。 若要仅单个恢复密钥备份到 AD DS，请追加 **-id**参数并指定特定的恢复密钥备份的 ID。                                                                           |
|  -aadbackup   |                                                                            备份到 Azure Active Directory (Azure Ad) 指定的驱动器的所有恢复信息。 若要备份到 Azure AD 的单个恢复密钥，请追加 **-id**参数并指定特定的恢复密钥备份的 ID。                                                                             |
|    <Drive>    |                                                                                                                                                                          表示驱动器号后, 接一个冒号。                                                                                                                                                                          |
| -computername |                                                                                                              指定将使用该管理 bde.exe 修改另一台计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。                                                                                                              |
|    <Name>     |                                                                                                                 表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。                                                                                                                  |
|   -? 或 /?    |                                                                                                                                                                            显示简短的命令提示符处的帮助。                                                                                                                                                                            |
|  -help 或-h  |                                                                                                                                                                          显示完整的命令提示符处的帮助。                                                                                                                                                                           |

### <a name="BKMK_addprotectors"></a>-添加语法和参数
```
manage-bde  protectors  add [<Drive>] [-forceupgrade] [-recoverypassword <NumericalPassword>] [-recoverykey <pathToExternalKeydirectory>]
[-startupkey <pathToExternalKeydirectory>] [-certificate {-cf <pathToCertificateFile>|-ct <CertificateThumbprint>}] [-tpm] [-tpmandpin] 
[-tpmandstartupkey <pathToExternalKeydirectory>] [-tpmandpinandstartupkey <pathToExternalKeydirectory>] [-password][-adaccountorgroup <securityidentifier> [-computername <Name>] 
[{-?|/?}] [{-help|-h}]
```

|          参数           |                                                                                                                                                                                   描述                                                                                                                                                                                   |
|------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           <Drive>            |                                                                                                                                                                 表示驱动器号后, 接一个冒号。                                                                                                                                                                  |
|      -recoverypassword       |                                                                                                                                    添加数字密码保护程序。 此外可以使用 **-rp**作为此命令的简化版本。                                                                                                                                     |
|     <NumericalPassword>      |                                                                                                                                                                        表示恢复密码。                                                                                                                                                                        |
|         -recoverykey         |                                                                                                                                添加外部密钥保护程序进行恢复。 此外可以使用 **-rk**作为此命令的简化版本。                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                               表示恢复密钥的目录路径。                                                                                                                                                                |
|         -startupkey          |                                                                                                                                 添加启动外部密钥保护程序。 此外可以使用 **-sk**作为此命令的简化版本。                                                                                                                                 |
| <pathToExternalKeydirectory> |                                                                                                                                                                表示启动密钥的目录路径。                                                                                                                                                                |
|         -certificate         |                                                                                                                               添加对数据驱动器的密钥保护程序。 此外可以使用 **-证书**作为此命令的简化版本。                                                                                                                               |
|             -cf              |                                                                                                                                              指定证书文件将用于提供的公钥证书。                                                                                                                                              |
|   <pathToCertificateFile>    |                                                                                                                                                             表示的证书文件的目录路径。                                                                                                                                                              |
|             -ct              |                                                                                                                                           指定证书指纹将用于标识的公钥证书                                                                                                                                           |
|   <CertificateThumbprint>    |                                                       指定想要使用的证书的指纹属性的值。 例如，证书指纹的值"a9 09 50 2d d8 2a e4 14 33 e6 f8 38 86 b0 0d 42 77 a3 2a 7b"应指定为"a909502dd82ae41433e6f83886b00d4277a32a7b。"                                                        |
|          -tpmandpin          |                                                                                           添加受信任的平台模块 (TPM) 和个人识别号 (PIN) 保护程序的操作系统驱动器。 此外可以使用 **-tp**作为此命令的简化版本。                                                                                           |
|      -tpmandstartupkey       |                                                                                                                    将添加为操作系统驱动器的 TPM 和启动密钥保护程序。 此外可以使用 **-tsk**作为此命令的简化版本。                                                                                                                    |
|   -tpmandpinandstartupkey    |                                                                                                                添加 TPM、 PIN 和启动操作系统驱动器的密钥保护程序。 此外可以使用 **-tpsk**作为此命令的简化版本。                                                                                                                 |
|          -password           |                                                                                                                              添加密码密钥保护程序对数据驱动器。 此外可以使用 **-pw**作为此命令的简化版本。                                                                                                                              |
|      -adaccountorgroup       | 添加一个安全标识符 (SID) 的基于卷的标识保护程序。  此外可以使用 **-sid**作为此命令的简化版本。 **重要：** 默认情况下，你不能添加远程使用 WMI 或管理 bde ADAccountOrGroup 保护程序。  如果您的部署需要远程添加此保护程序的功能必须启用约束的委派。 |
|        -computername         |                                                                                                       指定该管理 bde 用于修改在其他计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。                                                                                                       |
|            <Name>            |                                                                                                         表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。                                                                                                         |

### <a name="BKMK_deleteprotectors"></a>-删除语法和参数
```
manage-bde  protectors  delete <Drive> [-type {recoverypassword|externalkey|certificate|tpm|tpmandstartupkey|tpmandpin|tpmandpinandstartupkey|Password|Identity}] 
[-id <KeyProtectorID>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|       参数        |                                                                              描述                                                                               |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|        <Drive>         |                                                             表示驱动器号后, 接一个冒号。                                                             |
|         -type          |                               标识要删除的密钥保护程序。 此外可以使用 **-t**作为此命令的简化版本。                               |
|    recoverypassword    |                                                 指定应删除的任何恢复密码密钥保护程序。                                                 |
|      externalkey       |                                        指定应删除与驱动器关联任何外部密钥保护程序。                                         |
|      证书       |                                       指定应删除与驱动器关联任何证书密钥保护程序。                                       |
|          tpm           |                                        指定应删除的任何仅限 TPM 的密钥保护程序与驱动器相关联。                                         |
|    tpmandstartupkey    |                                指定应删除的任何 TPM 和启动密钥基于密钥保护程序与驱动器相关联。                                |
|       tpmandpin        |                                    指定应删除任何 TPM 和 PIN 基于与驱动器相关联的密钥保护程序。                                    |
| tpmandpinandstartupkey |                             指定应删除的任何 TPM、 PIN 和启动密钥基于密钥保护程序与驱动器相关联。                             |
|        密码        |                                        指定应删除与驱动器关联任何密码密钥保护程序。                                         |
|        标识        |                                        指定应删除与驱动器关联任何标识密钥保护程序。                                         |
|          -id           |                标识的密钥保护程序以使用的密钥标识符中删除。 此参数是对另一种 **-类型**参数。                 |
|    <KeyProtectorID>    |        标识单个密钥保护程序的驱动器上删除。 可以使用显示密钥保护程序 Id**管理 bde-保护程序-获取**命令。         |
|     -computername      | 指定将使用该管理 bde.exe 修改另一台计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。 |
|         <Name>         |    表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。     |
|        -? 或 /?        |                                                               显示简短的命令提示符处的帮助。                                                               |
|      -help 或-h       |                                                             显示完整的命令提示符处的帮助。                                                              |

### <a name="BKMK_disableprot"></a>-禁用语法和参数
```
manage-bde  protectors  disable <Drive> [-RebootCount <integer 0 - 15>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

|   参数   |                                                                                                                                                                                                                   描述                                                                                                                                                                                                                    |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    <Drive>    |                                                                                                                                                                                                  表示驱动器号后, 接一个冒号。                                                                                                                                                                                                  |
|  RebootCount  | 从 Windows 8 开始指定的操作系统卷保护已挂起，将会恢复后已被 Windows 重启 RebootCount 参数中指定的次数。 指定 0 可无限期挂起保护。 如果未指定此参数会自动将 BitLocker 保护，恢复时重新启动 Windows。 此外可以使用 **-rc**作为此命令的简化版本。 |
| -computername |                                                                                                                                      指定将使用该管理 bde.exe 修改另一台计算机上的 BitLocker 保护。 此外可以使用 **-cn**作为此命令的简化版本。                                                                                                                                      |
|    <Name>     |                                                                                                                                         表示要修改其 BitLocker 保护的计算机的名称。 接受的值包括计算机的 NetBIOS 名称和计算机的 IP 地址。                                                                                                                                          |
|   -? 或 /?    |                                                                                                                                                                                                    显示简短的命令提示符处的帮助。                                                                                                                                                                                                    |
|  -help 或-h  |                                                                                                                                                                                                  显示完整的命令提示符处的帮助。                                                                                                                                                                                                   |

## <a name="BKMK_Examples"></a>示例
下面的示例演示如何使用**的保护程序**命令，将添加到驱动器 E。 证书文件标识的证书密钥保护程序
```
manage-bde  protectors  add E: -certificate  cf "c:\File Folder\Filename.cer"
```
下面的示例演示如何使用**的保护程序**命令，将添加**adaccountorgroup**到驱动器 E 的域和用户名称所标识的密钥保护程序
```
manage-bde  protectors  add E: -sid DOMAIN\user
```
下面的示例演示如何使用**保护程序**命令来禁用保护，具有 3 次重启计算机。
```
manage-bde  protectors  disable C: -rc 3
```
下面的示例演示如何使用**的保护程序**命令以删除所有的 TPM 和启动密钥将密钥保护程序基于驱动器 c。
```
manage-bde  protectors  delete C: -type tpmandstartupkey
```
下面的示例演示如何使用**的保护程序**命令来为驱动器 C 的所有恢复信息都备份到 AD DS。
```
manage-bde  protectors  adbackup C:
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
-   [manage-bde](manage-bde.md)
