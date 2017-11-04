---
title: "Upgrade trezoru Site Recovery trezoru služeb zotavení Azure"
description: "Naučte se upgradovat trezoru služby Azure Site Recovery do trezoru služeb zotavení"
documentationcenter: 
author: rajani-janaki-ram
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/31/2017
ms.author: rajani-janaki-ram
ms.openlocfilehash: fdb33ea0d08353b491f2934fcf885fcb6910b9a2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="upgrade-a-site-recovery-vault-to-an-azure-resource-manager-based-recovery-services-vault"></a>Upgradujte trezoru Site Recovery trezoru služeb zotavení založené na Azure Resource Manager

Tento článek popisuje postup upgradu trezory Azure Site Recovery na službu založenou na Azure Resource Manager obnovení trezory bez žádný vliv na probíhající replikace. Další informace o funkce služby Správce prostředků Azure a výhod najdete v tématu [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md).

## <a name="introduction"></a>Úvod
Trezor služeb zotavení je prostředek Azure Resource Manageru pro správu zálohování a zotavení po havárii nativně v cloudu. Je jednotná trezoru, který můžete použít v nový portál Azure a nahradí classic zálohování a trezory Site Recovery.

Trezory služeb zotavení nabízí řadu funkcí, včetně:

* Podpora Azure Resource Manager: můžete chránit a převzetí služeb při selhání virtuálních počítačů a fyzických počítačů do Azure Resource Manager balíku.

* Vyloučení disku: Pokud máte dočasné soubory nebo vysokou změn dat, která nechcete použít všechny vaše šířka pásma pro, můžete z replikace vyloučit svazky. Tato funkce byla povolena aktuálně *VMware do Azure* a *technologie Hyper-V do Azure* a je rozšířeno na také jiné scénáře.

* Podpora pro premium a místně redundantního úložiště: teď mohou chránit servery v storage úrovně premium účty, které umožňují zákazníkům k ochraně aplikací s vyšší vstupně-výstupních operací za sekundu (IOPS). Tato funkce je aktuálně povoleno v *VMware do Azure*.

* Zjednodušený Začínáme zkušeností: rozšířené Začínáme prostředí má byly navrženy tak, aby instalační program zotavení po havárii snadno.

* Zálohování a správa Site Recovery z trezoru stejné: nyní může chránit servery pro zotavení po havárii nebo proveďte zálohu z ke stejnému trezoru, což může snížit režie výrazně vaší správy.

