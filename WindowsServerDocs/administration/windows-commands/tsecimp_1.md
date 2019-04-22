---
title: tsecimp
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 38582706dfa5db2b5069415b81dafc533c8a89b9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822098"
---
# <a name="tsecimp"></a>tsecimp



将分配信息从可扩展标记语言 (XML) 文件导入 TAPI 服务器安全文件 (Tsec.ini)。 您还可以使用此命令显示 TAPI 提供程序的列表以及与每个线路设备关联、 验证 XML 文件的结构，无需导入内容，并检查域成员身份。

## <a name="syntax"></a>语法

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|/f \<Filename>|必需。 指定包含要导入的分配信息的 XML 文件的名称。|
|/v|在不将信息导入 Tsec.ini 文件验证 XML 文件的结构。|
|/u|检查每个用户是否为 XML 文件中指定的域的成员。 在其使用此参数的计算机必须连接到网络。 如果要处理大量的用户分配信息，此参数可能会显著降低性能。|
|/d|显示已安装电话服务提供程序的列表。 每个电话服务提供程序，均会列出关联的线路设备，以及地址和与每个线路设备关联的用户。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   你想要导入分配信息的 XML 文件必须遵循下面所述的结构。  
    -   **UserList**元素

        **UserList**是 XML 文件的顶级元素。
    -   **用户**元素

        每个**用户**元素包含有关用户的域成员身份的信息。 每个用户可能会分配一个或多个线路设备。

        此外，每个**用户**元素可能具有一个名为属性**NoMerge**。 当指定此属性时，进行新的之前删除所有当前的线路设备分配的用户。 可以使用此属性可轻松删除不需要的用户分配。 默认情况下，未设置此属性。

        **用户**元素必须包含一个**DomainUserName**元素，它指定用户的域和用户。 **用户**元素还可能包含一个**FriendlyName**元素，它指定用户的友好名称。

        **用户**元素可能包含一个**LineList**元素。 如果**LineList**元素不存在，请删除此用户的所有线路设备。
    -   **LineList**元素

        **LineList**元素包含有关每个行或可能分配给用户的设备的信息。 每个**LineList**元素可以包含多个**行**元素。
    -   **行**元素

        每个**行**元素指定一个线路设备。 必须通过添加任何一个来标识每个线路设备**地址**元素或**PermanentID**元素下的**行**元素。

        每个**行**元素，可以设置**删除**属性。 如果设置此属性，用户无法再分配该线路设备。 如果未设置此属性，用户可访问该线路设备。 未向用户提供该线路设备时，会给不出任何错误。

## <a name="examples"></a>示例
-   以下 XML 代码段示例说明了上面定义的元素的正确用法。  
    -   下面的代码中删除分配给 User1 的所有线路设备。  
        ```
        <UserList>
          <User NoMerge="1">
            <DomainUser>domain1\user1</DomainUser>
          </User>
        </UserList>
        ```  
    -   下面的代码中删除分配地址为 99999 的一行之前分配给 User1 的所有线路设备。 User1 有任何其他线路设备分配，无论是否以前分配任何线路设备。  
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
    -   下面的代码而不会删除任何以前分配的线路设备，为 User1 添加一个线路设备。  
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
    -   以下代码添加了线路地址 99999，并从 User1 的访问权限中删除了线路地址 88888。  
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
    -   以下代码添加了永久性设备 1000年和 User1 的访问删除了线路地址 88888。  
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
-   以下示例输出显示后 **/d**指定命令行选项以显示当前的 TAPI 配置。 每个电话服务提供程序，均会列出关联的线路设备，以及地址和与每个线路设备关联的用户。  
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

[命令行语法解答](command-line-syntax-key.md)

[命令行解释器概述](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)