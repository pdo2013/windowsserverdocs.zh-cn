---
title: 装载
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dd9d7ecb-ef00-4aaa-bcd0-423fa636e34a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a225c847055198a9a48962a3b40969556f10ec1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373595"
---
# <a name="mount"></a>装载



你可以使用**装入**来装入网络文件系统（NFS）网络共享。

## <a name="syntax"></a>语法

```
mount [-o <Option>[...]] [-u:<UserName>] [-p:{<Password> | *}] {\\<ComputerName>\<ShareName> | <ComputerName>:/<ShareName>} {<DeviceName> | *}
```

## <a name="description"></a>描述

**装载**命令行实用程序将装载由*COMPUTERNAME*标识的 NFS 服务器导出的*共享名*所标识的文件系统，并将其与*DeviceName*或指定的驱动器号关联（ **&#42;** ）由第一个可用的驱动程序字母使用。 然后，用户可以访问导出的文件系统，就像它是本地计算机上的驱动器一样。 当不带选项或参数使用时，**装载**会显示有关所有已装载 NFS 文件系统的信息。

仅当安装了 NFS 客户端时，**装载**实用程序才可用。

以下选项和参数可用于**装载**实用工具。


|          术语          |                                                                                                                                                                                                                                                定义                                                                                                                                                                                                                                                |
|------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -o rsize = \<buffersize > |                                                                                                                                                                                            设置读取缓冲区的大小（kb）。 可接受的值为1、2、4、8、16和 32;默认值为 32 KB。                                                                                                                                                                                            |
| -o wsize = \<buffersize > |                                                                                                                                                                                           设置写入缓冲区的大小（以 kb 为单位）。 可接受的值为1、2、4、8、16和 32;默认值为 32 KB。                                                                                                                                                                                            |
| -o timeout = \<seconds >  |                                                                                                                                                                       设置远程过程调用（RPC）的超时值（以秒为单位）。 可接受的值为0.8、0.9 和范围1-60 内的任何整数;默认值为0.8。                                                                                                                                                                       |
|   -o retry = \<number >   |                                                                                                                                                                                             设置软装载的重试次数。 可接受的值为1-10 范围内的整数;默认值为1。                                                                                                                                                                                             |
|     -o mtype = {soft     |                                                                                                                                                                                                                                                  块                                                                                                                                                                                                                                                   |
|        -o anon         |                                                                                                                                                                                                                                       作为匿名用户进行安装。                                                                                                                                                                                                                                       |
|       -o nolock        |                                                                                                                                                                                                                                禁用锁定（默认为**启用**）。                                                                                                                                                                                                                                |
|    -o casesensitive    |                                                                                                                                                                                                                         强制服务器上的文件查找区分大小写。                                                                                                                                                                                                                          |
| -o fileaccess = \<mode >  | 指定在 NFS 共享上创建的新文件的默认权限模式。 将*模式*指定为*ogw*形式的三位数字，其中*o*、 *g*和*w*分别表示授予了文件所有者、组和世界的访问权限。 位数必须在0-7 范围内，其含义如下：</br>0不能访问</br>-1： x （执行访问）</br>-2： w （写入访问权限）</br>-3： wx</br>-4： r （读取访问权限）</br>-5： rx</br>-6： rw</br>-7： rwx |
|    -o lang = {euc-jp     |                                                                                                                                                                                                                                                  euc-幼圆                                                                                                                                                                                                                                                  |
|     -u： \<UserName >     |                                                                                                                                                                             指定用于装载共享的用户名。 如果*用户名*前面没有反斜杠（ **\\** ），则将其视为 UNIX 用户名。                                                                                                                                                                             |
|     -p： \<Password >     |                                                                                                                                                                                          用于装载共享的密码。 如果使用星号（ **&#42;** ），系统将提示你输入密码。                                                                                                                                                                                          |

> [!NOTE]
