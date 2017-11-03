---
title: "Počáteční událost velikosti fondu Azure Batch | Microsoft Docs"
description: "Referenční dokumentace pro událost počáteční změny velikosti fondu Batch."
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
ms.openlocfilehash: 826cd984d26b923ba38562e05a2e75c399be9121
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="pool-resize-start-event"></a>Událost spuštění změny velikosti fondu

 Tato událost je vygenerované při bylo zahájeno změny velikosti fondu. Vzhledem k tomu, že změny velikosti fondu je asynchronní událostí, můžete očekávat, že událost complete změny velikosti fondu pro vypuštění po dokončení operace změny velikosti.

 Následující příklad ukazuje text počáteční událost změny velikosti fondu pro fondu změnu velikosti od 0 do 2 uzlů se ruční změny velikosti.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Element|Typ|Poznámky|
|-------------|----------|-----------|
|poolId|Řetězec|Id fondu.|
|nodeDeallocationOption|Řetězec|Určuje, kdy může odebrat uzly ve fondu, pokud zmenšování velikosti fondu.<br /><br /> Možné hodnoty:<br /><br /> **requeue** – ukončí se všechny spuštěné úlohy a znovu je. Úkoly se znovu spustí, když bude povolena úloha. Odeberte uzly, jakmile úkoly ukončí.<br /><br /> **Ukončit** – ukončit spuštěné úkoly. Úkoly se znovu nespustí. Odeberte uzly, jakmile úkoly ukončí.<br /><br /> **taskcompletion** – povolit aktuálně spuštěných úloh k dokončení. Plánovat žádné nové úkoly při čekání. Odeberte uzly po dokončení všech úloh.<br /><br /> **Retaineddata** -povolit aktuálně spuštěných úkolů dokončily. pak počkejte, než všech úkolů období uchovávání dat vyprší. Plánovat žádné nové úkoly při čekání. Odeberte uzly, pokud vypršela platnost období uchovávání všech úloh.<br /><br /> Výchozí hodnota je requeue.<br /><br /> Pokud je zvýšení velikosti fondu, pak je hodnota nastavená **neplatný**.|
|currentDedicated|Int32|Počet aktuálně přiřazená k fondu výpočetních uzlů.|
|targetDedicated|Int32|Počet výpočetních uzlů, které jsou požadovány pro fond.|
|enableAutoScale|BOOL|Určuje, zda velikost fondu automaticky přizpůsobí v čase.|
|isAutoPool|BOOL|Speficies jestli fondu se vytvořil prostřednictvím mechanismu AutoPool úlohy.|
