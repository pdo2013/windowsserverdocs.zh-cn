---
title: cmstp
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 34aad544-11c3-4e85-8bbf-5bc5a971da93
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd2e8dbb08b41875335b35dd802007a0bd1fbd41
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379266"
---
# <a name="cmstp"></a>cmstp

>适用于：Windows Server （半年频道），Windows Server 2016，Windows Server 2012 R2，Windows Server 2012

安装或删除连接管理器服务配置文件。 使用不带可选参数的， **cmstp**将使用适用于操作系统和用户权限的默认设置安装服务配置文件。 
## <a name="syntax"></a>语法
语法1：
```
ServiceProfileFileName .exe /q:a /c:"cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]"
```
语法2：
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf"
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|< ServiceProfileFileName > .exe|按名称指定包含要安装的配置文件的安装包。<br /><br />对于语法1是必需的，但对于语法2是无效的。|
|/q： a|指定在不提示用户的情况下安装配置文件。 安装已成功的验证消息仍将出现。<br /><br />对于语法1是必需的，但对于语法2是无效的。|
|[驱动器：][path] @no__t 旁1/-0|必需。 按名称指定配置文件的配置文件，该配置文件确定如何安装配置文件。<br /><br />对于语法1，[Drive：] [path] 参数无效。|
|/nf|指定不应安装支持文件。|
|/ni|指定不应创建桌面图标。 此参数仅对运行 Windows 95、Windows 98、Windows NT 4.0 或 Windows Millennium edition 的计算机有效。|
|/ns|指定不应创建桌面快捷方式。 此参数仅对运行 Windows Server 2003 家族、Windows 2000 或 Windows XP 的成员的计算机有效。|
|/s|指定应以无提示方式安装或卸载服务配置文件（不提示用户响应或显示验证消息）。|
|/su|指定应为单一用户而不是所有用户安装服务配置文件。 此参数仅对运行 Windows Server 2003、Windows 2000 或 Windows XP 的计算机有效。|
|/au|指定应为所有用户安装服务配置文件。 此参数仅对运行 Windows Server 2003、Windows 2000 或 Windows XP 的计算机有效。|
|/u|指定应卸载服务配置文件。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
**/s**是可以结合使用 **/u**的唯一参数。
语法1是自定义安装应用程序中使用的典型语法。 若要使用此语法，必须从包含 <ServiceProfileFileName> 文件的目录中运行**cmstp** 。
## <a name="BKMK_Examples"></a>示例
若要在没有任何支持文件的情况下安装小说服务配置文件，请键入：
```
fiction.exe /c:"cmstp.exe fiction.inf /nf"
```
若要以无提示方式安装单个用户的小说服务配置文件，请键入：
```
fiction.exe /c:"cmstp.exe fiction.inf /s /su"
```
若要以无提示方式卸载小说服务配置文件，请键入：
```
fiction.exe /c:"cmstp.exe fiction.inf /s /u"
```
## <a name="additional-references"></a>其他参考
-   [命令行语法项](command-line-syntax-key.md)
