---
ms.assetid: e6fa9069-ec9c-4615-b266-957194b49e11
title: 将 AD RMS 升级到 Windows Server 2016
description: ''
author: msmbaldwin
ms.author: esaggese
ms.date: 05/30/2019
ms.topic: article
ms.openlocfilehash: ce058a2885315c84d2c1c6701ad2801790d3c590
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/07/2019
ms.locfileid: "66814070"
---
# <a name="upgrading-ad-rms-to-windows-server-2016"></a>将 AD RMS 升级到 Windows Server 2016

## <a name="introduction"></a>简介

Active Directory Rights Management Services (AD RMS) 是保护敏感的文档和电子邮件的 Microsoft 服务。 与传统的保护方法，例如防火墙和 Acl，不同的 AD RMS 加密和保护是永久的文件的位置或传输方式无关。 

本文档提供有关从 Windows Server 2012 R2 与 SQL Server 2012 迁移到 Windows Server 2016 和 SQL Server 2016 的指导。 可以使用相同的过程来从较旧但受支持版本的 AD RMS 迁移。
请注意，Active Directory Rights Management Services 不再处于积极开发阶段，并且为最新功能的客户应考虑迁移到[Azure 信息保护](https://azure.microsoft.com/services/information-protection/)，它提供了更多全面的功能以及更完整的设备和应用程序支持。 

有关迁移到 Azure 信息保护从 AD RMS 而无需重新保护你的内容的信息，请参阅[Azure 信息保护迁移文档](https://docs.microsoft.com/azure/information-protection/migrate-from-ad-rms-to-azure-rms)。

## <a name="about-the-environment-used-in-this-guide"></a>有关本指南中使用的环境

AD FS 是 AD RMS 安装的可选组件。 在本指南中，假定使用 ADFS。 如果已为支持 AD RMS 用户，未在环境中使用 ADFS，则可以跳到 ADFS 引用的所有步骤。

在本指南中，SQL Server 都将升级到 SQL Server 2016 通过执行并行安装，并通过备份将数据库移动到上方。 或者，如果可以对 SQL Server 2016 的就地升级你的 AD RMS 和 ADFS 数据库服务器，您可以后将移到此文档中的下一步部分完成后无需按照本部分中的步骤。  

## <a name="installation"></a>安装

### <a name="configuring-sql-server-2016"></a>配置 SQL Server 2016

以下部分详细信息实现任务直接相关的 SQL Server 2016 配置。 本指南重点介绍使用服务器管理器和 SQL Server Management Studio 来完成这些任务。

在 SQL Server 2016 安装必须完成这些步骤。 根据你的组织标准的实践和策略的合适硬件上安装 SQL Server 2016。 

#### <a name="preparing-the-sql-server"></a>准备 SQL Server

以下部分详细介绍如何进行准备，以便它可以升级到 SQL Server 2016 升级 AD RMS 平台使用 Windows Server 2016 中的其他服务之前，SQL Server。

##### <a name="adding-cname-for-sql-server-2016-to-dns"></a>适用于 SQL Server 2016 向 DNS 添加 CNAME

CNAME 用于帮助确保 Windows Server 2016 安装程序将会得到适当的数据由于指向新的 SQL Server 2016。 **注意：如果已使用 ADFS 和 AD RMS 服务的 CNAME，你可以转到下一个步骤。**


**若要向 DNS 添加 SQL Server 2016 CNAME**

1.  登录到 Windows Server 2012 R2 域控制器，具有域管理员凭据。

2.  打开服务器管理器。

3.  单击**工具**，然后选择**DNS**若要打开 DNS 管理器。

4.  从左侧的导航窗格中，展开 DC 并打开**正向查找区域**。

5.  打开相应的域资源，然后在右视图窗格中右键单击并选择**新别名 (CNAME)** 开始创建 CNAME。

6.  对于别名名称，输入中可能存在 （这以使其区别于其他的逻辑名称 SQLADRMS 或 SQLADFS）

7.  输入名称后, 提供将成为新的 SQL Server 2016 服务器的目标主机的 FQDN。 （例如。 SQL2016.contoso.com)

8.  后输入的所有信息后，单击**确定**。

##### <a name="backup-the-ad-rms-and-adfs-databases"></a>备份 AD RMS 和 ADFS 数据库

AD RMS 和 ADFS 数据库保存到 AD RMS，公钥的服务器许可方证书、 权限策略模板、 ADFS 配置数据和日志记录信息如有必要关键信息。 具备这些数据库，客户端无法颁发许可证才能使用受保护的内容，其他问题。

在数据库中，AD RMS 配置数据库被视为最重要的因为它已存储了 SLC，权限策略模板、 用户的密钥和配置信息。 因此，尽管您需要采取措施来备份所有 AD RMS 和 ADFS 数据库，您应计划定期备份配置数据库。

日志记录数据库存储有关用户请求的证书和使用许可证的 AD RMS 群集的信息。 此数据库的备份策略应基于保留此类信息的公司策略。

目录服务数据库不是 AD RMS 功能的关键，并且数据库丢失的最新数据时，将重新填充与信息由于 AD RMS 服务器收到的证书请求和使用许可证。 您不需要定期备份此数据库，但需要具有数据库的至少一个副本，它最初配置后部署 AD RMS。

**若要备份 AD RMS 和/或 ADFS 与 Microsoft SQL Server 数据库**

1.  登录到具有 SQL 2012 的 Windows Server 2012 R2 AD RMS 数据库服务器上。

2.  单击**启动**，单击**所有程序**，单击**Microsoft SQL Server**，然后单击**SQL Server Management Studio**。

3.  在中**连接到服务器**窗口中，确认承载 AD RMS 数据库的服务器处于**服务器名称**框，然后单击**Connect**。

4.  展开**数据库**。 右键单击相应的数据库 (**DRM**并**Adfs**)，指向**任务**，然后选择**备份**。

5.  为剩余的数据库重复步骤 4。

6.  请确保网络或使用存储设备，因为它们将需要后面的步骤在迁移期间的其他计算机可以访问数据库的备份。

现在可以将数据库副本存储在安全的位置。 请记住经常备份你的数据库。

##### <a name="adding-domain-admin-sql-ad-rms-andor-adfs-service-account-to-sql-server-2016"></a>添加域管理员，SQL、 AD RMS 和/或 ADFS 服务帐户添加到 SQL Server 2016

以下步骤将展示如何将多个服务帐户添加到 SQL Server 2016，以帮助从 Windows Server 2012 R2 环境迁移数据。 尝试访问的内容和处理数据时，这会提供适当的权限。

**若要将域管理员、 SQL、 AD RMS 和/或 ADFS 服务帐户添加到 SQL Server**

1.  本地管理员帐户登录到 SQL Server 2016 的服务器上。

2.  单击**启动**，单击**所有程序**，单击**Microsoft SQL Server**，然后单击**SQL Server Management Studio**。

3.  在中**连接到服务器**窗口中，确认承载 AD RMS 数据库的服务器处于**服务器名称**框，然后进行身份验证中，单击下拉列表菜单，然后选择**SQL Server身份验证**。

4.  在中**登录名**字段中，输入本地管理员帐户 （不包括的名称 localadmin) 并提供合适的密码，然后单击**Connect**。

