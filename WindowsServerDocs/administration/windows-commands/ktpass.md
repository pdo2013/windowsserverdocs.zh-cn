---
title: ktpass
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 47087676-311e-41f1-8414-199740d01444
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ae7508aaefd136a91bd19e547b407020293ca484
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437920"
---
# <a name="ktpass"></a>ktpass

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

Active directory 域服务 (AD DS) 中配置的主机或服务的服务器主体名称，并生成包含该服务的共享机密密钥的.keytab 文件。 .Keytab 文件基于麻省理工学院 (MIT) 对 Kerberos 身份验证协议的实现。 Ktpass 命令行工具允许支持 Kerberos 身份验证使用 Kerberos 密钥分发中心 (KDC) 服务提供的互操作性功能的非 Windows 服务。 本主题适用于中指定的操作系统版本**适用于**主题开始处的列表。  

## <a name="syntax"></a>语法  
```  
ktpass  
[/out <FileName>]   
[/princ <PrincipalName>]   
[/mapuser <UserAccount>]   
[/mapop {add|set}] [{-|+}desonly] [/in <FileName>]  
[/pass {Password|*|{-|+}rndpass}]  
[/minpass]  
[/maxpass]  
[/crypto {DES-CBC-CRC|DES-CBC-MD5|RC4-HMAC-NT|AES256-SHA1|AES128-SHA1|All}]  
[/itercount]  
[/ptype {KRB5_NT_PRINCIPAL|KRB5_NT_SRV_INST|KRB5_NT_SRV_HST}]  
[/kvno <KeyversionNum>]  
[/answer {-|+}]  
[/target]  
[/rawsalt] [{-|+}dumpsalt] [{-|+}setupn] [{-|+}setpass <Password>]  [/?|/h|/help]  
```  
## <a name="parameters"></a>Parameters  

