<properties 
    pageTitle="Úvod do služby Stream Analytics | Microsoft Azure" 
    description="Přečtěte si o službě Stream Analytics, která umožňuje v reálném čase analyzovat data streamovaná z platformy Internet věcí (IOT)." 
    keywords="analytics as a service, managed services, stream processing, streaming analytics, what is stream analytics"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="05/03/2016" 
    ms.author="jeffstok"/>


# Co je služba Stream Analytics?

Služba Azure Stream Analytics (ASA) je plně spravovaný nákladově efektivní stroj pro zpracování událostí v reálném čase, který pomáhá odkrývat informace hluboko skryté v získaných datech. Stream Analytics usnadňuje nastavení analytických výpočtů v reálném čase prováděných s daty streamovanými ze zařízení, senzorů, webů, sociálních médií, aplikací, systémů infrastruktury a dalších zdrojů.

Úlohy služby Stream Analytics můžete vytvářet pomocí několika kliknutí na Portálu Azure. Stačí určit zdroj vstupních dat, výstupní umístění výsledků úlohy a požadovanou transformaci dat vyjádřenou v jazyce podobném jazyku SQL. Na Portálu Azure můžete úlohu monitorovat, škálovat a měnit rychlost jejího zpracování z několika kilobajtů událostí až na gigabajty či ještě větší objemy událostí zpracovaných za sekundu.

Stream Analytics využívá výsledky dlouholeté práce divize Microsoft Research na vývoji vysoce vyladěných strojů pro zpracování datových proudů v časovém rámci a integraci programovacích jazyků k intuitivnímu definování takových úloh.

## K čemu můžu službu Stream Analytics používat?
V dnešní době nejrůznější sítě přenášejí obrovské objemy dat. Organizace, které mají nástroje umožňující zpracování těchto dat v reálném čase a provádění akcí na základě výsledků jejich analýz, mohou zásadním způsobem zvýšit efektivitu své činnosti a lépe se prosadit v konkurenčním boji na trhu. Scénáře analýz dat streamovaných v reálném čase se uplatňují ve všech odvětvích: na míru šité analýzy burzovních trhů v reálném čase a informace, které na jejich základě poskytují společnosti zabývající se poskytováním finančních služeb; zjišťování podvodů v reálném čase; služby ochrany dat a identity; spolehlivé ingestování a analýza dat vygenerovaných senzory a pohony vestavěnými ve fyzických objektech (technologie Internet věcí neboli IoT); analýzy dat o navštívených stránkách; aplikace pro správu vztahů se zákazníky (CRM) vysílající upozornění, když v určeném časovém rámci poklesne spokojenost zákazníků. Společnosti hledají nejpružnější, nespolehlivější a nejekonomičtější cesty provádění takových analýz datových proudů událostí v reálném čase ve vysoce konkurenčním prostředí moderních trhů.

## Klíčové funkce a výhody
-   **Snadné použití:** Stream Analytics podporuje jednoduchý deklarativní model dotazů popisujících transformace dat. K optimalizaci snadného použití Stream Analytics používá variantu jazyka SQL a osvobozuje zákazníky od nutnosti řešení komplexních technických problémů spojených se systémy pro zpracování datových proudů. Editor dotazů prohlížeče pro práci s jazykem [Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx) poskytuje funkci automatického dokončování IntelliSense, která pomáhá rychle a snadno implementovat dotazy pracující s časovou řadou, včetně dočasných spojení, agregací v časových oknech, dočasných filtrů a dalších běžných operací, jako jsou spojení, agreagace, projekce a filtry. Kromě toho máte k dispozici funkci testování dotazů s ukázkovými datovými soubory přímo v prohlížeči, která umožňuje rychlý iterativní způsob vývoje.  

-   **Škálovatelnost:** Stream Analytics poskytuje vysokou propustnost událostí až 1GB/s. Integrace se službou [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) umožňuje řešení ingestovat miliony událostí odesílaných z připojených zařízení, webů a souborů protokolů (mimo jiné) za sekundu. K dosažení tohoto výkonu Stream Analytics využívá funkci rozdělení do oddílů poskytovanou službou Event Hubs, která umožňuje zpracovat až 1 MB dat za sekundu na jeden oddíl. Uživatelé mohou v definici dotazu rozdělit výpočet do množství logických kroků, z nichž každý lze dále dělit, a dále tak zvyšovat škálovatelnost celého řešení.  

-   **Spolehlivost, opakovatelnost a rychlé obnovení:** Stream Analytics je služba spravovaná v cloudu, která pomáhá bránit ztrátám dat a podporovat kontinuitu provozu díky vestavěným funkcím pro obnovení, které lze využívat v případě chyb. Díky schopnosti interního zachování stavu služba poskytuje možnost archivace událostí a opakovaného zpracování dat v budoucnosti, které vždy vrátí stejné výsledky. To umožňuje zákazníkům vracet se v čase a při provádění analýz původních příčin a citlivostních analýz opakovaně zkoumat výpočty a jejich výsledky.  

-   **Nízké náklady:** Jako cloudová služba je služba Stream Analytics optimalizovaná tak, aby zprovoznění a následný provoz řešení analýz v reálném čase byl pro uživatele co nejekonomičtější. Služba je zpoplatněna na základě jednotek datových proudů a tedy míry svého používání ve smyslu objemu zpracovaných dat. Míra využití je založena na objemu zpracovaných událostí a výpočetním výkonu poskytnutém v rámci clusteru ke zpracování příslušné úlohy služby Stream Analytics.  

-   **Referenční data:** Stream Analytics poskytuje uživatelům možnost definování a používání referenčních dat. Může se jednat o historická data nebo data nepocházející z datových proudů, která se v průběhu času méně často mění. Systém zjednodušuje použití referenčních dat tím, že je lze používat jako jakékoliv jiné příchozí datové proudy a v reálném čase spojovat s dalšími datovými proudy událostí k provádění transformací.  

-   **Možnosti připojení:** Služba Stream Analytics se za účelem ingestování datových proudů připojuje přímo ke službám Azure Event Hubs a Azure IOT Hubs a za účelem ingestování historických dat přímo ke službě Azure Blob. Výsledky ze služby Stream Analytics lze zapisovat do služeb Azure Storage Blobs nebo Tables, Azure SQL DB, Event Hubs, Azure Service Bus Topics nebo Queues a Power BI, kde je lze následně vizualizovat, dále zpracovávat v pracovních postupech, používat v dávkových analýzách [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) nebo znovu zpracovávat jako řady událostí.. Při použití služby Event Hubs je možné kombinovat více různých instancí služby Stream Analytics s dalšími zdroji a stroji pro zpracování dat, aniž by došlo k narušení streamovací podstaty výpočtů.  

## Podpora
Další podporu naleznete v našem [fóru služby Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## Další kroky
Přečetli jste si úvod do služby Stream Analytics, spravované služby pro analýzy datových proudů z platformy Internet věcí. Další informace o této službě najdete v následujících článcích:

- [Začínáme používat službu Azure Stream Analytics](stream-analytics-get-started.md)
- [Škálování služby Stream Analytics](stream-analytics-scale-jobs.md)
- [Referenční příručka k jazyku Azure Stream Analytics Query Language](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Jun16_HO2-->


