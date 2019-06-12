---
title: nfsadmin
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7375b2cf-c6b8-45b5-abf6-6c10e462defd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4dc49e23d67ae68c598367de5a3fb0d7d6398a8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437162"
---
# <a name="nfsadmin"></a>nfsadmin

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

可以使用**nfsadmin** NFS 和 NFS 客户端的管理服务器。  
  
## <a name="syntax"></a>语法  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名*`[`\-p*密码*`]]` \-l  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` \-r `{`*客户端*`|`所有`}`  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]] {`启动`|`停止`}`  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` config*选项*`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` creategroup*名称*  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` listgroups  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` deletegroup*名称*  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` renamegroup *OldName NewName*  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` addmembers*名称主机*`[...]`  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` listmembers  
  
**nfsadmin server** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` deletemembers*组主机*`[...]`  
  
**客户端 nfsadmin** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]] {`启动`|`停止`}`  
  
**客户端 nfsadmin** `[` *computerName*`] [`\-u*用户名* `[` \-p*密码*`]]` config*选项*`[...]`  
  
## <a name="description"></a>描述  
**Nfsadmin**命令\-行实用程序用于服务器管理 NFS 或 nfs 客户端，运行网络文件系统的 Microsoft 服务的本地或远程计算机上\(NFS\)。 如果使用不具有所需的权限的帐户登录，可以指定用户名和密码的帐户。 执行的操作**nfsadmin**取决于提供的命令参数。  
  
除了服务之外\-特定的命令参数和选项， **nfsadmin**接受以下：  
  
*computerName*  
指定你想要管理的远程计算机。 可以指定使用 Windows Internet 名称服务的计算机\(WINS\)名称或域名系统\(DNS\)名称，或通过 Internet 协议\(IP\)地址。  
  
**\-u** *UserName*  
指定要使用其凭据的用户的用户名。 它可能需要将域名添加到窗体中的用户名称<em>域</em> **\\** <em>用户名</em>  
  
**\-p** *密码*  
指定使用指定的用户的密码 **\-u**选项。 如果指定 **\-u**选项，但是省略 **\-p**选项，系统会提示输入用户的密码。  
  
#### <a name="administering-server-for-nfs"></a>管理 nfs 服务器  
使用**nfsadmin server**命令来管理 nfs 服务器。 特定操作的**nfsadmin server**花费的时间取决于命令选项或您指定的参数：  
  
**\-l**  
列出了持有客户端的所有锁。  
  
**\-r** {*client* | **all**}  
释放持有锁*客户端*; 如果**所有**指定，则所有客户端。  
  
**start**  
启动 NFS 服务器服务。  
  
**stop**  
停止 NFS 服务器服务。  
  
**config**  
指定 nfs 服务器的常规设置。 必须提供至少使用以下选项之一**config**命令参数：  
  
**mapsvr\=** <em>server</em>  
集*server*为 nfs 服务器的用户名映射服务器。 虽然此选项可继续与早期版本的兼容性支持，应使用**sfuadmin**实用工具相反。  
  
**auditlocation\=** {**eventlog** | **file** | **both** | **none**}  
指定是否将审核事件和其中将记录事件。 以下参数之一是必需的。  
  
**eventlog**  
指定仅在事件查看器应用程序日志将记录审核的事件。  
  
**file**  
指定将仅在指定的文件中记录审核的事件**config fname**。  
  
**both**  
指定审核的事件将记录在事件查看器应用程序日志，以及指定的文件**config fname**。  
  
**none**  
指定不将审核事件。  
  
**fname\=** <em>file</em>  
设置指定的文件*文件*为审核文件。 默认值为 %sfudir%\\日志\\nfssvr.log  
  
**fsize\=** \=*size*  
集*大小*作为最大大小以兆字节的审核文件。 默认最大大小为 7 MB。  
  
