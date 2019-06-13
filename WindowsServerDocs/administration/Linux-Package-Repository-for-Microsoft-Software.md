---
title: Linux 软件存储库的 Microsoft 产品
description: 本文档介绍如何使用和安装 Linux 的 Microsoft 产品的软件程序包。
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
ms.openlocfilehash: 77b309739125a2114ef4ada4adb305f4dd169b06
ms.sourcegitcommit: 927adf32faa6052234ad08f21125906362e593dc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/12/2019
ms.locfileid: "67033322"
---
# <a name="linux-software-repository-for-microsoft-products"></a>Linux 软件存储库的 Microsoft 产品

## <a name="overview"></a>概述
Microsoft 生成和支持的 Linux 系统的各种软件产品，使其可通过标准 APT 和 YUM 包存储库。 本文档介绍如何在 Linux 系统上，配置存储库，以便你可以然后安装/升级使用分发的标准包管理工具的 Microsoft Linux 软件。

Microsoft 的 Linux 软件存储库包含多个的子存储库：

 - prod-子存储库，指定应在生产中使用的包的生产环境。 您与 Microsoft 的程序的适当的支持协议的条款中，Microsoft 商业上都支持这些包。

 - mssql-server-这些存储库包含包的 Linux-请参阅上的 Microsoft SQL Server 还：[Linux 上的 SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-vnext-including-Linux)。

> [!Note]
> Linux 软件存储库中的包位于包的许可条款。 请阅读许可条款才能使用包。 安装和使用包即表明您接受这些条款。 如果你不同意许可条款，请勿使用包。


## <a name="configuring-the-repositories"></a>配置存储库
可以通过安装适用于 Linux 分发版和版本的 Linux 包自动配置存储库。 包将安装工具，如 apt/yum/zypper 用于验证签名的包和/或元数据存储库 GPG 公钥以及存储库配置。

### <a name="enterprise-linux-rhel-and-variants"></a>Enterprise Linux （RHEL 和变体）

 - Enterprise Linux 6 (EL6)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/6/packages-microsoft-prod.rpm

 - Enterprise Linux 7 (EL7)

        sudo rpm -Uvh https://packages.microsoft.com/config/rhel/7/packages-microsoft-prod.rpm


### <a name="ubuntu"></a>Ubuntu

 - Ubuntu 14.04 （可信赖）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/14.04/prod
        sudo apt-get update

 - Ubuntu 16.04 (Xenial)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/16.04/prod
        sudo apt-get update

 - Ubuntu 18.04 (Bionic)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
        sudo apt-get update

 - Ubuntu 18.10 （宇宙）

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.10/prod
        sudo apt-get update

 - Ubuntu 19.04 (Disco)

        curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
        sudo apt-add-repository https://packages.microsoft.com/ubuntu/19.04/prod
        sudo apt-get update

### <a name="suse-linux-enterprise-12"></a>SUSE Linux Enterprise 12

        sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm


## <a name="manual-configuration"></a>手动配置
存储库配置文件中有[packages.microsoft.com/config](https://packages.microsoft.com/config/)。名称和这些文件的位置可以位于使用以下 URI 命名约定：

        https://packages.microsoft.com/config/<Distribution>/<Version>/prod.(repo|list)

**包和存储库中签名密钥**

 - Microsoft 的 GPG 公共密钥可能会在此处下载： [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
 - 公共密钥 ID:Microsoft （版本签名） <gpgsecurity@microsoft.com>
 - 公共密钥指纹： `BC52 8686 B50D 79E3 39D3 721C EB3E 94AD BE12 29CF`

### <a name="examples"></a>示例：

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



