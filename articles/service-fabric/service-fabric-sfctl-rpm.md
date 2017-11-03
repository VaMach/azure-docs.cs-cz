---
title: "Azure Service Fabric rozhraní příkazového řádku sfctl ot | Microsoft Docs"
description: "Popisuje příkazy Service Fabric rozhraní příkazového řádku sfctl ot. / min."
services: service-fabric
documentationcenter: na
author: rwike77
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: cli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 09/26/2017
ms.author: ryanwi
ms.openlocfilehash: f032af4714ad458fa6ad6fb0741f689d44f4098b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-rpm"></a>ot. / min sfctl
Dotaz a odesílat příkazy na service manager opravit.

## <a name="commands"></a>Příkazy
|Příkaz|Popis|
| --- | --- |
|    platnost schválení| Vynutí schválení úloha dané opravy.|
|    Odstranit       | Odstraní úloha dokončena opravy.|
|    seznam         | Získá seznam úloh oprava odpovídající dané filtry.|

## <a name="sfctl-rpm-delete"></a>Odstranit sfctl ot. / min
Odstraní úloha dokončena opravy.

Toto rozhraní API podporuje platformy Service Fabric; ji není určen pro použití přímo z vašeho kódu. 

### <a name="arguments"></a>Argumenty
|Argument|Popis|
| --- | --- |
|    – id úlohy [vyžaduje]| ID úloha dokončena opravy k odstranění.|
|    – verze           | Aktuální číslo verze úloha opravy. Pokud nulová, pak požadavek bude úspěšné, pouze pokud tato hodnota odpovídá skutečným aktuální verzi úloha opravy. Pokud nula, bude provedena žádná kontrola verze.|

### <a name="global-arguments"></a>Globální argumenty
|Argument|Popis|
| --- | --- |
|    – ladění             | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
|    – Nápověda -h           | Zobrazte tuto zprávu nápovědy a ukončení.|
|    --výstup -o         | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.
|    --dotazu             | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
|    -verbose           | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|


## <a name="sfctl-rpm-list"></a>sfctl rpm seznamu
Získá seznam úloh oprava odpovídající dané filtry.

Toto rozhraní API podporuje platformy Service Fabric; ji není určen pro použití přímo z vašeho kódu. 

### <a name="arguments"></a>Argumenty
|Argument|Popis|
| --- | --- |
|    --prováděcího modulu filtru| Název opravy prováděcího modulu, jehož deklarovaný úlohy by měl být součástí seznamu.|
|    --Filtr stavu   | Seznam výsledků by měl být součástí bitové operace OR následující hodnoty určující, který úkol stavy. -1 - vytvořený - 2 - deklarovaný - 4 – Příprava - 8 - schválené - 16 - provádění - 32 - obnovení - 64 - dokončit.|
|    --Filtr id úkolu | Oprava předpona ID úloh lze porovnat.|

### <a name="global-arguments"></a>Globální argumenty
|Argument|Popis|
| --- | --- |
|    – ladění          | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
|    – Nápověda -h        | Zobrazte tuto zprávu nápovědy a ukončení.|
|    --výstup -o      | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí| JSON.|
|    --dotazu          | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
|    -verbose        | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="next-steps"></a>Další kroky
- [Nastavit](service-fabric-cli.md) rozhraní příkazového řádku služby prostředků infrastruktury.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).