Další informace o upgradované prostředí a funkcích najdete v tématu [úložiště, zálohování a obnovení blog](https://azure.microsoft.com/blog/azure-site-recovery-now-available-in-a-new-experience-with-support-for-arm-and-csp/).

## <a name="salient-features"></a>Nejdůležitějšími funkce

* Žádný vliv na probíhající replikace: probíhající replikace pokračovat bez výpadku během a post upgradu.

* Bez dalších nákladů: Získejte celou sadu aktualizované funkcí bez dalších poplatků.

* Nedošlo ke ztrátě dat: protože tento proces je upgrade a není migrace, stávajících bodů obnovení replikace a nastavení zůstanou beze změn během a po dokončení upgradu.


## <a name="what-happens-during-the-vault-upgrade"></a>Co se stane během upgradu trezoru?

Během upgradu nelze provádět operace jako je registrace nového serveru nebo povolení replikace pro virtuální počítač (VM). Operace, které zahrnují čtení dat z nebo zápis dat do úložiště, jako je například probíhající replikace chráněných položek do trezoru, bez přerušení pokračovat.

### <a name="changes-to-automation-and-tooling-after-the-upgrade"></a>Změny automatizace a nástrojů po upgradu
Při upgradu trezoru typ z modelu nasazení classic do modelu nasazení Resource Manager, aktualizujte existující automatizace nebo nástrojů pro zajištění, že nadále fungovat po upgradu.

### <a name="prepare-your-environment-for-the-upgrade"></a>Příprava prostředí pro upgrade

* [Instalace prostředí PowerShell nebo upgradovat na verze 5 nebo novější](https://www.microsoft.com/download/details.aspx?id=50395)
* [Nainstalujte nejnovější verzi Azure PowerShell MSI](https://github.com/Azure/azure-powershell/releases)
* [Stáhnout upgradovací skript pro trezor služeb zotavení](https://aka.ms/vaultupgradescript)

### <a name="prerequisites"></a>Požadavky
K upgradu trezorů Site Recovery na trezory služby zotavení založené na Azure Resource Manager, musí splňovat následující požadavky:

* Verze agenta minimální: verzi Azure Site Recovery Provider nainstalovaný na serveru musí být 5.1.1700.0 nebo novější.

* Podporované konfigurace: trezoru nelze konfigurovat pomocí sítě SAN (SAN) nebo skupiny dostupnosti AlwaysOn serveru SQL. Jsou podporovány všechny ostatní konfigurace.

    >[!NOTE]
    >Po upgradu můžete spravovat úložiště mapování pouze pomocí prostředí PowerShell.

* Podporované scénáře: by neměl být trezoru *VMware do Azure* modelu starší verze nasazení. Než budete pokračovat, nejprve přesunete do modelu rozšířeného nasazení.

* Žádné aktivní úlohy spouštěné uživateli, které zahrnují správu roviny, operace: protože během upgradu má omezený přístup k rovině správy, dokončete všechny akce správy roviny předtím, než spustíte upgrade. Tento proces neobsahuje probíhající replikace.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

**Vliv tento upgrade Moje probíhající replikace?**

Ne. Probíhající replikace bude pokračovat bez přerušení, během a po dokončení upgradu.

**Co se stane nastavení sítě, jako je například nastavení site-to-site VPN a IP adresu?**

Upgrade nemá vliv nastavení sítě. Všechna připojení Azure na místní zůstanou beze změn.

**Co se stane Moje trezory, pokud nemáte chtít upgradovat v blízké budoucnosti?**

Podpora pro trezor Site Recovery na portálu Azure staré se od září 2017 zastaralé. Důrazně doporučujeme použijete funkci aktualizace pro přesun do nového portálu.

**Co znamená tento plán migrace pro moje existující nástrojů?**  

Aktualizace vašeho nástrojů k modelu nasazení Resource Manager je jedním z nejdůležitějších změny, které musíte vzít v úvahu pro v upgradu plánu. Trezory služeb zotavení jsou založené na modelu nasazení Resource Manager. 

**Jak dlouho nepodporuje správu roviny výpadek poslední?**

Upgrade obvykle trvá asi 15 až 30 minut, a to může trvat až maximálně jednu hodinu.

**Můžete I vrátit po provedení upgradu?**

Ne. Vrácení zpět není podporována po úspěšném upgradu prostředky.

**Můžete ověřit Moje předplatné nebo zdroji a zjistit, zda lze upgradovat?**

Ano. Prvním krokem při upgradu platformy podporované možnosti upgradu, je ověřit, zda jsou prostředky schopna upgradu. Pokud se ověření nezdaří, zobrazí se příslušná chybové zprávy nebo upozornění.

**Jak mohu ohlásit problém s nástrojem upgrade?**

Pokud dojde k jeho selhání během upgradu, poznamenejte si ID operace, která je uvedena v chybě. Microsoft Support fungovaly proaktivně v řešení problému. Můžete také kontaktovat tým podpory s ID předplatného, název trezoru a ID operace. Podpora bude fungovat k vyřešení daného problému provést co nejrychleji. Pokud jsou se na společnost Microsoft výslovně pokyn není opakujte operaci.

## <a name="run-the-script"></a>Spusťte skript

V prostředí PowerShell spusťte následující příkaz:

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1 -SubscriptionID <subscriptionID>  -VaultName <vaultname> -Location <location> -ResourceType HyperVRecoveryManagerVault -TargetResourceGroupName <rgname>

* ID předplatného: ID předplatného přidružené k trezoru, který upgradujete.

* VaultName: Název trezoru, který upgradujete.

* Umístění: Umístění úložiště, který upgradujete.

* Typ prostředku: Trezory HyperVRecoveryManagerVault Site Recovery.

* TargetResourceGroupName: Skupiny prostředků do kterého chcete upgradovaný trezoru umístit. TargetResourceGroupName může být existující skupinu prostředků v Azure Resource Manager nebo novou. Pokud TargetResourceGroupName, která se dodává neexistuje, bude vytvořen jako součást upgradu ve stejném umístění jako trezor. Další informace najdete v části "Prostředku skupiny" [přehled Azure Resource Manageru](../azure-resource-manager/resource-group-overview.md#resource-groups).

    >[!NOTE]
    >Názvy skupin prostředků je určitá omezení. Pokud chcete zabránit selhání aktualizace trezoru, je nutné pečlivě sledovat zásady vytváření názvů.
    >
    >Například:
    >
    >.\RecoveryServicesVaultUpgrade-1.0.0.ps1 - SubscriptionId 1234-54123-354354-56416-8645 - VaultName gen2dr-umístění "Severní Evropa" - ResourceType hypervrecoverymanagervault - TargetResourceGroupName abc

Alternativně můžete spustit následující skript. Zadejte hodnoty pro požadované parametry.

    PS > .\RecoveryServicesVaultUpgrade-1.0.0.ps1
    cmdlet RecoveryServicesVaultUpgrade-1.0.0.ps1 at command pipeline position 1

    Supply values for the following parameters:
    SubscriptionId:
    VaultName:
    Location:
    ResourceType:
    TargetResourceGroupName:

1. Skript prostředí PowerShell vás vyzve k zadání přihlašovacích údajů. Zadejte jejich dvakrát, jednou pro účet modelu nasazení classic a jednou pro účet Azure Resource Manager.

2. Po zadání přihlašovacích údajů, skript se spustí kontrolu k určení, zda vaše nastavení infrastruktury splňuje požadavky na výše uvedené.

3. Po požadavky byly zkontrolovat a potvrdit, zobrazí se výzva k pokračovat v upgradu trezoru. Proces upgradu spustí upgrade svůj trezor. Celý upgrade může trvat 15 až 30 minut.

4. Po upgradu byla úspěšně dokončena, můžete přístup k úložišti upgradovaný nového portálu Azure.

## <a name="post-upgrade-vault-management"></a>Po upgradu trezoru správy

### <a name="replicate-by-using-azure-site-recovery-in-the-recovery-services-vault"></a>Replikovat pomocí Azure Site Recovery v trezoru služeb zotavení

* Nyní můžete chránit virtuální počítače Azure z jedné oblasti do jiné. Další informace najdete v tématu [replikovat virtuální počítače Azure mezi oblastmi s Azure Site Recovery](site-recovery-azure-to-azure.md).

* Další informace o replikaci virtuálních počítačů VMware do Azure najdete v tématu [replikovat virtuální počítače VMware do Azure pomocí Site Recovery](vmware-walkthrough-overview.md).

* Další informace o replikaci virtuálních počítačů technologie Hyper-V (bez VMM) do Azure najdete v tématu [replikaci technologie Hyper-V virtuální počítače (bez VMM) do Azure](hyper-v-site-walkthrough-overview.md).

* Další informace o replikaci virtuálních počítačů Hyper-V (s nástrojem VMM) do Azure najdete v tématu [replikaci technologie Hyper-V virtuální počítače v cloudech VMM do Azure pomocí portálu Azure Site Recovery](vmm-to-azure-walkthrough-overview.md).

* Další informace o replikaci technologie Hyper-virtuálních počítačů (s VMM) do sekundární lokality najdete v tématu [replikaci technologie Hyper-V virtuální počítače v cloudech VMM do sekundární lokalita VMM pomocí portálu Azure](site-recovery-vmm-to-vmm.md).

* Další informace o replikaci virtuálních počítačů VMware do sekundární lokality najdete v tématu [replikovat místní virtuální počítače VMware nebo fyzických serverů do sekundární lokality na portálu Azure classic](site-recovery-vmware-to-vmware.md).

### <a name="view-your-replicated-items"></a>Zobrazení replikované položky

Následující obrázek znázorňuje stránky řídicího panelu trezoru služeb zotavení, který zobrazuje klíče entity trezoru. Chcete-li zobrazit seznam chráněných entit v trezoru, vyberte **Site Recovery** > **replikované položky**.


![Replikované položky](./media/upgrade-site-recovery-vaults/replicateditems.png)

Následující obrázek ukazuje pracovní postup pro zobrazení replikované položky a **převzetí služeb při selhání** příkaz pro inicializaci převzetí služeb při selhání.

![Replikované položky](./media/upgrade-site-recovery-vaults/failover.png)

### <a name="view-your-replication-settings"></a>Zobrazit nastavení replikace

V trezoru Site Recovery každou skupinu ochrany je nastavena frekvence kopírování, uchování bodu obnovení, frekvence snímků konzistentní aplikace a další nastavení replikace. V trezoru služeb zotavení tato nastavení jsou konfigurována jako zásady replikace. Název zásady je název skupiny ochrany nebo *primarycloud_Policy*.

Další informace o zásadách replikace najdete v tématu [Správa zásad replikace pro VMware do Azure](site-recovery-setup-replication-settings-vmware.md).
