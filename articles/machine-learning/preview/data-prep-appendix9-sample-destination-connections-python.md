---
title: "Příklad cíle nebo výstupy možné přípravy dat Azure Machine Learning | Microsoft Docs"
description: "Tento dokument obsahuje sadu příklady cílů vlastních dat nebo výstupy přípravy dat Azure Machine Learning"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 45522e23b592bfa7be2be22ef4ac7ef4713b79bf
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="sample-of-destination-connections-python"></a>Ukázka cílových připojení (Python) 
Před čtením této příloha, přečtěte si [Python rozšiřitelnost přehled](data-prep-python-extensibility-overview.md).


## <a name="write-to-excel"></a>Zápis do aplikace Excel 


Zápis do aplikace Excel vyžaduje další knihovny. Přidání nové knihovny je popsána v přehledu rozšíření. `openpyxl`je knihovnu, která je nutné přidat.

Než napíšete do aplikace Excel, může být potřeba některé další změny. Některé typy dat, které se používají v rámci přípravy dat nejsou podporovány v některé cílové formáty. Například pokud "Chyba" objekty existují, se nebude serializovat správně do aplikace Excel. Před pokusem o zápis do aplikace Excel, proto musíte transformace "Nahradí chybové hodnoty", který odebere chyby žádné sloupce.

Pokud všechny předchozí pracovní dokončení se zapíše následující řádek tabulky datového jeden list v dokumentu aplikace Excel. Přidejte transformace zápisu toku dat (skript). Potom zadejte následující kód ve výrazu oddílu.


### <a name="on-windows"></a>V systému Windows 
```python
df.to_excel('c:\dev\data\Output\Customer.xlsx', sheet_name='Sheet1')
```

### <a name="on-macosos-x"></a>V systému macOS/OS X ###
```python
df.to_excel('c:/dev/data/Output/Customer.xlsx', sheet_name='Sheet1')
```
