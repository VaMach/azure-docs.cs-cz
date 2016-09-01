<properties
    pageTitle="Migrace aplikací logiky na schéma verze 2015-08-01-preview | Microsoft Azure App Service"
    description="Aplikace logiky můžete snadno migrovat na schéma nejnovější verze. Postupujte podle těchto kroků."
    services="logic-apps"
    documentationCenter=""
    authors="MSFTMAN"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/20/2016"
    ms.author="deonhe"/>

# Migrace aplikací logiky na schéma verze 2015-08-01-preview

Pokud chcete stávající aplikaci logiky migrovat na nové schéma, postupujte následovně:  
1. Otevřete aplikaci logiky na portálu Azure  
2. Klikněte na tlačítko Aktualizovat schéma:

 ![Ikona rozhraní API][step1]   
Na stránce Aktualizovat schéma uvidíte a získáte odkaz na dokument obsahující informace o vylepšeních v novém schématu: ![ikona rozhraní API][step2]

>[AZURE.NOTE] Když vyberete položku **Update Schema** (Aktualizovat schéma), můžeme automaticky spustit kroky migrace a poskytnout vám výstupní kód. S jeho pomocí můžete aktualizovat svou definici. Dbejte ale na to, abyste se drželi osvědčených kódovacích postupů, které jsou uvedeny níže v části **Osvědčené postupy**.

## Osvědčené postupy při migraci vašich aplikací logiky na nejnovější verzi schématu:  

- Migrovaný skript zkopírujte do nové aplikace logiky. Tu původní ale nepředělávejte, dokud migrovanou aplikaci řádně neotestujete a neujistíte se, že správně funguje.
- Svou aplikaci logiky si otestujte ještě **předtím**, než ji předáte do produkce.
- Po dokončení migrace spusťte aktualizaci Logic Apps, abyste všude, kde je to možné, využili [spravované rozhraní API](./apis-list.md). Například můžete začít používat Dropbox v2 všude, kde jste používali DropBox v1.


## Kam dál
-  [Naučte se své aplikace logiky přenášet ručně](../app-service-logic/app-service-logic-schema-2015-08-01.md)


<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png









<!--HONumber=Aug16_HO4-->


