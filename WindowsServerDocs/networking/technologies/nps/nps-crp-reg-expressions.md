---
title: 在 NPS 中使用正则表达式
description: 本主题介绍有关 Windows Server 2016 中的 NPS 中的模式匹配使用正则表达式。 可以使用此语法来指定的网络策略属性和 RADIUS 领域的条件。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a985df9fea31e5ee180caef4e69899ae8468ff71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865258"
---
# <a name="use-regular-expressions-in-nps"></a>在 NPS 中使用正则表达式

>适用于：Windows 服务器 （半年频道），Windows Server 2016

本主题介绍有关 Windows Server 2016 中的 NPS 中的模式匹配使用正则表达式。 可以使用此语法来指定的网络策略属性和 RADIUS 领域的条件。

## <a name="pattern-matching-reference"></a>模式匹配参考

创建正则表达式模式匹配语法时，可以使用下表作为参考源。

|字符|描述|示例|
|---------|-----------|-------|
|`\`  |将下一个字符标记为要匹配的字符。 |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |匹配输入或行的开头。 | &nbsp; |
|`$`  |与输入或行的末尾匹配。 | &nbsp; |
|`*`  |匹配前面的字符零次或多次。 |`/zo*/ matches either "z" or "zoo."` |
|`+`  |匹配一个或多个前面的字符。 |`/zo+/ matches "zoo" but not "z."` |
|`?`  |匹配前面的字符零或一次。 |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |匹配任一单字符换行字符除外。  | &nbsp; |
|`(pattern)`  |匹配"模式"，并记住匹配。<br />若要匹配文本字符`(`并`)`（括号），使用`\(`或`\)`。   | &nbsp;  |
|`x|y `  |匹配 x 或 y。  |`/z|food?/ matches "zoo" or "food."` |
|`{n} `  |匹配正好 n 次\(n 是非\-负整数\)。  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{n,}`  |匹配至少 n 次\(n 是非\-负整数\)。  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{n,m}`  |匹配至少 n 个，最 m 次多次\(m 和 n 都包含非\-负整数\)。  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[xyz]`  |匹配任何一个括住的字符\(字符集\)。  |`/[abc]/ matches the "a" in "plain."`  |
|`[^xyz]`  |匹配不包含任何字符\(负字符组\)。  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |匹配字边界\(例如，空格\)。  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |与非字边界匹配。  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |匹配数字字符\(等效于数字从 0 到 9\)。  | &nbsp; |
|`\D`  |匹配非数字字符\(等效于`[^0-9]` \)。  | &nbsp; |
|`\f`  |匹配换页符。  | &nbsp; |
|`\n`  |匹配换行字符。  | &nbsp; |
|`\r`  |匹配回车符。  | &nbsp; |
|`\s`  |匹配任何空白字符，包括空间、 制表符和换页符\(等效于`[ \f\n\r\t\v]` \)。  | &nbsp; |
|`\S`  |匹配任何非空白字符\(等效于`[^ \f\n\r\t\v]` \)。  | &nbsp; |
|`\t`  |与制表符匹配。  | &nbsp; |
|`\v`  |与垂直制表符匹配。  | &nbsp; |
|`\w`  |匹配任何单词字符，包括下划线\(等效于`[A-Za-z0-9_]` \)。  | &nbsp; |
|`\W`  |匹配任何非\-单词字符，不包括下划线\(等效于`[^A-Za-z0-9_]` \)。  | &nbsp; |
|`\num`  |记住的匹配项是指\( `?num`，其中 num 是一个正整数\)。  可以使用此选项仅在**替换为**配置属性操作时的文本框。| `\1` 将替换第一个记住的匹配项中存储的内容。  |
|`/n/ `  |允许将 ASCII 代码插入正则表达式\( `?n`，其中 n 是八进制、 十六进制或十进制转义值\)。  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>网络策略属性的示例

下面的示例介绍如何使用模式匹配语法来指定网络策略属性：

- 若要指定 899 区号中的所有电话号码，语法为：

     `899.*`

- 若要指定以 192.168.1 开始一系列 IP 地址，语法为：

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>处理中的用户名属性的领域名称的示例

下面的示例介绍如何使用模式匹配语法来操作位于用户名称属性的领域名称**特性**的连接请求策略属性中的选项卡。

**若要删除的用户名属性的领域部分**

在外包拨号方案中的 Internet 服务提供商\(ISP\)将连接请求路由到组织 NPS，ISP RADIUS 代理可能需要使用领域名称来路由身份验证请求。 但是，NPS 不可能识别用户名的领域名称部分。 因此，领域名称必须通过 ISP RADIUS 代理删除之前将其转发到组织 NPS。

- 查找： @microsoft \.com

- 将：

**若要替换*user@example.microsoft.com*与*example.microsoft.com\user***

- 查找：`(.*)@(.*)`

- 将为：`$2\$1`



**若要替换*域 \ 用户*与*specific_domain\user***

- 查找：`(.*)\\(.*)`

- 替换： *specific_domain*`\$2`



**若要替换*用户*与 *user@specific_domain***

- 查找：`$`

- 替换: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>通过代理服务器转发 RADIUS 消息的示例

可以创建当 NPS 用作 RADIUS 代理时使用的一系列 RADIUS 服务器指定的领域名称的 RADIUS 消息转发的路由规则。 下面是根据领域名称路由请求的建议的语法。

- **NetBIOS 名称**: `WCOAST`
- **模式**:      `^wcoast\\`

在以下示例中，wcoast.microsoft.com 是 DNS 或 Active Directory 域 wcoast.microsoft.com 的唯一用户主体名称 (UPN) 后缀。 使用提供的模式，NPS 代理可以将消息路由基于域 NetBIOS 名称或 UPN 后缀。

- **NetBIOS 名称**: `WCOAST`
- **UPN 后缀**:   `wcoast.microsoft.com`
- **模式**:      `^wcoast\\|@wcoast\.microsoft\.com$`


有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器 (NPS)](nps-top.md)。
