---
title: tsecimp
description: '适用于 * * * * 的 Windows 命令主题 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1c596d6d24a611882c0ecf234c22c83a268ec53c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363932"
---
# <a name="tsecimp"></a>tsecimp



将可扩展标记语言 (XML) 文件中的分配信息导入到 TAPI 服务器安全文件 (Tsec.ini) 中。 你还可以使用此命令来显示 TAPI 提供程序和与每个设备关联的线路设备的列表, 验证 XML 文件的结构, 而无需导入内容以及检查域成员身份。

## <a name="syntax"></a>语法

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/f \<Filename >|必需。 指定包含要导入的分配信息的 XML 文件的名称。|
|/v|验证 XML 文件的结构, 而不将信息导入 Tsec.ini 文件。|
|/u|检查每个用户是否是 XML 文件中指定的域的成员。 使用此参数的计算机必须连接到网络。 如果要处理大量的用户分配信息, 此参数可能会显著降低性能。|
|/d|显示已安装电话服务提供程序的列表。 对于每个电话服务提供程序, 均会列出关联的线路设备, 以及与每个线路设备关联的地址和用户。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   要从中导入分配信息的 XML 文件必须遵循如下所述的结构。  
    -   **UserList**元素

        **UserList**是 XML 文件的顶级元素。
    -   **User**元素

        每个**User**元素都包含有关作为域成员的用户的信息。 可以为每个用户分配一个或多个线路设备。

        此外, 每个**用户**元素可能有一个名为**NoMerge**的属性。 如果指定此属性, 则在生成新的用户之前, 将删除该用户的所有当前线路设备分配。 可以使用此属性轻松删除不需要的用户分配。 默认情况下, 不设置此属性。

        **User**元素必须包含单个**DomainUserName**元素, 该元素指定用户的域和用户名。 **User**元素还可能包含一个**FriendlyName**元素, 该元素指定用户的友好名称。

        **User**元素可能包含一个**LineList**元素。 如果**LineList**元素不存在, 则将删除该用户的所有线路设备。
    -   **LineList**元素

        **LineList**元素包含有关可能分配给用户的每个线路或设备的信息。 每个**LineList**元素可以包含多个**Line**元素。
    -   **线条**元素

        每个**线条**元素都指定了线条设备。 必须通过在**line**元素下添加**Address**元素或**PermanentID**元素来标识每个行设备。

        对于每个**Line**元素, 可以设置**Remove**特性。 如果设置此属性, 则不再向用户分配该线路设备。 如果未设置此属性, 则用户将获得对该行设备的访问权限。 如果用户无法使用线路设备, 则不会提供错误。

## <a name="examples"></a>示例
- 下面的 XML 代码段示例说明了上面定义的元素的正确用法。  
  - 以下代码删除分配给 User1 的所有线路设备。  
    ```
    <UserList>
      <User NoMerge="1">
        <DomainUser>domain1\user1</DomainUser>
      </User>
    </UserList>
    ```  
  - 以下代码将删除分配给 User1 的所有线路设备, 然后再分配一个地址为99999的行。 无论先前是否分配了任何线路设备, User1 都不会为其分配其他线路设备。  
    ```
    <UserList>
      <User NoMerge="1">
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - 以下代码将为 User1 添加一个线路设备, 而不删除任何以前分配的线路设备。  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - 下面的代码添加行地址99999并从 User1's 访问中删除线路地址88888。  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
          <Line Remove="1">
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - 下面的代码添加永久设备1000并从 User1's 访问中删除行88888。  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <PermanentID>1000</PermanentID>
          </Line>
          <Line Remove="1">
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```

-   指定了 **/d**命令行选项以显示当前 TAPI 配置后, 会显示以下示例输出。 对于每个电话服务提供程序, 均会列出关联的线路设备, 以及与每个线路设备关联的地址和用户。  
    ```
    NDIS Proxy TAPI Service Provider
            Line: "WAN Miniport (L2TP)"
                    Permanent ID: 12345678910

    NDIS Proxy TAPI Service Provider
            Line: "LPT1DOMAIN1\User1"
                    Permanent ID: 12345678910

    Microsoft H.323 Telephony Service Provider
            Line: "H323 Line"
                    Permanent ID: 123456
                    Addresses:
                            BLDG1-TAPI32

    ```

#### <a name="additional-references"></a>其他参考

[命令行语法项](command-line-syntax-key.md)

[命令外壳概述](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)
