---
title: bitsadmin setaclflag
description: 适用于**bitsadmin setaclflag**的 Windows 命令主题-设置访问控制列表传播标志。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3bcda0-827d-4dfd-8384-d1da018f3e10
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fbdb12c29af7b4db8b25846d43ee1c93b2454ff2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380764"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

为作业设置访问控制列表（ACL）传播标志。 标志指示你希望在要下载的文件中维护所有者和 ACL 信息。 例如，若要通过文件维护所有者和组，请将 **Flags** to @no__t。

## <a name="syntax"></a>语法

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Flags|指定以下一个或多个标志值：</br>I/O将所有者信息复制到文件。</br>G用文件复制组信息。</br>2-D复制 DACL 信息和文件。</br>-S：复制具有文件的 SACL 信息。|

## <a name="remarks"></a>备注

当作业从 Windows （SMB）共享下载数据时，SetAclFlags 开关用于维护所有者和访问控制列表信息。

## <a name="BKMK_examples"></a>示例

下面的示例设置名为*myDownloadJob*的作业的访问控制列表传播标志，以使用下载的文件维护所有者和组信息。
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)