---
title: "Spravovat a monitorovat zálohování virtuálního počítače Azure | Microsoft Docs"
description: "Zjistěte, jak spravovat a monitorovat zálohování virtuálního počítače Azure"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: cwatson
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8dcefacd700f3b475eb477f80201659a2bea802
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/15/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-the-classic-portal"></a>Spravovat běžné úlohy zálohování Azure a aktivační události upozornění v portálu classic
> [!div class="op_single_selector"]
> * [Spravovat zálohy virtuálního počítače Azure](backup-azure-manage-vms.md)
> * [Spravovat zálohy Classic virtuálních počítačů](backup-azure-manage-vms-classic.md)
>
>

Tento článek obsahuje informace o běžné úlohy správy a sledování pro Classic model virtuální počítače chráněné v Azure.  

> [!NOTE]
> Azure obsahuje dva modely nasazení pro vytváření a práci s prostředky: [Resource Manager a Classic](../azure-resource-manager/resource-manager-deployment-model.md). V tématu [Příprava prostředí pro zálohování virtuálních počítačů Azure](backup-azure-vms-prepare.md) podrobnosti o práci s klasického nasazení modelu virtuálních počítačů.
>
> [!IMPORTANT]
>Od března 2017 již nelze k vytvoření trezorů služby Backup použít portál Classic.
>
> Nyní můžete trezory služby Backup upgradovat na trezory služby Recovery Services. Podrobnosti najdete v článku [Upgrade trezoru služby Backup na trezor služby Recovery Services](backup-azure-upgrade-backup-to-recovery-services.md). Microsoft doporučuje, abyste upgradovali své trezory služby Backup na trezory služby Recovery Services.<br/> Po 30. listopadu 2017 nebudete moct pomocí prostředí PowerShell vytvořit trezory Backup. **2017 30 listopadu**:
>- Všechny zbývající trezory služby Backup budou automaticky upgradovány na trezory služby Recovery Services.
>- Nebudete mít přístup k datům záloh na portálu Classic. Pro přístup k datům záloh v trezorech služby Recovery Services místo toho použijte Azure Portal.

## <a name="manage-protected-virtual-machines"></a>Správa chráněných virtuálních počítačů
Správa chráněných virtuálních počítačů:

