---
title: "Vítá vás aplikace Wingtips – Azure SQL Database | Microsoft Docs"
description: "Informace o modelech databáze klientů a o Wingtips SaaS ukázkovou aplikaci pro Azure SQL Database v cloudovém prostředí."
keywords: kurz k sql database
services: sql-database
documentationcenter: 
author: billgib
manager: craigg
editor: MightyPen
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Active
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/12/2017
ms.author: billgib;genemi
ms.openlocfilehash: e10a954ba57782f4f79131ab583b5a73edf4ba02
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/14/2017
---
# <a name="welcome-to-the-wingtip-tickets-sample-saas-azure-sql-database-tenancy-app"></a>Vítá vás adresář Wingtip lístky ukázková SaaS Azure SQL Database klientů aplikace

Vítá vás adresář Wingtip lístky ukázkové SaaS Azure SQL Database klientů aplikace a její kurzy. Databáze klientů odkazuje na režimu izolace data, která vaše aplikace poskytuje klientům, kteří platí pro hostování v aplikaci. Přes zjednodušit momentálně, buď každý klient má celé databáze na sebe, nebo sdílí databázi s jiným klientem.

## <a name="wingtip-tickets-app"></a>Adresář Wingtip lístky aplikace

Ukázkovou aplikaci Wingtip lístky znázorňuje důsledky jiné databázi klientů modely na návrh a správu víceklientské aplikace SaaS. Doprovodné kurzy přímo popisují tyto stejné účinky. Adresář Wingtip lístky je založený na Azure SQL Database.

Adresář Wingtip lístky je určený k řešení různé scénáře návrhu a správu, které používají klienti skutečné SaaS. Vzory použití, který vyplývá jsou zahrnuté v Wingtip lístků.

Adresář Wingtip lístky aplikaci můžete nainstalovat v rámci vlastní předplatného Azure za pět minut. Instalace zahrnuje vložení ukázková data pro několik klientů. Bezpečně můžete nainstalovat aplikace a skripty správy pro všechny modely, protože instalace portálu komunikovat nebo komunikaci.

#### <a name="code-in-github"></a>Kód v Githubu

Kód aplikace a skripty správy, jsou všechny dostupné na Githubu:

