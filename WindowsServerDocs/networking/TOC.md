# [网络](Networking.md)
## [Windows Server 支持的网络方案](windows-server-supported-networking-scenarios.md)

## [网络中的新增功能](What-s-New-in-Networking.md)

## [适用于 Windows Server 的核心网络指南](core-network-guide/core-network-guide-windows-server.md)
### [核心网络组件](core-network-guide/Core-Network-Guide.md)
### [核心网络助理指南](core-network-guide/cncg/Core-Network-companion-Guides.md)
#### [为 802.1X 有线和无线部署部署服务器证书](core-network-guide/cncg/server-certs/Deploy-Server-Certificates-for-802.1X-Wired-and-Wireless-Deployments.md)
##### [服务器证书部署概述](core-network-guide/cncg/server-certs/Server-Certificate-Deployment-Overview.md)
##### [服务器证书部署规划](core-network-guide/cncg/server-certs/Server-Certificate-Deployment-Planning.md)
##### [服务器证书部署](core-network-guide/cncg/server-certs/Server-Certificate-Deployment.md)
###### [安装 Web 服务器 WEB1](core-network-guide/cncg/server-certs/Install-the-Web-Server-WEB1.md)
###### [在 DNS 中为 WEB1 创建别名 (CNAME) 记录](core-network-guide/cncg/server-certs/create-an-Alias-CNAME-Record-in-DNS-for-WEB1.md)
###### [配置 WEB1 以分发证书吊销列表 (CRL)](core-network-guide/cncg/server-certs/Configure-WEB1-to-Distribute-Certificate-Revocation-lists.md)
###### [准备 CAPolicy.inf 文件](core-network-guide/cncg/server-certs/Prepare-the-CAPolicy-inf-File.md)
###### [安装证书颁发机构](core-network-guide/cncg/server-certs/Install-the-Certification-Authority.md)
###### [在 CA1 上配置 CDP 和 AIA 扩展](core-network-guide/cncg/server-certs/Configure-the-cdP-and-AIA-Extensions-on-CA1.md)
###### [将 CA 证书和 CRL 复制到虚拟目录](core-network-guide/cncg/server-certs/copy-the-CA-Certificate-and-CRL-to-the-Virtual-directory.md)
###### [配置服务器证书模板](core-network-guide/cncg/server-certs/Configure-the-Server-Certificate-Template.md)
###### [配置服务器证书自动注册](core-network-guide/cncg/server-certs/Configure-Server-Certificate-Autoenrollment.md)
###### [刷新组策略](core-network-guide/cncg/server-certs/Refresh-Group-Policy.md)
###### [验证服务器证书的服务器注册](core-network-guide/cncg/server-certs/verify-Server-Enrollment-of-a-Server-Certificate.md)
#### [部署基于密码的 802.1X 身份验证无线访问](core-network-guide/cncg/wireless/a-deploy-8021X-wireless-access.md)
##### [无线访问部署概述](core-network-guide/cncg/wireless/b-wireless-access-deploy-overview.md)
##### [无线访问部署过程](core-network-guide/cncg/wireless/c-wireless-access-deploy-process.md)
##### [无线访问部署规划](core-network-guide/cncg/wireless/d-wireless-access-planning.md)
##### [无线访问部署](core-network-guide/cncg/wireless/e-wireless-access-deployment.md)
#### [部署 BranchCache 托管缓存模式](core-network-guide/cncg/bc-hcm/1-Deploy-Bc-Hcm.md)
##### [BranchCache 托管缓存模式部署概述](core-network-guide/cncg/bc-hcm/2-Bc-Hcm-Deploy-Overview.md)
##### [BranchCache 托管缓存模式部署规划](core-network-guide/cncg/bc-hcm/3-Bc-Hcm-Plan.md)
##### [BranchCache 托管缓存模式部署](core-network-guide/cncg/bc-hcm/4-Bc-Hcm-Deployment.md)
###### [通过服务连接点安装 BranchCache 功能并配置托管缓存服务器](core-network-guide/cncg/bc-hcm/5-Bc-Feature-Scp.md)
###### [移动托管缓存并调整其大小（可选）](core-network-guide/cncg/bc-hcm/6-Bc-move-Resize-Cache.md)
###### [在托管缓存服务器上预哈希和预加载内容（可选）](core-network-guide/cncg/bc-hcm/7-Bc-Prehash-Preload.md)
####### [为 Web 和文件内容创建内容服务器数据包（可选）](core-network-guide/cncg/bc-hcm/8-Bc-Data-Packages.md)
####### [在托管缓存服务器上导入数据包（可选）](core-network-guide/cncg/bc-hcm/9-Bc-import-Data.md)
###### [通过服务连接点配置客户端自动托管缓存发现](core-network-guide/cncg/bc-hcm/10-Bc-Client-By-Scp.md)
##### [其他资源](core-network-guide/cncg/bc-hcm/11-Bc-Hcm-additional-resources.md)

