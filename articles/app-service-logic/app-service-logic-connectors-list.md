<properties
    pageTitle="Seznam dostupných konektorů a aplikací API | Microsoft Azure App Service"
    description="Přečtěte si o konektorech a aplikacích API v Azure App Service."
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/28/2016"
    ms.author="mandia"/>


# Seznam konektorů a aplikací API k použití ve vašich Logic Apps
>[AZURE.NOTE] Tato verze článku se vztahuje na verzi schématu Logic Apps ve verzi 2014-12-01-preview. Informace k Logic Apps ve verzi obecné dostupnosti najdete v tématu [Nový seznam konektorů](../connectors/apis-list.md).

Podívejte se na přehled všech dostupných konektorů a aplikací API vytvořených Microsoftem pro použití v rámci vašich Logic Apps.

Informace o cenách a obsahu jednotlivých úrovní služeb najdete v článku [Ceny služeb Azure App Service](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Pokud chcete začít se službou Logic Apps, přejděte před registrací účtu Azure k [možnosti vyzkoušet si aplikaci logiky](https://tryappservice.azure.com/?appservice=logic). V App Service si můžete hned vytvořit krátkodobou úvodní aplikaci logiky. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.

## Základní konektory
Následující tabulka uvádí všechny dostupné konektory a aplikace API vytvořená Microsoftem, které jsou k dispozici jako základní konektory:

Name (Název) | Popis
--- | ---
[Azure Service Bus](app-service-logic-connector-azureservicebus.md) | Může odesílat zprávy z front a témat Service Bus a přijímat zprávy z front a předplatných Service Bus.
[Překladač Bing](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Pomocí služby Bing můžete přeložit text do jiného jazyka.
[HTTP](app-service-logic-connector-http.md) | Naslouchací proces HTTP otevře koncový bod, který funguje jako server HTTP a naslouchá příchozím požadavkům HTTP nebo HTTPS. Akce HTTP nevyžaduje aplikaci API a je nativně podporována v rámci Logic Apps.
[Aplikace Microsoft Office 365](app-service-logic-connector-office365.md) | Konektor Office 365 může odesílat a přijímat e-maily, řídit kalendář a spravovat kontakty pomocí vašeho účtu Office 365.
[QuickBooks](app-service-logic-connector-quickbooks.md) | Z Intuit QuickBooks můžete provádět různé úlohy, včetně vytváření a aktualizací různých entit (jako jsou zákazníci, položky, faktury atd.) a dotazování na ně.
[Slack](app-service-logic-connector-slack.md) | Umožňuje připojení k systému Slack a publikování zpráv do kanálů Slack.
[Wait](app-service-logic-connector-wait.md) | Pomocí tohoto konektoru můžete zpozdit provádění vaší aplikace. Aplikaci může pozdržet pro určitou dobu nebo do výskytu v určitém čase.


## Konektory pro integraci ve velkých firmách
Následující tabulka uvádí všechny dostupné konektory a aplikace API vytvořená Microsoftem, které jsou k dispozici jako konektory pro integraci ve velkých firmách:

Název  | Popis
------------- | -------------
[AS2 Connector](app-service-logic-connector-as2.md) | Může přijímat a odesílat zprávy pomocí přenosového protokolu AS2. Data se přenášejí zabezpečeně a spolehlivě s použitím digitálních certifikátů a šifrování.
[BizTalk EDIFACT](app-service-logic-connector-edifact.md) | Přijímá a odesílá zprávy pomocí protokolu EDIFACT v rámci komunikace B2B.
[BizTalk Flat File Encoder](app-service-logic-flatfile-encoder.md) | Poskytuje interoperabilitu mezi daty z plochých souborů (např. excelových a .csv) a daty XML. Tato aplikace API může převést instanci plochého souboru do XML a naopak.
[BizTalk JSON Encoder](app-service-logic-connector-jsonencoder.md) | Kodér a dekodér, která ve vaší aplikaci usnadňuje interoperabilitu mezi daty JSON a XML. Může převést danou instance JSON do XML a naopak.
[BizTalk Rules](app-service-logic-use-biztalk-rules.md) | Pomocí BizTalk Rules můžete definovat a řídit obchodní logiky v rámci organizace. Firemní zásady můžete aktualizovat bez nutnosti rekompilace nebo opětovného nasazení přidružených aplikací.
[BizTalk Trading Partner Management](app-service-logic-connector-tpm.md) | Definuje a trvale uchovává vztahy B2B mezi partnery, smlouvami a schématy a certifikáty používanými ve smlouvách. Tyto vztahy se vynucují pomocí aplikací API pro AS2, EDIFACT a X12.
[BizTalk Transform Service](app-service-logic-transform-xml-documents.md) | Převádí data z jednoho formátu do jiného. Můžete také nahrát existující mapu (soubor .trfm), zobrazit si propojení mezi zdrojovými a cílovými schématy a využívat funkci Test s ukázkovým vstupním obsahem XML. K dispozici jsou různé integrované funkce, včetně manipulace s řetězci, podmíněného přiřazování a dalších.
[BizTalk X12](app-service-logic-connector-x12.md) | Přijímá a odesílá zprávy pomocí protokolu X12 v rámci komunikace B2B.
[BizTalk XML Validator](app-service-logic-xml-validator.md) | Ověří data XML oproti předdefinovaným schématům XML. Můžete využívat existující schémata nebo vygenerovat schémata na základě instance plochého souboru, instance JSON nebo existujících konektorů.
[BizTalk XPath Extractor](app-service-logic-xpath-extract.md) | Vyhledává a extrahuje data z obsahu XML na základě vámi zvoleného XPath.
[DB2 Connector](app-service-logic-connector-db2.md) | Připojí se k databázi IBM DB2 lokálně a na virtuálním počítači Azure s operačním systémem Windows. Může namapovat operace Web API a OData API k příkazům jazyka Informix Structured Query Language. <br/><br/>Žádné triggery. Akce zahrnují výběr tabulky, vložení, aktualizaci, odstranění a vlastní příkaz.<br/><br/>Tento konektor zahrnuje také Microsoft Client for DRDA, aby se bylo možné připojit k serveru Informix přes síť TCP/IP.
[File](app-service-logic-connector-file.md) | Pomocí tohoto konektoru se můžete připojit k lokálnímu systému souborů nebo síti a provádět další souborové úlohy, včetně jejich nahrávání, odstraňování, vytváření seznamů apod.
[FTP<br/>FTPS](app-service-logic-connector-ftp.md) | Připojí se k serveru FTP/FTPS a provádí různé úlohy FTP, včetně nahrávání, získávání a odstraňování souborů apod.
[Informix](app-service-logic-connector-informix.md) | Připojí se k databázi IBM Informix lokálně a na virtuálním počítači Azure s operačním systémem Windows. Může namapovat operace Web API a OData API k příkazům jazyka Informix Structured Query Language.<br/><br/>Žádné triggery. Akce zahrnují výběr tabulky, vložení, aktualizaci, odstranění a vlastní příkaz.<br/><br/>Při lokálním použití lze využít VPN nebo Azure ExpressRoute. Tento konektor zahrnuje také Microsoft Client for DRDA, aby se bylo možné připojit k serveru Informix přes síť TCP/IP.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Připojí se k lokálnímu systému SQL Server nebo k Azure SQL Database. Můžete vytvářet, aktualizovat, načítat a odstraňovat položky v tabulce databáze SQL.
MQ | Připojí se k systému IBM WebSphere MQ Server verze 8, a to lokálně a na virtuálním počítači Azure s operačním systémem Windows. Při lokálním použití lze využít VPN nebo Azure ExpressRoute. Tento konektor zahrnuje také Microsoft Client for MQ.<br/><br/>Žádné triggery. Žádná akce.<br/><br/>**Poznámka:** Aktuálně nelze používat s Logic Apps.
[Oracle Database](app-service-logic-connector-oracle.md) | Připojí se k lokální Oracle Database a může vytvářet, aktualizovat, načítat a odstraňovat položky v databázové tabulce.
[POP3](app-service-logic-connector-pop3.md) (Post Office Protocol)| Připojí se k serveru POP3, aby bylo možné načítat e-maily s přílohami.
[SAP](app-service-logic-connector-sap.md) | Připojí se k lokálnímu serveru SAP a provádí volání RFC, BAPI a tRFC a odesílá IDOC.

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



<!---HONumber=Aug16_HO4-->


