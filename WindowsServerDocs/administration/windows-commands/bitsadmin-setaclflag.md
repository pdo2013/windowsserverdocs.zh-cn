---
title: bitsadmin setaclflag
description: Windows 命令主题**bitsadmin setaclflag** -设置访问控制列表传播标志。
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 89d825a4bc4512022fed98a3188537d3977fa3c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867398"
---
# <a name="bitsadmin-setaclflag"></a>bitsadmin setaclflag

设置访问控制列表 (ACL) 为的作业的传播标志。 标志指示你想要保持与所下载的文件的所有者和 ACL 的信息。 例如，若要保持与该文件的所有者和组，设置 **标志** 到`OG`。

## <a name="syntax"></a>语法

```
bitsadmin /SetAclFlags <Job> <Flags>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Flags|指定一个或多个以下的标志值：</br>-O:与文件复制所有者信息。</br>-G:与文件复制组信息。</br>-D:与文件复制 DACL 信息。</br>-S： 复制 SACL 信息与文件。|

## <a name="remarks"></a>备注

SetAclFlags 开关用于作业从 Windows (SMB) 共享下载数据时维护所有者和访问控制列表信息。

## <a name="BKMK_examples"></a>示例

下面的示例设置的访问控制列表传播标志名为的作业*myDownloadJob*为了保持与下载的文件的所有者和组信息。
```
C:\>bitsadmin /setaclflags myDownloadJob OG
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)