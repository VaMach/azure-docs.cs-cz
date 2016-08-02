<properties
   pageTitle="SQL Data Warehouse (Preview) – očekávání | Microsoft Azure"
   description="Souhrn funkcí verze Public Preview a našich cílů vzhledem k obecné dostupnosti produktu SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="happynicolle"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="06/05/2016"
   ms.author="nicw;barbkess;sonyama"/>


# SQL Data Warehouse (Preview) – očekávání

Tento článek popisuje možnosti funkce SQL Data Warehouse Preview a naše cíle z hlediska služeb pro obecnou dostupnost. Tyto informace budeme průběžně aktualizovat společně s tím, jak budeme vylepšovat možnosti a funkce verze Public Preview.

Naše cíle pro produkt SQL Data Warehouse:

- Předvídatelný výkon a lineární škálovatelnost až na petabajty dat
- Vysoká spolehlivost pro všechny operace datového skladu
- Krátká doba od načtení dat po zobrazení statistiky dat napříč relačními i nerelačními daty

Tyto cíle se budeme snažit průběžně plnit při vylepšování Preview verze produktu SQL Data Warehouse.

## Předvídatelný a škálovatelný výkon

Azure SQL Data Warehouse zavádí tzv. jednotky datového skladu (DWU – Data Warehouse Units) jako způsob měření výpočetních prostředků (procesoru, paměti, úložiště a vstupně-výstupních operací) dostupných pro datový sklad. Zvýšením počtu jednotek DWU se zvyšuje objem prostředků. Při vyšších počtech jednotek DWU spouští SQL Data Warehouse operace paralelně (např. načtení dat nebo dotazy na data) napříč více distribuovanými zdroji. Tím se snižuje latence a zvyšuje výkon.

Každý datový sklad má 2 základní metriky výkonu:

- Rychlost načítání. Počet záznamů, které je možné načíst do datového skladu za sekundu. Konkrétně měříme počet záznamů, které je možné prostřednictvím PolyBase importovat z Azure Blob Storage do tabulky pomocí clusterovaného indexu úložiště sloupců.
- Rychlost prohledávání. Počet záznamů, které je možné sekvenčně načíst z datového skladu za sekundu. Konkrétně měříme počet záznamů vrácených dotazem pro clusterovaný index columnstore.

Vyhodnocujeme některá důležitá vylepšení výkonu a brzy se s vámi podělíme o očekávané rychlosti. Během doby, ve které bude dostupná verze Preview, se budeme průběžně snažit dosáhnout nejrůznějších vylepšení (např. zvýšení komprese a ukládání do mezipaměti), abychom dokázali tyto rychlosti zvýšit a zajistili jejich předvídatelné škálování.  

## Ochrana dat

SQL Data Warehouse ukládá všechna data v úložišti Azure pomocí místně redundantního úložiště. V místním datovém centru se udržuje několik synchronních kopií dat, aby se zajistila transparentní ochrana dat v případě lokálních selhání. 

## Zálohování

SQL Data Warehouse zálohuje všechna data minimálně každých 8 hodin pomocí snímků úložiště Azure Storage Snapshots. Tyto snímky jsou uchovávány 7 dní. To umožňuje obnovení dat minimálně do 21 bodů v čase za posledních 7 dní až do doby, kdy byl proveden poslední snímek. Data můžete ze snímku obnovit přes prostředí PowerShell nebo pomocí rozhraní REST API.

## Spolehlivost dotazů

Řešení SQL Data Warehouse je postaveno na architektuře MPP (Massively Parallel Processing). SQL Data Warehouse automaticky zjišťuje selhání uzlu Výpočty a řízení a migruje ho. V důsledku selhání nebo migrace uzlu ale může selhat operace (např. načtení dat nebo dotaz na data). Verzi Preview průběžně vylepšujeme, aby bylo možné operace dokončovat bez ohledu na selhání uzlů.


## Upgrady a výpadky

SQL Data Warehouse se bude pravidelně aktualizovat, aby bylo možné přidávat nové funkce a instalovat důležité opravy.  Tyto upgrady můžou narušovat běh produktu a v současné době se upgrady neprovádí podle předvídatelného plánu.  Pokud pro vás bude tento proces příliš rušivý, doporučujeme vám [vytvořit lístek podpory][], abychom vám mohli pomoct najít alternativní řešení.


## Další kroky

[Začínáme][] s verzí Public Preview.

<!--Image references-->

<!--Article references-->
[vytvořit lístek podpory]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Začínáme]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other Web references-->



<!--HONumber=Jun16_HO2-->


