---
title: HTTP 1.1/2 的性能优化
description: 性能优化建议的 HTTP 1.1/2
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: IvanPash; GMonte
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bf85efa88e377966135c23a548119f19c39cceba
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866368"
---
# <a name="performance-tuning-http-112"></a>性能优化 HTTP 1.1/2

HTTP/2 旨在提高客户端 （例如，页面加载时间在浏览器） 上的性能。 在服务器上，它可能表示略微增大 CPU 开销。 服务器不能再为每个请求需要单个 TCP 连接，而该状态的一些现在会保持 HTTP 层中。 此外，HTTP/2 具有标头压缩，它表示其他 CPU 负载。

某些情况下需要 HTTP/1.1 回退 （HTTP/2 连接重置和而建立一个新连接以使用 HTTP/1.1）。 特别是，TLS 重新协商和 HTTP 身份验证 （而不是基本和摘要式） 需要 HTTP/1.1 回退。 即使此增加的开销，这些操作已暗示一定的延迟，这样就不特别高性能。

## <a name="see-also"></a>请参阅
- [Web 服务器性能优化](index.md) 
- [IIS 10.0 性能优化](tuning-iis-10.md)