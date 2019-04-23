---
title: graftabl
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b08351d4-3d24-490c-86f6-1252da11d923
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 395873cf3dbeb574dd9abc69f45b410bece80c25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848438"
---
# <a name="graftabl"></a>graftabl



使 Windows 操作系统，以显示在图形模式下设置为扩展的字符。 如果使用不带参数， **graftabl**显示以前和当前代码页。

有关如何使用此命令的示例，请参阅[示例](#BKMK_examples)。

## <a name="syntax"></a>语法

```
graftabl <CodePage>
graftabl /status
```

## <a name="parameters"></a>Parameters

|参数|描述|
|---------|-----------|
|\<CodePage>|指定代码页，可以在图形模式下定义的扩展字符的外观。</br>有效的代码页识别号是：</br>437:美国</br>850:多语言 (拉丁文我)</br>852:西里尔语 （俄语）</br>855:西里尔语 （俄语）</br>857:土耳其语</br>860:葡萄牙语</br>861:冰岛语</br>863:加拿大法语</br>865:北欧</br>866:俄语</br>869:现代希腊语|
|/status|显示当前代码页**graftabl**正在使用。|
|/?|在命令提示符下显示帮助。|

## <a name="remarks"></a>备注

-   **Graftabl**会影响你指定的代码页的扩展字符的监视器显示。 它不会更改实际控制台输入的代码页。 若要更改控制台输入的代码页，请使用**模式下**或**chcp**命令。
-   下表列出了每个退出代码和它的简要描述。  
    |退出代码|描述|
    |---------|-----------|
    |0|已成功加载字符集。 已不加载任何以前的代码页。|
    |1|指定了不正确的参数。 未采取任何操作。|
    |2|出现文件错误。|
-   您可以在进程退出代码，以返回到批处理程序中使用 ERRORLEVEL 环境变量**graftabl**。

## <a name="BKMK_examples"></a>示例

若要查看使用的当前代码页**graftabl**，类型：
```
graftabl /status
```
若要加载到内存中的代码页 437 （美国） 图形字符集，请键入：
```
graftabl 437
```
若要加载的图形字符集代码页 850 （多语言） 到内存中，键入：
```
graftabl 850
```

#### <a name="additional-references"></a>其他参考

[命令行语法解答](command-line-syntax-key.md)

[freedisk](freedisk.md)

[Chcp](chcp.md)