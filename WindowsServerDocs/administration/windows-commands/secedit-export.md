---
title: secedit:export
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 49a8b241-aa8c-45b7-844d-67a29fab708e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 398d2fa47f2418aec910569c2eb85aec408ad482
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441588"
---
# <a name="seceditexport"></a>secedit:export



将导出存储在数据库中使用安全模板配置的安全设置。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Secedit /export /db <database file name> [/mergedpolicy] /cfg <configuration file name> [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|db|必需。</br>指定包含存储的配置将对其执行分析的数据库的路径和文件。</br>如果文件的名称指定的数据库尚未与其关联一个安全模板 （如由配置文件），`/cfg \<configuration file name>`还必须指定命令行选项。|
|mergedpolicy|可选。</br>将合并，并将导出域和本地策略安全设置。|
|cfg|必需。</br>指定将导入到用于分析的数据库的安全模板的路径和文件。</br>此 /cfg 选项才与一起使用时有效`/db \<database file name>`参数。 如果未指定，则对已存储在数据库中的任何配置执行分析。|
|区域|可选。</br>指定要应用到系统的安全方面。 如果未指定此参数，在数据库中定义的所有安全设置都应用到系统。 若要配置多个区域，每个空格分隔区域。 支持以下安全区域：</br>-   SecurityPolicy</br>    本地策略和系统，包括帐户策略的域策略审核策略、 安全选项等。</br>-   Group_Mgmt</br>    安全模板中指定的任何组的受限制的组设置。</br>-User_Rights</br>    用户登录权限和特权。</br>-RegKeys</br>    本地注册表项的安全性。</br>-文件存储</br>    本地文件存储的安全。</br>-服务</br>    所有已定义的服务的安全性。|
|日志|可选。</br>指定进程的日志文件的路径和文件。|
|静默|可选。</br>禁止显示屏幕和日志输出。 您可以通过使用安全配置和分析管理单元为 Microsoft 管理控制台 (MMC) 的仍查看分析结果。|

## <a name="remarks"></a>备注

此命令可用于备份您除了设置导入到另一台计算机的本地计算机上的安全策略。

如果日志文件的路径未提供，默认日志文件 (*systemroot*\Documents 和设置\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>。使用日志）。

在 Windows Server 2008`Secedit /refreshpolicy`已替换为`gpupdate`。 有关如何刷新的安全设置的信息，请参阅[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>示例

将安全数据库和域安全策略导出到 inf 文件，然后将该文件以复制另一台计算机上的安全策略设置导入不同的数据库。
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
该文件导入到另一台计算机上的其他数据库。
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>其他参考

-   [Secedit:import](secedit-import.md)
-   [Secedit](secedit.md)
-   [命令行语法项](command-line-syntax-key.md)