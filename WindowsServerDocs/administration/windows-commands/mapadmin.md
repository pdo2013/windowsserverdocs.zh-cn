---
title: mapadmin
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b17332c7-8622-4223-9c43-2fb9cf4d992d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ad9f55ba130014227326f4abe8540c78755f6c5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437373"
---
# <a name="mapadmin"></a>mapadmin



可以使用**Mapadmin**来管理用户映射的网络文件系统的 Microsoft 服务的名称。

## <a name="syntax"></a>语法
```
mapadmin [<computer>] [-u <user> [-p <password>]]
mapadmin [<computer>] [-u <user> [-p <password>]] {start | stop}
mapadmin [<computer>] [-u <user> [-p <password>]] config <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] add -wu <WindowsUser> -uu <UNIXUser> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] add -wg <WindowsGroup> -ug <UNIXGroup> [-setprimary]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wu <WindowsUser> [-uu <UNIXUser>]
mapadmin [<computer>] [-u <user> [-p <password>]] setprimary -wg <WindowsGroup> [-ug <UNIXGroup>]
mapadmin [<computer>] [-u <user> [-p <password>]] delete <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] list <option[...]>
mapadmin [<computer>] [-u <user> [-p <password>]] backup <filename> 
mapadmin [<computer>] [-u <user> [-p <password>]] restore <filename>
mapadmin [<computer>] [-u <user> [-p <password>]] adddomainmap -d <WindowsDomain> {-y <<NISdomain>> | -f <path>}
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -d <WindowsDomain> -y <<NISdomain>>
mapadmin [<computer>] [-u <user> [-p <password>]] removedomainmap -all
mapadmin [<computer>] [-u <user> [-p <password>]] listdomainmaps
```

## <a name="description"></a>描述
**Mapadmin**命令行实用程序用于管理用户名映射运行网络文件系统的 Microsoft 服务的本地或远程计算机上。 如果使用不具有管理凭据的帐户登录，可以指定用户名和密码的帐户。

除了特定的命令参数之外**mapadmin**接受以下参数和选项：

&lt;计算机&gt;指定运行用户名映射服务你想要管理的远程计算机。 可以指定计算机使用 Windows Internet 名称服务 (WINS) 名称或域名系统 (DNS) 名称，或通过 Internet 协议 (IP) 地址。

-u&lt;用户&gt;指定要使用其凭据的用户的用户名。 它可能需要将域名添加到窗体中的用户名称<em>域</em> **\\** <em>用户名</em>。

-p&lt;密码&gt;指定的用户的密码。 如果指定 **-u**选项，但是省略 **-p**选项，系统会提示输入用户的密码。
特定操作的**mapadmin**执行取决于您指定的命令参数：

## <a name="parameters"></a>Parameters
### <a name="start"></a>start
启动用户名映射服务。

### <a name="stop"></a>stop
停止用户名映射服务。

### <a name="config"></a>配置
指定用户名映射的常规设置。 使用此命令自变量提供了以下选项： **-r &lt;dddd&gt;:&lt;hh&gt;:&lt;mm&gt;**  -指定的刷新间隔从 Windows 和 NIS 数据库更新中天数、 小时和分钟。 最小时间间隔为 5 分钟。
**-i {是 | 没有}** -启用简单映射 (**是**) 或禁用 (**没有**)。 默认情况下，简单的映射上。
**添加**-创建新用户或组的映射。 使用此命令自变量提供了以下选项：

|Option|定义|
|-----|-------|
|-wu &lt;name&gt;|指定正在为其创建一个新的映射的 Windows 用户的名称。|
|-uu &lt;name&gt;|指定正在为其创建一个新的映射的 UNIX 用户的名称。|
|-wg &lt;group&gt;|指定正在为其创建一个新的映射的 Windows 组的名称。|
|-ug&lt;组&gt;|指定正在为其创建一个新的映射的 UNIX 组的名称。|
|-setprimary|指定新的映射的主映射。|

**setprimary** -指定的映射是使用多个映射的 UNIX 用户或组的主映射。 使用此命令自变量提供了以下选项：

|Option|定义|
|-----|-------|
|-wu &lt;name&gt;|指定 Windows 用户的主映射。 如果存在多个映射的用户，使用 **-uu**选项指定的主映射。|
|-uu &lt;name&gt;|指定的主映射的 UNIX 用户。|
|-wg &lt;group&gt;|指定的 Windows 组的主映射。 如果存在多个组的映射，使用 **-ug**选项指定的主映射。|
|-ug&lt;组&gt;|指定的主映射的 UNIX 组。|

**删除**-删除用户或组的映射。 此命令参数有以下选项：

