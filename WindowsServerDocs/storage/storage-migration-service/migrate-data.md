---
title: 使用存储迁移服务迁移文件服务器
description: 搜索引擎结果的主题的简短说明
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 879a87d4383d07e37227ecf6fffa5d002a77bbc0
ms.sourcegitcommit: 214e827934e7b3e8987e9e0ab2cf00047d332c89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153318"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>使用存储迁移服务的服务器迁移

本主题讨论如何将服务器，包括其文件和配置为使用另一台服务器迁移[存储迁移服务](overview.md)和 Windows Admin Center。 一旦安装了服务并打开任何必要的防火墙端口迁移需要以下三步： 清点您的服务器、 传输的数据，以及转换到新服务器。

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>步骤 0:安装存储迁移服务，并检查防火墙端口

在开始之前，安装存储迁移服务，并确保所需的防火墙端口已打开。

1. 检查[存储迁移服务要求](overview.md#requirements)并安装[Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md)在您的 PC 或如果你尚未准备好的管理服务器上。
2. 在 Windows Admin Center 连接到运行 Windows Server 2019 的业务流程协调程序服务器。 <br>这是你将在上安装存储迁移服务和用于管理迁移的服务器。 如果要迁移只有一台服务器，可以使用目标服务器，只要它运行 Windows Server 2019。 我们建议你使用单独的业务流程服务器的任何多服务器迁移。
1. 转到**服务器管理器**（在 Windows Admin Center) >**存储迁移服务**，然后选择**安装**安装存储迁移服务和其所需的组件（图 1 中显示）。
    ![显示安装按钮的存储迁移服务页面的屏幕截图](media/migrate/install.png)**图 1:安装存储迁移服务**
1. 在运行 Windows Server 2019 的所有目标服务器上安装的存储迁移服务代理。 这两倍时安装在目标服务器上的传输速度。 <br>要执行此操作，连接到 Windows Admin Center 中的目标服务器，然后转到**服务器管理器**（在 Windows Admin Center) >**角色和功能**，选择**存储迁移服务代理**，然后选择**安装**。
1. 对所有源服务器和 Windows Admin Center，在运行 Windows Server 2012 R2 或 Windows Server 2016 中，任何目标服务器上连接到每个服务器，请转到**服务器管理器**（在 Windows Admin Center) >**防火墙**  > **传入规则**，然后检查是否启用了以下规则：
    - 文件和打印机共享 (SMB-In)
    - Netlogon 服务 (Np-in)
    - Windows 管理规范 (Dcom-in)
    - Windows Management Instrumentation (WMI-In)

   > [!NOTE]
   > 如果使用的第三方防火墙，若要打开的入站的端口范围是 TCP/445 (SMB)、 TCP/135 （RPC/DCOM 终结点映射程序，） 和 TCP 1025 到 65535 之间 （RPC/DCOM 临时端口）。 存储迁移服务端口为 TCP/28940 （业务流程协调程序） 和 TCP/28941 （代理）。

1. 如果您正在使用的业务流程协调程序服务器管理迁移，并且你想要下载事件或数据传输的日志，检查，以及该服务器上启用文件和打印机共享 (Smb-in) 防火墙规则。

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>第 1 步：创建作业和清点您的服务器以找出要迁移的内容

在此步骤中，指定要迁移，然后扫描它们收集信息对其文件和配置什么服务器。

1. 选择**新的作业**，此作业命名，然后选择**确定**。
1. 上**输入凭据**页上，键入管理员凭据，你想要迁移，在服务器上工作，然后选择**下一步**。
1. 选择**添加设备**，键入源服务器的名称，并选择**确定**。 <br>要列出清单的任何其他服务器重复此过程。
1. 选择**开始扫描**。<br>向后的扫描就完成会显示在页面更新。
    ![显示服务器准备好进行扫描的屏幕截图](media/migrate/inventory.png)**图 2:清点服务器**
