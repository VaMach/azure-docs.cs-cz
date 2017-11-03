---
title: "Zpracování oznámení o údržby pro virtuální počítače s Linuxem v Azure | Microsoft Docs"
description: "Zobrazit oznámení údržby pro Linuxové virtuální počítače běžící v Azure a spusťte údržby samoobslužné služby."
services: virtual-machines-linux
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.author: zivr
ms.openlocfilehash: be062ce9cfbe7486ef500dd9d27418cbf245d6e0
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/27/2017
---
# <a name="handling-planned-maintenance-notifications-for-linux-virtual-machines"></a>Zpracování plánované údržby oznámení pro virtuální počítače s Linuxem

Azure pravidelně provádí aktualizace ke zlepšení spolehlivosti, výkonu a zabezpečení infrastruktury hostitele pro virtuální počítače. Aktualizace jsou změny jako opravy hostitelské prostředí nebo upgradu a vyřazování z provozu hardwaru. Většina tyto aktualizace se provádějí bez žádný vliv na hostované virtuální počítače. Ale existují případy, kdy aktualizace mít vliv:

- Pokud údržby nevyžaduje restartování, Azure používá místní migrace pro pozastavení virtuálního počítače během hostitele je aktualizována.

- Pokud údržby vyžaduje restartování, zobrazí oznámení o při plánované údržby. V těchto případech jsou zadané časové okno, kde můžete spustit údržby sami, pokud vám vyhovuje.


Plánované údržby, která vyžaduje restartování, je naplánováno témata. Každý wave má jiný rozsah (oblastí).

- Nejrůznější začíná oznámení pro zákazníky. Ve výchozím nastavení se vlastník předplatného a spoluvlastníci odesílány oznámení. Můžete přidat více příjemců a zasílání zpráv možnosti jako e-mailu, SMS a pomocí Webhooků, oznámení. 
- Krátce po oznámení je nastavit okno samoobslužné služby. Během této doby můžete najít, což virtuálních počítačů je součástí této údržby wave a spusťte pomocí proaktivní znovu ho zaveďte. 
- Následující okno samoobslužné služby začne plánované údržby. V tomto okamžiku Azure plány a může požadovaná údržba se vztahují k virtuálnímu počítači.  

Cílem v má dva windows je poskytnou dostatek času spuštění údržby a restartování virtuálního počítače a zároveň budete vědět, kdy Azure automaticky spustí údržby.

Rozhraní příkazového řádku Azure, prostředí PowerShell, REST API a portálu Azure můžete použít k dotazu pro okna údržby pro virtuální počítače a spouštění samoobslužné služby údržby.

 > [!NOTE]
 > Pokud se pokusíte spustit údržby a selže, Azure označí virtuálního počítače jako **přeskočen** a není restartování počítače během plánované údržby. Místo toho budou kontaktovány v později s nový plán. 

## <a name="find-vms-scheduled-for-maintenance-using-cli"></a>Najít virtuální počítače naplánována údržba pomocí rozhraní příkazového řádku

Informace o plánované údržbě, můžete zobrazit pomocí [virtuálního počítače azure get--zobrazení instance](/cli/azure/vm?view=azure-cli-latest#az_vm_get_instance_view).
 
Informace o údržby je vrácena pouze v případě, že je plánované údržby. Pokud není že naplánována žádná Údržba této důsledky pro virtuální počítač, příkaz nevrací žádné informace údržby. 

```azure-cli
az vm get-instance-view  - g rgName  -n vmName 
```

V části MaintenanceRedeployStatus se vrátí následující hodnoty: 

| Hodnota | Popis   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Určuje, zda můžete spustit údržby pro virtuální počítač v tuto chvíli ||
| PreMaintenanceWindowStartTime         | Na začátek samoobslužné služby okno údržby při údržby můžete spustit na vašem virtuálním počítači ||
| PreMaintenanceWindowEndTime           | Konec samoobslužné služby okno údržby při údržby můžete spustit na vašem virtuálním počítači ||
| MaintenanceWindowStartTime            | Na začátek okno plánované údržby při údržby můžete spustit na vašem virtuálním počítači ||
| MaintenanceWindowEndTime              | Konec období naplánované údržby když iniciujete údržby na vašem virtuálním počítači ||
| LastOperationResultCode               | Výsledek poslední pokus o inicializaci údržby ve virtuálním počítači ||


## <a name="start-maintenance-on-your-vm-using-cli"></a>Zahájení údržby na vašem virtuálním počítači pomocí rozhraní příkazového řádku

Toto volání bude zahájení údržby na virtuálním počítači, pokud `IsCustomerInitiatedMaintenanceAllowed` je nastaven na hodnotu true.

```azure-cli
az vm perform-maintenance rgName vmName 
```

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]


