---
title: "Zálohuje classic nasazené virtuální počítače Azure do trezoru záloh | Microsoft Docs"
description: "Zjistit, zaregistrujte a zálohování virtuálních počítačů s tyto postupy pro zálohování virtuálních počítačů Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "zálohování virtuálních počítačů; zálohování virtuálního počítače; zálohování a zotavení po havárii; zálohování virtuálních počítačů"
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: ca3fb650a133efb67c0ad2cd96847c6a0e21c876
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/11/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a>Vytvořte zálohu virtuálních počítačích Azure (klasický portál)
> [!div class="op_single_selector"]
> * [Zálohování virtuálních počítačů do trezoru služeb zotavení](backup-azure-arm-vms.md)
> * [Zálohování virtuálních počítačů do trezoru zálohování](backup-azure-vms.md)
>
>

Tento článek obsahuje postupy pro zálohování nasazení Classic Azure virtuálního počítače (VM) do úložiště záloh. Existuje několik úloh, které potřebujete k postará o předtím, než můžete zálohovat virtuální počítač Azure. Pokud jste tak již neučinili, proveďte [požadavky](backup-azure-vms-prepare.md) při přípravě svého prostředí pro zálohování virtuálních počítačů.

Další informace najdete v článcích na [plánování infrastruktury zálohování virtuálních počítačů v Azure](backup-azure-vms-introduction.md) a [virtuální počítače Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).

> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). Úložiště záloh lze chránit pouze virtuálních počítačů nasazených Classic. Nelze chránit virtuálních počítačů nasazených Resource Manager pomocí úložiště záloh. V tématu [zálohování virtuálních počítačů do trezoru služeb zotavení](backup-azure-arm-vms.md) podrobnosti o práci se službou obnovení trezory.
>
>

Zálohování virtuálních počítačů Azure zahrnuje tři základní kroky:

![Tři kroky pro zálohování virtuálního počítače Azure IaaS](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> Zálohování virtuálních počítačů je místní proces. Virtuální počítače v jedné oblasti nelze zálohovat do trezoru záloh v jiné oblasti. Proto je nutné vytvořit úložiště záloh v každé oblasti Azure, kde existují virtuální počítače, které se bude zálohovat.
>
> [!IMPORTANT]
> Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Po 30. listopadu 2017 nebudete moct pomocí prostředí PowerShell vytvořit trezory Backup. **2017 30 listopadu**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.
>

## <a name="step-1---discover-azure-virtual-machines"></a>Krok 1 – zjistit virtuální počítače Azure
Aby se identifikují všechny nové virtuální počítače (VM) k předplatnému přidat před registrací, spusťte proces vyhledávání. Proces se dotáže Azure na seznam virtuálních počítačů v rámci předplatného společně s dalšími informacemi, jako například název cloudové služby a oblast.

1. Přihlaste se k [portál Classic](http://manage.windowsazure.com/)
2. V seznamu služeb Azure, klikněte na **služeb zotavení** otevření seznamu trezorů zálohování a obnovení lokality.
    ![Otevřít trezor seznamu](./media/backup-azure-vms/choose-vault-list.png)
3. V seznamu trezory Backup vyberte trezor pro zálohování virtuálního počítače.

    Pokud je to nový trezor otevře na portálu **rychlý Start** stránky.

    ![Otevřete registrovaná položky nabídky](./media/backup-azure-vms/vault-quick-start.png)

    Pokud byl dříve nakonfigurován trezoru, portál ho otevře naposledy použité nabídky.
4. V nabídce trezoru (v horní části stránky), klikněte na tlačítko **registrované položky**.

    ![Otevřete registrovaná položky nabídky](./media/backup-azure-vms/vault-menu.png)
5. Z nabídky **Typ** vyberte **Virtuální počítač Azure**.

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
6. Klikněte na **Vyhledat** ve spodní části stránky.
    ![Tlačítko Vyhledat](./media/backup-azure-vms/discover-button-only.png)

    Proces vyhledávání může kvůli srovnávání virtuálních počítačů trvat i několik minut. Ve spodní části obrazovky je oznámení, které indikuje, že proces běží.

    ![Vyhledání virtuálních počítačů](./media/backup-azure-vms/discovering-vms.png)

    Po dokončení procesu se oznámení změní. Pokud proces zjišťování nebyl nalezen virtuální počítače, nejdříve se ujistěte, že existují virtuální počítače. Pokud existují virtuální počítače, ověřte, že jsou virtuální počítače ve stejné oblasti jako trezor záloh. Pokud virtuálních počítačů existují a jsou ve stejné oblasti, zajistěte, aby že virtuální počítače nejsou zaregistrována do trezoru záloh. Pokud je virtuální počítač přiřazen do trezoru záloh není k dispozici pro přiřazení dalších záloh.

    ![Vyhledávání dokončeno](./media/backup-azure-vms/discovery-complete.png)

    Po zjištění nové položky přejděte ke kroku 2 a zaregistrujte virtuální počítače.

## <a name="step-2---register-azure-virtual-machines"></a>Krok 2 – registrace virtuálních počítačů Azure
Zaregistrujte virtuální počítač Azure přidružit pomocí služby Azure Backup. Obvykle se jedná o jednorázové aktivity.

1. Přejděte do trezoru záloh v části **služeb zotavení** v portálu Azure a pak klikněte na tlačítko **registrované položky**.
2. Z rozevírací nabídky vyberte **Virtuální počítač Azure**

    ![Výběr úlohy](./media/backup-azure-vms/discovery-select-workload.png)
3. Klikněte na **Registrovat** ve spodní části stránky.
    ![Tlačítko Registrovat](./media/backup-azure-vms/register-button-only.png)
4. V místní nabídce **Registrovat položky** vyberte virtuální počítače, které chcete zaregistrovat. Pokud existují dvě nebo více virtuálních počítačů se stejným názvem, použijte k rozlišení mezi nimi cloudové služby.

   > [!TIP]
   > Najednou může být registrováno více virtuálních počítačů.
   >
   >

    Pro každý virtuální počítač, který jste vybrali, se vytvoří úloha.
5. Klikněte na **Zobrazit úlohy** v oznámení pro přechod na stránku **Úlohy**.

    ![Registrace úlohy](./media/backup-azure-vms/register-create-job.png)

    Virtuální počítač se také zobrazí v seznamu registrovaných položek společně se stavem operace registrování.

    ![Stav registrace 1](./media/backup-azure-vms/register-status01.png)

    Po dokončení operace se stav změní, aby reflektoval stav *registrován*.

    ![Stav registrace 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a>Krok 3 – chránit virtuální počítače Azure
Nyní můžete nastavit zásadu zálohování a uchovávání pro virtuální počítač. Více virtuálních počítačů může být chráněn pomocí jedné chránit akce.

Azure trezory Backup vytvořené po květen 2015 dodávají s výchozí zásadou integrovaný do trezoru. Tato výchozí zásada se dodává s výchozí uchování 30 dnů a plán zálohování jednou denně.

1. Přejděte do trezoru záloh v části **služeb zotavení** v portálu Azure a pak klikněte na tlačítko **registrované položky**.
2. Z rozevírací nabídky vyberte **Virtuální počítač Azure**

    ![Výběr úlohy v portálu](./media/backup-azure-vms/select-workload.png)
3. Klikněte na **Chránit** ve spodní části stránky.

    **Průvodce ochranou položek** se zobrazí. Průvodce je uveden pouze virtuální počítače, které jsou zaregistrované a nechráněné. Vyberte virtuální počítače, které chcete ochránit.

    Pokud existují dvě nebo více virtuálních počítačů se stejným názvem, použijte k rozlišení mezi virtuálními počítači cloudové služby.

   > [!TIP]
   > Najednou můžete chránit více virtuálních počítačů.
   >
   >

    ![Škálování konfigurace ochrany](./media/backup-azure-vms/protect-at-scale.png)

4. Vyberte **plán zálohování** zálohovat virtuální počítače, které jste vybrali. Můžete vybrat z existující sady zásad nebo zadejte nový.

    Ke každé zásadě zálohování může být přidružených více virtuálních počítačů. Však virtuálního počítače lze přidružit pouze s jednu zásadu v libovolném bodě v čase.

    ![Ochrana pomocí nové zásady](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > Zásada zálohování obsahuje schéma uchovávání pro plánované zálohy. Pokud vyberete existující zásady zálohování, nelze změnit možnosti uchovávání v dalším kroku.
   >
   >

5. Vyberte **rozsah uchování** přidružení k zálohování.

    ![Chránit pomocí flexibilní uchování](./media/backup-azure-vms/policy-retention.png)

    Zásada uchovávání specifikuje, jak dlouho se záloha má uchovat. Můžete zadat rozdílné zásady v závislosti na tom, kdy dochází k zálohování. Například bod zálohy pořizovat denně (který slouží jako bod obnovení provozní) může být zachována 90 dní. Porovnání může bod zálohy prováděné na konci každé čtvrtletí (pro účely auditu) potřeba zachovat pro mnoho měsíců nebo let.

    ![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/long-term-retention.png)

    Na tomto obrázku příklad:

   * **Denní zásady uchovávání informací**: zálohy pořizovat denně se uchovávají po dobu 30 dnů.
   * **Týdenní zásady uchovávání informací**: zálohy pořízené každý týden v neděli zachovány 104 týdny.
   * **Měsíční zásady uchovávání informací**: zálohy pořízené poslední neděli každého měsíce se zachovají pro 120 měsíců.
   * **Roční zásady uchovávání informací**: zálohy vytvořené v první neděli daného každých leden se zachovají pro 99 let.

     Úloha se vytvoří pro nakonfigurujte zásady ochrany a přidružte virtuální počítače k této zásadě pro každý virtuální počítač, který jste vybrali.
6. Chcete-li zobrazit seznam **Konfigurace ochrany** klikněte na úlohy, v nabídce trezory **úlohy** a vyberte **Konfigurace ochrany** z **operace** filtru.

    ![Úloha konfigurace ochrany](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a>Prvotní zálohování
Jakmile je virtuální počítač je chráněný pomocí zásad, se zobrazí v části **chráněné položky** karta se stavem *chráněno – (čekání na prvotní zálohování)*. Ve výchozím nastavení je první plánovanou zálohou *prvotní záloha*.

K aktivaci prvotní zálohování okamžitě po konfiguraci ochrany:

1. V dolní části **chráněné položky** klikněte na tlačítko **zálohovat nyní**.

    Služba Azure Backup vytvoří úlohu zálohování pro operaci prvotního zálohování.
2. Chcete-li zobrazit seznam úloh, klikněte na kartu **Úlohy**.

    ![Probíhá zálohování](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> Během operace zálohování vydá služba Azure Backup příkaz rozšířením zálohování v každém virtuálním počítači k vyprázdnění všech úloh zápisu a pořízení konzistentního snímku.
>
>

Po prvotní zálohování dokončení, stav virtuálního počítače **chráněné položky** karta je *chráněné*.

![Virtuální počítač je zálohovaný s bodem obnovení](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a>Zobrazení stavu zálohy a podrobnosti
Jakmile chráněný, také zvyšuje počet virtuálních počítačů v **řídicí panel** stránky Souhrn. **Řídicí panel** stránka zobrazuje také počet úloh z za posledních 24 hodin, které byly *úspěšné*, mají *se nezdařilo*a jsou *v průběhu*. Na **úlohy** stránky, použijte **stav**, **operace**, nebo **z** a **k** nabídky pro filtrování úloh.

![Stav služby backup v stránka řídicího panelu](./media/backup-azure-vms/dashboard-protectedvms.png)

Hodnoty v řídicím panelu se aktualizuje každých 24 hodin.

## <a name="troubleshooting-errors"></a>Řešení potíží s chybami
Pokud narazíte na problémy při zálohování virtuálního počítače, podívejte se na [článku Poradce při potížích virtuálních počítačů](backup-azure-vms-troubleshoot.md) nápovědu.

## <a name="next-steps"></a>Další kroky
* [Správa a monitorování virtuálních počítačů](backup-azure-manage-vms.md)
* [Obnovení virtuálních počítačů](backup-azure-restore-vms.md)