## [BranchCache](branchcache/BranchCache.md)
### [BranchCache netsh 和 Windows PowerShell 命令](branchcache/BranchCache-Network-Shell-and-Windows-powershell-Commands.md)
### [BranchCache 部署指南](branchcache/deploy/BranchCache-Deployment-Guide.md)
#### [选择一种 BranchCache 设计](branchcache/plan/Choosing-a-BranchCache-Design.md)
#### [部署 BranchCache](branchcache/deploy/Deploy-BranchCache.md)
##### [安装和配置内容服务器](branchcache/deploy/Install-and-Configure-Content-Servers.md)
###### [安装使用 BranchCache 功能的内容服务器](branchcache/deploy/Install-Content-Servers-that-Use-the-BranchCache-Feature.md)
####### [安装 BranchCache 功能](branchcache/deploy/Install-the-BranchCache-Feature.md)
####### [配置 Windows Server Update Services (WSUS) 内容服务器](branchcache/deploy/configure-wsus-content-servers.md)
###### [安装文件服务内容服务器](branchcache/deploy/Install-File-Services-Content-Servers.md)
####### [配置文件服务服务器角色](branchcache/deploy/Configure-the-File-Services-server-role.md)
######## [安装新的文件服务器作为内容服务器](branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md)
######## [将现有的文件服务器配置为内容服务器](branchcache/deploy/Configure-an-Existing-File-Server-as-a-Content-Server.md)
####### [启用文件服务器的哈希发布](branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)
######## [启用非域成员文件服务器的哈希发布](branchcache/deploy/Enable-Hash-Publication-for-Non-Domain-Member-File-Servers.md)
######## [启用域成员文件服务器的哈希发布](branchcache/deploy/Enable-Hash-Publication-for-Domain-Member-File-Servers.md)
######### [创建 BranchCache 文件服务器组织单位 (OU)](branchcache/deploy/create-the-BranchCache-File-Servers-Organizational-Unit.md)
######### [将文件服务器移到 BranchCache 文件服务器 OU](branchcache/deploy/move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)
######### [创建 BranchCache 哈希发布组策略对象 (GPO)](branchcache/deploy/create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)
######### [配置 BranchCache 哈希发布 GPO](branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)
####### [在文件共享上启用 BranchCache（可选）](branchcache/deploy/enable-bc-on-file-share.md)
##### [部署托管的缓存服务器（可选）](branchcache/deploy/deploy-hosted-cache-servers.md)
##### [在托管的缓存服务器上预哈希和预加载内容（可选）](branchcache/deploy/prehashing-and-preloading.md)
##### [配置 BranchCache 客户端计算机](branchcache/deploy/Configure-BranchCache-Client-computers.md)
###### [使用组策略配置域成员客户端计算机](branchcache/deploy/Use-Group-Policy-to-Configure-Domain-Member-Client-computers.md)
###### [使用 Windows PowerShell 配置非域成员客户端计算机](branchcache/deploy/Use-Windows-powershell-to-Configure-Non-Domain-Member-Client-computers.md)
####### [配置适用于非域成员的防火墙规则，以允许 BranchCache 流量](branchcache/deploy/Configure-Firewall-Rules-for-Non-Domain-Members-to-Allow-BranchCache-Traffic.md)
###### [验证客户端计算机设置](branchcache/deploy/verify-Client-computer-Settings.md)