|                                             参数                                              |                                                                                                                                                                                                                                                                                                      描述                                                                                                                                                                                                                                                                                                       |
|----------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                          /out <FileName>                                           |                                                                                                                                                                        指定要生成的 Kerberos 版本 5.keytab 文件的名称。 **注意：** 这是与你现有的.keytab 文件 /Etc/Krb5.keytab 的.keytab 文件传输到未运行 Windows 操作系统的计算机，然后替换或合并。                                                                                                                                                                        |
|                                       /princ <PrincipalName>                                       |                                                                                                                                                                                                                   在窗体中指定的主体名称host/computer.contoso.com@CONTOSO.COM。 **警告：** 此参数是区分大小写。 请参阅[备注](#BKMK_remarks)有关详细信息。                                                                                                                                                                                                                    |
|                                       /mapuser <UserAccount>                                       |                                                                                                                                                                                                                                                将由指定的 Kerberos 主体名称映射**princ**参数指定的域帐户。                                                                                                                                                                                                                                                |
|                                       /mapop {add&#124;set}                                        |                                                                                                                                                                             指定如何设置映射特性。<br /><br />-   **添加**添加指定的本地用户名称的值。 这是默认设置。<br />-   **设置**设置的值的数据加密标准 (DES)-仅加密指定的本地用户名称。                                                                                                                                                                             |
|                                         {-&#124;+}desonly                                          |                                                                                                                                                            默认情况下设置仅 DES 加密。<br /><br />-    **+** 设置为仅 DES 加密的帐户。<br />-    **-** 释放对仅 DES 加密的帐户的限制。 **重要：** 从 Windows 7 和 Windows Server 2008 R2 开始，Windows 不支持 DES 默认情况下。                                                                                                                                                            |
|                                           /in <FileName>                                           |                                                                                                                                                                                                                                                       指定要读取从主计算机未运行 Windows 操作系统的.keytab 文件。                                                                                                                                                                                                                                                        |
|                          /pass {Password&#124;\*&#124;{-&#124;+}rndpass}                           |                                                                                                                                                                                                                                           指定用于通过指定的主体用户名的密码**princ**参数。 使用"\*"以提示输入密码。                                                                                                                                                                                                                                            |
|                                              /minpass                                              |                                                                                                                                                                                                                                                                            设置为 15 个字符的随机密码的最小长度。                                                                                                                                                                                                                                                                            |
|                                              /maxpass                                              |                                                                                                                                                                                                                                                                           设置随机密码的最大长度为 256 个字符。                                                                                                                                                                                                                                                                            |
| /crypto {DES-CBC-CRC&#124;DES-CBC-MD5&#124;RC4-HMAC-NT&#124;AES256-SHA1&#124;AES128-SHA1&#124;All} | 指定在 keytab 文件中生成的键：<br /><br />-   **DES CBC CRC**用于实现兼容性。<br />-   **DES CBC MD5**更紧密地符合 MIT 实现以及用于实现兼容性。<br />-   **RC4 HMAC NT**使用 128 位加密。<br />-   **AES256-SHA1**使用 AES256-CTS 的 HMAC-SHA1-96 加密。<br />-   **AES128 SHA1**使用 AES128-CTS 的 HMAC-SHA1-96 加密。<br />-   **所有**可以使用所有支持的加密类型的状态。 **注意：** 默认设置基于较旧 MIT 版本。 因此，`/crypto`应始终指定。 |
|                                             /itercount                                             |                                                                                                                                                                                                                        指定用于 AES 加密的迭代次数。 默认值是**itercount**是设置为 4,096 的 AES 加密和忽略非 AES 加密。                                                                                                                                                                                                                         |
|               /ptype {KRB5_NT_PRINCIPAL&#124;KRB5_NT_SRV_INST&#124;KRB5_NT_SRV_HST}                |                                                                                                                                                                                         指定主体类型。<br /><br />-   **KRB5_NT_PRINCIPAL**是常用的主体类型 （推荐）。<br />-   **KRB5_NT_SRV_INST**是用户的服务实例。<br />-   **KRB5_NT_SRV_HST**主机服务实例。                                                                                                                                                                                         |
|                                       /kvno <KeyversionNum>                                        |                                                                                                                                                                                                                                                                               指定密钥的版本号。 默认值为 1。                                                                                                                                                                                                                                                                                |
|                                         问答 {-&#124;+}                                         |                                                                                                                                                                                                                    设置背景应答模式：<br /><br />**-** 答案重置密码的提示自动与不可以。<br /><br />**+** 答案重置密码是使用自动的提示。                                                                                                                                                                                                                     |
|                                              /target                                               |                                                                                                                                                                                           设置要使用哪个域控制器。 默认值为域控制器，以检测到，根据主体的名称。 如果域控制器的名称不能解决，对话框将提示输入有效的域控制器。                                                                                                                                                                                           |
|                                              /rawsalt                                              |                                                                                                                                                                                                                                                           强制 ktpass rawsalt 算法生成时要使用该密钥。 不需要此参数。                                                                                                                                                                                                                                                            |
|                                         {-&#124;+}dumpsalt                                         |                                                                                                                                                                                                                                                           此参数的输出显示了用于生成密钥的 MIT salt 算法。                                                                                                                                                                                                                                                            |
|                                          {-&#124;+}setupn                                          |                                                                                                                                                                                                                                          设置用户主体名称 (UPN) 除了服务主体名称 (SPN)。 默认值为同时.keytab 文件中设置。                                                                                                                                                                                                                                           |
|                                    {-&#124;+}setpass <Password>                                    |                                                                                                                                                                                                                                                          设置用户的密码时提供。 如果使用 rndpass，而生成一个随机密码。                                                                                                                                                                                                                                                           |
|                                       /?&#124;/h&#124;/help                                        |                                                                                                                                                                                                                                                                                         显示命令行帮助 ktpass。                                                                                                                                                                                                                                                                                         |

## <a name="BKMK_remarks"></a>备注  
可以使用 active directory 域服务中的服务实例帐户配置不运行 Windows 操作系统的系统上运行的服务。 这样，任何 Kerberos 客户端向服务未运行 Windows 操作系统通过使用 Windows Kdc 进行身份验证。  
/Princ 参数 ktpass 不计，并提供使用。 未检查参数是否与正确的大小写形式匹配**userPrincipalName**生成 Keytab 文件时，属性值。 使用此 Keytab 文件的区分大小写 Kerberos 分发可能会遇到问题没有确切的大小写匹配项时，在预身份验证过程可能会失败。 检查并检索正确**userPrincipalName**从 LDifDE 导出文件属性值。 例如：  
```  
ldifde /f keytab_user.ldf /d "CN=Keytab User,OU=UserAccounts,DC=contoso,DC=corp,DC=microsoft,DC=com" /p base /l samaccountname,userprincipalname  
```  
## <a name="BKMK_examples"></a>示例  
下面的示例说明了如何在用户 Sample1 的当前目录中创建的 Kerberos.keytab 文件 machine.keytab。 （将合并此文件，其未运行 Windows 操作系统的主机计算机上的 Krb5.keytab 文件。）将创建一般主体类型的所有支持的加密类型的 Kerberos.keytab 文件。  
若要生成未运行 Windows 操作系统的主机计算机的.keytab 文件，请使用以下步骤，将该主体映射到帐户设置的主机主体密码：  
1.  使用 active directory 用户和计算机管理单元未运行 Windows 操作系统的计算机上创建一个服务的用户帐户。 例如，使用名称 Sample1 创建帐户。  
2.  Ktpass 用于设置用户帐户的标识映射通过在命令提示符下键入以下内容：  
    ```  
    ktpass /princ host/Sample1.contoso.com@CONTOSO.COM /mapuser Sample1 /pass MyPas$w0rd /out Sample1.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set   
    ```  

    > [!NOTE]  
    > 不能将多个服务实例映射到相同的用户帐户。  

3.  合并具有 /Etc/Krb5.keytab 文件未运行 Windows 操作系统的主机计算机上的.keytab 文件。 

#### <a name="additional-references"></a>其他参考  
[命令行语法项](command-line-syntax-key.md)  
