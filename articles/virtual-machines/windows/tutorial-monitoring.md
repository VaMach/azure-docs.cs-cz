---
title: "Azure monitorování a aktualizace a virtuální počítače s Windows | Microsoft Docs"
description: "Kurz – monitorovat a aktualizovat virtuální počítač Windows v prostředí Azure PowerShell"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/04/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: a37aed8b3321d3518ffd73e09f5bb21266a7e577
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-update-a-windows-virtual-machine-with-azure-powershell"></a>Monitorovat a aktualizovat virtuální počítač s Windows v prostředí Azure PowerShell

Agenty Azure monitorování používá ke shromažďování dat spouštěcí a výkonu z virtuálních počítačů Azure, uložení dat této v úložišti Azure a zpřístupnit prostřednictvím portálu, modulu Azure PowerShell a rozhraní příkazového řádku Azure. Správa aktualizací můžete spravovat aktualizace a opravy pro virtuální počítače Windows Azure.

V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Povolit Diagnostika spouštění na virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazit metriky hostitele virtuálního počítače
> * Instalace rozšíření diagnostiky
> * Zobrazit metriky virtuálního počítače
> * Vytvořit výstrahu
> * Správa aktualizací Windows
> * Nastavit pokročilé monitorování

Tento kurz vyžaduje modul Azure PowerShell verze 3.6 nebo novější. Verzi zjistíte spuštěním příkazu ` Get-Module -ListAvailable AzureRM`. Pokud potřebujete upgrade, přečtěte si téma [Instalace modulu Azure PowerShell](/powershell/azure/install-azurerm-ps).

Pokud chcete provést v příkladu v tomto kurzu, musí mít existující virtuální počítač. V případě potřeby to [ukázka skriptu](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) můžete vytvořit za vás. Při práci se prostřednictvím tohoto kurzu je nahradit skupinu prostředků, název virtuálního počítače a umístění tam, kde je potřeba.

## <a name="view-boot-diagnostics"></a>Zobrazení diagnostiky spouštění

Jak spustit virtuální počítače s Windows, zaznamená agenta pro diagnostiku spouštěcí obrazovky výstupu, který lze použít pro účely odstraňování potíží. Tato funkce je ve výchozím nastavení povolené. Snímky obrazovky zaznamenané jsou uložené v účtu úložiště Azure, který se také vytvoří ve výchozím nastavení. 

