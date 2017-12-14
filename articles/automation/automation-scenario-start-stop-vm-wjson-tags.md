---
title: "Naplánování stav virtuálního počítače Azure pomocí formátu JSON značky | Microsoft Docs"
description: "Tento článek ukazuje, jak použít řetězce formátu JSON na značky k automatizaci plánování virtuálních počítačů při spuštění a vypnutí."
services: automation
documentationcenter: 
author: georgewallace
manager: jwhit
editor: tysonn
ms.assetid: 6afed5d2-e939-4749-8b2c-9312b4c16fb2
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: magoedte;paulomarquesc
ms.openlocfilehash: 9855921f4a3aa9cda8497b400d50a186d7162dc3
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="azure-automation-scenario-using-json-formatted-tags-to-create-a-schedule-for-azure-vm-startup-and-shutdown"></a>Azure scénář automatizace: použití formátu JSON značek k vytvoření plánu pro virtuální počítač Azure spuštění a vypnutí
Zákazníci chtějí často naplánovat spuštění a vypnutí virtuálních počítačů můžete snížit náklady na předplatné nebo podporu obchodních a technických požadavků.

V následujícím scénáři můžete nastavit automatické spuštění a vypnutí virtuální počítače pomocí značku plán na úrovni skupiny prostředků nebo úrovni virtuálního počítače v Azure. Tento plán lze nakonfigurovat od neděle do soboty času spuštění a čas ukončení.

Máme některé možnosti se na pole. Mezi ně patří:

* [Sady škálování virtuálního počítače](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) s nastavení automatického škálování, které vám umožní škálování příchozí nebo odchozí.
* [DevTest Labs](../devtest-lab/devtest-lab-overview.md) služby, která obsahuje integrovanou schopnost plánování operace spuštění a vypnutí.

Ale tyto možnosti podporují pouze konkrétní scénáře a nelze ji použít k virtuálním počítačům infrastruktury jako služba (IaaS).

Plán značky se použijí pro skupinu prostředků, se taky použije pro všechny virtuální počítače v příslušné skupině prostředků. Pokud plán platí také přímo k virtuálnímu počítači, poslední plán má přednost před v následujícím pořadí:

1. Plán použijí pro skupinu prostředků
2. Plán u virtuálního počítače ve skupině prostředků a skupina prostředků
3. Plán pro virtuální počítač

Tento scénář v podstatě přebírá řetězec formátu JSON s zadaný formát a přidá ji jako hodnotu pro značku plán. Potom runbook jsou uvedeny všechny skupiny prostředků a virtuální počítače a identifikuje plány pro každý virtuální počítač, na základě scénářů uvedena i výše. V dalším prochází virtuální počítače, které mají plány připojen a vyhodnotí, jakou akci by měla být provedena. Například se určuje, které virtuální počítače je potřeba zastavit, vypnout nebo ignorovat.

Tyto sady runbook ověřit pomocí [účet spustit v Azure jako](automation-sec-configure-azure-runas-account.md).

