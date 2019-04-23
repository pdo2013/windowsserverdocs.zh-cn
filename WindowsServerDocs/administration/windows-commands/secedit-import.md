---
title: Secedit:import
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f24cf173d1bacd70d92b325bfe7b342d0589a490
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874278"
---
# <a name="seceditimport"></a>Secedit:import



导入之前导出配置使用安全模板的数据库从一个 inf 文件中存储的安全设置。 有关如何使用此命令的示例，请参阅[示例](#BKMK_Examples)。

## <a name="syntax"></a>语法

```
Secedit /import /db <database file name> /cfg <configuration file name> [/overwrite] [/areas [securitypolicy | group_mgmt | user_rights | regkeys | filestore | services]] [/log <log file name>] [/quiet]

```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|db|必需。</br>指定包含将在其中执行导入的存储的配置的数据库的路径和文件。</br>如果文件的名称指定的数据库尚未与其关联一个安全模板 （如由配置文件），`/cfg \<configuration file name>`还必须指定命令行选项。|
|overwrite|可选。</br>指定的任何模板或存储在数据库而不是将结果追加到存储的模板中的复合模板是否应覆盖 /cfg 参数中的安全模板。</br>此命令行选项才有效`/cfg \<configuration file name>`也使用参数。 如果未指定，/cfg 参数中的模板被追加到存储的模板。|
|cfg|必需。</br>指定将导入到用于分析的数据库的安全模板的路径和文件。</br>此 /cfg 选项才与一起使用时有效`/db \<database file name>`参数。 如果未指定，则对已存储在数据库中的任何配置执行分析。|
|overwrite|可选。</br>指定的任何模板或存储在数据库而不是将结果追加到存储的模板中的复合模板是否应覆盖 /cfg 参数中的安全模板。</br>此命令行选项才有效`/cfg \<configuration file name>`也使用参数。 如果未指定，/cfg 参数中的模板被追加到存储的模板。|
|区域|可选。</br>指定要应用到系统的安全方面。 如果未指定此参数，在数据库中定义的所有安全设置都应用到系统。 若要配置多个区域，每个空格分隔区域。 支持以下安全区域：</br>-   SecurityPolicy</br>    本地策略和系统，包括帐户策略的域策略审核策略、 安全选项等。</br>-   Group_Mgmt</br>    安全模板中指定的任何组的受限制的组设置。</br>-User_Rights</br>    用户登录权限和特权。</br>-RegKeys</br>    本地注册表项的安全性。</br>-文件存储</br>    本地文件存储的安全。</br>-服务</br>    所有已定义的服务的安全性。|
|日志|可选。</br>指定进程的日志文件的路径和文件。|
|静默|可选。</br>禁止显示屏幕和日志输出。 您可以通过使用安全配置和分析管理单元为 Microsoft 管理控制台 (MMC) 的仍查看分析结果。|

## <a name="remarks"></a>备注

在导入到另一台计算机上的.inf 文件之前, 运行命令 secedit /generaterollback 对数据库将执行导入和 secedit / 导入文件以验证其完整性验证。

如果日志文件的路径未提供，默认日志文件 (*systemroot*\Documents 和设置\*UserAccount*\My Documents\Security\Logs\*DatabaseName*。使用日志）。

在 Windows Server 2008`Secedit /refreshpolicy`已替换为`gpupdate`。 有关如何刷新的安全设置的信息，请参阅[Gpupdate](gpupdate.md)。

## <a name="BKMK_Examples"></a>示例

将安全数据库和域安全策略导出到 inf 文件，然后将该文件以复制另一台计算机上的安全策略设置导入不同的数据库。
```
Secedit /export /db C:\Security\FY11\SecDbContoso.sdb /mergedpolicy /cfg NetworkShare\Policies\SecContoso.inf /log C:\Security\FY11\SecAnalysisContosoFY11.log /quiet
```
导入到另一台计算机上的不同数据库的只是安全策略的文件的部分。
```
Secedit /import /db C:\Security\FY12\SecDbContoso.sdb /cfg NetworkShare\Policies\SecContoso.inf /areas securitypolicy /log C:\Security\FY11\SecAnalysisContosoFY12.log /quiet
```

#### <a name="additional-references"></a>其他参考

-   [Secedit:export](secedit-export.md)
-   [secedit:generaterollback](secedit-generaterollback.md)
-   [secedit： 验证](secedit-validate.md)
-   [Secedit](secedit.md)
-   [命令行语法解答](command-line-syntax-key.md)