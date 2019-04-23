---
title: bitsadmin setclientcertificatebyid
description: Windows 命令主题**bitsadmin setclientcertificatebyid**指定要使用 HTTPS (SSL) 请求中的客户端身份验证的客户端证书的标识符
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8585a7a1-7472-437b-b04a-a11925782a3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2424de18ee8aaec73b086207e8ef56d85df862fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863928"
---
# <a name="bitsadmin-setclientcertificatebyid"></a>bitsadmin setclientcertificatebyid



指定要使用 HTTPS (SSL) 请求中的客户端身份验证的客户端证书的标识符。

## <a name="syntax"></a>语法

```
bitsadmin /SetClientCertificateByID <Job> <store_location> <store_name> hexa-decimal_cert_id>
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|作业|该作业的显示名称或 GUID|
|Store_location|标识要用于查找证书的系统存储位置。 可能值的包括：</br>1 (CURRENT_USER)</br>2 (LOCAL_MACHINE)</br>3 (CURRENT_SERVICE)</br>4 (SERVICES)</br>5 （用户）</br>6 (CURRENT_USER_GROUP_POLICY)</br>7 (LOCAL_MACHINE_GROUP_POLICY)</br>8 (LOCAL_MACHINE_ENTERPRISE)|
|Store_name|证书存储区的名称。 可能值的包括：</br>CA （证书颁发机构证书）</br>我 （个人证书）</br>根 （根证书）</br>SPC （软件发行者证书）|
|Hexadecimal_cert_id|一个表示证书的哈希的十六进制数字|

## <a name="BKMK_examples"></a>示例

下面的示例指定要使用 HTTPS (SSL) 请求名为的作业中的客户端身份验证的客户端证书的标识符*myJob*。
```
C:\>bitsadmin Bitsadmin /SetClientCertificateByID myJob BG_CERT_STORE_LOCATION_CURRENT_USER MY A106B52356D3FBCD1853A41B619358BD 
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)