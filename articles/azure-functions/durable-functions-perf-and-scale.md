---
title: "Výkonu a možností škálování trvanlivý funkcí – Azure"
description: "Úvod do rozšíření trvanlivý funkce pro Azure Functions."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 10ce74097388a0283797e4692126c5039e8d4dd0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Výkonu a možností škálování trvanlivý funkcí (Azure Functions)

K optimalizaci výkonu a škálovatelnosti, je důležité si uvědomit, jedinečné škálování charakteristika [trvanlivý funkce](durable-functions-overview.md).

Pochopit chování škálování, je nutné pochopit podrobnosti o výchozí zprostředkovatel úložiště Azure používá trvanlivý funkce.

## <a name="history-table"></a>Tabulka historie

Tabulka historie je tabulka Azure Storage, která obsahuje historie událostí pro všechny instance orchestration. Jak spustit instance, přidávání řádků do této tabulky. Klíč oddílu této tabulky je odvozený od instance ID orchestration. Tyto hodnoty jsou náhodně ve většině případů, které zajistí optimální distribuční interní oddílů ve službě Azure Storage.

## <a name="internal-queue-triggers"></a>Aktivační události vnitřní fronty

Orchestrator funkce a funkce aktivity se spouštějí ve vnitřní fronty v aplikaci funkce výchozí účet úložiště. Existují dva typy front trvanlivý funkcí: **fronty řízení** a **fronty pracovních položek**.

### <a name="the-work-item-queue"></a>Fronta pracovních položek

Je jedna fronta pracovních položek za úloh centra trvanlivý funkce. Toto je základní frontu a žádné jiné se chová podobně jako `queueTrigger` fronty v Azure Functions. Tato fronta se používá k aktivaci bezstavové *aktivita funkce*. Pokud aplikace trvanlivý funkce horizontálně navýší kapacitu na víc virtuálních počítačů, všechny tyto virtuální počítače se pokouší získat pracovní z fronty pracovní položku.

### <a name="control-queues"></a>Ovládací prvek fronty

*Fronty řízení* je složitější než jednodušší fronty pracovní položku. Slouží k aktivaci funkce stavová orchestrator. Protože instance funkce orchestrator stavová jednotlivých prvků, není možné použít model konkurence mezi spotřebiteli distribuovat zatížení napříč virtuálními počítači. Místo toho orchestrator zprávy jsou vyrovnávání zatížení napříč více front ovládacího prvku. Další informace o to v následujících částech.

Řízení front obsahovat celou řadu typů zpráv orchestration životního cyklu. Mezi příklady patří [zprávy ovládacího prvku orchestrator](durable-functions-instance-management.md), aktivita funkce *odpovědi* zprávy a časovač zprávy.

## <a name="orchestrator-scale-out"></a>Orchestrator Škálováním na více systémů

Aktivita funkce jsou bezstavové a automaticky škálovat přidáním virtuálních počítačů. Funkce produktu Orchestrator na druhé straně jsou *oddílů* napříč jednu či více front ovládacího prvku. Počet front ovládací prvek je pevná a nelze změnit po spuštění vytváření zatížení.

Při zmenšování měřítka na více instancí hostitele – funkce (obvykle na různé virtuální počítače), každá instance získá zámku na jednom front ovládacího prvku. Tato zámku zajišťuje, že orchestration instance pouze běží na jeden virtuální počítač najednou. To znamená, pokud rozbočovač úkolů je nakonfigurované tři řízení front, orchestration instance může být vyrovnávání zatížení napříč až tři virtuální počítače. Další virtuální počítače mohou být přidány do zvýšit kapacitu pro provedení funkce aktivity.  Ale další prostředky nebudou lze použít ke spuštění funkce produktu orchestrator.

Následující diagram znázorňuje, jak Azure Functions hostitele komunikuje s entity úložiště v prostředí s škálovaný.

![Diagram škálování](media/durable-functions-perf-and-scale/scale-diagram.png)

Jak je vidět všechny virtuální počítače můžete pokouší o zprávy ve frontě pracovních položek. Ale jenom tři virtuální počítače můžete získat z řízení front zpráv a každý virtuální počítač zamkne fronty jeden ovládací prvek.

Instance Orchestration jsou rozmístěny v řízení fronty instancí spuštěním funkce interní hodnota hash pro ID orchestration instance. ID instance jsou automaticky generovaný a náhodných ve výchozím nastavení, která zajišťuje, že instance jsou rovnoměrně rozdělen mezi všechny dostupné řízení fronty. Je aktuální výchozí počet oddílů fronty podporované řízení **4**.

> [!NOTE]
> Není aktuálně možné nakonfigurovat počet oddílů v Azure Functions. [Práce na podporu této možnosti konfigurace je sledována](https://github.com/Azure/azure-functions-durable-extension/issues/73).

Obecně platí funkce orchestrator by měla být jednoduché a nemělo by být třeba velké množství výpočetní výkon. Z tohoto důvodu není potřebné k vytvoření velkého počtu oddílů fronty řízení získat skvělé propustnost. Místo toho většina těžká práce se provádí v bezstavové aktivita funkce, které lze škálovat nekonečnou.

## <a name="auto-scale"></a>Automatické škálování

S spuštěn v plánu spotřeby všechny funkce Azure, podporovat trvanlivý funkce automatického škálování prostřednictvím [škálování Azure Functions-controller](https://docs.microsoft.com/azure/azure-functions/functions-scale#runtime-scaling). Škálování řadiče monitoruje délka fronty pracovní položku a každý z fronty řízení přidáním nebo odebráním prostředky virtuálních počítačů odpovídajícím způsobem. Řízení délky fronty jsou v průběhu času zvětšuje, řadičem škálování bude přidávání instancí, dokud nebude dosaženo počet oddílů fronty ovládacího prvku. Pokud v průběhu času zvětšuje délky fronty pracovní položku, jsou řadičem škálování bude pokračovat, přidávání prostředky virtuálních počítačů, dokud ho může odpovídat zatížení, bez ohledu na počet oddílů řízení fronty.

## <a name="thread-usage"></a>Využití přístup z více vláken

Funkce Orchestrator jsou spouštěny na jedno vlákno. To je potřeba zkontrolujte, zda je provádění funkce orchestrator deterministický. Myslete na to je důležité nikdy zachovat orchestrator funkce zbytečně zaneprázdněn prováděním úlohy, jako je vstupu a výstupu (což je zakázán pro celou řadu důvodů) blokování nebo otáčí operací pro přístup z více vláken. Veškerou práci, které může být vstupně-výstupních operací blokování, nebo do funkce aktivity přesunout více vláken.

Aktivita funkce mají stejné chování jako běžné funkce aktivované fronty. To znamená, že můžou bezpečně provést vstupně-výstupních operací, provedení operace náročné na prostředky procesoru a používat více vláken. Vzhledem k aktivační události aktivity jsou bezstavové, že můžete volně horizontální navýšení kapacity bez vazby počet virtuálních počítačů.

## <a name="next-steps"></a>Další kroky

> [!div class="nextstepaction"]
> [Nainstalujte rozšíření trvanlivý funkce a ukázky](durable-functions-install.md)
