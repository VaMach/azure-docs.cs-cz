---
title: "Přehled škálovacích sad virtuálních počítačů Azure | Dokumentace Microsoftu"
description: "Další informace o škálovacích sadách virtuálních počítačů Azure"
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
ms.date: 03/10/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 2071debe24ef2705b2ee05ce1210a813234cf277
ms.lasthandoff: 03/10/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Co jsou škálovací sady virtuálních počítačů v Azure?
Škálovací sady virtuálních počítačů jsou výpočetním prostředkem Azure, který můžete použít k nasazení a správě sady identických virtuálních počítačů. V těchto škálovacích sadách jsou všechny virtuální počítače nakonfigurované stejně, protože jejich účelem je podporovat skutečné automatické škálování – virtuální počítače není třeba zřizovat předem. Díky tomu je snazší zajistit rozsáhlé služby nabízející vysoký výpočetní výkon a zpracovávající velké objemy dat nebo kontejnerové úlohy.

U aplikací, které vyžadují horizontální navyšování a snižování výpočetní kapacity, se škálovací operace implicitně vyvažují napříč doménami selhání a aktualizačními doménami. Úvod ke škálovacím sadám najdete v [oznámení na blogu Azure](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Více o škálovacích sadách se dozvíte v těchto videích:

* [Mark Russinovich hovoří o škálovacích sadách Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman provádí škálovacími sadami virtuálních počítačů](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Vytváření a správa škálovacích sad
Škálovací sadu můžete vytvořit na webu [Azure Portal](https://portal.azure.com) – vyberte položku *Nový* a zadejte text „škálovací“ na vyhledávacím panelu. Ve výsledcích se objeví „Škálovací sada virtuálních počítačů“. Z této položky můžete přejít k vyplnění požadovaných polí pro přizpůsobení a nasazení škálovací sady. Poznámka: Na portálu existují také možnosti nastavit základní pravidla automatického škálování v závislosti na využití procesoru.

Škálovací sady je také možné definovat a nasazovat pomocí šablon JSON a [rozhraní REST API](https://msdn.microsoft.com/library/mt589023.aspx) stejně jako jednotlivé virtuální počítače vytvořené pomocí Azure Resource Manageru. Proto můžete použít všechny standardní metody nasazení v Azure Resource Manageru. Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Sadu ukázkových šablon škálovacích sad virtuálních počítačů najdete v [úložišti GitHub pro šablony Azure pro rychlý start](https://github.com/Azure/azure-quickstart-templates) (hledejte šablony, které mají v názvu *vmss*).

Na stránce podrobností pro tyto šablony je tlačítko, které odkazuje na funkci nasazení na portálu. Pokud chcete nasadit škálovací sadu, klikněte na toto tlačítko a potom na portálu vyplňte požadované parametry. Pokud si nejste jistí, jestli určitý prostředek podporuje velká písmena nebo kombinaci velkých a malých písmen, je vždy bezpečnější hodnoty parametrů zadat malými písmeny a čísly. Můžete se podívat na video s užitečným rozborem šablony škálovací sady:

[Rozbor šablony škálovací sady virtuálních počítačů](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-scale-set-out-and-in"></a>Horizontální navyšování a snižování kapacity škálovací sady
Kapacitu škálovací sady můžete změnit na webu Azure Portal kliknutím na _Škálování_ v části _Nastavení_. 

Pokud chcete změnit kapacitu škálovací sady z příkazového řádku, v [Azure CLI](https://github.com/Azure/azure-cli) je k dispozici příkaz _scale_. Například pokud chcete nastavit kapacitu škálovací sady na 10 virtuálních počítačů:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

Pokud chcete nastavit počet virtuálních počítačů ve škálovací sadě pomocí PowerShellu,, použijte k tomu příkaz _Update-AzureRmVmss_:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Pokud chcete zvýšit nebo snížit počet virtuálních počítačů ve škálovací sadě pomocí šablony Azure Resource Manageru, změňte vlastnost představující *kapacitu* a znovu nasaďte šablonu. Takto můžete jednoduše integrovat škálovací sady s automatickým škálováním Azure nebo vytvořit vlastní škálovací vrstvu, pokud potřebujete definovat vlastní škálovací události, které nejsou podporovány automatickým škálováním Azure. 

Pokud šablonu Azure Resource Manageru znovu nasazujete s cílem změnit kapacitu, stačí definovat mnohem menší šablonu, která bude obsahovat pouze paket vlastnosti SKU s aktualizovanou kapacitou. Příklad najdete [tady](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

## <a name="autoscale"></a>Automatické škálování

Škálovací sadu můžete při vytváření na webu Azure Portal volitelně nakonfigurovat s nastavením automatického škálování a umožnit tak navyšování nebo snižování počtu virtuálních počítačů v závislosti na průměrném využití procesoru. Mnoho šablon škálovacích sad v [šablonách Azure pro rychlý start](https://github.com/Azure/azure-quickstart-templates) definuje nastavení automatického škálování. Nastavení automatického škálování můžete také přidat do stávající škálovací sady. Tady je pro ukázku skript Azure PowerShellu, který do škálovací sady přidá automatické škálování v závislosti na využití procesoru:

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

 Seznam platných metrik, na základě kterých je možné škálovat, najdete v tématu [Podporované metriky ve službě Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) v části _Microsoft.Compute/virtualMachineScaleSets_. K dispozici jsou také pokročilejší možnosti automatického škálování, včetně automatického škálování na základě plánu nebo integrace se systémy pro výstrahy pomocí webhooků.

## <a name="monitoring-your-scale-set"></a>Monitorování škálovací sady
Na webu [Azure Portal](https://portal.azure.com) je zobrazený seznam škálovacích sad a jejich vlastností. Portál také podporuje operace správy, které lze provádět se škálovacími sadami nebo s jednotlivými virtuálními počítači v rámci škálovací sady. Na portálu najdete také přizpůsobitelný graf využití prostředků. Pokud potřebujete zobrazit nebo upravit základní definici JSON prostředku Azure, můžete použít také [Průzkumníka prostředků Azure](https://resources.azure.com). Škálovací sady jsou prostředky v části Microsoft.Compute poskytovatele prostředků Azure, takže je můžete na tomto webu zobrazit rozbalením následujících odkazů:

**Subscriptions (Předplatná) -> vaše předplatné -> resourceGroups (skupiny prostředků) -> providers (poskytovatelé) -> Microsoft.Compute -> virtualMachineScaleSets (škálovací sady virtuálních počítačů) -> vaše škálovací sada -> atd.**

## <a name="scale-set-scenarios"></a>Scénáře použití škálovacích sad
Tato část uvádí některé typické scénáře použití škálovacích sad. Tyto scénáře využívají některé služby Azure na vyšší úrovni (třeba Batch, Service Fabric, Azure Container Service).

* **Instance s připojením RDP/SSH ke škálovací sadě:** Škálovací sada se vytvoří ve virtuální síti a jednotlivým virtuálním počítačům ve škálovací sadě se nebudou přidělovat veřejné IP adresy. Díky této zásadě se vyhnete nákladům a administrativní režii spojeným s přidělováním samostatných veřejných IP adres všem uzlům ve výpočetní síti. K těmto virtuálním počítačům se můžete připojit z jiných prostředků ve virtuální síti, jako jsou nástroje pro vyrovnávání zatížení a samostatné virtuální počítače, kterým můžou být přidělené veřejné IP adresy.
* **Připojení k virtuálním počítačům pomocí pravidel pro překlad adres (NAT):** Můžete vytvořit veřejnou IP adresu, přiřadit ji k nástroji pro vyrovnávání zatížení a definovat fond příchozího překladu adres, který mapuje porty u IP adresy na port virtuálního počítače ve škálovací sadě. Například:
  
  | Zdroj | Zdrojový port | Cíl | Cílový port |
  | --- | --- | --- | --- |
  |  Veřejná IP adresa |Port 50000 |vmss\_0 |Port 22 |
  |  Veřejná IP adresa |Port 50001 |vmss\_1 |Port 22 |
  |  Veřejná IP adresa |Port 50002 |vmss\_2 |Port 22 |
  
   V tomto příkladu se definují pravidla překladu adres umožňující připojení SSH ke každému virtuálnímu počítači ve škálovací sadě s použitím jedné veřejné IP adresy: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).
  
   Tady je příklad stejného nastavení s pomocí protokolu RDP a Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Připojení k virtuálním počítačům pomocí hostitele typu „jumpbox“:** Pokud ve stejné virtuální síti vytvoříte škálovací sadu a samostatný virtuální počítač, pak se všechny tyto virtuální počítače můžou navzájem připojovat pomocí svých interních IP adres, jak jsou definovány ve virtuální síti a podsíti. Pokud vytvoříte veřejnou IP adresu a přiřadíte ji k samostatnému virtuálnímu počítači, můžete se k samostatnému virtuálnímu počítači připojit pomocí RDP nebo SSH a pak se z tohoto počítače připojit k instancím škálovací sady. V tomto bodě si můžete všimnout, že jednoduchá škálovací sada je ze své podstaty bezpečnější než jednoduchý samostatný virtuální počítač s veřejnou IP adresou ve své výchozí konfiguraci.
  
   Například tato šablona nasadí jednoduchou škálovací sadu se samostatným virtuálním počítačem: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox).
* **Vyrovnávání zatížení v instancích škálovací sady:** Pokud chcete dodávat práci do výpočetního clusteru virtuálních počítačů na bázi kruhového dotazování, můžete v nástroji Azure Load Balancer nakonfigurovat odpovídající pravidla vyrovnávání zatížení úrovně&4;. Můžete definovat sondy, které budou ověřovat, že aplikace běží, pomocí portů pro příkazy ping s určeným protokolem, intervalem a cestou pro žádosti. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) podporuje jak škálovací sady, tak úroveň&7; i sofistikovanější scénáře vyrovnávání zatížení.
  
   Tento příklad vytvoří škálovací sadu, ve které běží webové servery Apache a na jednotlivých virtuálních počítačích se používá nástroj pro vyrovnávání zatížení: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) (podívejte se na typ prostředku Microsoft.Network/loadBalancers a položky networkProfile a extensionProfile v části virtualMachineScaleSet).

   Tento příklad využívá službu Application Gateway. Linux:  [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway). Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) 

* **Nasazení škálovací sady jako výpočetního clusteru ve správci clusteru PaaS:** Škálovací sady e někdy označují jako role pracovních procesů nové generace. Přestože je tento popis výstižný, existuje riziko, že dojde k záměně funkcí škálovacích sad s funkcemi služby Azure Cloud Services. Škálovací sady skutečně představují „roli pracovního procesu“, a to v tom smyslu, že poskytují generalizovaný výpočetní prostředek, který je nezávislý na platformě nebo modulu runtime, umožňuje přizpůsobení a integruje se s IaaS s nástrojem Azure Resource Manager.
  
   Role pracovního procesu služby Cloud Services má sice omezení z hlediska podpory platforem nebo modulů runtime (k dispozici jsou jen image pro platformu Windows), ale obsahuje i další služby, jako je prohození virtuálních IP adres, konfigurovatelné nastavení upgradu nebo nastavení specifické pro nasazení modulu runtime a aplikace. Ty *ještě* nejsou ve škálovacích sadách k dispozici a možná se tam ani neobjeví, protože je budou zajišťovat služby PaaS vyšší úrovně, jako je Service Fabric. Na škálovací sady se můžete dívat jako na infrastrukturu, která podporuje model PaaS. Na této infrastruktuře jsou postavena řešení PaaS, jako je [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/).
  
   Jako příklad tohoto přístupu nasadí [Azure Container Service](https://azure.microsoft.com/services/container-service/) cluster na základě škálovacích sad pomocí orchestrátoru kontejneru: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="scale-set-performance-and-scale-guidance"></a>Pokyny týkající se výkonu a škálování u škálovacích sad
* V jedné škálovací sadě je podporováno až 1 000 virtuálních počítačů. Pokud vytváříte a nahráváte vlastní image virtuálních počítačů, platí omezení na 100 virtuálních počítačů. Důležité informace o používání velkých škálovacích sad najdete v tématu [Práce s velkými škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-placement-groups.md).
* K používání škálovacích sad není potřeba předem vytvářet účty úložiště Azure. Škálovací sady podporují službu Azure Managed Disks, díky které nemusíte mít starosti o výkon s ohledem na počet disků v účtu úložiště. Další informace najdete v tématu [Škálovací sady virtuálních počítačů Azure a spravované disky](virtual-machine-scale-sets-managed-disks.md).
* Zvažte použití služby Azure Storage úrovně Premium namísto úrovně Standard pro rychlejší a předvídatelnější zřizování virtuálních počítačů a vy.lepšení výkonu vstupně-výstupních operací.
* Počet virtuálních počítačů, které můžete vytvořit, je omezen kvótou pro počet jader v oblasti, ve které nasazení provádíte. Možná budete muset požádat zákaznickou podporu o navýšení vaší kvóty pro výpočetní prostředky, i když už dnes máte vysoký limit pro jádra, který platí pro cloudové služby Azure. Dotaz na kvótu spustíte pomocí následujícího příkazu Azure CLI: `azure vm list-usage` nebo následujícího příkazu PowerShellu: `Get-AzureRmVMUsage` (pokud používáte nižší verzi PowerShellu než 1.0, použijte `Get-AzureVMUsage`).

## <a name="scale-set-frequently-asked-questions"></a>Nejčastější dotazy ke škálovacím sadám
**Otázka:** Kolik virtuálních počítačů může obsahovat škálovací sada?

**Odpověď:** Škálovací sada může obsahovat 0 až 1 000 virtuálních počítačů založených na imagích platformy nebo 0 až 100 virtuálních počítačů založených na vlastních imagích. 

**Otázka:** Podporují se ve škálovacích sadách datové disky?

**Odpověď:** Ano. Škálovací sada může definovat konfiguraci připojených datových jednotek, která se použije na všechny virtuální počítače v sadě. Další informace najdete v tématu (Škálovací sady Azure a připojené datové disky)[virtual-machine-scale-sets-attached-disks.md]. Další možnosti ukládání dat zahrnují:

* Soubory Azure (sdílené jednotky SMB)
* Jednotka operačního systému
* Dočasné jednotky (místní, nezálohované úložiště Azure)
* Datová služba Azure (např. tabulky Azure, objekty blob Azure)
* Externí datová služba (např. vzdálená databáze)

**Otázka:** Které oblasti Azure podporují škálovací sady?

**Odpověď:** Všechny oblasti podporují škálovací sady.

**Otázka:** Jak se vytváří škálovací sada s použitím vlastní image?

**Odpověď:** Na základě virtuálního pevného disku vlastní image vytvoříte spravovaný disk, na který budete odkazovat v šabloně škálovací sady. Tady je příklad: [https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**Otázka:** Pokud snížím kapacitu škálovací sady z 20 na 15, které virtuální počítače budou odebrány?

**Odpověď:** Virtuální počítače se ze škálovací sady odebírají rovnoměrně napříč upgradovací doménami a doménami selhání, aby se maximalizovala dostupnost. Nejprve se odeberou virtuální počítače s nejvyšším ID.

**Otázka:** A co když pak zvýším kapacitu z 15 až 18?

**Odpověď:** Pokud zvýšíte kapacitu na 18, vytvoří se 3 nové virtuální počítače. ID instance virtuálního počítače se přírůstkově zvýší oproti předchozí nejvyšší hodnotě (např. 20, 21, 22). Virtuální počítače se vyvažují mezi doménami selhání a aktualizačními doménami.

**Otázka:** Pokud ve škálovací sadě používám několik rozšíření, je možné vynucovat určitou posloupnost provádění?

**Odpověď:** Ne přímo, ale u rozšíření customScript by váš skript mohl čekat na dokončení jiného rozšíření ([například tím, že bude monitorovat protokol rozšíření](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Další informace o nastavení posloupnosti rozšíření najdete v tomto blogovém příspěvku: [Určení posloupnosti rozšíření ve škálovacích sadách virtuálních počítačů Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**Otázka:** Spolupracují škálovací sady se skupinami dostupnosti Azure?

**Odpověď:** Ano. Škálovací sada je implicitní skupina dostupnosti s 5 doménami selhání a 5 aktualizačními doménami. Škálovací sady s více než 100 virtuálních počítačů pokrývají více skupin umístění odpovídajících více skupinám dostupnosti. Další informace o skupinách umístění najdete v tématu [Práce s velkými škálovacími sadami virtuálních počítačů](virtual-machine-scale-sets-placement-groups.md). Skupina dostupnosti virtuálních počítačů může existovat ve stejné virtuální síti jako škálovací sada virtuálních počítačů. Běžnou konfigurací je umístění virtuálních počítačů řídicích uzlů, které často vyžadují jedinečnou konfiguraci, do skupiny dostupnosti, a datových uzlů do škálovací sady.

Další nejčastější dotazy ohledně škálovacích sad najdete v tématu [Nejčastější dotazy ohledně škálovacích sad virtuálních počítačů Azure](virtual-machine-scale-sets-faq.md).