5.  展开**安全**，然后右击**登录名**，然后选择**新登录名**从显示的上下文菜单。

6.  窗口出现输入中的域管理员帐户后**登录名**字段 （例如， Contoso\\ContosoAdmin)

7.  在左侧的导航窗格中，选择**服务器角色**。

8.  然后将标记的复选框**sysadmin**下的服务器角色和单击**确定**。

9.  重新启动**SQL Server 管理**。

10. 在中**连接到服务器**窗口中，确认承载 AD RMS 数据库的服务器处于**服务器名称**框，然后进行身份验证中，单击下拉列表菜单，然后选择**Windows身份验证**然后单击**Connect**。

##### <a name="restoring-the-ad-rms-and-adfs-databases-to-sql-server-2016"></a>将 AD RMS 和 ADFS 数据库还原到 SQL Server 2016

以下步骤将展示如何从早期的 SQL Server 实例将数据还原到新的 2016年实例。 这将允许新的 SQL，以利用从以前的 AD RMS 和 ADFS 数据库相关的配置数据。

**若要从以前的 SQL Server 将数据还原到新的 SQL Server**

1.  登录到使用适当帐户具有 SQL Server 2016 的服务器上。

2.  从左侧的导航窗格中，右键单击**数据库**，然后选择**Restore Database**开始还原过程。

