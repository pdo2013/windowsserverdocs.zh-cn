---
title: Scwcmd 转换
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a6a6e37c2c2a362f3aa0aeadef615ff5065713f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843808"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> 适用于：Windows Server 2012 R2, Windows Server 2012

转换到新组策略对象 (GPO) Active Directory 域服务中使用安全配置向导 (SCW) 生成的安全策略文件。 转换操作不更改其执行的服务器上的任何设置。 转换操作完成后，管理员必须将 GPO 链接到所需的 Ou，若要将策略部署到服务器。

完成转换操作需要域管理员凭据。

> [!IMPORTANT]
> 不能通过使用组策略部署 Internet 信息服务 (IIS) 安全策略设置。</br>> 防火墙策略，除非 Windows 防火墙服务自动启动服务器时进行最后一次后，不应列出批准的应用程序到服务器中部署已启动。

有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/p:\<Policyfile.xml >|指定应该应用的.xml 策略文件的路径和文件。 必须指定此参数。|
|/g:\<GPODisplayName>|指定 GPO 的显示名称。 必须指定此参数。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

Scwcmd.exe 才可在运行 Windows Server 2008 R2、 Windows Server 2008 或 Windows Server 2003 的计算机上。

## <a name="BKMK_Examples"></a>示例

若要创建一个名为 FileServerSecurity 从名为 FileServerPolicy.xml 的文件的 GPO，请键入：
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>其他参考

-   [命令行语法解答](command-line-syntax-key.md)