Můžete získat spouštění diagnostiky dat pomocí [Get-AzureRmVMBootDiagnosticsData](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmbootdiagnosticsdata) příkaz. V následujícím příkladu se Diagnostika spouštění stáhnou do kořenového adresáře * c:\* jednotky. 

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup -Name myVM -Windows -LocalPath "c:\"
```

## <a name="view-host-metrics"></a>Zobrazit hostitele metriky

Virtuální počítač s Windows má vyhrazený virtuální počítač hostitele v Azure, který komunikuje se službou. Metriky jsou automaticky shromažďovat pro hostitele a lze je zobrazit na portálu Azure.

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
2. Klikněte na tlačítko **metriky** v okně virtuálního počítače a potom vyberte některé z metriky hostitele pod **dostupné metriky** zobrazíte, jaký je výkon hostitele virtuálního počítače.

    ![Zobrazit hostitele metriky](./media/tutorial-monitoring/tutorial-monitor-host-metrics.png)

## <a name="install-diagnostics-extension"></a>Instalace rozšíření diagnostiky

Metriky základní hostitele jsou dostupné, ale podrobnější a metriky specifické pro virtuální počítač, budete muset nainstalovat rozšíření diagnostiky Azure ve virtuálním počítači. Rozšíření diagnostiky Azure umožňuje další funkce monitorování a diagnostická data mají být načteny z virtuálního počítače. Můžete zobrazit tyto metriky výkonu a vytvářet výstrahy založené na tom, jak se provádí virtuálního počítače. Diagnostické rozšíření nainstalovaný prostřednictvím portálu Azure následujícím způsobem:

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
2. Klikněte na tlačítko **diagnostiku nastavení**. V seznamu uvedena, který *spouštění diagnostiky* jsou již povolené z předchozí části. Klikněte na zaškrtávací políčko pro *základní metriky*.
3. Klikněte **povolit sledování na úrovni hosta** tlačítko.

    ![Zobrazit diagnostické metriky](./media/tutorial-monitoring/enable-diagnostics-extension.png)

## <a name="view-vm-metrics"></a>Zobrazit metriky virtuálního počítače

Metriky virtuálního počítače lze zobrazit stejným způsobem, že jste si zobrazili hostitele metriky virtuálního počítače:

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
2. Chcete-li zjistit, jaký je výkon virtuálního počítače, klikněte na tlačítko **metriky** v okně virtuálního počítače a potom vyberte některé z metriky diagnostiky v části **dostupné metriky**.

    ![Zobrazit metriky virtuálního počítače](./media/tutorial-monitoring/monitor-vm-metrics.png)

## <a name="create-alerts"></a>Vytváření upozornění

Můžete vytvořit na základě metriky výkonu specifických výstrah. Výstrahy lze oznámí, že jste při průměrné využití procesoru překračuje prahovou hodnotu nebo volné místo na disku klesne pod určitou částku, například. Výstrahy jsou zobrazeny v portálu Azure nebo lze odeslat e-mailem. Runbooky služby automatizace Azure nebo Azure Logic Apps můžete také aktivovat v reakci na výstrahy generován.

Následující příklad vytvoří výstrahu při průměrné využití procesoru.

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
2. Klikněte na tlačítko **výstrah pravidla** v okně virtuálního počítače klikněte **přidat metriky upozornění** v horní části okna výstrahy.
4. Zadejte **název** výstrahy, jako například *myAlertRule*
5. Spustí výstrahu, pokud procento využití procesoru překročí 1.0 pět minut, ponechte všechny ostatní výchozí nastavení vybrané.
6. Volitelně můžete zaškrtnout políčko pro *e-mailu vlastníci, přispěvatelé a čtenáři* k odesílání e-mailové oznámení. Výchozí akce je k dispozici oznámení na portálu.
7. Klikněte na tlačítko **OK**.

## <a name="manage-windows-updates"></a>Správa aktualizací Windows

Správa aktualizací můžete spravovat aktualizace a opravy pro virtuální počítače Windows Azure.
Přímo z virtuálního počítače, můžete rychle vyhodnotit stav dostupné aktualizace, naplánovat instalaci požadovaných aktualizací a zkontrolovat výsledky nasazení k ověření, že bylo úspěšně nasazení aktualizací do virtuálního počítače.

Informace o cenách naleznete v tématu [ceny automatizace správy aktualizací](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management"></a>Povolení správy aktualizací

Povolení správy aktualizací pro virtuální počítač:
 
1. Na levé straně obrazovky vyberte **virtuální počítače**.
2. V seznamu vyberte virtuální počítač.
3. Na obrazovce virtuálních počítačů v **operace** klikněte na tlačítko **Správa aktualizací**. **Povolit správu aktualizace** obrazovky otevře.

Chcete-li zjistit, zda je povoleno správy aktualizací pro tento virtuální počítač se provádí ověřování. Ověření zahrnuje kontroly pro pracovní prostor analýzy protokolů a propojené účet Automation, a pokud řešení v pracovním prostoru.

Pracovní prostor analýzy protokolů se používá ke shromažďování dat, které se generují ve funkcích a službách, například správu aktualizací. Pracovní prostor poskytuje jedno umístění ke kontrole a analyzovat data z více zdrojů. K provedení další akce na virtuálních počítačích, které vyžadují aktualizace, Azure Automation umožňuje spouštět skripty u virtuálních počítačů, například ke stažení a použití aktualizací.

Proces ověření také zkontroluje, pokud je virtuální počítač opatřen Microsoft Monitoring Agent (MMA) a hybridní pracovní proces. Tento agent se používá ke komunikaci s virtuálním Počítačem a získat informace o stavu aktualizace. 

Pokud nejsou splněny tyto požadavky, zobrazí se banner, která vám dává možnost povolit řešení.

![Aktualizovat banner zařadit configuration Management](./media/tutorial-monitoring/manageupdates-onboard-solution-banner.png)

Kliknutím na banner řešení povolte. Pokud některý z těchto předpokladů byl zjištěn chybět po ověření, budou automaticky přidány:

* [Analýza protokolu](../../log-analytics/log-analytics-overview.md) prostoru
* [Automation](../../automation/automation-offering-get-started.md)
* A [hybridní pracovní proces runbooku](../../automation/automation-hybrid-runbook-worker.md) je povoleno na virtuálním počítači

**Povolit správu aktualizace** obrazovky otevře. Nakonfigurujte nastavení a klikněte na tlačítko **povolit**.

![Povolit řešení pro správu aktualizací](./media/tutorial-monitoring/manageupdates-update-enable.png)

Povolení řešení může trvat až 15 minut a během této doby by neměl zavřete okno prohlížeče. Po povolení řešení informace o chybějící aktualizace ve virtuálním počítači jsou k analýze protokolů.
Může trvat až 30 minut, 6 hodin pro data, která mají být k dispozici pro analýzu.

### <a name="view-update-assessment"></a>Zobrazení posouzení aktualizací

Po povolení **správy aktualizací** se zobrazí obrazovka **Správa aktualizací**. Na kartě **Chybějící aktualizace** můžete zobrazit seznam chybějících aktualizací.

 ![Stav aktualizace zobrazení](./media/tutorial-monitoring/manageupdates-view-status-win.png)

### <a name="schedule-an-update-deployment"></a>Naplánování nasazení aktualizace

Pokud chcete nainstalovat aktualizace, naplánujte nasazení odpovídající vašemu plánu vydávání a časovému intervalu pro správu a údržbu.
Můžete zvolit typy aktualizací, které budou součástí nasazení. Například můžete zahrnout kritické nebo aktualizace zabezpečení a vyloučení kumulativní aktualizace.

Naplánovat nové nasazení aktualizace pro virtuální počítač kliknutím **nasazení aktualizace plánu** v horní části **Správa aktualizací** obrazovky. V **nové nasazení aktualizace** obrazovky, zadejte následující informace:

* **Název** – Zadejte jedinečný název pro identifikaci nasazení aktualizací.
* **Klasifikace aktualizací** -vyberte typy softwaru, nasazení aktualizace, které jsou zahrnuty do nasazení. Typy klasifikace jsou:
  * Důležité aktualizace
  * Aktualizace zabezpečení
  * Kumulativní aktualizace
  * Balíčky funkcí
  * Aktualizace Service Pack
  * Aktualizace definic
  * Nástroje
  * Aktualizace

* **Nastavení plánu** – Můžete přijmout výchozí datum a čas, což je 30 minut od aktuálního času, nebo zadat jiný čas.
  Můžete také určit, jestli nasazení proběhne jednou, nebo nastavit plán opakování. Pokud chcete nastavit plán opakování, klikněte na možnost Opakovat v části Opakování.

  ![Obrazovka nastavení plánu aktualizací](./media/tutorial-monitoring/manageupdates-schedule-win.png)

* **Časové období údržby (minuty)** – Zadejte časové období, ve kterém má dojít k nasazení aktualizací.  Pomůžete tím zajistit, že se změny provedou v rámci vašich definovaných časových intervalů pro správu a údržbu.

Jakmile dokončíte konfiguraci plánu, klikněte na tlačítko **Vytvořit** a vrátíte se na řídicí panel stavu.
Všimněte si, že **naplánovaná** tabulka ukazuje plán nasazení jste vytvořili.

> [!WARNING]
> Aktualizace, které vyžadují restartování je virtuální počítač automaticky restartuje.

### <a name="view-results-of-an-update-deployment"></a>Zobrazení výsledků nasazení aktualizací

Po spuštění naplánovaného nasazení se stav tohoto nasazení zobrazí na kartě **Nasazení aktualizací** na obrazovce **Správa aktualizací**.
Pokud je nasazení aktuálně spuštěno, jeho stav je **Probíhající**. Po úspěšném dokončení se změní na **Úspěch**.
Pokud u jedné nebo více aktualizací v nasazení dojde k chybě, stav je **Částečné selhání**.
Kliknutím na dokončené nasazení aktualizací zobrazíte řídicí panel pro toto nasazení aktualizací.

   ![Řídicí panel stavu nasazení aktualizací pro konkrétní nasazení](./media/tutorial-monitoring/manageupdates-view-results.png)

V **aktualizaci výsledků** dlaždice je uveden seznam celkový počet aktualizací a výsledky nasazení do virtuálního počítače.
V tabulce vpravo je podrobný rozpis všech aktualizací a výsledků instalace, které můžou mít jednu z následujících hodnot:

* **Nebyl proveden pokus o** -aktualizace nenainstalovala, protože došlo k dispozici dostatečná doba podle doba trvání okna údržby definované.
* **Úspěšné** -aktualizace byla úspěšná
* **Se nezdařilo** -aktualizace se nezdařila

Kliknutím na **Všechny protokoly** zobrazíte všechny položky protokolu, které toto nasazení vytvořilo.

Klikněte **výstup** dlaždice zobrazíte stream úloh sady runbook zodpovědní za správu nasazení aktualizací na cílovém virtuálním počítači.

Kliknutím na **Chyby** zobrazíte podrobné informace o případných chybách nasazení.

## <a name="advanced-monitoring"></a>Pokročilé sledování 

Můžete provést rozšířené monitorování vašeho virtuálního počítače pomocí [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Pokud jste tak již neučinili, můžete si zaregistrovat [bezplatnou zkušební verzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) služby Operations Management Suite.

Až budete mít přístup k portálu OMS, můžete najít klíč pracovního prostoru a identifikátor prostoru v okně nastavení. Použití [Set-AzureRmVMExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmextension) příkaz pro přidání rozšíření OMS k virtuálnímu počítači. Aktualizace hodnoty proměnné v níže ukázka tak, aby odrážela jste klíč pracovního prostoru OMS a prostoru ID.  

```powershell
$omsId = "<Replace with your OMS Id>"
$omsKey = "<Replace with your OMS key>"

Set-AzureRmVMExtension -ResourceGroupName myResourceGroup `
  -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
  -VMName myVM `
  -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
  -ExtensionType "MicrosoftMonitoringAgent" `
  -TypeHandlerVersion 1.0 `
  -Settings @{"workspaceId" = $omsId} `
  -ProtectedSettings @{"workspaceKey" = $omsKey} `
  -Location eastus
```

Po několika minutách měli byste vidět nový virtuální počítač v pracovním prostoru OMS. 

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Další kroky
V tomto kurzu jste nakonfigurovali a zkontrolovat virtuálních počítačů pomocí Azure Security Center. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Vytvoření virtuální sítě
> * Vytvoření skupiny prostředků a virtuálních počítačů 
> * Povolit Diagnostika spouštění ve virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazit hostitele metriky
> * Instalace rozšíření diagnostiky
> * Zobrazit metriky virtuálního počítače
> * Vytvořit výstrahu
> * Správa aktualizací Windows
> * Nastavit pokročilé monitorování

Přechodu na v dalším kurzu se dozvíte o službě Azure security center.

> [!div class="nextstepaction"]
> [Správa zabezpečení virtuálních počítačů](./tutorial-azure-security.md)