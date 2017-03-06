---
title: "Přehled škálovacích sad virtuálních počítačů | Dokumentace Microsoftu"
description: "Další informace o škálovacích sadách virtuálních počítačů"
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
ms.date: 1/25/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 54673cbc69e418bdbe7cb5791dfb5d20e7ebcb9f
ms.openlocfilehash: 2294587fd3f978a3f8383112ece329b47307db7a
ms.lasthandoff: 03/01/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Co jsou škálovací sady virtuálních počítačů v Azure?
Škálovací sady virtuálních počítačů jsou výpočetním prostředkem Azure, který můžete použít k nasazení a správě sady identických virtuálních počítačů. V těchto sadách jsou všechny virtuální počítače nakonfigurované stejně, protože jejich účelem je podporovat skutečné automatické škálování – virtuální počítače není třeba zřizovat předem. Díky tomu je snazší zajistit rozsáhlé služby nabízející vysoký výpočetní výkon a zpracovávající velké objemy dat nebo kontejnerové úlohy.

U aplikací, které vyžadují horizontální navyšování a snižování výpočetní kapacity, se škálovací operace implicitně vyvažují napříč doménami selhání a aktualizačními doménami. Úvod ke škálovacím sadám virtuálních počítačů najdete v tématu [Oznámení k Blogu Azure](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/).

Více o škálovacích sadách virtuálních počítačů se dozvíte v těchto videích:

* [Mark Russinovich hovoří o škálovacích sadách Azure](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman provádí škálovacími sadami virtuálních počítačů](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>Vytváření a správa škálovacích sad virtuálních počítačů
Škálovací sadu virtuálních počítačů můžete vytvořit na portálu [Azure Portal](https://portal.azure.com) – vyberte položku *Nový* a zadejte text „škálovací“ na vyhledávacím panelu. Ve výsledcích se objeví „Škálovací sada virtuálních počítačů“. Z této položky můžete přejít k vyplnění požadovaných polí pro přizpůsobení a nasazení škálovací sady. Poznámka: Na portálu existují také možnosti nastavit základní pravidla automatického škálování v závislosti na využití procesoru.

Škálovací sady virtuálních počítačů je také možné definovat a nasazovat pomocí šablon JSON a [rozhraní REST API](https://msdn.microsoft.com/library/mt589023.aspx) stejně jako jednotlivé virtuální počítače v Azure Resource Manageru. Proto můžete použít všechny standardní metody nasazení v Azure Resource Manageru. Další informace o šablonách najdete v tématu o [vytváření šablon Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).

Sadu příkladových šablon škálovacích sad virtuálních počítačů najdete v [úložišti GitHub pro šablony Quickstart určené pro Azure](https://github.com/Azure/azure-quickstart-templates). (Hledejte šablony, které mají v názvu *vmss*.)

Na stránce podrobností pro tyto šablony se zobrazí tlačítko, které odkazuje na funkci nasazení na portálu. Pokud chcete škálovací sadu virtuálních počítačů nasadit, klikněte na toto tlačítko a potom na portálu vyplňte požadované parametry. Pokud si nejste jistí, jestli určitý prostředek podporuje velká písmena nebo kombinaci velkých a malých písmen, je vždy bezpečnější hodnoty parametrů zadat malými písmeny a čísly. Můžete se podívat na video s užitečným rozborem šablony škálovací sady:

[Rozbor šablony škálovací sady virtuálních počítačů](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Horizontální navyšování a snižování kapacity u škálovací sady virtuálních počítačů
Pokud chcete zvýšit nebo snížit počet virtuálních počítačů ve škálovací sadě virtuálních počítačů, jednoduše změňte vlastnost představující *kapacitu* a znovu nasaďte šablonu. Takto jednoduše můžete vytvořit i vlastní škálovací vrstvu, pokud budete chtít definovat vlastní škálovací události, které nejsou podporovány automatickým škálováním Azure.

Pokud šablonu znovu nasazujete s cílem změnit kapacitu, stačí definovat mnohem menší šablonu, která bude obsahovat pouze paket vlastnosti SKU s aktualizovanou kapacitou. Příklad najdete [tady](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).

Pokud si chcete projít postup vytvoření škálovací sady, která se škáluje automaticky, nahlédněte do tématu [Automatické škálování počítačů ve škálovací sadě virtuálních počítačů](virtual-machine-scale-sets-windows-autoscale.md).

## <a name="monitoring-your-vm-scale-set"></a>Monitorování škálovací sady virtuálních počítačů
[Azure Portal](https://portal.azure.com) uvádí škálovací sady a zobrazuje základní vlastnosti a operace, včetně seznamu virtuálních počítačů v sadě a grafu využití prostředků. Další podrobnosti si můžete zobrazit v [Průzkumníkovi prostředků Azure](https://resources.azure.com). Škálovací sady virtuálních počítačů jsou prostředky v části Microsoft.Compute, takže z tohoto webu si je můžete zobrazit rozbalením následujících odkazů:

**Předplatná -> vaše předplatné -> resourceGroups -> poskytovatelé -> Microsoft.Compute -> virtualMachineScaleSets -> vaše škálovací sada virtuálních počítačů -> atd.**

## <a name="vm-scale-set-scenarios"></a>Scénáře použití škálovacích sad virtuálních počítačů
Tato část uvádí některé typické scénáře použití škálovacích sad virtuálních počítačů. Tyto scénáře využívají některé služby Azure na vyšší úrovni (třeba Batch, Service Fabric, Azure Container Service).

* **Instance s připojením RDP/SSH ke škálovací sadě virtuálních počítačů:** Škálovací sada virtuálních počítačů se vytvoří v síti VNET a jednotlivým virtuálním počítačům ve škálovací sadě se nebudou přidělovat veřejné IP adresy. To je dobrá věc, protože obecně je žádoucí se vyhnout výdajům a administrativní režii spojeným s přidělováním samostatných veřejných IP adres všem bezstavovým prostředkům ve výpočetní síti a protože se k těmto virtuálním počítačům můžete snadno připojit z jiných prostředků v síti VNET, včetně těch, které mají veřejné IP adresy, jako jsou nástroje pro vyrovnávání zatížení nebo samostatné virtuální počítače.
* **Připojení k virtuálním počítačům pomocí pravidel pro překlad adres (NAT):** Můžete vytvořit veřejnou IP adresu, přiřadit ji k nástroji pro vyrovnávání zatížení a definovat fond příchozího překladu adres, který mapuje porty u IP adresy na port virtuálního počítače ve škálovací sadě. Například:
  
  | Zdroj | Zdrojový port | Cíl | Cílový port |
  | --- | --- | --- | --- |
  |  Veřejná IP adresa |Port 50000 |vmss\_0 |Port 22 |
  |  Veřejná IP adresa |Port 50001 |vmss\_1 |Port 22 |
  |  Veřejná IP adresa |Port 50002 |vmss\_2 |Port 22 |
  
   Zde je příklad vytvoření škálovací sady virtuálních počítačů, která používá pravidla pro překlad adres (NAT) umožňující připojení SSH ke každému virtuálnímu počítači v sadě s použitím jedné veřejné IP adresy: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Tady je příklad stejného nastavení s pomocí protokolu RDP a Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Připojování k virtuálním počítačům pomocí hostitele typu „jumpbox“:** Pokud ve stejné síti VNET vytvoříte škálovací sadu virtuálních počítačů a samostatný virtuální počítač, pak se všechny tyto virtuální počítače můžou navzájem připojovat pomocí svých interních IP adres, jak jsou definovány v síti VNET a podsíti. Pokud vytvoříte veřejnou IP adresu a přiřadíte ji k samostatnému virtuálnímu počítači, můžete nastavit připojení RDP nebo SSH k samostatnému virtuálnímu počítači a pak se z tohoto počítače připojit k vašim instancím škálovací sady virtuálních počítačů. V tomto bodě si můžete povšimnout, že jednoduchá škálovací sada virtuálních počítačů je ze své podstaty bezpečnější než jednoduchý samostatný virtuální počítač s veřejnou IP adresou ve své výchozí konfiguraci.
  
   Například tato šablona nasadí jednoduchou škálovací sadu se samostatným virtuálním počítačem: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox).
* **Vyrovnávání zatížení v instancích škálovací sady virtuálních počítačů:** Pokud chcete dodávat práci do výpočetního clusteru virtuálních počítačů na bázi kruhového dotazování, můžete v nástroji Azure Load Balancer nakonfigurovat odpovídající pravidla vyrovnávání zatížení úrovně&4;. Můžete definovat sondy, které budou ověřovat, že aplikace běží, pomocí portů pro příkazy ping s určeným protokolem, intervalem a cestou pro žádosti. Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) podporuje jak škálovací sady, tak úroveň&7; i sofistikovanější scénáře vyrovnávání zatížení.
  
   Zde je příklad, v rámci něhož se vytvoří škálovací sada virtuálních počítačů, na kterých běží webové servery Apache, a na jednotlivých virtuálních počítačích se používá nástroj pro vyrovnávání zatížení: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) (podívejte se na typ prostředku Microsoft.Network/loadBalancers a položky networkProfile a extensionProfile v části virtualMachineScaleSet).
* **Nasazení škálovací sady virtuálních počítačů jako výpočetního clusteru ve správci clusteru PaaS:** Škálovací sady virtuálních počítačů se někdy označují jako role pracovních procesů nové generace. Tento popis je výstižný, ale je tu riziko, že dojde k záměně funkcí škálovacích sad s funkcemi role pracovního procesu PaaS v1. Škálovací sady virtuálních počítačů skutečně představují „roli pracovního procesu“, a to v tom smyslu, že poskytují generalizovaný výpočetní prostředek, který je nezávislý na platformě nebo modulu runtime, umožňuje přizpůsobení a integruje se s IaaS s nástrojem Azure Resource Manager.
  
   Role pracovního procesu PaaS v1 má sice omezení z hlediska podpory platforem nebo modulů runtime (k dispozici jsou jen image pro platformu Windows), ale obsahuje i další služby, jako je prohození virtuálních IP adres, konfigurovatelné nastavení upgradu nebo nastavení specifické pro nasazení modulu runtime a aplikace. Ty *ještě* nejsou ve škálovacích sadách k dispozici a možná se tam ani neobjeví, protože je budou zajišťovat služby PaaS vyšší úrovně, jako je Service Fabric. Z těchto důvodů můžete na škálovací sady virtuálních počítačů pohlížet jako na infrastrukturu, která podporuje PaaS. To znamená, že nad škálovací sady virtuálních počítačů je možné jako na škálovatelnou výpočetní vrstvu nasadit řešení PaaS, jako je Service Fabric, nebo správce clusteru, jako je Mesos.
  
   Jako příklad tohoto přístupu nasadí Azure Container Service cluster na základě škálovacích sad pomocí orchestrátoru kontejneru: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Pokyny týkající se výkonu a škálování u škálovacích sad virtuálních počítačů
* Při použití škálovacích sad s nespravovanými disky nepočítejte s více než 20 virtuálními počítači na jeden účet úložiště (pokud jste vlastnost pro *nadměrné zřízení * nenastavili na false – v tom případě jich může být až 40). Na spravované disky se tato omezení na jeden účet úložiště nevztahují.
* Pokud použijete škálovací sady s nespravovanými disky, první písmena v názvech účtů úložiště co nejvíce rozprostřete. Příklady, jak to udělat, najdete v ukázkových šablonách VMSS v [šabloně Quickstart pro Azure](https://github.com/Azure/azure-quickstart-templates/).
* Pokud používáte vlastní virtuální počítače s nespravovanými disky, nepočítejte s více než 40 virtuálními počítači ve škálovací sadě na jeden účet úložiště. Než zahájíte nasazování škálovací sady virtuálních počítačů, musíte mít na účet úložiště zkopírovanou image. Škálovací sady se spravovanými disky podporují až 100 virtuálních počítačů založených na vlastních imagích. Další informace najdete v nejčastějších dotazech.
* Nekalkulujte s více než 4096 virtuálními počítači v jedné síti VNET.
* Počet virtuálních počítačů, které můžete vytvořit, je omezen kvótou pro počet jader v oblasti, ve které nasazení provádíte. Možná budete muset požádat zákaznickou podporu o navýšení vaší kvóty pro výpočetní prostředky, i když už dnes máte vysoký limit pro jádra, který platí pro cloudové služby nebo IaaS v1. Dotaz na kvótu spustíte pomocí následujícího příkazu na Azure CLI: `azure vm list-usage`. A následujícího příkazu pro PowerShell: `Get-AzureRmVMUsage` (pokud používáte nižší verzi prostředí PowerShell než 1.0, použijte `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>Nejčastější dotazy ke škálovacím sadám virtuálních počítačů
**Otázka:** Kolik virtuálních počítačů může obsahovat škálovací sada?

**Odpověď:** Se spravovanými disky může škálovací sada obsahovat 0 až 1 000 virtuálních počítačů založených na imagích platformy nebo 0 až 100 virtuálních počítačů založených na vlastních imagích. U nespravovaných disků (kdy definujete vlastní účty úložiště) platí omezení na 100 virtuálních počítačů pro image platformy a až 40 virtuálních počítačů pro vlastní image (pokud je vlastnost *nadměrné zřízení* nastavena na false, ve výchozím nastavení je to 20), protože vlastní image u nespravovaných disků jsou omezeny na jeden účet úložiště.

**Otázka:** Podporují se ve škálovacích sadách datové disky?

**Odpověď:** Ano. Škálovací sada s nadefinovaným spravovaným úložištěm může definovat konfiguraci připojených datových jednotek, která se použije na všechny virtuální počítače v sadě. Škálovací sady, které nemají definované spravované úložiště, neobsahují připojené datové jednotky. Další možnosti ukládání dat zahrnují:

* Soubory Azure (sdílené jednotky SMB)
* Jednotka operačního systému
* Dočasné jednotky (místní, nezálohované úložiště Azure)
* Datová služba Azure (např. tabulky Azure, objekty blob Azure)
* Externí datová služba (např. vzdálená databáze)

**Otázka:** Které oblasti Azure podporují škálovací sady virtuálních počítačů?

**Odpověď:** Všechny oblasti podporují škálovací sady virtuálních počítačů.

**Otázka:** Jak se vytváří škálovací sada virtuálních počítačů s použitím vlastní image?

**Odpověď:** Ponechte vlastnost vhdContainers prázdnou (nebo ji vynechte úplně) a zadejte identifikátor URI vlastnosti image. Například: [201-vmss-windows-customimage].(https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-customimage)


**Otázka:** Pokud snížím kapacitu škálovací sady virtuálních počítačů z 20 na 15, které virtuální počítače budou odebrány?

**Odpověď:** Virtuální počítače se ze škálovací sady odebírají rovnoměrně napříč upgradovací doménami a doménami selhání, aby se maximalizovala dostupnost. Nejprve se odeberou virtuální počítače s nejvyšším ID.

**Otázka:** A co když pak zvýším kapacitu z 15 až 18?

**Odpověď:** Pokud zvýšíte kapacitu na 18, vytvoří se 3 nové virtuální počítače. ID instance virtuálního počítače se přírůstkově zvýší oproti předchozí nejvyšší hodnotě (např. 20, 21, 22). Virtuální počítače se vyvažují mezi doménami selhání a aktualizačními doménami.

**Otázka:** Pokud ve škálovací sadě virtuálních počítačů používám několik rozšíření, je možné vynucovat určitou posloupnost provádění?

**Odpověď:** Ne přímo, ale u rozšíření customScript by váš skript mohl čekat na dokončení jiného rozšíření ([například tím, že bude monitorovat protokol rozšíření](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Další informace o nastavení posloupnosti rozšíření najdete v tomto blogovém příspěvku: [Určení posloupnosti rozšíření ve škálovacích sadách virtuálních počítačů Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**Otázka:** Spolupracují škálovací sady virtuálních počítačů se skupinami dostupnosti Azure?

**Odpověď:** Ano. Škálovací sada virtuálních počítačů je implicitní skupina dostupnosti s 5 doménami selhání a 5 aktualizačními doménami. Pod virtualMachineProfile nemusíte nic konfigurovat. Škálovací sady virtuálních počítačů s více než 100 virtuálních počítačů pokrývají více skupin umístění odpovídajících více skupinám dostupnosti. Skupina dostupnosti virtuálních počítačů může existovat ve stejné virtuální síti jako škálovací sada virtuálních počítačů. Běžnou konfigurací je umístění virtuálních počítačů řídicích uzlů, které často vyžadují jedinečnou konfiguraci, do skupiny dostupnosti, a datových uzlů do škálovací sady.