3.  下**源**选择**设备**，然后浏览数据库文件已存储在前面步骤中的位置。

4.  一旦选择了文件，单击**确定**。

5.  确保所有数据库文件已添加，并通过单击完成过程**确定**。

### <a name="configuring-windows-server-2016-active-directory-federation-services-ad-fs"></a>配置 Windows Server 2016 Active Directory 联合身份验证服务 (AD FS)

若要为 AD RMS 的应用程序提供单一登录 (SSO) 访问，已部署 AD FS。 它同时已配置与 AD RMS 移动设备扩展 (MDE)，可让你 Mac 和最终用户的移动设备支持。

以下各节提供有关可能需要在 AD FS 部署上执行的操作任务的指导。

#### <a name="adding-a-2016-ad-fs-server-to-the-farm"></a>向场中添加 2016 AD FS 服务器

您可以部署附加的 AD FS 服务器以支持 AD RMS 部署。 您可以选择执行此操作发生到的 AD RMS 服务器或其他应用程序的流量增加时或如果你需要停用某个当前正在使用 AD FS 服务器。

**若要向场中添加 2016 AD FS 服务器**

1.  从 Azure AD Connect 服务器中，双击**Azure AD Connect**图标以启动 Azure AD Connect 向导。

2.  在欢迎页上，单击**配置**。

3.  在其他任务页中，单击**部署其他联合身份验证服务器**，然后单击**下一步**。

4.  在连接到 Azure AD 页中，输入用户名和密码的具有全局管理权限的帐户，然后单击**下一步**。

5.  在域管理员凭据页中，输入用户名和密码的具有域管理员权限的帐户，然后单击**下一步**。

6.  单击**浏览**和证书文件时使用的选择配置使用 Azure AD Connect 的 AD FS 场。

7.  单击**输入密码**以打开证书密码对话框。

8.  在密码字段中输入证书的密码，然后单击**确定**。

9.  单击“下一步”  。

10. 在 AD FS 服务器页中，输入名称或新的 AD FS 服务器的 IP 地址，然后单击**添加**。

11. 在已准备好配置页上，单击**安装**。

12. 在安装完成页上，单击**退出**。

#### <a name="raising-the-adfs-farm-behavior-level"></a>引发的 ADFS 场行为级别

当部署超出了当前的环境级别，如 ADFS 对 Windows Server 2012 R2，然后添加 ADFS Windows Server 2016、 场行为级别将需要增加的 ADFs 服务器。 这被需要确保在环境使用的最新信息和函数。

**若要将 ADFS 场行为级别提升**

1.  导航到 Windows Server 2016 ADFS。

2.  打开管理员 PowerShell 会话。

3.  输入以下命令：  **\$cred = Get-credential**

4.  将出现一个窗口，要求提供凭据，输入域管理员凭据。

5.  然后输入以下命令：**Invoke-AdfsFarmBehaviorLevelRaise -Credential \$cred**

6.  在提示符下会出现询问，**是否要继续此操作？** 然后输入  接受提示。

7.  该命令完成后，场行为级别将安装程序并准备就绪。

#### <a name="enabling-mobile-device-extension-logging"></a>启用移动设备扩展日志记录

