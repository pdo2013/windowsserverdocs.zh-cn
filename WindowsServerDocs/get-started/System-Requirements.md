---
title: 系统要求
description: 每个安装选项的干净安装中存储、CPU、网络、内存、RAM 的最低要求是什么。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 10/17/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8b42d7-9fe5-4efe-9ea1-ace2131fe068
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: d089af3562467aa1c222b17d9a1ad69d9c1b5008
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810586"
---
# <a name="system-requirements"></a>系统要求

>适用于：Windows Server（半年频道）、Windows Server 2016 

本主题介绍运行 Windows Server&reg; 2016 或 Windows Server 版本 1709 的最低系统要求。

> [!NOTE]  
> 在此版本中，推荐干净安装。  

> [!NOTE]  
> 如果安装时选择通过“服务器核心”选项进行安装，则应注意，没有安装任何 GUI 组件，并且将不能使用服务器管理器安装或卸载它们。 如果需要 GUI 功能，请务必在安装 Windows Server 2016 时，选择“带桌面体验的服务器”选项。 有关详细信息，请参阅[安装 Nano Server](Getting-Started-with-Nano-Server.md)  


## <a name="review-system-requirements"></a>查看操作系统要求  
以下是 Windows Server 2016 预计的系统要求。 如果计算机未满足“最低”要求，将无法正确安装本产品。 实际要求将因系统配置和所安装应用程序及功能而异。

除非另有指定，否则最低系统要求适用于所有安装选项（服务器核心、带桌面体验的服务器和 Nano Server）以及标准版和数据中心版。  

> [!IMPORTANT]  
> 因为可能的部署方案多种多样，所以确定“推荐的”系统要求有些不切实际，只是大体适用而已。 请针对要部署的每个服务器角色查阅相关文档，以便获得特定服务器角色的资源需求的更多详细信息。 要获得最佳结果，请安排测试部署来确定特定部署方案的相应系统要求。  


## <a name="processor"></a>处理器  
处理器性能不仅取决于处理器的时钟频率，还取决于处理器内核数以及处理器缓存大小。 以下是本产品对处理器的要求：  

**最低**：  
- 1.4 GHz 64 位处理器  
- 与 x64 指令集兼容  
- 支持 NX 和 DEP  
- 支持 CMPXCHG16b、LAHF/SAHF 和 PrefetchW  
- 支持二级地址转换（EPT 或 NPT）  

[Coreinfo](https://technet.microsoft.com/sysinternals/cc835722.aspx) 是可用于确认 CPU 具有这些功能中的哪种功能的工具。

## <a name="ram"></a>RAM  
以下是本产品对 RAM 的预计要求：  

**最低**：  
- 512 MB（对于带桌面体验的服务器安装选项为 2 GB）
- ECC（纠错代码）类型或类似技术  

> [!IMPORTANT]  
> 如果你要使用支持的最低硬件参数（1 个处理器核心，512 MB RAM）创建一个虚拟机，然后尝试在该虚拟机上安装此版本，则安装将会失败。  
>   
> 为了避免这个问题，请执行下列操作之一：  
>   
> -   向要在其上安装此版本的虚拟机分配 800 MB 以上的 RAM。 在完成安装后，可以根据实际服务器配置更改 RAM 分配，最少分配量可为 512 MB。  
> -   使用 SHIFT+F10 中断此版本在虚拟机上的引导进程。 在打开的命令提示符下，使用 Diskpart.exe 创建并格式化一个安装分区。 运行 **Wpeutil createpagefile /path=C:\pf.sys** （假设创建的安装分区为 C:）。 关闭命令提示符并继续安装。  

## <a name="storage-controller-and-disk-space-requirements"></a>存储控制器和磁盘空间要求  
运行 Windows Server 2016 的计算机必须包括符合 PCI Express 体系结构规范的存储适配器。 服务器上归类为硬盘驱动器的永久存储设备不能为 PATA。 Windows Server 2016 不允许将 ATA/PATA/IDE/EIDE 用于启动驱动器、页面驱动器或数据驱动器。  

以下是系统分区对磁盘空间的预计 **最低** 要求。  

**最低**：32 GB  

> [!NOTE]
> 请注意，32 GB 应视为确保成功安装的*绝对最低*值。 满足此最低值应该能够以“服务器核心”模式安装包含 Web 服务 (IIS) 服务器角色的 Windows Server 2016。 “服务器核心”模式中的服务器比带有 GUI 模式的服务器中的相同服务器大约 4 GB。 
> 
> 系统分区在以下任何情形中将需要额外空间：  
> 
> -   如果通过网络安装系统。  
> -   RAM 超过 16 GB 的计算机还需要为页面文件、休眠文件和转储文件分配额外磁盘空间。  

## <a name="network-adapter-requirements"></a>网络适配器要求  

与此版本一起使用的网络适配器应包含以下特征：  

**最低要求**：  
- 至少有千兆位吞吐量的以太网适配器。  
- 符合 PCI Express 体系结构规范。  
- 支持预启动执行环境 (PXE)。  

支持网络调试 (KDNet) 的网络适配器很有用，但不是最低要求。   

## <a name="other-requirements"></a>其他要求  
运行此版本的计算机还必须具有：  


-   DVD 驱动器（如果要从 DVD 媒体安装操作系统）  

以下并不是严格需要的，但某些特定功能需要：  

- 基于 UEFI 2.3.1c 的系统和支持安全启动的固件  
- 受信任的平台模块  

-   支持超级 VGA (1024 x 768) 或更高分辨率的图形设备和监视器  

-   键盘和 Microsoft&reg; 鼠标（或其他兼容的指针设备）  

-   Internet 访问（可能需要付费）  

> [!NOTE]  
> 尽管必须安装受信任的平台模块 (TPM) 芯片才能使用某些功能（如 BitLocker 驱动器加密），但它不是安装此版本的硬性要求。 如果你的计算机使用 TPM，则它必须满足以下要求：  
>  
> - 基于硬件的 TPM 必须实现的 TPM 规范的版本 2.0。  
> - 实现 2.0 版的 TPM 必须具有符合以下条件之一的 EK 证书：由硬件供应商预配到 TPM 或在首次启动期间能够由设备进行检索。  
> - 实现 2.0 版的 TPM 必须随附有 SHA 256 PCR 库并且对 SHA 256 实现 PCR 0 到 23。 可以将 TPM 与单个可切换 PCR 库一起寄送，后者可用于 SHA-1 和 SHA-256 度量。  
> - 不要求用于关闭 TPM 的 UEFI 选项。  

## <a name="installation-of-nano-server"></a>Nano Server 安装  
有关将 Windows Server 2016 安装为 Nano Server 的详细步骤，请参阅[安装 Nano Server](Getting-Started-with-Nano-Server.md)。

## <a name="additional-resources"></a>其他资源
- [Windows 处理器要求](https://docs.microsoft.com/windows-hardware/design/minimum/windows-processor-requirements)
- [Windows Server 2016 Standard 和 Datacenter 版本的比较](https://docs.microsoft.com/windows-server/get-started/2016-edition-comparison)
- [Windows 10 系统要求](https://www.microsoft.com/windows/windows-10-specifications#system-specifications)
- [下载 Windows Server 2016 许可数据表](http://download.microsoft.com/download/7/2/9/7290EA05-DC56-4BED-9400-138C5701F174/WS2016LicensingDatasheet.pdf)
