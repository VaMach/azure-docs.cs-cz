---
title: "Azure Service Fabric rozhraní příkazového řádku sfctl úložiště | Microsoft Docs"
description: "Popisuje příkazy sfctl úložiště Service Fabric rozhraní příkazového řádku."
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
ms.date: 09/22/2017
ms.author: ryanwi
ms.openlocfilehash: 2af6dff4ffcdf295731f2d61b5f9e35af40615e5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="sfctl-store"></a>sfctl úložiště
Proveďte základní souborové úrovni operace na clusteru úložiště bitových kopií.

## <a name="commands"></a>Příkazy

|Příkaz|Popis|
| --- | --- |
|    Odstranit| Odstraní existující obsahu úložiště bitové kopie.|
|    kořenové informace| Získá informace o obsahu v kořenovém adresáři úložiště bitových kopií.|
|    Stat  | Získá informace o obsahu pro image store.|


## <a name="sfctl-store-delete"></a>Odstranění úložiště sfctl
Odstraní existující obsahu úložiště bitové kopie.

Odstraní existující obsah image úložiště byl nalezen v rámci dané image uložit relativní cestu. Tento příkaz lze použít k odstranění balíčků nahrané aplikace po jejich zřízení.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Cesta obsahu [vyžaduje]| Relativní cesta k souboru nebo složce v úložišti bitové kopie z svůj kořen.|
| časový limit – -t          | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění               | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h             | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o           | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu               | Řetězec dotazu JMESPath. Další informace a příklady naleznete v tématu http://jmespath.org/.|
| -verbose             | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="sfctl-store-stat"></a>stat sfctl úložiště
Získá informace o obsahu pro image store.

Vrátí informace o obsahu úložiště bitové kopie v zadané contentPath kořeni úložiště bitových kopií.

### <a name="arguments"></a>Argumenty

|Argument|Popis|
| --- | --- |
| – Cesta obsahu [vyžaduje]| Relativní cesta k souboru nebo složce v úložišti bitové kopie z svůj kořen.|
| časový limit – -t          | Server časový limit v sekundách.  Výchozí: 60.|

### <a name="global-arguments"></a>Globální argumenty

|Argument|Popis|
| --- | --- |
| – ladění               | Zvýšit protokolování podrobností zobrazit, že všechny protokoly ladění.|
| – Nápověda -h             | Zobrazte tuto zprávu nápovědy a ukončení.|
| --výstup -o           | Výstupní formát.  Povolené hodnoty: formát json, jsonc, tabulce, tsv.  Výchozí: json.|
| --dotazu               | Řetězec dotazu JMESPath. V tématu http://jmespath.org/ Další informace a příklady.|
| -verbose             | Zvýšit protokolování podrobností. Použití – ladění pro úplné ladění protokoly.|

## <a name="next-steps"></a>Další kroky
- [Instalační program](service-fabric-cli.md) Service Fabric rozhraní příkazového řádku.
- Další informace o použití pomocí Service Fabric rozhraní příkazového řádku [ukázkové skripty](/azure/service-fabric/scripts/sfctl-upgrade-application).