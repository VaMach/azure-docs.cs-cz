---
title: "Příklad transformace dat toku transformace možné pomocí Azure Machine Learning Data přípravy | Microsoft Docs"
description: "Tento dokument obsahuje sadu příklady transformace datového toku transformací možné pomocí Azure Machine Learning Příprava dat"
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/11/2017
ms.openlocfilehash: 5491548885709c1c1048e45d699ef385a7c49a74
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/31/2017
---
# <a name="sample-of-custom-data-flow-transforms-python"></a>Ukázka vlastních dat toku transformací (Python) 
Je název transformace v nabídce **transformace toku dat (skript)**. Před čtením této příloha, přečtěte si [Python rozšiřitelnost přehled](data-prep-python-extensibility-overview.md).

## <a name="transform-frame"></a>Transformace rámce
### <a name="create-a-new-column-dynamically"></a>Vytvoří nový sloupec, dynamicky 
Dynamicky vytvoří sloupec (**city2**) a sloučí více různých verzích Brno na jednu z existující sloupec města.
```python
    df.loc[(df['city'] == 'San Francisco') | (df['city'] == 'SF') | (df['city'] == 'S.F.') | (df['city'] == 'SAN FRANCISCO'), 'city2'] = 'San Francisco'
```

### <a name="add-new-aggregates"></a>Přidat nové agregace
Vytvoří nový snímek se první a poslední agregace, počítaného sloupce skóre. Tyto jsou seskupené podle **risk_category** sloupce.
```python
    df = df.groupby(['risk_category'])['Score'].agg(['first','last'])
```
### <a name="winsorize-a-column"></a>Winsorize sloupec 
Reformulates data podle vzorce pro snížení zatížení odlehlé hodnoty ve sloupci.
```python
    import scipy.stats as stats
    df['Last Order'] = stats.mstats.winsorize(df['Last Order'].values, limits=0.4)
```

## <a name="transform-data-flow"></a>Transformace toku dat
### <a name="fill-down"></a>Vyplnit dolů 
Výplně dolů vyžaduje dva transformací. Předpokládá data, která vypadá takto:


|Stav         |Město       |
|--------------|-----------|
|Washington    |Redmond    |
|              |Bellevue   |
|              |Issaquah   |
|              |Seattle    |
|Kalifornie    |Los Angeles|
|              |Síť San Diego  |
|              |Síť San Jose   |
|Texas         |Dallas     |
|              |Síť SAN Antonio|
|              |Houston    |

Nejprve vytvořte transformaci přidat sloupec (skript), který obsahuje následující kód:
```python
    row['State'] if len(row['State']) > 0 else None
```
Nyní vytvoří transformace transformace toku dat (skript), který obsahuje následující kód:
```python
    df = df.fillna( method='pad')
```

Data nyní vypadá takto:

|Stav         |Nový stav         |Město       |
|--------------|--------------|-----------|
|Washington    |Washington    |Redmond    |
|              |Washington    |Bellevue   |
|              |Washington    |Issaquah   |
|              |Washington    |Seattle    |
|Kalifornie    |Kalifornie    |Los Angeles|
|              |Kalifornie    |Síť San Diego  |
|              |Kalifornie    |Síť San Jose   |
|Texas         |Texas         |Dallas     |
|              |Texas         |Síť SAN Antonio|
|              |Texas         |Houston    |

