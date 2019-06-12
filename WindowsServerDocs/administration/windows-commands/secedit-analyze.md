---
title: secedit： 分析
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3430cf9d-1411-48b1-b5a9-2e47701dc87f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9122c5c0fa8c42b0ccfc77ceb3f2d337b44ee5dc
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441558"
---
# <a name="seceditanalyze"></a>secedit： 分析



可以分析针对存储在数据库的基准设置的当前系统设置。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Secedit /analyze /db <database file name> [/cfg <configuration file name>] [/overwrite] [/log <log file name>] [/quiet}]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|db|必需。</br>指定包含存储的配置将对其执行分析的数据库的路径和文件。</br>如果文件的名称指定的数据库尚未与其关联一个安全模板 （如由配置文件），`/cfg \<configuration file name>`还必须指定命令行选项。|
|cfg|可选。</br>指定将导入到用于分析的数据库的安全模板的路径和文件。</br>此 /cfg 选项才与一起使用时有效`/db \<database file name>`参数。 如果未指定，则对已存储在数据库中的任何配置执行分析。|
|overwrite|可选。</br>指定的任何模板或存储在数据库而不是将结果追加到存储的模板中的复合模板是否应覆盖 /cfg 参数中的安全模板。</br>此命令行选项才有效`/cfg \<configuration file name>`也使用参数。 如果未指定，/cfg 参数中的模板被追加到存储的模板。|
|日志|可选。</br>指定要在过程中使用的日志文件的路径和文件。|
|静默|可选。</br>禁止显示屏幕输出。 您可以通过使用安全配置和分析管理单元为 Microsoft 管理控制台 (MMC) 的仍查看分析结果。|

## <a name="remarks"></a>备注

分析结果存储在数据库的一个独立区域中，可以在安全配置和分析中的管理单元 MMC。

如果日志文件的路径未提供，默认日志文件 (*systemroot*\Documents 和设置\*UserAccount<em>\My Documents\Security\Logs\*DatabaseName</em>。使用日志）。

在 Windows Server 2008`Secedit /refreshpolicy`已替换为`gpupdate`。 有关如何刷新的安全设置的信息，请参阅[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>示例

执行分析上，对安全数据库 SecDbContoso.sdb，您使用的安全配置和分析管理单元中创建的安全参数。 将输出定向到文件并使你可以验证该命令提示您进行 SecAnalysisContosoFY11 正常运行。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /log C:\Security\FY11\SecAnalysisContosoFY11.log
```
让我们假设分析，因此修改了安全模板，SecContoso.inf，会显示一些不能满足。 运行命令以纳入更改，将定向到现有文件 SecAnalysisContosoFY11 并没有提示您进行输出。
```
Secedit /analyze /db C:\Security\FY11\SecDbContoso.sdb /cfg SecContoso.inf /overwrite /log C:\Security\FY11\SecAnalysisContosoFY11.xml /quiet
```

#### <a name="additional-references"></a>其他参考

-   [Secedit](secedit.md)
-   [命令行语法项](command-line-syntax-key.md)