## <a name="faq"></a>Nejčastější dotazy


**Otázka: Proč potřebujete Můj virtuální počítače restartovat nyní?**

**Odpověď:** sice většinou aktualizací a upgradů platformy Azure nemají vliv na dostupnost virtuálního počítače, existují případy, kdy jsme nelze vyhnout restartování virtuálních počítačů hostovaných v Azure. Shromáždily jsme několik změn, které vyžadují nám restartovat naše servery, které bude mít za následek restartování virtuálních počítačů.

**Otázka: Pokud mám postupovat podle vaše doporučení pro zajištění vysoké dostupnosti pomocí skupiny dostupnosti, jsem bezpečné?**

**Odpověď:**nastavit virtuálních počítačů nasazených v dostupnosti nebo sady škálování virtuálního počítače mají znalost problematicky domén aktualizace (UD). Při provádění údržby, Azure ctí UD omezení a nebude restartovat virtuální počítače z různých UD (v rámci stejné skupině dostupnosti).  Azure také čeká alespoň 30 minut, než budete pokračovat na další skupinu virtuálních počítačů. 

Další informace o vysoké dostupnosti odkazovat na Správa dostupnosti virtuálních počítačů s Windows v Azure nebo Správa dostupnosti virtuálních počítačích s Linuxem v Azure.

**Otázka: je nutné nastavit v jiné oblasti zotavení po havárii. Jsem bezpečné?**

**Odpověď:** každý Azure oblast je spárován s jinou oblast v rámci stejné geography (například USA, Evropa nebo Asie). Plánované aktualizace Azure se pro spárované oblasti nasazují po jedné. Tím se minimalizují prostoje a riziko výpadku aplikací. Během plánované údržby Azure naplánovat podobné okna pro uživatele ke spuštění správy, ale bude okno plánované údržby liší spárované oblasti.  

Další informace o oblastech Azure najdete v části oblasti a dostupnosti pro virtuální počítače v Azure.  Zobrazí úplný seznam dvojic regionální sem.

**Otázka: Jak mohu získat oznámení o plánované údržby?**

**Odpověď:** wave plánované údržby spustí nastavením plánu na jeden nebo více oblastech Azure. Jakmile, e-mailové oznámení se posílá vlastníky předplatného (jednu e-mailovou jedno předplatné). Další kanály a příjemce pro toto oznámení se daly konfigurovat pomocí aktivity protokolu výstrahy. V případě, že nasazujete virtuální počítač v oblasti, kde je již naplánována plánované údržbě, nebude přijímat oznámení ale spíš potřebují kontrolovat stav údržby virtuálního počítače.

**Otázka: nezobrazí jakoukoli indikaci toho plánované údržby v portálu, prostředí Powershell nebo rozhraní příkazového řádku, co je chybný?**

**Odpověď:** během plánované údržby wave pouze pro virtuální počítače, které se chystáte být ovlivněn je k dispozici informace týkající se plánované údržby. Jinými slovy Pokud se zobrazí ne data, může být, že wave údržby je dokončeno (nebo není spuštěna) nebo, virtuální počítač je již hostována v aktualizovaný server.

**Otázka: měli začít údržby na virtuální počítač?**

