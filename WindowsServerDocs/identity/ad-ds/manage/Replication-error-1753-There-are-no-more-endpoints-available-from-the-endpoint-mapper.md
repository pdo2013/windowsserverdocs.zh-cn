---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: 复制错误 1753：端点映射程序中没有更多可用的端点
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9c8efee98cc8128443d9c835ccc5cb6b7695a094
ms.sourcegitcommit: a9625758fbfb066494fe62e0da5f9570ccb738a3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/12/2019
ms.locfileid: "68952472"
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>复制错误 1753：端点映射程序中没有更多可用的端点

>适用于：Windows Server

本文介绍了由于 Win32 错误 1753 Active Directory 操作失败的症状、原因和解决步骤:"终结点映射器中没有更多可用的终结点。"

DCDIAG 报告连接测试、Active Directory 复制测试或 KnowsOfRoleHolders 测试失败, 出现错误 1753:"终结点映射器中没有更多可用的终结点。"

```
Testing server: <site><DC Name>
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[<DC Name>] DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..
Printing RPC Extended Error Info:
Error Record 1, ProcessID is <process ID> (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: <source DC object GUID>._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag)
System Time is: <date> <time>
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper.
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,<DC Name>] A recent replication attempt failed:
From <source DC> to <destination DC>
Naming Context: <DN path of directory partition>
The replication generated an error (1753):
There are no more endpoints available from the endpoint mapper.
The failure occurred at <date> <time>.
The last success occurred at <date> <time>.
3 failures have occurred since the last success.
The directory on <DC name> is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
```

REPADMIN.EXE 报告复制尝试失败, 状态为1753。
通常引用1753状态的 REPADMIN 命令包括但不限于:

* REPADMIN/REPLSUM
* REPADMIN/SHOWREPL
* REPADMIN/SHOWREPS
* REPADMIN/SYNCALL

"REPADMIN/SHOWREPS" 中的示例输出描述从 CONTOSO 到 CONTOSO-DC1 的入站复制失败, 并显示 "复制访问被拒绝" 错误, 如下所示:

```
Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ <date> <time> failed, result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.
<#> consecutive failure(s).
Last success @ <date> <time>.
```

Active Directory 站点和服务中的 "**检查复制拓扑**" 命令返回 "终结点映射程序中没有更多可用的终结点"。

右键单击源 DC 中的连接对象, 然后选择 "**检查复制拓扑**失败", 并显示 "终结点映射程序中没有更多可用的终结点"。 屏幕错误消息如下所示:

对话框标题文本:"检查复制拓扑" 对话框消息文本:尝试联系域控制器时出现以下错误:端点映射程序中未提供更多端点。

Active Directory 站点和服务中的 "**立即复制**" 命令返回 "终结点映射器中没有更多可用的终结点"。
右键单击源 DC 中的连接对象并选择 "复制"**现在**会失败, 并出现 "终结点映射程序中没有更多可用的终结点"。
屏幕错误消息如下所示:

对话框标题文本:立即复制对话框消息文本:尝试将命名上下文\<% directory 分区名称% > 从域控制器\<源 dc > 同步到域控制器\<目标 dc > 时出现以下错误:

端点映射程序中未提供更多端点。
操作不会继续

在事件查看器的目录服务日志中记录 NTDS KCC、NTDS General 或2146893022状态的 ActiveDirectory_DomainService 事件。

Active Directory 通常引用-2146893022 状态的事件包括但不限于:

| 事件 ID | 事件来源 | 事件字符串|
| --- | --- | --- |
| 1655 | NTDS 常规 | Active Directory 尝试与以下全局编录通信, 尝试未成功。 |
| 1925 | NTDS KCC | 尝试为以下可写目录分区建立复制链接失败。 |
| 1265 | NTDS KCC | 知识一致性检查器 (KCC) 尝试添加下列目录分区和源域控制器的复制协议失败。 |

## <a name="cause"></a>原因

