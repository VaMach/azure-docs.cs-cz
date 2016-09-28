<properties 
    pageTitle="Co jsou konektory a aplikace API pro BizTalk" 
    description="Informace o aplikacích API, konektorech a aplikacích API BizTalk" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="09/01/2016" 
    ms.author="mandia"/>


# Co jsou konektory a aplikace API pro BizTalk

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


*Konektor* je typ aplikace API, která se zaměřuje na připojení. Konektory, podobně jako ostatní aplikace API, se používají ve webových aplikacích, mobilních aplikacích a aplikacích logiky. Konektory usnadňují připojování existujících služeb a pomáhají spravovat ověřování a poskytovat monitorování, analýzu a další funkce.

Kdokoli z vývojářů může vytvořit své vlastní aplikace API a soukromě je nasadit. V budoucnu budou vývojáři moci sdílet a monetizovat své vlastní aplikace API přes Marketplace. 

![Marketplace aplikací API](./media/app-service-logic-what-are-biztalk-api-apps/Marketplace.png)

Aby vývojáři mohli rychleji vytvářet řešení, přidal tým Azure do Marketplace několik konektorů, které vyhovují mnoha běžným scénářům. Kromě toho je k dispozici i několik funkcí Premium a BizTalk, aby bylo možné rozšířit dosah i na složité a pokročilé scénáře integrace.

K dispozici jsou různé úrovně služeb. Všechny úrovně zahrnují všechny konektory a aplikace API, včetně všech jejich funkcí.  

V části [Ceny App Service](https://azure.microsoft.com/pricing/details/app-service/) jsou tyto úrovně služeb popsány, a to včetně přehledu, co je jejich součástí. Následující oddíly popisují různé kategorie konektorů a aplikací API pro BizTalk.


## Hybridní konektory 
Hybridní konektory dále rozšiřují využitelnost ve velkých firmách tím, že přidávají možnost připojení pro [DB2](app-service-logic-connector-db2.md), [Informix](app-service-logic-connector-informix.md) a WebSphere MQ. 

## Služby EAI a EDI
Vytváření důležitých podnikových aplikací vyžaduje více než jen možnost propojení. Aplikace API pro BizTalk mají své základy ve špičkové integrační platformě Microsoftu – BizTalk Serveru – a poskytují tak funkce pro pokročilou integraci, které je možné snadno připojit k webové aplikaci, mobilní aplikaci nebo aplikaci logiky. Mezi některé z těchto integračních funkcí patří [ověření](app-service-logic-xml-validator.md), [extrahování](app-service-logic-xpath-extract.md), [transformace](app-service-logic-transform-xml-documents.md), [kodéry](app-service-logic-connector-jsonencoder.md), [správa obchodních partnerů](app-service-logic-connector-tpm.md) a podpora formátů EDI, jako jsou [X12](app-service-logic-connector-x12.md), [EDIFACT](app-service-logic-connector-edifact.md) a [AS2](app-service-logic-connector-as2.md).

Další prostředky: [Konektory B2B a aplikace API](app-service-logic-b2b-connectors.md)  
[Vytvoření procesu B2B](app-service-logic-create-a-b2b-process.md)  
[Vytvoření smlouvy s obchodním partnerem](app-service-logic-create-a-trading-partner-agreement.md)  
[Sledování zpráv B2B](app-service-logic-track-b2b-messages.md)  


## Pravidla
Obchodní pravidla v sobě nesou zásady a rozhodnutí řídící podnikové procesy. Pravidla jsou obvykle dynamická a v čase se z různých důvodů mění, například kvůli obchodním plánům, regulacím a mnoha dalším důvodům. [Pravidla BizTalk](app-service-logic-use-biztalk-rules.md) vám umožňují oddělit tyto zásady od kódu aplikace a zjednodušit a urychlit proces změn.

## Seznam konektorů a aplikací API
Úplný seznam konektorů a aplikací API obsažených v jednotlivých kategoriích, včetně konektorů Standard, BizTalk EAI, konektorů Premium a dalších, najdete v tématu [Seznam konektorů a aplikací API](app-service-logic-connectors-list.md).
 



<!--HONumber=Sep16_HO3-->


