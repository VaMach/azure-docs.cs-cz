<properties
 pageTitle="Co je Azure Scheduler? | Microsoft Azure"
 description="Azure Scheduler vám umožní deklarativně popisovat akce, které mají běžet v cloudu. Potom naplánuje a automaticky spustí tyto akce."
 services="scheduler"
 documentationCenter=".NET"
 authors="krisragh"
 manager="dwrede"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="hero-article"
 ms.date="08/18/2016"
 ms.author="krisragh"/>


# Co je Azure Scheduler?

Azure Scheduler vám umožní deklarativně popisovat akce, které mají běžet v cloudu. Potom naplánuje a automaticky spustí tyto akce.  Scheduler k tomu použije [portál Azure](scheduler-get-started-portal.md), kód, [REST API](https://msdn.microsoft.com/library/mt629143.aspx) nebo Azure PowerShell.

Scheduler vytváří, udržuje a vyvolává naplánovanou práci.  Scheduler nehostuje žádné úlohy ani nespouští žádný kód. On _vyvolává_ kód hostovaný jinde – v Azure, v lokálním umístění nebo u jiného poskytovatele. Vyvolává přes HTTP, frontu úložiště, frontu sběrnice nebo téma sběrnice.

Scheduler plánuje [úlohy](scheduler-concepts-terms.md) uchovává historii výsledků provedení úloh, která se dá zobrazit, a deterministicky a spolehlivě plánuje spouštění úloh. Azure WebJobs (součást funkce Web Apps v Azure App Service) a další plánovací nástroje Azure používají Scheduler na pozadí. [REST API Scheduleru ](https://msdn.microsoft.com/library/mt629143.aspx) pomáhá spravovat komunikaci pro tyto akce. Scheduler jako takový bez problémů podporuje [komplexní plánování a pokročilé opakování](scheduler-advanced-complexity.md).

Použití Scheduleru se samo nabízí v několika situacích. Příklad:

+ _Opakující se akce aplikace:_ Pravidelné shromažďování dat z Twitteru do informačního kanálu.
+ _Každodenní údržba:_ Každodenní vyřazování protokolů, zálohování a další úkoly týkající se údržby. Správce se například může rozhodnout zálohovat databáze v 1:00 v noci každý den dalších devět měsíců.

Scheduler umožní vytvářet, aktualizovat, odstraňovat, zobrazovat a spravovat úlohy a [kolekce úloh](scheduler-concepts-terms.md) programově pomocí skriptů a pomocí portálu.

## Viz také

 [Koncepty, terminologie a hierarchie entit služby Azure Scheduler](scheduler-concepts-terms.md)

 [Úvod do používání Scheduleru na portálu Azure](scheduler-get-started-portal.md)

 [Plány a fakturace ve službě Azure Scheduler](scheduler-plans-billing.md)

 [Sestavení komplexních plánů a pokročilé opakování ve službě Azure Scheduler](scheduler-advanced-complexity.md)

 [REST API služby Azure Scheduler – referenční informace](https://msdn.microsoft.com/library/mt629143)

 [Rutiny PowerShellu pro službu Azure Scheduler – referenční informace](scheduler-powershell-reference.md)

 [Vysoká dostupnost a spolehlivost služby Azure Scheduler](scheduler-high-availability-reliability.md)

 [Omezení, výchozí hodnoty a chybové kódy služby Azure Scheduler](scheduler-limits-defaults-errors.md)

 [Odchozí ověření služby Azure Scheduler](scheduler-outbound-authentication.md)



<!--HONumber=Sep16_HO3-->


