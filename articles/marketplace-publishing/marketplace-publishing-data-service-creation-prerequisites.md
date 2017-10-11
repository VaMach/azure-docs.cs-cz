---
title: "Technické požadavky pro vytváření dat služby pro Marketplace | Microsoft Docs"
description: "Pochopit požadavky pro vytvoření datové služby pro nasazení a prodeje na Azure Marketplace"
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: aaff609a-1cd1-4146-98f4-d04166b0fce0
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 52827723477677bc292c645e2390c435fbad3ee4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Nabídka služeb pro Azure Marketplace technické požadavky pro vytvoření datové služby
> [!IMPORTANT]
> **V tuto chvíli jsme již nejsou registrace všechny nové služby Data vydavatele. Nové dataservices nebude získat schválení pro výpis.** Pokud máte SaaS obchodní aplikace, který chcete publikovat na AppSource můžete najít další informace [zde](https://appsource.microsoft.com/partners). Pokud máte IaaS aplikace nebo služby vývojáře, které chcete publikovat na webu Azure Marketplace můžete najít další informace [zde](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Přečtěte si důkladně před zahájením procesu a pochopit, kde a proč se provádí každý krok. Co nejvíce, měli byste Příprava informací o společnosti a další data, stáhněte potřebné nástroje, a vytvořit technické součásti před zahájením procesu vytvoření nabídky.

Musí mít připravené před zahájením procesu následující položky:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Rozhodnutí, na jakou technologii bude používat k publikování vaši nabídku Data Service
Vydavatel můžete rozhodnout mezi více technologií při publikování dat služby v Azure Marketplace. Hlavní technologie, které jsou podporovány popsané dole. Bez ohledu na to jakou technologii se používá k publikování dat služby, koncový uživatel používá data prostřednictvím **datový kanál OData** vystavený službou Azure Marketplace. Úplné informace týkající se služby OData lze najít na [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL databáze Azure
Datová sada připravena v produktech SQL Azure je odpovědnost vydavatele. Budete potřebovat pro předplatné Azure, zřídit odpovídající velikost databáze a nahrát Data do databáze SQL Azure. Vydavatel zodpovídá taky zachovat jejich data vždy aktuální. Další informace o odběru služby Azure můžete najít na [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)

Při přesunu dat do SQL Azure, Azure Marketplace můžete vystavit tabulek a zobrazení. Vydavatele můžete určit, které tabulek nebo zobrazení a sloupce jsou viditelné na koncový uživatel. Dále můžete také zadat poskytovateli obsahu sloupce, které může být dotazován koncový uživatel a ty, které jsou vrácena pouze v datové části. To poskytuje vysokou úroveň flexibilitu o tom, které by měly být vystaveny data v databázi. Sloupce, které může být dotazován musí být založenou na jeden nebo více indexů databáze.

## <a name="rest-based-web-service"></a>Na základě REST webové služby
Podporované protokol: **pouze HTTPS**

Existující služby REST, na základě mohou být zpřístupněny prostřednictvím Azure Marketplace. Protože datovou sadu je vždy vystavený pro koncového uživatele jako datový kanál OData, je nutné službu Azure Marketplace mohli mapovat službu OData na základě služby. K tomu, aby ZBÝVAJÍCÍ na základě koncové body nutné vystavit všech parametrů jako parametry protokolu HTTP.

Datová část musí být ve formuláři, který lze mapovat do odpovědi ATOM. Proto odpověď ze služby musí být ve formátu XML a můžete je jenom obsahovat jednu opakující se element, který obsahuje hodnoty datová část (např. sada záznamů). Službu Azure Marketplace namapuje uzlu s opakováním do uzlu položka ATOM a datové hodnoty do vlastnosti uzlů v rámci uzlu položka.

Informace o ověření (například rozhraní API klíče, ověřování tokenu, atd.) musí být zadaný jako parametr HTTP nebo v hlavičce protokolu HTTP (pár klíčových hodnot) – základní ověřování je podporováno také. Je třeba zadat platný klíč a tento klíč jsou určené všechny požadavky prostřednictvím Azure Marketplace. Ve vrstvě Azure Marketplace se stane uživatele monitorování a fakturace.

Chyby vrácené služby musí být namapována na stavové kódy HTTP. V případě, že služba vrátí kód XML, který obsahuje chybu tyto se chystáte mapovat pomocí služby Azure Marketplace stavové kódy HTTP.

## <a name="soap-based-web-services"></a>Webové služby SOAP na základě
Protokol: **pouze HTTPS**

Požadavky jsou stejné jako ostatní založené na části služby. Jediným rozdílem je, že parametry se dá zadat i v textu XML účtované službě vydavatele s každou žádost odeslanou prostřednictvím Azure Marketplace. To znamená, že jsou se překlad vztahuje HTTP parametry, které uživatel nezadá na front-endu, do elementů XML dokumentu XML účtované se žádostí o poskytovateli obsahu webové služby.

## <a name="odata-based-web-services"></a>OData na základě webové služby
Protokol: **pouze HTTPS**

Data mohou být zpřístupněny jako služba OData do Azure Marketplace. Systém bude předat služby pomocí a nahradí kořenový adresář služby Azure Marketplace kořenový adresář – zajistit, že všechny následné volání procházejí přes Azure Marketplace.

Služby OData nepotřebují pouze přejdete na databázi v back-end. OData podporuje jakýkoli druh úložiště nebo obchodní logiku k řízení služby.

## <a name="next-steps"></a>Další kroky
Zkontrolovat požadavky a dokončit nezbytné úlohy, můžete dál přesunout s vytvářením vaši nabídku služba dat podle popisu v [Průvodce publikování dat služby](marketplace-publishing-data-service-creation.md).

Nebo, pokud chcete zkontrolovat celkový proces a příslušné články pro každou z publikování fází, najdete v článku [Začínáme: postup publikování nabídky pro Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
