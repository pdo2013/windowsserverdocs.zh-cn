---
title: macfile
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c50c53fc277626d1b83268f5e0c8dcc95161f35a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854048"
---
# <a name="macfile"></a>macfile

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

对于 Macintosh 服务器、 卷、 目录和文件管理文件服务器。 您可以通过在批处理文件中包含的一系列命令，并手动启动它们自动执行管理任务，或在预定时间。 
-   [若要修改 Macintosh 可访问卷中的目录](#BKMK_Moddirs)
-   [若要加入 Macintosh 文件的数据和资源分叉](#BKMK_Joinforks)
-   [若要更改登录消息，并限制会话](#BKMK_LogonLimit)
-   [若要添加、 更改或删除可访问 Macintosh 的卷](#BKMK_addvol)

## <a name="BKMK_Moddirs"></a>若要修改 Macintosh 可访问卷中的目录

### <a name="syntax"></a>语法
```
macfile directory[/server:\\<computerName>] /path:<directory> [/owner:<OwnerName>] [/group:<GroupName>] [/permissions:<Permissions>]
```

### <a name="parameters"></a>Parameters
-   /server:\\\\<computerName>指定要更改的目录服务器。 如果省略，将在本地计算机上执行操作。
-   /path:<directory>必需。 指定你想要更改目录的路径。 该目录必须存在。 **macfile directory**不会创建目录。
-   /owner:<OwnerName>更改目录的所有者。 如果省略，所有者将保持不变。
-   /group:<GroupName>指定或更改与目录关联的 Macintosh 主要组。 如果省略，主要组将保持不变。
-   /permissions:<Permissions>在所有者、 主要组和世界 (everyone) 的目录上设置权限。 一个 11 位数字号用于设置权限。 授予的权限数 1 和 0 撤消的权限 (例如，11111011000)。 如果省略，则权限保持不变。
    数字的位置确定设置哪些权限下, 表中所述。
    
    |位置|设置权限|
    |------|------------|
    |第一个|OwnerSeeFiles|
    |第二个|OwnerSeeFolders|
    |第三个|OwnerMakechanges|
    |第四个|GroupSeeFiles|
    |第五个|GroupSeeFolders|
    |第六个|GroupMakechanges|
    |第七个|WorldSeeFiles|
    |第八个|WorldSeeFolders|
    |第 9 个|WorldMakechanges|
    |第 10 个|不能重命名、 移动或删除该目录。|
    |第 11 个|所做的更改适用于当前目录和所有子目录。|
    
-   /?
    在命令提示符下显示帮助。
    
### <a name="remarks"></a>备注
-   如果你提供的信息包含空格或特殊字符，使用文本周围的引号 (例如， **"***计算机名称***"**)。
-   使用**macfiledirectory**若要将 Macintosh 可访问卷中的现有目录提供给 Macintosh 用户。 **Macfiledirectory**命令不会创建目录。 使用文件管理器中，在命令提示符下或**macintosh 新文件夹**命令，在 Macintosh 可访问的卷中创建一个目录，然后使用**macfile directory**命令。
### <a name="BKMK_Examples"></a>示例
下面的示例更改子目录 5 月销售、 Macintosh 可访问卷统计信息，在 E 驱动器的本地服务器中的权限。 示例可将查看文件，请参阅文件夹和品牌更改权限分配给所有者和其他所有用户，请参阅文件和查看文件夹权限，同时防止目录重命名、 移动或删除。
```
macfile directory /path:"e:\statistics\may sales" /permissions:11111011000
```

## <a name="BKMK_Joinforks"></a>若要加入 Macintosh 文件的数据和资源分叉

### <a name="syntax"></a>语法
```
macfile forkize[/server:\\<computerName>] [/creator:<CreatorName>] [/type:<typeName>]  [/datafork:<Filepath>] [/resourcefork:<Filepath>] /targetfile:<Filepath>
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/server:\\\\<computerName>|指定要加入的文件服务器。 如果省略，将在本地计算机上执行操作。|
|/creator:<CreatorName>|指定的文件的创建者。 使用 Macintosh finder **/creator**命令行选项，以确定创建该文件的应用程序。|
|/type:<typeName>|指定文件的类型。 使用 Macintosh finder **/type**命令行选项，以确定创建该文件的应用程序中的文件类型。|
|/datafork:<Filepath>|指定是要联接的数据派生的位置。 您可以指定远程路径。|
|/resourcefork:<Filepath>|指定是要加入的资源分叉的位置。 您可以指定远程路径。|
|/targetfile:<Filepath>|必需。 指定通过联接数据派生和资源派生，创建的文件的位置或指定文件的位置，其类型或要更改的创建者。 文件必须指定服务器上。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注
-   如果你提供的信息包含空格或特殊字符，使用文本周围的引号 (例如， **"***计算机名称***"**)。

### <a name="examples"></a>示例
创建文件 treeapp Macintosh 可访问的卷 D:\Release，使用资源分叉 C:\Cross\Mac\Appcode，从而使在显示 Macintosh 客户端应用程序为此新文件 （Macintosh 应用程序使用的类型应用） 的创建者 （签名) 设置为木兰，类型：
```
macfile forkize /resourcefork:c:\cross\mac\appcode /type:APPL /creator:MAGNOLIA /targetfile:D:\Release\treeapp
```
文件创建者更改为 Microsoft Word 5.1 中，文件在目录 D:\Word documents\Group 文件服务器上的 WOrd.txt \\\SERverA 中，键入：
```
macfile forkize /server:\\servera /creator:MSWD /type:TEXT /targetfile:"d:\Word documents\Group files\Word.txt"
```

## <a name="BKMK_LogonLimit"></a>若要更改登录消息，并限制会话
### <a name="syntax"></a>语法
```
macfile server [/server:\\<computerName>] [/maxsessions:{Number | unlimited}] [/loginmessage:<Message>]
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/server:\\\\<computerName>|指定要更改参数的服务器。 如果省略，将在本地计算机上执行操作。|
|/maxsessions: {数&#124;不受限制}|指定的用户可以同时使用文件和 Macintosh 的打印服务器的最大数目。 如果省略， **maxsessions**设置服务器保持不变。|
|/loginmessage:<Message>|更改 Macintosh 用户看到的消息用于 Macintosh 服务器的文件服务器进行日志记录时。 最大登录消息的字符数为 199。 如果省略， **loginmessage**消息的服务器将保持不变。 若要删除现有的登录消息，包括 **/loginmessage**参数，但保留*消息*变量为空。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注
-   如果你提供的信息包含空格或特殊字符，使用文本周围的引号 (例如， **"***计算机名称***"**)。

### <a name="examples"></a>示例
若要更改的文件数和打印服务器的 Macintosh 会话允许在本地服务器上从当前设置为五个会话，并添加登录消息"从服务器 for Macintosh 时注销完成。"，请键入：
```
macfile server /maxsessions:5 /loginmessage:"Log off from Server for Macintosh when you are finished."
```

## <a name="BKMK_addvol"></a>若要添加、 更改或删除可访问 Macintosh 的卷
### <a name="syntax"></a>语法
```
macfile volume {/add|/set} [/server:\\<computerName>] /name:<volumeName>/path:<directory>[/readonly:{true | false}] [/guestsallowed:{true | false}] [/password:<Password>] [/maxusers:{<Number>>|unlimited}]
macfile volume /remove[/server:\\<computerName>] /name:<volumeName>
```

### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|{/add &#124; /set}|要添加或更改可访问 Macintosh 的卷时需要。 添加或更改指定的卷。|
|/server:\\\\<computerName>|指定要添加、 更改或删除卷的服务器。 如果省略，将在本地计算机上执行操作。|
|/name:<volumeName>|必需。 指定要添加、 更改或删除的卷名称。|
|/path:<directory>|必需的和仅在添加卷时有效。 指定要添加的卷的根目录的路径。|
|/readonly:{true &#124; false}|指定用户是否可以更改卷中的文件。 类型为 true，则指定用户不能更改卷中的文件。 键入 false 以指定用户可以更改卷中的文件。 如果省略添加卷时，，允许对文件的更改。 如果在更改卷时省略**readonly**设置使卷保持不变。|
|/guestsallowed: {true &#124; false}|指定是否以来宾身份登录的用户可以使用该卷。 类型为 true，则指定来宾可以使用该卷。 键入 false 以指定来宾不能使用该卷。 如果省略添加卷时，来宾可以使用该卷。 如果在更改卷时省略**guestsallowed**设置使卷保持不变。|
|/password:<Password>|指定需要访问该卷的密码。 如果省略添加卷时，会创建没有密码。 如果省略更改卷时，密码将保持不变。|
|/maxusers:{<Number>>&#124;unlimited}|指定的用户都能同时使用卷上的文件的最大数目。 如果省略添加卷时，无限的数量的用户可以使用该卷。 如果在更改卷时省略**maxusers**值保持不变。|
|/remove|要删除的 Macintosh 访问卷时需要。 删除指定的卷。|
|/?|在命令提示符下显示帮助。|

### <a name="remarks"></a>备注
-   如果你提供的信息包含空格或特殊字符，使用文本周围的引号 (例如， **"***计算机名称***"**)。

### <a name="examples"></a>示例
若要创建的本地服务器上，在 E 驱动器中使用的统计信息目录名美国市场营销统计信息的卷并指定来宾无法访问该卷，请键入：
```
macfile volume /add /name:"US Marketing Statistics" /guestsallowed:false /path:e:\Stats
```
若要更改的量将上面创建为只读且需要密码，同时将最大用户数设置为 5，请键入：
```
macfile volume /set /name:"US Marketing Statistics" /readonly:true /password:saturn /maxusers:5
```
若要添加的服务器上名环境设计的卷\\\Magnolia，在 E 驱动器，以指定来宾，类型可以访问该卷使用的树目录：
```
macfile volume /add /server:\\Magnolia /name:"Landscape Design" /path:e:\trees
```
若要删除的卷，本地服务器上名为 Sales 的报表，请键入：
```
macfile volume /remove /name:"Sales Reports"
```

## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