下面的步骤显示 RPC 工作流, 该工作流从在步骤1中的 RPC 终结点映射器 (EPM) 注册服务器应用程序开始, 到步骤7中将数据从 RPC 客户端传递到客户端应用程序。

### <a name="adds-rpc-workflow"></a>添加 RPC 工作流

1. 服务器应用程序将其终结点注册到 RPC 终结点映射器 (EPM)
1. 客户端发出 RPC 调用 (代表用户、操作系统或应用程序启动的操作)
1. 客户端 RPC 联系目标计算机 EPM 并请求终结点完成客户端调用
1. 服务器计算机的 EPM 使用终结点作出响应
1. 客户端 RPC 联系服务器应用
1. 服务器应用执行调用, 并将结果返回到客户端 RPC
1. 客户端 RPC 将结果传递回客户端应用程序

失败1753由步骤 #3 和 #4 之间的故障生成。 具体而言, 错误1753表示 RPC 客户端 (目标 DC) 能够通过端口135与 RPC 服务器 (源 DC) 联系, 但 RPC 服务器 (源 DC) 上的 EPM 找不到感兴趣的 RPC 应用程序, 并且返回的服务器端错误1753。 出现1753错误表示 RPC 客户端 (目标 DC) 通过网络从 RPC 服务器 (AD 复制源 DC) 收到服务器端错误响应。

1753错误的特定根本原因包括:

