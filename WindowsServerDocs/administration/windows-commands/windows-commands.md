---
title: Windows 命令
description: Windows 命令
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c703d07c-8227-4e86-94a6-8ef390f94cdc
author: jasongerend
ms.author: jgerend
manager: dongill
ms.date: 06/26/2019
ms.prod: windows-server
ms.openlocfilehash: 5cb26bcff99d9cf3a1ee8b3a937ad6098a913c3d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362064"
---
# <a name="windows-commands"></a>Windows 命令

所有受支持的 Windows 版本（服务器和客户端）都具有内置的一组 Win32 控制台命令。

此文档集介绍了可用于通过脚本或脚本工具自动执行任务的 Windows 命令。

若要查找有关特定命令的信息，请在下面的 A-z 菜单中，单击命令开头的字母，然后单击命令名称。

[@NO__T-](#a)1[B](#b) | 
[C](#c) | 
[D](#d) | 
[E](#e) | 
[F](#f) |
1[G](#g)3[H](#h)5[I](#i)7[J](#j)9[K](#k)1[L](#l)3[M](#m)5[N](#n)7[O](#o)9[P](#p)1[Q](#q)3[R](#r)5[S](#s)@NO__T[-37 @NO__T](#t)-39[U](#u)@NO__T[-41 3](#v)[W](#w)5[X](#x) |Y |Z

## <a name="prerequisites"></a>先决条件

本主题中包含的信息适用于：

-   Windows Server 2019
-   Windows Server（半年频道）
-   Windows Server 2016
-   Windows Server 2012 R2
-   Windows Server 2012 
-   Windows Server 2008 R2
-   Windows Server 2008
-   Windows 10
-   Windows 8.1

### <a name="command-shell-overview"></a>命令外壳概述

命令行界面是 Windows 中内置的第一个 shell，用批处理（.bat）文件自动完成日常任务，如用户帐户管理或夜间备份。 借助 Windows 脚本宿主，你可以在命令行界面中运行更复杂的脚本。 有关详细信息，请参阅[cscript](cscript.md)或[wscript.echo](wscript.md)。 使用脚本可以更有效地执行操作，而不是使用用户界面。 脚本接受命令行中可用的所有命令。

Windows 有两个命令 shell：命令行界面和[PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6)。 每个 shell 是一种软件程序，它提供你与操作系统或应用程序之间的直接通信，同时提供用于自动执行 IT 操作的环境。

PowerShell 旨在扩展命令行界面的功能，以运行称为 cmdlet 的 PowerShell 命令。 Cmdlet 类似于 Windows 命令，但提供更可扩展的脚本语言。 可以在 Powershell 中运行 Windows 命令和 PowerShell cmdlet，但命令 shell 只能运行 Windows 命令，而不能运行 PowerShell cmdlet。

对于最新的 Windows automation 最新功能，建议使用 PowerShell，而不是 Windows 命令或 windows 脚本宿主来实现 Windows automation。 
> [!NOTE]
>你还可以下载和安装 powershell [Core](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6)，即 powershell 的开源版本。 

> [!CAUTION]
> 不正确地编辑注册表可能会对系统造成严重损坏。 在对注册表进行以下更改之前，应备份计算机上任何有价值的数据。

> [!NOTE]
> 若要在计算机或用户登录会话上的命令行界面中启用或禁用文件和目录名完成，请运行**regedit.exe**并设置以下**reg_DWOrd 值**：
> 
> HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\completionChar\reg_DWOrd
> 
> 若要设置**reg_DWOrd**值，请将控制字符的十六进制值用于特定函数（例如， **0 9**为 Tab， **0 08**为 Backspace）。 用户指定的设置优先于计算机设置，命令行选项优先于注册表设置。

## <a name="command-line-reference-a-z"></a>命令行参考 a-z

若要查找有关特定 Windows 命令的信息，请在以下 a-z 菜单中，单击该命令的开头字母，然后单击命令名称。

[@NO__T-](#a)1[B](#b) | 
[C](#c) | 
[D](#d) | 
[E](#e) | 
[F](#f) |
1[G](#g)3[H](#h)5[I](#i)7[J](#j)9[K](#k)1[L](#l)3[M](#m)5[N](#n)7[O](#o)9[P](#p)1[Q](#q)3[R](#r)5[S](#s)@NO__T[-37 @NO__T](#t)-39[U](#u)@NO__T[-41 3](#v)[W](#w)5[X](#x) |Y |Z

### <a name="a"></a>A
-   [append](append.md)
-   [arp](arp.md)
-   [assoc](assoc.md)
-   [at](at.md)
-   [atmadm](atmadm.md)
-   [attrib](attrib.md)
-   [auditpol](auditpol.md)
-   [autochk](autochk.md)
-   [autoconv](autoconv.md)
-   [autofmt](autofmt.md)

### <a name="b"></a>B
- [bcdboot](bcdboot.md)
- [bcdedit](bcdedit.md)
- [bdehdcfg](bdehdcfg.md)
- [bitsadmin](bitsadmin.md)
  -   [bitsadmin addfile](bitsadmin-addfile.md)
  -   [bitsadmin addfileset](bitsadmin-addfileset.md)
  -   [bitsadmin addfilewithranges](bitsadmin-addfilewithranges.md)
  -   [bitsadmin cancel](bitsadmin-cancel.md)
  -   [bitsadmin complete](bitsadmin-complete.md)
  -   [bitsadmin create](bitsadmin-create.md)
  -   [bitsadmin getaclflags](bitsadmin-getaclflags.md)
  -   [bitsadmin getbytestotal](bitsadmin-getbytestotal.md)
  -   [bitsadmin getbytestransferred](bitsadmin-getbytestransferred.md)
  -   [bitsadmin getcompletiontime](bitsadmin-getcompletiontime.md)
  -   [bitsadmin getcreationtime](bitsadmin-getcreationtime.md)
  -   [bitsadmin getdescription](bitsadmin-getdescription.md)
  -   [bitsadmin getdisplayname](bitsadmin-getdisplayname.md)
  -   [bitsadmin geterror](bitsadmin-geterror.md)
  -   [bitsadmin geterrorcount](bitsadmin-geterrorcount.md)
  -   [bitsadmin getfilestotal](bitsadmin-getfilestotal.md)
  -   [bitsadmin getfilestransferred](bitsadmin-getfilestransferred.md)
  -   [bitsadmin getminretrydelay](bitsadmin-getminretrydelay.md)
  -   [bitsadmin getmodificationtime](bitsadmin-getmodificationtime.md)
  -   [bitsadmin getnoprogresstimeout](bitsadmin-getnoprogresstimeout.md)
  -   [bitsadmin getnotifycmdline](bitsadmin-getnotifycmdline.md)
  -   [bitsadmin getnotifyflags](bitsadmin-getnotifyflags.md)
  -   [bitsadmin getnotifyinterface](bitsadmin-getnotifyinterface.md)
  -   [bitsadmin getowner](bitsadmin-getowner.md)
  -   [bitsadmin get priority](bitsadmin-getpriority.md)
  -   [bitsadmin getproxybypasslist](bitsadmin-getproxybypasslist.md)
  -   [bitsadmin getproxylist](bitsadmin-getproxylist.md)
  -   [bitsadmin getproxyusage](bitsadmin-getproxyusage.md)
  -   [bitsadmin getreplydata](bitsadmin-getreplydata.md)
  -   [bitsadmin getreplyfilename](bitsadmin-getreplyfilename.md)
  -   [bitsadmin getreplyprogress](bitsadmin-getreplyprogress.md)
  -   [bitsadmin getstate](bitsadmin-getstate.md)
  -   [bitsadmin gettype](bitsadmin-gettype.md)
  -   [bitsadmin help](bitsadmin-help.md)
  -   [bitsadmin info](bitsadmin-info.md)
  -   [bitsadmin list](bitsadmin-list.md)
  -   [bitsadmin listfiles](bitsadmin-listfiles.md)
  -   [bitsadmin monitor](bitsadmin-monitor.md)
  -   [bitsadmin nowrap](bitsadmin-nowrap.md)
  -   [bitsadmin rawreturn](bitsadmin-rawreturn.md)
  -   [bitsadmin removecredentials](bitsadmin-removecredentials.md)
  -   [bitsadmin replaceremoteprefix](bitsadmin-replaceremoteprefix.md)
  -   [bitsadmin reset](bitsadmin-reset.md)
  -   [bitsadmin resume](bitsadmin-resume.md)
  -   [bitsadmin setaclflag](bitsadmin-setaclflag.md)
  -   [bitsadmin setcredentials](bitsadmin-setcredentials.md)
  -   [bitsadmin setdescription](bitsadmin-setdescription.md)
  -   [bitsadmin setdisplayname](bitsadmin-setdisplayname.md)
  -   [bitsadmin setminretrydelay](bitsadmin-setminretrydelay.md)
  -   [bitsadmin setnoprogresstimeout](bitsadmin-setnoprogresstimeout.md)
  -   [bitsadmin setnotifycmdline](bitsadmin-setnotifycmdline.md)
  -   [bitsadmin setnotifyflags](bitsadmin-setnotifyflags.md)
  -   [bitsadmin setpriority](bitsadmin-setpriority.md)
  -   [bitsadmin setproxysettings](bitsadmin-setproxysettings.md)
  -   [bitsadmin setreplyfilename](bitsadmin-setreplyfilename.md)
  -   [bitsadmin suspend](bitsadmin-suspend.md)
  -   [bitsadmin takeownership](bitsadmin-takeownership.md)
  -   [bitsadmin 传输](bitsadmin-transfer.md)
  -   [bitsadmin util](bitsadmin-util.md)
  -   [bitsadmin wrap](bitsadmin-wrap.md)
- [bootcfg](bootcfg.md)
  -   [bootcfg addsw](bootcfg-addsw.md)
  -   [bootcfg copy](bootcfg-copy.md)
  -   [bootcfg dbg1394](bootcfg-dbg1394.md)
  -   [bootcfg debug](bootcfg-debug.md)  
  -   [bootcfg default](bootcfg-default.md)
  -   [bootcfg delete](bootcfg-delete.md)
  -   [bootcfg ems](bootcfg-ems.md)
  -   [bootcfg query](bootcfg-query.md)
  -   [bootcfg raw](bootcfg-raw.md)
  -   [bootcfg rmsw](bootcfg-rmsw.md)
  -   [bootcfg timeout](bootcfg-timeout.md)
- [break](break_1.md)

### <a name="c"></a>C
- [cacls](cacls_1.md)
- [call](call.md)
- [cd](cd.md)
- [certreq](certreq_1.md)
- [certutil](certutil.md)
- [change](change.md)
  -   [change logon](change-logon.md)
  -   [change port](change-port.md)
  -   [change user](change-user.md)
- [chcp](chcp.md)
- [chdir](chdir_1.md)
- [chglogon](chglogon.md)
- [chgport](chgport.md)
- [chgusr](chgusr.md)
- [chkdsk](chkdsk.md)
- [chkntfs](chkntfs.md)
- [choice](choice.md)
- [cipher](cipher.md)
- [cleanmgr](cleanmgr.md)
- [clip](clip.md)
- [cls](cls.md)
- [Cmd](Cmd.md)
- [cmdkey](cmdkey.md)
- [cmstp](cmstp.md)
- [color](color.md)
- [comp](comp.md)
- [compact](compact.md)
- [convert](convert.md)
- [copy](copy.md)
- [cprofile](cprofile.md)
- [cscript](cscript.md)

### <a name="d"></a>D
-   [date](date.md)
-   [dcgpofix](dcgpofix.md)
-   [defrag](defrag.md)
-   [del](del.md)
-   [dfsrmig](dfsrmig.md)
-   [diantz](diantz.md)
-   [dir](dir.md)
-   [diskcomp](diskcomp.md)
-   [diskcopy](diskcopy.md)
-   [diskpart](diskpart.md)
-   [diskperf](diskperf.md)
-   [diskraid](diskraid.md)
-   [diskshadow](diskshadow.md)
-   [dispdiag](dispdiag.md)
-   [dnscmd](Dnscmd.md)
-   [doskey](doskey.md)
-   [driverquery](driverquery.md)

### <a name="e"></a>E
-   [echo](echo.md)
-   [edit](edit.md)
-   [endlocal](endlocal.md)
-   [erase](erase.md)
-   [eventcreate](eventcreate.md)
-   [eventquery](eventquery.md)
-   [eventtriggers](eventtriggers.md)
-   [evntcmd](Evntcmd.md)
-   [exit](exit_2.md)
-   [expand](expand.md)
-   [extract](extract.md)

### <a name="f"></a>F
- [fc](fc.md)
- [find](find.md)
- [findstr](findstr.md)
- [finger](finger.md)
- [flattemp](flattemp.md)
- [fondue](fondue.md)
- [for](for.md)
- [forfiles](forfiles.md)
- [format](format.md)
- [freedisk](freedisk.md)
- [fsutil](fsutil.md)
  -   [fsutil 8dot3name](fsutil-8dot3name.md) 
  -   [fsutil behavior](fsutil-behavior.md) 
  -   [fsutil file](fsutil-file.md)
  -   [fsutil fsinfo](fsutil-fsinfo.md)
  -   [fsutil hardlink](fsutil-hardlink.md)
  -   [fsutil objectid](fsutil-objectid.md)
  -   [fsutil quota](fsutil-quota.md)
  -   [fsutil repair](fsutil-repair.md)
  -   [fsutil reparsepoint](fsutil-reparsepoint.md)
  -   [fsutil resource](fsutil-resource.md)
  -   [fsutil sparse](fsutil-sparse.md)
  -   [fsutil tiering](fsutil-tiering.md)
  -   [fsutil transaction](fsutil-transaction.md)
  -   [fsutil usn](fsutil-usn.md)
  -   [fsutil volume](fsutil-volume.md)
  -   [fsutil wim](fsutil-wim.md)
- [ftp](ftp.md)
- [ftype](ftype.md)
- [fveupdate](fveupdate.md)

### <a name="g"></a>G
-   [getmac](getmac.md)
-   [gettype](gettype.md)
-   [goto](goto.md)
-   [gpfixup](gpfixup.md)
-   [gpresult](gpresult.md)
-   [gpupdate](gpupdate.md)
-   [graftabl](graftabl.md)

### <a name="h"></a>H
-   [help](help.md)
-   [helpctr](helpctr.md)
-   [hostname](hostname.md)

### <a name="i"></a>I
-   [icacls](icacls.md)
-   [if](if.md)
-   [inuse](inuse.md)
-   [ipconfig](ipconfig.md)
-   [ipxroute](ipxroute.md)
-   [irftp](irftp.md)

### <a name="j"></a>J
-   [jetpack](jetpack.md)

### <a name="k"></a>K
- [klist](klist.md)
- [ksetup](ksetup.md)
  -   [ksetup： setrealm](ksetup-setrealm.md)
  -   [ksetup： mapuser](ksetup-mapuser.md)
  -   [ksetup： addkdc](ksetup-addkdc.md)
  -   [ksetup： delkdc](ksetup-delkdc.md)
  -   [ksetup： addkpasswd](ksetup-addkpasswd.md)
  -   [ksetup： delkpasswd](ksetup-delkpasswd.md)
  -   [ksetup：服务器](ksetup-server.md)
  -   [ksetup： setcomputerpassword](ksetup-setcomputerpassword.md)
  -   [ksetup： removerealm](ksetup-removerealm.md)
  -   [ksetup：域](ksetup-domain.md)
  -   [ksetup： changepassword](ksetup-changepassword.md)
  -   [ksetup： listrealmflags](ksetup-listrealmflags.md)
  -   [ksetup： setrealmflags](ksetup-setrealmflags.md)
  -   [ksetup： addrealmflags](ksetup-addrealmflags.md)
  -   [ksetup： delrealmflags](ksetup-delrealmflags.md)
  -   [ksetup： dumpstate](ksetup-dumpstate.md)
  -   [ksetup： addhosttorealmmap](ksetup-addhosttorealmmap.md)
  -   [ksetup： delhosttorealmmap](ksetup-delhosttorealmmap.md)
  -   [ksetup： setenctypeattr](ksetup-setenctypeattr.md)
  -   [ksetup： getenctypeattr](ksetup-getenctypeattr.md)
  -   [ksetup： addenctypeattr](ksetup-addenctypeattr.md)
  -   [ksetup： delenctypeattr](ksetup-delenctypeattr.md) 
- [ktmutil](ktmutil.md)
- [ktpass](ktpass.md)

### <a name="l"></a>L
- [label](label.md)
- [lodctr](lodctr.md)
- [logman](logman.md)
  -   [logman create](logman-create.md)
  -   [logman query](logman-query.md)
  -   [logman start & 124;stop](logman-start-stop.md)
  -   [logman delete](logman-delete.md)
  -   [logman update](logman-update.md)
  -   [logman import & 124;先导](logman-import-export.md)
- [logoff](logoff.md)
- [lpq](lpq.md)
- [lpr](lpr.md)

### <a name="m"></a>M
- [macfile](macfile.md)
- [makecab](makecab.md)
- [manage-bde](manage-bde.md)
  -   [manage-bde：状态](manage-bde-status.md)
  -   [manage-bde：开启](manage-bde-on.md)
  -   [manage-bde： off](manage-bde-off.md)
  -   [manage-bde： pause](manage-bde-pause.md)
  -   [manage-bde： resume](manage-bde-resume.md)
  -   [manage-bde： lock](manage-bde-lock.md)
  -   [manage-bde：解除锁定](manage-bde-unlock.md)
  -   [manage-bde： autounlock](manage-bde-autounlock.md)
  -   [manage-bde：保护程序](manage-bde-protectors.md)
  -   [manage-bde： tpm](manage-bde-tpm.md)
  -   [manage-bde： setidentifier](manage-bde-setidentifier.md)
  -   [manage-manage-bde：ForceRecovery](manage-bde-forcerecovery.md)
  -   [manage-bde： changepassword](manage-bde-changepassword.md)
  -   [manage-bde： changepin](manage-bde-changepin.md)
  -   [manage-bde： changekey](manage-bde-changekey.md)
  -   [manage-manage-bde：KeyPackage](manage-bde-keypackage.md)
  -   [manage-bde：升级](manage-bde-upgrade.md)
  -   [manage-manage-bde：WipeFreeSpace](manage-bde-wipefreespace.md)
- [mapadmin](mapadmin.md)
- [Md](Md.md)
- [mkdir](mkdir.md)
- [mklink](mklink.md)
- [mmc](mmc.md)
- [mode](mode.md)
- [more](more.md)
- [mount](mount.md)
- [mountvol](mountvol.md)
- [move](move.md)
- [mqbkup](mqbkup.md)
- [mqsvc](mqsvc.md)
- [mqtgsvc](mqtgsvc.md)
- [msdt](msdt.md)
- [msg](msg.md)
- [msiexec](msiexec.md)
- [msinfo32](msinfo32.md)
- [mstsc](mstsc.md)

### <a name="n"></a>N
- [nbtstat](nbtstat.md)
- [netcfg](netcfg.md)
- [netsh](netsh.md)
- [netstat](netstat.md)
- [Net print](net-print.md)
- [nfsadmin](nfsadmin.md)
- [nfsshare](nfsshare.md)
- [nfsstat](nfsstat.md)
- [nlbmgr](nlbmgr.md)
- [nslookup](nslookup.md)
  -   [nslookup exit 命令](nslookup-exit-command.md)
  -   [nslookup finger 命令](nslookup-finger-command.md)
  -   [nslookup help](nslookup-help.md)
  -   [nslookup ls](nslookup-ls.md)
  -   [nslookup lserver](nslookup-lserver.md)
  -   [nslookup root](nslookup-root.md)
  -   [nslookup server](nslookup-server.md)
  -   [nslookup set](nslookup-set.md)
  -   [nslookup set all](nslookup-set-all.md)
  -   [nslookup set class](nslookup-set-class.md)
  -   [nslookup set d2](nslookup-set-d2.md)
  -   [nslookup set debug](nslookup-set-debug.md)
  -   [nslookup set domain](nslookup-set-domain.md)
  -   [nslookup set port](nslookup-set-port.md)
  -   [nslookup set querytype](nslookup-set-querytype.md)
  -   [nslookup set recurse](nslookup-set-recurse.md)
  -   [nslookup set retry](nslookup-set-retry.md)
  -   [nslookup set root](nslookup-set-root.md)
  -   [nslookup set search](nslookup-set-search.md)
  -   [nslookup set srchlist](nslookup-set-srchlist.md)
  -   [nslookup set timeout](nslookup-set-timeout.md)
  -   [nslookup set type](nslookup-set-type.md)
  -   [nslookup set vc](nslookup-set-vc.md)
  -   [nslookup view](nslookup-view.md)
- [ntbackup](ntbackup.md)
- [ntcmdprompt](ntcmdprompt.md)
- [ntfrsutl](ntfrsutl.md)

### <a name="o"></a>O
-   [openfiles](openfiles.md)

### <a name="p"></a>P
-   [pagefileconfig](pagefileconfig.md)
-   [path](path.md)
-   [pathping](pathping.md)
-   [pause](pause.md)
-   [pbadmin](pbadmin.md)
-   [pentnt](pentnt.md)
-   [perfmon](perfmon.md)
-   [ping](ping.md)
-   [pnpunattend](pnpunattend.md)
-   [pnputil](pnputil.md)
-   [popd](popd.md)
-   [PowerShell](PowerShell.md)
-   [PowerShell_ise](PowerShell_ise.md)
-   [print](print.md)
-   [prncnfg](prncnfg.md)
-   [prndrvr](prndrvr.md)
-   [prnjobs](prnjobs.md)
-   [prnmngr](prnmngr.md)
-   [prnport](prnport.md)
-   [prnqctl](prnqctl.md)
-   [prompt](prompt.md)
-   [pubprn](pubprn.md)
-   [pushd](pushd.md)
-   [pushprinterconnections](pushprinterconnections.md)

### <a name="q"></a>Q
-   [qappsrv](qappsrv.md)
-   [qprocess](qprocess.md)
-   [查询](query.md)
-   [quser](quser.md)
-   [qwinsta](qwinsta.md)

### <a name="r"></a>R
- [rcp](rcp.md)
- [rd](rd.md)
- [rdpsign](rdpsign.md)
- [recover](recover.md)
- [reg](reg.md)
  -   [reg 添加](reg-add.md)
  -   [注册表比较](reg-compare.md)
  -   [注册副本](reg-copy.md)
  -   [注册删除](reg-delete.md)
  -   [reg export](reg-export.md)
  -   [注册导入](reg-import.md)
  -   [reg 负载](reg-load.md)
  -   [reg 查询](reg-query.md)
  -   [reg restore](reg-restore.md)
  -   [注册保存](reg-save.md)
  -   [注册卸载](reg-unload.md)
- [regini](regini.md)
- [regsvr32](regsvr32.md)
- [relog](relog.md)
- [rem](rem.md)
- [ren](ren.md)
- [rename](rename.md)
- [repair-bde](repair-bde.md)
- [replace](replace.md)
- [reset session](reset-session.md)
- [rexec](rexec.md)
- [risetup](risetup.md)
- [rmdir](rmdir.md)
- [robocopy](robocopy.md)
- [route_ws2008](route_ws2008.md)
- [rpcinfo](rpcinfo.md)
- [rpcping](rpcping.md)
- [rsh](rsh.md)
- [rundll32](rundll32.md)
- [rwinsta](rwinsta.md)

### <a name="s"></a>S
- [schtasks](schtasks.md)
- [scwcmd](Scwcmd.md)
  -   [scwcmd：分析](scwcmd-analyze.md)
  -   [scwcmd：配置](scwcmd-configure.md)
  -   [scwcmd：注册](scwcmd-register.md) 
  -   [scwcmd： rollback](scwcmd-rollback.md) 
  -   [scwcmd：转换](scwcmd-transform.md) 
  -   [scwcmd： view](scwcmd-view.md) 
- [secedit](secedit.md)
  -   [secedit：分析](secedit-analyze.md)
  -   [secedit：配置](secedit-configure.md)
  -   [secedit： export](secedit-export.md)
  -   [secedit： generaterollback](secedit-generaterollback.md)
  -   [secedit：导入](secedit-import.md)
  -   [secedit：验证](secedit-validate.md)
- [serverceipoptin](serverceipoptin.md)
- [Servermanagercmd](Servermanagercmd.md)
- [serverweroptin](serverweroptin.md)
- [set](set_1.md)
- [setlocal](setlocal.md)
- [setx](setx.md)
- [sfc](sfc.md)
- [shadow](shadow.md)
- [shift](shift.md)
- [showmount](showmount.md)
- [shutdown](shutdown.md)
- [sort](sort.md)
- [start](start.md)
- [subst](subst.md)
- [sxstrace](sxstrace.md)
- [sysocmgr](sysocmgr.md)
- [systeminfo](systeminfo.md)

### <a name="t"></a>T
-   [takeown](takeown.md)
-   [tapicfg](tapicfg.md)
-   [taskkill](taskkill.md)
-   [tasklist](tasklist.md)
-   [tcmsetup](tcmsetup.md)
-   [telnet](telnet.md)
-   [tftp](tftp.md)
-   [time](time.md)
-   [timeout](timeout_1.md)
-   [title](title_1.md)
-   [tlntadmn](tlntadmn.md)
-   [tpmvscmgr](tpmvscmgr.md)
-   [tracerpt](tracerpt_1.md)
-   [tracert](tracert.md)
-   [tree](tree.md)
-   [tscon](tscon.md)
-   [tsdiscon](tsdiscon.md)
-   [tsecimp](tsecimp_1.md)
-   [tskill](tskill.md)
-   [tsprof](tsprof.md)
-   [type](type.md)
-   [typeperf](typeperf.md)
-   [tzutil](tzutil.md)

### <a name="u"></a>U
-   [unlodctr](unlodctr_1.md)

### <a name="v"></a>V
-   [ver](ver.md)
-   [verifier](verifier.md)
-   [verify](verify_1.md)
-   [vol](vol.md)
-   [vssadmin](vssadmin.md)- 

### <a name="w"></a>W
- [waitfor](waitfor.md)
- [wbadmin](wbadmin.md)
  -   [wbadmin 启用备份](wbadmin-enable-backup.md)
  -   [wbadmin 禁用备份](wbadmin-disable-backup.md)
  -   [wbadmin 开始备份](wbadmin-start-backup.md)
  -   [wbadmin 停止作业](wbadmin-stop-job.md)
  -   [wbadmin get 版本](wbadmin-get-versions.md)
  -   [wbadmin 获取项](wbadmin-get-items.md)
  -   [wbadmin 开始恢复](wbadmin-start-recovery.md)
  -   [wbadmin 获取状态](wbadmin-get-status.md)
  -   [wbadmin 获取磁盘](wbadmin-get-disks.md)
  -   [wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)
  -   [wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)
  -   [wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)
  -   [wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)
  -   [wbadmin restore catalog](wbadmin-restore-catalog.md)
  -   [wbadmin 删除目录](wbadmin-delete-catalog.md)
- [wdsutil](wdsutil.md)
- [wecutil](wecutil.md)
- [wevtutil](wevtutil.md)
- [where](where_1.md)
- [whoami](whoami.md)
- [winnt](winnt.md)
- [winnt32](winnt32.md)
- [winpop](winpop.md)
- [winrs](winrs.md)
- [wlbs.exe](wlbs_1.md)
- [wmic](wmic.md)
- [wscript](wscript.md)

### <a name="x"></a>X
-   [xcopy](xcopy.md)
