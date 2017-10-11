---
title: "Monitorování a řešení ochrany pro virtuální počítače a fyzické servery | Microsoft Docs"
description: "Azure Site Recovery koordinuje replikaci, převzetí služeb při selhání a obnovení virtuálních počítačů, které jsou umístěné na místní servery Azure nebo do sekundárního datacentra. V tomto článku použijte ke sledování a řešení potíží s ochrana webu Virtual Machine Manager nebo technologie Hyper-V."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 0fc8e368-0c0e-4bb1-9d50-cffd5ad0853f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: 5bcb5dcb6afc3909e34dde31f845e014e7c539e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Monitorování a řešení ochrany pro virtuální počítače a fyzické servery
Tato příručka monitorování a řešení potíží se umožňuje zjistěte, jak sledovat stav replikace a řešení potíží s techniky Azure Site Recovery.

## <a name="understand-the-components"></a>Seznámení s komponentami
### <a name="vmware-virtual-machine-or-physical-server-site-deployment-for-replication-between-on-premises-and-azure"></a>Virtuální počítač VMware nebo fyzických serverů lokality nasazení pro replikaci mezi místními a Azure
Chcete-li nastavit obnovení databáze mezi místními VMware virtuálního počítače nebo fyzického serveru a Azure, nastavení konfigurace serveru, hlavní cílový server a součásti serveru proces na virtuální počítač nebo server. Pokud povolíte ochranu pro zdrojový server, nainstaluje Azure Site Recovery funkce Mobile Apps služby Microsoft Azure App Service. Po výpadku místní a po zdrojový server převezme do Azure, zákazníci potřeba nastavit procesní server v Azure a hlavní cílový server místně a znovu sestavte zdrojového serveru na místní.

