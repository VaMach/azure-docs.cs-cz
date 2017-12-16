---
title: "Obnovení virtuálních počítačů ze zálohy | Microsoft Docs"
description: "Zjistěte, jak obnovit virtuální počítač Azure z bodu obnovení"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: "obnovení zálohy; Postup obnovení; bod obnovení;"
ms.assetid: fed877b3-b496-49fb-99e4-653be7c23e3a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cwatson
ms.openlocfilehash: 08edd8981a8230848c97a86e752ef3fdb28ca330
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="restore-virtual-machines-in-azure"></a>Obnovení virtuálních počítačů v Azure
> [!div class="op_single_selector"]
> * [Obnovení virtuálních počítačů na portálu Azure](backup-azure-arm-restore-vms.md)
> * [Obnovení virtuálních počítačů v klasickém portálu](backup-azure-restore-vms.md)
>
>

Obnovení virtuálního počítače na nový virtuální počítač ze záloh uložených v trezoru zálohování Azure pomocí následujících kroků.

> [!IMPORTANT]
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services. Po <br/> **30. listopadu 2017**, už nebudete moct pomocí prostředí PowerShell vytvořit trezory Backup. <br/> Podle **30. listopadu 2017**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>

## <a name="restore-workflow"></a>Obnovení pracovního postupu
### <a name="step-1-choose-an-item-to-restore"></a>Krok 1: Vyberte položku, kterou chcete obnovit
1. Přejděte na **chráněné položky** a vyberte virtuální počítač, který chcete obnovit na nový virtuální počítač.

    ![Chráněné položky](./media/backup-azure-restore-vms/protected-items.png)

    **Bod obnovení** sloupec v **chráněné položky** stránky zjistíte počet bodů obnovení pro virtuální počítač. **Nejnovější bod obnovení** sloupec vám říká, čas poslední zálohy, ze kterého lze obnovit virtuální počítač.
2. Klikněte na tlačítko **obnovení** otevřete **obnovit položky** průvodce.

    ![Obnovit položky](./media/backup-azure-restore-vms/restore-item.png)

### <a name="step-2-pick-a-recovery-point"></a>Krok 2: Vyberte bod obnovení
1. V **vyberte bod obnovení** obrazovky, můžete obnovit z nejnovější bod obnovení, nebo z předchozího bodu v čase. Když se Průvodce otevře vybranou výchozí možnost je *nejnovější bod obnovení*.

    ![Vyberte bod obnovení](./media/backup-azure-restore-vms/select-recovery-point.png)