1. 选择要查看共享、 配置、 网络适配器和卷已列出清单的每个服务器。 <br><br>存储迁移服务不会传输文件或文件夹，我们知道可能会影响 Windows 运行，因此在此版本中，您会看到位于 Windows 系统文件夹中的任何共享的警告。 您必须在传输阶段期间跳过这些共享。 有关详细信息，请参阅[哪些文件和文件夹从传输中排除](faq.md#excluded-files)。
1. 选择**下一步**转到传输数据。

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>步骤 2：将数据从旧服务器传输到目标服务器

在此步骤中指定目标服务器上放置的位置之后传输数据。

1. 上**将数据传输** > **输入凭据**页上，键入你想要迁移到目标服务器上工作，然后选择的管理员凭据**下一步**.
2. 上**将目标设备和映射添加**页上，第一个源服务器列出。 键入你想要迁移，然后选择的服务器的名称**扫描设备**。
3. 清除源卷映射到目标卷**Include**您不希望任何共享对应的复选框以传输 （包括任何的管理共享 Windows 系统文件夹中），然后选择**下一步**.
   ![显示源服务器和其卷和共享的屏幕截图，其中它们将传输到目标上](media/migrate/transfer.png)**图 3:源服务器和其存储到要从中传输**
4. 添加目标服务器和任何更多的源服务器的映射，然后选择**下一步**。
5. 选择性地调整传输设置，并选择**下一步**。
6. 选择**Validate** ，然后选择**下一步**。
7. 选择**开始传输**来开始传输数据。<br>第一次传输，我们将到备份文件夹在目标继续任何现有文件。 在后续传输，默认情况下我们将刷新目标而不备份第一个。 <br>此外，存储迁移服务是智能，它能够处理重叠共享-我们不会将相同的文件夹复制两次在同一个作业。
8. 在传输完成后，请查看目标服务器以确保所有内容正确传输。 选择**错误日志仅**如果你想要下载的未传输任何文件的日志。

   > [!NOTE]
   > 如果想要保留传输的审核线索或计划作业中执行多个传输，请单击**传输日志**保存 CSV 副本。 每个后续传输将覆盖以前运行的数据库信息。 

此时，您有三个选项：

- **转到下一步**，以便目标服务器采用的源服务器的标识转移剪切。
- **请考虑在迁移完成**而无需使对源服务器的身份。
- **重新传输**，复制自最后一个传输以来已更新的文件。

如果你的目标是要使用 Azure 文件同步，你可以设置使用 Azure 文件同步服务器与目标服务器后传输文件，或者在转移到目标服务器的领域 (请参阅[规划 Azure 文件同步部署](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning))。

## <a name="step-3-optionally-cut-over-to-the-new-servers"></a>步骤 3:根据需要转换到新服务器

在此步骤中您转换来自源服务器到目标服务器将 IP 地址和计算机名称移动到目标服务器。 完成此步骤后，应用和用户访问通过名称和从迁移的服务器地址的新服务器。

 1. 如果您已经通过导航离开迁移作业，在 Windows Admin Center，请转到**服务器管理器** > **存储迁移服务**，然后选择你想要完成的作业。 
 1. 上**转换到新服务器** > **输入凭据**页上，选择**下一步**来使用先前键入的凭据。
 1. 上**配置直接转换**页上，指定的网络适配器，以高于每个源设备的设置。 此 IP 地址将从源系统移到目标直接转换迁移的一部分。
 1. 指定要用于源服务器后直接转换移动到目标其地址哪些 IP 地址。 可以使用 DHCP 或静态地址。 如果使用静态地址，新的子网必须是相同的旧子网或直接转换将失败。
    ![显示源服务器及其 IP 地址和计算机名称和内容的屏幕截图它们将替换为后直接转换迁移](media/migrate/cutover.png)
    **图 4:源服务器和其网络配置移动到目标的方式**
 1. 指定如何在目标服务器取代其名称后，重命名源服务器。 可以使用随机生成的名称，也可以键入自己。 然后选择**下一步**。
 1. 选择**下一步**上**调整直接转换设置**页。
 1. 选择**Validate**上**验证源和目标设备**页，，然后选择**下一步**。
 1. 如果你已准备好执行直接转换迁移，选择**启动直接转换**。 <br>用户和应用可能会遇到中断时移动的地址和名称和服务器重新启动几次每个，但将是不受迁移影响。 多长时间直接转换花费的时间取决于如何快速服务器重新启动，以及 Active Directory 和 DNS 复制时间。

## <a name="see-also"></a>请参阅

- [存储迁移服务概述](overview.md)
- [存储迁移服务方面的常见问题 (FAQ)](faq.md)
- [规划 Azure 文件同步部署](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)
