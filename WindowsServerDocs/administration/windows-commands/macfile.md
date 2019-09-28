---
title: macfile
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e2ce586c-b316-41d3-90f8-4be0d074cc0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 194d1adaaf64ffee2a217982638ddf0661dd0369
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374175"
---
# <a name="macfile"></a>macfile

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

管理 Macintosh 服务器、卷、目录和文件的文件服务器。 您可以通过在批处理文件中包含一系列命令，然后手动或在预定时间启动它们来自动执行管理任务。 
-   [修改 Macintosh 可访问卷中的目录](#BKMK_Moddirs)
-   [联接 Macintosh 文件的数据和资源分叉](#BKMK_Joinforks)
-   [更改登录消息和限制会话](#BKMK_LogonLimit)
-   [添加、更改或删除 Macintosh 可访问的卷](#BKMK_addvol)

## <a name="BKMK_Moddirs"></a>修改 Macintosh 可访问卷中的目录

### <a name="syntax"></a>语法
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

### <a name="parameters"></a>Parameters
-   /server： \\ @ no__t-1 @ no__t-2指定要在其上更改目录的服务器。 如果省略，则在本地计算机上执行该操作。
-   /path： <directory>必需。 指定要更改的目录的路径。 该目录必须存在。 **macfile 目录**不创建目录。
-   /owner： <OwnerName> 更改目录的所有者。 如果省略，所有者将保持不变。
-   /group： <GroupName>指定或更改与目录关联的 Macintosh 主组。 如果省略，则主组将保持不变。
-   /permissions： <Permissions>为所有者、主要组和世界（everyone）设置目录权限。 11位数字用于设置权限。 Number 1 授予权限和0吊销权限（例如，11111011000）。 如果省略，则权限保持不变。
    数字的位置确定设置的权限，如下表所述。

    |位置|设置权限|
    |------|------------|
    |第一个|OwnerSeeFiles|
    |第二个|OwnerSeeFolders|
    |第三个|OwnerMakechanges|
    |第四个|GroupSeeFiles|
    |第五个|GroupSeeFolders|
    |连续|GroupMakechanges|
    |第七个|WorldSeeFiles|
    |第|WorldSeeFolders|
    |第九个|WorldMakechanges|
    |十分|不能重命名、移动或删除该目录。|
    |第|更改将应用到当前目录和所有子目录。|

-   /?
    在命令提示符下显示帮助。

### <a name="remarks"></a>备注
- 如果提供的信息包含空格或特殊字符，请使用引号将文本括起来（例如， **"** <em>computer Name</em> **"** ）。
- 使用**macfiledirectory**将可供 macintosh 用户使用的 macintosh 可访问卷中的现有目录。 **Macfiledirectory**命令不会创建目录。 使用**macfile directory**命令之前，请使用文件管理器、命令提示符或**macintosh 新文件夹**命令在 macintosh 可访问的卷中创建一个目录。
  ### <a name="BKMK_Examples"></a>示例
  下面的示例在本地服务器的 E 驱动器上更改了在 Macintosh 可访问的卷统计信息中，子目录的权限。 该示例分配 "查看文件"、"查看文件夹" 并对所有者做出更改权限，查看文件并查看文件夹对所有其他用户的权限，同时防止目录被重命名、移动或删除。
  ```
  macfile directory /path:"e:\statistics\may sales" /permissions:11111011000
  ```

## <a name="BKMK_Joinforks"></a>联接 Macintosh 文件的数据和资源分叉

### <a name="syntax"></a>语法
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

### <a name="parameters"></a>Parameters

|         参数          |                                                                                                           描述                                                                                                            |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /server： \\ @ no__t-1 @ no__t-2 |                                                            指定要在其上联接文件的服务器。 如果省略，则在本地计算机上执行该操作。                                                            |
|   /creator： <CreatorName>   |                                      指定文件的创建者。 Macintosh 查找器使用 **/creator**命令行选项来确定创建该文件的应用程序。                                       |
|      /type： <typeName>      |                                 指定文件的类型。 Macintosh 查找器使用 **/type**命令行选项来确定应用程序中创建文件的文件类型。                                 |
|    /datafork： <Filepath>    |                                                                   指定要联接的数据分叉的位置。 可以指定远程路径。                                                                   |
|  /resourcefork： <Filepath>  |                                                                 指定要联接的资源分叉的位置。 可以指定远程路径。                                                                 |
|   /targetfile： <Filepath>   | 必需。 指定通过联接数据分叉和资源分叉创建的文件的位置，或指定要更改其类型或创建者的文件的位置。 文件必须位于指定服务器上。 |
|             /?             |                                                                                               在命令提示符下显示帮助。                                                                                               |

### <a name="remarks"></a>备注
- 如果提供的信息包含空格或特殊字符，请使用引号将文本括起来（例如， **"** <em>computer Name</em> **"** ）。

### <a name="examples"></a>示例
若要在 Macintosh 可访问的卷 D:\Release 上创建 treeapp 文件，请使用资源分叉 C:\Cross\Mac\Appcode，并将此新文件显示为 Macintosh 客户端（Macintosh 应用程序使用类型 APPL.EXE）和创建者（签名））设置为木兰，请键入：
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
若要将文件创建者更改为 Microsoft Word 5.1，请在 server \\ \ SERverA 上的 documents\Group 文件中，键入以下内容：
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:"d:\Word documents\Group files\Word.txt"
```

## <a name="BKMK_LogonLimit"></a>更改登录消息和限制会话
### <a name="syntax"></a>语法
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

### <a name="parameters"></a>Parameters

|               参数                |                                                                                                                                                                           描述                                                                                                                                                                            |
|----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       /server： \\ @ no__t-1 @ no__t-2       |                                                                                                                        指定要在其上更改参数的服务器。 如果省略，则在本地计算机上执行该操作。                                                                                                                         |
| /maxsessions： {Number &#124;无限制} |                                                                                         指定可以同时对 Macintosh 使用文件和打印服务器的用户的最大数量。 如果省略，则服务器的**maxsessions**设置保持不变。                                                                                         |
|        /loginmessage： <Message>         | 更改 Macintosh 用户登录到 Macintosh 服务器的文件服务器时看到的消息。 用于登录消息的最大字符数为199。 如果省略，则服务器的**loginmessage**消息将保持不变。 若要删除现有的登录消息，请包含 **/loginmessage**参数，但将*消息*变量留空。 |
|                   /?                   |                                                                                                                                                               在命令提示符下显示帮助。                                                                                                                                                               |

### <a name="remarks"></a>备注
- 如果提供的信息包含空格或特殊字符，请使用引号将文本括起来（例如， **"** <em>computer Name</em> **"** ）。

### <a name="examples"></a>示例
若要更改从当前设置到5个会话中允许在本地服务器上使用的 Macintosh 会话文件和打印服务器的数量，并添加登录消息 "在完成时从服务器 for Macintosh 注销"。键入：
```
macfile server /maxsessions:5 /loginmessage:"Log off from Server for Macintosh when you are finished."
```

## <a name="BKMK_addvol"></a>添加、更改或删除 Macintosh 可访问的卷
### <a name="syntax"></a>语法
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

### <a name="parameters"></a>Parameters

|              参数               |                                                                                                                                                                       描述                                                                                                                                                                        |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          {/add &#124; /set}          |                                                                                                                      在添加或更改 Macintosh 可访问的卷时是必需的。 添加或更改指定的卷。                                                                                                                       |
|      /server： \\ @ no__t-1 @ no__t-2      |                                                                                                             指定要在其上添加、更改或删除卷的服务器。 如果省略，则在本地计算机上执行该操作。                                                                                                              |
|          /name： <volumeName>          |                                                                                                                                          必需。 指定要添加、更改或删除的卷名称。                                                                                                                                           |
|          /path： <directory>           |                                                                                                                仅在添加卷时才是必需的。 指定要添加的卷的根目录的路径。                                                                                                                 |
|    /readonly： {true &#124; false}     | 指定用户是否可以更改卷中的文件。 若要指定用户不能更改卷中的文件，请键入 true。 键入 false 可指定用户可以更改卷中的文件。 如果在添加卷时省略，则允许对文件进行更改。 如果在更改卷时省略，则卷的**readonly**设置将保持不变。 |
|  /guestsallowed： {true &#124; false}  |      指定以来宾身份登录的用户是否可以使用该卷。 若要指定来宾可以使用卷，请键入 true。 键入 false 可指定来宾不能使用该卷。 如果在添加卷时省略，来宾可以使用该卷。 如果在更改卷时省略，则卷的**guestsallowed**设置保持不变。       |
|         /password： <Password>         |                                                                               指定访问卷所需的密码。 如果在添加卷时省略，则不会创建密码。 如果在更改卷时省略此密码，则密码将保持不变。                                                                               |
| /maxusers： {<Number> >&#124;无限制} |                                                 指定可以同时使用卷上的文件的最大用户数。 如果在添加卷时省略，则不限数量的用户可以使用该卷。 如果在更改卷时省略，则**maxusers**值保持不变。                                                 |
|               /remove                |                                                                                                                                删除访问卷时是必需的。 删除指定的卷。                                                                                                                                |
|                  /?                  |                                                                                                                                                           在命令提示符下显示帮助。                                                                                                                                                           |

### <a name="remarks"></a>备注
- 如果提供的信息包含空格或特殊字符，请使用引号将文本括起来（例如， **"** <em>computer Name</em> **"** ）。

### <a name="examples"></a>示例
若要在本地服务器上创建名为 "美国市场营销统计信息" 的卷，使用 E 驱动器中的 "统计信息" 目录，并指定来宾不能访问该卷，请键入：
```
macfile volume /add /name:"US Marketing Statistics" /guestsallowed:false /path:e:\Stats
```
若要将上面创建的卷更改为只读并且需要密码，并将最大用户数设置为5，请键入：
```
macfile volume /set /name:"US Marketing Statistics" /readonly:true /password:saturn /maxusers:5
```
若要添加名为横向设计的卷，请在服务器上 \\ \ 木兰上，使用 E 驱动器中的树目录，并指定来宾可以访问该卷，请键入：
```
macfile volume /add /server:\\Magnolia /name:"Landscape Design" /path:e:\trees
```
若要删除本地服务器上名为 "销售报表" 的卷，请键入：
```
macfile volume /remove /name:"Sales Reports"
```

## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