**审核\=** \[ **\+** | **\-** \]**装载** \[ **\+** | **\-** \]**读取** \[ **\+** | **\-** \]**编写** \[ **\+** | **\-**  \]**创建** \[ **\+** | **\-** \]**删除** \[ **\+** | **\-** \]**锁定** \[ **\+** | **\-** \]**所有**  
指定要记录的事件。 若要启动的事件记录日志，请键入一个加号\( **\+** \)之前的事件名称; 若要停止记录事件时，键入减号\( **\-** \)之前的事件名称。 如果省略符号，则假定加号。 不要使用**所有**使用任何其他的事件名称。  
  
**lockperiod\=** <em>seconds</em>  
指定 nfs 服务器与 nfs 服务器的连接已丢失，然后重新建立连接后，或重新启动 NFS 服务的服务器后回收锁将等待的秒数。  
  
Portmapprotocol\={TCP |UDP |TCP\+UDP  
指定端口映射支持的传输协议。 默认设置是**TCP\+UDP**。  
  
mountprotocol\={TCP | UDP | TCP\+UDP}  
指定哪种传输协议装载支持。 默认设置是**TCP\+UDP**。  
  
nfsprotocol\={TCP | UDP | TCP\+UDP}  
指定哪种传输协议，网络文件系统\(NFS\)支持。 默认设置是**TCP\+UDP**  
  
nlmprotocol\={TCP | UDP | TCP\+UDP}  
指定哪种传输协议，网络锁定管理器\(NLM\)支持。 默认设置是**TCP\+UDP**。  
  
nsmprotocol\={TCP | UDP | TCP\+UDP}  
指定哪种传输协议的网络状态管理器\(NSM\)支持。 默认设置是**TCP\+UDP**。  
  
**enableV3\=** {**yes** | **no**}  
指定是否将支持 NFS 版本 3 协议。 默认设置是**是**。  
  
**renewauth\=** {**是** | **没有**}  
指定客户端连接是否将需要重新进行身份验证后指定的时期**config renewauthinterval**。 默认设置是**没有**。  
  
**renewauthinterval\=** <em>seconds</em>  
指定客户端强制如果重新进行身份验证之前经过的秒数**config renewauth**设置为**是**。 默认值为 600 秒。  
  
**dircache\=** <em>size</em>  
指定的大小以千字节为单位的目录缓存。 为指定的数字*大小*必须是介于 4 和 128 之间的 4 的倍数。 默认目录\-缓存大小为 128 KB。  
  
**translationfile**\=\[file\]  
指定包含用于从 Windows 进行移动时替换文件的名称中的字符映射信息的文件\-基于到 UNIX\-基于文件系统。 如果*文件*未指定，则禁用文件名字符转换。 如果的值**translationfile**是发生更改时，您必须重新启动服务器以使更改生效。  
  
**dotfileshidden**\={**yes** | **no**}  
指定文件是否使用以句点开头的名称创建\(。\)将标记为隐藏 Windows 文件系统中并因此从 NFS 客户端中隐藏。 默认设置是**没有**。  
  
**casesensitivelookups\=** {**是** | **没有**}  
指定是否目录查找将区分大小写\(要求的字符大小写完全匹配\)。  
  
此外需要禁用 Windows 内核用例\-服务器的 NFS 以支持用例的顺序不区分大小写\-敏感文件的名称。 您可以禁用 Windows 内核用例\-通过清除为 0 以下注册表项不区分大小写：  
  
HKLM\\SYSTEM\\CurrentControlSet\\Control\\Session Manager\\kernel  
  
DWOrd obcaseinsensitive   
  
> [!IMPORTANT]  
> 本部分仅适用于 Windows Server 2008 R2、 Windows Server 2008 和 Windows Server 2003。 本部分不适用于 Windows Server 2012 R2 或 Windows Server 2012。  
  
**ntfscase\=** {**lower** | **upper** | **preserve**}  
指定是否将以小写字母，大写，或在窗体存储在目录中返回的 NTFS 文件系统中的文件的名称中的字符的大小写。 默认设置是**保留**。 不能更改此设置，如果**casesensitivelookups**设置为**是**。  
  
**creategroup** *name*  
创建一个新的客户端组，为其提供指定*名称*。  
  
**listgroups**  
显示所有客户端组的名称。  
  
**deletegroup** *name*  
移除由指定的客户端组*名称*。  
  
**renamegroup** *OldName NewName*  
由指定的客户端组的名称更改*OldName*到*NewName*  
  
**addmembers** *名称主机*\[...\]  
将添加*主机*到由指定的客户端组*名称*。  
  
**listmembers** *名称*  
列出了由指定的客户端组中的主机计算机*名称*。  
  
**deletemembers** *组主机*\[...\]  
删除指定的客户端*主机*从指定的客户端组*组*。  
  
如果不指定命令选项或参数， **nfsadmin server**显示 NFS 配置设置的当前服务器。  
  
#### <a name="administering-client-for-nfs"></a>管理 nfs 客户端  
使用**nfsadmin 客户端**命令来管理 nfs 客户端。 特定操作的**nfsadmin 客户端**花费的时间取决于您指定的命令参数：  
  
**start**  
启动 NFS 客户端服务。  
  
**stop**  
停止 NFS 客户端服务。  
  
**config**  
指定 nfs 客户端的常规设置。 必须提供至少使用以下选项之一**config**命令参数：  
  
**fileaccess\=** <em>mode</em>  
-   指定网络文件系统上创建的文件的默认权限模式\(NFS\)服务器。 *模式下*自变量包含从 0 到 7 的三个数字\(非独占\)表示的默认权限授予用户、 组和其他\(分别\)。 数字转换为 UNIX\-样式的权限，如下所示：0\=无，1\=x 2\=w，3\=wx，4\=r，5\=rx，6\=rw 和 7\=rwx。 例如， **fileaccess\=750**提供了对所有者的 rwx 权限，rx 到组的权限，向其他人没有访问权限。  
  
**mapsvr\=** <em>server</em>  
集*server*为 nfs 客户端的用户名映射服务器。 虽然此选项可继续与早期版本的兼容性支持，应使用**sfuadmin**实用工具相反。  
  
**mtype\=** {**hard** | **soft**}  
指定默认装载类型。 为硬装载 nfs 客户端将继续重试失败的 RPC，直到成功。 对于软装载 nfs 客户端返回失败重试该调用后调用的应用程序的数目指定的次数**重试**选项。  
  
**retry\=** <em>number</em>  
指定的次数来尝试为软装载建立连接。 此值必须介于 1 到 10，非独占。 默认值为 1。  
  
**timeout\=** <em>seconds</em>  
指定要连接时等待的秒数\(远程过程调用\)。 此值必须是 0.8、 0.9 或从 1 到 60，非独占的整数。 默认值为 0.8。  
  
**Protocol\={TCP | UDP | TCP\+UDP}**  
指定哪种传输协议客户端支持。 默认设置是**TCP\+UDP**  
  
**rsize\=** <em>size</em>  
指定的大小，以千字节为单位的读取缓冲区。 此值可以为 0.5、 1、 2、 4、 8、 16 或 32。 默认值为 32。  
  
**wsize\=** <em>size</em>  
指定的大小，以千字节为单位，写入缓冲区。 此值可以为 0.5、 1、 2、 4、 8、 16 或 32。 默认值为 32。  
  
**perf\=default**  
将以下性能设置还原为默认值：  
  
-   **mtype**  
  
-   **retry**  
  
-   **timeout**  
  
-   **rsize**  
  
-   **wsize**  
  
**fileaccess\=** <em>mode</em>  
指定网络文件系统上创建的文件的默认权限模式\(NFS\)服务器。 *模式下*自变量包含从 0 到 7 的三个数字\(非独占\)表示的默认权限授予用户、 组和其他\(分别\)。 数字转换为 UNIX\-样式的权限，如下所示：0\=无，1\=x 2\=w，3\=wx，4\=r，5\=rx，6\=rw 和 7\=rwx。 例如， **fileaccess\=750**提供了对所有者的 rwx 权限，rx 到组的权限，向其他人没有访问权限。  
  
如果不指定命令选项或参数， **nfsadmin 客户端**显示 NFS 配置设置的当前客户端。  
  

