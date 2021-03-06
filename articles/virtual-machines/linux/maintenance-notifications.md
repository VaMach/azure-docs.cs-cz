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
ms.date: 12/15/2017
ms.author: zivr
ms.openlocfilehash: d551a62a59e0a7f63f5fd4862680a271de659a19
ms.sourcegitcommit: 7d4b3cf1fc9883c945a63270d3af1f86e3bfb22a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/08/2018
---
# <a name="handling-planned-maintenance-notifications-for-linux-virtual-machines"></a>Zpracování plánované údržby oznámení pro virtuální počítače s Linuxem

Azure pravidelně provádí aktualizace ke zlepšení spolehlivosti, výkonu a zabezpečení infrastruktury hostitele pro virtuální počítače. Aktualizace jsou změny jako opravy hostitelské prostředí nebo upgradu a vyřazování z provozu hardwaru. Většina těchto aktualizací se provést bez žádný vliv na hostované virtuální počítače. Ale existují případy, kdy aktualizace mít vliv:

- Pokud údržby nevyžaduje restartování, Azure používá místní migrace pro pozastavení virtuálního počítače během hostitele je aktualizována.

- Pokud údržby vyžaduje restartování, zobrazí oznámení o při plánované údržby. V těchto případech jsou zadané časové okno, kde můžete spustit údržby sami, pokud vám vyhovuje.


Plánované údržby, která vyžaduje restartování je naplánováno témata. Každý wave má jiný rozsah (oblastí).

- Nejrůznější začíná oznámení pro zákazníky. Ve výchozím nastavení se vlastník předplatného a spoluvlastníci odesílány oznámení. Můžete přidat další možnosti zasílání zpráv jako e-mailu, SMS a pomocí webhooků a příjemce k oznámení pomocí Azure [aktivity protokolu výstrahy](../../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).  
- V době oznámení *samoobslužné služby okno* je k dispozici. Během této doby můžete najít, které virtuální počítače jsou uvedeny v této wave a proaktivní údržby spustit podle vlastních potřeb plánování.
- Po okno samoobslužné služby *plánované údržby* začne. V určitém okamžiku během této doby Azure plány a může požadovaná údržba se vztahují k virtuálnímu počítači. 

Cílem v má dva windows je poskytnou dostatek času spuštění údržby a restartování virtuálního počítače a zároveň budete vědět, kdy Azure automaticky spustí údržby.


Portál Azure, prostředí PowerShell, REST API a rozhraní příkazového řádku můžete použít k dotazu pro okna údržby pro virtuální počítače a spouštění samoobslužné služby údržby.

 > [!NOTE]
 > Pokud spustíte údržby a požadavek selže, Azure označí virtuálního počítače jako **přeskočen**. Už nebudete moci používat možnost zákazníka iniciované údržby. Virtuální počítač bude muset restartovat Azure během fáze plánované údržby.


 
## <a name="should-you-start-maintenance-using-during-the-self-service-window"></a>Měli spuštění údržby pomocí během časového období samoobslužné služby?  

Následující pokyny by měly pomoci můžete se rozhodnout, zda by měl použít tuto funkci a spuštění vlastní během údržby.

> [!NOTE] 
> Samoobslužné služby údržby nemusí být dostupné pro všechny virtuální počítače. Chcete-li určit, zda je k dispozici pro virtuální počítač proaktivní znovu ho zaveďte, vyhledejte **nyní spustit** ve stavu údržby. Samoobslužné služby údržby není aktuálně k dispozici pro cloudové služby (Role Web nebo Worker), Service Fabric a sady škálování virtuálního počítače.


