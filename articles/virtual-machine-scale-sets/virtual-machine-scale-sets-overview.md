---
title: "Přehled škálovacích sad virtuálních počítačů Azure | Dokumentace Microsoftu"
description: "Přečtěte si podrobnosti o škálovacích sadách virtuálních počítačů Azure."
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6dbb88577733d5ec0dc17acf7243b2ba7b829b38
ms.openlocfilehash: 8b2fbc230faf01797109114d6ebdffe5ec50e48b
ms.contentlocale: cs-cz
ms.lasthandoff: 07/04/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Co jsou škálovací sady virtuálních počítačů v Azure?
Škálovací sady virtuálních počítačů jsou výpočetním prostředkem Azure, který můžete použít k nasazení a správě sady identických virtuálních počítačů. Protože jsou všechny virtuální počítače ve škálovací sadě nakonfigurované stejně, podporují tyto sady skutečné automatické škálování – virtuální počítače není třeba zřizovat předem. Je proto snadné vytvářet rozsáhlé služby zaměřené na vysoký výpočetní výkon, velké objemy dat a kontejnerizované úlohy.

U aplikací, které vyžadují horizontální navyšování a snižování výpočetní kapacity, se škálovací operace implicitně vyvažují napříč doménami selhání a aktualizačními doménami. Další úvod ke škálovacím sadám najdete v [oznámení na blogu Azure](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Více se o škálovacích sadách dozvíte také v těchto videích:

* [Mark Russinovich hovoří o škálovacích sadách Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman provádí škálovacími sadami virtuálních počítačů](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Vytváření a správa škálovacích sad
Škálovací sadu můžete vytvořit na webu [Azure Portal](https://portal.azure.com) – vyberte položku **Nový** a zadejte text **škálovací** na vyhledávacím panelu. Ve výsledcích se objeví **Škálovací sada virtuálních počítačů**. Z této položky můžete přejít k vyplnění požadovaných polí pro přizpůsobení a nasazení škálovací sady. Máte také možnost nastavit základní pravidla automatického škálování v závislosti na využití procesoru.

Škálovací sady můžete definovat a nasazovat pomocí šablon JSON a [rozhraní REST API](https://msdn.microsoft.com/library/mt589023.aspx), podobně jako jednotlivé virtuální počítače v Azure Resource Manageru. Proto můžete použít všechny standardní metody nasazení v Azure Resource Manageru. Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Sadu ukázkových šablon škálovacích sad virtuálních počítačů můžete najít v [úložišti GitHub šablon pro rychlý začátek pro Azure](https://github.com/Azure/azure-quickstart-templates). (Hledejte šablony, které mají v názvu **vmss**.)

V příkladech šablon pro rychlý začátek odkazuje tlačítko Nasazení do Azure v souboru readme k jednotlivým šablonám na funkce nasazení na portálu. Pokud chcete nasadit škálovací sadu, klikněte na toto tlačítko a potom na portálu vyplňte požadované parametry. 

## <a name="scaling-a-scale-set-out-and-in"></a>Horizontální navyšování a snižování kapacity škálovací sady
Kapacitu škálovací sady můžete změnit na webu Azure Portal kliknutím na **Škálování** v části **Nastavení**. 

Pokud chcete změnit kapacitu škálovací sady z příkazového řádku, použijte v [Azure CLI](https://github.com/Azure/azure-cli) příkaz **scale**. Například pokud chcete nastavit kapacitu škálovací sady na 10 virtuálních počítačů, zadejte příkaz:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

Pokud chcete nastavit počet virtuálních počítačů ve škálovací sadě pomocí PowerShellu, použijte k tomu příkaz **Update-AzureRmVmss**:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Pokud chcete zvýšit nebo snížit počet virtuálních počítačů ve škálovací sadě pomocí šablony Azure Resource Manageru, změňte vlastnost **capacity** a znovu nasaďte šablonu. Takto můžete jednoduše integrovat škálovací sady s automatickým škálováním Azure nebo vytvořit vlastní škálovací vrstvu, pokud potřebujete definovat vlastní škálovací události, které nejsou podporovány automatickým škálováním Azure. 

Pokud šablonu Azure Resource Manageru znovu nasazujete s cílem změnit kapacitu, stačí definovat mnohem menší šablonu, která bude obsahovat pouze paket vlastnosti **SKU** s aktualizovanou kapacitou. [Tady je příklad](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

## <a name="autoscale"></a>Automatické škálování

Škálovací sadu můžete při vytváření na webu Azure Portal volitelně nakonfigurovat s nastavením automatického škálování. Umožníte tak navyšování nebo snižování počtu virtuálních počítačů v závislosti na průměrném využití procesoru. 

Mnoho šablon škálovacích sad v [šablonách Azure pro rychlý začátek](https://github.com/Azure/azure-quickstart-templates) definuje nastavení automatického škálování. Nastavení automatického škálování můžete také přidat do stávající škálovací sady. Tady je pro ukázku skript Azure PowerShellu, který do škálovací sady přidá automatické škálování v závislosti na využití procesoru:

```PowerShell

$subid = "yoursubscriptionid"
$rgname = "yourresourcegroup"
$vmssname = "yourscalesetname"
$location = "yourlocation" # e.g. southcentralus

$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator LessThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Decrease -ScaleActionValue 1
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "autoprofile1"
Add-AzureRmAutoscaleSetting -Location $location -Name "autosetting1" -ResourceGroup $rgname -TargetResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -AutoscaleProfiles $profile1
```

Seznam platných metrik, na základě kterých je možné škálovat, najdete v tématu [Podporované metriky ve službě Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) v části Microsoft.Compute/virtualMachineScaleSets. K dispozici jsou také pokročilejší možnosti automatického škálování, včetně automatického škálování na základě plánu nebo integrace se systémy pro výstrahy pomocí webhooků.

## <a name="monitoring-your-scale-set"></a>Monitorování škálovací sady
Na webu [Azure Portal](https://portal.azure.com) můžete najít seznam škálovacích sad a jejich vlastností. Portál také podporuje operace správy. Operace správy můžete provádět se škálovacími sadami nebo s jednotlivými virtuálními počítači v rámci škálovací sady. Na portálu najdete také přizpůsobitelný graf využití prostředků. 

Pokud potřebujete zobrazit nebo upravit základní definici JSON prostředku Azure, můžete použít také [Průzkumníka prostředků Azure](https://resources.azure.com). Škálovací sady jsou prostředky v poskytovateli prostředků Azure Microsoft.Compute. Na tomto webu je můžete zobrazit rozbalením následujících odkazů:

**Subscriptions (Předplatná)** > **vaše předplatné** > **resourceGroups (skupiny prostředků)** > **providers (poskytovatelé)** > **Microsoft.Compute** > **virtualMachineScaleSets (škálovací sady virtuálních počítačů)** > **vaše škálovací sada** > atd.

## <a name="scale-set-scenarios"></a>Scénáře použití škálovacích sad
Tato část uvádí některé typické scénáře použití škálovacích sad. Tyto scénáře využívají některé služby Azure vyšších úrovní (třeba Batch, Service Fabric nebo Container Service).

* **Použití RDP nebo SSH k připojení k instancím škálovací sady:** Škálovací sada se vytvoří ve virtuální síti a jednotlivým virtuálním počítačům ve škálovací sadě se ve výchozím nastavení nebudou přidělovat veřejné IP adresy. Díky této zásadě se vyhnete nákladům a administrativní režii spojeným s přidělováním samostatných veřejných IP adres všem uzlům ve výpočetní síti. Pokud potřebujete přímá externí připojení k virtuálním počítačům škálovací sady, můžete nakonfigurovat, aby škálování sada automaticky přiřazovala novým virtuálním počítačům veřejné IP adresy. Případně se k virtuálním počítačům můžete připojit z jiných prostředků ve virtuální síti, kterým je možné přidělit veřejné IP adresy, jako jsou nástroje pro vyrovnávání zatížení nebo samostatné virtuální počítače. 
* **Připojení k virtuálním počítačům pomocí pravidel pro překlad adres (NAT):** Můžete vytvořit veřejnou IP adresu, přiřadit ji k nástroji pro vyrovnávání zatížení a definovat fond příchozího překladu adres. Tímto způsobem namapujete porty u IP adresy na porty virtuálních počítačů ve škálovací sadě. Například:
  
  | Zdroj | Zdrojový port | Cíl | Cílový port |
  | --- | --- | --- | --- |
  |  Veřejná IP adresa |Port 50000 |vmss\_0 |Port 22 |
  |  Veřejná IP adresa |Port 50001 |vmss\_1 |Port 22 |
  |  Veřejná IP adresa |Port 50002 |vmss\_2 |Port 22 |
  
   V [tomto příkladu](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat) se definují pravidla překladu adres umožňující připojení SSH ke každému virtuálnímu počítači ve škálovací sadě s použitím jedné veřejné IP adresy.
  
   [Tento příklad](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat) dělá totéž s RDP a systémem Windows.
* **Připojení k virtuálním počítačům pomocí hostitele typu „jumpbox“:** Pokud ve stejné virtuální síti vytvoříte škálovací sadu a samostatný virtuální počítač, pak se všechny tyto virtuální počítače můžou navzájem připojovat pomocí svých interních IP adres, jak jsou definovány ve virtuální síti nebo podsíti. Pokud vytvoříte veřejnou IP adresu a přiřadíte ji k samostatnému virtuálnímu počítači, můžete se k samostatnému virtuálnímu počítači připojit pomocí RDP nebo SSH. Z tohoto počítače se pak můžete připojit k instancím škálovací sady. V tomto bodě si můžete všimnout, že jednoduchá škálovací sada je ze své podstaty bezpečnější než jednoduchý samostatný virtuální počítač s veřejnou IP adresou ve své výchozí konfiguraci.
  
   Například [tato šablona](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox) nasadí jednoduchou škálovací sadu se samostatným virtuálním počítačem. 
* **Vyrovnávání zatížení v instancích škálovací sady:** Pokud chcete dodávat práci do výpočetního clusteru virtuálních počítačů na bázi kruhového dotazování, můžete v nástroji Azure Load Balancer nakonfigurovat odpovídající pravidla vyrovnávání zatížení úrovně 4. Můžete definovat sondy, které budou ověřovat, že aplikace běží, pomocí portů pro příkazy ping s určeným protokolem, intervalem a cestou pro žádosti. [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) podporuje jak škálovací sady, tak úroveň 7 i sofistikovanější scénáře vyrovnávání zatížení.
  
   [Tento příklad](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) vytvoří škálovací sadu, ve které běží webový server Apache a která používá nástroj pro vyrovnávání zatížení k rozdělení práce mezi jednotlivé virtuální počítače. (Podívejte se na typ prostředku Microsoft.Network/loadBalancers a položky networkProfile a extensionProfile v části virtualMachineScaleSet.)

   [Tento příklad pro Linux](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway) a [tento příklad pro Windows](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) používají Application Gateway.  

* **Nasazení škálovací sady jako výpočetního clusteru ve správci clusteru PaaS:** Škálovací sady se někdy označují jako role pracovních procesů nové generace. Přestože je tento popis výstižný, existuje riziko, že dojde k záměně funkcí škálovacích sad s funkcemi služby Azure Cloud Services. V jistém smyslu poskytují škálovací sady skutečné pracovní role pro pracovní prostředky. Jsou generalizovaným výpočetním prostředkem, který je nezávislý na platformě nebo modulu runtime, umožňuje přizpůsobení a integruje se s IaaS s nástrojem Azure Resource Manager.
  
   Pracovní role cloudové služby je omezena z hlediska podpory platformy/runtime (platí jen pro image platformy Windows). Zahrnuje ale také služby jako záměnu virtuální IP adresy, konfigurovatelné nastavení upgradu a nastavení pro konkrétní nasazení runtime nebo aplikace. Tyto služby *ještě* nejsou dostupné jako škálovací sady ani nejsou doručované jinými vysokoúrovňovými službami PaaS jako Azure Service Fabric. Na škálovací sady se můžete dívat jako na infrastrukturu, která podporuje model PaaS. Na této infrastruktuře jsou postavena řešení PaaS, jako je [Service Fabric](https://azure.microsoft.com/services/service-fabric/).
  
   V [tomto příkladu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) takového přístupu nasadí [Azure Container Service](https://azure.microsoft.com/services/container-service/) cluster na základě škálovacích sad pomocí orchestrátoru kontejneru.

## <a name="scale-set-performance-and-scale-guidance"></a>Pokyny týkající se výkonu a škálování u škálovacích sad
* Škálovací sada podporuje až 1 000 virtuálních počítačů. Pokud vytváříte a nahráváte vlastní image virtuálních počítačů, platí omezení na 100 virtuálních počítačů. Důležité informace o používání velkých škálovacích sad najdete v tématu [Práce s velkými škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-placement-groups.md).
* K používání škálovacích sad není potřeba předem vytvářet účty úložiště Azure. Škálovací sady podporují službu Azure Managed Disks, díky které nemusíte mít starosti o výkon, pokud jde o počet disků v účtu úložiště. Další informace najdete v tématu [Škálovací sady virtuálních počítačů Azure a spravované disky](virtual-machine-scale-sets-managed-disks.md).
* Zvažte použití služby Azure Storage úrovně Premium namísto úrovně Standard pro rychlejší a předvídatelnější zřizování virtuálních počítačů a vylepšení výkonu vstupně-výstupních operací.
* Počet virtuálních počítačů, které můžete vytvořit, je omezen kvótou pro počet jader v oblasti, ve které nasazení provádíte. Možná budete muset požádat zákaznickou podporu o navýšení vaší kvóty pro výpočetní prostředky, i když už dnes máte vysoký limit pro jádra, který platí pro cloudové služby Azure. Dotaz na kvótu spustíte pomocí následujícího příkazu Azure CLI: `azure vm list-usage`. Nebo pomocí následujícího příkazu PowerShellu: `Get-AzureRmVMUsage`.

## <a name="frequently-asked-questions-for-scale-sets"></a>Nejčastější dotazy ke škálovacím sadám
**Otázka:** Kolik virtuálních počítačů může obsahovat škálovací sada?

**Odpověď:** Škálovací sada může obsahovat 0 až 1 000 virtuálních počítačů založených na imagích platformy nebo 0 až 100 virtuálních počítačů založených na vlastních imagích. 

**Otázka:** Podporují se ve škálovacích sadách datové disky?

**Odpověď:** Ano. Škálovací sada může definovat konfiguraci připojených datových jednotek, která se použije na všechny virtuální počítače v sadě. Další informace najdete v tématu [Škálovací sady Azure a připojené datové disky](virtual-machine-scale-sets-attached-disks.md). Další možnosti ukládání dat zahrnují:

* Soubory Azure (sdílené jednotky SMB)
* Jednotka operačního systému
* Dočasné jednotky (místní úložiště, nezálohované pomocí Azure Storage)
* Datová služba Azure (např. tabulky Azure, objekty blob Azure)
* Externí datová služba (např. vzdálená databáze)

**Otázka:** Které oblasti Azure podporují škálovací sady?

**Odpověď:** Všechny oblasti podporují škálovací sady.

**Otázka:** Jak se vytváří škálovací sada s použitím vlastní image?

**Odpověď:** Na základě virtuálního pevného disku vlastní image vytvoříte spravovaný disk, na který budete odkazovat v šabloně škálovací sady. [Tady je příklad](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**Otázka:** Pokud snížím kapacitu škálovací sady z 20 na 15, které virtuální počítače budou odebrány?

**Odpověď:** Virtuální počítače se ze škálovací sady odebírají rovnoměrně napříč aktualizačními doménami a doménami selhání, aby se maximalizovala dostupnost. Nejprve se odeberou virtuální počítače s nejvyšším ID.

**Otázka:** A co když pak zvýším kapacitu z 15 na 18?

**Odpověď:** Pokud zvýšíte kapacitu na 18, vytvoří se 3 nové virtuální počítače. ID instance virtuálního počítače se přírůstkově zvýší oproti předchozí nejvyšší hodnotě (např. 20, 21, 22). Virtuální počítače se vyvažují mezi doménami selhání a aktualizačními doménami.

**Otázka:** Pokud ve škálovací sadě používám několik rozšíření, je možné vynucovat určitou posloupnost provádění?

**Odpověď:** Ne přímo, ale u rozšíření customScript by váš skript mohl čekat na dokončení jiného rozšíření. Další informace o nastavení posloupnosti rozšíření najdete v tomto blogovém příspěvku: [Určení posloupnosti rozšíření ve škálovacích sadách virtuálních počítačů Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**Otázka:** Spolupracují škálovací sady se skupinami dostupnosti Azure?

**Odpověď:** Ano. Škálovací sada je implicitní skupina dostupnosti s 5 doménami selhání a 5 aktualizačními doménami. Škálovací sady s více než 100 virtuálních počítačů pokrývají více *skupin umístění* odpovídajících více skupinám dostupnosti. Další informace o skupinách umístění najdete v tématu [Práce s velkými škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-placement-groups.md). Skupina dostupnosti virtuálních počítačů může existovat ve stejné virtuální síti jako škálovací sada virtuálních počítačů. Běžnou konfigurací je umístění virtuálních počítačů řídicích uzlů, které často vyžadují jedinečnou konfiguraci, do skupiny dostupnosti, a datových uzlů do škálovací sady.

Další nejčastější dotazy ohledně škálovacích sad najdete v tématu [Nejčastější dotazy ohledně škálovacích sad virtuálních počítačů Azure](virtual-machine-scale-sets-faq.md).

