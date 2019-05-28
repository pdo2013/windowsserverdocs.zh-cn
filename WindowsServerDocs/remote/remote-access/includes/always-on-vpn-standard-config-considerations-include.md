## <a name="standard-configuration-considerations"></a>标准配置注意事项

Always On VPN 有很多配置选项。 但是，选择你的 VPN 配置，不过，包含以下信息：

-   **连接类型。** 连接协议选择很重要，最终将使用的身份验证类型发展同步并进。 有关可用的隧道协议的详细信息，请参阅[VPN 连接类型](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-connection-type/)。

-   **路由。** 在此上下文中，路由规则确定用户是否可以使用其他网络路由到 VPN 连接时。

    -   _拆分隧道_允许同时访问其他网络，如 Internet。

    -   _强制隧道_要求所有流量以独占方式通过 VPN 和不允许同时访问其他网络。

-   **触发。** _触发_确定 （例如，在应用打开时，打开设备时，手动用户） 如何以及何时启动 VPN 连接。 用于触发选项，请参阅[VPN 自动触发的配置文件选项](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-auto-trigger-profile/)。

-   **设备或用户的身份验证。** Always On VPN 使用设备证书并通过名为的功能由设备发起连接[设备隧道](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config)。 该连接可自动启动，并且持久性，类似于 DirectAccess 基础结构隧道连接。

>[!TIP]
>当从 DirectAccess 迁移到 Always On VPN，请考虑从开始到你，并从该处然后展开可比较的配置选项。

通过使用用户证书，Always On VPN 客户端将连接自动，但它会在用户级别 （用户在登录后） 而不是在设备级别 （之前用户登录）。 体验仍感觉不到任何用户，但它支持更高级的身份验证机制，例如 Windows hello 企业版。