## [DirectAccess](../remote/remote-access/da-stub.md) 

## [域名系统 (DNS)](dns/dns-top.md)
### [Windows Server 中 DNS 客户端的新增功能](dns/What-s-New-in-DNS-Client.md)
### [Windows Server 中 DNS 服务器的新增功能](dns/What-s-New-in-DNS-Server.md)
### [DNS 策略方案指南](dns/deploy/DNS-Policy-Scenario-Guide.md)
#### [DNS 策略概述](dns/deploy/DNS-Policies-Overview.md)
#### [使用 DNS 策略通过主服务器实现地理位置流量管理](dns/deploy/primary-geo-location.md)
#### [使用 DNS 策略通过主-辅部署实现地理位置流量管理](dns/deploy/primary-secondary-geo-location.md)
#### [使用 DNS 策略实现基于时间的智能 DNS 响应](dns/deploy/dns-tod-intelligent.md)
#### [基于时间的 DNS 响应和 Azure 云应用服务器](dns/deploy/dns-tod-azure-cloud-app-server.md)
#### [使用 DNS 策略实现拆分式 DNS 部署](dns/deploy/split-brain-DNS-deployment.md)
#### [在 Active Directory 中使用针对拆分式 DNS 的 DNS 策略](dns/deploy/dns-sb-with-ad.md)
#### [使用 DNS 策略在 DNS 查询上应用筛选器](dns/deploy/apply-filters-on-dns-queries.md)
#### [使用 DNS 策略实现应用负载均衡](dns/deploy/app-lb.md)
#### [使用 DNS 策略通过地理位置感知执行应用负载均衡](dns/deploy/app-lb-geo.md)
### [排查 DNS 问题](dns/troubleshoot/troubleshoot-dns-data-collection.md)
#### [排查 DNS 客户端问题](dns/troubleshoot/troubleshoot-dns-client.md)
##### [在 DNS 客户端上禁用 DNS 客户端缓存](dns/troubleshoot/disable-dns-client-side-caching.md)
#### [排查 DNS 服务器问题](dns/troubleshoot/troubleshoot-dns-server.md)

## [动态主机配置协议 (DHCP)](technologies/dhcp/dhcp-top.md)
### [DHCP 中的新功能](technologies/dhcp/What-s-New-in-DHCP.md)
#### [DHCP 子网选择选项](technologies/dhcp/dhcp-subnet-options.md)
#### [用于 DNS 记录注册的 DHCP 日志记录事件](technologies/dhcp/dhcp-dns-events.md)
### [使用 Windows PowerShell 部署 DHCP](technologies/dhcp/dhcp-deploy-wps.md)

## [高性能网络 (HPN)](technologies/hpn/hpn-top.md)
### [网络卸载和优化技术](technologies/hpn/network-offload-and-optimization.md)
#### [仅软件 (SO) 功能和技术](technologies/hpn/hpn-software-only-features.md)
#### [软件和硬件 (SH) 集成功能和技术](technologies/hpn/hpn-software-hardware-features.md)
#### [仅硬件 (HO) 功能和技术](technologies/hpn/hpn-hardware-only-features.md)
#### [NIC 高级属性](technologies/hpn/hpn-nic-advanced-properties.md)

### [Insider Preview](technologies/hpn/hpn-insider-preview.md)
### [在 vSwitch 中接收段合并 (RSC)](technologies/hpn/rsc-in-the-vswitch.md)

### [聚合 NIC 配置指南](technologies/conv-nic/cnic-top.md)
#### [单个网络适配器配置](technologies/conv-nic/cnic-single.md)
#### [数据中心网络适配器配置](technologies/conv-nic/cnic-datacenter.md)
#### [物理交换机配置](technologies/conv-nic/cnic-app-switch-config.md)
#### [聚合 NIC 疑难解答](technologies/conv-nic/cnic-app-troubleshoot.md)

