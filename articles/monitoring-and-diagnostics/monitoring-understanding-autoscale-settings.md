---
title: "Principy nastavení automatického škálování | Microsoft Docs"
description: "Podrobné rozpis nastavení automatického škálování a jak pracují."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ce2930aa-fc41-4b81-b0cb-e7ea922467e1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: ancav
ms.openlocfilehash: cff2be1818417a19f36da08d8c2eaa227bb945ec
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/20/2017
---
# <a name="understand-autoscale-settings"></a>Pochopit nastavení automatického škálování
Nastavení automatického škálování umožňují Ujistěte se, že máte správného množství prostředků pro zpracování kolísání zátěže vaší aplikace spuštěna. Můžete nakonfigurovat nastavení automatického škálování, aby se spouštěly podle metriky, které znamenat výkon nebo zatížení nebo aktivovat v naplánované datum a čas. V tomto článku se podíváme podrobné na anatomy nastavení automatického škálování. Článek začne pochopení schématu a vlastnosti nastavení a potom provede jiný profil typy, které lze konfigurovat a nakonec popisuje, jak škálování vyhodnotí který profil provést v daném okamžiku.

## <a name="autoscale-setting-schema"></a>Schéma nastavení automatického škálování
Pro ilustraci schéma nastavení automatického škálování, je použít následující nastavení automatického škálování. Je důležité si uvědomit, že toto nastavení automatického škálování nemá:
- Jeden profil 
- Má dvě pravidla metriky v tomto profilu; jeden pro Škálováním na více systémů a jeden pro škálování v.
- Pravidlo Škálováním na více systémů se aktivuje, když procento procesoru metrika průměrná škálovací sadu virtuálních počítačů je větší než 85 % na posledních 10 minut.
- Pravidlo škálování v aktivováno, když průměr škálovací sadu virtuálních počítačů je menší než 60 % pro uplynulé minuty.

