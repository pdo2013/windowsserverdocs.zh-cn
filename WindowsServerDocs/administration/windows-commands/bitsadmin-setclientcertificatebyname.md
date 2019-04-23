---
title: bitsadmin setclientcertificatebyname
description: Windows 命令主题**bitsadmin setclientcertificatebyname** -指定要使用 HTTPS (SSL) 请求中的客户端身份验证的客户端证书的使用者名称。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f308a6d9-d0da-48be-ae41-eced14b3cccb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 76150ccb34693eb692d27efbd6538f5363ba1c26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871308"
---
# <a name="bitsadmin-setclientcertificatebyname"></a>bitsadmin setclientcertificatebyname



指定要使用 HTTPS (SSL) 请求中的客户端身份验证的客户端证书的使用者名称。

## <a name="syntax"></a>语法

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> <subject_name>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Store_location|标识要用于查找证书的系统存储位置。 可能值的包括：</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVICES)</br>5 （用户）</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|证书存储区的名称。 可能值的包括：</br>CA （证书颁发机构证书）</br>我 （个人证书）</br>根 （根证书）</br>SPC （软件发行者证书）|
|Subject_name|证书的名称|

## <a name="BKMK_examples"></a>示例

下面的示例指定客户端证书的名称*myCertificate*要用于 HTTPS (SSL) 请求名为的作业中的客户端身份验证*myJob*。
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByName myJob 1 MY myCertificate 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)