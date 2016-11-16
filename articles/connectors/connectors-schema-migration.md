---
title: "Migrace aplikací logiky na schéma verze 2015-08-01-preview | Dokumentace Microsoftu"
description: "Aplikace logiky můžete snadno migrovat na schéma nejnovější verze. Postupujte podle těchto kroků."
services: logic-apps
documentationcenter: 
author: MSFTMAN
manager: erikre
editor: 
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ab22e0369781f9f9afb9b3df9e7fdd54660a959d


---
# <a name="how-to-migrate-logic-apps-to-schema-version-20150801preview"></a>Migrace aplikací logiky na schéma verze 2015-08-01-preview
Pokud chcete stávající aplikaci logiky migrovat na nové schéma, postupujte následovně:  

1. Otevřete aplikaci logiky na portálu Azure  
2. Klikněte na tlačítko Aktualizovat schéma:
   
   ![Ikona rozhraní API][step1]   
   Na stránce Aktualizovat schéma uvidíte a získáte odkaz na dokument obsahující informace o vylepšeních v novém schématu: ![ikona rozhraní API][step2]

> [!NOTE]
> Když vyberete položku **Update Schema** (Aktualizovat schéma), můžeme automaticky spustit kroky migrace a poskytnout vám výstupní kód. S jeho pomocí můžete aktualizovat svou definici. Dbejte ale na to, abyste se drželi osvědčených kódovacích postupů, které jsou uvedeny níže v části **Osvědčené postupy**.
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Osvědčené postupy při migraci vašich aplikací logiky na nejnovější verzi schématu:
* Migrovaný skript zkopírujte do nové aplikace logiky. Tu původní ale nepředělávejte, dokud migrovanou aplikaci řádně neotestujete a neujistíte se, že správně funguje.
* Svou aplikaci logiky si otestujte ještě **předtím**, než ji předáte do produkce.
* Po dokončení migrace spusťte aktualizaci Logic Apps, abyste všude, kde je to možné, využili [spravované rozhraní API](apis-list.md). Například můžete začít používat Dropbox v2 všude, kde jste používali DropBox v1.

## <a name="whats-next"></a>Kam dál
* [Naučte se své aplikace logiky přenášet ručně](../app-service-logic/app-service-logic-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png









<!--HONumber=Nov16_HO2-->