**Odpověď:** úlohy, které jsou nasazeny v cloudové službě, skupinu dostupnosti nebo škálovací sadu virtuálních počítačů, jsou obecně odolné vůči plánované údržby.  Během plánované údržby je v každém okamžiku dopad pouze jednu aktualizaci domény. Upozorňujeme, že pořadí aktualizace domén ovlivněný neodehrává nutně postupně.

Chcete spustit údržby v následujících případech:
- Spuštění aplikace na jednom virtuálním počítači a je třeba použít všechny údržby špičku
- Budete se muset domluvit čas údržby jako součást vaší smlouvě SLA
- Je třeba víc než 30 minut mezi každou restartování virtuálních počítačů i v rámci dostupnosti nastavit.
- Chcete vypnout celou aplikaci (několik vrstev, více domén aktualizace), aby bylo možné dokončit údržbu rychlejší.

**Otázka: je způsob, jak zjistit přesně v případě, že virtuální počítač bude mít vliv na?**

**Odpověď:** při nastavování plánu, jsme definovali časové okno několik dní. Přesný sekvencování serverů (a virtuálních počítačů) v rámci tohoto okna je ale neznámý. Zákazníci, kteří chtějí znát přesný čas pro jejich virtuální počítače můžete použít naplánované události a dotaz z virtuálního počítače a nechte si zaslat oznámení 10 minut před restartování virtuálního počítače.
  
**Otázka: jak dlouho bude to trvat restartování virtuální počítač?**

**Odpověď:** v závislosti na velikosti virtuálního počítače, restartování může trvat několik minut. Všimněte si, že v případě, že používáte cloudové služby, škálovat nastaví, nebo nastavení dostupnosti, budete mít 30 minut mezi každou skupinu virtuálních počítačů (UD). 

**Otázka: co bude prostředí v případě cloudové služby, sady škálování a Service Fabric?**

**Odpověď:** během plánované údržby je postiženo těchto platforem, zákazníků týkající se použití těchto platforem jsou považovány za bezpečné pro danou této pouze virtuální počítače v jedné upgradu domény (UD) bude mít vliv na daném okamžiku.  

**Otázka: I dostali e-mailu o vyřazení hardwaru, je to stejné jako plánované údržby?**

**Odpověď:** při vyřazení z provozu hardwaru události plánované údržby, jsme ještě nebyla zařazený nemá tento případ použití na nové prostředí.  Očekáváme, že zákazníci zmatení, v případě, že obdrží dvě podobné e-mailů o vlny dva různé plánované údržby.

**Otázka: se nezobrazí žádné informace údržby na virtuálních počítačů. Kde došlo k chybě?**

**Odpověď:** tady je několik důvodů, proč nevidíte žádné informace údržby na virtuální počítače:
1.  Používáte předplatné označen jako Microsoft interní.
2.  Virtuální počítače nejsou naplánována údržba. Může to být, že skončila wave údržby, zrušení nebo upraví tak, aby virtuální počítače jsou již vliv ho.
3.  Nemáte sloupci "údržbou" přidali do zobrazení seznamu virtuálních počítačů. Když v tomto sloupci jsme přidali výchozí zobrazení, zákazníky, kteří nakonfigurovaný tak, aby najdete v části jiné než výchozí sloupců musí ručně přidat **údržby** sloupec, který se jejich zobrazení seznamu virtuálních počítačů.

**Otázka: virtuální počítač je naplánována údržba podruhé. Proč?**

**Odpověď:** existuje několik případů použití, kde uvidíte naplánována údržba po jste už dokončili údržby-opětovném nasazení virtuálního počítače:
1.  Jsme zrušena wave údržby a restartuje s jinou datovou část. Může to být, že zjistili jsme chybný datové části a potřebujeme jen nasadit další datové části.
2.  Virtuální počítač byl *služby zacelené* do jiného uzlu z důvodu selhání hardwaru
3.  Rozhodli jste se zastavit (zrušit přidělení) a restartujte virtuální počítač
4.  Máte **vypnutí automatického** zapnuté pro virtuální počítač.


## <a name="next-steps"></a>Další kroky

Zjistěte, jak můžete zaregistrovat pro události údržby z v rámci virtuálního počítače pomocí [naplánované události](scheduled-events.md).