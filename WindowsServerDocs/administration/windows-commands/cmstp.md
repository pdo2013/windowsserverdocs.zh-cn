---
title: cmstp
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 0ee5c5189b4eab21994def221dd757b0061ef22d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836378"
---
# <a name="cmstp"></a>cmstp

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

安装或删除连接管理器服务配置文件。 使用不带可选参数**cmstp**将服务配置文件安装使用默认设置相应的操作系统和用户的权限。 
## <a name="syntax"></a>语法
语法 1:
```
ServiceProfileFileName .exe /q:a /c:"cmstp.exe ServiceProfileFileName .inf [/nf] [/ni] [/ns] [/s] [/su] [/u]"
```
语法 2:
```
cmstp.exe [/nf] [/ni] [/ns] [/s] [/su] [/u]  [Drive:][path]ServiceProfileFileName.inf"
```
### <a name="parameters"></a>Parameters
|参数|描述|
|-------|--------|
|< ServiceProfileFileName >.exe|按名称，指定包含你想要安装的配置文件的安装包。<br /><br />对于语法 2，所需语法 1 而不是有效。|
|/q:a|指定应不提示用户安装配置文件。 仍将显示安装成功的确认消息。<br /><br />对于语法 2，所需语法 1 而不是有效。|
|[驱动器:][path]<ServiceProfileFileName>.inf|必需。 按名称指定配置文件，用于确定应如何安装配置文件。<br /><br />[驱动器:] [路径] 参数对于语法 1 无效。|
|/nf|指定不应安装支持文件。|
|/ni|指定不应创建桌面图标。 此参数才有效运行 Windows 95、 Windows 98、 Windows NT 4.0 或 Windows Millennium edition 的计算机。|
|/ns|指定不应创建桌面快捷方式。 此参数才有效运行 Windows Server 2003 家族、 Windows 2000 或 Windows XP 的成员的计算机。|
|/s|指定应安装或卸载以无提示方式 （而不提示用户响应或显示验证消息） 的服务配置文件。|
|/su|指定适用于单个用户而不是为所有用户，应安装的服务配置文件。 此参数才有效运行 Windows Server 2003、 Windows 2000 或 Windows XP 的计算机。|
|/au|指定应为所有用户安装的服务配置文件。 此参数才有效运行 Windows Server 2003、 Windows 2000 或 Windows XP 的计算机。|
|/u|指定应卸载服务配置文件。|
|/?|在命令提示符下显示帮助。|
## <a name="remarks"></a>备注
**/s**是可以结合使用的唯一参数 **/u**。
语法 1 是自定义安装应用程序中使用的典型语法。 若要使用此语法，必须运行**cmstp**所在的目录从<ServiceProfileFileName>.exe 文件。
## <a name="BKMK_Examples"></a>示例
若要安装的小说服务配置文件而无需任何支持文件，请键入：
```
fiction.exe /c:"cmstp.exe fiction.inf /nf"
```
若要以无提示方式安装适用于单个用户的小说服务配置文件，请键入：
```
fiction.exe /c:"cmstp.exe fiction.inf /s /su"
```
若要以无提示方式卸载小说服务配置文件，请键入：
```
fiction.exe /c:"cmstp.exe fiction.inf /s /u"
```
## <a name="additional-references"></a>其他参考
-   [命令行语法解答](command-line-syntax-key.md)
