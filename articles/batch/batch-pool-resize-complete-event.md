---
title: "Událost complete resize fondu Azure Batch | Microsoft Docs"
description: "Referenční dokumentace pro fondu Batch změnit velikost událost complete."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 7072293d98526812cb42ce9c2f8e33bfcafaa149
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="pool-resize-complete-event"></a>Dokončení událost změny velikosti fondu

 Tato událost je vygenerované při změny velikosti fondu má byla dokončena nebo se nezdařilo.

 Následující příklad ukazuje text událost complete změny velikosti fondu pro skupinu, která tento nárůst velikosti a byla úspěšně dokončena.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "The operation succeeded"
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|id|Řetězec|Id fondu.|
|nodeDeallocationOption|Řetězec|Určuje, kdy může odebrat uzly ve fondu, pokud zmenšování velikosti fondu.<br /><br /> Možné hodnoty:<br /><br /> **requeue** – ukončí se všechny spuštěné úlohy a znovu je. Úkoly se znovu spustí, když bude povolena úloha. Odeberte uzly, jakmile úkoly ukončí.<br /><br /> **Ukončit** – ukončit spuštěné úkoly. Úkoly se znovu nespustí. Odeberte uzly, jakmile úkoly ukončí.<br /><br /> **taskcompletion** – povolit aktuálně spuštěných úloh k dokončení. Plánovat žádné nové úkoly při čekání. Odeberte uzly po dokončení všech úloh.<br /><br /> **Retaineddata** -povolit aktuálně spuštěných úkolů dokončily. pak počkejte, než všech úkolů období uchovávání dat vyprší. Plánovat žádné nové úkoly při čekání. Odeberte uzly, pokud vypršela platnost období uchovávání všech úloh.<br /><br /> Výchozí hodnota je requeue.<br /><br /> Pokud je zvýšení velikosti fondu, pak je hodnota nastavená **neplatný**.|
|currentDedicated|Int32|Počet aktuálně přiřazená k fondu výpočetních uzlů.|
|targetDedicated|Int32|Počet výpočetních uzlů, které jsou požadovány pro fond.|
|enableAutoScale|BOOL|Určuje, zda velikost fondu automaticky přizpůsobí v čase.|
|isAutoPool|BOOL|Určuje, zda fondu se vytvořil prostřednictvím mechanismu AutoPool úlohy.|
|startTime|Data a času|Změnit velikost fondu čas spuštění.|
|čas ukončení|Data a času|Čas změnit velikost fondu byla dokončena.|
|resultCode|Řetězec|Výsledek změna.|
|resultMessage|Řetězec|Chyba změny velikosti obsahuje podrobnosti výsledku.<br /><br /> Pokud se změna úspěšně dokončit ho stavy, které operace byla úspěšná.|