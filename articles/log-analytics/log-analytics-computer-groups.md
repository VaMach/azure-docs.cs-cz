---
title: "Skupiny počítačů v analýzy protokolů protokolu hledání | Microsoft Docs"
description: "Skupiny počítačů v analýzy protokolů umožňují oboru vyhledávání protokolu na konkrétní sadu počítačů.  Tento článek popisuje různé metody, které můžete použít k vytvoření skupiny počítačů a jejich použití v hledání protokolů."
services: log-analytics
documentationcenter: 
author: bwren
manager: jwhit
editor: 
ms.assetid: a28b9e8a-6761-4ead-aa61-c8451ca90125
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/11/2017
ms.author: bwren
ms.openlocfilehash: f27f038e0507270c0bfe200cb8c86622ebac5372
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/16/2017
---
# <a name="computer-groups-in-log-analytics-log-searches"></a>Skupiny počítačů v analýzy protokolů protokolu hledání

Skupiny počítačů v analýzy protokolů umožňují oboru [protokolu hledání](log-analytics-log-search-new.md) na konkrétní sadu počítačů.  Každé skupiny se naplní se počítači buď pomocí dotaz, který definujete nebo importem skupiny z různých zdrojů.  Pokud skupina je obsažena v protokolu vyhledávání, výsledky jsou omezeny na záznamy, které odpovídají počítače ve skupině.

## <a name="creating-a-computer-group"></a>Vytvoření skupiny počítačů
Můžete vytvořit skupinu počítačů v analýzy protokolů pomocí libovolné metody v následující tabulce.  Podrobnosti o jednotlivých metod jsou uvedeny v následujících částech. 

| Metoda | Popis |
|:--- |:--- |
| Prohledávání protokolů |Vytvořte vyhledávání protokolu, který vrátí seznam počítačů. |
| Rozhraní API pro prohledávání protokolů |Pomocí rozhraní API pro vyhledávání protokolu prostřednictvím kódu programu vytvořit skupinu počítačů, na základě výsledků hledání protokolů. |
| Active Directory |Automaticky kontrolu členství ve skupině všechny počítače agenta, které jsou členy domény služby Active Directory a vytvořte skupinu ve analýzy protokolů pro každou skupinu zabezpečení. |
| Configuration Manager | Importovat kolekce nástroje System Center Configuration Manager a vytvořte skupinu ve analýzy protokolů pro každý. |
| Windows Server Update Services |Automatické prohledávání WSUS servery nebo klienty pro cílové skupiny a vytvořte skupinu ve analýzy protokolů pro každý. |

### <a name="log-search"></a>Prohledávání protokolů
Skupiny počítačů, které jsou vytvořené z hledání protokolů obsahovat všechny počítače vrácených dotazem, které definujete.  Tento dotaz běží pokaždé, když se používá skupina počítačů, tak, aby se projeví všechny změny, protože byla skupina vytvořena.  

Pro skupinu počítačů můžete použít jakýkoli dotaz, ale musí vracet odlišnou sadu počítačů pomocí `distinct Computer`.  Následuje typický příklad hledání, které můžete použít pro jako skupinu počítačů.

    Heartbeat | where Computer contains "srv" | distinct Computer

Následující tabulka popisuje vlastnosti, které definují skupinu počítačů.

| Vlastnost | Popis |
|:---|:---|
| Zobrazovaný název   | Název hledání pro zobrazení na portálu. |
| Kategorie       | Kategorie k uspořádání vyhledávání v portálu. |
| Dotaz          | Dotaz pro skupinu počítačů. |
| Alias – funkce | Jedinečný odkaz použít k identifikaci skupiny počítačů v dotazu. |

Použijte následující postup k vytvoření skupiny počítačů z protokolu hledání na portálu Azure.

2. Otevřete **hledání protokolů** a pak klikněte na **uložená hledání** v horní části obrazovky.
3. Klikněte na tlačítko **přidat** a zadejte hodnoty pro každou vlastnost pro skupinu počítačů.
4. Vyberte **uložit tento dotaz jako skupinu počítačů** a klikněte na tlačítko **OK**.


