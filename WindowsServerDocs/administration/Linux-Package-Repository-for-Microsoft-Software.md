---
title: 适用于 Microsoft 产品的 Linux 软件存储库
description: 本文档介绍了如何使用和安装适用于 Microsoft 产品的 Linux 软件包。
ms.custom: na
ms.prod: windows-server-threshold
ms.service: na
manager: szark
ms.technology: compute
ms.topic: article
ms.assetid: b5387444-595f-4f38-abb7-163a70ea1895
author: szarkos
ms.author: szark
ms.date: 10/16/2017
ms.openlocfilehash: bade9fff306272188ac8d2b91a3d9921c80fe036
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866891"
---
# <a name="linux-software-repository-for-microsoft-products"></a>适用于 Microsoft 产品的 Linux 软件存储库

## <a name="overview"></a>概述
Microsoft 构建并支持适用于 Linux 系统的各种软件产品，并使它们可通过标准 APT 和 YUM 程序包存储库提供。 本文档介绍如何在 Linux 系统上配置存储库，以便可以使用分发的标准包管理工具来安装/升级 Microsoft 的 Linux 软件。

Microsoft 的 Linux 软件存储库由多个子存储库组成：

 - 生产–为要在生产中使用的包指定生产子存储库。 根据 Microsoft 的适用支持协议或计划，Microsoft 对这些包进行商业支持。

 - mssql-服务器-这些存储库包含 Linux 上的 Microsoft SQL Server 的包-另请参阅：[Linux 上的 SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux)。

> [!Note]
> Linux 软件存储库中的包受包中的许可条款的约束。 使用包之前, 请阅读许可条款。 安装和使用此包即表示你接受这些条款。 如果不同意许可条款, 请不要使用包。


## <a name="configuring-the-repositories"></a>配置存储库
可以通过安装适用于 Linux 分发版和版本的 Linux 包自动配置存储库。 此包将安装存储库配置以及工具（如 apt/yum/zypper）使用的 GPG 公钥来验证已签名的包和/或存储库元数据。

### <a name="enterprise-linux-rhel-and-variants"></a>企业 Linux （RHEL 和变体）

 - Enterprise Linux 6 （64.RPM）

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 （EL7）

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14.04 （t）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/14.04/prod
        sudo apt-get update

 - Ubuntu 16.04 （Xenial）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod
        sudo apt-get update

 - Ubuntu 18.04 （Bionic）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
        sudo apt-get update

 - Ubuntu 18.10 （宇宙射线）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.10/prod
        sudo apt-get update

 - Ubuntu 19.04 （Disco）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/19.04/prod
        sudo apt-get update

### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>手动配置
存储库配置文件可从[packages.microsoft.com/config](https://packages.microsoft.com/config/)获取。可以使用以下 URI 命名约定来查找这些文件的名称和位置：

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**包和存储库签名密钥**

 - 可在此处下载 Microsoft 的 GPG 公钥：[https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - 公钥 ID：Microsoft （版本签名）<gpgsecurity@microsoft.com>
 - 公钥指纹：`BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>例如：

 - RHEL/CentOS 7

        # Install repository configuration
        curl https://packages.microsoft.com/config/rhel/7/prod.repo > ./microsoft-prod.repo
        sudo cp ./microsoft-prod.repo /etc/yum.repos.d/

        # Install Microsoft's GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc > ./microsoft.asc
        sudo rpm --import ./microsoft.asc

 - Ubuntu 16.04

        # Install repository configuration
        curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > ./microsoft-prod.list
        sudo cp ./microsoft-prod.list /etc/apt/sources.list.d/

        # Install Microsoft GPG public key
        curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
        sudo cp ./microsoft.gpg /etc/apt/trusted.gpg.d/



