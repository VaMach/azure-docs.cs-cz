---
title: Seznam dostupných konektorů a aplikací API | Microsoft Docs
description: Přečtěte si o konektorech a aplikacích API v Azure App Service.
services: logic-apps
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: cgronlun

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: mandia

---
# Seznam konektorů a aplikací API k použití ve vašich Logic Apps
> [!NOTE]
> Tato verze článku se vztahuje na verzi schématu Logic Apps ve verzi 2014-12-01-preview. Informace k Logic Apps v obecně dostupné (GA) verzi najdete v tématu [Nový seznam konektorů](../connectors/apis-list.md).
> 
> 

Podívejte se na přehled všech dostupných konektorů a aplikací API vytvořených Microsoftem pro použití v rámci vašich Logic Apps.

Informace o cenách a obsahu jednotlivých úrovní služeb najdete v článku [Ceny služeb Azure App Service](https://azure.microsoft.com/pricing/details/app-service/).

> [!NOTE]
> Pokud chcete začít se službou Logic Apps, přejděte před registrací účtu Azure k [možnosti vyzkoušet si aplikaci logiky](https://tryappservice.azure.com/?appservice=logic). V App Service si můžete hned vytvořit krátkodobou úvodní aplikaci logiky. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.
> 
> 

## Základní konektory
Následující tabulka uvádí všechny dostupné konektory a aplikace API vytvořená Microsoftem, které jsou k dispozici jako základní konektory:

| Name (Název) | Popis |
| --- | --- |
| [Překladač Bing](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) |Pomocí služby Bing můžete přeložit text do jiného jazyka. |
| [HTTP](app-service-logic-connector-http.md) |Naslouchací proces HTTP otevře koncový bod, který funguje jako server HTTP a naslouchá příchozím požadavkům HTTP nebo HTTPS. Akce HTTP nevyžaduje aplikaci API a je nativně podporována v rámci Logic Apps. |
| [Slack](app-service-logic-connector-slack.md) |Umožňuje připojení k systému Slack a publikování zpráv do kanálů Slack. |

## Konektory pro integraci ve velkých firmách
Následující tabulka uvádí všechny dostupné konektory a aplikace API vytvořená Microsoftem, které jsou k dispozici jako konektory pro integraci ve velkých firmách:

| Název | Popis |
| --- | --- |
| [BizTalk Rules](app-service-logic-use-biztalk-rules.md) |Pomocí BizTalk Rules můžete definovat a řídit obchodní logiky v rámci organizace. Firemní zásady můžete aktualizovat bez nutnosti rekompilace nebo opětovného nasazení přidružených aplikací. |
| [BizTalk XPath Extractor](app-service-logic-xpath-extract.md) |Vyhledává a extrahuje data z obsahu XML na základě vámi zvoleného XPath. |
| [DB2 Connector](app-service-logic-connector-db2.md) |Připojí se k databázi IBM DB2 lokálně a na virtuálním počítači Azure s operačním systémem Windows. Může namapovat operace Web API a OData API k příkazům jazyka Informix Structured Query Language. <br/><br/>Žádné triggery. Akce zahrnují výběr tabulky, vložení, aktualizaci, odstranění a vlastní příkaz.<br/><br/>Tento konektor zahrnuje také Microsoft Client for DRDA, aby se bylo možné připojit k serveru Informix přes síť TCP/IP. |
| [File](app-service-logic-connector-file.md) |Pomocí tohoto konektoru se můžete připojit k lokálnímu systému souborů nebo síti a provádět další souborové úlohy, včetně jejich nahrávání, odstraňování, vytváření seznamů apod. |
| [Informix](app-service-logic-connector-informix.md) |Připojí se k databázi IBM Informix lokálně a na virtuálním počítači Azure s operačním systémem Windows. Může namapovat operace Web API a OData API k příkazům jazyka Informix Structured Query Language.<br/><br/>Žádné triggery. Akce zahrnují výběr tabulky, vložení, aktualizaci, odstranění a vlastní příkaz.<br/><br/>Při lokálním použití lze využít VPN nebo Azure ExpressRoute. Tento konektor zahrnuje také Microsoft Client for DRDA, aby se bylo možné připojit k serveru Informix přes síť TCP/IP. |
| [Microsoft SQL Server](app-service-logic-connector-sql.md) |Připojí se k lokálnímu systému SQL Server nebo k Azure SQL Database. Můžete vytvářet, aktualizovat, načítat a odstraňovat položky v tabulce databáze SQL. |
| MQ |Připojí se k systému IBM WebSphere MQ Server verze 8, a to lokálně a na virtuálním počítači Azure s operačním systémem Windows. Při lokálním použití lze využít VPN nebo Azure ExpressRoute. Tento konektor zahrnuje také Microsoft Client for MQ.<br/><br/>Žádné triggery. Žádná akce.<br/><br/>**Poznámka:** Aktuálně nelze používat s Logic Apps. |

## Konektory jako triggery
Několik konektorů poskytuje triggery pro Logic Apps. Tyto triggery jsou dvou typů:

1. Triggery dotazů: Tyto triggery se s určenou frekvencí dotazují služby, zda jsou k dispozici nová data. Když se objeví nová data, spustí se nová instance aplikace logiky, která použije tato data jako vstup. Aby se zabránilo vícenásobnému použití stejných dat, může trigger data, která již byla přečtena a předána do aplikace logiky, vyčistit. Příkladem takových konektorů jsou File, SQL a Azure Storage.
2. Triggery nabízených oznámení: Tyto triggery naslouchají datům v koncovém bodě nebo čekají na výskyt určité události. Pak spustí novou instanci aplikace logiky. Příkladem takových konektorů jsou naslouchací proces HTTP a Twitter.

## Konektory jako akce
Konektory lze použít také jako akce v rámci vaší aplikace logiky. Akce jsou užitečné pro vyhledávání dat v rámci aplikace logiky, který lze potom použít při jejich provádění. Při zpracování objednávky můžete například potřebovat vyhledat další informace o zákazníkovi z databáze SQL. Nebo může být nutné zapsat, aktualizovat nebo odstranit data v cílovém umístění. Můžete to provést pomocí akcí zajišťovaných konektory. Akce se mapují k operacím v aplikacích API (jak jsou definována v jejich metadatech Swagger).

## Vytvoření vlastních konektorů a aplikací API
[Referenční informace ke konektorům a aplikacím API](http://aka.ms/appservicesconnectorreference)  
[Triggery pro aplikace API Azure App Service](../app-service-api/app-service-api-dotnet-triggers.md)  
[Referenční informace k aplikacím logiky](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## Další informace o konektorech a aplikacích API
[Co jsou konektory a aplikace API pro BizTalk](app-service-logic-what-are-biztalk-api-apps.md)  
[Pomocí Správce hybridního připojení v Azure App Service](app-service-logic-hybrid-connection-manager.md)  
[Správa a monitorování integrovaných aplikací API a konektorů](app-service-logic-monitor-your-connectors.md)

<!--HONumber=Oct16_HO3-->