## <a name="download-the-runbooks-for-the-scenario"></a>Stáhnout runbooky pro scénář
Tento scénář se skládá ze čtyř runbooky pracovních postupů Powershellu, které si můžete stáhnout z [Galerii TechNet](https://gallery.technet.microsoft.com/Azure-Automation-Runbooks-84f0efc7) nebo [Githubu](https://github.com/paulomarquesdacosta/azure-automation-scheduled-shutdown-and-startup) úložiště pro tento projekt.

| Runbook | Popis |
| --- | --- |
| Test ResourceSchedule |Zkontroluje každý plán virtuální počítač a provede vypínání nebo spouštění podle plánu. |
| Přidat ResourceSchedule |Přidá značku plán pro skupinu virtuálních počítačů nebo prostředku. |
| Aktualizace ResourceSchedule |Upravuje existující plán značky tak, že nahradíte novým připojením. |
| Odebrat ResourceSchedule |Značky plán odebere ze skupiny virtuálních počítačů nebo prostředku. |

## <a name="install-and-configure-this-scenario"></a>Instalace a konfigurace tohoto scénáře
### <a name="install-and-publish-the-runbooks"></a>Instalace a zveřejnění runbooků
Po stažení sady runbook, můžete je importovat pomocí postupu v [vytvoření nebo import runbooku ve službě Azure Automation](automation-creating-importing-runbook.md#importing-a-runbook-from-a-file-into-azure-automation).  Každá sada runbook publikujte po byl úspěšně importován do vašeho účtu Automation.

### <a name="add-a-schedule-to-the-test-resourceschedule-runbook"></a>Přidání do runbooku ResourceSchedule testovací plán
Použijte následující postup povolení plán pro testovací ResourceSchedule runbook. Toto je sada runbook, která ověřuje, které virtuální počítače by měla být spuštěna, vypnutí nebo ponechán beze.

1. Z portálu Azure otevřete účet Automation a klikněte **Runbooky** dlaždici.
2. Na **Test ResourceSchedule** okně klikněte na tlačítko **plány** dlaždici.
3. Na **plány** okně klikněte na tlačítko **přidat plán**.
4. Na **plány** vyberte **propojit plán s runbookem**. Potom vyberte **vytvořte nový plán**.
5. Na **nový plán** okno, zadejte název tohoto plánu, například: *HourlyExecution*.
6. Pro plán **spustit**, nastavte hodnotu doby spuštění na vyšší než hodinu.
7. Vyberte **opakování**a pak pro **opakovat každých interval**, vyberte **1 hodina**.
8. Ověřte, že **nastavit dobu platnosti** je nastaven na **ne**a potom klikněte na **vytvořit** uložit nový plán.
9. Na **plán Runbook** okno Možnosti, vyberte **nastavení parametrů a běhu**. V testu-ResourceSchedule **parametry** okno, zadejte název svého předplatného v **Název_předplatného** pole.  Toto je jediný parametr, který vyžaduje pro sadu runbook.  Až budete hotovi, klikněte na tlačítko **OK**.

Až se dokončí, plán sad runbook by měl vypadat takto:

![Nakonfigurované ResourceSchedule testu runbooku](./media/automation-scenario-start-stop-vm-wjson-tags/automation-schedule-config.png)<br>

## <a name="format-the-json-string"></a>Formátování řetězce formátu JSON
Toto řešení v podstatě trvá řetězce s zadaný formát JSON a přidá ji jako hodnotu pro značku volá plán. Potom runbook jsou uvedeny všechny skupiny prostředků a virtuální počítače a identifikuje plány pro každý virtuální počítač.

Sada runbook cyklicky prochází přes virtuální počítače, které mají plány připojen a zkontroluje, jaké akce by měla být provedena. Následuje příklad formátování řešení:

```json
{
    "TzId": "Eastern Standard Time",
    "0": {
        "S": "11",
        "E": "17"
    },
    "1": {
        "S": "9",
        "E": "19"
    },
    "2": {
        "S": "9",
        "E": "19"
    },
}
```

Zde jsou některé podrobné informace o tuto strukturu:

1. Formát tato struktura JSON je optimalizovaná tak, aby obejít omezení na 256 znaků jedinou značku hodnoty v Azure.
2. *TzId* představuje časové pásmo pro virtuální počítač. Toto ID můžete získat pomocí třídě TimeZoneInfo .NET v relaci prostředí PowerShell –**[System.TimeZoneInfo]:: GetSystemTimeZones()**.

   ![GetSystemTimeZones v prostředí PowerShell](./media/automation-scenario-start-stop-vm-wjson-tags/automation-get-timzone-powershell.png)

   * Dny v týdnu jsou reprezentované pomocí číselnou hodnotu nula na šest. Neděle se rovná hodnotě nula.
   * Čas spuštění je reprezentována pomocí **S** atribut a jeho hodnota je ve 24hodinovém formátu.
   * Čas end nebo ukončení je reprezentována pomocí **E** atribut a jeho hodnota je ve 24hodinovém formátu.

     Pokud **S** a **E** atributy každý mít hodnotu nula (0), virtuální počítač zůstane za současného stavu v době vyhodnocení.
3. Pokud chcete nechat přeskočit vyhodnocení pro určitý den v týdnu, nepřidáte oddíl pro daný den v týdnu. V následujícím příkladu je vyhodnotit pouze pondělí a ignorují jiných dny v týdnu:

    ```json
    {
        "TzId": "Eastern Standard Time",
        "1": {
            "S": "11",
            "E": "17"
        }
    }
    ```

## <a name="tag-resource-groups-or-vms"></a>Skupiny prostředků značka nebo virtuální počítače
Vypnout virtuální počítače, musíte označit virtuální počítače nebo skupiny prostředků, ve kterých se nachází. Virtuální počítače, které nemají značku plán nevyhodnocují. Proto není spuštěna nebo vypnout.

Existují dva způsoby skupiny prostředků značka nebo virtuální počítače s tímto řešením. Můžete provést přímo z portálu. Nebo můžete přidat ResourceSchedule, aktualizace ResourceSchedule a odebrat ResourceSchedule sady runbook.

### <a name="tag-through-the-portal"></a>Značka prostřednictvím portálu
Postupujte podle těchto kroků k označení virtuální počítač nebo skupinu prostředků na portálu:

1. Vyrovnání řetězce formátu JSON a ověřte, že nejsou žádné mezery.  Řetězec vašeho JSON by měl vypadat takto:

    ```json
    {"TzId":"Eastern Standard Time","0":{"S":"11","E":"17"},"1":{"S":"9","E":"19"},"2": {"S":"9","E":"19"},"3":{"S":"9","E":"19"},"4":{"S":"9","E":"19"},"5":{"S":"9","E":"19"},"6":{"S":"11","E":"17"}}
    ```

2. Vyberte **značky** ikonu pro skupinu virtuálních počítačů nebo prostředek použít tento plán.

   ![Možnosti značky virtuálních počítačů](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-tag-option.png)

3. Značky jsou definovány následující dvojici klíč/hodnota. Typ **plán** v **klíč** pole a vložte do řetězce formátu JSON **hodnotu** pole. Klikněte na **Uložit**. Novou značku by se měla zobrazit v seznamu značky prostředku.

   ![Virtuální počítač plán značky](./media/automation-scenario-start-stop-vm-wjson-tags/automation-vm-schedule-tag.png)

### <a name="tag-from-powershell"></a>Značka z prostředí PowerShell
Všechny importované sady runbook obsahují informace nápovědy na začátku skriptu, který popisuje, jak při spuštění sady runbook přímo z prostředí PowerShell. Přidat ScheduleResource a ScheduleResource aktualizace sad runbook můžete volat z prostředí PowerShell. Uděláte to pomocí předání požadované parametry, které vám umožní vytvořit nebo aktualizovat plán značky pro skupinu virtuálních počítačů nebo prostředek mimo portál.

Pokud chcete vytvořit, přidat a odstranit značky pomocí prostředí PowerShell, nejprve musíte [nastavení prostředí PowerShell pro Azure](/powershell/azure/overview). Po dokončení instalace, můžete pomocí následujících kroků.

### <a name="create-a-schedule-tag-with-powershell"></a>Vytvoření plánu značku pomocí prostředí PowerShell
1. Otevřete relaci prostředí PowerShell. Pak použijte následující příklad k ověření pomocí účtu spustit jako a zadejte předplatné:

    ```powershell
    $Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Definujte plán zatřiďovací tabulku. Tady je příklad, jak by měla zkonstruovat:

    ```powershell
    $schedule= @{ "TzId"="Eastern Standard Time"; "0"= @{"S"="11";"E"="17"};"1"= @{"S"="9";"E"="19"};"2"= @{"S"="9";"E"="19"};"3"= @{"S"="9";"E"="19"};"4"= @{"S"="9";"E"="19"};"5"= @{"S"="9";"E"="19"};"6"= @{"S"="11";"E"="17"}}
    ```

3. Zadejte parametry, které jsou vyžadovány sady runbook. V následujícím příkladu jsme cílení virtuálního počítače:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "VmName"="VM01";"Schedule"=$schedule}
    ```

    Pokud jste označování skupinu prostředků, odeberte *VMName* parametr z hodnoty hash $params tabulky následujícím způsobem:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"; "Schedule"=$schedule}
    ```

4. Spuštění sady runbook přidat ResourceSchedule s následujícími parametry pro vytvoření plán značky:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Add-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

5. Chcete-li aktualizovat skupinu prostředků nebo značka virtuálního počítače, spusťte **aktualizace ResourceSchedule** runbook s následujícími parametry:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Update-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

### <a name="remove-a-schedule-tag-with-powershell"></a>Odebrání značky plánu pomocí prostředí PowerShell
1. Otevřete relaci prostředí PowerShell a spusťte následující příkaz k ověření pomocí účtu spustit jako a a vyberte a zadejte předplatné:

    ```powershell
    Conn = Get-AutomationConnection -Name AzureRunAsConnection
    Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    Select-AzureRmSubscription -SubscriptionName "MySubscription"
    ```

2. Zadejte parametry, které jsou vyžadovány sady runbook. V následujícím příkladu jsme cílení virtuálního počítače:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01";"VmName"="VM01"}
    ```

    Pokud odstraňujete značku ze skupiny prostředků, odeberte *VMName* parametr z hodnoty hash $params tabulky následujícím způsobem:

    ```powershell
    $params = @{"SubscriptionName"="MySubscription";"ResourceGroupName"="ResourceGroup01"}
    ```

3. Spuštění sady runbook odebrat ResourceSchedule odebrat plán značky:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

4. Chcete-li aktualizovat skupinu prostředků nebo značky virtuální počítač, spusťte odebrat ResourceSchedule runbook s následujícími parametry:

    ```powershell
    Start-AzureRmAutomationRunbook -Name "Remove-ResourceSchedule" -Parameters $params `
    -AutomationAccountName "AutomationAccount" -ResourceGroupName "ResourceGroup01"
    ```

> [!NOTE]
> Doporučujeme aktivně sledovat tyto sady runbook (a stavech virtuálních počítačů), chcete-li ověřit, že virtuální počítače jsou vypínán dolů a spuštění odpovídajícím způsobem.
>

Chcete-li zobrazit podrobnosti o ResourceSchedule testovací úlohy runbooku na portálu Azure, vyberte **úlohy** dlaždice sady runbook. V souhrnu úlohy se zobrazí vstupní parametry a výstupní datový proud a také obecné informace o příslušné úloze a případné výjimky, které nastaly.

**Souhrn úlohy** zahrnuje zprávy z datových proudů výstupu, upozornění a chyb. Pokud chcete zobrazit podrobné výsledky spuštění runbooku, vyberte dlaždici **Výstup**.

![Výstup testu ResourceSchedule](./media/automation-scenario-start-stop-vm-wjson-tags/automation-job-output.png)

## <a name="next-steps"></a>Další kroky
* První kroky s runbooky pracovních postupů PowerShellu najdete v článku [Můj první runbook pracovního postupu PowerShellu](automation-first-runbook-textual.md).
* Další informace o typech runbooků a jejich výhody a omezení, najdete v části [typy runbooků ve službě Azure Automation](automation-runbook-types.md).
* Další informace o funkcích podpory skript prostředí PowerShell najdete v tématu [nativní podpora Powershellových skriptů ve službě Azure Automation](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/).
* Další informace o protokolování sad runbook a výstup, najdete v části [Runbook výstupu a zpráv ve službě Azure Automation](automation-runbook-output-and-messages.md).
* Další informace o účtu spustit v Azure jako a ověřit svoje runbooky použití naleznete v tématu [ověření runbooků pomocí účtu spustit v Azure jako](automation-sec-configure-azure-runas-account.md).
