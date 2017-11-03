---
title: "Zálohy virtuálních počítačů nasazených Resource Managerem monitorování | Microsoft Docs"
description: "Monitorování události a upozornění ze záloh virtuálních počítačů nasazených Resource Managerem. Odeslání e-mailu na základě výstrah."
services: backup
documentationcenter: dev-center-name
author: markgalioto
manager: carmonm
editor: 
ms.assetid: fed32015-2db2-44f8-b204-d89f6fd1bea2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2016
ms.author: markgal;trinadhk;giridham;
ms.openlocfilehash: b9dc3f52e5fc275bc56b9964f2115833f2dde42e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-alerts-for-azure-virtual-machine-backups"></a>Správa výstrah pro virtuální počítače Azure
Výstrahy jsou odpovědi ze služby, aby byla splněny nebo překročení prahová hodnota události. Zároveň budete vědět, když může být kritické nízkými náklady obchodní problémy spuštění. Výstrahy obvykle nedojde k podle plánu, a proto je užitečné vědět, co nejdříve po generována výstraha. Například pokud se nezdaří úlohy zálohování nebo obnovení, zobrazení výstrahy do pěti minut selhání. Na řídicím panelu trezoru na dlaždici zálohování výstrahy zobrazuje kritická a úroveň pro upozornění události. V nastavení zálohování výstrah můžete zobrazit všechny události. Ale co dělat v případě výstrahu při práci na samostatné problém? Pokud si nejste jisti, když se stane, výstrahy, může to být méně závažné potíže, nebo ji mohl ohrozit zabezpečení dat. Pokud chcete mít jistotu, že oprávnění uživatelé by se měl dozvědět výstrahy – když dojde, nakonfigurujte službu pro odeslání oznámení o výstrahách e-mailem. Podrobnosti o nastavení e-mailová oznámení najdete v tématu [konfigurace oznámení](backup-azure-monitor-vms.md#configure-notifications).

## <a name="how-do-i-find-information-about-the-alerts"></a>Jak se najít informace o výstrahách?
K zobrazení informací o události, která způsobila výstrahu, je nutné otevřít okno zálohování výstrahy. Existují dva způsoby, otevřete okno zálohování výstrahy: některý z výstrah zálohování dlaždici na řídicím panelu trezoru, nebo v okně výstrahy a události.

Otevřete okno zálohování výstrahy z dlaždice výstrahy zálohování:

* Na **zálohování výstrahy** dlaždici na řídicím panelu trezoru, klikněte na tlačítko **kritický** nebo **upozornění** Chcete-li zobrazit provozní události pro tuto úroveň závažnosti.

    ![Dlaždice výstrahy zálohy](./media/backup-azure-monitor-vms/backup-alerts-tile.png)

Otevřete okno zálohování výstrahy z okna výstrahy a události:

1. Na řídicím panelu trezoru, klikněte na tlačítko **všechna nastavení**. ![Tlačítko všechna nastavení](./media/backup-azure-monitor-vms/all-settings-button.png)
2. Na **nastavení** okně klikněte na tlačítko **výstrahy a události**. ![Tlačítko výstrahy a události](./media/backup-azure-monitor-vms/alerts-and-events-button.png)
3. Na **výstrahy a události** okně klikněte na tlačítko **zálohování výstrahy**. ![Zálohování tlačítko výstrahy](./media/backup-azure-monitor-vms/backup-alerts.png)

    **Zálohování výstrahy** okno otevře a zobrazí filtrované výstrahy.

    ![Dlaždice výstrahy zálohy](./media/backup-azure-monitor-vms/backup-alerts-critical.png)
4. Chcete-li zobrazit podrobné informace o určité výstraze, ze seznamu události, klikněte na výstrahu otevřete jeho **podrobnosti** okno.

    ![Podrobnosti události](./media/backup-azure-monitor-vms/audit-logs-event-detail.png)

    Přizpůsobení atributů zobrazit v seznamu, najdete v části [zobrazit další událost atributy](backup-azure-monitor-vms.md#view-additional-event-attributes)

## <a name="configure-notifications"></a>Konfigurace oznámení
 Můžete nakonfigurovat službu pro odeslání e-mailová oznámení pro výstrahy, které přes poslední hodinu, nebo pokud dojde k určité typy událostí došlo k chybě.

Nastavení e-mailová oznámení pro výstrahy

1. V nabídce výstrahy zálohování, klikněte na tlačítko **konfigurace oznámení**

    ![Zálohování nabídky výstrahy](./media/backup-azure-monitor-vms/backup-alerts-menu.png)

    Otevře se okno oznámení konfigurace.

    ![Konfigurace oznámení okno](./media/backup-azure-monitor-vms/configure-notifications.png)
2. V okně Konfigurace oznámení pro e-mailová oznámení, klikněte na **na**.

    Příjemci a závažnost dialogová okna mají hvězdu vedle jejich, protože tyto informace je vyžadován. Zadejte aspoň jednu e-mailovou adresu a vyberte alespoň jeden závažnosti.
3. V **příjemce (e-mailu)** dialogové okno, zadejte e-mailové adresy, pro který dostávat oznámení. Použijte formát: username@domainname.com. Jednotlivé e-mailové adresy oddělte středníkem (;).
4. V **oznámení** oblasti, zvolte **za výstraha** k odeslání oznámení, když dojde k zadané výstrahy, nebo **hodinové Digest** k odeslání souhrn za poslední hodinu.
5. V **závažnost** dialogovém okně, vyberte jeden nebo více úrovní, které chcete aktivovat e-mailové oznámení.
6. Klikněte na **Uložit**.

   ### <a name="what-alert-types-are-available-for-azure-iaas-vm-backup"></a>Jaké typy výstrah jsou k dispozici pro zálohování virtuálních počítačů Azure IaaS?
   | Úroveň výstrahy | Zasílání upozornění |
   | --- | --- |
   | Kritické |Selhání zálohování, obnovení selhání |
   | Upozornění |Žádný |
   | Informační |Žádný |

### <a name="are-there-situations-where-email-isnt-sent-even-if-notifications-are-configured"></a>Dochází k situacím, že se e-mail neodešle, i když jsou oznámení nakonfigurovaná?
Existují situacích, kde se neposílají výstrahu, i když oznámení správně nakonfigurovaný. V následujících situacích e-mailu s oznámení neodešlou předejdete výstrahy nepůsobily:

* Pokud oznámení jsou nakonfigurovány pro hodinové Digest, a je výstraha vyvolána a vyřešit v rámci hodiny.
* Úloha je zrušena.
* Úloha zálohování se aktivuje a pak se nezdaří a probíhá další úloha zálohování.
* Spustí naplánované úlohy zálohování pro virtuální počítač povolena Resource Manager, ale virtuální počítač už existuje.

## <a name="customize-your-view-of-events"></a>Přizpůsobení zobrazení událostí
**Protokoly auditu** nastavení obsahuje předem definovaná sadu filtrů a sloupce, které zobrazuje informace o provozní události. Můžete přizpůsobit zobrazení, který po **události** otevře se okno, zobrazuje informace, které mají.

1. V [panelu trezoru](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), vyhledejte a klikněte na **protokoly auditu** otevřete **události** okno.

    ![Protokoly auditu](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    **Události** otevře se okno pro provozní události filtrované právě pro aktuální trezoru.

    ![Filtr protokolů auditu](./media/backup-azure-monitor-vms/audit-logs-filter.png)

    V okně zobrazuje seznam kritický, chyby, upozornění a informační události, které došlo k chybě minulého týdne. Časové rozpětí je výchozí hodnotu nastavenou v **filtru**. **Události** okno také ukazuje pruhový graf sledování při výskytu události. Pokud nechcete, aby zobrazíte pruhový graf v **události** nabídky, klikněte na tlačítko **skrýt graf** k přepnutí vypnout grafu. Výchozí zobrazení událostí zobrazuje informace o operaci, úroveň, stav, prostředků a času. Informace o vystavení další atributy, které události, najdete v části [rozšíření informací o události](backup-azure-monitor-vms.md#view-additional-event-attributes).
2. Další informace o provozních událostí v **operace** sloupce, klikněte na tlačítko provozních událostí otevřete její okno. Okno obsahuje podrobné informace o událostech. Události jsou seskupené podle jejich ID korelace a seznam událostí, které nastaly v časové rozpětí.

    ![Podrobnosti o operaci](./media/backup-azure-monitor-vms/audit-logs-details-window.png)
3. Chcete-li zobrazit podrobné informace o určité události, ze seznamu události, klikněte na událost otevřete jeho **podrobnosti** okno.

    ![Podrobnosti události](./media/backup-azure-monitor-vms/audit-logs-details-window-deep.png)

    Je také podrobné informace získá informace o úroveň události. Pokud raději prohlížet tomto velkého množství informací o jednotlivých událostí a chcete přidat tuto množství podrobností, které se **události** okně najdete v části [rozšíření informací o události](backup-azure-monitor-vms.md#view-additional-event-attributes).

## <a name="customize-the-event-filter"></a>Přizpůsobení filtr událostí
Použití **filtru** upravit nebo vyberte informace, které se zobrazí v konkrétní okno. Pro filtrování informací o události:

1. V [panelu trezoru](backup-azure-manage-vms.md#open-a-recovery-services-vault-in-the-dashboard), vyhledejte a klikněte na **protokoly auditu** otevřete **události** okno.

    ![Protokoly auditu](./media/backup-azure-monitor-vms/audit-logs-1606-1.png)

    **Události** otevře se okno pro provozní události filtrované právě pro aktuální trezoru.

    ![Filtr protokolů auditu](./media/backup-azure-monitor-vms/audit-logs-filter.png)
2. Na **události** nabídky, klikněte na tlačítko **filtru** k otevření tohoto okna.

    ![Otevřete okno filtru](./media/backup-azure-monitor-vms/audit-logs-filter-button.png)
3. Na **filtru** okně Upravit **úroveň**, **časové rozpětí**, a **volající** filtry. Ostatní filtry nejsou k dispozici, protože byly nastavené zajistit aktuální informace o trezoru služeb zotavení.

    ![Podrobnosti o dotazu protokoly auditu](./media/backup-azure-monitor-vms/filter-blade.png)

    Můžete zadat **úroveň** události: kritická, chyba, varování nebo informační. Můžete zvolit libovolnou kombinaci úrovní událostí, ale musí mít nejméně jedna vybraná úroveň. Úroveň zapnout nebo vypnout. **Časové rozpětí** filtru můžete zadat dobu pro zachycení událostí. Pokud používáte vlastní časové období, můžete nastavit počáteční a koncový čas.
4. Až budete připravení dotaz protokolů operací pomocí filtru, klikněte na **aktualizace**. Ve výsledcích se zobrazí v **události** okno.

    ![Podrobnosti o operaci](./media/backup-azure-monitor-vms/edited-list-of-events.png)

### <a name="view-additional-event-attributes"></a>Zobrazení událostí další atributy
Pomocí **sloupce** tlačítko, můžete povolit v seznamu se zobrazí na další událost atributy **události** okno. Výchozí seznam událostí zobrazí informace o operaci, úroveň, stav, prostředků a času. Chcete-li povolit další atributy:

1. Na **události** okně klikněte na tlačítko **sloupce**.

    ![Otevřete sloupců](./media/backup-azure-monitor-vms/audi-logs-column-button.png)

    **Zvolit sloupce** otevře se okno.

    ![Okno sloupce](./media/backup-azure-monitor-vms/columns-blade.png)
2. Pokud chcete vybrat atribut, klikněte na zaškrtávací políčko. Zaškrtávací políčko atribut přepne zapnout a vypnout.
3. Klikněte na tlačítko **resetovat** resetovat seznam atributů v **události** okno. Po přidání nebo odebrání atributů ze seznamu, použijte **resetovat** zobrazíte nový seznam událostí atributy.
4. Klikněte na tlačítko **aktualizace** aktualizujte data události atributy. Následující tabulka obsahuje informace o každý atribut.

| Název sloupce | Popis |
| --- | --- |
| Operace |Název operace |
| Úroveň |Úroveň operace hodnoty mohou být: informační, upozornění, chyby nebo kritický |
| Status |Popisný stav operace |
| Prostředek |Adresa URL, která identifikuje prostředek; také označované jako ID prostředku |
| Čas |Čas, měřenou z aktuální čas, kdy došlo k události |
| Volající |Kdo nebo co názvem nebo aktivuje událost. může být systém nebo uživatel |
| časové razítko |Čas, kdy byla aktivována událost |
| Skupina prostředků |Skupina přidružených prostředků |
| Typ prostředku |Interní typ prostředku používaný správcem prostředků |
| ID předplatného |ID související předplatného |
| Kategorie |Kategorie události |
| ID korelace |ID společné souvisejících událostí |

## <a name="use-powershell-to-customize-alerts"></a>Přizpůsobení upozornění pomocí prostředí PowerShell
Vlastní oznámení výstrah pro úlohy můžete získat na portálu. Chcete-li získat tyto úlohy, definujte pravidla výstrah pomocí prostředí PowerShell operační protokoly událostí. Použití *prostředí PowerShell, verze 1.3.0 nebo novější*.

Pokud chcete definovat vlastní oznámení a výstrahy pro selhání zálohování, použijte příkaz jako následující skript:

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.RecoveryServices/recoveryServicesVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/Microsoft.RecoveryServices/vaults/trinadhVault -Actions $actionEmail
```

**ResourceId** : ResourceId můžete získat z protokolů auditu. ID prostředku není zadaná ve sloupci Zdroj protokolů operaci adresa URL.

**OperationName** : OperationName je ve formátu "Microsoft.RecoveryServices/recoveryServicesVault/*EventName*" kde *EventName* může být:<br/>

* Registrace <br/>
* Zrušit registraci <br/>
* ConfigureProtection <br/>
* Zálohování <br/>
* Obnovení <br/>
* StopProtection <br/>
* DeleteBackupData <br/>
* CreateProtectionPolicy <br/>
* DeleteProtectionPolicy <br/>
* UpdateProtectionPolicy <br/>

**Stav** : podporované hodnoty jsou Začínáme, bylo úspěšné nebo neúspěšné.

**ResourceGroup** : Toto je skupina prostředků, do které patří k prostředku. Skupina prostředků sloupec můžete přidat do protokolů vytvořených. Skupina prostředků je jeden z dostupných typů informací o události.

**Název** : název pravidla výstrahy.

**CustomEmail** : Zadejte vlastní e-mailovou adresu, na který chcete odeslat oznámení výstrahy

**SendToServiceOwners** : tuto možnost odesílá oznámení výstrah pro všechny správce a spolusprávci předplatného. Je možné v **New-AzureRmAlertRuleEmail** rutiny

### <a name="limitations-on-alerts"></a>Omezení výstrahy
Výstrahy na základě událostí se vztahují následující omezení:

1. Výstrahy se spouštějí na všechny virtuální počítače v trezoru služeb zotavení. Nelze nastavit upozornění pro podmnožiny virtuální počítače v trezoru služeb zotavení.
2. Tato funkce je ve verzi Preview. [Další informace](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. Výstrahy jsou odesílány z "alerts-noreply@mail.windowsazure.com". Momentálně nelze upravit odesílatelem e-mailu.

## <a name="next-steps"></a>Další kroky
Protokoly událostí povolit skvělé postmortální a auditování podpory u operací zálohování. Jsou zaznamenány následující operace:

* Registrace
* Zrušit registraci
* Konfigurace ochrany
* Zálohování (obě plánované i zálohování na vyžádání)
* Obnovení
* Zastavení ochrany
* Odstranit záložní data
* Přidání zásad
* Odstranit zásadu
* Aktualizovat zásady
* Zrušení úlohy

Široká vysvětlení události, operace a protokoly auditu napříč službami Azure, najdete v článku [zobrazení událostí a protokolů auditování](../monitoring-and-diagnostics/insights-debugging-with-events.md).

Informace o opětovné vytvoření virtuálního počítače z bodu obnovení, podívejte se na [obnovení virtuálních počítačů Azure](backup-azure-restore-vms.md). Pokud potřebujete informace o ochraně virtuálních počítačů, přečtěte si [první pohled: zálohování virtuálních počítačů do trezoru služeb zotavení](backup-azure-vms-first-look-arm.md). Další informace o úlohách správy pro zálohování virtuálních počítačů v článku, [záloh virtuálních počítačů Azure spravovat](backup-azure-manage-vms.md).