![Nasazení VMware nebo fyzický lokality pro replikaci mezi místními a Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-sites"></a>Nasazení lokality nástroje Virtual Machine Manager pro replikaci mezi místními servery
Obnovení databáze mezi dvěma místními umístěními nastavit, musíte stáhnout zprostředkovatele Azure Site Recovery a nainstalujte ji na serveru nástroje Virtual Machine Manager. Zprostředkovatel musí připojení k Internetu k zajištění, že všechny operace, které se spouštějí z portálu Azure získat převedeny na místní operace.

![Nasazení lokality nástroje Virtual Machine Manager pro replikaci mezi místními servery](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Nasazení lokality nástroje Virtual Machine Manager pro replikaci mezi místními umístěními a Azure
Při nastavování obnovení databáze mezi místními umístěními a Azure, budete muset stáhnout zprostředkovatele Azure Site Recovery a nainstalujte ji na server nástroje Virtual Machine Manager. Také musíte nainstalovat agenta služeb zotavení Azure, které musí být nainstalovaný na každém hostiteli technologie Hyper-V. [Další informace](site-recovery-hyper-v-azure-architecture.md) Další informace.

![Nasazení lokality nástroje Virtual Machine Manager pro replikaci mezi místními umístěními a Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Nasazení technologie Hyper-V lokality pro replikaci mezi místními umístěními a Azure
Tento proces je podobný nasazení nástroje Virtual Machine Manager. Jediným rozdílem je, že zprostředkovatele Azure Site Recovery a agenta služeb zotavení Azure získat nainstalovaná v hostiteli technologie Hyper-V sám sebe. [Další informace](site-recovery-hyper-v-azure-architecture.md). .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Sledování operací konfigurace, ochrany a obnovení
Všechny operace v Azure Site Recovery je auditovat a sledovat pod **úlohy** kartě. Pro všechny konfigurace, ochrany nebo obnovení chyba, přejděte na **úlohy** kartě a vyhledejte chyby.

![Na kartě úlohy v filtru se nezdařilo](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Pokud narazíte na selhání v části **úlohy** kartě, klikněte na úlohu a klikněte na tlačítko **podrobnosti o CHYBĚ** pro tuto úlohu.

![Na tlačítko Podrobnosti o CHYBĚ](media/site-recovery-monitoring-and-troubleshooting/image4.png)

Podrobnosti o chybě vám pomůže identifikovat možná příčina a doporučení pro problém.

![Okno, které zobrazuje podrobnosti o chybě pro určité úlohy](media/site-recovery-monitoring-and-troubleshooting/image5.png)

V předchozím příkladu jiná operace, která je v průběhu zdá být příčinou selhání konfigurace ochrany. Vyřešte problém podle doporučení a pak klikněte na tlačítko **RESART** zahájíte operaci opakujte.

![Tlačítko RESTARTOVÁNÍ na kartě úlohy v](media/site-recovery-monitoring-and-troubleshooting/image6.png)

**RESTARTUJTE** možnost není k dispozici pro všechny operace. Pokud nemá operace **RESTARTUJTE** možnost, přejděte zpět na objekt a znovu proveďte operaci znovu. Můžete zrušit všechny úlohy, který je v průběhu pomocí **zrušit** tlačítko.

![Tlačítko Zrušit](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machines"></a>Monitorování stavu replikace pro virtuální počítače
Na portálu Azure můžete vzdáleně monitorovat zprostředkovatele Azure Site Recovery pro každý chráněný entit. Klikněte na tlačítko **chráněné položky**a potom klikněte na **CLOUDECH VMM** nebo **skupin ochrany**. **CLOUDECH VMM** karta je dostupná jenom pro nasazení, které jsou založeny na nástroje Virtual Machine Manager. Pro ostatní scénáře chráněné entity se v části **skupin ochrany** kartě.

![Možnosti Cloudech VMM a skupin ochrany](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Klikněte na tlačítko chráněná entita v rámci příslušného cloudu nebo skupiny ochrany, které chcete zobrazit všechny dostupné operace se zobrazují v dolním podokně.

![Dostupné možnosti pro vybrané chráněné entity](media/site-recovery-monitoring-and-troubleshooting/image9.png)

Jak ukazuje předchozí snímek obrazovky, stav virtuálního počítače je **kritický**. Můžete kliknout na **podrobnosti o CHYBĚ** tlačítko v dolní části zobrazíte následující chyba. Na základě **možné příčiny** a **doporučení**, vyřešte problém.

![Na tlačítko Podrobnosti o chybě](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Chyby a doporučení v dialogovém okně Podrobnosti o chybě](media/site-recovery-monitoring-and-troubleshooting/image11.png)

> [!NOTE]
> Pokud žádné aktivní operace probíhá nebo se nezdařilo, přejděte na **úlohy** zobrazit, jak je uvedeno výše zobrazíte chyby pro konkrétní úlohu.
>
>

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Řešení problémů místní technologie Hyper-V
Připojení k místní konzole Správce technologie Hyper-V, vyberte virtuální počítač a zobrazit stav replikace.

![Možnost zobrazit stav replikace v konzole Správce technologie Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image12.png)

V takovém případě **stav replikace** je **kritický**. Klikněte pravým tlačítkem na virtuální počítač a pak klikněte na **replikace** > **zobrazit stav replikace** a zobrazit podrobnosti.

![Stav replikace pro konkrétní virtuální počítač](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Pokud replikace se pozastavila pro virtuální počítač, klikněte pravým tlačítkem na virtuální počítač a pak klikněte na tlačítko **replikace** > **obnovit replikaci**.

![Možnost pokračovat v replikaci v konzole Správce technologie Hyper-V](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Pokud nové hostitele technologie Hyper-V, který je v rámci clusteru nebo na samostatný počítač migruje virtuální počítač a hostitele Hyper-V je nakonfigurovaný pomocí Azure Site Recovery, nebude to mít dopad replikace pro virtuální počítač. Zkontrolujte, zda splňuje všechny požadavky na nového hostitele technologie Hyper-V a je nakonfigurováno pomocí Azure Site Recovery.

### <a name="event-log"></a>V protokolu událostí
| Zdroje událostí | Podrobnosti |
| --- |:--- |
| **Aplikace a služby protokoly/Microsoft/VirtualMachineManager/Server/Admin** (serveru Virtual Machine Manager) |Poskytuje užitečné protokolování řešení problémů s mnoha různých nástroje Virtual Machine Manager. |
| **Aplikace a služby protokoly/MicrosoftAzureRecoveryServices/replikace** (hostitele Hyper-V) |Poskytuje užitečné protokolování mnoho problémů s agenta služeb zotavení Microsoft Azure. <br/> ![Umístění zdroje událostí replikace pro hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Aplikace a služby Logs/Microsoft/Azure lokality obnovení/poskytovatele/Operational** (hostitele Hyper-V) |Poskytuje užitečné protokolování mnoho problémů s službu Microsoft Azure Site Recovery. <br/> ![Umístění zdroje provozních událostí pro hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Aplikace a služby protokoly/Microsoft/Windows/Hyper-V-VMMS/Admin** (hostitele Hyper-V) |Poskytuje užitečné protokolování pro řešení potíží s mnoha potíží se správou virtuálních počítačů technologie Hyper-V. <br/> ![Umístění zdroje událostí nástroje Virtual Machine Manager pro hostitele Hyper-V](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |

### <a name="hyper-v-replication-logging-options"></a>Možnosti protokolování replikaci technologie Hyper-V
Všechny události, které se vztahují k replikaci technologie Hyper-V se protokolují v Hyper-V-VMMS\\správce protokol umístěný v části protokoly aplikací a služeb\\Microsoft\\Windows. Kromě toho můžete povolit protokol analytické pro služba Správa virtuálních počítačů technologie Hyper-V. Pokud chcete povolit tento protokol, nejprve zkontrolujte protokoly analýzy a ladění lze zobrazit v prohlížeči událostí. Otevřete Prohlížeč událostí a pak klikněte na **zobrazení** > **zobrazit protokoly pro ladění a**.

![Zobrazit protokoly pro ladění a možnost](media/site-recovery-monitoring-and-troubleshooting/image14.png)

Analytické protokolu je viditelný v **technologie Hyper-V-VMMS**.

![Analýzu protokolu události prohlížeč stromu](media/site-recovery-monitoring-and-troubleshooting/image15.png)

V **akce** podokně klikněte na tlačítko **povolit protokol**. Když je tato funkce povolená, zobrazí se v **sledování výkonu** jako **relaci trasování událostí** přidávaném **sady kolekcí dat.**

![Relace trasování událostí ve stromu monitorování výkonu](media/site-recovery-monitoring-and-troubleshooting/image16.png)

Chcete-li zobrazit shromážděné informace, nejprve zastavte trasování relace zakázáním v protokolu. Uložit protokol a znovu ho otevřete v prohlížeči událostí nebo použít jiné nástroje pro převod podle potřeby.

## <a name="reach-out-for-microsoft-support"></a>Oslovení pro systém Microsoft Support
### <a name="log-collection"></a>Shromáždění protokolů
Ochrana webu nástroje Virtual Machine Manager, najdete v části [shromáždění protokolů Azure Site Recovery pomocí nástroje podpory diagnostiky Platform (SDP)](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) ke shromažďování požadované protokoly.

Ochrana webu technologie Hyper-V, stáhněte si [nástroj](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) a spusťte jej na hostiteli technologie Hyper-V, aby shromažďovat protokoly.

Scénáře VMware nebo fyzický server, najdete v části [shromažďování protokolů Azure Site Recovery u VMware a fyzické lokalitě ochrany](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) ke shromažďování požadované protokoly.

Nástroj shromažďuje protokoly místně v náhodným názvem podsložky ve složce % LocalAppData%\ElevatedDiagnostics.

![Ukázkové postupy z ochrany lokality Hyper-V.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="open-a-support-ticket"></a>Otevřete lístek podpory
Vyvolat lístek podpory pro Azure Site Recovery, oslovení pro podporu Azure pomocí adresy URL v <http://aka.ms/getazuresupport>.

## <a name="kb-articles"></a>Články znalostní BÁZE
* [Zachování písmeno jednotky pro chráněné virtuální počítače, které jsou převzetí služeb při selhání nebo migrovat na Azure](http://support.microsoft.com/kb/3031135)
* [Jak spravovat místním nasazením a využití šířky pásma sítě pro ochranu Azure.](https://support.microsoft.com/kb/3056159)
* [Azure Site Recovery: Chyba "prostředek clusteru nebyl nalezen" při pokusu o povolení ochrany pro virtuální počítač](http://support.microsoft.com/kb/3010979)
* [Rady pro pochopení a Průvodce replikace řešení potíží s Hyper-V](http://social.technet.microsoft.com/wiki/contents/articles/21948.hyper-v-replica-troubleshooting-guide.aspx)

## <a name="common-azure-site-recovery-errors-and-their-resolutions"></a>Běžné chyby Azure Site Recovery a jejich řešení
Níže jsou uvedeny běžné chyby a jejich řešení. Jednotlivé chyby jsou uvedené na stránce samostatné wiki.

### <a name="general"></a>Obecné
* <span style="color:green;">NOVÉ</span> [úlohy selhání s chybou "operace probíhá." Chyba 505, 514, 532.](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
* <span style="color:green;">NOVÉ</span> [úlohy selhání s chybou "Server není připojený k Internetu". Došlo k chybě 25018.](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Nastavení
* [Server Virtual Machine Manager nelze zaregistrovat kvůli vnitřní chybě. Naleznete v zobrazení úlohy v portálu Site Recovery další podrobnosti o chybě. Spusťte instalaci znovu a registraci serveru.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
* [Nelze se připojit k úložišti Správce obnovení technologie Hyper-V. Ověřte nastavení proxy serveru a zkuste to znovu později.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfigurace
* [Vytvoření skupiny ochrany se nezdařilo: došlo k chybě při načítání seznamu serverů.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
* [Cluster hostitele technologie Hyper-V obsahuje minimálně jeden statický síťový adaptér nebo připojené adaptéry nejsou nakonfigurované k použití DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
* [Nástroj Virtual Machine Manager nemá oprávnění k dokončení akce.](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
* [Nelze vybrat účet úložiště v rámci předplatného. při konfiguraci ochrany.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Ochrana
* <span style="color:green;">NOVÉ</span> [povolení ochrany selhání s chybou "Nepodařilo se nakonfigurovat ochranu pro virtuální počítač". Chyba 60007, 40003.](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
* <span style="color:green;">NOVÉ</span> [povolení ochrany selhání s chybou "Nebylo možné povolit ochranu pro virtuální počítač." Došlo k chybě 70094.](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
* <span style="color:green;">NOVÉ</span> [migrace za provozu chyba 23848 - virtuální počítač bude se přesune pomocí typu živé. To by mohlo způsobit narušení stavu ochrany pro obnovení virtuálního počítače.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx)
* [Povolení ochrany se nezdařila, protože Agent není nainstalovaný na hostitelském počítači.](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
* [Nebyl nalezen vhodný hostitel pro virtuální počítač repliky - kvůli nízká výpočetní prostředky.](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
* [Vhodný hostitel pro virtuální počítač repliky nebyl nalezen - kvůli žádná logická síť připojen.](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
* [Nelze se připojit k počítači hostitele repliky – nelze navázat připojení.](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)

### <a name="recovery"></a>Obnovení
* Nástroj Virtual Machine Manager nemůže dokončit hostitelskou operaci:
  * [Převzetí služeb při selhání vybraný bod obnovení pro virtuální počítač: Obecná chyba odepření přístupu.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
  * [Převzetí služeb při selhání vybraný bod obnovení pro virtuální počítač technologie Hyper-V se nezdařilo: operace byla přerušena.  Zkuste nejnovější bod obnovení. (0x80004004).](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
  * Připojení se serverem nebylo možné zavedených (0x00002EFD).
    * [Povolení zpětné replikace pro virtuální počítač technologie Hyper-V se nezdařilo.](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
    * [Povolení replikace pro virtuální počítač virtuální počítač technologie Hyper-V se nezdařilo.](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
  * [Nelze předat převzetí služeb při selhání pro virtuální počítač.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
* [Plán obnovení obsahuje virtuální počítače, které nejsou připraveny pro plánované převzetí služeb při selhání.](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
* [Virtuální počítač není připraven pro plánované převzetí služeb při selhání.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* [Virtuální počítač není spuštěna a není vypnout.](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
* [Operace vzdálené proběhlo na virtuálním počítači a potvrzení převzetí služeb při selhání se nezdařilo.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* Testovací převzetí služeb při selhání
  * [Převzetí služeb při selhání nebylo možné zahájit, protože probíhá test převzetí služeb při selhání.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
* <span style="color:green;">NOVÉ</span> převzetí služeb při selhání časového limitu 'PreFailoverWorkflow úlohy WaitForScriptExecutionTaskTimeout' z důvodu nastavení konfigurace na skupinu zabezpečení sítě, který je přidružený k virtuálnímu počítači nebo podsíť, do které počítač patří. Odkazovat na ['PreFailoverWorkflow úloh WaitForScriptExecutionTaskTimeout'](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) podrobnosti.

### <a name="configuration-server-process-server-master-target"></a>Konfigurační server, server procesu, hlavní cíl
* [Hostiteli ESXi, který je hostitelem PS/CS jako virtuální počítač se nezdaří s fialové obrazovka smrti.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Řešení potíží s po převzetí služeb při selhání vzdálené plochy
* Mnoho zákazníků mít potýkají problémy se připojit k neúspěšný přes virtuální počítač v Azure. [Řešení potíží dokument pro připojení RDP do virtuálního počítače použít](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Přidání veřejnou IP adresu ve virtuálním počítači resource manager
Pokud **Connect** není k dispozici tlačítko na portálu a nejste připojení k Azure prostřednictvím připojení VPN Express Route nebo Site-to-Site, musíte vytvořit a přiřadit veřejnou IP adresu virtuálního počítače před použitím vzdálené plochy / Sdílené prostředí. Poté můžete přidat veřejnou IP adresu na síťové rozhraní virtuálního počítače.  

![Přidání veřejnou IP adresu na rozhraní sítě při selhání virtuálního počítače](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)
