> [!Note] 
> 运行受保护的结构诊断工具 (Get-HgsTrace -RunDiagnostics) 时可能会返回不正确的状态，声明 HTTPS 配置已损坏，而其实际上并未损坏或未使用。 任何 HGS 认证模式可能都会返回此错误。 根本原因可能是：
>
> - HTTPS 确实未正确配置/已损坏<br>
> - 你正在使用受信任的管理员证明，而信任关系已中断<br>
> &nbsp;&nbsp;&nbsp;&nbsp;-在所有这是无论 HTTPS 是否配置正确，不正确，或在不使用。<br>
>
> 请注意，诊断仅在面向 Hyper-V 主机时返回此不正确的状态。 如果诊断面向主机保护者服务服务，将返回正确的状态。

<!-- Appears in guarded-fabric-setting-up-the-host-guardian-service-hgs.md and guarded-fabric-troubleshoot-diagnostics.md
-->