Samoobslužné služby údržby se nedoporučuje pro nasazení pomocí **skupiny dostupnosti** vzhledem k tomu, že toto jsou vysoce dostupná nastavení, kde ovlivní pouze jednu aktualizaci domény v každém okamžiku. 
    - Umožní Azure aktivační události údržby, ale mějte na paměti, že pořadí aktualizace domén ovlivněný neodehrává nutně postupně a že je 30 minut pozastavení mezi doménami aktualizace.
    - Pokud vám záleží hlavně k dočasné ztrátě některých vaše kapacita (počet domén 1 nebo update), je lze snadno kompenzovat přidělí přidání instance během údržby. 

**Nemáte** použít samoobslužné služby údržby v následujících scénářích: 
    - Pokud vypnete virtuální počítače často, buď ručně, používá DevTest labs, pomocí automatického vypnutí nebo následující plánu, se může vrátit stav údržby a proto způsobit další výpadku.
    - Na krátkodobou virtuálních počítačích, které znáte, se odstraní před koncem wave údržby. 
    - Pro úlohy se stavem velké uložené v místní disk (dočasné), který je žádoucí, aby nakládat při aktualizaci. 
    - V případech, kde změníte velikost virtuálního počítače, často, protože by bylo možné vrátit stav údržby. 
    - Pokud přijaly naplánované události, které chcete povolit proaktivní převzetí služeb při selhání nebo řádné vypnutí úlohy, 15 minut před zahájením údržby vypnutí

**Použití** samoobslužné služby údržby, pokud máte v úmyslu spustit virtuální počítač bez přerušení během fáze plánované údržby a žádná z výše uvedených čítačů údajů platí. 

Je nejvhodnější použít samoobslužné služby údržby v následujících případech:
    - Budete muset komunikovat přesný údržby pro správu nebo koncového zákazníka. 
    - Potřebujete k dokončení údržby podle konkrétního data. 
    - Je nutné určit pořadí údržby, například vícevrstvé aplikace k zajištění bezpečného obnovení.
    - Je nutné o čase obnovení virtuálního počítače mezi dvěma doménami aktualizace (UDs) více než 30 minut. K řízení času mezi doménami aktualizace, musíte aktivovat údržby na vaše virtuální počítače jednu aktualizační doménu (UD) v čase.



## <a name="find-vms-scheduled-for-maintenance-using-cli"></a>Najít virtuální počítače naplánována údržba pomocí rozhraní příkazového řádku