移动设备扩展可以记录它从最终用户设备收到的请求。 默认情况下禁用日志记录，我们建议仅启用故障排除方案中的日志记录。 AD RMS 日志记录数据库或 Azure 存储帐户中记录所有请求，来自移动设备和台式计算机，若要启动或获取最终用户许可证。 MDE 日志记录将创建到 AD RMS 使用的 SQL Server 的两个其他表： 客户端调试日志表和客户端性能日志表。

**若要启用移动设备扩展日志记录**

1.  从 AD RMS 服务器，以管理员身份打开 Windows PowerShell。

2.  键入以下命令并按**Enter**:**导入模块 AdRmsAdmin**

3.  键入以下命令并按**Enter**:**New-PSDrive -Name AdrmsCluster -PsProvider AdRmsAdmin -Root https://localhost**

4.  键入以下命令并按**Enter**:**Set-itemproperty-路径 AdrmsCluster:\\ -名称 IsLoggingEnabled-值\$，则返回 true**

如果使用 MDE 日志记录进行故障排除，我们建议解决此问题后禁用它。

**若要禁用移动设备扩展日志记录**

1.  从 AD RMS 服务器，以管理员身份打开 Windows PowerShell。

2.  键入以下命令并按**Enter**:**导入模块 AdRmsAdmin**

3.  键入以下命令并按**Enter**:**New-PSDrive -Name AdrmsCluster -PsProvider AdRmsAdmin -Root https://localhost**

4.  键入以下命令并按**Enter**:**Set-itemproperty-路径 AdrmsCluster:\\ -名称 IsLoggingEnabled-值\$false**

### <a name="upgrading-ad-rms-to-windows-server-2016"></a>将 AD RMS 升级到 Windows Server 2016

以下各节提供有关如何将基于 Windows Server 2016 的 AD RMS 服务器添加到当前的 Windows Server 2012 R2 群集的指导。 服务器将添加到群集和的信息将复制到它，以便可以弃用以前的 AD RMS 服务器以释放资源。

添加一个基于 Windows Server 2016 的 AD RMS 服务器已添加到你的 AD RMS 群集后，基于较旧版本的 Windows 上的所有节点将都变为不活动状态。 此操作完成后可以解除配置这些服务器 （例如关闭、 重新调整其用途或重新安装 Windows Server 2016 加入 AD RMS 群集）。 

可以将其他 AD RMS 服务器部署到群集以支持你的 AD RMS 部署上的负载。 您还可以选择执行此操作发生到 AD RMS 服务器的流量增加时。

本指南不涉及更改负载平衡的机制，您可能会在环境中使用来排除弃用的服务器并包含要添加到群集的所需的步骤。 

#### <a name="adding-a-2016-ad-rms-server"></a>添加 2016 AD RMS 服务器

如果你的 AD RMS 群集使用硬件安全模块而不集中管理密钥其服务器许可方证书，需要在服务器上安装 AD RMS 之前安装软件和其他 HSM 项目 （例如密钥和配置文件）。 此外需要将 HSM 连接到服务器，以物理方式或通过相关的网络配置。 请按照以下步骤在 HSM 指南。 

**若要添加的 2016 AD RMS 服务器**

1.  在所需的 Windows Server 2016 部署上安装 AD RMS 角色。

2.  安装完成后，选择链接到**执行其他配置**。

3.  选择**加入现有 AD RMS 群集**然后单击**下一步**。

4.  上**选择配置数据库**页上，输入 2016 SQL server (FQDN) 在 DNS 中指定的 CNAME。

5.  单击**列表**第二个行，然后选择**DefaultInstance**从下拉列表。

6.  下**配置数据库的名称**、 选择下拉列表菜单，然后选择显示的 DRM 配置。 然后，单击“下一步”  。

7.  上**数据库信息**页上，提供的字段中输入群集密钥密码。 之后，单击**下一步**。

8.  在向导的下一页上，指定 AD RMS 服务帐户和为其提供密码，然后单击**下一步**后已经过验证。

9.  一次**群集网站**页出现后，只需确保已选择相应的网站，然后单击**下一步**。

