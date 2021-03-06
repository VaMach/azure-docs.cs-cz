---
title: "Řešení potíží s Azure Backup selhání: hosta stavu agenta není k dispozici | Microsoft Docs"
description: "Příznaky, příčiny a řešení Azure Backup chyby související s agenta, rozšíření a disky."
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
keywords: "Zálohování Azure; Agent virtuálního počítače; Připojení k síti;"
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 01/09/2018
ms.author: genli;markgal;sogup;
ms.openlocfilehash: c205023b025a477ee05ddcbfc536573f31426167
ms.sourcegitcommit: e19742f674fcce0fd1b732e70679e444c7dfa729
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/01/2018
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Řešení potíží s Azure Backup selhání: problémy s agenta nebo rozšíření

Tento článek obsahuje kroky řešení potíží, které vám mohou pomoci vyřešit Azure Backup chyby související s komunikaci s agenta virtuálního počítače a rozšíření.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="vm-agent-unable-to-communicate-with-azure-backup"></a>Agent virtuálního počítače nemůže komunikovat s Azure Backup

Chybová zpráva: "Nelze navázat komunikaci s Azure Backup Agent virtuálního počítače"

> [!NOTE]
> Pokud váš virtuální počítač Azure s Linuxem zálohování se nezdaří s touto chybou od 4 ledna 2018, spusťte následující příkaz ve virtuálním počítači a poté opakujte zálohování:`sudo rm -f /var/lib/waagent/*.[0-9]*.xml`

Po registraci a naplánovat virtuálního počítače pro službu zálohování, zálohování spustí úlohu komunikaci s agentem virtuálních počítačů k pořízení snímku v daném okamžiku. Snímek některý z následujících podmínek může zabránit se aktivuje. Při aktivaci snímku není zálohování může selhat. Proveďte následující kroky odstraňování potíží v uvedeném pořadí a poté operaci:

