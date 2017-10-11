---
title: "Přesunutí aplikace ze služby BizTalk Azure Logic Apps | Microsoft Docs"
description: "Přesuňte nebo migrovat MABS služby BizTalk Azure Logic Apps"
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: ladocs; jonfan; mandia
ms.openlocfilehash: e58c6950d1d9420f32fc98ca917216dc5fae4fc3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="move-from-biztalk-services-to-logic-apps"></a>Přesunutí ze služby BizTalk Services Logic Apps

Microsoft Azure BizTalk Services (MABS) jsou vyřazována z provozu. Použijte toto téma pro přesun řešení MABS integrace do Azure Logic Apps. 

## <a name="overview"></a>Přehled

BizTalk Services se skládá ze dvou dílčí služeb:

1.  Microsoft BizTalk Services hybridní připojení
2.  EAI a EDI na základě most integrace

Pokud hledáte, můžete přesunout hybridních připojení, potom [Azure App Service hybridní připojení](../app-service/app-service-hybrid-connections.md) popisuje změny a funkce této služby. Azure hybridní připojení nahrazuje BizTalk Services hybridní připojení. Azure hybridní připojení se službou Azure App Service k dispozici a je k dispozici v portálu Azure. Azure hybridní připojení poskytuje také nové správce hybridního připojení ke správě stávající hybridní připojení služby BizTalk Services a nové hybridní připojení, které vytvoříte na portálu. Azure App Service hybridní připojení je všeobecně dostupná (GA).

EAI a EDI na základě most integrace Logic Apps je náhradou. Služba Logic Apps poskytuje stejné funkce jako službu BizTalk Services a další. Služba Logic Apps poskytuje pracovních postupů založených na využívání cloudové škálování a orchestration funkce, které vám umožní rychle a snadno vytváření složitých integrace řešení pomocí prohlížeče, nebo pomocí nástrojů v sadě Visual Studio.

Následující tabulka obsahuje mapování funkcí služby BizTalk Services na Logic Apps.

| BizTalk Services   | Logic Apps            | Účel                  |
| ------------------ | --------------------- | ---------------------------- |
| konektor          | konektor             | Odesílání a příjmu dat   |
| Most             | Aplikace logiky             | Procesor kanálu           |
| Ověření fáze     | Ověření XML akce      | Ověření dokument XML pro schéma.             |
| Zlepšit komunikaci oddělení fáze       | Tokeny dat      | Povyšte vlastnosti do zprávy nebo pro rozhodnutí o směrování             |
| Fáze transformace    | Transformace akce      | Převod XML zprávy z jednoho formátu do druhého             |
| Dekódovat fáze       | Plochý soubor dekódovat akce      | Převést z plochého souboru XML             |
| Kódování fáze       |  Plochý soubor kódování akce      | Převod XML na plochý soubor             |
| Zpráva Inspector       |  Azure Functions nebo aplikace API      | Spuštění vlastního kódu v vaše integrace             |
| Akce trasy      |  Podmínka nebo přepínače      | Směrování zpráv do jednoho zadaného konektorů             |

Existuje několik různých typů artefakt ve službě BizTalk Services.

## <a name="connectors"></a>Konektory
Konektory ve službě BizTalk Services povolit mostů odesílat a přijímat data, včetně obousměrný mostů, které povolené interakce založené na protokolu HTTP žádosti a odpovědi. V Logic Apps se používá stejné technologiím. Konektory v aplikacích logiky mají stejné účely a také obsahovat více než 140, který se může připojit k široké pole technologie a služby, jak místně pomocí místní brána Data Gateway (nahrazení služba BizTalk Adapter Service používá služba BizTalk Services), a cloud SaaS a PaaS služby, jako je například OneDrive, Office 365, Dynamics CRM a mnoho dalších.

Zdroje ve službě BizTalk Services je omezen na předplatné FTP, SFTP a frontou Service Bus nebo téma.

![](media/logic-apps-move-from-mabs/sources.png)

Každý most má ve výchozím nastavení, který je konfigurován s adresou Runtime a vlastnosti relativní adresy mostu koncový bod HTTP. K dosažení totožný s Logic Apps, použít [žádostí a odpovědí](../connectors/connectors-native-reqres.md) akce.

