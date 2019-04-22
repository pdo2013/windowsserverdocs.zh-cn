---
title: freedisk
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 91c15166-5baa-4b80-9e0c-4cd815d00530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 138d637ba3da983ccb931d489f7c22b651e07a0c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817218"
---
# <a name="freedisk"></a>freedisk

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

检查指定的磁盘空间量然后才能继续完成安装过程中是否可用。

## <a name="syntax"></a>语法
```
freedisk [/s <computer> [/u [<Domain>\]<User> [/p [<Password>]]]] [/d <Drive>] [<Value>]
```
## <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|/s <computer>|指定的名称或远程计算机的 IP 地址 （不使用反斜杠）。 默认值为本地计算机。 此参数适用于所有文件和命令中指定的文件夹。|
|/u [<Domain>\\]<User>|指定的用户帐户的权限来运行脚本。 默认值是系统的权限。|
|/p [<Password>]|指定在指定的用户帐户的密码 **/u**。|
|/d <Drive>|指定你想要找出的可用空间的可用性的驱动器。 必须指定<Drive>远程计算机。|
|<Value>|检查特定量的可用磁盘空间。 您可以指定 <Value>以 KB、 MB、 GB、 TB、 PB、 EB、 ZB 或 YB，字节为单位。|
## <a name="remarks"></a>备注
-   使用 **/s**， **/u**，并 **/p**仅使用可以提供了命令行选项 **/s**。 必须使用 **/p**与 **/u**提供 s 用户密码。
-   对于无人参与安装，可以使用**freedisk**中安装批处理文件以继续安装之前检查可用空间数量。
-   当你使用**freedisk**在批处理文件中，它将返回**0**如果没有足够的空间且**1**如果没有足够的空间。
## <a name="BKMK_examples"></a>示例
若要确定是否有至少 50 MB 的可用空间可用 c： 驱动器上，请键入：
```
freedisk 50mb 
```
在屏幕上显示类似于以下输出：
```
INFO: The specified 52,428,800 byte(s) of free space is available on current drive.
```
## <a name="additional-references"></a>其他参考
[命令行语法解答](command-line-syntax-key.md)