10. 上**选择一个服务器身份验证证书**页上，选择导入的 SSL 证书，然后单击**下一步**。

11. 单击“安装”  以开始安装。

12. 配置完成后，你将需要注销然后重新登录以管理 AD RMS。

13. 一旦登录后，打开**服务器管理器**选择**工具**，然后**Active Directory Rights Management**。 管理窗口应出现，并且指示该群集在群集中拥有其他服务器。

14. 14. 如果 AD RMS 移动设备扩展安装在原始的 AD RMS 群集，需要已更新的群集节点中还安装 MDE。 按照 MDE 文档中的说明将 MDE 添加到你的 AD RMS 群集。 此时，可以重新调整其用途预先存在的所有节点或将其升级到 Windows Server 2016 并重新将其联接到 AD RMS 群集使用上面所述的相同过程。 

### <a name="configuring-windows-server-2016-web-application-proxy-wap"></a>配置 Windows Server 2016 Web 应用程序代理 (WAP)

以下各节提供有关可能需要对你的 Web 应用程序代理部署执行的操作任务的指导。 这是一个可选步骤，不是必需的如果要通过其他机制向 Internet 发布 AD RMS。 

#### <a name="adding-a-windows-server-2016-wap-server"></a>添加 Windows Server 2016 WAP 服务器

您可以部署附加的 Web 应用程序代理服务器以支持 AD RMS 部署。 您可以选择执行此操作发生到 AD RMS 服务器的流量增加或如果你需要停用某个当前正在使用 Web 应用程序代理服务器时。

**若要添加的 2016 Web 应用程序代理服务器**

1.  在您希望安装程序作为 Web 应用程序代理服务器，导航到服务器管理器控制台，然后单击**添加角色和功能**。

2.  在中**添加角色和功能向导**，单击**下一步**直到你转到服务器角色选择屏幕。

3.  在选择服务器角色屏幕中，选择**远程访问**，然后单击**下一步**直到您在选择服务器角色屏幕。

4.  在选择服务器角色屏幕中，选择**Web 应用程序代理**，单击**添加功能**，然后单击**下一步**。

5.  在确认安装选择屏幕上，单击**安装**。

6.  完成安装后，单击**关闭**。

7.  现在就来配置服务器。 若要执行此操作，请打开 Web 应用程序代理服务器上的远程访问管理控制台。 打开**启动**菜单中，键入**RAMgmtUI.exe**，然后选择应用程序。

8.  在导航窗格中，单击**Web 应用程序代理**。

9.  在远程访问管理控制台中单击**运行 Web 应用程序代理配置向导**。 一次在向导中，单击**下一步**。

10. 在联合身份验证服务器屏幕上输入 （这在 AD FS 服务器的完全限定的域名 adfs.contoso.com) 和 AD FS 服务器上，然后输入为管理员凭据。

11. 在 AD FS 代理证书屏幕上，在 Web 应用程序代理服务器上当前安装的证书列表中选择要由 AD FS 代理，Web 应用程序代理的证书，然后单击**下一步**。

12. 在确认屏幕上，查看设置，然后单击**配置**。

13. 配置完成后，单击**关闭**。

#### <a name="dns-configuration-for-2016-wap-server"></a>2016 年的 DNS 配置 WAP 服务器

后已在位置放置 Windows Server 2016 Web 应用程序代理服务器，将需要进行一些 DNS 更改。 这将需要在 2016 WAP 服务器上使用 GoDaddy 点 ADFS 之类的服务和 AD RMS 服务。

**若要在 WAP 服务器指向 DNS**

1.  导航到提供商的网站 （例如。 GoDaddy)。

2.  进入域管理和 DNS 管理。

3.  找到 ADFS 和 AD RMS 服务并替换**指向**2016 WAP 服务器的公共 IP 地址的部分以及**保存**。

4.  所做的更改可能需要时间才能传播，但它们执行操作后将完成此安装程序。

#### <a name="enabling-debugging-logs"></a>启用调试日志

