---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: 故障转移群集
ms.prod: windows-server-threshold
layout: LandingPage
ms.topic: landing-page
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 03/08/2019
ms.localizationpriority: high
ms.openlocfilehash: 445de065ff5b68b83481ee5bd83ebf18fdd180a7
ms.sourcegitcommit: b0fece76b871da3fa9d6a996798a5008756f486b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2019
ms.locfileid: "9178598"
---
# Windows Server 中的故障转移群集

> 适用范围： Windows Server 2019、Windows Server 2016、Windows Server（半年频道）

>[!TIP]
> 需要有关较旧版本的 Windows Server 的信息？ 在 docs.microsoft.com 上查看我们的其他 [Windows Server 库](/previous-versions/windows/)。 你还可以[搜索此网站](https://docs.microsoft.com/search/index?search=Windows+Server&dataSource=previousVersions)查找特定信息。

<hr />

故障转移群集是一组独立的计算机，这些计算机相互协作以提高群集角色（之前称为应用程序和服务）的可用性和可伸缩性。 群集服务器（称为节点）通过物理电缆和软件连接。 如果一个或多个群集节点出现故障，其他节点就会开始提供服务（该过程称为故障转移）。 此外，会主动监视群集角色，以验证它们是否正常工作。 如果不工作，则会重启这些角色或将其移动到其他节点。

故障转移群集还提供群集共享卷 (CSV) 功能，该功能提供一致的分布式命名空间，群集角色可以使用这样的命名空间，从所有的节点访问共享存储。 借助故障转移群集功能，用户将会在服务中体验到最低程度的中断。

故障转移群集有许多实际应用，包括：
* 用于如 Microsoft SQL Server 和 Hyper-V 虚拟机等应用程序的高度可用或持续可用文件共享存储
* 在物理服务器或虚拟机（安装在运行 Hyper-V 的服务器上）上运行的高度可用群集角色

<hr />

<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-whats-new.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h2><a href="whats-new-in-failover-clustering.md">故障转移群集中的新增功能</a></h2>
                                        </div>
                                    </div>
                                </div>
                             </div>
                          </a>
                        </li>
                     </ul>
<HR />

<ul class="cardsF panelContent">

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>了解</h3>
<HR />
                                        <p><a href="sofs-overview.md">应用程序数据的横向扩展文件服务器</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/understand-quorum.md">群集和池仲裁</a></p>
<HR />
                                        <p><a href="fault-domains.md">故障域感知</a></p>
<HR />
                                        <p><a href="smb-multichannel.md">简化 SMB 多通道和多 NIC 群集网络</a></p>
<HR />
                                        <p><a href="vm-load-balancing-overview.md">VM 负载均衡</a></p>
<HR />
                                        <p><a href="../storage/storage-spaces/cluster-sets.md">群集集</a></p>
<HR />
                                        <p><a href="cluster-affinity.md">群集相关性</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>

<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>计划</h3>
<HR />
                                        <p><a href="clustering-requirements.md">故障转移群集硬件要求和存储选项</a></p>
<HR />
                                        <p><a href="failover-cluster-csvs.md">使用群集共享卷 (CSV)</a></p>               
<HR />
                                        <p><a href="../storage/storage-spaces/storage-spaces-direct-in-vm.md">使用带存储空间直通的来宾虚拟机群集</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>部署</a></h3> 
<HR />
                                        <p><a href="prestage-cluster-adds.md">在 Active Directory 域服务中预留群集计算机对象</a></p>
<HR />
                                        <p><a href="create-failover-cluster.md">创建故障转移群集</a></p> 
<HR />
                                        <p><a href="deploy-two-node-clustered-file-server.md">部署双节点文件服务器</a></p> 
<HR />
                                        <p><a href="manage-cluster-quorum.md">管理仲裁和见证</a></p> 
<HR />
                                        <p><a href="deploy-cloud-witness.md">部署云见证</a></p>
<HR />
                                        <p><a href="file-share-witness.md">部署文件共享见证</a></p>
<HR />
                                        <p><a href="cluster-operating-system-rolling-upgrade.md">群集操作系统滚动升级</a></p> 
<HR />
                                        <p><a href="upgrade-option-same-hardware.md">升级在同一硬件上的故障转移群集</a></p>
<HR />
                                        <p><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\)">部署与 Active Directory 分离的群集</a></p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
                     </ul>
<HR />
<ul class="cardsF panelContent">
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>管理</h3>
<HR />
                                        <p><a href="cluster-aware-updating.md">群集感知更新</a></p> 
<HR />
                                        <p><a href="health-service-overview.md">运行状况服务</a></p>
<HR />
                                        <p><a href="cluster-domain-migration.md">群集域迁移</a></p>
<HR />
                                        <p><a href="troubleshooting-using-wer-reports.md">使用 Windows 错误报告进行疑难解答</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>工具和设置</a></h3>
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps">故障转移群集 PowerShell Cmdlet</a></p> 
<HR />
                                        <p><a href="https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps">群集感知更新 PowerShell Cmdlet</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
<li>
                         <div class="cardSize">
                                <div class="cardPadding">
                                    <div class="card">
                                        <div class="cardImageOuter">
                                            <div class="cardImage">
                                                <img src="../media/i-cluster.svg" alt="" />
                                            </div>
                                        </div>
                                        <div class="cardText">
                                        <h3>社区资源</a></h3>
<HR />
                                        <p><a href="https://go.microsoft.com/fwlink/p/?LinkId=230641">高可用性（群集）论坛</a></p> 
<HR />
                                        <p><a href="http://blogs.msdn.com/b/clustering/">故障转移群集和网络负载均衡团队博客</a></p> 
                                        </div>
                                    </div>
                                </div>
                            </div>
                          </a>
                        </li>
</ul>