Použijte následující postup k vytvoření skupiny počítačů z protokolu hledání na portálu OMS.

1. Otevřete **hledání protokolů** a vytvořte hledání protokolů pro skupinu počítačů.  
2. Klikněte **Uložit** tlačítka v horní části obrazovky.
3. Vyberte **Ano** k **uložit tento dotaz jako skupinu počítačů**.
5. Zadejte hodnoty pro každou vlastnost pro skupinu počítačů. 


>[!NOTE]
> Pokud je stále pomocí pracovního prostoru [starší verze protokolu Analytics query language](log-analytics-log-search-upgrade.md) pak použijte stejný postup pro vytvoření skupiny počítačů, ale vy musíte použít syntaxi jazyka dotazů starší verze.


### <a name="log-search-api"></a>Hledání protokolů rozhraní API
Skupiny počítačů, které jsou vytvořené pomocí rozhraní API pro vyhledávání protokolu jsou stejné jako rozšířeného hledání protokolů hledání.  Podrobné informace o vytváření skupiny počítačů pomocí rozhraní API pro vyhledávání protokolu najdete v tématu [skupiny počítačů v protokolu analýzy protokolů hledání REST API](log-analytics-log-search-api.md#computer-groups).

### <a name="active-directory"></a>Active Directory
Když konfigurujete analýzy protokolů pro import členství ve skupině služby Active Directory, analyzuje členství ve skupině všechny počítače připojené k doméně s agentem OMS.  Skupina počítačů se vytvoří v analýzy protokolů pro každou skupinu zabezpečení ve službě Active Directory a každý počítač se přidá do odpovídající skupiny zabezpečení, které jsou členy skupin počítačů.  Členství v této se průběžně aktualizuje každé 4 hodiny.  

Konfigurace analýzy protokolů k importu skupin zabezpečení služby Active Directory z analýzy protokolů **upřesňující nastavení** na portálu Azure.  Vyberte **skupiny počítačů**, **služby Active Directory**a potom **členství ve skupině služby Active Directory Import z počítačů**.  Není nutná žádná další konfigurace.

![Skupiny počítačů ze služby Active Directory](media/log-analytics-computer-groups/configure-activedirectory.png)

Importu skupin v nabídce uvádí počet počítačů s zjistil členství ve skupině a počet skupin importovat.  Kliknutím na některou z těchto odkazů se vrátíte **ComputerGroup** záznamy s těmito informacemi.

### <a name="windows-server-update-service"></a>Windows Server Update Service
Když konfigurujete analýzy protokolů pro import členství ve skupině WSUS, analyzuje zaměřená na členství ve skupině všechny počítače s agentem OMS.  Pokud používáte klienta cílení na skupiny cílení na jakýkoli počítač, který je připojený k OMS a je součástí všech WSUS má jeho členství ve skupinách importovat k analýze protokolů. Pokud používáte serverové cílení OMS agenta by měly být nainstalovány na serveru WSUS, aby se informace o členství ve skupině k importu OMS.  Členství v této se průběžně aktualizuje každé 4 hodiny. 

Konfigurace analýzy protokolů k importu skupin WSUS z analýzy protokolů **upřesňující nastavení** na portálu Azure.  Vyberte **skupiny počítačů**, **WSUS**a potom **Import WSUS členství ve skupinách**.  Není nutná žádná další konfigurace.

![Skupiny počítačů ze služby WSUS](media/log-analytics-computer-groups/configure-wsus.png)

Importu skupin v nabídce uvádí počet počítačů s zjistil členství ve skupině a počet skupin importovat.  Kliknutím na některou z těchto odkazů se vrátíte **ComputerGroup** záznamy s těmito informacemi.

### <a name="system-center-configuration-manager"></a>System Center Configuration Manager
Při konfiguraci analýzy protokolů pro import členství kolekce nástroje Configuration Manager vytvoří skupiny počítačů pro každou kolekci.  Informace o členství v kolekci je načíst každé 3 hodiny zachovat aktuální skupiny počítačů. 

Před importem kolekcí nástroje Configuration Manager, musíte [připojení nástroje Configuration Manager k analýze protokolů](log-analytics-sccm.md).  Potom můžete nakonfigurovat importovat z analýzy protokolů **upřesňující nastavení** na portálu Azure.  Vyberte **skupiny počítačů**, **SCCM**a potom **členství v kolekcích Import Configuration Manager**.  Není nutná žádná další konfigurace.

![Skupiny počítačů z SCCM](media/log-analytics-computer-groups/configure-sccm.png)

Při kolekce byly importovány, v nabídce uvádí počet počítačů s zjistil členství ve skupině a počet skupin importovat.  Kliknutím na některou z těchto odkazů se vrátíte **ComputerGroup** záznamy s těmito informacemi.

## <a name="managing-computer-groups"></a>Správa skupin počítačů
Můžete zobrazit skupiny počítačů, které byly vytvořeny z vyhledávání protokolu nebo rozhraní API pro vyhledávání protokolu z analýzy protokolů **upřesňující nastavení** na portálu Azure.  Vyberte **skupiny počítačů** a potom **uložené skupiny**.  

Klikněte **x** v **odebrat** sloupec odstranit skupinu počítačů.  Klikněte **zobrazit členy** ikona pro skupinu pro spuštění vyhledávání protokolu skupiny, který vrátí její členy.  Nelze upravit skupinu počítačů, ale místo toho musíte odstranit a znovu ji vytvořte s upravené nastavení.

![Skupiny uložené počítačů](media/log-analytics-computer-groups/configure-saved.png)


## <a name="using-a-computer-group-in-a-log-search"></a>Použití skupiny počítačů do protokolu hledání
Skupinu počítačů můžete použít v dotazu tak, že považuje jeho alias jako funkce, obvykle s následující syntaxí:

  `Table | where Computer in (ComputerGroup)`

Například můžete použít následující vrátit UpdateSummary záznamy pro pouze počítače ve skupině počítač s názvem mycomputergroup.
 
  `UpdateSummary | where Computer in (mycomputergroup)`

>[!NOTE]
> Pokud je stále pomocí pracovního prostoru [starší verze protokolu Analytics query language](log-analytics-log-search-upgrade.md)>, pak můžete odkazovat na skupinu počítačů do protokolu hledání použijte následující syntaxi.  Určení **kategorie** > je volitelné a vyžaduje, pokud máte skupiny počítačů se stejným názvem v různých kategorií. 
>
>    `$ComputerGroups[Category: Name]`
>
>Skupiny počítačů se obvykle používají se **IN** klauzuli v hledání protokolů jako v následujícím příkladu:
>
>    `Type=UpdateSummary Computer IN $ComputerGroups[My Computer Group]`



## <a name="computer-group-records"></a>Záznamů skupiny počítače
V úložišti OMS pro každý členství ve skupině počítačů vytvořené pomocí služby Active Directory nebo služby WSUS se vytvoří záznam.  Tyto záznamy mají typ **ComputerGroup** a mít vlastnosti v následující tabulce.  Záznamy nejsou vytvořeny pro skupiny počítačů, které jsou založené na protokolu hledání.

| Vlastnost | Popis |
|:--- |:--- |
| Typ |*ComputerGroup* |
| SourceSystem |*SourceSystem* |
| Počítač |Název členského počítače. |
| Skupina |Název skupiny. |
| GroupFullName |Úplná cesta ke skupině, včetně zdroje a název zdroje. |
| GroupSource |Zdroj této skupině se shromážděných z. <br><br>Active Directory<br>SLUŽBY WSUS<br>WSUSClientTargeting |
| GroupSourceName |Název zdroje, který skupině nebyla shromážděna z.  U služby Active Directory je to název domény. |
| ManagementGroupName |Název skupiny pro správu agentů SCOM.  Pro jiné agenty jde AOI -\<ID pracovního prostoru\> |
| TimeGenerated |Datum a čas vytvoření nebo aktualizovat skupiny počítačů. |

## <a name="next-steps"></a>Další kroky
* Další informace o [protokolu hledání](log-analytics-log-searches.md) analyzovat data shromážděná ze zdrojů dat a řešení.  