在 Web 应用程序代理服务器上提供了详细日志记录信息。 你可以配置高级调试日志记录使用事件查看器。 其他设置也可以选择用于日志的大小以帮助确保分析查看器非常有用。

**为 Web 应用程序代理启用调试日志**

1.  打开**事件查看器**控制台在 Web 应用程序代理服务器上的。

2.  展开**Microsoft**节点。

3.  展开**Windows**节点。

4.  打开**Web 应用程序代理**日志。

5.  然后将能够打开**管理员**日志。

6.  打开**操作**菜单中，位于左上方，然后选择**属性**。

7.  下**常规**选项卡上，选择选项**启用日志记录**。

8.  最后，你就能够自定义最大日志大小和已达到最大事件日志大小会发生什么情况。

### <a name="configuring-high-availability-for-windows-server-2016-services"></a>为 Windows Server 2016 服务配置高可用性

以下各节提供有关操作的任务可能需要设置你的 Windows Server 2016 环境中实现高可用性的指导。

#### <a name="adding-a-2016-ad-rms-server-for-high-availability"></a>添加 2016 AD RMS 服务器以实现高可用性

您可以部署附加的 AD RMS 服务器设置高可用性。 您可以选择执行此操作发生到 AD RMS 服务器的流量增加时。

**若要添加的 2016 AD RMS 服务器以实现高可用性**

1.  在所需的 Windows Server 2016 部署上安装 AD RMS 角色。

2.  安装完成后，选择链接到**执行其他配置**。

3.  选择**加入现有 AD RMS 群集**然后单击**下一步**。

4.  上**选择配置数据库**页上，输入 2016 SQL server (FQDN) 在 DNS 中指定的 CNAME。

5.  单击**列表**第二个行，然后选择**DefaultInstance**从下拉列表。

6.  下**配置数据库的名称**、 选择下拉列表菜单，然后选择显示的 DRM 配置。 然后，单击“下一步”  。

7.  上**数据库信息**页上，提供的字段中输入群集密钥密码。 之后，单击**下一步**。

8.  在向导的下一页上，指定 AD RMS 服务帐户和为其提供密码，然后单击**下一步**后已经过验证。

9.  一次**群集网站**页出现后，只需确保已选择相应的网站，然后单击**下一步**。

10. 上**选择一个服务器身份验证证书**页上，选择导入的 SSL 证书，然后单击**下一步**。

11. 单击“安装”  以开始安装。

12. 配置完成后，你将需要注销然后重新登录以管理 AD RMS。

13. 一旦登录后，打开**服务器管理器**选择**工具**，然后**Active Directory Rights Management**。 管理窗口应出现，并且指示该群集在群集中拥有其他服务器。

14. 在确认服务器安装程序，将负载平衡服务配置为群集中不同的 AD RMS 服务器之间平衡负载。

#### <a name="adding-a-windows-server-2016-ad-fs-server-for-high-availability"></a>添加 Windows Server 2016 AD FS 服务器以实现高可用性

您可以部署附加的 AD FS 服务器设置高可用性。 您可以选择执行此操作发生到 AD FS 服务器的流量增加时。 **注意： 提高场行为级别后, 一个新的数据库条目将会输入到 SQL Server 2016 (Adfs Configv3) 并继续这些步骤之前，必须删除旧的配置数据库。**

**若要添加的 Windows Server 2016 AD FS 服务器以实现高可用性**

1.  在所需的 Windows Server 2016 部署上安装 AD RMS 角色。

2.  安装完成后，选择链接到**此服务器上配置联合身份验证服务**。

3.  在向导的欢迎使用部分中，选择选项**将联合身份验证服务器添加到联合服务器场**，然后单击**下一步**。

4.  指定正确的管理员帐户，然后单击**下一步**。

5.  上**指定场**页上，选择**指定使用 SQL Server 的现有场的数据库位置**数据库主机名的 SQL 服务输入 cname 记录，然后单击**下一步**.

