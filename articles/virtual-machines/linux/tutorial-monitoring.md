---
title: "Sledujte a aktualizujte virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Zjistěte, jak monitorování, Diagnostika spouštění a metriky výkonu a Správa balíčku aktualizací na virtuální počítač s Linuxem v Azure"
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
ms.openlocfilehash: cde484dd59ec6e2821678766726c02362222d496
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="how-to-monitor-and-update-a-linux-virtual-machine-in-azure"></a>Jak monitorovat a aktualizovat virtuální počítač s Linuxem v Azure

K zajištění, že virtuální počítače (VM) v Azure běží správně, můžete zkontrolovat Diagnostika spouštění, metriky výkonu a spravovat aktualizace balíčků. V tomto kurzu se naučíte:

> [!div class="checklist"]
> * Povolit Diagnostika spouštění ve virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazit hostitele metriky
> * Povolit rozšíření diagnostiky na virtuálním počítači
> * Zobrazit metriky virtuálního počítače
> * Vytvářet výstrahy na základě diagnostiky metriky
> * Správa balíčku aktualizací
> * Nastavit pokročilé monitorování


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Pokud si zvolíte instalaci a použití rozhraní příkazového řádku místně, tento kurz vyžaduje, že používáte Azure CLI verze verze 2.0.4 nebo novější. Verzi zjistíte spuštěním příkazu `az --version`. Pokud potřebujete instalaci nebo upgrade, přečtěte si téma [Instalace Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Vytvoření virtuálního počítače

Pokud chcete zobrazit diagnostiku a metriky v akci, je nutné virtuální počítač. Nejprve vytvořte skupinu prostředků s [vytvořit skupinu az](/cli/azure/group#create). Následující příklad vytvoří skupinu prostředků s názvem *myResourceGroupMonitor* v *eastus* umístění.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Teď vytvořte virtuální počítač s [vytvořit virtuální počítač az](https://docs.microsoft.com/cli/azure/vm#az_vm_create). Následující příklad vytvoří virtuální počítač s názvem *Můjvp*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Povolit spuštění diagnostiky

Jak spustit virtuální počítače s Linuxem, rozšíření diagnostiky spouštěcí zaznamená spouštěcí výstup a uloží je v úložišti Azure. Tato data můžete použít k odstraňování problémů spouštění virtuálních počítačů. Diagnostika spouštění nepovolí automaticky při vytváření virtuálního počítače s Linuxem pomocí rozhraní příkazového řádku Azure.

Před povolením Diagnostika spouštění, musí být vytvořen pro ukládání protokolů spouštěcí účet úložiště. Účty úložiště musí mít globálně jedinečného názvu, být v rozmezí 3 až 24 znaků a musí obsahovat pouze čísla a malá písmena. Vytvořit účet úložiště s [vytvořit účet úložiště az](/cli/azure/storage/account#create) příkaz. V tomto příkladu je náhodný řetězec použít k vytvoření název účtu úložiště jedinečný. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Při povolování Diagnostika spouštění, je potřeba identifikátor URI pro kontejner úložiště objektů blob. Následující příkaz dotazuje účet úložiště, který chcete vrátit tento identifikátor URI. Hodnota identifikátoru URI je uložen v názvech proměnných *bloburi*, která je použita v dalším kroku.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Teď povolit Diagnostika spouštění s [povolit az virtuálního počítače – Diagnostika spouštění](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_enable). `--storage` Hodnota je objekt blob URI shromážděných v předchozím kroku.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Zobrazení diagnostiky spouštění

Pokud jsou povolené Diagnostika spouštění, pokaždé, když zastavení a spuštění virtuálního počítače, informace o procesu spouštění je zapsán do souboru protokolu. V tomto příkladu nejprve zrušit přidělení virtuálního počítače s [az OM deallocate](/cli/azure/vm#deallocate) příkaz takto:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Nyní spusťte virtuální počítač s [spuštění virtuálního počítače az]( /cli/azure/vm#stop) příkaz takto:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Můžete získat data diagnostiky spouštění pro *Můjvp* s [Diagnostika spouštění virtuálních počítačů az – get spouštěcí protokolu](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#az_vm_boot_diagnostics_get_boot_log) příkaz takto:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Zobrazit hostitele metriky

Virtuální počítač s Linuxem má vyhrazený hostitel v Azure, který komunikuje se službou. Metriky jsou automaticky shromažďovat pro hostitele a lze je zobrazit na portálu Azure následujícím způsobem:

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroupMonitor**a potom vyberte **Můjvp** v seznamu prostředků.
1. Chcete-li zjistit, jaký je výkon hostitelů virtuálních počítačů, klikněte na tlačítko **metriky** v okně virtuálního počítače, pak vyberte některé z *[hostitel]* metriky v části **dostupné metriky**.

    ![Zobrazit hostitele metriky](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Instalace rozšíření diagnostiky

> [!IMPORTANT]
> Tento dokument popisuje 2.3 verzi rozšíření diagnostiky Linux, která se už nepoužívá. Verze 2.3 bude až do 30. června 2018 podporována.
>
> Místo toho lze povolit rozšíření diagnostiky Linux verze 3.0. Další informace najdete v tématu [dokumentaci](./diagnostic-extension.md).

Základní hostitele metriky, které jsou k dispozici, ale pokud chcete zobrazit podrobné a specifické pro virtuální počítač metriky, musíte nainstalovat rozšíření diagnostiky Azure ve virtuálním počítači. Rozšíření diagnostiky Azure umožňuje další funkce monitorování a diagnostická data mají být načteny z virtuálního počítače. Můžete zobrazit tyto metriky výkonu a vytvářet výstrahy založené na tom, jak se provádí virtuálního počítače. Diagnostické rozšíření nainstalovaný prostřednictvím portálu Azure následujícím způsobem:

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
1. Klikněte na tlačítko **diagnostiku nastavení**. V seznamu uvedena, který *spouštění diagnostiky* jsou již povolené z předchozí části. Klikněte na zaškrtávací políčko pro *základní metriky*.
1. V *účet úložiště* , vyhledejte a vyberte *mydiagdata [1234]* účet vytvořený v předchozí části.
1. Klikněte na tlačítko **Uložit**.

    ![Zobrazit diagnostické metriky](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Zobrazit metriky virtuálního počítače

Metriky virtuálního počítače lze zobrazit stejným způsobem, že jste si zobrazili hostitele metriky virtuálního počítače:

1. Na portálu Azure klikněte na tlačítko **skupiny prostředků**, vyberte **myResourceGroup**a potom vyberte **Můjvp** v seznamu prostředků.
1. Chcete-li zjistit, jaký je výkon virtuálního počítače, klikněte na tlačítko **metriky** v okně virtuálního počítače a potom vyberte některé z metriky diagnostiky v části **dostupné metriky**.

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

## <a name="manage-package-updates"></a>Správa balíčku aktualizací

Pomocí správy aktualizací můžete spravovat aktualizace balíčků a oprav pro virtuální počítače Linux Azure. Přímo z virtuálního počítače, můžete rychle vyhodnotit stav dostupné aktualizace, naplánovat instalaci požadovaných aktualizací a zkontrolovat výsledky nasazení k ověření, že bylo úspěšně nasazení aktualizací do virtuálního počítače.

Informace o cenách naleznete v tématu [ceny automatizace správy aktualizací](https://azure.microsoft.com/pricing/details/automation/)

### <a name="enable-update-management-preview"></a>Povolení správy aktualizací (Preview)

Povolení správy aktualizací pro virtuální počítač

1. Na levé straně obrazovky vyberte **virtuální počítače**.
1. V seznamu vyberte virtuální počítač.
1. Na obrazovce virtuálních počítačů v **operace** klikněte na tlačítko **Správa aktualizací**. **Správy aktualizací povolit** obrazovky otevře.

Chcete-li zjistit, zda je povoleno správy aktualizací pro tento virtuální počítač se provádí ověřování. Ověření zahrnuje kontroly pro pracovní prostor analýzy protokolů a propojené účet Automation, a pokud řešení v pracovním prostoru.

Pracovní prostor analýzy protokolů se používá ke shromažďování dat, které se generují ve funkcích a službách, například správu aktualizací. Pracovní prostor poskytuje jedno umístění ke kontrole a analyzovat data z více zdrojů. K provedení další akce na virtuálních počítačích, které vyžadují aktualizace, Azure Automation umožňuje spouštět skripty u virtuálních počítačů, například ke stažení a použití aktualizací.

Proces ověření také zkontroluje, pokud je virtuální počítač opatřen Microsoft Monitoring Agent (MMA) a hybridní pracovní proces. Tento agent se používá ke komunikaci s virtuálním Počítačem a získat informace o stavu aktualizace. 

Pokud nejsou splněny tyto požadavky, zobrazí se banner, která vám dává možnost povolit řešení.

![Aktualizovat banner zařadit configuration Management](./media/tutorial-monitoring/manage-updates-onboard-solution-banner.png)

Kliknutím na banner řešení povolte. Pokud některý z těchto předpokladů byl zjištěn chybět po ověření, budou automaticky přidány:

* [Analýza protokolu](../../log-analytics/log-analytics-overview.md) prostoru
* [Automation](../../automation/automation-offering-get-started.md)
* A [hybridní pracovní proces runbooku](../../automation/automation-hybrid-runbook-worker.md) je povoleno na virtuálním počítači

**Povolit správu aktualizace** obrazovky otevře. Nakonfigurujte nastavení a klikněte na tlačítko **povolit**.

![Povolit řešení pro správu aktualizací](./media/tutorial-monitoring/manage-updates-update-enable.png)

Povolení řešení může trvat až 15 minut a během této doby by neměl zavřete okno prohlížeče. Po povolení řešení informace o chybějící aktualizace ze Správce balíčků ve virtuálním počítači jsou k analýze protokolů.
Může trvat až 30 minut, 6 hodin pro data, která mají být k dispozici pro analýzu.

### <a name="view-update-assessment"></a>Zobrazení posouzení aktualizací

Po **Správa aktualizací** řešení je povoleno, **Správa aktualizací** se zobrazí obrazovka. Na kartě **Chybějící aktualizace** můžete zobrazit seznam chybějících aktualizací.

![Stav aktualizace zobrazení](./media/tutorial-monitoring/manage-updates-view-status-linux.png)

### <a name="schedule-an-update-deployment"></a>Naplánování nasazení aktualizace

Chcete-li nainstalovat aktualizace, naplánujte nasazení, které odpovídá vaší verzi okno plán a údržby.

Naplánovat nové nasazení aktualizace pro virtuální počítač kliknutím **nasazení aktualizace plánu** v horní části **Správa aktualizací** obrazovky. V **nové nasazení aktualizace** obrazovky, zadejte následující informace:

* **Název** – Zadejte jedinečný název pro identifikaci nasazení aktualizací.
* **Aktualizace pro vyloučení** – výběrem této možnosti zadejte názvy balíčků, které mají být vyloučeny z aktualizace.
* **Nastavení plánu** – Můžete přijmout výchozí datum a čas, což je 30 minut od aktuálního času, nebo zadat jiný čas. Můžete také určit, jestli nasazení proběhne jednou, nebo nastavit plán opakování. Pokud chcete nastavit plán opakování, klikněte na možnost Opakovat v části Opakování.

  ![Obrazovka nastavení plánu aktualizací](./media/tutorial-monitoring/manage-updates-schedule-linux.png)

* **Časové období údržby (minuty)** – Zadejte časové období, ve kterém má dojít k nasazení aktualizací.  To pomáhá zajistit, že se změny provést v rámci vaší definovaná časová období údržby. 

Jakmile dokončíte konfiguraci plánu, klikněte na tlačítko **Vytvořit** a vrátíte se na řídicí panel stavu.
Všimněte si, že **naplánovaná** tabulka ukazuje plán nasazení jste vytvořili.

> [!WARNING]
> Virtuální počítač se restartuje automaticky po instalaci aktualizací a pokud je dostatek času pro správu a údržbu.

Správa aktualizací používá existující Správce balíčků na vašem virtuálním počítači k instalaci balíčků.

### <a name="view-results-of-an-update-deployment"></a>Zobrazení výsledků nasazení aktualizací

Po spuštění naplánovaného nasazení se stav tohoto nasazení zobrazí na kartě **Nasazení aktualizací** na obrazovce **Správa aktualizací**.
Pokud je nasazení aktuálně spuštěno, jeho stav je **Probíhající**. Po úspěšném dokončení se změní na **Úspěch**.
Pokud dojde k selhání s minimálně jednu aktualizaci v nasazení, je stav **se nezdařilo**.
Kliknutím na dokončené nasazení aktualizací zobrazíte řídicí panel pro toto nasazení aktualizací.

![Řídicí panel stavu nasazení aktualizací pro konkrétní nasazení](./media/tutorial-monitoring/manage-updates-view-results.png)

V **aktualizaci výsledků** dlaždice je uveden seznam celkový počet aktualizací a výsledky nasazení do virtuálního počítače.
V tabulce vpravo je podrobný rozpis všech aktualizací a výsledků instalace, které můžou mít jednu z následujících hodnot:

* **Nebyl proveden pokus o** -aktualizace nenainstalovala, protože došlo k dispozici dostatečná doba podle doba trvání okna údržby definované.
* **Úspěšné** -aktualizace byla úspěšně stáhli a nainstalovali do virtuálního počítače
* **Se nezdařilo** -aktualizace se nepodařilo stáhnout nebo nainstalovat do virtuálního počítače.

Kliknutím na **Všechny protokoly** zobrazíte všechny položky protokolu, které toto nasazení vytvořilo.

Klikněte **výstup** dlaždice zobrazíte stream úloh sady runbook zodpovědní za správu nasazení aktualizací na cílovém virtuálním počítači.

Kliknutím na **Chyby** zobrazíte podrobné informace o případných chybách nasazení.

## <a name="advanced-monitoring"></a>Pokročilé sledování 

Můžete provést rozšířené monitorování vašeho virtuálního počítače pomocí [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Pokud jste tak již neučinili, můžete si zaregistrovat [bezplatnou zkušební verzi](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) služby Operations Management Suite.

Až budete mít přístup k portálu OMS, můžete najít klíč pracovního prostoru a identifikátor prostoru v okně nastavení. Nahrazení < klíč pracovního prostoru > a < id pracovního prostoru > hodnotami pro z vaší OMS prostoru a pak můžete použít **nastavení rozšíření virtuálního az** přidat příponu OMS na virtuální počítač:

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

V okně hledání protokolů na portálu OMS byste měli vidět *Můjvp* například informace zobrazené na následujícím obrázku:

![Okno OMS](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Další postup

V tomto kurzu nakonfigurované, zkontrolovat a spravovat aktualizace pro virtuální počítač. Naučili jste se tyto postupy:

> [!div class="checklist"]
> * Povolit Diagnostika spouštění ve virtuálním počítači
> * Zobrazení diagnostiky spouštění
> * Zobrazit hostitele metriky
> * Povolit rozšíření diagnostiky na virtuálním počítači
> * Zobrazit metriky virtuálního počítače
> * Vytvářet výstrahy na základě diagnostiky metriky
> * Správa balíčku aktualizací
> * Nastavit pokročilé monitorování

Přechodu na v dalším kurzu se dozvíte o Azure Security Center.

> [!div class="nextstepaction"]
> [Správa zabezpečení virtuálních počítačů](./tutorial-azure-security.md)
