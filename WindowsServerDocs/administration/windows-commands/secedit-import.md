---
title: secedit：导入
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1dd59d4c-9d48-444a-871b-b957eb682597
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb90096c3483b44cc1285fa3531f47b2bf5c6d2f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384235"
---
# <a name="seceditimport"></a>secedit：导入



导入以前从配置了安全模板的数据库中导出的 inf 文件中存储的安全设置。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|db|必需。</br>指定数据库的路径和文件名，该数据库包含将在其中执行导入的存储配置。</br>如果文件名指定的数据库不具有与其关联的安全模板（由配置文件表示），则`/cfg \<configuration file name>`还必须指定命令行选项。|
|overwrite|可选。</br>指定/cfg 参数中的安全模板是否应覆盖数据库中存储的任何模板或复合模板，而不是将结果追加到存储的模板。</br>此命令行选项仅在使用`/cfg \<configuration file name>`参数时才有效。 如果未指定此参数，则将/cfg 参数中的模板追加到存储的模板。|
|cfg|必需。</br>指定将导入到数据库中进行分析的安全模板的路径和文件名。</br>此/cfg 选项仅在与`/db \<database file name>`参数一起使用时才有效。 如果未指定此项，则对已存储在数据库中的任何配置执行分析。|
|overwrite|可选。</br>指定/cfg 参数中的安全模板是否应覆盖数据库中存储的任何模板或复合模板，而不是将结果追加到存储的模板。</br>此命令行选项仅在使用`/cfg \<configuration file name>`参数时才有效。 如果未指定此参数，则将/cfg 参数中的模板追加到存储的模板。|
|区域|可选。</br>指定要应用于系统的安全区域。 如果未指定此参数，则会将数据库中定义的所有安全设置应用到系统。 若要配置多个区域，请用空格分隔每个区域。 支持以下安全区域：</br>-Ws-securitypolicy</br>    系统的本地策略和域策略，包括帐户策略、审核策略、安全选项等。</br>- Group_Mgmt</br>    在安全模板中指定的任何组的限制组设置。</br>- User_Rights</br>    用户登录权限和授予权限。</br>- RegKeys</br>    本地注册表项的安全性。</br>-%</br>    本地文件存储的安全性。</br>-服务</br>    所有已定义服务的安全性。|
|log|可选。</br>指定进程的日志文件的路径和文件名。|
|低温|可选。</br>禁止显示屏幕和日志输出。 你仍可以使用 Microsoft 管理控制台（MMC）的 "安全配置和分析" 管理单元查看分析结果。|

## <a name="remarks"></a>备注

在将 .inf 文件导入到另一台计算机之前，请在执行导入的数据库上运行命令 secedit/generaterollback，并在导入文件上对/validate 执行命令以验证其完整性。

如果未提供日志文件的路径，则使用默认的日志文件（*systemroot*\Documents 和 Settings\*用户帐户<em>\My Documents\Security\Logs\*DatabaseName</em>）。

在 Windows Server 2008 中`Secedit /refreshpolicy` ，已`gpupdate`替换为。 有关如何刷新安全设置的信息，请参阅[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>示例

将安全数据库和域安全策略导出到一个 inf 文件，然后将该文件导入到另一个数据库，以便在另一台计算机上复制安全策略设置。
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
仅将文件的安全策略部分导入另一台计算机上的其他数据库。
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>其他参考

-   [Secedit:export](secedit-export.md)
-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit:validate](secedit-validate.md)
-   [Secedit](secedit.md)
-   [命令行语法项](command-line-syntax-key.md)