**Příčina 1: [virtuální počítač nemá přístup k Internetu](#the-vm-has-no-internet-access)**  
**Příčina 2: [je agent nainstalován ve virtuálním počítači, ale jeho reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Příčina 3: [agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Příčina 4: [nelze načíst stav snímku ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**Příčina 5: [rozšíření zálohování se nezdaří aktualizace nebo zatížení](#the-backup-extension-fails-to-update-or-load)**  

## <a name="snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>Operace snímku se nezdaří, protože virtuální počítač není připojený k síti

Chybová zpráva: "Snímku operace se nezdařila z důvodu žádné připojení k síti na virtuálním počítači"

Po registraci a naplánovat virtuálního počítače pro službu Azure zálohování, zálohování spustí úlohu komunikaci s rozšíření zálohování virtuálních počítačů k pořízení snímku v daném okamžiku. Snímek některý z následujících podmínek může zabránit se aktivuje. Pokud není aktivované snímku, může dojít k selhání zálohování. Proveďte následující kroky odstraňování potíží v uvedeném pořadí a poté operaci:    
**Příčina 1: [virtuální počítač nemá přístup k Internetu](#the-vm-has-no-internet-access)**  
**Příčina 2: [nelze načíst stav snímku ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Příčina 3: [rozšíření zálohování se nezdaří aktualizace nebo zatížení](#the-backup-extension-fails-to-update-or-load)**  

## <a name="vmsnapshot-extension-operation-failed"></a>Dojde k selhání operace rozšíření VMSnapshot

Chybová zpráva: "VMSnapshot rozšíření operace se nezdařilo"

Po registraci a naplánovat virtuálního počítače pro službu Azure zálohování, zálohování spustí úlohu komunikaci s rozšíření zálohování virtuálních počítačů k pořízení snímku v daném okamžiku. Snímek některý z následujících podmínek může zabránit se aktivuje. Pokud není aktivované snímku, může dojít k selhání zálohování. Proveďte následující kroky odstraňování potíží v uvedeném pořadí a poté operaci:  
**Příčina 1: [nelze načíst stav snímku ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Příčina 2: [rozšíření zálohování se nezdaří aktualizace nebo zatížení](#the-backup-extension-fails-to-update-or-load)**  
**Příčina 3: [virtuální počítač nemá přístup k Internetu](#the-vm-has-no-internet-access)**  
**Příčina 4: [je agent nainstalován ve virtuálním počítači, ale jeho reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Příčina 5: [agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backup-fails-because-the-vm-agent-is-unresponsive"></a>Zálohování se nezdaří, protože agent virtuálního počítače je reagovat

Chyba messagae: "Nelze provést operaci, protože agenta virtuálního počítače není přizpůsobivý"

Po registraci a naplánovat virtuálního počítače pro službu Azure zálohování, zálohování spustí úlohu komunikaci s rozšíření zálohování virtuálních počítačů k pořízení snímku v daném okamžiku. Snímek některý z následujících podmínek může zabránit se aktivuje. Pokud není aktivované snímku, může dojít k selhání zálohování. Proveďte následující kroky odstraňování potíží v uvedeném pořadí a poté operaci:  
**Příčina 1: [je agent nainstalován ve virtuálním počítači, ale jeho reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Příčina 2: [agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Příčina 3: [virtuální počítač nemá přístup k Internetu](#the-vm-has-no-internet-access)**  

## <a name="backup-fails-with-an-internal-error"></a>Zálohování se nezdaří, k interní chybě

Chybová zpráva: "zálohování došlo k vnitřní chybě - opakujte operaci za několik minut."

Po registraci a naplánovat virtuálního počítače pro službu Azure zálohování, zálohování spustí úlohu komunikaci s rozšíření zálohování virtuálních počítačů k pořízení snímku v daném okamžiku. Snímek některý z následujících podmínek může zabránit se aktivuje. Pokud není aktivované snímku, může dojít k selhání zálohování. Proveďte následující kroky odstraňování potíží v uvedeném pořadí a poté operaci:  
**Příčina 1: [virtuální počítač nemá přístup k Internetu](#the-vm-has-no-internet-access)**  
**Příčina 2: [agent nainstalovaný v virtuálního počítače, ale je reagovat (pro virtuální počítače Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Příčina 3: [agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Příčina 4: [nelze načíst stav snímku ani snímku nelze provést.](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Příčina 5: [rozšíření zálohování se nezdaří aktualizace nebo zatížení](#the-backup-extension-fails-to-update-or-load)**  
**Příčina 6: [zálohování služby nemá oprávnění k odstranění staré body obnovení z důvodu zámek skupiny prostředků](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

## <a name="disk-configuration-is-not-supported"></a>Konfigurace disku nejsou podporovány

Chybová zpráva: "zadané konfigurace disku nejsou podporovány."

> [!NOTE]
> Máme privátní Preview verzi pro podporu zálohování pro virtuální počítače, které mají disky, které jsou větší než 1 TB. Podrobnosti najdete v tématu [privátní Preview verzi pro podporu zálohování virtuálních počítačů velké diskové](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a).
>
>

Zálohování Azure v současné době nepodporuje disky, které jsou [větší než 1,023 GB](https://docs.microsoft.com/azure/backup/backup-azure-arm-vms-prepare#limitations-when-backing-up-and-restoring-a-vm). Pokud máte disky, které jsou větší než 1 TB:  
1. [Připojte nový disky](https://docs.microsoft.com/azure/virtual-machines/windows/attach-managed-disk-portal) , které jsou menší než 1 TB.  
2. Zkopírování dat z disků, které jsou větší než 1 TB na nově vytvořený disky, které jsou menší než 1 TB.  
3. Ujistěte se, že byl zkopírován všechna data. Potom odeberte disky, které jsou větší než 1 TB.  
4. Zahajte zálohování.

## <a name="causes-and-solutions"></a>Příčiny a řešení

### <a name="the-vm-has-no-internet-access"></a>Virtuální počítač nemá přístup k Internetu
Virtuální počítač na požadavcích nasazení nemá přístup k Internetu. Nebo může mít omezení, která umožňují přístup na infrastrukturu Azure.

Správné fungování rozšíření Backup vyžaduje připojení k veřejným IP adresám Azure. Rozšíření odešle příkazy do úložiště Azure koncový bod (adresa URL protokolu HTTP) ke správě snímky virtuálního počítače. Pokud rozšíření nemá přístup do veřejného Internetu, zálohování se nakonec nezdaří.

####  <a name="solution"></a>Řešení
Chcete-li vyřešit tento problém, zkuste jeden z následujících metod:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Povolit přístup k úložišti Azure, která odpovídá oblasti

Můžete použít [služby značky](../virtual-network/security-overview.md#service-tags) umožňující připojení k úložišti určité oblasti. Zkontrolujte, zda pravidlo, které umožňuje přístup k účtu úložiště má vyšší prioritu než pravidlo tento přístup k Internetu bloky. 

![Skupina zabezpečení sítě se značky úložiště pro oblast](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Značky služby úložiště jsou ve verzi preview. Jsou k dispozici pouze v určitých oblastí. Seznam oblastí naleznete v tématu [služby značky pro úložiště](../virtual-network/security-overview.md#service-tags).

##### <a name="create-a-path-for-http-traffic"></a>Vytvoření cesty pro provoz protokolu HTTP

1. Pokud máte omezení síťového na místě (například skupinu zabezpečení sítě), nasazení proxy server HTTP směrovat provoz.
2. Povolit přístup k Internetu prostřednictvím serveru proxy protokolu HTTP, přidejte pravidla na skupinu zabezpečení sítě, pokud nemáte.

Další informace o nastavení proxy serveru HTTP pro zálohování virtuálních počítačů naleznete v tématu [Příprava prostředí pro zálohování virtuálních počítačů Azure](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

Pokud používáte Azure spravované disky, bude pravděpodobně nutné počáteční další port (port 8443) na bránu firewall.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>Agent byl nainstalován ve virtuálním počítači, ale je reagovat (pro virtuální počítače Windows)

#### <a name="solution"></a>Řešení
Agent virtuálního počítače může dojít k poškození nebo služba může byla zastavena. Opětovné instalace agenta virtuálního počítače pomáhá získat nejnovější verzi. Pomáhá také restartovat komunikace se službou.

1. Zjistěte, zda v služby virtuálních počítačů (services.msc) spouští službu systému Windows agenta hosta. Pokuste se restartovat službu systému Windows agenta hosta a zahájit zálohování.    
2. Pokud není viditelná v služby v Ovládacích panelech službu agenta hosta Windows přejděte do **programy a funkce** k určení, zda je nainstalována služba Windows agenta hosta.
4. Pokud se zobrazí v agentovi hosta Windows **programy a funkce**, odinstalujte agenta hosta systému Windows.
5. Stáhněte a nainstalujte [nejnovější verzi MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Musí mít oprávnění správce k dokončení instalace.
6. Ověřte, že služby systému Windows agenta hosta se zobrazí v služby.
7. Spusťte zálohu na vyžádání: 
    * Na portálu, vyberte **zálohovat nyní**.

Také ověřte, že [je nainstalované rozhraní Microsoft .NET 4.5](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) ve virtuálním počítači. Rozhraní .NET 4.5 je vyžadována pro agenta virtuálního počítače ke komunikaci se službou.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Agent nainstalovaný ve virtuálním počítači je zastaralý (pro virtuální počítače s Linuxem)

#### <a name="solution"></a>Řešení
Většina související s agenta nebo rozšíření selhání pro virtuální počítače s Linuxem jsou způsobeny problémy, které ovlivňují zastaralé agenta virtuálního počítače. Chcete-li vyřešit tento problém, postupujte podle následujících obecných pokynů:

1. Postupujte podle pokynů pro [aktualizace agenta virtuálního počítače s Linuxem](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Jsme *důrazně doporučujeme* aktualizovat agenta pouze prostřednictvím distribuční úložiště. Nedoporučujeme stáhne kód agenta přímo z Githubu a jeho aktualizaci. Pokud nejnovější verzi agenta pro distribuční není k dispozici, obraťte distribuční podporu pokyny o tom, jak ji nainstalovat. Chcete-li vyhledat nejnovější agenta, přejděte na [Windows Azure Linux agent](https://github.com/Azure/WALinuxAgent/releases) stránky v úložišti GitHub.

2. Ujistěte se, že Azure agent běží ve virtuálním počítači tak, že spustíte následující příkaz:`ps -e`

 Pokud proces není spuštěná, restartujte ji pomocí následujících příkazů:

 * Pro Ubuntu:`service walinuxagent start`
 * Pro ostatní distribuce:`service waagent start`

3. [Konfigurace automatického restartování agenta](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Spusťte nové zálohování testu. Je-li chyba přetrvávat, shromážděte tyto protokoly z virtuálního počítače:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * / var/protokolu/azure / *

Pokud jsme vyžadovat podrobné protokolování pro příkaz waagent, postupujte podle těchto kroků:

1. V souboru /etc/waagent.conf vyhledejte následující řádek: **zapnout podrobné protokolování (y | n)**
2. Změna **Logs.Verbose** z hodnoty  *n*  k *y*.
3. Změnu uložíte a znovu spusťte příkaz waagent pomocí kroků popsaných výše v této části.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Nelze načíst stav snímku ani snímku nelze provést.
Zálohování virtuálních počítačů spoléhá na vydání snímku příkazu k základní účtu úložiště. Zálohování může selhat, protože nemá žádný přístup k účtu úložiště nebo protože provádění úlohy snímku je zpožděno.

#### <a name="solution"></a>Řešení
Úlohu snímku k chybě může dojít k následující podmínky:

| Příčina | Řešení |
| --- | --- |
| Virtuální počítač má nakonfigurované zálohování serveru SQL Server. | Ve výchozím nastavení zálohování virtuálních počítačů běží na svazku stínové kopie Service (VSS) úplného zálohování na virtuální počítače Windows. Na virtuální počítače, které jsou spuštěné na serveru SQL servery a na které systém SQL Server zálohování je nakonfigurované, může docházet k prodlevám provádění snímku.<br><br>Pokud dojde k selhání zálohování kvůli potížím snímek, nastavte následující klíč registru:<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| Stav virtuálního počítače je nesprávně uvést, protože virtuální počítač je vypnutý v protokolu RDP (Remote Desktop). | Pokud se vypnout virtuální počítač v protokolu RDP, zkontrolujte portálu k určení, zda je správný stav virtuálního počítače. Pokud není správný, vypněte virtuální počítač na portálu pomocí **vypnutí** možnost na řídicím panelu virtuálních počítačů. |
| Virtuální počítač nemůže získat adresu hostitele nebo z prostředků infrastruktury z DHCP. | DHCP musí být povolen v hosta pro zálohování virtuálních počítačů IaaS pracovat. Pokud virtuální počítač nemůže získat adresu hostitele nebo z prostředků infrastruktury z odpovědi DHCP 245, nemůže stáhnout nebo spustit libovolné rozšíření. Pokud potřebujete statickou privátní IP adresu, můžete ho nakonfigurujte prostřednictvím platformy. Možnost DHCP ve virtuálním počítači by měly být vlevo povoleny. Další informace najdete v tématu [nastavit statickou privátní IP interní](../virtual-network/virtual-networks-reserved-private-ip.md). |

### <a name="the-backup-extension-fails-to-update-or-load"></a>Rozšíření zálohování se nezdaří aktualizace nebo zatížení
Pokud rozšíření nelze načíst, zálohování se nezdaří, protože snímek nemůže být provedeny.

#### <a name="solution"></a>Řešení

**Pro hosty Windows:** ověřte, zda je povoleno iaasvmprovider služby a má typ spuštění *automatické*. Pokud služba není nakonfigurována tímto způsobem, povolte službu, kterou chcete zjistit, zda je úspěšné další zálohování.

**Pro hosty Linux:** ověřte, zda 1.0.91.0 nejnovější verzi VMSnapshot pro Linux (rozšíření používané zálohování).<br>


Pokud se aktualizace nebo zatížení stále rozšíření zálohování nezdaří, odinstalujte rozšíření vynutit rozšíření VMSnapshot znovu načíst. Další zálohování pokus znovu načte rozšíření.

Chcete-li odinstalovat rozšíření:

1. V [portál Azure](https://portal.azure.com/), přejděte k virtuálnímu počítači, kde dochází k selhání zálohování.
2. Vyberte **nastavení**.
3. Vyberte **rozšíření**.
4. Vyberte **Vmsnapshot rozšíření**.
5. Vyberte **odinstalovat**.

Dokončení tohoto postupu způsobí, že rozšíření nutné přeinstalovat během příští zálohování.

### <a name="backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock"></a>Služba zálohování nemá oprávnění k odstranění staré body obnovení z důvodu zámek skupiny prostředků
Tento problém je specifická pro spravovaných virtuálních počítačů, ve kterých uživatel uzamkne skupině prostředků. V takovém případě služby zálohování nelze odstranit starší body obnovení. Protože je omezeno na 18 body obnovení, nových záloh začnou mít problémy.

#### <a name="solution"></a>Řešení

Chcete-li vyřešit tento problém, proveďte následující kroky k odebrání kolekce bodu obnovení: <br>
 
1. Odebere se zámek ve skupině prostředků, ve kterém je umístěn virtuální počítač. 
2. Pomocí Chocolatey nainstalujte ARMClient: <br>
   https://github.com/projectkudu/ARMClient
3. Přihlaste se k ARMClient: <br>
    `.\armclient.exe login`
4. Získání kolekce bodu obnovení, která odpovídá virtuální počítač: <br>
    `.\armclient.exe get https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30`

    Příklad:`.\armclient.exe get https://management.azure.com/subscriptions/f2edfd5d-5496-4683-b94f-b3588c579006/resourceGroups/winvaultrg/providers/Microsoft.Compute/restorepointcollections/AzureBackup_winmanagedvm?api-version=2017-03-30`
5. Odstraňte kolekci bodu obnovení: <br>
    `.\armclient.exe delete https://management.azure.com/subscriptions/<SubscriptionId>/resourceGroups/<ResourceGroupName>/providers/Microsoft.Compute/restorepointcollections/AzureBackup_<VM-Name>?api-version=2017-03-30` 
6. Další naplánované zálohování automaticky vytvoří kolekci bod obnovení a nové body obnovení.

 
Pokud znovu zamknout skupinu prostředků, bude znovu nastane problém. 

