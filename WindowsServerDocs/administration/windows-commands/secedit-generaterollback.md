---
title: secedit： generaterollback
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 385a6799-51a7-4fe3-bd73-10c7998b6680
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ce4bd83e6eda24c10f65bd9d450a204906ff7fd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384218"
---
# <a name="seceditgeneraterollback"></a>secedit： generaterollback



允许您为指定的配置模板生成回滚模板。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|db|必需。</br>指定数据库的路径和文件名，该数据库包含将对其执行分析的存储配置。</br>如果文件名指定的数据库不具有与其关联的安全模板（由配置文件表示），则`/cfg \<configuration file name>`还必须指定命令行选项。|
|cfg|必需。</br>指定将导入到数据库中进行分析的安全模板的路径和文件名。</br>此/cfg 选项仅在与`/db \<database file name>`参数一起使用时才有效。 如果未指定此项，则对已存储在数据库中的任何配置执行分析。|
|rbk|必需。</br>指定要向其中写入回滚信息的安全模板。 安全模板是使用 "安全模板" 管理单元创建的。 可以通过此命令创建回滚文件。|
|log|可选。</br>指定进程的日志文件的路径和文件名。|
|低温|可选。</br>禁止显示屏幕和日志输出。 你仍可以使用 Microsoft 管理控制台（MMC）的 "安全配置和分析" 管理单元查看分析结果。|

## <a name="remarks"></a>备注

如果未提供日志文件的路径，则使用默认日志文件（*systemroot*\Users \*用户帐户<em>\My Documents\Security\Logs\*DatabaseName</em>）。

从 Windows Server 2008 开始， `Secedit /refreshpolicy`已`gpupdate`替换为。 有关如何刷新安全设置的信息，请参阅[Gpupdate](gpupdate.md)。

成功运行此命令将会出现 "任务已成功完成"。 和仅记录规定的安全模板和安全策略配置之间的不匹配。 它列出了 scesrv.dll 中的这些不匹配。

如果指定了现有的回滚模板，则此命令将覆盖它。 可以使用此命令创建新的回滚模板。 这两个条件都不需要其他参数。

## <a name="BKMK_Examples"></a>示例

使用 "安全配置和分析" 管理单元（SecTmplContoso）创建安全模板之后，请创建回滚配置文件以保存原始设置。 将操作写出到 FY11 日志文件。
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>其他参考

-   [Secedit](secedit.md)
-   [命令行语法项](command-line-syntax-key.md)