> [!NOTE]
> Můžete mít víc profilů, přejít na nastavení [profily](#autoscale-profiles) části Další informace.
> Profil může mít několik horizontální pravidel a pravidel škálování v definovány, přejít na [vyhodnocení části](#autoscale-evaluation) zobrazíte, jak se vyhodnocují

```JSON
{
  "id": "/subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/autoscalesettings/setting1",
  "name": "setting1",
  "type": "Microsoft.Insights/autoscaleSettings",
  "location": "East US",
  "properties": {
    "enabled": true,
    "targetResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
    "profiles": [
      {
        "name": "mainProfile",
        "capacity": {
          "minimum": "1",
          "maximum": "4",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 85
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
    {
            "metricTrigger": {
              "metricName": "Percentage CPU",
              "metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.Compute/virtualMachineScaleSets/vmss1",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT10M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 60
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ]
  }
}
```

| Sekce | Název elementu | Popis |
| --- | --- | --- |
| Nastavení | ID | ID nastavení automatického škálování prostředku. Nastavení automatického škálování je prostředek Azure Resource Manager. |
| Nastavení | jméno | Název nastavení automatického škálování. |
| Nastavení | location | Umístění, nastavení automatického škálování. Toto umístění se může lišit od umístění prostředku se škálovat. |
| properties | targetResourceUri | ID prostředku se škálovat prostředek. Může mít pouze jeden nastavení automatického škálování na prostředku. |
| properties | Profily | Nastavení automatického škálování se skládá z nejméně jeden profil. Pokaždé, když modul škálování běží, provede jeden profil. |
| Profil | jméno | Název profilu, můžete použít libovolný název, který pomáhá identifikovat profil. |
| Profil | Capacity.maximum | Maximální kapacita povoleny. Zajišťuje, že škálování při provádění tohoto profilu, není škálovat prostředek výše toto číslo. |
| Profil | Capacity.minimum | Minimální kapacita povoleny. Zajišťuje, že škálování při provádění tohoto profilu, není škálování prostředku pod tuto hodnotu. |
| Profil | Capacity.default | Pokud dojde k problému čtení metrika prostředků (v tomto případě procesoru "vmss1") a aktuální kapacita je nižší než výchozí kapacitu, potom k zajištění dostupnosti prostředku, automatické škálování měřítka se na více systémů na výchozí hodnoty. Pokud je aktuální kapacita již vyšší než výchozí kapacita, automatické škálování nebude škálování v. |
| Profil | pravidla | Škálování automaticky přizpůsobí mezi maximální a minimální kapacitou, pomocí pravidel v profilu. Můžete mít více pravidel v profilu. Základní scénáře je mít dvě pravidla, jeden k určení, kdy Škálováním na více systémů a druhý k určení, kdy se mají škálování v. |
| Pravidlo | metricTrigger | Definuje metriky podmínku pravidla. |
| metricTrigger | metricName | Název metriky. |
| metricTrigger |  metricResourceUri | ID prostředku prostředek, který vysílá metriku. Ve většině případů je stejný jako prostředek se škálovat. V některých případech mohou být různé, například je možné škálovat škálovací sadu virtuálních počítačů, na základě počtu zpráv ve frontě úložiště. |
| metricTrigger | Časovými úseky | Doba trvání metriky vzorkování. Například časovými úseky = "PT1M" znamená, že by měl být metriky agregují každých 1 minuta agregace metody popsané v "statistiky." |
| metricTrigger | statistiky | Metoda agregace v období časovými úseky. Například statistiky = "Průměrná" a časovými úseky = "PT1M" znamená, že by měl být metriky agregovat použitím průměru každou 1 minutu. Tato vlastnost určuje, jak je metrika vzorků. |
| metricTrigger | Hodnota timeWindow | Množství času do minulosti metrik. Například hodnota timeWindow = "PT10M" znamená, že pokaždé, když spustí škálování, dotazuje metriky pro za posledních 10 minut. Časový interval umožňuje vaše metriky budou normalizovány a zabraňuje reagovat na přechodný špičky. |
| metricTrigger | Agregace času | Metoda agregace použitá k agregaci jen Vzorkovaná metriky. Například agregace času = "Průměrná" by měl agregovat jen Vzorkovaná metriky provedením průměr. V případě, že výše vzorky deseti 1 minutu a průměrné je. |
| Pravidlo | scaleAction | Akce, který se má provést, když se aktivuje metricTrigger pravidla. |
| scaleAction | směr | "Zvýšit" na Škálováním na více systémů, "Snížit" pro škálování v|
| scaleAction | hodnota | Kolik můžete zvýšit nebo snížit kapacitu prostředku |
| scaleAction | cooldown | Množství času čekání po operaci škálování před škálování znovu. Například pokud cooldown = "PT10M", pak po dokončení operace škálování škálování se nepokusí škálování znovu pro jiné 10 minut. Cooldown je umožnit metriky stabilizovat po přidání nebo odebrání instance. |

## <a name="autoscale-profiles"></a>Profilů automatického škálování

Existují tři typy profilů automatického škálování:

1. **Regulární profilu:** nejběžnější profilu. Pokud nepotřebujete škálování prostředku různě v závislosti na den v týdnu, nebo v určitý den, pak stačí nastavit profil regulární v vašeho nastavení automatického škálování. Tento profil můžete pak nakonfigurovat metriky pravidla, která určují, kdy Škálováním na více systémů a kdy škálování v. Měli byste mít jenom jeden profil regulární definované.

    Příklad profilu použít dříve v tomto článku je příklad regulární profilu. Proveďte, není je také možné nastavit profil škálovat na počet statická instance prostředku.

2. **Pevné datum profilu:** s profilem regulární definované, Řekněme, že máte důležité události objevuje na 26 prosinec 2017 (PST) a má minimální a maximální kapacitou prostředku lišit v daný den, ale stále škálovat na stejné metriky . V takovém případě by měl přidat profil pevné datum na seznam profilů vaše nastavení. Profil je nakonfigurována pro spuštění pouze dne události. Pro druhý den se spustí regulární profilu.

    ``` JSON
    "profiles": [{
    "name": " regularProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    },
    {
    ...
    }]
    },
    {
    "name": "eventProfile",
    "capacity": {
    ...
    },
    "rules": [{
    ...
    }, {
    ...
    }],
    "fixedDate": {
        "timeZone": "Pacific Standard Time",
               "start": "2017-12-26T00:00:00",
               "end": "2017-12-26T23:59:00"
    }}
    ]
    ```
    
3. **Profil opakování:** tohoto typu profilu umožňuje zajistit, aby tento profil je vždy používaly v určitý den v týdnu. Opakování profily pouze mají čas zahájení, v důsledku spustit až další profil opakování nebo pevného data profilu je nastaven na spuštění. Automatické škálování, nastavení se jenom jeden profil opakování, zpracuje tento profil, i v případě, že je profil regulární definovaný ve stejné nastavení. Následující dva příklady ilustrují použití tohoto profilu:

    **Příklad 1 - vs den v týdnu. Víkendů** Řekněme, že o víkendech chcete, aby vaše maximální kapacita být 4, ale ve všední dny, protože očekáváte, že další zatížení, má vaše maximální kapacita být 10. V takovém případě vaše nastavení by obsahovat dva profily opakování, jednu pro spuštění na víkendy a druhá ve všední dny.
    Toto nastavení bude vypadat takto:

    ``` JSON
    "profiles": [
    {
    "name": "weekdayProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }}
    },
    {
    "name": "weekendProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Saturday"
            ],
            "hours": [
                0
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```

    Pohledem na předchozí nastavení, můžete si všimnout, že každý profil opakování obsahuje plán, tento plán Určuje profil při spuštění, provádění. Profil zastaví prováděny, kdy je čas spuštění jiný profil.

    V předchozí nastavení, například "weekdayProfile" nastavená na spuštění v pondělí v 12 hodin, tzn. Tento profil spuštění, provádění na 12.am v pondělí. Ho pokračuje v provádění až sobotu 12a.m. Pokud je naplánováno spuštění provádění "weekendProfile".

    **Příklad 2 - pracovní dobu** Podívejme další příklad, možná budete chtít mít metriky prahová hodnota = "x" během pracovní doby, 9: 00 do 17: 00 a pak z 17: 00 do 9: 00 Další den, budete chtít metriky prahovou hodnotu na "y".
    Toto nastavení bude vypadat takto:
    
    ``` JSON
    "profiles": [
    {
    "name": "businessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }],
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                9
            ],
            "minutes": [
                0
            ]
        }
    }
    },
    {
    "name": "nonBusinessHoursProfile",
    "capacity": {
        ...
    },
    "rules": [{
        ...
    }]
    "recurrence": {
        "frequency": "Week",
        "schedule": {
            "timeZone": "Pacific Standard Time",
            "days": [
                "Monday", “Tuesday”, “Wednesday”, “Thursday”, “Friday”
            ],
            "hours": [
                17
            ],
            "minutes": [
                0
            ]
        }
    }
    }]
    ```
    
    Pohledem na předchozí nastavení "businessHoursProfile" zahájí provádění v pondělí v 9: 00 a uchovávat provádění až 17: 00 Když je totiž "nonBusinessHoursProfile" spustí provádění. "nonBusinessHoursProfile" se spustí až 9: 00 Úterý a pak převezme "businessHoursProfile". To se opakuje, dokud pátek 17: 00, v tomto bodě "nonBusinessHoursProfile" se spustí až pondělí 9: 00 vzhledem k tomu, že "businessHoursProfile" nespustí provádění do pondělí 9: 00
    
> [!Note]
> Škálování UX na portálu Azure vynucuje koncový čas pro opakování profily a zahájí provádění nastavení automatického škálování výchozí profil mezi opakování profily.
    
## <a name="autoscale-evaluation"></a>Vyhodnocení škálování
Zadané této škálování nastavení může mít několik profilů automatického škálování a každý profil může mít několik metriky pravidla, která je důležité pochopit, jak se vyhodnocují na nastavení automatického škálování. Při každém spuštění úloh škálování začne výběrem profil, který se vztahuje, po výběru profilu škálování vyhodnotí minimální, maximální hodnoty a všechny metriky pravidel v profilu a rozhodne, pokud je akce škálování nezbytné.

### <a name="which-profile-will-autoscale-pick"></a>Který profil vyzvedne bude škálování?
- Škálování nejprve hledá všechny pevné datum profil, který je nakonfigurován na spuštění nyní, pokud existuje, automatické škálování spustí ho. Pokud existuje víc profilů pevné datum, které se má spustit, automatické škálování vybere první z nich.
- Pokud neexistují žádné profily pevné datum, automatické škálování zjistí opakování profily, pokud najde, pak se provede ji.
- Pokud neexistují žádné pevné nebo opakování profily a pak škálování spuštění regulární profilu.

### <a name="how-does-autoscale-evaluate-multiple-rules"></a>Jak škálování vyhodnocení více pravidel?

Jakmile škálování Určuje, který profil by měl provést, začne vyhodnocením všechny pravidlo Škálováním na více systémů v profilu (pravidla s směr = "Zvýšit").
- Pokud jeden nebo více pravidel Škálováním na více systémů se aktivují, vypočítá škálování nové kapacity dáno scaleAction každého z těchto pravidel. Potom ji měřítka se na více systémů na maximální těchto kapacity k zajištění dostupnosti služeb.
- Například: Pokud je škálovací sadu virtuálních počítačů s aktuální kapacitou 10 a existují dvě pravidla Škálováním na více systémů; ten, který zvyšuje kapacitu o 10 % a ten, který zvyšuje kapacitu o 3. První pravidlo by mělo za následek nové kapacity 11 a druhé pravidlo by mělo za následek kapacitou 13. Zkontrolujte dostupnost služeb škálování rozhodne, že je zvolen akci, která je výsledkem maximální kapacitu, takže druhé pravidlo.

Pokud žádná pravidla Škálováním na více systémů se aktivují, automatické škálování vyhodnotí všechny škálovací v pravidla (pravidel se směr = "Snížit"). Škálování akci škálování v přijímá pouze, pokud všechna pravidla v škálování se aktivují.
- Škálování vypočítá nové kapacity dáno scaleAction každého z těchto pravidel. Potom vybere akci škálování, jejímž výsledkem maximálně těchto kapacity k zajištění dostupnosti služeb.
- Například: Pokud je škálovací sadu virtuálních počítačů s aktuální kapacitou 10 a existují dvě škálování v pravidla; ten, který snižuje kapacitu 50 % a ten, který snižuje kapacitu 3. První pravidlo by mělo za následek nové kapacity 5 a druhé pravidlo by mělo za následek kapacitou 7. Zkontrolujte dostupnost služeb škálování rozhodne, že je zvolen akci, která je výsledkem maximální kapacitu, takže druhé pravidlo.

## <a name="next-steps"></a>Další kroky
Další informace o škálování najdete v následujících zdrojích informací:

* [Přehled automatického škálování](monitoring-overview-autoscale.md)
* [Azure monitorování běžné metriky automatického škálování](insights-autoscale-common-metrics.md)
* [Osvědčené postupy pro monitorování Azure škálování](insights-autoscale-best-practices.md)
* [Akce škálování používat k odesílání e-mailu a webhooku oznámení výstrah](insights-autoscale-to-webhook-email.md)
* [Škálování REST API](https://msdn.microsoft.com/library/dn931953.aspx)