6.  下**指定服务帐户**区域中的向导中，输入 AD FS 服务帐户的凭据，然后单击**下一步**。

7.  在中**审查选项**，单击**下一步**。

8.  单击**配置**按钮变为可用时。

9.  配置之后，重新启动计算机。

10. 后确认 server 安装程序的负载平衡所需的 AD FS 服务器。

#### <a name="adding-a-windows-server-2016-wap-server-for-high-availability"></a>添加 Windows Server 2016 WAP 服务器以实现高可用性

您可以部署附加的 WAP 服务器设置高可用性。 您可以选择执行此操作发生到 AD RMS 服务器的流量增加时。

**若要添加的 Windows Server 2016 Web 应用程序代理服务器以实现高可用性**

1.  在您希望安装程序作为 Web 应用程序代理服务器，导航到服务器管理器控制台，然后单击**添加角色和功能**。

2.  在中**添加角色和功能向导**，单击**下一步**直到你转到服务器角色选择屏幕。

3.  在选择服务器角色屏幕中，选择**远程访问**，然后单击**下一步**直到您在选择服务器角色屏幕。

4.  在选择服务器角色屏幕中，选择**Web 应用程序代理**，单击**添加功能**，然后单击**下一步**。

5.  在确认安装选择屏幕上，单击**安装**。

6.  完成安装后，单击**关闭**。

7.  现在就来配置服务器。 若要执行此操作，请打开 Web 应用程序代理服务器上的远程访问管理控制台。 打开**启动**菜单中，键入**RAMgmtUI.exe**，然后选择应用程序。

8.  在导航窗格中，单击**Web 应用程序代理**。

9.  在远程访问管理控制台中单击**运行 Web 应用程序代理配置向导**。 一次在向导中，单击**下一步**。

10. 在联合身份验证服务器屏幕上输入 （这在 AD FS 服务器的完全限定的域名 adfs.contoso.com) 和 AD FS 服务器上，然后输入为管理员凭据。

11. 在 AD FS 代理证书屏幕上，在 Web 应用程序代理服务器上当前安装的证书列表中选择要由 AD FS 代理，Web 应用程序代理的证书，然后单击**下一步**。

12. 在确认屏幕上，查看设置，然后单击**配置**。

13. 配置完成后，单击**关闭**。

14. 在确认服务器安装程序，负载均衡在外围网络中的 WAP 服务器。

#### <a name="adding-a-sql-server-2016-node-for-always-on-high-availability"></a>将 SQL Server 2016 节点添加为 Always On 高可用性

您可以部署附加的 SQL 服务器设置 Always On 高可用性。 您可以选择执行此操作发生到 AD RMS 服务器的流量增加时。 **注意： 请确保这两个 SQL Server 的入站的端口 5022 打开。**

**若要将 SQL server 2016 服务器添加为 Always On 高可用性**

1.  从服务器到另一台 SQL Server 2016 服务器的安装程序，导航到服务器管理器控制台，然后单击**添加角色和功能**。

2.  单击**下一步**直到**选择功能**对话框。

3.  选择**故障转移群集**复选框。 **注意： 此步骤是为也原始的 SQL server 2016 服务器，以便这两个 SQL Server 的故障转移群集功能。**

4.  单击**安装**安装故障转移群集功能。

5.  现在，打开**服务器管理器**，然后选择**工具**然后**故障转移群集管理器**。

6.  从左侧的菜单窗格中，右键单击**故障转移群集管理器**，然后选择**创建群集**

7.  这将打开**创建群集向导**。

8.  将用于 Always On 高可用性，然后单击在输入的 SQL server 2016 服务器浏览**下一步**。

9.  你将收到的验证警告。 选择**是**以验证群集节点，然后单击**下一步**。

10. 下**测试选项**页上，选择的选项**运行所有测试**单击**下一步。**

11. **注意：群集验证向导被应返回多条警告消息，尤其是当您将不使用共享的存储。除此之外，如果发现任何错误消息需要修复它们在创建 Windows Server 故障转移群集前**。