2. Chcete-li vybrat dřívějšího bodu v čase, zvolte **vyberte datum** možnost v rozevírací nabídce a vyberte datum v ovládacím prvku kalendář kliknutím na **ikonu kalendáře**. V ovládacím prvku všech kalendářních dat, které mají body obnovení jsou vyplněny světla šedé stín a jsou volitelný uživatelem.

    ![Vyberte datum](./media/backup-azure-restore-vms/select-date.png)

    Po kliknutí na tlačítko datum v ovládacím prvku kalendář, body obnovení dostupné na že data budou zobrazena v následující tabulce body obnovení. **Čas** sloupec zobrazuje informace o době pořízení snímku. **Typ** sloupec zobrazuje [konzistence](https://azure.microsoft.com/documentation/articles/backup-azure-vms/#consistency-of-recovery-points) bodu obnovení. Záhlaví tabulky se zobrazuje počet bodů obnovení, které jsou k dispozici v daný den v závorkách.

    ![Body obnovení](./media/backup-azure-restore-vms/recovery-points.png)
3. Vyberte bod obnovení z **body obnovení** tabulky a klikněte na šipku další přejdete na zobrazení další obrazovky.

### <a name="step-3-specify-a-destination-location"></a>Krok 3: Zadejte cílové umístění
1. V **výběr obnovení instance** obrazovky zadat podrobnosti o umístění, kam obnovit virtuální počítač.

   * Zadejte název virtuálního počítače: V dané cloudové službě, musí být jedinečný název virtuálního počítače. Nepodporujeme přepsání zápis existující virtuální počítač.
   * Vyberte cloudovou službu pro virtuální počítač: Toto je povinné pro vytvoření virtuálního počítače. Můžete buď použít stávající cloudovou službu nebo vytvořit novou cloudovou službu.

        Ať název cloudové služby je zachyceny by měly být globálně jedinečný. Obvykle získá název cloudové služby související s veřejnou adresou URL ve formátu [cloudservice]. cloudapp.net. Azure nebude možné vytvořit novou cloudovou službu, pokud název se již používá. Pokud vyberete možnost vytvořit novou cloudovou službu, bude udělená stejný název jako virtuální počítač – ve kterém musí být dostatečně má být použita pro související cloudové služby jedinečný název virtuálního počítače zachyceny – případ.

        Nemůžeme zobrazit pouze cloudové služby a virtuální sítě, které nejsou přiřazeny žádné skupiny vztahů v podrobnostech obnovení instance. [Další informace](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).
2. Vyberte účet úložiště pro virtuální počítač: Toto je povinné pro vytvoření virtuálního počítače. Můžete vybrat z existující účty úložiště ve stejné oblasti jako trezor zálohování Azure. Účty úložiště, které jsou zóny redundantní nebo typ úložiště Premium se nepodporuje.

    Pokud nejsou žádné účty úložiště s podporovanou konfiguraci, vytvořte účet úložiště podporované konfigurace před zahájením operace obnovení.

    ![Vyberte virtuální síť](./media/backup-azure-restore-vms/restore-sa.png)
3. Vyberte virtuální síť: při vytváření virtuálního počítače je nutné vybrat virtuální síť (VNET) pro virtuální počítač. Obnovení uživatelského rozhraní obsahuje všechny virtuální sítě v rámci tohoto předplatného, který lze použít. Není to povinné k výběru virtuální sítě pro virtuální počítač obnovený – bude moct připojit k obnovený virtuální počítač přes internet, i v případě, že není použita virtuální sítě.

    Pokud vybraná cloudové služby je přidružený k virtuální síti, nelze změnit virtuální sítě.

    ![Vyberte virtuální síť](./media/backup-azure-restore-vms/restore-cs-vnet.png)
4. Vyberte podsíť: V případě, že virtuální sítě má podsítě, ve výchozím nastavení bude vybrána první podsíť. Z možností rozevíracího seznamu vyberte podsíť podle svého výběru. Podrobnosti podsíť, přejděte na rozšíření sítě [domovské stránky portálu](https://manage.windowsazure.com/), přejděte na **virtuální sítě** a vyberte virtuální síť a k podrobnostem konfigurovat zobrazíte podrobnosti o podsíti.

    ![Vyberte podsíť](./media/backup-azure-restore-vms/select-subnet.png)
5. Klikněte **odeslání** ikona v průvodci a odešlete podrobnosti a vytvořit úlohu obnovení.

## <a name="track-the-restore-operation"></a>Sledování operací obnovení
Po vstupu všechny informace do Průvodce obnovením a odeslat ho Azure Backup se pokusí vytvořit úlohu pro sledování operaci obnovení.

![Vytvoření úlohy obnovení](./media/backup-azure-restore-vms/create-restore-job.png)

Pokud úloha vytvoření úspěšné, zobrazí se informační zprávy oznámení s informací, že se vytvoří úloha. Další informace získáte kliknutím **zobrazit úlohy** tlačítko, které se dostanete k **úlohy** kartě.

![Úloha obnovení byla vytvořena](./media/backup-azure-restore-vms/restore-job-created.png)

Po dokončení operace obnovení bude označena jako dokončená v **úlohy** kartě.

![Dokončení úlohy obnovení](./media/backup-azure-restore-vms/restore-job-complete.png)

Po obnovení virtuálního počítače budete muset znovu nainstalovat rozšíření stávajících původní virtuální počítač a [upravit koncových bodů](../virtual-machines/windows/classic/setup-endpoints.md) pro virtuální počítač na portálu Azure.

## <a name="post-restore-steps"></a>Po obnovení kroky
Pokud použijete distribuci systému Linux na základě inicializací cloudu, jako například Ubuntu, z bezpečnostních důvodů, bude blokován heslo post obnovení. Na obnovený virtuální počítač použijte rozšíření VMAccess [resetování hesla](../virtual-machines/linux/classic/reset-access.md). Doporučujeme, abyste pomocí klíče SSH na tyto distribuce, aby se zabránilo resetování hesla post obnovení.

## <a name="backup-for-restored-vms"></a>Zálohování pro obnovený virtuální počítače
Pokud jste obnovili virtuálních počítačů do stejné cloudové služby se stejným názvem jako původně zálohovat virtuální počítač, zálohování bude pokračovat na post obnovení virtuálního počítače. Pokud máte virtuální počítač obnovit do jiné cloudové služby, nebo zadat jiný název pro virtuální počítač obnovený, to bude považována za nového virtuálního počítače a budete muset instalační program Zálohování pro virtuální počítač obnovený.

## <a name="restoring-a-vm-during-azure-datacenter-disaster"></a>Obnovení virtuálního počítače během Azure DataCenter po havárii
Zálohování Azure umožňuje obnovení zálohovat virtuální počítače do spárované datového centra v případě, že primární datového centra, kde běží virtuálních počítačů po havárii prostředí a jste nakonfigurovali úložiště záloh být geograficky redundantní. Během takových scénářů je nutné vybrat účet úložiště, který je přítomen v spárované datového centra a zbytek procesu obnovení zůstává stejné. Azure Backup používá výpočetní služby z spárované geograficky vytvořit obnovený virtuální počítač. Další informace o [Azure Data center odolnost proti chybám](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md)

## <a name="restoring-domain-controller-vms"></a>Obnovení virtuálních počítačů řadiče domény
Zálohování virtuálních počítačů řadiči domény (DC) je podporované scénáře s Azure Backup. Však musí dát pozor během procesu obnovení. Proces obnovení správný závisí na strukturu domény. V nejjednodušším případě máte u jednoho řadiče domény v jedné doméně. Běžně pro produkční zatížení, budete mít na jednu doménu s více řadičů domény, případně s řadiče domény se místně. Nakonec můžete mít doménová struktura s více domén.

Z hlediska Active Directory je virtuální počítač Azure jako další virtuální počítač na moderní podporovaném hypervisoru. Hlavní rozdíl s místními hypervisory je, že je v Azure k dispozici žádné konzoly virtuálního počítače. Konzola je nutný v některých případech, například obnovení pomocí typ zálohy úplného obnovení (BMR). Obnovení virtuálního počítače z trezoru záloh je však úplné nahrazení pro úplné obnovení systému. Active Directory obnovení režimu (DSRM) je také k dispozici, proto všechny scénáře obnovení služby Active Directory je přijatelná. Podrobnější informace, zkontrolujte [zálohování a obnovení aspekty virtualizovaných řadičů domény](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) a [plánování obnovení doménové struktury služby Active Directory](https://technet.microsoft.com/en-us/library/planning-active-directory-forest-recovery(v=ws.10).aspx).

### <a name="single-dc-in-a-single-domain"></a>Jednoho řadiče domény v jedné doméně
Virtuální počítač může být obnovena (např. žádné jiné virtuální počítače) z Azure portálu nebo pomocí prostředí PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Víc řadičů domény v jedné doméně
Pokud jiné řadiče domény do stejné domény dostupný přes síť, můžete je obnovit řadič domény jako žádné virtuální počítače. Pokud je poslední zbývající řadič domény v doméně, nebo se provádí obnovení v izolované síti, musí být sledována procedury obnovení doménové struktury.

### <a name="multiple-domains-in-one-forest"></a>V jedné doménové struktuře víc domén
Pokud jiné řadiče domény do stejné domény dostupný přes síť, můžete je obnovit řadič domény jako žádné virtuální počítače. Však ve všech ostatních případech se doporučuje obnovení doménové struktury.

<!--- WK: this following original supportability statement is incorrect, taking it out.
The challenge arises because DSRM mode is not present in Azure. So to restore such a VM, you cannot use the Azure portal. The only supported restore mechanism is disk-based restore using PowerShell.

> [!WARNING]
> For Domain Controller VMs in a multi-DC environment, do not use the Azure portal for restore! Only PowerShell based restore is supported
>
>

Read more about the [USN rollback problem](https://technet.microsoft.com/library/dd363553) and the strategies suggested to fix it.
--->

## <a name="restoring-vms-with-special-network-configurations"></a>Obnovení virtuálních počítačů s zvláštní síťové konfigurace
Azure Backup podporuje zálohování pro následující konfigurace speciální sítě virtuálních počítačů.

* Virtuální počítače v části Vyrovnávání zatížení (interních a externích)
* Virtuální počítače s víc vyhrazených IP adres
* Virtuální počítače s více síťovými kartami

Tyto konfigurace vyžádá následující aspekty při jejich obnovení.

> [!TIP]
> Použijte prosím tok obnovení prostředí PowerShell na základě konfigurace speciální sítě virtuálních počítačů post obnovení znovu vytvořit.
>
>

### <a name="restoring-from-the-ui"></a>Obnovení z uživatelského rozhraní:
Při obnovení z uživatelského rozhraní, **vždy vyberte novou cloudovou službu**. Upozorňujeme, že vzhledem k tomu, že portál trvá pouze povinné parametry během obnovení tok, virtuální počítače obnovit pomocí uživatelského rozhraní ztratí zvláštní síťové konfigurace, které mohou mít. Jinými slovy, obnovení virtuálních počítačů bude normální virtuálních počítačů bez konfigurace pro vyrovnávání zatížení nebo více síťového adaptéru nebo více vyhrazenou IP adresu.

### <a name="restoring-from-powershell"></a>Obnovení z prostředí PowerShell:
Prostředí PowerShell má schopnost právě obnovit ze zálohy disky virtuálních počítačů a vytvořit virtuální počítač. To je užitečné, když obnovení virtuálních počítačů, které vyžadují zvláštní síťové konfigurace uvedených výše.

Chcete-li znovu plně obnovení disky virtuálního počítače post, postupujte takto:

1. Obnovení disky pomocí úložiště záloh [zálohování Azure PowerShell](backup-azure-vms-classic-automation.md#restore-an-azure-vm)
2. Vytvoření virtuálního počítače Konfigurace požadované pro nástroj pro vyrovnávání zatížení nebo více Síťových nebo více vyhrazenou IP adresu pomocí rutiny prostředí PowerShell a použít je pro vytvoření virtuálního počítače z žádoucí konfigurace.

   * Vytvoření virtuálního počítače v rámci cloudové služby s [interní zátěže.](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/)
   * Vytvoření virtuálního počítače pro připojení k [internetové nástroj pro vyrovnávání zatížení](https://azure.microsoft.com/en-us/documentation/articles/load-balancer-internet-getstarted/)
   * Vytvoření virtuálního počítače s [několik síťových adaptérů](https://azure.microsoft.com/documentation/articles/virtual-networks-multiple-nics/)
   * Vytvoření virtuálního počítače s [více vyhrazené IP adresy](https://azure.microsoft.com/documentation/articles/virtual-networks-reserved-public-ip/)

## <a name="next-steps"></a>Další kroky
* [Řešení potíží s chybami](backup-azure-vms-troubleshoot.md#restore)
* [Správa virtuálních počítačů](backup-azure-manage-vms.md)
