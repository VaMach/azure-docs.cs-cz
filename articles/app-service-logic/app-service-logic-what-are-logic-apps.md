<properties 
    pageTitle="Co jsou Logic Apps?" 
    description="Další informace o App Service Logic Apps" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="04/07/2016"
    ms.author="klam"/>

#Co jsou Logic Apps?

| Stručná referenční příručka |
| --------------- |
| [Jazyk pro definování Logic Apps](https://msdn.microsoft.com/library/azure/mt643789.aspx) |
| [Dokumentace ke konektoru Logic Apps](../connectors/apis-list.md) |
| [Fórum Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) |

Azure App Service je plně spravovaná platforma typu PaaS (Platforma jako služba) pro vývojáře, která usnadňuje tvorbu webových, mobilních a integračních aplikací. Služba Logic Apps je součástí této sady. Umožňuje jakémukoli technickému uživateli nebo vývojáři automatizovat spouštění firemních procesů a workflow pomocí snadno použitelného vizuálního návrháře.

Nejlepší na tom je, že je možné Logic Apps kombinovat s integrovanými [spravovanými rozhraními API][spravovaná rozhraní API], díky čemuž lze snadno řešit i složité scénáře integrace: 

![Návrhář aplikace na základě toku](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

Jak již bylo zmíněno, s aplikacemi logiky je možné automatizovat podnikové procesy. Zde je několik příkladů:  
 
* Je možné automaticky replikovat nové záznamy v SQL databázi a pak poslat poštu na recepční místo.   
* Lze automaticky hledat negativní tweety a posílat je na kanál Slack.

Podobné scénáře je možné konfigurovat ve všech vizuálních návrhářích bez nutnosti napsat jediný řádek kódu. Začněte [vytvářet aplikaci logiky už teď][vytvoření].

## Proč Logic Apps?

Logic Apps umožňují vývojářům navrhovat postupy workflow, které začínají triggerem a pak provádějí posloupnost kroků. Každý krok vyvolá rozhraní API a zároveň se bezpečně postará o ověření a osvědčené postupy, jako je třeba stanovení kontrolních bodů a trvalé provádění.

Pokud chcete automatizovat jakýkoli firemní proces (například hledat negativní tweety a posílat je do interního kanálu Slack nebo replikovat záznamy o nových zákaznících z SQL do systému CRM hned, jak přijdou), pak Logic Apps usnadňují integraci různorodých zdrojů dat, od cloudových po lokální. Inspirujte se našimi [spravovanými rozhraními API][spravovaná rozhraní API], hned [začněte][vytvoření] a podívejte se, co je možné dělat. 

S našimi [spravovanými rozhraními API BizTalk][BizTalk] je navíc možné přejít i k pokročilým scénářům integrace. Pomohou vám v tom funkce [stroj pravidel][pravidla], [správa obchodních partnerů][TPM] a další.

- **Snadno použitelné návrhové nástroje** – Logic Apps je možné od začátku do konce navrhovat v prohlížeči. Začnete triggerem – od jednoduchého časového plánu až po scénáře, kdy k aktivaci dojde vždy, kdy se objeví tweet o vaší společnosti. Pak pomocí rozsáhlé galerie konektorů můžete nastavit orchestraci libovolného počtu akcí.

- **Snadné sestavení SaaS** – I rozložitelné úkoly, které je možné snadno popsat, je těžké implementovat v kódu. Díky Logic Apps je propojení různorodých systémů velmi jednoduché. Chcete v softwaru CRM vytvořit úkol, který je založen na aktivitě facebookového nebo twitterového účtu? Chcete propojit své cloudové marketingové řešení s lokálním účetním systémem? Aplikace logiky jsou ten nejrychlejší, nejspolehlivější způsob, jak dodat řešení těchto problémů.

- **Rychlý začátek pomocí šablon** – Abychom vám pomohli začít, poskytujeme [galerii šablon][šablony], které umožňují rychle vytvářet některá běžná řešení. Od pokročilých řešení BizTalk po jednoduché připojení SaaS, a dokonce i pár šablon vytvořených jen tak „pro zábavu“ – galerie je prostě nejrychlejší způsob, jak pochopit sílu Logic Apps.

- **Zaručená rozšiřitelnost** – Nevidíte rozhraní API, které potřebujete? Služba Logic Apps je navržena tak, aby pracovala s aplikacemi API. Je tak možné snadno vytvářet vlastní aplikace API, které se použijí jako vlastní rozhraní API. Můžete vytvořit novou aplikaci jen pro sebe nebo ji sdílet a monetizovat na Marketplace.

- **Skutečně efektivní integrace** – Začněte lehce a řiďte svůj růst podle toho, jak se vyvíjí vaše potřeby. Služba Logic Apps dokáže snadno využít síly BizTalku. BizTalk je špičkové integrační řešení od Microsoftu, které umožňuje profesionálům v oblasti integrace vytvářet řešení, která potřebují. Další informace o [možnostech BizTalku poskytovaných spolu s Logic Apps][BizTalk]

## Koncepty Logic Apps

Následující pojmy představují některé z klíčových součástí, které dohromady utváří prostředí Logic Apps. 

- **Workflow** – Logic Apps nabízí grafický způsob modelování firemních procesů jako posloupnosti kroků, tedy jako workflow.
- **Spravovaná rozhraní API** – Aplikace logiky potřebují přístup k datům a službám. Spravovaná rozhraní API jsou vytvořena speciálně k tomu, aby vám pomohla při připojování k datům a práci s nimi. Přehled dostupných rozhraní API najdete ve [spravovaných rozhraních API][spravovaná rozhraní API].
- **Triggery** – Některá spravovaná rozhraní API se mohou chovat i jako trigger. Trigger spouští novou instanci workflow na základě konkrétní události, například doručení e-mailu nebo změny v účtu Azure Storage.
-  **Akce** – Každému kroku, který je proveden po triggeru ve workflow, se říká akce. Každá akce je obvykle namapována k operaci spravované nebo vlastní aplikace API.
- **BizTalk** – Pro pokročilejší scénáře integrace nabízí služba Logic Apps možnosti z BizTalku. BizTalk je špičková platforma pro integraci od Microsoftu. Aplikace API pro BizTalk umožňují snadno zahrnout ověřování, transformaci, pravidla a další funkce do postupů workflow Logic Apps. Další informace najdete v části [Co jsou aplikace API pro BizTalk][BizTalk].

## Začínáme  

 - Pokud chcete začít s Logic Apps, postupujte podle kurzu [vytvoření aplikace logiky][vytvoření].  
 - [Zobrazení běžných příkladů a scénářů](app-service-logic-examples-and-scenarios.md)
 - [S Logic Apps můžete automatizovat firemní procesy.](http://channel9.msdn.com/Events/Build/2016/T694) 
 - [Jak integrovat systémy s Logic Apps](http://channel9.msdn.com/Events/Build/2016/P462)
- Další informace o platformě Azure App Service najdete v tématu [Azure App Service][App Service].

[BizTalk]: app-service-logic-what-are-biztalk-api-apps.md
[App Service]: ../app-service/app-service-value-prop-what-is.md
[vytvoření]: app-service-logic-create-a-logic-app.md
[spravovaná rozhraní API]: ../connectors/apis-list.md
[TPM]: app-service-logic-create-a-trading-partner-agreement.md
[pravidla]: app-service-logic-use-biztalk-rules.md
[šablony]: app-service-logic-use-logic-app-templates.md



<!--HONumber=Jun16_HO2-->