### [数据中心桥接 \(DCB\)](technologies/dcb/dcb-top.md)
#### [安装 DCB](technologies/dcb/dcb-install.md)
#### [管理 DCB](technologies/dcb/dcb-manage.md)

### [虚拟接收方缩放 (vRSS)](technologies/vrss/vrss-top.md)
#### [规划 vRSS 的使用](technologies/vrss/vrss-plan.md)
#### [在虚拟网络适配器上启用 vRSS](technologies/vrss/vrss-enable.md)
#### [管理 vRSS](technologies/vrss/vrss-manage.md)
#### [vRSS 常见问题解答](technologies/vrss/vrss-faq.md)
#### [用于 RSS 和 vRSS 的 Windows PowerShell 命令](technologies/vrss/vrss-wps.md)
#### [解决 vRSS 问题](technologies/vrss/vrss-resolve-issues.md)

## [主机计算网络 (HCN) 服务 API](technologies/hcn/hcn-top.md)   
### [常见 HCN 方案](technologies/hcn/hcn-scenarios.md)
### [HCN RPC 上下文句柄](technologies/hcn/hcn-declaration-handles.md)
### [HCN JSON 文档架构](technologies/hcn/hcn-json-document-schemas.md)
### [C# 生成的代码示例](technologies/hcn/example-c-sharp.md)
### [Go 生成的代码示例](technologies/hcn/example-go.md)

## [Hyper-V 虚拟交换机](technologies/vswitch-stub.md)

## [IP 地址管理 (IPAM)](technologies/ipam/ipam-top.md)
### [IPAM 中的新增功能](technologies/ipam/What-s-New-in-IPAM.md)
### [管理 IPAM](technologies/ipam/Manage-IPAM.md)
#### [DNS 资源记录管理](technologies/ipam/DNS-Resource-Record-Management.md)
##### [添加 DNS 资源记录](technologies/ipam/add-a-DNS-Resource-Record.md)
##### [删除 DNS 资源记录](technologies/ipam/delete-DNS-Resource-Records.md)
##### [筛选 DNS 资源记录视图](technologies/ipam/Filter-the-View-of-DNS-Resource-Records.md)
##### [查看用于特定 IP 地址的 DNS 资源记录](technologies/ipam/View-DNS-Resource-Records-for-a-Specific-IP-address.md)
#### [DNS 区域管理](technologies/ipam/DNS-Zone-Management.md)
##### [创建 DNS 区域](technologies/ipam/create-a-DNS-Zone.md)
##### [编辑 DNS 区域](technologies/ipam/edit-a-DNS-Zone.md)
##### [查看用于 DNS 区域的 DNS 资源记录](technologies/ipam/View-DNS-Resource-Records-for-a-DNS-Zone.md)
##### [查看 DNS 区域](technologies/ipam/View-DNS-Zones.md)
#### [管理多个 Active Directory 林中的资源](technologies/ipam/Manage-Resources-in-Multiple-active-directory-forests.md)
#### [清除利用率数据](technologies/ipam/Purge-Utilization-Data.md)
#### [基于角色的访问控制](technologies/ipam/Role-based-Access-Control.md)
##### [使用服务器管理器管理基于角色的访问控制](technologies/ipam/Manage-Role-Based-Access-Control-with-server-manager.md)
###### [为访问控制创建用户角色](technologies/ipam/create-a-User-Role-for-Access-Control.md)
###### [创建访问策略](technologies/ipam/create-an-Access-Policy.md)
###### [为 DNS 区域设置访问范围](technologies/ipam/Set-Access-Scope-for-a-DNS-Zone.md)
###### [为 DNS 资源记录设置访问范围](technologies/ipam/Set-Access-Scope-for-DNS-Resource-Records.md)
###### [查看角色和角色权限](technologies/ipam/View-Roles-and-Role-Permissions.md)
##### [使用 Windows PowerShell 管理基于角色的访问控制](technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-powershell.md)

## [网络负载均衡](technologies/Network-Load-Balancing.md)