|Option|定义|
|-----|-------|
|-wu &lt;user&gt;|为其映射将被删除，指定为 Windows 用户&lt; *WindowsDomain&gt;\\&lt;用户名&gt;* 。 您必须指定这两 **-wu**或 **-uu**选项，和 / 或。 如果指定这两个选项，将删除由两个选项标识该特定映射。 如果仅指定 **-wu**选项，所有映射，将删除指定的用户。|
|-wg &lt;group&gt;|为其映射将被删除，为指定的 Windows 组&lt;WindowsDomain&gt;\\&lt;groupname&gt;。 您必须指定这两 **-wg**或 **-ug**选项，和 / 或。 如果指定这两个选项，将删除由两个选项标识该特定映射。 如果仅指定 **-wg**选项，所有映射，将删除指定的组。|
|-uu &lt;user&gt;|为其映射将被删除，指定为 UNIX 用户&lt;用户名&gt;。 您必须指定这两 **-wu**或 **-uu**选项，和 / 或。 如果指定这两个选项，将删除由两个选项标识该特定映射。 如果仅指定 **-uu**选项，所有映射，将删除指定的用户。|
|-ug&lt;组&gt;|为其映射将被删除，指定为 UNIX 组&lt;groupname&gt;。 您必须指定这两 **-wg**或 **-ug**选项，和 / 或。 如果指定这两个选项，将删除由两个选项标识该特定映射。 如果仅指定 **-ug**选项，所有映射，将删除指定的组。|

**列表**-显示有关用户和组映射的信息。 使用此命令自变量提供了以下选项：

|Option|定义|
|-----|-------|
|-所有|列出了用户和组的简单和高级映射。|
|-simple|列出所有简单映射的用户和组。|
|-高级|列出所有高级映射的用户和组。 映射将列计算它们的顺序。 跟辅助地图，标有脱字号 (^) 首先，列出主要的地图，标有星号 （*）。|
|-wu &lt;name&gt;|列出指定 Windows 用户的映射。|
|-wg &lt;group&gt;|列出了 Windows 组的映射。|
|-uu &lt;name&gt;|列出了 UNIX 用户的映射。|
|-ug&lt;组&gt;|列出了 UNIX 组的映射。|

**备份**-保存用户名映射配置，并将数据映射到指定的文件&lt;文件名&gt;。
**还原**-配置和映射的数据替换文件中的数据 (通过指定&lt;文件名&gt;)，使用创建**备份**命令参数。
**adddomainmap** -添加 Windows 域和 NIS 域或密码和组文件之间的简单映射。 此命令参数有以下选项：

|Option|定义|
|-----|-------|
|-d &lt;WindowsDomain&gt;|指定要映射的 Windows 域。|
|-y &lt;NISdomain&gt;|指定要映射的 NIS 域。&lt;b /&gt;&lt;b /&gt; **-n** &lt;nisServer&gt;指定与指定的 NIS 域的 NIS 服务器 **-y**选项。|
|-f &lt;path&gt;|指定包含要映射的密码和组文件的目录的完全限定的路径。 文件必须位于正在管理的计算机上并不能使用**mapadmin**来管理远程计算机中以设置密码和组文件的基础的映射。|

**removedomainmap** -删除 Windows 域和 NIS 域之间的简单映射。 以下选项和参数是可用于此命令参数：

|Option|定义|
|-----|-------|
|-d &lt;WindowsDomain&gt;|指定要删除的映射的 Windows 域。|
|-y &lt;NISdomain&gt;|指定要删除的映射的 NIS 域。|
|-所有|指定要删除所有 Windows 和 NIS 域之间的简单映射。 这将删除任何 Windows 域和密码与组文件之间的简单映射。|

**listdomainmaps** -列出了映射到 NIS 域或密码和组文件的 Windows 域。

## <a name="notes"></a>说明
-   如果未指定命令参数， **mapadmin**显示用户名映射的当前设置。
-   对于指定的用户或组名称的所有选项，可以使用以下格式：
-   对于 Windows 用户，使用窗体&lt;域&gt;\\&lt;用户名&gt;， \\ \\&lt;计算机&gt;\\&lt;用户名称&gt;， \\&lt;计算机&gt;\\&lt;用户名称&gt;，或&lt;计算机&gt;\\&lt;用户名称&gt;
-   对于 Windows 组，使用窗体&lt;域&gt;\\&lt;&lt;groupname&gt;&gt;， \\ \\&lt;计算机&gt;\\ &lt; &lt;groupname&gt;&gt;， \\&lt;计算机&gt;\\&lt;&lt;groupname&gt; &gt;，或&lt;计算机&gt;\\&lt;&lt;groupname&gt;&gt;
-   对于 UNIX 用户，使用窗体&lt;NISdomain&gt;\\&lt;用户名&gt;，&lt;用户名称&gt;@&lt;NISdomain&gt;，用户&lt;名称&gt;@PCNFS，或 PCNFS\\&lt;用户名&gt;
-   对于 UNIX 组，使用窗体&lt;NISdomain&gt;\\&lt;groupname&gt;， &lt;groupname&gt;@&lt;NISdomain&gt;， &lt;groupname&gt;@PCNFS，或 PCNFS\\&lt;groupname&gt;

## <a name="additional-references"></a>其他参考
[命令行语法项](command-line-syntax-key.md)
