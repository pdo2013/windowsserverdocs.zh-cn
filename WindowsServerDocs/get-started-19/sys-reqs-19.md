---
title: Windows Server 2019 系统要求
description: 存储、 CPU、 网络、 内存、 Windows Server 2019 的干净安装中的 RAM 的最低要求。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4a8b42d7-9fe5-4efe-9ea1-ace2131f860e
author: coreyp-at-msft
ms.author: coreyp
manager: jasgroce
ms.localizationpriority: medium
ms.openlocfilehash: 82a42cd219e41330fe4215124c21e799a41e412c
ms.sourcegitcommit: fcc26ec5a2cc73b59c5752377b39c070d288655e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2018
ms.locfileid: "8976653"
---
# 系统要求

>适用于：Windows Server 2019 

本主题概述了运行 Windows Server 的最低系统要求&reg;2019年。

## 查看操作系统要求  
以下是 Windows Server 2019 的估计的系统要求。 如果计算机未满足“最低”要求，将无法正确安装本产品。 实际要求将因系统配置和所安装应用程序及功能而异。

除非另有指定，否则最低系统要求适用于所有安装选项（服务器核心、带桌面体验的服务器和 Nano Server）以及标准版和数据中心版。  

> [!IMPORTANT]  
> 因为可能的部署方案多种多样，所以确定“推荐的”系统要求有些不切实际，只是大体适用而已。 请针对要部署的每个服务器角色查阅相关文档，以便获得特定服务器角色的资源需求的更多详细信息。 要获得最佳结果，请安排测试部署来确定特定部署方案的相应系统要求。  


## 处理器  
处理器性能不仅取决于处理器的时钟频率，还取决于处理器内核数以及处理器缓存大小。 以下是本产品对处理器的要求：  

**最低要求**：  
- 1.4 GHz 64 位处理器  
- 与 x64 指令集兼容  
- 支持 NX 和 DEP  
- 支持 CMPXCHG16b、LAHF/SAHF 和 PrefetchW  
- 支持二级地址转换（EPT 或 NPT）  

[Coreinfo](https://technet.microsoft.com/sysinternals/cc835722.aspx)是可用于确认 CPU 具有这些功能中的哪一个工具。

## RAM  
以下是本产品对 RAM 的预计要求：  

**最低要求**：  
- 512 MB（对于带桌面体验的服务器安装选项为 2 GB）
- ECC （纠错代码） 类型或类似的技术，对于物理主机部署

> [!IMPORTANT]  
> 如果你要使用支持的最低硬件参数（1 个处理器核心，512 MB RAM）创建一个虚拟机，然后尝试在该虚拟机上安装此版本，则安装将会失败。  
>   
> 为了避免这个问题，请执行下列操作之一：  
>   
> -   向要在其上安装此版本的虚拟机分配 800 MB 以上的 RAM。 在完成安装后，可以根据实际服务器配置更改 RAM 分配，最少分配量可为 512 MB。  
> -   使用 SHIFT+F10 中断此版本在虚拟机上的引导进程。 在打开的命令提示符下，使用 Diskpart.exe 创建并格式化一个安装分区。 运行 **Wpeutil createpagefile /path=C:\pf.sys** （假设创建的安装分区为 C:）。 关闭命令提示符并继续安装。  

## 存储控制器和磁盘空间要求  
运行 Windows Server 2019 的计算机必须包括符合 PCI Express 体系结构规范的存储适配器。 服务器上归类为硬盘驱动器的永久存储设备不能为 PATA。 Windows Server 2019 不允许 ATA/PATA/IDE/EIDE 用于启动、 页面或数据驱动器。  

以下是系统分区对磁盘空间的预计 **最低** 要求。  

**最低**：32 GB  

   > [!NOTE]  
    > 请注意，32 GB 应视为确保成功安装的*绝对最低*值。 满足此最低值应该允许你安装 Windows Server 2019 在服务器核心模式下，与 Web 服务 (IIS) 服务器角色。 “服务器核心”模式中的服务器比带有 GUI 模式的服务器中的相同服务器大约 4 GB。 
    >   
    > 系统分区在以下任何情形中将需要额外空间：  
    >   
    > -   如果通过网络安装系统。  
    > -   RAM 超过 16 GB 的计算机还需要为页面文件、休眠文件和转储文件分配额外磁盘空间。  

## 网络适配器要求  

与此版本一起使用的网络适配器应包含以下特征：  

**最低要求**：  
- 至少有千兆位吞吐量的以太网适配器。  
- 符合 PCI Express 体系结构规范。  

支持网络调试 (KDNet) 的网络适配器很有用，但不是最低要求。   

网络适配器支持预启动执行环境 (PXE) 很有用，但不是最低要求。



## 其他要求  
运行此版本的计算机还必须具有：  

-   DVD 驱动器（如果要从 DVD 媒体安装操作系统）  

以下并不是严格需要的，但某些特定功能需要：  

- 基于 UEFI 2.3.1c 的系统和支持安全启动的固件  
- 受信任的平台模块  

-   支持超级 VGA (1024 x 768) 或更高分辨率的图形设备和监视器  

-   键盘和 Microsoft&reg; 鼠标（或其他兼容的指针设备）  

-   Internet 访问（可能需要付费）  

>[!NOTE]  
> 尽管必须安装受信任的平台模块 (TPM) 芯片才能使用某些功能（如 BitLocker 驱动器加密），但它不是安装此版本的硬性要求。 如果你的计算机使用 TPM，则它必须满足以下要求：  
>  
>- 基于硬件的 TPM 必须实现的 TPM 规范的版本 2.0。  
>- 实现 2.0 版的 TPM 必须具有符合以下条件之一的 EK 证书：由硬件供应商预配到 TPM 或在首次启动期间能够由设备进行检索。  
>- 实现 2.0 版的 TPM 必须随附有 SHA 256 PCR 库并且对 SHA 256 实现 PCR 0 到 23。 可以接受将 TPM 随附可用于 SHA-1 和 SHA-256 度量值的单个可切换 PCR 库。  
>- 不要求用于关闭 TPM 的 UEFI 选项。  
