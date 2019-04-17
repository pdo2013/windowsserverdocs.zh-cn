---
title: 在 NPS 使用常规表情
description: 本主题介绍常规表情用于模式在 Windows Server 2016 中 NPS 匹配。 你可以使用此语法指定网络策略特性和 RADIUS 领域的条件。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f84173f1f51be9fd44995dc41f759bbea4fb3539
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/28/2018
---
# <a name="use-regular-expressions-in-nps"></a>在 NPS 使用常规表情

>适用于：Windows Server（半年通道），Windows Server 2016

本主题介绍常规表情用于模式在 Windows Server 2016 中 NPS 匹配。 你可以使用此语法指定网络策略特性和 RADIUS 领域的条件。

## <a name="pattern-matching-reference"></a>模式匹配参考

创建模式匹配语法常规表情时，可以使用下表为参考的源。

|字符|描述|示例|
|---------|-----------|-------|
|`\`  |标记为字符匹配的下一个字符。 |`/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`  |
|`^`  |匹配输入或行的开始。 | &nbsp; |
|`$`  |匹配的末尾输入或行。 | &nbsp; |
|`*`  |匹配前面的字符或有更多时间。 |`/zo*/ matches either "z" or "zoo."` |
|`+`  |匹配前面的字符一个或多个时间。 |`/zo+/ matches "zoo" but not "z."` |
|`?`  |匹配上述字符零或一次。 |`/a?ve?/ matches the "ve" in "never."` |
|`.`  |匹配除外换行符任何一个字符。  | &nbsp; |
|`( pattern )`  |匹配"模式"，并会记住匹配。   |`To match ( ) (parentheses), use "\(" or "\)".`  |
|x | y  |匹配 x 或 y。  |/z|美食？/ 匹配"动物园"或"美食。"` |
|`{ n } `  |匹配完全 n 次数 \（n 是 non\ 负极 integer\）。  |`/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`  |
|`{ n ,}`  |匹配 n 至少次数 \（n 是 non\ 负极 integer\）。  |`/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.`  |
|`{ n , m }`  |匹配至少 n 和在大多数 m 时间 \（m 和 n 均 non\ 负极 integers\）。  |`/o{1,3}/ matches the first three instances of the letter o in "fooooood."`  |
|`[ xyz ]`  |匹配的任何一个随附的字符 \(a character set\)。  |`/[abc]/ matches the "a" in "plain."`  |
|`[^ xyz ]`  |匹配不包含任何字符 \ (负字符 set\)。  |`/[^abc]/ matches the "p" in "plain."`  |
|`\b`  |匹配 word 边界 \ (例如，space\)。  |`/ea*r\b/ matches the "er" in "never early."`  |
|`\B`  |非单词边界匹配。  |`/ea*r\B/ matches the "ear" in "never early."`  |
|`\d`  |数字字符匹配 \（相当于数字 0 到 9\ 从）。  | &nbsp; |
|`\D`  |匹配非数字字符 \ (等同于`[^0-9]`\)。  | &nbsp; |
|`\f`  |匹配窗体哺育字符。  | &nbsp; |
|`\n`  |匹配项换行符。  | &nbsp; |
|`\r`  |匹配托架退货字符。  | &nbsp; |
|`\s`  |匹配包括空间，选项卡，然后源的形式的任何空白区域字符 \ (等同于`[ \f\n\r\t\v]`\)。  | &nbsp; |
|`\S`  |匹配的任何空白区域字符 \ (等同于`[^ \f\n\r\t\v]`\)。  | &nbsp; |
|`\t`  |匹配的选项卡字符。  | &nbsp; |
|`\v`  |匹配垂直的选项卡字符。  | &nbsp; |
|`\w`  |匹配的任何 word 字符，包括下划线 \ (等同于`[A-Za-z0-9_]`\)。  | &nbsp; |
|`\W`  |匹配的任何 non\ word 字符，除下划线 \ (等同于`[^A-Za-z0-9_]`\)。  | &nbsp; |
|`\ num`  |请参考记住匹配 \ (`?num`，其中数字是正 integer\)。  可以使用此选项仅在**替换**配置属性操作时的文本框。| `\1` 将替换中的第一个记住匹配存储的内容。  |
|`/ n / `  |允许 ASCII 代码插入常规表情 \ (`?n`、n 所在八进制、十六进制或小数点转义 value\)。  | &nbsp; |

## <a name="examples-for-network-policy-attributes"></a>网络策略特性的示例

下面的示例中介绍了如何使用指定网络策略特性模式匹配语法：

- 若要指定 899 区域代码内的所有电话号码，语法是：

     `899.*`

- 若要指定的 192.168.1 开始范围内的 IP 地址，语法是：

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>在用户名属性领域名操作的示例

下面示例介绍如何使用的模式匹配语法操作领域名位于用户名属性**特性**连接请求策略属性选项卡。

**若要删除的领域部分 User Name 属性**

在此时 Internet 服务提供商 \(ISP\) 路线连接请求到组织 NPS 服务器，ISP RADIUS 代理服务器可能需要领域名身份验证请求路由外包拨号方案。 但是，NPS 服务器可能无法识别的用户名的领域名称部分。 因此，领域名称必须 ISP RADIUS 代理通过删除组织 NPS 服务器转发之前。

- 查找：@microsoft\.com

- 替换：

**若要更换*user@example.microsoft.com*与*example.microsoft.com\user***

- 查找：`(.*)@(.*)`

- 替换：`$2\$1`



**若要更换*域名 \*与*specific_domain\user***

- 查找：`(.*)\\(.*)`

- 替换：*specific_domain*`\$2`



**若要更换*用户*与*user@specific_domain***

- 查找：`$`

- 替换: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>示例为 RADIUS 邮件转发由代理服务器的

你可以创建转发到一组 RADIUS 服务器同名指定的领域 RADIUS 消息 NPS RADIUS 代理用作时的路由规则。 下面是为基于领域名称路由请求推荐的语法。

- **NetBIOS 名称 **: `WCOAST`
- **模式 **:      `^wcoast\\`

在以下示例中，wcoast.microsoft.com 是 DNS 或 Active Directory 域 wcoast.microsoft.com 唯一的用户主要名称 (UPN) 职务。使用提供的模式，NPS 代理可以发送根据 NetBIOS 域名或 UPN 职务的消息。

- **NetBIOS 名称 **: `WCOAST`
- **UPN 职务 **:   `wcoast.microsoft.com`
- **模式 **:      `^wcoast\\|@wcoast\.microsoft\.com$`


有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略 Server (NPS)](nps-top.md)。