1. Zobrazení a správa klikněte na tlačítko Nastavení zálohování pro virtuální počítač **chráněné položky** kartě.
2. Klikněte na název chráněné položky zobrazíte **zálohování podrobnosti** kartu, která vám zobrazí informace o poslední zálohy.

    ![Záloha virtuálního počítače](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. Zobrazení a správa klikněte na nastavení zásad zálohování pro virtuální počítač **zásady** kartě.

    ![Virtuální počítač zásad](./media/backup-azure-manage-vms/manage-policy-settings.png)

    **Zásady zálohování** kartě se zobrazují existující zásady. Můžete upravit podle potřeby. Pokud budete muset vytvořit nové zásady, klepněte na tlačítko **vytvořit** na **zásady** stránky. Všimněte si, že pokud chcete odebrat zásadu by neměl mít všechny virtuální počítače s ním spojená.

    ![Virtuální počítač zásad](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. Můžete získat další informace o stavu nebo akce pro virtuální počítač **úlohy** stránky. Klikněte na úlohu v seznamu zobrazíte další podrobnosti, nebo filtrovat úlohy pro konkrétní virtuální počítač.

    ![Úlohy](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a>Zálohování na vyžádání virtuálního počítače
Na vyžádání můžete provést zálohování virtuálního počítače, jakmile je nakonfigurován pro ochranu. Pokud prvotní zálohování čeká na vyřízení pro virtuální počítač, bude zálohování na vyžádání vytvoření úplné kopie virtuálního počítače v záložním trezoru Azure. Pokud se první zálohování je dokončeno, bude zálohování na vyžádání jen odeslání změn z předchozí zálohy pro zálohování Azure trezoru tj ho je vždy přírůstkové.

> [!NOTE]
> Rozsah uchování zálohu na vyžádání je nastavena na hodnotu uchování zadaná pro denní doba uchování ve zásady zálohování odpovídající virtuální počítač.  
>
>

Chcete-li provést zálohování virtuálního počítače na vyžádání:

1. Přejděte na **chráněné položky** a vyberte **virtuální počítač Azure** jako **typ** (Pokud již není vybrána) a klikněte na **vyberte** tlačítko.

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. Vyberte virtuální počítač, na kterém chcete provést zálohování na vyžádání a klikněte na **zálohování** tlačítko v dolní části stránky.

    ![Zálohovat nyní](./media/backup-azure-manage-vms/backup-now.png)

    Tím se vytvoří úlohu zálohování na vybraný virtuální počítač. Rozsah uchování bodu obnovení, které jsou vytvořené pomocí této úlohy bude stejné jako příkaz, který je uveden v zásadách, které jsou spojené s virtuálním počítačem.

    ![Vytvoření úlohy zálohování](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > Chcete-li zobrazit zásady přidružené virtuální počítač, přejděte do virtuálního počítače v **chráněné položky** stránky a přejděte na kartu zásady zálohování.
   >
   >
3. Po vytvoření úlohy, můžete kliknutím na **zobrazit úlohy** tlačítka na panelu informační zobrazíte odpovídající úlohy na stránce úloh.

    ![Vytvořit úlohu zálohování](./media/backup-azure-manage-vms/created-job.png)
4. Po úspěšném dokončení úlohy bude možné vytvořit bod obnovení, který můžete použít k obnovení virtuálního počítače. Hodnota sloupce bodu obnovení to také se zvýší o 1 v **chráněné položky** stránky.

## <a name="stop-protecting-virtual-machines"></a>Zastavte ochranu virtuálních počítačů
Můžete zastavit budoucí zálohy virtuálního počítače s následujícími parametry:

* Zachovat zálohovaná data související s virtuálním počítačem v trezoru zálohování Azure
* Odstranit záložní data související s virtuálním počítačem

Pokud jste vybrali možnost zachování zálohovaná data související s virtuálním počítačem, můžete tento virtuální počítač obnovit zálohovaná data. O cenách podrobnosti takových virtuálních počítačů, klikněte na tlačítko [zde](https://azure.microsoft.com/pricing/details/backup/).

Ukončete ochranu pro virtuální počítač:

1. Přejděte na **chráněné položky** a vyberte **virtuální počítač Azure** jako typ filtru (Pokud již není vybrána) a klikněte na **vyberte** tlačítko.

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. Vyberte virtuální počítač a klikněte na **zastavit ochranu** v dolní části stránky.

    ![Zastavení ochrany](./media/backup-azure-manage-vms/stop-protection.png)
3. Ve výchozím nastavení zálohování Azure nedojde k odstranění zálohovaná data související s virtuálním počítačem.

    ![Potvrďte zastavení ochrany](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    Pokud chcete odstranit záložní data, zaškrtněte políčko.

    ![Zaškrtávací políčko](./media/backup-azure-manage-vms/checkbox.png)

    Vyberte důvod zastavení zálohování. Zatímco tato položka je nepovinná, poskytování důvod pomůže Azure Backup za účelem pracovat na zpětnou vazbu a určit jejich prioritu scénáře zákazníka.
4. Klikněte na **odeslání** tlačítko Odeslat **zastavit ochranu** úlohy. Klikněte na **zobrazit úlohy** zobrazíte úlohu v odpovídající **úlohy** stránky.

    ![Zastavení ochrany](./media/backup-azure-manage-vms/stop-protect-success.png)

    Pokud nevyberete **odstranit související zálohovaná data** možnost během **zastavit ochranu** průvodce a po dokončení úlohy post, stav ochrany se změní na **ukončena ochrana**. Data zůstávají s Azure Backup, dokud je explicitně odstranit. Data můžete odstranit vždy tak, že vyberete virtuální počítač **chráněné položky** stránky a kliknutím na **odstranit**.

    ![Ukončeno ochrany](./media/backup-azure-manage-vms/protection-stopped-status.png)

    Pokud jste vybrali **odstranit související zálohovaná data** možnost, virtuální počítač nebude součástí **chráněné položky** stránky.

## <a name="re-protect-virtual-machine"></a>Znovu nastavit ochranu virtuálního počítače
Pokud nevyberete **odstranění přidružení zálohovaná data** možnost **zastavit ochranu**, můžete znovu nastavit ochranu virtuálního počítače pomocí kroků podobná zálohování registrované virtuálních počítačů. Jakmile chráněný, tento virtuální počítač bude mít zálohovaná data zachována před zastavení ochrany a vytvoří body obnovení po se znovu nastavit ochranu.

Po znovu nastavit ochranu, se změní stav ochrany virtuálního počítače na **chráněné** Pokud existují bodů obnovení před **zastavit ochranu**.

  ![Vytvořit ochranu virtuálního počítače](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> Při znovu ochranu virtuálního počítače, můžete zvolit jinou zásadu než zásada, ke které byl virtuální počítač původně chráněný.
>
>

## <a name="unregister-virtual-machines"></a>Zrušit registraci virtuálních počítačů
Pokud chcete odebrat virtuální počítač z trezoru záloh:

1. Klikněte na **UNREGISTER** tlačítko v dolní části stránky.

    ![Zakažte ochranu](./media/backup-azure-manage-vms/unregister-button.png)

    Informační zpráva se zobrazí v dolní části obrazovky žádají o potvrzení. Klikněte na tlačítko **Ano** pokračujte.

    ![Zakažte ochranu](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a>Odstranění dat zálohy
Můžete odstranit záložní data související s virtuálním počítačem, buď:

* Během úlohu zastavení ochrany
* Po zastavení ochrany dokončení úlohy na virtuálním počítači

Odstranit záložní data na virtuálním počítači, který je v *ukončena ochrana* stavu post úspěšné dokončení **zastavení zálohování** úlohy:

1. Přejděte na **chráněné položky** a vyberte **virtuální počítač Azure** jako *typ* a klikněte na tlačítko **vyberte** tlačítko.

    ![Typ virtuálního počítače](./media/backup-azure-manage-vms/vm-type.png)
2. Vyberte virtuální počítač. Virtuální počítač bude v **ukončena ochrana** stavu.

    ![Zastavení ochrany](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. Klikněte **odstranit** tlačítko v dolní části stránky.

    ![Odstranit zálohování](./media/backup-azure-manage-vms/delete-backup.png)
4. V **odstranit záložní data** průvodce, vyberte důvod pro odstraňují se záložní data (důrazně doporučujeme) a klikněte na **odeslání**.

    ![Odstranit záložní data](./media/backup-azure-manage-vms/delete-backup-data.png)
5. Tím se vytvoří úlohu odstranit záložní data vybraný virtuální počítač. Klikněte na tlačítko **zobrazit úlohy** zobrazíte odpovídající úlohy na stránce úloh.

    ![Úspěšné odstranění dat](./media/backup-azure-manage-vms/delete-data-success.png)

    Po dokončení úlohy na záznam odpovídající virtuální počítač se odebere z **chráněné položky** stránky.

## <a name="dashboard"></a>Řídicí panel
Na **řídicí panel** stránce můžete zkontrolovat informace o virtuálních počítačích Azure, jejich úložiště a úlohy spojené s nimi za posledních 24 hodin. Můžete zobrazit stav zálohování a všechny související chyby zálohování.

![Řídicí panel](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> Hodnoty v řídicím panelu se aktualizuje každých 24 hodin.
>
>

## <a name="auditing-operations"></a>Auditování operací
Azure backup poskytuje kontrolu "operace protokoly" operací zálohování aktivovány zákazníka, což usnadňuje najdete v části přesně byly provedeny jaké operace správy v trezoru záloh. Protokoly operací povolit skvělé postmortální a auditování podpory u operací zálohování.

Protokoly operací přihlášeni následující operace:

* Registrace
* Zrušit registraci
* Konfigurace ochrany
* Zálohování (obě plánované i zálohování na vyžádání prostřednictvím BackupNow)
* Obnovení
* Zastavení ochrany
* Odstranit záložní data
* Přidání zásad
* Odstranit zásadu
* Aktualizovat zásady
* Zrušení úlohy

Chcete-li zobrazit protokoly operací odpovídající do trezoru záloh:

1. Přejděte na **služby pro** v portálu Azure a pak klikněte na tlačítko **protokoly operací** kartě.

    ![Protokoly operací](./media/backup-azure-manage-vms/ops-logs.png)
2. Filtry, vyberte **zálohování** jako *typ* a zadejte název úložiště záloh v *název služby* a klikněte na **odeslání**.

    ![Operace protokoly filtru](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. V protokolech operací, vyberte všechny operace a klikněte na **podrobnosti** zobrazíte podrobnosti odpovídající operace.

    ![Podrobnosti operace načítání protokolů](./media/backup-azure-manage-vms/ops-logs-details.png)

    **Podrobnosti průvodce** obsahuje informace o operaci aktivuje úloha s Id prostředku, na které tato operace se aktivuje a počáteční čas operace.

    ![Podrobnosti o operaci](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a>Oznámení o výstrahách
Vlastní oznámení výstrah pro úlohy můžete získat na portálu. Toho dosáhnete tak, že definujete pravidla výstrah pomocí prostředí PowerShell operační protokoly událostí. Doporučujeme používat *prostředí PowerShell, verze 1.3.0 nebo vyšší*.

Pokud chcete definovat vlastní oznámení a výstrahy pro selhání zálohování, bude vypadat Ukázkový příkaz:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

**ResourceId**: vám to z protokolů Operations místní podle popisu ve výše části. ResourceUri v automaticky otevíraném okně Podrobnosti operace je ResourceId poskytované pro tuto rutinu.

**OperationName**: to bude mít formát "Microsoft.Backup/backupvault/<EventName>" EventName jsou jedním z registru, Unregister, ConfigureProtection, zálohování, obnovení, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy

**Stav**: podporované hodnoty jsou – Začínáme, úspěšná a se nezdařilo.

**ResourceGroup**: ResourceGroup prostředku, na kterém se spustí operaci. To můžete získat od ResourceId hodnota. Hodnota mezi poli */resourceGroups/* a */providers/* v ResourceId hodnota je hodnota ResourceGroup.

**Název**: název pravidla výstrahy.

**CustomEmail**: Zadejte vlastní e-mailovou adresu, na který chcete odeslat oznámení výstrah

**SendToServiceOwners**: tuto možnost odesílá oznámení výstrah pro všechny správce a spolusprávci předplatného. Je možné v **New-AzureRmAlertRuleEmail** rutiny

### <a name="limitations-on-alerts"></a>Omezení výstrahy
Výstrahy na základě událostí se podrobí následující omezení:

1. Výstrahy se spouštějí na všechny virtuální počítače v trezoru záloh. Nelze přizpůsobit ho k získání výstrah pro konkrétní sadu virtuálních počítačů v trezoru záloh.
2. Tato funkce je ve verzi Preview. [Další informace](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Zobrazí se výstrahy z "alerts-noreply@mail.windowsazure.com". Momentálně nelze upravit odesílatelem e-mailu.

## <a name="next-steps"></a>Další kroky
* [Obnovení virtuálních počítačů Azure](backup-azure-restore-vms.md)