12. 在中**用于管理群集的访问点**对话框中，为 Windows Server 故障转移群集中，输入群集名称和虚拟 IP 地址，然后单击**下一步**。

13. 验证配置是否成功**摘要**然后单击**完成**。

14. 回到**故障转移群集管理器中，** 右键单击群集，然后选择**更多操作**然后选择**配置群集仲裁设置**

15. 单击**下一步**，然后选取的选项**选择仲裁见证**并点击**下一步**试。

16. 在中**选择仲裁见证**页上，选择**配置文件共享见证**选项。 然后，单击“下一步”  。

17. 选择**浏览**并找到你想要在文件共享路径对话框中使用的文件共享的路径。 单击“下一步”  。

18. 在确认页上单击**下一步**。

19. 在摘要页上单击**完成**。

20. 现在，打开**启动**菜单，然后搜索**SQL Server 配置管理器**。

21. 右键单击 SQL Server 名称，然后选取**属性**。

22. 在属性对话框中，选择**AlwaysOn 高可用性**选项卡。检查**启用 AlwaysOn 可用性组**复选框。 单击 **“确定”** 。 **注意： 执行此操作这两个 SQL server 2016 的服务器。**

23. 然后重新启动 SQL Server 服务。

24. 现在，打开**启动**菜单，然后搜索**SQL Server Management Studio** ，然后从左侧的导航窗格中，右键单击**可用性组**单击**新建可用性组向导**然后单击**下一步**。

25. 在中**指定可用性组名称**页选择的组名称 (Ex.SQLAvailabilityGroup2016)。 然后，单击“下一步”  。

26. 下**选择数据库**部分中，指定数据库。 然后，单击“下一步”。 **注意： 某些数据库可能需要再次备份或放入完全恢复模式**。

27. 上一次**指定副本**页上，单击**将副本添加**按钮，然后选择其他 2016 SQL Server。

28. 添加另一台服务器之后, 单击对应的复选框并设置为可读辅助副本的辅助服务器。

29. 导航到**终结点**选项卡，单击**刷新**选项。 同时还在这里，滚动，并确保主要和辅助节点上是相同的服务帐户。

30. 现在，选择**备份首选项**选项卡并选择**首选辅助**选项。

31. 转到**侦听器**选项卡。

32. 指定一个名称 （例如， SQLListener)，并确保端口**1433年**，然后单击**下一步**。

33. 在中**选择初始数据同步**页的向导中，选择**完整**选项并指定所有 SQL server 可访问的网络位置，然后单击**下一步**.

34. 最后，单击**完成**并且完成该过程。

### <a name="decommission-windows-server-2012-r2-nodes"></a>解除 Windows Server 2012 R2 节点的授权

以下各节提供有关操作的任务可能需要在成功升级到 Windows Server 2016 AD RMS 群集后删除你的 Windows Server 2012 R2 服务器的指导。

#### <a name="removing-a-windows-server-2012-r2-ad-rms-server"></a>删除 Windows Server 2012 R2 AD RMS 服务器

升级后，可以删除不必要的 AD RMS 服务器。 您可以选择将成为需要解除 AD RMS 服务器的授权时执行此操作。

**若要删除** **Windows Server 2012 R2 AD RMS 服务器**

1.  在 Windows Server 2012 R2 AD RMS 服务器在服务器管理器中，选择**管理**顶部右键菜单，然后选择**删除角色和功能**。

2.  **删除角色和功能向导**以及将打开**开始之前**屏幕上，单击**下一步**。

3.  上**服务器选择**屏幕上，单击**下一步**。

4.  上**服务器角色**屏幕上，清除该复选框旁边**Active Directory Rights Management Services**然后单击**下一步**。

5.  上**功能**屏幕上，单击**下一步**。

6.  上**确认**屏幕上，单击**删除**。

7.  完成此操作，重新启动服务器。

8.  现在可以关闭此服务器，并根据需要重新分配资源。