* 服务器应用从未开始 (例如, 在上面的 "详细信息" 关系图中的步骤 #1 从未尝试过)。
* 服务器应用程序已启动, 但在初始化期间发生了一些故障, 阻止其向 RPC 终结点映射器注册 (即, 尝试了上述 "详细信息" 关系图中的步骤 #1 但失败)。
* 服务器应用程序已启动, 但随后终止。 (即上面的 "详细信息" 关系图中的步骤 #1 已成功完成, 但稍后已撤消, 因为服务器终止)。
* 服务器应用手动取消注册其终结点 (类似于3但有意)。 出于完整性考虑, 不太可能包含在内。)
* 由于 DNS、WINS 或主机/Lmhosts 文件中的名称到 IP 映射错误, RPC 客户端 (目标 DC) 联系了不同的 RPC 服务器。

错误1753不是由以下原因引起:

* RPC 客户端 (目标 DC) 与 RPC 服务器 (源 DC) 之间缺少通过端口135的网络连接
* 通过暂时端口使用端口135和 RPC 客户端 (目标 DC) 在 RPC 服务器 (源 DC) 之间缺少网络连接。
* 密码不匹配或源 DC 无法解密 Kerberos 加密的数据包

## <a name="resolutions"></a>解决方法

验证在终结点映射器中注册其服务的服务是否已启动

* 对于 Windows 2000 和 Windows Server 2003 Dc:确保将源 DC 启动到正常模式。
* 对于 Windows Server 2008 或 Windows Server 2008 R2:在源 DC 的控制台中, 启动服务管理器 (services.msc) 并验证 Active Directory 域服务服务是否正在运行。

验证连接到目标 RPC 服务器 (源 DC) 的 RPC 客户端 (目标 DC)

公用 Active Directory 林中的所有 Dc 都注册 _msdcs 中的域控制器 CNAME 记录。 \<目录林根级域 > DNS 区域, 而不考虑它们在林中驻留的域。 DC CNAME 记录派生自每个域控制器的 "NTDS 设置" 对象的 objectGUID 属性。

执行基于复制的操作时, 目标 DC 会向 DNS 查询源 Dc CNAME 记录。 CNAME 记录包含源 DC 完全限定的计算机名称, 此名称用于通过 DNS 客户端缓存查找、主机/LMHost 文件查找、在 DNS 中托管 A/AAAA 记录或 WINS 来派生源 Dc IP 地址。

在 DNS、WINS、主机和 LMHOST 文件中, 陈旧的 NTDS 设置对象和错误的名称到 IP 的映射可能会导致 RPC 客户端 (目标 DC) 连接到错误的 RPC 服务器 (源 DC)。 而且, 错误的名称到 IP 映射可能会导致 RPC 客户端 (目标 DC) 连接到甚至没有相关 RPC 服务器应用程序的计算机 (在本例中为 Active Directory 角色)。 (示例: DC2 的陈旧主机记录包含 DC3 的 IP 地址或成员计算机)。

验证 Active Directory 目标 Dc 副本中存在的源 DC 的 objectGUID 与 Active Directory 的源 dc 副本中存储的源 DC objectGUID 匹配。 如果存在差异, 请使用 "ntds 设置" 对象上的 repadmin/showobjmeta, 以查看哪一个对应于源 DC 的上次升级 (提示: 针对 NTDS 设置对象的日期戳进行比较。源 Dc dcpromo 日志文件。 可能需要使用 DCPROMO 的最后修改/创建日期。日志文件本身)。 如果对象 Guid 不完全相同, 则目标 DC 可能会为源 DC 提供陈旧的 NTDS 设置对象, 其 CNAME 记录将引用具有错误名称的主机记录到 IP 映射。

在目标 DC 上, 运行 IPCONFIG/ALL 以确定目标 DC 用于名称解析的 DNS 服务器:

```
c:>ipconfig /all
```

在目标 DC 上, 对源 Dc 完全限定的 DC CNAME 记录运行 NSLOOKUP:

```
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs primary DNS Server IP >
c:>nslookup -type=cname <fully qualified cname of source DC> <destination DCs secondary DNS Server IP>
```

验证 NSLOOKUP "拥有" 源 DC 的主机名称/安全标识返回的 IP 地址:

```
C:>NBTSTAT -A <IP address returned by NSLOOKUP in the step above>
```

登录到源 DC 的控制台, 从 CMD 提示符运行 "IPCONFIG", 并验证源 DC 是否拥有上述 NSLOOKUP 命令返回的 IP 地址

在 DNS 中检查 IP 映射的过时/重复主机

```
NSLOOKUP -type=hostname <single label hostname of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <single label hostname of source DC> <secondary DNS Server IP on destination DC>

NSLOOKUP -type=hostname <fully qualified computer name of source DC> <primary DNS Server IP on destination DC>
NSLOOKUP -type=hostname <fully qualified computer name of source DC> <secondary DNS Server IP on dest. DC>
```

如果主机记录中存在无效的 IP 地址, 请调查是否已启用并正确配置了 DNS 清理。

如果上面的测试或网络跟踪未显示名称查询返回无效的 IP 地址, 请考虑主机文件、LMHOSTS 文件和 WINS 服务器中的过时条目。 请注意, 还可以将 DNS 服务器配置为执行 WINS 回退名称解析。

* 验证服务器应用程序 (Active Directory et al) 是否已向 RPC 服务器上的终结点映射器注册 (源 DC)
* Active Directory 混合使用众所周知的动态注册端口。 此表列出了 Active Directory 域控制器使用的知名端口和协议。

| RPC 服务器应用程序 | Port | TCP | UDP |
| --- | --- | --- | --- |
| DNS 服务器 | 53 | X | X |
| Kerberos | 88 | X | X |
| LDAP 服务器 | 389 | X | X |
| Microsoft-DS | 445 | X | X |
| LDAP SSL | 636 | X | X |
| 全局编录服务器 | 3268 | X |   |
| 全局编录服务器 | 3269 | X |   |

已知端口未注册到终结点映射器。

Active Directory 和其他应用程序还会注册在 RPC 临时端口范围内接收动态分配的端口的服务。 此类 RPC 服务器应用程序是动态分配的 TCP 端口, 在 windows 2000 和 windows server 2003 计算机上的 TCP 5000 1024 端口和 windows server 49152 和 Windows Server 65535 R2 计算机上的2008和2008范围内的端口之间动态分配。 使用[知识库文章 224196](https://support.microsoft.com/kb/224196)中所述的步骤, 可以在注册表中对复制使用的 RPC 端口进行硬编码。 当配置为使用硬编码端口时, Active Directory 继续向 EPM 注册。

验证感兴趣的 RPC 服务器应用程序是否已向 RPC 服务器上的 RPC 终结点映射器注册自身 (对于 AD 复制, 则为源 DC)。

完成此任务的方法有很多, 其中一种方法是使用以下语法在源 DC 的控制台上使用管理员特权的 CMD 提示符安装和运行 PORTQRY:

```
portquery -n <source DC> -e 135 > file.txt
```

在 portqry 输出中, 请注意 ncacn_ip_tcp 协议的 "MS NT Directory DRS Interface" (UUID = 351 ...) 动态注册的端口号。 以下代码片段显示了来自 Windows Server 2008 R2 DC 的示例 portquery 输出:

```
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]
```

其他可能的解决此错误的方法:

* 验证是否在正常模式下启动了源 DC, 并且源 DC 上的操作系统和 DC 角色已完全启动。
* 验证 Active Directory 域服务是否正在运行。 如果服务当前已停止或未配置为具有默认启动值, 请重置默认启动值, 重新启动修改后的 DC, 然后重试该操作。
* 验证 rpc 服务和 rpc 定位器的启动值和服务状态对于 RPC 客户端 (目标 DC) 和 RPC 服务器 (源 DC) 的 OS 版本是否正确。 如果服务当前已停止或未配置为具有默认启动值, 请重置默认启动值, 重新启动修改后的 DC, 然后重试该操作。
   * 此外, 请确保服务上下文与下表中列出的默认设置匹配。

      | 服务 | Windows Server 2003 和更高版本中的默认状态 (启动类型) | Windows Server 2000 中的默认状态 (启动类型) |
      | --- | --- | --- |
      | 远程过程调用 | 已启动 (自动) | 已启动 (自动) |
      | 远程过程调用定位符 | Null 或已停止 (手动) | 已启动 (自动) |

* 验证动态端口范围的大小是否尚未受到约束。 用于枚举 RPC 端口范围的 Windows Server 2008 和 Windows Server 2008 R2 NETSH 语法如下所示:

   ```
   netsh int ipv4 show dynamicport tcp
   netsh int ipv4 show dynamicport udp
   netsh int ipv6 show dynamicport tcp
   netsh int ipv6 show dynamicport udp
   ```

* 验证在 KB 224196 中定义的硬编码端口定义是否在源 Dc 操作系统版本的动态端口范围内。 查看[知识库文章 224196](https://support.microsoft.com/kb/224196) , 并确保硬编码端口处于源 DC 的操作系统版本的临时端口范围内。

* 验证协议项是否存在于 HKLM\Software\Microsoft\Rpc 下, 并包含以下5个默认值:

   ```
   ncacn_http REG_SZ rpcrt4.dll
   ncacn_ip_tcp REG_SZ rpcrt4.dll
   ncacn_nb_tcp REG_SZ rpcrt4.dll
   ncacn_np REG_SZ rpcrt4.dll
   ncacn_ip_udp REG_SZ rpcrt4.dll
   ```

## <a name="more-information"></a>详细信息

导致 RPC 错误1753与-2146893022 的 IP 映射的错误名称示例: 目标主体名称不正确

Contoso.com 域由 DC1 和 DC2 组成, 其中包含 IP 地址: x. x. x. x. x. x. x. x. x. x。 已在为 DC1 配置的所有 DNS 服务器上正确注册了 DC2 的主机 "A"/"AAAA" 记录。 此外, DC1 上的 HOSTS 文件包含一个条目映射, DC2s 完全限定的主机名到 IP 地址 x. x. x. x. x。 稍后, DC2's IP 地址将从 X. x. x. X. X. x. x. x. x. x. x. x. x. x. x. x. x. x. x. x. x。 Active Directory 站点和服务 "管理单元中的"**立即复制**"命令触发的 AD 复制尝试失败, 出现错误 1753, 如下跟踪所示:

```
F# SRC    DEST    Operation
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP)
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0
10 x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
11 x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
```

在第**10**帧, 目标 dc 通过端口135查询源 dc 终结点映射器 Active Directory 复制服务类 UUID E351 。

在帧**11**中, 源 DC (在本例中为尚未托管 DC 角色的成员计算机) 未注册 E351 。复制服务与其本地 EPM 响应的 UUID 使用符号错误 EP_S_NOT_REGISTERED 进行响应, 此错误映射到十进制错误 1753, 十六进制错误 0x6d9, 友好错误 "终结点映射器中没有更多可用的终结点"。

稍后, IP 地址为 MayberryDC 的成员计算机在 contoso.com 域中升级为副本 ""。 同样,**立即复制**命令用于触发复制, 但这次操作失败, 出现屏幕错误 "目标主体名称不正确"。 向其网络适配器分配了 IP 地址的计算机: x. x. x. x. x. x. x. x. x. x. x. x. x。复制服务 UUID 与其本地 EPM 一起提供, 但它不拥有 DC2 的名称或安全标识, 因此无法解密 DC1 发出的 Kerberos 请求, 因此请求现在失败并出现错误 "目标主体名称不正确"。 错误映射为十进制错误-2146893022/十六进制错误0x80090322。

此类无效的主机到 IP 映射可能是由于主机/lmhost 文件中的过时条目、在 DNS 中托管 A/AAAA 注册或 WINS 导致的。

小结此示例失败, 因为在此情况下, 主机文件中的主机到 IP 映射无效, 导致目标 DC 解析为未运行 Active Directory 域服务服务 (甚至出于此目的而安装) 的 "源" DCSPN 尚未注册, 并且源 DC 返回了错误1753。 在第二种情况下, 无效的主机到 IP 映射 (在主机文件中再次) 导致目标 DC 连接到已注册了 E351 的 DC 。复制 SPN, 但该源的主机名和安全标识与预期的源 DC 不同, 因此尝试失败并出现错误-2146893022:目标主体名称不正确。

## <a name="related-topics"></a>相关主题

* [排查 Active Directory 操作失败并出现错误 1753:终结点映射器中没有更多可用的终结点。](https://support.microsoft.com/kb/2089874)
* [知识库文章839880使用产品 CD 中的 Windows Server 2003 支持工具排查 RPC 终结点映射程序错误](https://support.microsoft.com/kb/839880)
* [知识库文章 832017 Windows Server 系统的服务概述和网络端口要求](https://support.microsoft.com/kb/832017/)
* [知识库文章224196将 Active Directory 复制流量和客户端 RPC 流量限制到特定端口](https://support.microsoft.com/kb/224196/)
* [知识库文章154596如何配置与防火墙一起使用的 RPC 动态端口分配](https://support.microsoft.com/kb/154596)
* [RPC 的工作方式](https://msdn.microsoft.com/library/aa373935(VS.85).aspx)
* [服务器如何为连接做好准备](https://msdn.microsoft.com/library/aa373938(VS.85).aspx)
* [客户端如何建立连接](https://msdn.microsoft.com/library/aa373937(VS.85).aspx)
* [注册接口](https://msdn.microsoft.com/library/aa375357(VS.85).aspx)
* [使服务器在网络上可用](https://msdn.microsoft.com/library/aa373974(VS.85).aspx)
* [注册终结点](https://msdn.microsoft.com/library/aa375255(VS.85).aspx)
* [侦听客户端调用](https://msdn.microsoft.com/library/aa373966(VS.85).aspx)
* [客户端如何建立连接](https://msdn.microsoft.com/library/aa373937(VS.85).aspx)
* [限制到特定端口的 Active Directory 复制流量和客户端 RPC 流量](https://support.microsoft.com/kb/224196)
* [AD DS 中的目标 DC 的 SPN](https://msdn.microsoft.com/library/dd207688(PROT.13).aspx)