## [网络策略服务器 (NPS)](technologies/nps/nps-top.md)
### [NPS 最佳做法](technologies/nps/nps-best-practices.md)
### [NPS 入门](technologies/nps/nps-getstart-top.md)
#### [连接请求处理](technologies/nps/nps-crp-top.md)
##### [连接请求策略](technologies/nps/nps-crp-crpolicies.md)
##### [领域名](technologies/nps/nps-crp-realm-names.md)
##### [远程 RADIUS 服务器组](technologies/nps/nps-crp-rrsg.md)
#### [网络策略](technologies/nps/nps-np-overview.md)
##### [访问权限](technologies/nps/nps-np-access.md)
#### [NPS 模板](technologies/nps/nps-templates.md)
#### [RADIUS 客户端](technologies/nps/nps-radius-clients.md)
### [规划 NPS](technologies/nps/nps-plan-top.md)
#### [将 NPS 规划为 RADIUS 服务器](technologies/nps/nps-plan-server.md)
#### [将 NPS 规划为 RADIUS 代理](technologies/nps/nps-plan-proxy.md)
### [部署 NPS](technologies/nps/nps-deploy.md)
### [管理 NPS](technologies/nps/nps-manage-top.md)
#### [使用管理工具管理网络策略服务器](technologies/nps/nps-admintools.md)
#### [配置连接请求策略](technologies/nps/nps-crp-configure.md)
#### [针对 RADIUS 流量配置防火墙](technologies/nps/nps-firewalls-configure.md)
#### [配置网络策略](technologies/nps/nps-np-configure.md)
#### [配置 NPS 记帐](technologies/nps/nps-accounting-configure.md)
#### [配置 RADIUS 客户端](technologies/nps/nps-radius-clients-configure.md)
#### [配置远程 RADIUS 服务器组](technologies/nps/nps-crp-rrsg-configure.md)
#### [管理用于 NPS 的证书](technologies/nps/nps-manage-certificates.md)
##### [配置用于满足 PEAP 和 EAP 要求的证书模板](technologies/nps/nps-manage-cert-requirements.md)
#### [管理多个 NPS](technologies/nps/nps-manage-servers.md)
##### [在多宿主计算机上配置 NPS](technologies/nps/nps-multihomed-configure.md)
##### [配置 NPS UDP 端口信息](technologies/nps/nps-udp-ports-configure.md)
##### [禁用 NAS 通知转发](technologies/nps/nps-disable-nas-notifications.md)
##### [导出 NPS 配置以在另一台服务器上导入](technologies/nps/nps-manage-export.md)
##### [增加由 NPS 处理的并发身份验证](technologies/nps/nps-concurrent-auth.md)
##### [安装 NPS](technologies/nps/nps-manage-install.md)
##### [NPS 代理服务器负载均衡](technologies/nps/nps-manage-proxy-lb.md)
##### [在 Active Directory 域中注册 NPS](technologies/nps/nps-manage-register.md)
##### [从 Active Directory 域中注销 NPS](technologies/nps/nps-manage-unregister.md)
##### [在 NPS 中使用正则表达式](technologies/nps/nps-crp-reg-expressions.md)
##### [在 NPS 更改后验证配置](technologies/nps/nps-manage-verify.md)
##### [NPS 数据收集](technologies/nps/nps-data-collection.md)
#### [管理 NPS 模板](technologies/nps/nps-manage-templates.md)

## [Network Shell (Netsh)](technologies/netsh/netsh.md)
### [Netsh 命令语法、上下文和格式设置](technologies/netsh/netsh-contexts.md)
### [Network Shell (Netsh) 示例批处理文件](technologies/netsh/netsh-wins.md)
### [Netsh http 命令](technologies/netsh/netsh-http.md)
### [Netsh 界面端口代理命令](technologies/netsh/netsh-interface-portproxy.md)

## [网络子系统性能优化](technologies/network-subsystem/net-sub-performance-top.md)
### [选择网络适配器](technologies/network-subsystem/net-sub-choose-nic.md)
### [配置网络接口顺序](technologies/network-subsystem/net-sub-interface-metric.md)
### [性能优化网络适配器](technologies/network-subsystem/net-sub-performance-tuning-nics.md)
### [与网络相关的性能计数器](technologies/network-subsystem/net-sub-performance-counters.md)
### [用于网络工作负载的性能工具](technologies/network-subsystem/net-sub-performance-tools.md)

