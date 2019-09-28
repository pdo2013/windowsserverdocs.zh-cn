---
title: 在 NPS 中使用正则表达式
description: 本主题说明如何将正则表达式用于 Windows Server 中 NPS 中的模式匹配。 您可以使用此语法来指定网络策略属性和 RADIUS 领域的条件。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: jgerend
author: jasongerend
msdate: 08/16/2019
ms.openlocfilehash: 94bb9b54dba046c57c6f82e6a52a71adbcf4ce75
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396374"
---
# <a name="use-regular-expressions-in-nps"></a>在 NPS 中使用正则表达式

> 适用于：Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

本主题说明如何将正则表达式用于 Windows Server 中 NPS 中的模式匹配。 您可以使用此语法来指定网络策略属性和 RADIUS 领域的条件。

## <a name="pattern-matching-reference"></a>模式匹配引用

使用模式匹配语法创建正则表达式时，可以使用下表作为参考源。 请注意，正则表达式模式通常由正斜杠（/）括起来。

|  字符  |  描述  |   示例                                                                 |
| ----------- | ------------- | ------------------------------------------------------------------------  |
|     `\ `     | 指示后面的字符是特殊字符，或应按原义解释。  | `/n/ matches the character "n" while the sequence /\n/ matches a line feed or newline character.`  |
|     `^`     |                                                                 匹配输入或行的开头。                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    匹配输入或行的末尾。                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             匹配前面的字符零次或多次。                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              匹配前面的字符一次或多次。                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              匹配前面的字符零次或一次。                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           匹配除换行符外的任何单个字符。                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         匹配 "pattern" 并记住匹配。<br />若要匹配文本字符 `(` 和 `)` （括号），请使用 `\(` 或 `\)`。                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               与 x 或 y 匹配。                                                          |
|   `{n} `    |                                                          精确匹配 n 次 \(n 为非 @ no__t-1negative integer @ no__t。                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          至少匹配 n 次，@no__t 0n 为非 @ no__t-1negative integer @ no__t。                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                至少匹配 n 次，至多 \(m，n 为非 @ no__t-1negative integer @ no__t。                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       匹配任何一个包含的字符 \(a 字符集 @ no__t-1。                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  匹配不包含 \(a 负字符集 @ no__t-1 的任何字符。                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              匹配字边界 \(for 示例，空格 @ no__t-1。                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         与非单词边界匹配。                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       匹配 \(equivalent 的数字字符到从0到 9 @ no__t 的数字。                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           匹配 nondigit 字符 \(equivalent 到 `[^0-9]` @ no__t。                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        匹配换页符。                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        匹配换行符。                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     匹配回车符。                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   将所有空格字符（包括空格、制表符和换 @no__t 页符）与 `[ \f\n\r\t\v]` @ no__t-2 的0equivalent 匹配。                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  匹配任何非空白字符 \(equivalent 到 `[^ \f\n\r\t\v]` @ no__t-2。                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           匹配制表符。                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      匹配垂直制表符。                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              匹配任意单词字符，其中包括下划线 \(equivalent 到 `[A-Za-z0-9_]` @ no__t-2。                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           匹配任何非 @ no__t-0word 字符，不包括下划线 \(equivalent 到 `[^A-Za-z0-9_]` @ no__t。                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | 引用记住的匹配项 \( @ no__t-1，其中 num 为正整数 @ no__t-2。  此选项只能在配置属性操作时在 "**替换**" 文本框中使用。 |                                       `\1` 替换首次记住的匹配中存储的内容。                                       |
|   `/n/ `    |                      允许将 ASCII 代码插入正则表达式中 \( @ no__t-1，其中 n 是八进制、十六进制或十进制转义值 @ no__t-2。                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>网络策略属性的示例

以下示例描述了如何使用模式匹配语法来指定网络策略属性：

- 若要指定899区号内的所有电话号码，语法为：

     `899.*`

- 若要指定以192.168.1 开头的 IP 地址范围，语法为：

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>用户名属性中的领域名称操作示例

下面的示例说明如何使用模式匹配语法来操作用户名属性的领域名称，该属性位于连接请求策略属性中的 "**属性**" 选项卡上。

**删除用户名属性的领域部分**

在外包拨号方案中，Internet 服务提供商 \(ISP @ no__t 将连接请求路由到组织 NPS，ISP RADIUS 代理可能需要使用领域名来路由身份验证请求。 但是，NPS 可能无法识别用户名的领域名称部分。 因此，在将领域名称转发到组织 NPS 之前，必须由 ISP RADIUS 代理将其删除。

- Find： @microsoft @ no__t-1com

- 将：

**替换<em>user@example.microsoft.com</em> _com\user_**

- 查找： `(.*)@(.*)`

- 替换： `$2\$1`



**将_domain\user_替换为_specific_domain\user_**

- 查找： `(.*)\\(.*)`

- Replace： *specific_domain*`\$2`



<strong>用 *user@specific_domain</strong>替换*用户**

- 查找： `$`

- Replace： @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>代理服务器发送 RADIUS 消息的示例

将 NPS 用作 RADIUS 代理时，可以创建将具有指定领域名称的 RADIUS 消息转发到一组 RADIUS 服务器的路由规则。 下面是基于领域名称路由请求的推荐语法。

- **NetBIOS 名称**： `WCOAST`
- **模式**： `^wcoast\\`

在以下示例中，wcoast.microsoft.com 是 DNS 或 Active Directory 域 wcoast.microsoft.com 的唯一用户主体名称（UPN）后缀。 NPS 代理使用提供的模式基于域 NetBIOS 名称或 UPN 后缀来路由消息。

- **NetBIOS 名称**： `WCOAST`
- **UPN 后缀**： `wcoast.microsoft.com`
- **模式**： `^wcoast\\|@wcoast\.microsoft\.com$`


有关管理 NPS 的详细信息，请参阅[管理网络策略服务器](nps-manage-top.md)。

有关 NPS 的详细信息，请参阅[网络策略服务器（NPS）](nps-top.md)。
