---
title: prnport
description: 了解如何创建、 删除和列出打印机端口。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a0ec638-a21e-4a34-be5c-bd0f7ca89ffe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eb6ef7632c10217c4fdf114d4e67c8bc52be07e2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856888"
---
# <a name="prnport"></a>prnport

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

创建、 删除和列出了标准 TCP/IP 打印机端口，以及显示和更改端口配置。

## <a name="syntax"></a>语法
```
cscript prnport {-a | -d | -l | -g | -t | -?} [-r <PortName>] 
[-s <ServerName>] [-u <UserName>] [-w <Password>] [-o {raw | lpr}] 
[-h <Hostaddress>] [-q <QueueName>] [-n <PortNumber>] -m{e | d} 
[-i <SNMPIndex>] [-y <CommunityName>] -2{e | -d}
```

## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|-a|创建一个标准 TCP/IP 打印机端口。|
|-d|删除标准 TCP/IP 打印机端口。|
|-l|列出与指定的计算机上所有标准 TCP/IP 打印机端口 **-s**参数。|
|-g|显示一个标准 TCP/IP 打印机端口的配置。|
|-t|配置标准 TCP/IP 打印机端口的端口设置。|
|-r\<端口名 >|指定将打印机连接的端口。|
|-s\<服务器名 >|指定托管你想要管理的打印机的远程计算机的名称。 如果不指定计算机，则使用本地计算机。|
|-u \<UserName> -w <Password>|指定的帐户有权连接到承载你想要管理的打印机的计算机。 所有目标计算机的本地 Administrators 组的成员都具有这些权限，但也可以向其他用户授予权限。 如果不指定一个帐户，你必须登录才能使用该命令这些权限的帐户下。|
|-o {raw &#124; lpr}|指定该协议端口使用：原始的 TCP 或 TCP lpr。 如果使用原始 TCP 时，可以通过选择指定的端口号 **-n**参数。 默认端口号是 9100。|
|-h\<主机地址 >|（按 IP 地址） 指定你想要配置的端口的打印机。|
|-q \<QueueName>|指定 TCP 端口，原始的队列名称。|
|-n\<端口号 >|指定 TCP 原始端口的端口号。 默认端口号是 9100。|
|-m{e &#124; d}|指定是否启用了 SNMP。 将参数**e**使 SNMP。 将参数**d**禁用 SNMP。|
|-i \<SNMPIndex|如果启用了 SNMP，指定 SNMP 索引。 有关详细信息，请参阅 Rfc 1759 [Rfc 编辑器网站](https://go.microsoft.com/fwlink/?LinkId=569)。|
|-y \<CommunityName>|指定 SNMP 共同体名称，如果启用了 SNMP。|
|-2{e &#124; -d}|指定是否为采用 TCP lpr 端口启用双重后台打印 （也称为二次后台打印）。 双重后台打印是必要的因为 TCP lpr 必须发送到打印机的控制文件中包含的精确的字节计数，但协议不能从本地打印提供程序获取的计数。 因此，当文件将处于假脱机为 TCP lpr 打印队列时，还假脱机为 system32 目录中的临时文件。 TCP lpr 确定临时文件的大小，并发送到运行 LPD 的服务器的大小。 将参数**e**启用双重后台打印。 将参数**d**禁用双重后台打印。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注
-   **Prnport**命令是 Visual Basic 脚本位于 %WINdir%\System32\printing_Admin_Scripts\\ <language>目录。 若要使用此命令中，在命令提示符处，键入**cscript**然后到 prnport 文件或将目录更改为适当的文件夹的完整路径。 例如：
    ```
    cscript %WINdir%\System32\printing_Admin_Scripts\en-US\prnport
    ```
-   如果你提供的信息包含空格，使用文本周围的引号 (例如， `"computer Name"`)。
-   TCP 原始协议是一个更高性能协议在 Windows 上的比 lpr 协议。

## <a name="BKMK_examples"></a>示例
若要显示服务器上的所有标准的 TCP/IP 打印端口\\\Server1 中，键入：
```
cscript prnport -l -s Server1
```
若要删除的服务器上的标准 TCP/IP 打印端口\\\Server1 连接到网络打印机在 10.2.3.4，类型：
```
cscript prnport -d -s Server1 -r IP_10.2.3.4
```
若要在服务器上添加标准的 TCP/IP 打印端口\\\Server1，连接到网络打印机在 10.2.3.4 并在端口 9100，类型上使用 TCP 原始协议：
```
cscript prnport -a -s Server1 -r IP_10.2.3.4 -h 10.2.3.4 -o raw -n 9100
```
若要启用 SNMP，指定"public"社区名称和网络打印机在 10.2.3.4 服务器共享上设置为 1 的 SNMP 索引\\\Server1 中，键入：
```
cscript prnport -t -s Server1 -r IP_10.2.3.4 -me -y public -i 1 -n 9100
```
若要连接到网络打印机在 10.2.3.4 并自动从打印机获取的设备设置的本地计算机上添加标准的 TCP/IP 打印端口，请键入：
```
cscript prnport -a -r IP_10.2.3.4 -h 10.2.3.4
```

#### <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
[打印命令参考](print-command-reference.md)