- **Samostatné aplikace** modelu: *(už v rámci dnů).*
- **Databáze za klienta** modelu: [WingtipSaaS úložiště](https://github.com/Microsoft/WingtipSaaS/).
- **Horizontálně dělené víceklientské** modelu *hybridní*: *(už v rámci dnů).*

Stejný základní aplikace Wingtip lístky jeden kód se znovu použije pro všechny předchozí modely uvedené. Můžete spustit projekty SaaS kód z Githubu.



## <a name="major-database-tenancy-models"></a>Hlavní databáze klientů modely

Adresář Wingtip lístky je událost výpis a lístků pro aplikace SaaS. Adresář Wingtip poskytuje služby, které jsou vyžadovány místa. Všechny následující položky se vztahují na každý místo:

- Platí, můžete pro hostování v aplikaci.
- Je *klienta* v Wingtip.
- Události hostitele. Se podílejí následující události:
    - Ceny lístku.
    - Prodej lístků.
    - Zákazníci, kteří si lístků.

Aplikace, společně s kurzy a správu skripty umožňující prezentovat úplného scénáře SaaS. Tento scénář zahrnuje tyto činnosti:

- Zřizování klientů.
- Monitorování a správa výkonu.
- Správa schématu.
- Mezi klienta, sestav a analýzy.

Všechny tyto aktivity jsou uvedené v jakémkoli škálování je potřeba.



## <a name="code-samples-for-each-tenancy-model"></a>Ukázky kódu pro každý model klientů

Se zdůrazňují sadu modely aplikace. Jiných implementacích však může kombinovat elementy dva nebo víc modelů.

#### <a name="standalone-app-model"></a>Samostatné aplikace modelu

![Samostatné aplikace modelu][standalone-app-model-62s]

Tento model používá jedním klientské aplikace. Proto tento model musí pouze jednu databázi a ukládá data pro pouze jednoho klienta. Klient má úplné izolaci od ostatních klientů v databázi.

Tento model můžete použít při prodeji instance aplikace na mnoha různých klientů, každý klient spustit samostatně. Klient je pak jenom klient. Zatímco databáze ukládá data pouze jednoho klienta, databáze ukládá data pro mnoho zákazníků klienta.

- *(Kurzy pro tento model bude publikována zde do několika dní. Odkaz bude v tomto poli.)*

#### <a name="database-per-tenant"></a>Databáze za klienta

![Databáze za klienta modelu][database-per-tenant-model-35d]

Tento model má několik klientů v instanci aplikace. Ještě pro každého nového klienta, jiná databáze je přidělena pro použití pouze k novému klientovi.

Tento model zajišťuje izolaci úplné databáze pro každého klienta. Služba Azure SQL Database má vyspělosti, aby tento model vyhovující.

- [Úvod do příklad databáze SQL SaaS víceklientské aplikace] [ saas-dbpertenant-wingtip-app-overview-15d] -Další informace o tomto modelu.

#### <a name="sharded-multi-tenant-databases-the-hybrid"></a>Horizontálně dělené víceklientské databází, hybridním

![Model horizontálně dělené víceklientské databáze, hybridním][sharded-multitenantdb-model-hybrid-79m]

Tento model má několik klientů v instanci aplikace. Tento model má také více klientů v některé nebo všechny její databáze. Tento model je vhodný pro nabídky různých úrovních služby tak, aby klienti mohou platit více Pokud hodnota úplné izolace databáze.

Schéma každé databáze obsahuje identifikátor klienta. Identifikátor klienta je i v tyto databáze, které ukládají pouze jednoho klienta.

- *(Kurzy pro tento model bude publikována zde do několika dní. Odkaz bude v tomto poli.)*




## <a name="tutorials-for-each-tenancy-model"></a>Kurzy pro každý model klientů

Každý model klientů je popsána následující:

- Sada kurz články.
- Zdrojový kód uložené v úložišti Github, který je vyhrazen pro model:
    - Kód pro aplikaci Wingtip lístků.
    - Kód skriptu pro scénáře správy.

#### <a name="tutorials-for-management-scenarios"></a>Podrobné pokyny pro scénáře správy

Kurz články pro každý model zahrnovat následující scénáře správy:

- Zřizování klienta.
- Sledování výkonu a správy.
- Správa schématu.
- Mezi klienta, sestav a analýzy.
- Obnovení jedné klienta do dřívějšího bodu v čase.
- Zotavení po havárii.



## <a name="next-steps"></a>Další kroky

- [Úvod do příklad databáze SQL SaaS víceklientské aplikace] [ saas-dbpertenant-wingtip-app-overview-15d] -Další informace o tomto modelu.

- [Víceklientské SaaS databáze klientů vzory][multi-tenant-saas-database-tenancy-patterns-60p]



<!-- Image references. -->

[standalone-app-model-62s]: media/saas-tenancy-welcome-wingtip-tickets-app/model-standalone-app.png "Samostatné aplikace modelu"

[database-per-tenant-model-35d]: media/saas-tenancy-welcome-wingtip-tickets-app/model-database-per-tenant.png "Databáze za klienta modelu"

[sharded-multitenantdb-model-hybrid-79m]: media/saas-tenancy-welcome-wingtip-tickets-app/model-sharded-multitenantdb-hybrid.png "Model horizontálně dělené víceklientské databáze, hybridním"



<!-- Article references. -->

[saas-dbpertenant-wingtip-app-overview-15d]: saas-dbpertenant-wingtip-app-overview.md

[multi-tenant-saas-database-tenancy-patterns-60p]: saas-tenancy-app-design-patterns.md