## [NIC 组合](technologies/nic-teaming/NIC-Teaming.md)
### [NIC 组合 MAC 地址的使用和管理](technologies/nic-teaming/NIC-Teaming-MAC-address-Use-and-Management.md)
### [在主计算机或 VM 上创建新的 NIC 组](technologies/nic-teaming/create-a-New-NIC-Team-on-a-Host-computer-or-VM.md)
### [NIC 组合疑难解答](technologies/nic-teaming/Troubleshooting-NIC-Teaming.md)

## [服务质量 (QoS) 策略](technologies/qos/qos-policy-top.md)
### [QoS 策略入门](technologies/qos/qos-policy-get-started.md)
#### [QoS 策略的工作原理](technologies/qos/qos-policy-works.md)
#### [QoS 策略体系结构](technologies/qos/qos-policy-architecture.md)
#### [QoS 策略方案](technologies/qos/qos-policy-scenarios.md)
###[管理 QoS 策略](technologies/qos/qos-policy-manage.md)
#### [QoS 策略事件和错误](technologies/qos/qos-policy-errors.md)
### [QoS 策略常见问题解答](technologies/qos/qos-policy-faq.md)

## [软件定义的网络 (SDN)](sdn/index.yml)

### [Windows Server 中的 SDN 概述](sdn/software-defined-networking.md)

### [SDN 技术](sdn/technologies/Software-Defined-Networking-Technologies.md)
#### [Hyper-V 网络虚拟化](sdn/technologies/hyper-v-network-virtualization/Hyper-V-Network-Virtualization.md)
##### [Hyper-V 网络虚拟化概述](sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server.md)
##### [Hyper-V 网络虚拟化技术细节](sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-technical-details-windows-server.md)
##### [Hyper-V 网络虚拟化中的新增功能](sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)
#### [SDN 的内部 DNS 服务 (iDNS)](sdn/technologies/Idns-for-Sdn.md)
#### [网络控制器](sdn/technologies/network-controller/Network-Controller.md)
##### [网络控制器高可用性](sdn/technologies/network-controller/network-controller-high-availability.md)
##### [使用服务器管理器安装网络控制器服务器角色](sdn/technologies/network-controller/Install-the-Network-Controller-server-role-using-server-manager.md)
##### [网络控制器的后期部署步骤](sdn/technologies/network-controller/post-deploy-steps-nc.md)
#### [网络功能虚拟化](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md)
##### [数据中心防火墙概述](sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md)
##### [用于 SDN 的 RAS 网关](sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md)
###### [RAS 网关中的新增功能](sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)
###### [RAS 网关部署体系结构](sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)
###### [RAS 网关高可用性](sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)
##### [用于 SDN 的软件负载均衡 (SLB)](sdn/technologies/network-function-virtualization/software-load-balancing-for-sdn.md)
#### [用于 SDN 的交换机嵌入式组合 (SET)](sdn/technologies/Set-for-Sdn.md)
#### [容器网络](sdn/technologies/Containers/Container-networking-overview.md)

### [规划 SDN](sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)
#### [部署网络控制器的安装和准备要求](sdn/plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)

### [部署 SDN](sdn/deploy/Deploy-Software-Defined-Networking.md)
#### [部署 SDN 基础结构](sdn/deploy/Deploy-a-Software-Defined-Network-Infrastructure.md)
##### [使用脚本部署 SDN 基础结构](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)
#### [使用 Windows PowerShell 部署 SDN 技术](sdn/deploy/Deploy-Software-Defined-Network-Technologies-using-Windows-powershell.md)
##### [使用 Windows PowerShell 部署网络控制器](sdn/deploy/Deploy-Network-Controller-using-Windows-powershell.md)

