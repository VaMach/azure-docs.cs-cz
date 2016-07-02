<properties
    pageTitle="Seznam konektorů spravovaných společností Microsoft určených k použití v aplikacích Microsoft Azure Logic Apps | Microsoft Azure App Service"
    description="Získejte úplný seznam konektorů spravovaných společností Microsoft, které lze použít k vytváření aplikací logiky ve službě Azure App Service."
    services="app-service\logic"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/31/2016"
    ms.author="deonhe"/>

# Seznam spravovaných konektorů

>[AZURE.NOTE] Tato verze článku se vztahuje na verzi schématu 2015-08-01-preview platformy Logic Apps. Pokud potřebujete seznam pro verzi schématu 2014-12-01-preview, klikněte na [seznam konektorů](../app-service-logic/app-service-logic-connectors-list.md). 

Informace o cenách a obsahu jednotlivých úrovní služeb najdete v článku [Ceny služeb Azure App Service](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Pokud chcete začít se službou Azure Logic Apps, ještě než si zaregistrujete účet Azure, přejděte na webovou stránku s možností [ vyzkoušení vytvoření aplikace logiky](https://tryappservice.azure.com/?appservice=logic). V App Service si můžete okamžitě vytvořit jednoduchou aplikaci logiky s krátkou životností. Nevyžaduje se žádná platební karta a nevzniká žádný závazek.

Výběrem příslušné ikony získáte informace o tom, jak můžete tyto konektory rychle využít k vytváření aplikací volajících příslušné služby. Konektory lze použít k vytváření aplikací logiky, aplikací PowerApps a toků.

|Konektory||||
|-----------|-----------|-----------|-----------|
|[![Ikona rozhraní API][blobicon]<br/>**Azure Blob**][azureblobdoc]|[![Ikona rozhraní API][boxicon]<br/>**Box**][boxDoc]|[![Ikona rozhraní API][crmonlineicon]<br/>**CRM Online**][crmonlinedoc]|[![Ikona rozhraní API][dropboxicon]<br/>**Dropbox**][dropboxdoc]|
|[![Ikona rozhraní API][facebookicon]<br/>**Facebook**][facebookdoc]|[![Ikona rozhraní API][ftpicon]<br/>**FTP**][ftpdoc]|[![Ikona rozhraní API][githubicon]<br/>**GitHub**][githubdoc]|[![Ikona rozhraní API][googledriveicon]<br/>**Google Drive**][googledrivedoc]|
|[![Ikona rozhraní API][mailchimpicon]<br/>**MailChimp**][mailchimpdoc]|[![Ikona rozhraní API][microsofttranslatoricon]<br/>**Translator**][microsofttranslatordoc]|[![Ikona rozhraní API][office365icon]<br/>**Office 365**<br/>**Outlook**][office365outlookdoc]|[![Ikona rozhraní API][office365icon]<br/>**Office 365**<br/>**Users**][office365usersdoc]|
|[![Ikona rozhraní API][office365icon]<br/>**Office 365**<br/>**Video**][office365videodoc]|[![Ikona rozhraní API][onedriveicon]<br/>**OneDrive**][onedrivedoc]|[![Ikona rozhraní API][onedriveicon]<br/>**OneDrive<br/>for Business**][onedriveforbusinessdoc]|[![Ikona rozhraní API][outlookicon]<br/>**Outlook**][outlookdoc]|
|[![Ikona rozhraní API][projectonlineicon]<br/>**Project<br/>Online**][projectonlinedoc]|[![Ikona rozhraní API][rssicon]<br/>**RSS**][rssdoc]|[![Ikona rozhraní API][salesforceicon]<br/>**Salesforce**][salesforcedoc]|[![Ikona rozhraní API][sendgridicon]<br/>**SendGrid**][sendgriddoc]|
|[![Ikona rozhraní API][servicebusicon]<br/>**Service Bus**][servicebusdoc]|[![Ikona rozhraní API][sftpicon]<br/>**SFTP**][sftpdoc]|[![Ikona rozhraní API][sharepointicon]<br/>**SharePoint**<br/>**Online**][sharepointdoc]|[![Ikona rozhraní API][slackicon]<br/>**Slack**<br/>][slackdoc]|
|[![Ikona rozhraní API][smtpicon]<br/>**SMTP**][smtpdoc]|[![Ikona rozhraní API][sqlicon]<br/>**SQL Azure**][sqldoc]|[![Ikona rozhraní API][trelloicon]<br/>**Trello**][trellodoc]|[![Ikona rozhraní API][twilioicon]<br/>**Twilio**][twiliodoc]|
|[![Ikona rozhraní API][twittericon]<br/>**Twitter**][twitterdoc]|[![Ikona rozhraní API][wunderlisticon]<br/>**Wunderlist**][wunderlistdoc]|[![Ikona rozhraní API][yammericon]<br/>**Yammer**][yammerdoc] | |

> [AZURE.NOTE] Pokud jste již vytvářeli aplikace Logic Apps pomocí schématu 2014-12-01-preview, jistě jste si všimli, že konektory pro podnikovou integraci, například konektory pro BizTalk, zde nejsou uvedeny. Víme, že jsou důležité, a tvrdě pracujeme na tom, abyste je brzy mohli také používat. Sice vám nemůžeme říct, kdy přesně to bude, ale jejich zpřístupnění je jednou z našich největších priorit. Do té doby můžete [ze svých aplikací Logic Apps přistupovat k rozhraním API v1 a rozhraním API BizTalku](https://blogs.msdn.microsoft.com/logicapps/2016/02/25/accessing-v1-apis-and-biztalk-apis-from-logic-apps/). Děkujeme za pochopení. Sledujte nejnovější informace.


### Konektory lze používat jako triggery
Několik konektorů poskytuje triggery, které mohou vaší aplikaci odesílat upozornění na konkrétní události. Konektor FTP má například trigger OnUpdatedFile. Můžete vytvořit aplikaci logiky, aplikaci PowerApp nebo tok, které tomuto triggeru naslouchají a provádějí akci, kdykoliv se trigger spustí.

Existují dva typy triggerů:  

* Triggery dotazů: Tyto triggery odesílají službě v zadaných intervalech dotazy a kontrolují jejich pomocí dostupnost nových dat. Pokud jsou k dispozici nová data, spustí novou instanci aplikace a získaná data se použijí jako vstup. Aby se stejná data nepoužívala vícekrát, trigger také může odstranit data, která již aplikace přečetla a zpracovala.
* Triggery nabízených oznámení: Tyto triggery naslouchají datům nebo událostem v koncovém bodě. Poté spustí novou instanci aplikace. Jedním z příkladů je konektor twitter.


### Konektory mohou být akce
Konektory lze také v aplikacích používat jako akce. Akce jsou užitečné pro vyhledávání dat, která lze poté použít za běhu aplikace. Při zpracování objednávky můžete například potřebovat vyhledat informace o zákazníkovi v databázi SQL. Nebo může být nutné zapsat, aktualizovat nebo odstranit data v cílové tabulce. Můžete to provést pomocí akcí poskytovaných konektory. Akce se mapují na operace, které jsou definované v metadatech Swaggeru.


[Co je nového](../app-service-logic/app-service-logic-schema-2015-08-01.md)  
[Vytvořte si aplikaci logiky](../app-service-logic/app-service-logic-create-a-logic-app.md)  
[Začněte s aplikacemi PowerApps](../power-apps/powerapps-get-started-azure-portal.md)  
[Migrujte existující aplikace logiky na nejnovější verzi schématu](connectors-schema-migration.md) 

<!--Connectors Documentation-->
[azureblobdoc]: ./connectors-create-api-azureblobstorage.md "Umožňuje připojení k objektům blob Azure ke správě souborů v kontejnerech objektů blob."
[bingsearchDoc]: ./connectors-create-api-bingsearch.md "Umožňuje vyhledávání webů, obrázků, zpráv a videí pomocí Bingu."
[boxDoc]: ./connectors-create-api-box.md "Umožňuje připojení k Boxu a odesílání, získávání, odstraňování, zobrazování seznamů a další operace se soubory."
[crmonlinedoc]: ./connectors-create-api-crmonline.md "Umožňuje připojení k aplikaci Dynamics CRM Online a provádění dalších akcí s daty aplikace CRM Online."
[dropboxdoc]: ./connectors-create-api-dropbox.md "Umožňuje připojení k Dropboxu a odesílání, získávání, odstraňování, zobrazování seznamů a další operace se soubory."
[exceldoc]: ./connectors-create-api-excel.md "Umožňuje připojení k Excelu."
[facebookdoc]: ./connectors-create-api-facebook.md "Umožňuje připojení k Facebooku, přidávání příspěvků na Timeline, získávání kanálů stránek a provádění dalších akcí."
[ftpdoc]: ./connectors-create-api-ftp.md "Umožňuje připojení k serverům FTP/FTPS a provádění různých úloh protokolu FTP, včetně odesílání, získávání a odstraňování souborů a provádění dalších akcí."
[googledrivedoc]: ./connectors-create-api-googledrive.md "Umožňuje připojení ke GoogleDrivu a interagování s daty."
[microsofttranslatordoc]: ./connectors-create-api-microsofttranslator.md
[office365outlookdoc]: ./connectors-create-api-office365-outlook.md "Konektor Office 365 umožňuje odesílat a přijímat e-maily, spravovat kalendář a spravovat kontakty pomocí účtu Office 365."
[officeunifieddoc]: ./connectors-create-api-bingsearch.md
[office365usersdoc]: ./connectors-create-api-office365-users.md
[office365videodoc]: ./connectors-create-api-office365-video.md
[onedrivedoc]: ./connectors-create-api-onedrive.md "Umožňuje připojení k vašemu osobnímu Microsoft OneDrivu a odesílání souborů, odstraňování souborů, zobrazování seznamů souborů a provádění dalších akcí."
[onedriveforbusinessdoc]: ./connectors-create-api-onedriveforbusiness.md "Umožňuje připojení k vašemu obchodnímu Microsoft OneDrivu a odesílání souborů, odstraňování souborů, zobrazování seznamů souborů a provádění dalších akcí."
[outlookdoc]: ./connectors-create-api-outlook.md "Umožňuje připojení k vaší poštovní schránce Outlooku, přístup k e-mailu a provádění dalších akcí."
[projectonlinedoc]: ./connectors-create-api-projectonline.md "Umožňuje připojení k Microsoft Projectu Online."
[rssdoc]: ./connectors-create-api-rss.md "Konektor RSS umožňuje uživatelům publikovat a načítat položky informačních kanálů. Umožňuje také uživatelům spouštět operace, když jsou do informačních kanálů přidávány nové položky."
[salesforcedoc]: ./connectors-create-api-salesforce.md "Umožňuje připojení účtu Salesforcu a správu účtů, zájemců, příležitostí a provádění dalších akcí."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Umožňuje připojení k Microsoft Projectu Online."
[servicebusdoc]: ./connectors-create-api-servicebus.md "Umožňuje odesílání zpráv z fronty služby Service Bus a témat a příjem zpráv z fronty služby Service Bus a odběrů."
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Umožňuje připojení k SharePointu Online ke správě dokumentů a položek seznamů."
[slackdoc]: ./connectors-create-api-slack.md "Umožňuje připojení ke Slacku a odesílání zpráv do kanálů Slacku."
[sftpdoc]: ./connectors-create-api-sftp.md "Umožňuje připojení k protokolu SFTP a odesílání, získávání a odstraňování souborů a provádění dalších akcí."
[githubdoc]: ./connectors-create-api-github.md "Umožňuje připojení ke GitHubu a sledování problémů."
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Umožňuje odesílání lepších e-mailových zpráv."
[smtpdoc]: ./connectors-create-api-smtp.md "Umožňuje připojení k serveru SMTP a odesílání e-mailů s přílohami."
[sqldoc]: ./connectors-create-api-sqlazure.md "Umožňuje připojení ke službě Azure SQL Database. V tabulce databáze SQL můžete vytvářet, aktualizovat, získávat a odstraňovat položky."
[trellodoc]: ./connectors-create-api-trello.md "Trello je bezplatný flexibilní vizuální způsob organizace čehokoliv s kýmkoliv."
[twiliodoc]: ./connectors-create-api-twilio.md "Umožňuje připojení k Twiliu a odesílání a získávání zpráv, získávání dostupných čísel, správu příchozích hovorů z telefonních čísel a provádění dalších akcí."
[twitterdoc]: ./connectors-create-api-twitter.md "Umožňuje připojení k Twitteru a získávání časových os, odesílání tweetů a provádění dalších akcí."
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Umožňuje synchronizovat vše, co potřebujete k životu."
[yammerdoc]: ./connectors-create-api-yammer.md "Umožňuje připojení k Yammeru k odesílání zpráv a získávání nových zpráv."

<!--Icon references-->
[blobicon]: ./media/apis-list/blobicon.png
[bingsearchicon]: ./media/apis-list/bingsearchicon.png
[boxicon]: ./media/apis-list/boxicon.png
[ftpicon]: ./media/apis-list/ftpicon.png
[githubicon]: ./media/apis-list/githubicon.png
[crmonlineicon]: ./media/apis-list/dynamicscrmicon.png
[dropboxicon]: ./media/apis-list/dropboxicon.png
[excelicon]: ./media/apis-list/excelicon.png
[facebookicon]: ./media/apis-list/facebookicon.png
[googledriveicon]: ./media/apis-list/googledriveicon.png
[mailchimpicon]: ./media/apis-list/mailchimpicon.png
[microsofttranslatoricon]: ./media/apis-list/translatoricon.png
[office365icon]: ./media/apis-list/office365icon.png
[onedriveicon]: ./media/apis-list/onedriveicon.png
[onedriveforbusinessicon]: ./media/apis-list/onedriveforbusinessicon.png
[outlookicon]: ./media/apis-list/outlookicon.png
[projectonlineicon]: ./media/apis-list/projectonlineicon.png
[rssicon]: ./media/apis-list/rssicon.png
[salesforceicon]: ./media/apis-list/salesforceicon.png
[sendgridicon]: ./media/apis-list/sendgridicon.png
[servicebusicon]: ./media/apis-list/servicebusicon.png
[sftpicon]: ./media/apis-list/sftpicon.png
[sharepointicon]: ./media/apis-list/sharepointicon.png
[slackicon]: ./media/apis-list/slackicon.png
[smtpicon]: ./media/apis-list/smtpicon.png
[sqlicon]: ./media/apis-list/sqlicon.png
[trelloicon]: ./media/apis-list/trelloicon.png
[twilioicon]: ./media/apis-list/twilioicon.png
[twittericon]: ./media/apis-list/twittericon.png
[wunderlisticon]: ./media/apis-list/wunderlisticon.png
[yammericon]: ./media/apis-list/yammericon.png



<!--HONumber=Jun16_HO2-->


