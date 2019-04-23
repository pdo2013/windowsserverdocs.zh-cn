---
title: Sc config
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ad4d68a6-efe5-452b-8501-7f1f1c552a4a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 361a8407aae3b5e823b58cd71b97b043159146e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837218"
---
# <a name="sc-config"></a>Sc config



修改服务的项在注册表和服务控制管理器数据库中的值。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ServerName>|指定的服务所在的远程服务器的名称。 该名称必须使用通用命名约定 (UNC) 格式 (例如， \\ \\myserver)。 若要本地运行 SC.exe，忽略此参数。|
|\<ServiceName>|指定返回的服务名称**getkeyname**操作。|
|类型 = {自己\|共享\|内核\|文件系统恢复\|rec\|调整\|交互类型 = {自己\|共享}} | 指定服务类型。</br>**自己**-指定在其自己的进程中运行的服务。 它不会与其他服务共享的可执行文件。 这是默认值。</br>**共享**-指定将作为共享进程运行的服务。 它与其他服务共享的可执行文件。</br>**内核**-指定的驱动程序。</br>**文件系统恢复**-指定的文件系统驱动程序。</br>**rec** -指定的文件系统识别驱动程序，用于标识的计算机上使用的文件系统。</br>**调整**-指定标识硬件设备，例如键盘、 鼠标，适配器驱动程序和磁盘驱动器。</br>**交互**-与桌面，从用户接收输入指定可以进行交互的服务。 交互服务必须在本地系统帐户下运行。 此类型必须与结合使用**类型 = 自己**或**类型 = 共享**(例如，**类型 = 交互****类型 = 自己**)。 使用**类型 = 交互**本身将生成错误。|
|开始 = {引导\|系统\|自动\|需\|禁用\|延迟自动}|指定服务的启动类型。</br>**启动**-指定启动加载程序加载的设备驱动程序。</br>**系统**-指定内核初始化过程中启动的设备驱动程序。</br>**自动**-指定服务自动启动每个时间在计算机重新启动，即使没有人登录到计算机运行。</br>**需**-指定必须手动启动的服务。 这是默认值，如果**启动 =** 未指定。</br>**禁用**-指定不能启动的服务。 若要启动禁用的服务，为其他值更改启动类型。</br>**延迟自动**-指定启动自动在短时间之后其他自动服务是否已启动, 的服务。|
|错误 = {正常\|严重\|关键\|忽略}|如果无法在引导时启动该服务，请指定错误的严重性。</br>**正常**-指定将记录错误，并显示一个消息框，通知的用户的服务未能启动。 将继续启动。 这是默认设置。</br>**严重**-指定错误记录 （如果可能）。 计算机会尝试使用最后一个已知的正确配置重新启动。 这可能导致计算机无法重新启动，但该服务可能仍无法运行。</br>**关键**-指定错误记录 （如果可能）。 计算机会尝试使用最后一个已知的正确配置重新启动。 如果最后一次的正确配置失败，启动也将失败，并启动过程将暂停且出现停止错误。</br>**忽略**-指定将记录错误，启动继续。 给超出将错误记录在事件日志中的用户进行提示。|
|binpath= \<BinaryPathName>|指定的服务二进制文件的路径。|
|group= \<LoadOrderGroup>|指定此服务的组的名称。 组的列表存储在注册表中，在**HKLM\System\CurrentControlSet\Control\ServiceGroupOrder**子项。 默认值为 null。|
|标记 = {是\|无}|指定从 CreateService 调用中获取 TagID。 标记用于引导启动或系统启动驱动程序。|
|depend= \<dependencies>|指定的服务或必须在此服务之前启动的组的名称。 正斜杠 （/） 分隔名称。|
|obj= {\<AccountName> \| \<ObjectName>}|指定的帐户服务将开始运行，或指定的驱动程序将在其中运行的 Windows 驱动程序对象的名称的名称。 默认设置是**LocalSystem**。|
|displayname= \<DisplayName>|指定用于标识用户界面程序中的服务的描述性名称。 例如，一个特定服务的子项名称是**wuauserv**，其中包含自动更新的更友好显示名称。|
|password= \<Password>|指定的密码。 如果使用除本地系统帐户以外，这是必需的。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   对于每个命令行选项 （参数），等号是选项名称的一部分。
-   一个选项及其值之间需要空间时 (例如，**类型 = 自己**。 如果省略空间，则操作将失败。

## <a name="BKMK_examples"></a>示例

若要指定 NEWSERVICE 服务二进制文件路径，请键入：
```
sc config NewService binpath= "ntsd -d c:\windows\system32\NewServ.exe"
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)