## <a name="xml-processing-and-bridges"></a>Zpracování kódu XML a přemostění
Most ve službě BizTalk Services je obdobou zpracování kanálu. Most může trvat data přijatá z konektoru a provést některé práce s daty a potom ji odešlete do jiného systému. Služba Logic Apps dělá to stejné díky podpoře stejné vzory interakce na základě kanálu jako službu BizTalk Services a také poskytuje řadu dalších vzorech integrace. [Most požadavek-odpověď XML](https://msdn.microsoft.com/library/azure/hh689781.aspx) ve službě BizTalk Services se označuje jako VETER kanálu, který se skládá z fází, které můžou:

* (V) ověření
* (E) zlepšit komunikaci
* (T) transformace
* (E) zlepšit komunikaci
* (R) trasy

Jak je vidět na následujícím obrázku, zpracování je rozdělená mezi dotazů a odpovědí a umožňuje řídit požadavku a odpovědi cesty samostatně (například pomocí různých mapy pro každou):

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Kromě toho XML jednosměrný most přidá na začátek a konec zpracování dekódování a kódovat fázích a průchozí most obsahuje jeden úsek Enrich.

### <a name="message-processing-and-decodingencoding"></a>Zpracování a dekódování kódování zpráv
Ve službě BizTalk Services přijímat zprávy XML různých typů a určit odpovídající schéma pro přijatá zpráva. To se provádí v **typy zpráv** fáze kanálu zpracování příjmu. Potom fázi dekódování používá typ zjištěné zprávy k dekódování pomocí zadané schéma. Pokud schéma je schéma flatfile, převede příchozí flatfile do formátu XML. 

Služba Logic Apps poskytuje podobné funkce. Můžete přijímat flatfile přes velkého množství různé protokoly, pomocí různých konektor aktivačních událostí (systém souborů, FTP, HTTP a tak dále) a pomocí [plochý soubor dekódovat](../logic-apps/logic-apps-enterprise-integration-flatfile.md) akce příchozí data převést do formátu XML. Můžete přesunout vaší existující schémata plochý soubor přímo do aplikace logiky bez nutnosti změny a pak nahrajte schémata ke svému účtu integrace.

### <a name="validation"></a>Ověření
Po převedení příchozích dat XML (nebo pokud XML byl formát zprávy přijaté), probíhá ověření k určení, pokud zpráva dodržuje schéma XSD. Chcete-li to provést v Logic Apps, použijte [ověření XML](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) akce. Znovu můžete použít stejné schémat ze služby BizTalk Services beze změn.

### <a name="transform-messages"></a>Transformace zprávy
Ve službě BizTalk Services fázi transformace převod jednoho formátu zprávu formátu XML. To se provádí použitím mapu, pomocí na základě TRFM mapper. V Logic Apps je podobný procesu. Transformace akci provede mapu z vašeho účtu integrace. Hlavní rozdíl je, že map v Logic Apps jsou ve formátu XSLT. XSLT zahrnuje možnost opakovaně použít existující XSLT je již, včetně map vytvořených pro BizTalk serveru, které obsahují functoids. 

### <a name="routing-rules"></a>Pravidla směrování
BizTalk Services umožňuje směrování rozhodnutí, na které koncový bod/konektoru k odeslání příchozí zprávy nebo data. Umožňuje vybrat jednu z řady předem nakonfigurované koncové body je možné pomocí možnosti směrování filtru:

![](media/logic-apps-move-from-mabs/route-filter.png)

Služba Logic Apps poskytuje možnosti složitější logiku [podmínku](../logic-apps/logic-apps-use-logic-app-features.md) a [přepínač](../logic-apps/logic-apps-switch-case.md), povolení pokročilé řízení toku a směrování. Převádění filtrech směrování ve službě BizTalk Services nejlépe dosáhnete pomocí **podmínku** *Pokud* pouze dvě možnosti. Pokud je větší než dvě, použijte **přepínač**.

### <a name="enrich"></a>Zlepšit komunikaci oddělení
Fáze Enrich ve službě BizTalk Services zpracování přidává vlastnosti k zpráva kontext přidružený získaná data. Například povýšení vlastnosti, které chcete použít pro směrování (viz následující popis) z vyhledávání v databázi nebo extrahováním hodnotu pomocí výrazu XPath. Služba Logic Apps poskytuje přístup k všechny výstupy kontextuální data z předchozích akce, což přehledné replikovat stejné chování. Například pomocí `Get Row` akce připojení SQL, můžete vrátit data z databáze systému SQL Server a pomocí dat v rámci akce rozhodnutí pro směrování. Podobně vlastnosti sběrnice příchozích zpráv zařazených do fronty aktivační procedura jsou adresovatelné, jakož i pomocí výraz xpath pracovní postup definice jazyka XPath.

### <a name="use-custom-code"></a>Použít vlastní kód
Služba BizTalk Services nabízí možnost [spuštění vlastního kódu](https://msdn.microsoft.com/library/azure/dn232389.aspx) nahrát vlastní sestavení. Tato možnost je implementovaná pomocí [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx) rozhraní. Každá fáze v most zahrnuje dvě vlastnosti (na zadejte Inspector a na konec Inspector), poskytující .net typ, který jste vytvořili, který toto rozhraní implementuje. Vlastní kód umožňuje provádět složitější zpracování dat, jakož i opakovaně používat existující kód v sestavení, které provádějí běžné obchodní logiku. 

Služba Logic Apps poskytuje dva primární způsoby, jak provést vlastní kód: Azure Functions a aplikace API. Azure Functions můžete vytvořit a volání z aplikace logiky. V tématu [přidat a spuštění vlastní kód pro logic apps prostřednictvím Azure Functions](../logic-apps/logic-apps-azure-functions.md). Pomocí aplikace API, součástí Azure App Service, můžete vytvořit vlastní triggery a akce. Další informace o [vytváření vlastní rozhraní API pro použití s Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Pokud máte vlastní kód v assmeblies, kterou je možné volat z BizTalk Services, můžete přesunout tento kód do Azure Functions, nebo vytvořte vlastní rozhraní API s API Apps; v závislosti na tom, že implementace. Například pokud máte kód, který zabalí jiné službě, že Logic Apps nemá konektor, pak vytvořit aplikaci API a pomocí akce, které vaše aplikace API poskytuje v rámci aplikace logiky. Pokud máte podpůrné funkce nebo knihovny, pak Azure Functions je pravděpodobně nejlepší.

### <a name="edi-processing-and-trading-partner-management"></a>EDI zpracování a správa obchodních partnerů
BizTalk Services zahrnuje zpracování EDI a B2B s podporou pro AS2 (použitelnosti příkaz 2), X12 a EDIFACT. Proto nemá Logic Apps. Ve službě BizTalk Services vaší vytvoření mostů EDI a vytvořit a spravovat obchodní partnery a smluv na portálu správy a sledování vyhrazené.

Logic Apps, tato funkce je součástí [Enterprise integračního balíčku](../logic-apps/logic-apps-enterprise-integration-overview.md). Tento postup se skládá z integrace účet a B2B akcí pro zpracování EDI a B2B. [Integrace účet](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) slouží k vytváření a správě [obchodních partnerů](../logic-apps/logic-apps-enterprise-integration-partners.md) a [smlouvy](../logic-apps/logic-apps-enterprise-integration-agreements.md). Jakmile vytvoříte účet integrace, můžete přidružit jeden nebo více aplikací logiky k účtu. Jakmile spojen, můžete pro přístup k obchodním informace o partnerovi v rámci aplikace logiky B2B akce. Jsou k dispozici následující akce:

* Kódování AS2
* Dekódovat AS2
* X12 kódování
* Dekódovat X12
* EDIFACT kódování
* Dekódovat EDIFACT

Na rozdíl od služby BizTalk tyto akce jsou odpojené od přenosové protokoly. Při vytváření aplikace logiky, takže máte větší flexibilitu, na které konektory použijete k odesílat a přijímat data. Například možné jeho přijímat X12 soubory jako přílohy z e-mailu a pak proces těchto souborů v aplikaci logiky. 

## <a name="manage-and-monitor"></a>Správa a sledování
A správa obchodních partnerů, vyhrazené portál služby BizTalk Services zadat možnosti sledování ke sledování a řešení problémů. 

Služba Logic Apps poskytuje bohatší sledování a monitorování funkcí v [portál Azure](../logic-apps/logic-apps-monitor-your-logic-apps.md)a s [Operations Management Suite B2B řešení](../logic-apps/logic-apps-monitor-b2b-message.md), včetně mobilní aplikaci pro sledování věcí při jste na cestách.

## <a name="high-availability"></a>Vysoká dostupnost
K dosažení vysoké dostupnosti (HA) ve službě BizTalk Services, můžete použít více než jedna instance v dané oblasti sdílení zatížení. S logic apps v oblasti HA je integrovaný a dodává bez dalších poplatků. Pro zotavení po havárii na více systémů oblast pro zpracování B2B ve službě BizTalk Services se vyžaduje procesu zálohování a obnovení. V Logic Apps, mezi oblastmi aktivní/pasivní [možnost zotavení po Havárii](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) je poskytována – což umožňuje synchronizace dat B2B mezi účty pro integraci v různých oblastech pro kontinuitu podnikových procesů.

## <a name="next"></a>Další
* [Co je služba Logic Apps?](logic-apps-what-are-logic-apps.md)
* [Vytvořte první aplikaci logiky](logic-apps-create-a-logic-app.md) nebo můžete rychle začít pomocí [předem připravené šablony](logic-apps-use-logic-app-templates.md).  
* [Prohlédněte si všechny dostupné konektory](../connectors/apis-list.md), které můžete použít v aplikaci logiky.