### [管理 SDN](sdn/manage/manage-sdn.md)
#### [管理租户虚拟网络](sdn/manage/Manage-Tenant-Virtual-Networks.md)
##### [了解虚拟网络和 VLAN 的使用情况](sdn/manage/Understanding-Usage-of-Virtual-Networks-and-VLANs.md)
##### [使用访问控制列表 (ACL) 管理数据中心网络流量](sdn/manage/use-acls-for-traffic-flow.md)
##### [创建、删除或更新租户虚拟网络](sdn/manage/create,-delete,-or-Update-Tenant-Virtual-Networks.md)
##### [将虚拟网关添加到租户虚拟网络](sdn/manage/add-a-Virtual-Gateway-to-a-Tenant-Virtual-Network.md)
##### [将容器终结点连接到租户虚拟网络](sdn/manage/Connect-container-endpoints-to-a-Tenant-Virtual-Network.md)
##### [配置虚拟子网的加密](sdn/vnet-encryption/sdn-config-vnet-encryption.md)
##### [虚拟网络中的出站计量](sdn/manage/sdn-egress.md)

#### [管理租户工作负载](sdn/manage/Manage-Tenant-Workloads.md)
##### [创建 VM 并连接到租户虚拟网络或 VLAN](sdn/manage/create-a-Tenant-VM.md)
##### [为租户 VM 网络适配器配置 QoS](sdn/manage/Configure-QoS-for-Tenant-VM-Network-Adapter.md)
##### [配置数据中心防火墙 ACL](sdn/manage/Configure-Datacenter-Firewall-Acls.md)
##### [配置软件负载均衡器以实现负载均衡和网络地址转换 (NAT)](sdn/manage/Configure-SLB-and-Nat.md)
##### [在虚拟网络上使用网络虚拟设备](sdn/manage/Use-Network-Virtual-Appliances-on-a-VN.md)
##### [虚拟网络中的来宾群集](sdn/manage/guest-clustering.md)
#### [更新、备份和还原 SDN 基础结构](sdn/manage/Update-Backup-Restore.md)

### [SDN 的安全性](sdn/security/sdn-security-top.md)
#### [保护网络控制器](sdn/security/nc-security.md)
#### [管理 SDN 证书](sdn/security/sdn-manage-certs.md)
#### [包含服务主体名称 (SPN) 的 Kerberos](sdn/security/kerberos-with-spn.md)
#### [SDN 防火墙审核](sdn/security/sdn-firewall-auditing.md)

### [虚拟网络对等](sdn/vnet-peering/sdn-vnet-peering.md)
#### [配置虚拟网络对等](sdn/vnet-peering/sdn-configure-vnet-peering.md)

### [Windows Server 2019 网关性能](sdn/gateway-performance.md)
### [网关带宽分配](sdn/gateway-allocation.md)

### [SDN 疑难解答](sdn/troubleshoot/Troubleshoot-Software-Defined-Networking.md)
#### [Windows Server 软件定义的网络堆栈疑难解答](sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack.md)

### [用于 SDN 的 System Center 技术](sdn/Sc-Tech-for-Sdn.md)
### [Microsoft Azure 和 SDN](sdn/Azure_and_Sdn.md)
### [联系数据中心和云网络团队](sdn/contact-sdn-team.md)

## [虚拟专用网 (VPN)](technologies/vpn-stub.md)

## [Windows Internet 名称服务 (WINS)](technologies/wins/wins-top.md)

## [Windows 时间服务](windows-time-service/windows-time-service-top.md)
### [Insider Preview - Windows Server 2019 中的 Windows 时间服务](windows-time-service/insider-preview.md)
### [Windows Server 2016 准确时间](windows-time-service/accurate-time.md)
### [支持高精度时间边界](windows-time-service/support-boundary.md)
### [配置高精度的系统](windows-time-service/configuring-systems-for-high-accuracy.md)
### [Windows 时间实现可追溯性](windows-time-service/windows-time-for-traceability.md)
### [Windows 时间服务技术参考](windows-time-service/windows-time-service-tech-ref.md)
#### [Windows 时间服务的工作原理](windows-time-service/How-the-Windows-Time-Service-Works.md)
#### [Windows 时间服务工具和设置](windows-time-service/Windows-Time-Service-Tools-and-Settings.md)

