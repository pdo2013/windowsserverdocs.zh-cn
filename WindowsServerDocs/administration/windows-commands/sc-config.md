---
title: Sc config
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 26157df1db358dd1a0e0fb48d334dc0e131c5089
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371328"
---
# <a name="sc-config"></a>Sc config



修改注册表和服务控制管理器数据库中服务项的值。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
sc [<ServerName>] config [<ServiceName>] [type= {own | share | kernel | filesys | rec | adapt | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <BinaryPathName>] [group= <LoadOrderGroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<AccountName> | <ObjectName>}] [displayname= <DisplayName>] [password= <Password>]
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<ServerName >|指定服务所在的远程服务器的名称。 名称必须使用通用命名约定（UNC）格式（例如 \\ @ no__t-1myserver）。 若要在本地运行 SC.EXE，请省略此参数。|
|\<ServiceName >|指定**getkeyname**操作返回的服务名称。|
|type = {自有 @ no__t-0 share \| 内核 \| filesys \| rec \| 改编 \| 交互类型 = {自有 \| share}} | 指定服务类型。</br>**拥有**-指定在其自己的进程中运行的服务。 它不与其他服务共享可执行文件。 这是默认值。</br>**共享**-指定作为共享进程运行的服务。 它与其他服务共享可执行文件。</br>**内核**-指定驱动程序。</br>**filesys** -指定文件系统驱动程序。</br>**rec** -指定文件系统识别的驱动程序，该驱动程序标识计算机上使用的文件系统。</br>**改编**-指定标识硬件设备（例如键盘、鼠标和磁盘驱动器）的适配器驱动程序。</br>**交互**-指定可与桌面交互的服务，接收来自用户的输入。 交互式服务必须在 LocalSystem 帐户下运行。 此类型必须与**类型 = "拥有**" 或 "**类型" 为 "共享**" （例如， **type = 交互** **类型 = "拥有**"）结合使用。 使用**type = 自行交互**会生成错误。|
|开始 = {boot \| system \| 自动 \| @no__t 要求禁用 \| 个延迟-自动}|指定服务的启动类型。</br>**启动**-指定由启动加载程序加载的设备驱动程序。</br>**系统**-指定在内核初始化过程中启动的设备驱动程序。</br>**自动**指定一项服务，该服务在计算机每次重新启动时自动启动并运行（即使没有人登录到计算机）。</br>**demand** -指定必须手动启动的服务。 如果未指定**start =** ，则此值为默认值。</br>**disabled** -指定无法启动的服务。 若要启动已禁用的服务，请将启动类型更改为其他某个值。</br>**延迟-自动**指定一项服务，该服务将在启动其他自动服务之后的短时间自动启动。|
|错误 = {正常 \| 严重 \| 严重 \| 忽略}|如果服务在启动时无法启动，则指定错误的严重性。</br>**normal** -指定记录错误并显示一个消息框，通知用户服务无法启动。 启动将继续。 此为默认设置。</br>**严重**-指定记录错误（如果可能）。 计算机尝试用最后一次正确的配置重新启动。 这可能会导致计算机能够重启，但仍可能无法运行该服务。</br>**严重**-指定记录错误（如果可能）。 计算机尝试用最后一次正确的配置重新启动。 如果最后一次已知的正确配置失败，则启动也会失败，并且启动过程将停止并出现停止错误。</br>**ignore** -指定已记录错误并继续启动。 超出事件日志中记录错误的用户不会提供通知。|
|binpath = \<BinaryPathName >|指定服务二进制文件的路径。|
|group = \<LoadOrderGroup >|指定此服务所属的组的名称。 组列表存储在注册表的**HKLM\System\CurrentControlSet\Control\ServiceGroupOrder**子项中。 默认值为 null。|
|标记 = {是 \| no}|指定是否从 CreateService 调用中获取 TagID。 标记仅用于启动和系统启动驱动程序。|
|依赖 = \<dependencies >|指定必须在此服务之前启动的服务或组的名称。 名称由正斜杠（/）分隔。|
|obj = {@no__t > \| \<ObjectName >}|指定服务将在其中运行的帐户的名称，或指定将在其中运行驱动程序的 Windows 驱动程序对象的名称。 默认设置为**LocalSystem**。|
|displayname = \<DisplayName >|指定用于标识用户界面程序中的服务的描述性名称。 例如，一个特定服务的子项名称是**wuauserv**，它具有更友好的显示名称自动更新。|
|password = \<Password >|指定密码。 如果使用了除 LocalSystem 帐户之外的帐户，则这是必需的。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   对于每个命令行选项（参数），等号都是选项名称的一部分。
-   选项和其值之间需要空格（例如， **type =** an）。 如果省略此空间，则操作将失败。

## <a name="BKMK_examples"></a>示例

若要指定 NEWSERVICE 服务的二进制路径，请键入：
```
sc config NewService binpath= "ntsd -d c:\windows\system32\NewServ.exe"
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)