---
title: secedit:generaterollback
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7a55ddc3caea1002ab51ce4f992b36673ea312b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825638"
---
# <a name="seceditgeneraterollback"></a>secedit:generaterollback



可以生成指定的配置模板的回滚模板。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Secedit /generaterollback /db <database file name> /cfg <configuration file name> /rbk <rollback template file name> [log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|db|必需。</br>指定包含存储的配置将对其执行分析的数据库的路径和文件。</br>如果文件的名称指定的数据库尚未与其关联一个安全模板 （如由配置文件），`/cfg \<configuration file name>`还必须指定命令行选项。|
|cfg|必需。</br>指定将导入到用于分析的数据库的安全模板的路径和文件。</br>此 /cfg 选项才与一起使用时有效`/db \<database file name>`参数。 如果未指定，则对已存储在数据库中的任何配置执行分析。|
|rbk|必需。</br>指定的回滚信息写入到其中一种安全模板。 使用安全模板管理单元创建安全模板。 可以使用以下命令创建回滚文件。|
|日志|可选。</br>指定进程的日志文件的路径和文件。|
|静默|可选。</br>禁止显示屏幕和日志输出。 您可以通过使用安全配置和分析管理单元为 Microsoft 管理控制台 (MMC) 的仍查看分析结果。|

## <a name="remarks"></a>备注

如果日志文件的路径未提供，默认日志文件 (*systemroot*\Users \*UserAccount*\My Documents\Security\Logs\*DatabaseName*.log) 使用。

从 Windows Server 2008 开始`Secedit /refreshpolicy`已替换为`gpupdate`。 有关如何刷新的安全设置的信息，请参阅[Gpupdate](gpupdate.md)。

成功运行此命令将状态"任务已成功完成。？ 和日志仅声明的安全模板和安全策略配置之间不匹配。 它列出了这些安全中的不匹配。

如果指定现有回滚模板，则此命令将覆盖它。 可以使用此命令创建新的回滚模板。 任何一种情况不需要任何其他参数。

## <a name="BKMK_Examples"></a>示例

创建使用的安全配置和分析管理单元中，SecTmplContoso.inf，安全模板后创建回滚配置文件将保存原始设置。 写出到 FY11 日志文件的操作。
```
Secedit /generaterollback /db C:\Security\FY11\SecDbContoso.sdb /cfg sectmplcontoso.inf /rbk sectmplcontosoRBK.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log
```

#### <a name="additional-references"></a>其他参考

-   [Secedit](secedit.md)
-   [命令行语法解答](command-line-syntax-key.md)