---
title: 使用受保护的构造诊断工具进行故障排除
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 07691d5b-046c-45ea-8570-a0a85c3f2d22
manager: dongill
author: huu
ms.technology: security-guarded-fabric
ms.openlocfilehash: 0fb257f693cc27c0bc6dd18fc89e8dc6328ee638
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447338"
---
# <a name="troubleshooting-using-the-guarded-fabric-diagnostic-tool"></a>使用受保护的构造诊断工具进行故障排除

>适用于：Windows Server 2019，Windows Server （半年频道），Windows Server 2016

本主题介绍如何使用受保护的构造诊断工具以识别并解决部署、 配置和正在进行的操作的受保护的构造基础结构中的常见失败。 这包括主机保护者服务 (HGS)、 所有受保护的主机和支持服务，如 DNS 和 Active Directory。 诊断工具可以用于执行第一个阶段，在会审失败受保护的构造，管理员提供用于解决服务中断和确定错误配置的资产的起始点。 该工具不替代声音掌握现有的操作系统受保护的构造，并仅可用于快速验证在日常操作过程中遇到的最常见的问题。

本主题中使用的 cmdlet 的文档可能位于[TechNet](https://technet.microsoft.com/library/mt718834.aspx)。

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

# <a name="quick-start"></a>快速启动

可以通过从具有本地管理员权限的 Windows PowerShell 会话中调用以下诊断是受保护的主机或 HGS 节点：
```PowerShell
Get-HgsTrace -RunDiagnostics -Detailed
```
这将自动检测当前主机的角色并诊断可以自动检测到的任何相关问题。  由于存在将显示所有在此过程中生成的结果的`-Detailed`切换。

此主题的其余部分将提供的高级用法的详细的演练`Get-HgsTrace`适用于在一次诊断的多个主机和检测复杂的跨节点配置不正确。

## <a name="diagnostics-overview"></a>诊断概述
受保护的构造诊断是与受防护的虚拟机的任何主机上提供相关工具和功能安装，包括运行服务器核心的主机。  目前，诊断是包含以下功能包：

1. 主机保护者服务角色
2. 主机保护者 Hyper-V 支持
3. 用于结构管理的 VM 防护工具
4. 远程服务器管理工具 (RSAT)

这意味着诊断工具可在所有受保护的主机、 HGS 节点、 某些构造管理服务器和使用任何 Windows 10 的工作站[RSAT](https://www.microsoft.com/download/details.aspx?id=45520)安装。  诊断可以从任何具有诊断任何受保护的主机或在受保护的构造; HGS 节点的意图的更高版本计算机调用使用远程跟踪目标，诊断可以找到并连接到运行诊断程序的计算机以外的主机。

针对诊断的每个主机被称为"跟踪目标"。  跟踪目标标识由其主机名和角色。  角色描述给定的跟踪目标执行中受保护的构造函数。  跟踪目前，支持的目标`HostGuardianService`和`GuardedHost`角色。  请注意可以使主机可以一次性占用多个角色和诊断，也支持此功能，但不应在生产环境中完成这。  非重复在任何时候和 HGS 和 HYPER-V 主机应保持隔离。

管理员可以通过运行开始任何诊断任务`Get-HgsTrace`。  此命令执行两个不同的功能基础提供在运行时的开关： 跟踪集合和诊断。  结合使用这两个构成了受保护的构造诊断工具的完整。  尽管未明确要求，但最有用的诊断要求才可收集使用管理员凭据跟踪目标的跟踪。  如果执行跟踪集合的用户所具有足够的特权，而所有其他人将传递将失败需提升权限的跟踪。  这允许部分的诊断事件未得到充分特权运算符执行会审中。 

### <a name="trace-collection"></a>跟踪集合
默认情况下，`Get-HgsTrace`将仅收集跟踪，并将其保存到临时文件夹。  跟踪采用后填充使用特殊格式描述了如何配置主机的文件的目标主机名为的文件夹。  跟踪还包含描述如何调用诊断以收集跟踪的元数据。  诊断使用此数据执行手动诊断时，解除冻结有关主机的信息。

如有必要，可以手动查看跟踪。  所有格式都是用户可读 (XML) 或可能使用标准工具轻松地检查 (例如 X509 证书和 Windows 加密外壳扩展)。  但是请注意，跟踪不用于手动诊断，因此始终会有效地使用的诊断功能处理跟踪`Get-HgsTrace`。

运行跟踪集合的结果不使任何指明运行状况的给定主机。  它们只需指示已成功收集跟踪。  需要使用的诊断功能`Get-HgsTrace`确定跟踪是否表示失败环境。

使用`-Diagnostic`参数，可以限制为仅这些操作指定的诊断所需的跟踪的跟踪集合。  这将减少调用诊断所需的权限以及收集的数据量。

### <a name="diagnosis"></a>诊断
可以通过诊断收集的跟踪，提供`Get-HgsTrace`通过跟踪的位置`-Path`参数并指定`-RunDiagnostics`切换。  此外，`Get-HgsTrace`可以收集和诊断中执行一次传递通过提供`-RunDiagnostics`开关和跟踪目标的列表。  如果提供了没有跟踪目标，在当前计算机被用作通过检查已安装的 Windows PowerShell 模块来推断出其角色的隐式的目标。

诊断将提供结果，该层次结构格式显示的跟踪目标、 诊断设置，而单个诊断负责特定的错误。  如果可以最新的操作应采取下一步做决定，故障包括修正和解决方法建议。  默认情况下，通过和不相关的结果处于隐藏状态。  若要查看诊断测试的所有内容，请指定`-Detailed`切换。  这将导致所有结果，而不考虑它们的状态显示。

可以限制的一套使用运行的诊断`-Diagnostic`参数。  这允许您指定的诊断的类应运行对跟踪目标，并禁止显示所有其他用户。  可用诊断类的示例包括网络、 最佳实践和客户端硬件。  请查阅[cmdlet 文档](https://technet.microsoft.com/library/mt718831.aspx)若要查找可用诊断的最新列表。

> [!WARNING]
> 诊断不可替代的强监视和事件响应管道。  可用于监视受保护的构造，以及可以进行监视以及早检测到问题的各种事件日志通道是 System Center Operations Manager 包。  诊断可以用于快速会审这些故障和建立一系列操作。

## <a name="targeting-diagnostics"></a>目标的诊断

`Get-HgsTrace` 对跟踪目标进行操作。  跟踪目标是对应于 HGS 节点或受保护的主机在受保护的构造的对象。  它可以作为扩展看作`PSSession`包括仅通过诊断，如在构造中的主机的角色所需的信息。  目标可以是隐式生成 （例如本地或手动诊断） 或使用显式`New-HgsTraceTarget`命令。

### <a name="local-diagnosis"></a>本地诊断

默认情况下，`Get-HgsTrace`本地主机 （即，该 cmdlet 的调用） 为目标。  这称为隐式的本地目标。  中提供了不到目标时，才使用隐式的本地目标`-Target`中找到参数和任何预先存在的跟踪`-Path`。

隐式的本地目标使用角色推理来确定当前的主机在受保护的构造中扮演何种角色。  这基于已安装的 Windows PowerShell 模块，它们大致对应于在系统上安装了哪些功能。  是否存在`HgsServer`模块将导致要使角色的跟踪目标`HostGuardianService`是否存在`HgsClient`模块将导致要使角色的跟踪目标`GuardedHost`。  可以为给定主机有两个模块存在这种情况下，它将被视为两`HostGuardianService`和一个`GuardedHost`。

因此，为收集跟踪本地诊断的默认调用：
```PowerShell
Get-HgsTrace
```
...等效于以下：
```PowerShell
New-HgsTraceTarget -Local | Get-HgsTrace
```
> [!TIP]
> `Get-HgsTrace` 可以接受目标通过管道或直接通过`-Target`参数。  这二者之间没有差异操作状态。

### <a name="remote-diagnosis-using-trace-targets"></a>远程诊断使用跟踪目标

就可以通过生成具有远程连接信息的跟踪目标来远程诊断主机。  只需要是凭据的主机名和一组能够使用 Windows PowerShell 远程处理进行连接。
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $server
```
此示例将生成提示来收集远程用户凭据，，然后运行诊断程序将使用的远程主机`hgs-01.secure.contoso.com`完成跟踪集合。  生成的跟踪是下载到本地主机，然后进行诊断。  诊断结果执行时是否呈现相同[本地诊断](#local-diagnosis)。  同样，不需要指定角色，它可推断出基于远程系统上安装的 Windows PowerShell 模块。

远程诊断利用 Windows PowerShell 远程处理到远程主机的所有访问。  因此，它是跟踪目标已启用 Windows PowerShell 远程处理的先决条件 (请参阅[启用 PSRemoting](https://technet.microsoft.com/library/hh849694.aspx)) 和本地主机是正确配置为启动到目标的连接。

> [!NOTE]
> 在大多数情况下，才需要本地主机是相同的 Active Directory 林的一部分，并使用有效的 DNS 主机名。  如果你的环境使用更复杂的联合身份验证模型，或者你想要使用直接 IP 地址进行连接，您可能需要执行其他配置，例如设置 WinRM[受信任的主机](https://technet.microsoft.com/library/ff700227.aspx)。

你可以验证跟踪目标正确实例化并配置为接受使用连接`Test-HgsTraceTarget`cmdlet:
```PowerShell
$server = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential)
$server | Test-HgsTraceTarget
```
此命令将返回`$True`当且仅当`Get-HgsTrace`能够建立与跟踪目标的远程诊断会话。  如果失败，此 cmdlet 将返回用于进一步执行故障排除的 Windows PowerShell 远程处理连接的相关状态信息。

#### <a name="implicit-credentials"></a>隐式凭据

在执行远程诊断从具有足够的权限的用户能够远程连接到跟踪目标时，不需要提供凭据`New-HgsTraceTarget`。  `Get-HgsTrace` Cmdlet 将自动重复使用的打开连接时调用该 cmdlet 的用户凭据。

> [!WARNING]
> 某些限制适用于重复使用凭据，尤其是当执行所谓的"第二个跃点。"  尝试重复使用凭据从远程会话内的到另一台计算机时，将发生这种情况。  它是所需[设置 CredSSP](https://technet.microsoft.com/library/hh849872.aspx)为了支持此方案中，但这是范围之外的受保护的构造管理和故障排除。

#### <a name="using-windows-powershell-just-enough-administration-jea-and-diagnostics"></a>使用 Windows PowerShell Just Enough Administration (JEA) 和诊断

远程诊断支持使用 JEA 约束 Windows PowerShell 终结点。 默认情况下，远程跟踪目标将连接使用默认`microsoft.powershell`终结点。  如果跟踪目标已`HostGuardianService`角色，它也会尝试使用`microsoft.windows.hgs`HGS 安装时配置的终结点。

如果你想要使用自定义终结点，则必须构造跟踪目标使用时指定的会话配置名称`-PSSessionConfigurationName`参数，如下面：

```PowerShell
New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Role HostGuardianService -Credential (Enter-Credential) -PSSessionConfigurationName "microsoft.windows.hgs"
```

#### <a name="diagnosing-multiple-hosts"></a>诊断的多个主机

可以将传递到多个跟踪目标`Get-HgsTrace`在一次。  这包括本地和远程目标的组合。  又要跟踪的每个目标，然后将同时诊断从每个目标的跟踪。  诊断工具可以使用你的部署的增加的知识来标识不应可检测的复杂跨节点配置错误。  使用此功能只需提供目标的多个主机时调用的跟踪从多个主机同时 （对于手动诊断） 或由`Get-HgsTrace`（对于远程诊断）。

下面是使用远程诊断会审 fabric 组成两个 HGS 节点和两个受保护的主机，其中一个受保护的主机用于启动的示例`Get-HgsTrace`。

```PowerShell
$hgs01 = New-HgsTraceTarget -HostName "hgs-01.secure.contoso.com" -Credential (Enter-Credential)
$hgs02 = New-HgsTraceTarget -HostName "hgs-02.secure.contoso.com" -Credential (Enter-Credential)
$gh01 = New-HgsTraceTarget -Local
$gh02 = New-HgsTraceTarget -HostName "guardedhost-02.contoso.com"
Get-HgsTrace -Target $hgs01,$hgs02,$gh01,$gh02 -RunDiagnostics
```

> [!NOTE]
> 不需要诊断多个节点时诊断在整个受保护的构造。  在许多情况下就足以在给定的失败条件包括可能会涉及到的所有节点。  这通常是受保护的主机和一定数量的从 HGS 群集节点的子集。

## <a name="manual-diagnosis-using-saved-traces"></a>手动诊断使用已保存的跟踪

有时你可能想要重新运行诊断程序而无需收集跟踪，或您没有所需的凭据来远程诊断的所有结构中主机同时。  手动诊断是一种机制，通过这仍可以执行整个 fabric 会审使用`Get-HgsTrace`，但不支持使用远程跟踪集合。

在执行之前手动诊断，你需要确保将会审的结构中每个主机的管理员已做好准备，愿意代表你执行命令。  诊断跟踪输出不显示任何通常被视为敏感、 的信息，但负责在用户以确定它是否可以安全地公开给其他人此信息。

> [!NOTE]
> 跟踪未进行匿名处理并显示网络配置、 PKI 设置和其他配置，有时会被视为私人信息。  因此，跟踪只应传输到组织内受信任的实体，并永远不会公开发布。

执行手动诊断步骤如下所示：

1. 每个主机管理员运行的请求`Get-HgsTrace`指定一个已知`-Path`和诊断你想要对生成的跟踪运行的列表。  例如：

   ```PowerShell
   Get-HgsTrace -Path C:\Traces -Diagnostic Networking,BestPractices
   ```
2. 请求每个主机管理员包生成的跟踪文件夹并将其发送给你。  可以通过电子邮件、 文件共享或基于操作的策略和过程由你的组织建立的任何其他机制通过驱动此过程。

3. 将接收到的所有跟踪都合并到一个文件夹，与任何其他内容或文件夹。

    * 例如，假设您有管理员发送您跟踪从计算机中收集四个名为 HGS-01、 HGS-02、 RR1N2608-12 和 RR1N2608 13。  每个管理员将已向你发送一个文件夹相同的名称。  将组合显示，如下所示的目录结构：

      ```
      FabricTraces
      |- HGS-01
      |  |- TargetMetadata.xml
      |  |- Metadata.xml
      |  |- [any other trace files for this host]
      |- HGS-02
      |  |- [...]
      |- RR1N2608-12
      |  |- [...]
      |- RR1N2608-13
         |- [..]
      ```

4. 执行诊断，提供有关已组装的跟踪文件夹的路径`-Path`参数并指定`-RunDiagnostics`切换以及提出你的管理员以收集跟踪这些诊断。  诊断将假定它不能访问路径内找到的主机，并因此将尝试使用仅预先收集的跟踪。  如果任何跟踪丢失或损坏，诊断将仅受影响的测试失败，并继续正常工作。  例如：

   ```PowerShell
   Get-HgsTrace -RunDiagnostics -Diagnostic Networking,BestPractices -Path ".\FabricTraces"
   ```

### <a name="mixing-saved-traces-with-additional-targets"></a>混合使用与其他目标一起保存的跟踪

在某些情况下，可能会有一组预先收集你想要增加额外的主机跟踪的跟踪。  它是可以混合使用其他目标，将跟踪和诊断在单个调用中的诊断使用预先收集的跟踪。

以下说明进行操作，以收集和组装上面指定的跟踪文件夹，调用`Get-HgsTrace`包含预先收集的跟踪文件夹中找不到更多跟踪目标：

```PowerShell
$hgs03 = New-HgsTraceTarget -HostName "hgs-03.secure.contoso.com" -Credential (Enter-Credential)
Get-HgsTrace -RunDiagnostics -Target $hgs03 -Path .\FabricTraces
``` 

诊断 cmdlet 将在预先收集的所有主机，以及一个仍需跟踪的其他主机标识，并将执行必要的跟踪。  然后将诊断预先收集和全新收集到的所有跟踪的总和。  生成的跟踪文件夹将包含旧的和新跟踪。
