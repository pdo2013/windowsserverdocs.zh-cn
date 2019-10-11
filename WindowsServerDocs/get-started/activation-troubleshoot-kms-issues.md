---
title: KMS 激活已知问题
description: 描述在 KMS 激活过程中可能发生的常见问题，并提供解决方法和指南
ms.topic: article
ms.date: 10/3/2019
ms.technology: server-general
ms.assetid: ''
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: dab8294837a5f9116328e59364de9beb139a4b77
ms.sourcegitcommit: 9855d6b59b1f8722f39ae74ad373ce1530da0ccf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/04/2019
ms.locfileid: "71963003"
---
# <a name="kms-activation-known-issues"></a>KMS 激活：已知问题

本文描述在密钥管理服务 (KMS) 激活期间可能产生的常见问题，并提供解决问题的指南。

> [!NOTE]
> 如果你怀疑问题与 DNS 相关，请参阅 [KMS 和 DNS 问题的常见排查过程](common-troubleshooting-procedures-kms-dns.md)。

## <a name="should-i-back-up-kms-host-information"></a>我是否应备份 KMS 主机信息？

KMS 主机无需备份。 但是，如果使用工具定期清理事件日志，则存储在日志中的激活历史记录可能丢失。 如果使用事件日志跟踪或记录 KMS 激活，请定期导出事件查看器的“应用程序和服务日志”文件夹中的密钥管理服务事件日志。

如果使用 System Center Operations Manager，System Center数据仓库数据库会存储用于报告的事件日志数据，因此无需单独备份事件日志。

## <a name="is-the-kms-client-computer-activated"></a>KMS 客户端计算机是否已激活？

在 KMS 客户端计算机上，打开“系统”控制面板并查找“Windows 已激活”消息   。 或者，运行 Slmgr.vbs 并使用 /dli 命令行选项  。

## <a name="the-kms-client-computer-does-not-activate"></a>KMS 客户端计算机未激活

验证是否已达到 KMS 激活阈值。 在 KMS 主机上，运行 Slmgr.vbs 并使用 /dli 命令行选项来确定主机的当前计数  。 在 KMS 主机的计数达到 25 之前，Windows 7 客户端计算机都无法激活。 Windows Server 2008 R2 KMS 客户端需要在 KMS 计数为 5 时才能激活。 有关 KMS 要求的详细信息，请参阅[批量激活计划指南](http://go.microsoft.com/fwlink/?linkid=155926)。 

在 KMS 客户端计算机上，在应用程序事件日志中查找事件 ID 12289。 查看此事件以了解以下信息：

- 结果代码是否为 0  ？ 其他任何值均为错误。
- 事件中的 KMS 主机名是否正确？
- KMS 端口是否正确？
- 是否可访问 KMS 主机？
- 如果客户端正在运行非 Microsoft 防火墙，是否需要配置出站端口？

在 KMS 主机上，在 KMS 事件日志中查找事件 ID 12290。 查看此事件以了解以下信息：

- KMS 主机是否记录了来自客户端计算机的请求？ 验证是否列出了 KMS 客户端计算机名称。 验证客户端和 KMS 主机是否能够通信。 客户端是否收到响应？
- 如果 KMS 客户端未记录任何事件，则请求不会到达 KMS 主机，或者 KMS 主机无法处理该请求。 请确保路由器不会阻止使用 TCP 端口1688（如果使用默认端口）的流量，并允许流向 KMS 客户端的有状态的流量。

## <a name="what-does-this-error-code-mean"></a>此错误代码的含义是什么？

除了包含事件 ID 12290 的 KMS 事件，Windows 将所有激活事件记录到名为 Microsoft-Windows-Security-SPP 的事件提供程序下的应用程序事件日志。 Windows 将 KMS 事件记录到“应用程序和服务”文件夹中的密钥管理服务日志中。 IT 专业人员可以运行 Slui.exe 来显示与大部分激活相关的错误代码说明。 此命令的常规语法如下所示：

```cmd
slui.exe 0x2a ErrorCode
```

例如，如果事件 ID 12293 包含错误代码0x8007267C，可以通过运行以下命令来显示该错误的描述：

```cmd
slui.exe 0x2a 0x8007267C
```

有关具体错误代码以及如何解决它们的详细信息，请参阅[解决常见激活错误代码](activation-error-codes.md)。

## <a name="clients-are-not-adding-to-the-kms-count"></a>客户端未添加到 KMS 计数

若要重置客户端计算机 ID (CMID) 和其他产品激活信息，请运行 sysprep /generalize 或 slmgr /rearm   。 否则，每台客户端计算机看起来相同，KMS 主机不会将它们作为单独的 KMS 客户端进行计数。

## <a name="kms-hosts-are-unable-to-create-srv-records"></a>KMS 主机无法创建 SRV 记录

域名系统 (DNS) 可能会限制写权限，或者可能不支持动态 DNS (DDNS)。 在这种情况下，请向 KMS 主机授予对 DNS 数据库的写权限，或者手动创建服务 (SRV) 资源记录 (RR)。 有关 KMS 和 DNS 问题的详细信息，请参阅 [KMS 和 DNS 问题的常见排查过程](common-troubleshooting-procedures-kms-dns.md)。

## <a name="only-the-first-kms-host-is-able-to-create-srv-records"></a>只有第一台 KMS 主机能够创建 SRV 记录

如果组织拥有多台 KMS 主机，其他主机可能无法更新 SRV RR，除非更改 SRV 默认权限。 有关 KMS 和 DNS 问题的详细信息，请参阅 [KMS 和 DNS 问题的常见排查过程](common-troubleshooting-procedures-kms-dns.md)。

## <a name="i-installed-a-kms-key-on-the-kms-client"></a>我在 KMS 客户端上安装了 KMS 密钥

KMS 密钥应仅安装在 KMS 主机上，而不是 KMS 客户端上。 运行 slmgr.vbs -ipk &lt;SetupKey&gt;  。 有关可用于将计算机配置为 KMS 客户端的密钥表，请参阅 [KMS 客户端安装密钥](KMSclientkeys.md)。 这些密钥是公开的，并且是版本特定的。 请记住从 DNS 中删除任何不必要的 SRV RR，再重启计算机。

## <a name="a-kms-host-failed"></a>KMS 主机出现故障

如果 KMS 主机出现故障，则必须在新主机上安装 KMS 主机密钥，然后再激活该主机。 请确保新的 KMS 主机在 DNS 数据库中具有 SRV RR。 如果使用与出现故障的 KMS 主机相同的计算机名和 IP 地址来安装新的 KMS 主机，则新的 KMS 主机可以使用故障主机的 DNS SRV 记录。 如果新主机具有其他计算机名，则可以手动删除故障主机的 DNS SRV RR，或让 DNS 自动删除它（如果在 DNS 中启用了清理）。 如果网络使用的是 DDNS，则新的 KMS 主机会自动在 DNS 服务器上创建新的 SRV RR。 然后，新的 KMS 主机会开始收集客户端续订请求，并在达到 KMS 激活阈值时立即开始激活客户端。

如果 KMS 客户端使用自动发现，则当原始 KMS 主机不响应续订请求时，它们会自动选择另一台 KMS 主机。 如果客户端不使用自动发现，则必须通过运行 slmgr.vbs /skms 手动更新已分配给故障 KMS 主机的 KMS 客户端计算机  。 若要避免这种情况，请将 KMS 客户端配置为使用自动发现。 有关详细信息，请参阅[批量激活部署指南](http://go.microsoft.com/fwlink/?linkid=150083)。
