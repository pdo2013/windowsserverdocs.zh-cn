---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 4e8e3234b89630bf16148eef644f0c6607ad38bd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852338"
---
## <a name="importance-of-time-protocols"></a>时间协议的重要性
时间协议来交换时间信息，然后使用该信息以使其时钟同步的两台计算机之间进行通信。 使用 Windows 时间服务时间协议，客户端从服务器请求时间的信息，并使其根据收到的信息的时钟同步。
  
Windows 时间服务使用 NTP 来帮助跨网络同步时间。 NTP 是一个 Internet 时间协议，包括同步时钟所需的专业算法。 NTP 是更准确的时间协议比简单网络时间协议 (SNTP)，可在某些版本的 Windows;但是，W32Time 继续支持 SNTP 若要启用与运行 SNTP 基于时服务，例如 Windows 2000 的计算机的向后兼容性。

有许多不同原因，可能需要准确的时间。  Windows 的典型情况是准确性的 Kerberos，需要 5 分钟的客户端和服务器之间。  但是，有可能受到时间准确性包括的许多其他方面：


- 政府法规所示：
    - 在美国的 FINRA 的 50 毫秒准确性
    - 1 ms ESMA (MiFID II) 在欧盟。
- 加密算法
- 群集/SQL/Exchange 和文档数据库这类分布式的系统
- 用于事务比特币区块链框架
- 分布式的日志和威胁分析 
- AD 复制
- PCI （支付卡行业） 当前 1 的第二个准确性