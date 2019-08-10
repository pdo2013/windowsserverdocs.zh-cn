---
title: 域名系统 (DNS) 问题疑难解答
description: 本文介绍如何在出现 DNS 问题时收集数据。
manager: willchen
ms.prod: ''
ms.technology: networking-dns
ms.topic: article
ms.author: delhan
ms.date: 8/8/2019
author: Deland-Han
ms.openlocfilehash: a86b1f34c3b21f5bcde710e2a98323492ea51b62
ms.sourcegitcommit: 0e3c2473a54f915d35687d30d1b4b1ac2bae4068
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/09/2019
ms.locfileid: "68917774"
---
# <a name="troubleshooting-domain-name-system-dns-issues"></a>域名系统 (DNS) 问题疑难解答
 
域名解析问题可分解为客户端和服务器端问题。 通常, 你应该首先进行客户端故障排除, 除非你在范围阶段确定问题在服务器端确实会出现。

- [DNS 客户端疑难解答](troubleshoot-dns-client.md)

- [DNS 服务器疑难解答](troubleshoot-dns-server.md)
 
## <a name="data-collection"></a>数据收集
 
建议在出现问题时同时在客户端和服务器端收集数据。 但是, 根据实际问题, 你可以在 DNS 客户端或 DNS 服务器上的单个数据集中启动集合。
 
若要从受影响的客户端和配置的 DNS 服务器收集 Windows 网络诊断, 请执行以下步骤:

1. 在客户端和服务器上启动网络捕获:

   ```cmd
   netsh trace start capture=yes tracefile=c:\%computername%_nettrace.etl
   ```

2. 通过运行以下命令, 在 DNS 客户端上清除 DNS 缓存:

   ```cmd
   ipconfig /flushdns
   ```

3. 重现问题。

4. 停止并保存跟踪:

   ```cmd
   netsh trace stop
   ```

5. 保存每台计算机上的 Nettrace 文件。 当你联系 Microsoft 支持部门时, 此信息将非常有用。