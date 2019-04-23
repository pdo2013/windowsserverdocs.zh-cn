---
title: verifier
description: 'Windows 命令主题 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 782173d6-f196-4bc4-a547-76109829453c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ab0833d4fdb11c4962d4916ec2e32097e08ca04
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865868"
---
# <a name="verifier"></a>verifier

>适用于：Windows 服务器 （半年频道），Windows Server 2016 中，Windows Server 2012 R2、 Windows Server 2012

驱动程序验证程序管理器。  

## <a name="syntax"></a>语法  
```  
verifier /standard /driver <name> [<name> ...]  
verifier /standard /all  
verifier [/flags <flags>] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /driver <name> [<name>...]  
verifier [/flags FLAGS] [/faults [<probability> [<tags> [<applications> [<minutes>]]]] /all  
verifier /querysettings  
verifier /volatile /flags <flags>  
verifier /volatile /adddriver <name> [<name>...]  
verifier /volatile /removedriver <name> [<name>...]  
verifier /volatile /faults [<probability> [<tags> [<applications>]]  
verifier /reset  
verifier /query  
verifier /log <LogFileName> [/interval <seconds>]  
```  
### <a name="parameters"></a>Parameters  
|参数|描述|  
|-------|--------|  
|\<flags>|必须是数字中的位的十进制或十六进制，组合：<br /><br />-   **值： 说明**<br />-   **第 0 位：** 特殊池检查<br />-   **第 1 位：** 强制 irql 检查<br />-   **第 2 位：** 低资源模拟<br />-   **第 3 位：** 池跟踪<br />-   **位 4:** I/O 验证<br />-   **位 5:** 发生死锁检测<br />-   **位 6:** 未使用<br />-   **7 位：** DMA 验证<br />-   **位 8:** 安全检查<br />-   **位 9:** 强制挂起 I/O 请求<br />-   **位 10:** IRP 日志记录<br />-   **位 11:** 杂项检查<br /><br />例如， **/flags 27**等效与 **/flags 0x1B**|  
|/ 易失性|用于动态更改的验证程序设置，无需重新启动系统。 重新启动系统时，任何新设置将会丢失。|  
|\<概率 >|介于 1 和 10000 指定错误注入概率之间的数字。 例如，指定 100 表示 1%（100/10,000 个） 的故障注入的概率。<br /><br />如果未指定此参数，则将使用默认的 6%的概率。|  
|\<tags>|指定将注入的池标记包含错误，由空格分隔。 如果未指定此参数，则可以使用错误注入任何池分配。|  
|\<applications>|指定将注入的应用程序的图像文件名称包含错误，由空格分隔。 如果未指定此参数，资源不足模拟可以执行的任何应用程序中的位置。|  
|\<minutes>|指定的长度超过这个有效期之后重新启动，以分钟为单位的正数期间的任何故障注入将出现。 如果未指定此参数，则将使用 8 分钟的默认长度。|  
|/?|在命令提示符下显示帮助。|  

## <a name="additional-references"></a>其他参考  
-   [命令行语法解答](command-line-syntax-key.md)  