---
title: "Monitorování a aktualizace virtuálních počítačů s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak v Azure monitorovat diagnostiku spouštění a metriky výkonu a jak spravovat aktualizace balíčků na virtuálním počítači s Linuxem"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 230ce6a6b33e63bcced5f520b57b63ef4ed05448
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Jak monitorovat a aktualizovat virtuální počítač s Linuxem v Azure

K zajištění správného chodu virtuálních počítačů v Azure můžete zkontrolovat diagnostiku spouštění, metriky výkonu a spravovat aktualizace balíčků. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Povolení diagnostiky spouštění ve virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazení metrik hostitele
> * Povolení diagnostického rozšíření ve virtuálním počítači
> * Zobrazení metrik virtuálního počítače
> * Vytvoření výstrah na základě diagnostických metrik
> * Správa aktualizace balíčků
> * Nastavení pokročilého monitorování


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud se rozhodnete nainstalovat a místně používat rozhraní příkazového řádku, musíte mít Azure CLI verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Vytvoření virtuálního počítače

Pokud chcete vidět, jak funguje diagnostika a metriky, potřebujete virtuální počítač. Nejdřív vytvořte skupinu prostředků pomocí příkazu [az group create](/cli/azure/group#az_group_create). Následující příklad vytvoří skupinu prostředků *myResourceGroupMonitor* v umístění *eastus*.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Teď pomocí příkazu [az vm create](https://docs.microsoft.com/cli/azure/vm#az_vm_create) vytvořte virtuální počítač. Následující příklad vytvoří virtuální počítač *myVM*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Povolení diagnostiky spouštění

Při spouštění virtuálních počítačů s Linuxem zaznamenává diagnostické rozšíření výstup spouštění a uloží ho v úložišti Azure. Tato data můžete použít k odstraňování problémů při spouštění virtuálních počítačů. Po vytvoření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure není diagnostika spouštění automaticky povolená.

Před povolením diagnostiky spouštění je třeba vytvořit účet úložiště pro ukládání protokolů spouštění. Účty úložiště musí mít globální jedinečný název v rozmezí 3 až 24 znaků a musí obsahovat pouze čísla a malá písmena. Účet úložiště vytvoříte příkazem [az storage account create](/cli/azure/storage/account#az_storage_account_create). V tomto příkladu se k vytvoření jedinečného názvu účtu úložiště použil náhodný řetězec. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Při povolování diagnostiky spouštění je potřeba identifikátor URI pro kontejner úložiště objektů blob. Následující příkaz se dotazuje na účet úložiště za účelem vrácení tohoto identifikátoru URI. Hodnota identifikátoru URI je uložená v názvech proměnných *bloburi*, které se používají v dalším kroku.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Teď povolte diagnostiku spouštění pomocí příkazu [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). Hodnota `--storage` je identifikátor URI objektu blob získaný v předchozím kroku.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Zobrazení diagnostiky spouštění

Pokud je povolená diagnostika spouštění, zapíše se při každém spuštění a vypnutí virtuálního počítače informace o procesu spouštění do souboru protokolu. V tomto příkladu nejprve zrušte přidělení virtuálního počítače příkazem [az OM deallocate](/cli/azure/vm#az_vm_deallocate) takto:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Nyní spusťte virtuální počítač pomocí příkazu [az vm start]( /cli/azure/vm#az_vm_stop) následujícím způsobem:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Data diagnostiky spouštění pro *myVM* můžete získat pomocí příkazu [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) takto:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Zobrazení metrik hostitele

Virtuální počítač s Linuxem má vyhrazeného hostitele v Azure, který s ním komunikuje. Metriky se pro hostitele shromažďují automaticky a lze je zobrazit na portálu Azure Portal následujícím způsobem:

1. Na portálu Azure Portal klikněte na tlačítko **Skupiny prostředků**, vyberte **myResourceGroupMonitor** a potom v seznamu prostředků vyberte **myVM**.
1. Pokud chcete zjistit, jaký je výkon virtuálního počítače hostitele, klikněte na tlačítko **Metriky** v okně virtuálního počítače, pak vyberte některou z metrik *[hostitele]* v části **Dostupné metriky**.

    ![Zobrazení metrik hostitele](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Instalace diagnostického rozšíření

> [!IMPORTANT]
> Tento dokument popisuje verzi 2.3 diagnostického rozšíření Linux, které je již zastaralé. Podpora verze 2.3 končí 30. června 2018.
>
> Místo toho lze povolit diagnostické rozšíření Linux verze 3.0. Další informace najdete v [dokumentaci](./diagnostic-extension.md).

Jsou k dispozici základní metriky hostitele, ale pokud chcete zobrazit podrobné metriky specifické pro virtuální počítač, musíte ve virtuálním počítači nainstalovat diagnostické rozšíření Azure. Diagnostické rozšíření Azure umožňuje načítání dalších monitorovacích a diagnostických dat z virtuálního počítače. Tyto metriky výkonu můžete zobrazit a vytvářet výstrahy na základě výkonnosti virtuálního počítače. Diagnostické rozšíření se instaluje prostřednictvím portálu Azure Portal následujícím způsobem:

1. Na portálu Azure Portal klikněte na tlačítko **Skupiny prostředků**, vyberte **myResourceGroup** a potom v seznamu prostředků vyberte **myVM**.
1. Klikněte na **Nastavení diagnostiky**. V seznamu je vidět, že *Diagnostika spouštění* je již povolená z předchozí části. Zaškrtněte políčko *Základní metriky*.
1. V části *Účet úložiště* vyhledejte a vyberte účet *mydiagdata [1234]* vytvořený v předchozí části.
1. Klikněte na tlačítko **Uložit**.

    ![Zobrazení diagnostických metrik](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Zobrazení metrik virtuálního počítače

Metriky virtuálního počítače lze zobrazit stejným způsobem jako metriky virtuálního počítače hostitele:

1. Na portálu Azure Portal klikněte na tlačítko **Skupiny prostředků**, vyberte **myResourceGroup** a potom v seznamu prostředků vyberte **myVM**.
1. Pokud chcete zjistit, jaký je výkon virtuálního počítače, klikněte na tlačítko **Metriky** v okně virtuálního počítače a pak vyberte některou z diagnostických metrik v části **Dostupné metriky**.

    ![Zobrazení metrik virtuálního počítače](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a>Vytváření upozornění

Na základě konkrétních metrik výkonu můžete vytvořit výstrahy. Výstrahy lze například použít k upozornění, že průměrné využití procesoru překračuje prahovou hodnotu nebo že volné místo na disku kleslo pod určitou velikost. Výstrahy ze zobrazují v portálu Azure Portal nebo je lze odeslat e-mailem. V reakci na vygenerované výstrahy můžete také aktivovat runbooky Azure Automation nebo Azure Logic Apps.

Následující příklad vytvoří výstrahu týkající se průměrného využití procesoru.

1. Na portálu Azure Portal klikněte na tlačítko **Skupiny prostředků**, vyberte **myResourceGroup** a potom v seznamu prostředků vyberte **myVM**.
2. Klikněte na tlačítko **Pravidla výstrah** v okně virtuálního počítače a potom na **Přidat upozornění metriky** v horní části okna výstrahy.
4. Zadejte **Název** výstrahy, například *mojePravidloVystrahy*.
5. Pokud chcete spustit výstrahu, pokud procento využití procesoru překročí hodnotu 1,0 po dobu pěti minut, ponechte výchozí výběr všech ostatních nastavení.
6. Volitelně můžete zaškrtnutím políčka *Vlastníci, přispěvatelé a čtenáři e-mailů* odesílat oznámení e-mailem. Výchozí akce je zobrazení oznámení na portálu.
7. Klikněte na tlačítko **OK**.

## <a name="manage-package-updates"></a>Správa aktualizace balíčků

Pomocí řešení Update Management můžete spravovat aktualizace balíčků a oprav pro virtuální počítače Linux Azure. Přímo z virtuálního počítače můžete rychle vyhodnotit stav dostupných aktualizací, naplánovat instalaci požadovaných aktualizací a zkontrolovat výsledky nasazení za účelem ověření správného použití aktualizací ve virtuálních počítačích.

Informace o cenách najdete na stránce s [cenami služby Automation za správu aktualizací](https://azure.microsoft.com/pricing/details/automation/).

### <a name="enable-update-management-preview"></a>Povolení řešení Update Management (Preview)

Povolení řešení Update Management pro virtuální počítač

1. Na levé straně obrazovky vyberte **Virtuální počítače**.
1. V seznamu vyberte virtuální počítač.
1. Na obrazovce virtuálního počítače v části **Operace** klikněte na **Update Management**. Otevře se obrazovka **Povolit řešení Update Management**.

Provede se ověření, pomocí kterého se určí, jestli je pro tento virtuální počítač povolené řešení Update Management. Toto ověření zahrnuje kontroly pracovního prostoru Log Analytics a propojeného účtu Automation a kontrolu, jestli se řešení nachází v tomto pracovním prostoru.

Pracovní prostor Log Analytics slouží ke shromažďování dat generovaných funkcemi a službami, jako je řešení Update Management. Tento pracovní prostor poskytuje možnost kontroly a analýzy dat z několika zdrojů na jednom místě. Pokud chcete na virtuálních počítačích, které vyžadují aktualizace, provádět další akce, umožňuje Azure Automation spouštět ve virtuálních počítačích skripty, například ke stažení a použití aktualizací.

Proces ověřování také zkontroluje, jestli je virtuální počítač zřízený s agentem Microsoft Monitoring Agent (MMA) a procesem hybrid worker. Agent slouží ke komunikaci s virtuálním počítačem a získávání informací o nainstalovaném softwaru. 

Pokud se nesplní tyto požadavky, zobrazí se banner nabízející možnost povolit dané řešení.

![Banner konfigurace připojení k řešení Update Management](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Kliknutím na banner řešení povolte. Pokud po ověření chyběla některá z následujících požadovaných součástí, automaticky se přidá:

* Pracovní prostor [Log Analytics](../../log-analytics/log-analytics-overview.md)
* [Automation](../../automation/automation-offering-get-started.md)
* Povolený [hybridní pracovní proces runbooku](../../automation/automation-hybrid-runbook-worker.md) na virtuálním počítači

Otevře se obrazovka **Povolit řešení Update Management**. Nakonfigurujte nastavení a klikněte na tlačítko **Povolit**.

![Povolení řešení Update Management](./media/tutorial-monitoring/manage-updates-update-enable.png)

Povolení řešení může trvat až 15 minut a během této doby byste neměli zavírat okno prohlížeče. Po povolení řešení začnou do Log Analytics proudit ze správce balíčků informace o chybějících aktualizacích na virtuálních počítačích.
Zpřístupnění dat pro analýzu může trvat 30 minut až 6 hodin.

### <a name="view-update-assessment"></a>Zobrazení posouzení aktualizací

Po povolení řešení **Update Management** se zobrazí obrazovka **Update Management**. Na kartě **Chybějící aktualizace** můžete zobrazit seznam chybějících aktualizací.

![Zobrazení stavu aktualizace](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Naplánování nasazení aktualizace

Pokud chcete nainstalovat aktualizace, naplánujte nasazení odpovídající vašemu plánu vydávání a časovému intervalu pro údržbu.

Naplánujte nové nasazení aktualizací pro virtuální počítač kliknutím na **Naplánovat nasazení aktualizace** v horní části obrazovky **Update Management**. Na obrazovce **Nové nasazení aktualizací** zadejte následující informace :

* **Název** – Zadejte jedinečný název pro identifikaci nasazení aktualizací.
* **Aktualizace k vyloučení** – Výběrem této možnosti zadáte názvy balíčků, které mají být vyloučeny z aktualizace.
* **Nastavení plánu** – Můžete přijmout výchozí datum a čas, což je 30 minut od aktuálního času, nebo zadat jiný čas. Můžete také určit, jestli nasazení proběhne jednou, nebo nastavit plán opakování. Pokud chcete nastavit plán opakování, klikněte na možnost Opakovat v části Opakování.

  ![Obrazovka nastavení plánu aktualizací](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Časové období údržby (minuty)** – Zadejte časové období, ve kterém má dojít k nasazení aktualizací.  Pomůžete tím zajistit, že se změny provedou v rámci vašich definovaných časových intervalů pro údržbu. 

Jakmile dokončíte konfiguraci plánu, klikněte na tlačítko **Vytvořit** a vrátíte se na řídicí panel stavu.
Všimněte si, že v tabulce **Naplánované** se zobrazí plán nasazení, který jste vytvořili.

> [!WARNING]
> Virtuální počítač se restartuje automaticky po instalaci aktualizací a pokud je dostatek času v intervalu pro údržbu.

Řešení Update Management používá k instalaci balíčků na virtuálním počítači existujícího správce balíčků.

### <a name="view-results-of-an-update-deployment"></a>Zobrazení výsledků nasazení aktualizací

Po spuštění naplánovaného nasazení se stav tohoto nasazení zobrazí na kartě **Nasazení aktualizací** na obrazovce **Správa aktualizací**.
Pokud je nasazení aktuálně spuštěno, jeho stav je **Probíhající**. Po úspěšném dokončení se změní na **Úspěch**.
Pokud dojde u jedné nebo více aktualizací v nasazení k chybě, stav bude mít hodnotu **Neúspěšné**.
Kliknutím na dokončené nasazení aktualizací zobrazíte řídicí panel pro toto nasazení aktualizací.

![Řídicí panel stavu nasazování aktualizací pro konkrétní nasazení](./media/tutorial-monitoring/manage-updates-view-results.png)

Na dlaždici **Výsledky aktualizací** je souhrn celkového počtu aktualizací a výsledků nasazení na virtuálním počítači.
V tabulce vpravo je podrobný rozpis všech aktualizací a výsledků instalace, které můžou mít jednu z následujících hodnot:

* **Nebyl proveden pokus** – aktualizace se nenainstalovala, protože podle definovaného trvání časového období údržby nebylo k dispozici dostatek času.
* **Úspěšné** – aktualizace se úspěšně stáhla a nainstalovala do virtuálního počítače.
* **Neúspěšné** – aktualizaci se nepodařilo stáhnout nebo nainstalovat do virtuálního počítače.

Kliknutím na **Všechny protokoly** zobrazíte všechny položky protokolu, které toto nasazení vytvořilo.

Kliknutím na dlaždici **Výstup** zobrazíte datový proud úlohy runbooku zodpovědného za správu nasazení aktualizací na cílovém virtuálním počítači.

Kliknutím na **Chyby** zobrazíte podrobné informace o případných chybách nasazení.

## <a name="advanced-monitoring"></a>Pokročilé sledování 

Pomocí sady [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) můžete provést rozšířené monitorování virtuálního počítače. Pokud jste to již neudělali, můžete si zaregistrovat [bezplatnou zkušební verzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) sady Operations Management Suite.

Až budete mít přístup k portálu OMS, najdete klíč a identifikátor pracovního prostoru v okně Nastavení. Nahraďte <klíč-pracovního-prostoru> a <id-pracovního-prostoru> hodnotami z pracovního prostoru OMS a pak můžete pomocí příkazu **az vm extension set** přidat rozšíření OMS na virtuální počítač:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

V okně Hledání v protokolu na portálu OMS byste měli vidět *myVM*, jak ukazuje následující obrázek:

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste nakonfigurovali, zkontrolovali a spravovali aktualizace pro virtuální počítač. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Povolení diagnostiky spouštění ve virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazení metrik hostitele
> * Povolení diagnostického rozšíření ve virtuálním počítači
> * Zobrazení metrik virtuálního počítače
> * Vytvoření výstrah na základě diagnostických metrik
> * Správa aktualizace balíčků
> * Nastavení pokročilého monitorování

V dalším kurzu se dozvíte něco o Azure Security Center.

> [!div class="nextstepaction"]
> [Správa zabezpečení virtuálních počítačů](./tutorial-azure-security.md)