Informace o plánované údržbě, můžete zobrazit pomocí [virtuálního počítače azure get--zobrazení instance](/cli/azure/vm?view=azure-cli-latest#az_vm_get_instance_view).
 
Informace o údržby je vrácena pouze v případě, že je plánované údržby. Pokud není že naplánována žádná Údržba této důsledky pro virtuální počítač, příkaz nevrací žádné informace údržby. 

```azure-cli
az vm get-instance-view -g rgName -n vmName
```

V části MaintenanceRedeployStatus se vrátí následující hodnoty: 

| Hodnota | Popis   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Určuje, zda můžete spustit údržby pro virtuální počítač v tuto chvíli ||
| PreMaintenanceWindowStartTime         | Na začátek samoobslužné služby okno údržby při údržby můžete spustit na vašem virtuálním počítači ||
| PreMaintenanceWindowEndTime           | Konec samoobslužné služby okno údržby při údržby můžete spustit na vašem virtuálním počítači ||
| MaintenanceWindowStartTime            | Na začátek okno plánované údržby, ve kterém Azure zahájí údržby na vašem virtuálním počítači ||
| MaintenanceWindowEndTime              | Konec okno plánované údržby, ve kterém Azure zahájí údržby na vašem virtuálním počítači ||
| LastOperationResultCode               | Výsledek poslední pokus o inicializaci údržby ve virtuálním počítači ||




## <a name="start-maintenance-on-your-vm-using-cli"></a>Zahájení údržby na vašem virtuálním počítači pomocí rozhraní příkazového řádku

Toto volání bude zahájení údržby na virtuálním počítači, pokud `IsCustomerInitiatedMaintenanceAllowed` je nastaven na hodnotu true.

```azure-cli
az vm perform-maintenance rgName vmName 
```

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="classic-deployments"></a>Nasazení Classic

Pokud máte starší verze virtuálních počítačů, které byly nasazeny pomocí modelu nasazení classic, můžete pomocí rozhraní příkazového řádku 1.0 dotaz pro virtuální počítače a zahájení údržby.

Ujistěte se, že jste do správného režimu pro práci s classic virtuálních počítačů zadáním:

```
azure config mode asm
```

Chcete-li získat stav údržby virtuální počítač s názvem *Můjvp*, zadejte:

```
azure vm show myVM 
``` 

Ke spuštění údržby na klasické virtuální počítač s názvem *Můjvp* v *Moje_služba* služby a *myDeployment* nasazení, typ:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```


## <a name="faq"></a>Nejčastější dotazy


**Otázka: Proč potřebujete Můj virtuální počítače restartovat nyní?**

**Odpověď:** sice většinou aktualizací a upgradů platformy Azure nemají vliv na dostupnost virtuálního počítače, existují případy, kdy jsme nelze vyhnout restartování virtuálních počítačů hostovaných v Azure. Shromáždily jsme několik změn, které vyžadují nám restartovat naše servery, které bude mít za následek restartování virtuálních počítačů.

**Otázka: Pokud mám postupovat podle vaše doporučení pro zajištění vysoké dostupnosti pomocí skupiny dostupnosti, jsem bezpečné?**

**Odpověď:** nastavit virtuálních počítačů nasazených v dostupnosti nebo sady škálování virtuálního počítače mají znalost problematicky domén aktualizace (UD). Při provádění údržby, Azure ctí UD omezení a nebude restartovat virtuální počítače z různých UD (v rámci stejné skupině dostupnosti).  Azure také čeká alespoň 30 minut, než budete pokračovat na další skupinu virtuálních počítačů. 

Další informace o vysoké dostupnosti najdete v tématu [oblastech a dostupnosti pro virtuální počítače v Azure](regions-and-availability.MD).

**Otázka: Jak mohu získat oznámení o plánované údržby?**

**Odpověď:** wave plánované údržby spustí nastavením plánu na jeden nebo více oblastech Azure. Jakmile, e-mailové oznámení se posílá vlastníky předplatného (jednu e-mailovou jedno předplatné). Další kanály a příjemce pro toto oznámení se daly konfigurovat pomocí aktivity protokolu výstrahy. V případě, že nasazujete virtuální počítač v oblasti, kde je již naplánována plánované údržbě, nebude přijímat oznámení ale spíš potřebují kontrolovat stav údržby virtuálního počítače.

**Otázka: nezobrazí jakoukoli indikaci toho plánované údržby v portálu, prostředí Powershell nebo rozhraní příkazového řádku, co je chybný?**

**Odpověď:** během plánované údržby wave pouze pro virtuální počítače, které se chystáte být ovlivněn je k dispozici informace týkající se plánované údržby. Jinými slovy Pokud se zobrazí ne data, může být, že wave údržby je dokončeno (nebo není spuštěna) nebo, virtuální počítač je již hostována v aktualizovaný server.

**Otázka: je způsob, jak zjistit přesně v případě, že virtuální počítač bude mít vliv na?**

**Odpověď:** při nastavování plánu, jsme definovali časové okno několik dní. Přesný sekvencování serverů (a virtuálních počítačů) v rámci tohoto okna je ale neznámý. Zákazníci, kteří chtějí znát přesný čas pro jejich virtuální počítače můžete použít [naplánované události](scheduled-events.md) dotaz ze v rámci virtuálního počítače a nechte si zaslat oznámení 15 minut před restartování virtuálního počítače.

**Otázka: jak dlouho bude to trvat restartování virtuální počítač?**

**Odpověď:** v závislosti na velikosti virtuálního počítače, restartování může trvat několik minut během časového období údržby samoobslužné služby. Během Azure inicioval restartování počítače v okně plánované údržby restartování obvykle trvat asi 25 minut. Všimněte si, že v případě, že používáte cloudové služby (Role Web nebo Worker), nastaví škálování virtuálního počítače nebo skupiny dostupnosti, budete mít 30 minut mezi každou skupinou z virtuálních počítačů (UD) během plánované údržby.

**Otázka: co je prostředí v případě cloudové služby (Role Web nebo Worker), Service Fabric a sady škálování virtuálního počítače?**

**Odpověď:** během plánované údržby je postiženo těchto platforem, zákazníků týkající se použití těchto platforem jsou považovány za bezpečné pro danou této pouze virtuální počítače v jedné upgradu domény (UD) bude mít vliv na daném okamžiku. Samoobslužné služby údržby není aktuálně k dispozici pro cloudové služby (Role Web nebo Worker), Service Fabric a sady škálování virtuálního počítače.

**Otázka: I dostali e-mailu o vyřazení hardwaru, je to stejné jako plánované údržby?**

**Odpověď:** při vyřazení z provozu hardwaru události plánované údržby, jsme ještě nebyla zařazený nemá tento případ použití na nové prostředí.  

**Otázka: se nezobrazí žádné informace údržby na virtuálních počítačů. Kde došlo k chybě?**

**Odpověď:** tady je několik důvodů, proč nevidíte žádné informace údržby na virtuální počítače:
1.  Používáte předplatné označen jako Microsoft interní.
2.  Virtuální počítače nejsou naplánována údržba. Může to být, že skončila wave údržby, zrušení nebo upraví tak, aby virtuální počítače jsou již vliv ho.
3.  Nemáte **údržby** sloupec přidán do zobrazení seznamu virtuálních počítačů. Když v tomto sloupci jsme přidali výchozí zobrazení, zákazníky, kteří nakonfigurovaný tak, aby najdete v části jiné než výchozí sloupců musí ručně přidat **údržby** sloupec, který se jejich zobrazení seznamu virtuálních počítačů.

**Otázka: virtuální počítač je naplánována údržba podruhé. Proč?**

**Odpověď:** existuje několik případů použití, kde uvidíte naplánována údržba po jste už dokončili údržby-opětovném nasazení virtuálního počítače:
1.  Jsme zrušena wave údržby a restartuje s jinou datovou část. Může to být, že zjistili jsme chybný datové části a potřebujeme jen nasadit další datové části.
2.  Virtuální počítač byl *služby zacelené* do jiného uzlu z důvodu selhání hardwaru
3.  Rozhodli jste se zastavit (zrušit přidělení) a restartujte virtuální počítač
4.  Máte **vypnutí automatického** zapnuté pro virtuální počítač.


**Otázka: údržby Moje sady dostupnosti trvá příliš dlouho a vidím "přeskočen" stav na některém z mé dostupnosti nastavit instancí. Proč?** 

**Odpověď:** Pokud jste klikli na aktualizace více instancí ve skupině dostupnosti nastavit krátký po sobě, Azure bude ve frontě pro tyto požadavky a spustí se aktualizovat jenom virtuální počítače v jedné aktualizace domény (UD) v čase. Ale vzhledem k tomu může být pozastavení mezi doménami aktualizace, aktualizace může vypadat trvá déle. Když aktualizace fronty trvá déle než 60 minut, zobrazí se některé instance **přeskočen** stavu i v případě, že bylo úspěšně aktualizováno. Abyste se vyhnuli tento nesprávný stav, aktualizaci, kterou vaše dostupnosti nastaví kliknutím pouze na instance v rámci jednoho dostupnosti nastavit a počkat na aktualizaci na tento virtuální počítač pro dokončení před kliknutím na další virtuální počítač v doméně jiné aktualizace.


## <a name="next-steps"></a>Další kroky

Zjistěte, jak můžete zaregistrovat pro události údržby z v rámci virtuálního počítače pomocí [naplánované události](scheduled-events.md).
