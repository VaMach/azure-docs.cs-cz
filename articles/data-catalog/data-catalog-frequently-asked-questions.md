---
title: "Nejčastější dotazy k Azure Data Catalog | Microsoft Docs"
description: "Nejčastější dotazy k Azure Data Catalog, včetně funkcí pro zjišťování zdroje dat, poznámky a správu."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/15/2017
ms.author: maroche
ms.openlocfilehash: 7aa0f9b292a98c6137ecf4ae957b3cb86062f250
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Nejčastější dotazy k Azure Data Catalog
Tento článek obsahuje odpovědi na nejčastější dotazy týkající se služby Azure Data Catalog.

## <a name="what-is-azure-data-catalog"></a>Co je Azure Data Catalog?
Data Catalog je plně spravovaná služba, hostované ve službě Microsoft Azure, která slouží jako systém registrace a zjišťování datových zdrojů organizace. Pomocí katalogu Data Catalog každý uživatel, od analytiků datových vědců a vývojářů, může zaregistrovat, zjišťovat, pochopit a využívat zdroje dat.

## <a name="what-customer-challenges-does-it-solve"></a>Jaké zákazníka vyzve nemá vyřešit?
Katalog data Catalog řeší problémy "tmavý data" a zdroj dat zjišťování, aby uživatelé mohli zjistit a pochopení datových zdrojů organizace.

## <a name="what-are-its-target-audiences"></a>Jaké jsou jeho cílové skupiny?
Katalog data Catalog je určen pro technická a Netechnická uživatelů, včetně:

* Data vývojáři a profesionálové BI a analýzy: lidé, kteří jsou zodpovědní za vytváření dat a analýzy obsahu pro další osoby mohly využívat.
* Data stewards: uživatelé, kteří mají znalosti o datech, co znamená a jak je určen pro použití.
* Spotřebitelé dat: uživatelé, kteří potřebují mohli snadno zjistit, pochopit a připojte se k datům potřebují pro své práci, pomocí nástroje, které si sami vyberou.
* Centrální IT: uživatelé, kteří potřebují zjistitelnost stovky zdroje dat ve firemním uživatelům a potřebují k udržování dohled nad využití dat a kdo.

## <a name="what-is-its-availability-by-region"></a>Co je jeho dostupnost podle oblasti?
Data katalogu služby jsou nyní k dispozici v následujících datových centrech:

* Západní USA
* Východ USA
* Západní Evropa
* Severní Evropa
* Austrálie – východ
* Jihovýchodní Asie

## <a name="what-are-its-limits-on-the-number-of-data-assets"></a>Jaké jsou jeho omezení počtu datových prostředků?
Bezplatná edice Data Catalog je omezený na 5000 registrovaných datových prostředků.

Na Data Catalog, Standard Edition podporuje až 100 000 registrované datové prostředky.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Jaké jsou jeho podporované datové typy zdroje a asset?
Seznam aktuálně podporované zdroje dat, naleznete v části [DSR katalogu dat](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Jak lze požádat o podporu pro jiný zdroj dat?
Odeslání žádosti o funkce a další zpětnou vazbu, přejděte na [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-do-i-get-started-with-data-catalog"></a>Jak můžu začít pracovat s katalogem Data Catalog?
Nejlepší způsob, jak začít pracovat se přechodem na [Začínáme s katalogem Data Catalog](data-catalog-get-started.md). Tento článek je začátku do konce přehled funkcí ve službě.

## <a name="how-do-i-register-my-data"></a>Jak se zaregistrovat svá data?
Registrace vaše data v katalogu Data Catalog:
1. Na portálu Azure Data Catalog v **publikovat** oblasti, spusťte nástroj pro registraci Azure Data Catalog. 
2. V katalogu Data Catalog publikování aplikaci Přihlaste se pomocí stejných přihlašovacích údajů, které umožňují přístup k portálu katalogu Data Catalog.
3. Vyberte zdroj dat a specifické prostředky, které chcete zaregistrovat.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Jaké vlastnosti ho extrahování datových prostředků, které jsou registrovány?
Konkrétní vlastnosti, které se liší od zdroj dat pro zdroj dat, ale obecně publikování služby Data Catalog extrahuje následující informace:

* Název prostředku
* Typ prostředku
* Popis prostředku
* Názvy atributů nebo sloupce
* Atribut nebo sloupce datové typy
* Popis atributu nebo sloupce

> [!IMPORTANT]
> Registrace datové prostředky pomocí katalogu Data Catalog přesuňte nebo zkopírujte svá data do cloudu. Registrace prostředky ze zdroje dat zkopíruje metadata datových zdrojů do Azure, ale data zůstávají v existujícím zdroji dat umístění. Výjimka, která má toto pravidlo je, pokud jste se rozhodli odeslat preview záznamy nebo profil dat při registraci prostředky. Pokud zahrnete náhledu, až 20 záznamů zkopírovány z každé asset a uložený jako snímek v katalogu Data Catalog. Když zahrnete data profilu, agregační informace se počítá a součástí metadata, která je uložené v katalogu. Agregační informace mohou zahrnovat velikost tabulky, procento hodnoty null na sloupec nebo minimální, maximální a průměrné hodnoty pro sloupce. 
>
>

> [!NOTE]
> Zdroje dat, jako je například SQL Server Analysis Services, které mají první třídy **popis** vlastnost katalogu dat publikování aplikace extrahuje hodnota této vlastnosti. Pro relační databáze systému SQL Server, které nemají první třídy **popis** vlastnost katalogu dat publikování aplikace extrahuje hodnotu z **ms_description** rozšířené vlastnosti pro objekty a sloupců. Další informace najdete v tématu [pomocí rozšířené vlastnosti pro databázové objekty](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-to-appear-in-the-catalog"></a>Jak dlouho má trvat pro nově registrovaných aktiv se objeví v katalogu?
Po registraci prostředky pomocí katalogu Data Catalog, může být po dobu 5 až 10 sekund, než se objeví na portálu katalogu Data Catalog.

## <a name="how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Jak opatřit poznámkami a zlepšit komunikaci metadata pro moje registrované datové prostředky?
Nejjednodušší způsob, jak poskytnout metadata pro registrovaných aktiv je vyberte asset na portálu katalogu Data Catalog a pak zadejte hodnoty v okně Vlastnosti nebo schéma pro vybraný objekt.

Některé metadata, například odborníky a značky, můžete zadat taky během procesu registrace. Hodnoty, které jsou uvedeny v publikování služby katalogu dat platí pro všechny prostředky registrované v daném čase. Chcete-li zobrazit objekty, nedávno registrovaný na portálu pro další poznámky, vyberte **zobrazit portál** tlačítko na poslední obrazovka katalog dat publikování aplikace.

## <a name="how-do-i-delete-my-registered-data-objects"></a>Jak lze odstranit mé registrované datové objekty?
Při odstraňování objektu z katalogu Data Catalog výběrem objektu v portálu a kliknutím **odstranit** tlačítko. Odebrání objektu odebere jeho metadata z katalogu Data Catalog, ale nemá vliv na podkladový zdroj dat.

## <a name="what-is-an-expert"></a>Co je odborník?
Odborník je osoba, která má informovanému perspektivy o datový objekt. Objekt může mít několik odborníky. Odborník nemusí být "vlastník" pro objekt, ale je jednoduše někoho, kdo ví, jak a slouží data.

## <a name="how-do-i-share-information-with-the-data-catalog-team-if-i-encounter-problems"></a>Jak sdílet informace s týmem katalogu Data Catalog problémy dojde-li?
Chcete odesílat zprávy o problémech, sdílet informace a ptají, přejděte na [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-the-catalog-work-with-another-data-source-that-im-interested-in"></a>Funguje s jiným zdrojem dat, která mám zájem katalogu?
Aktivně pracujeme na přidání více zdrojů dat do datového katalogu. Pokud chcete najdete v části konkrétní zdroj dat. podporována, ho navrhne (nebo hlasové podporu, pokud již byla navrhované) přechodem na [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Azure Data Catalog vztah do katalogu dat Power BI pro Office 365?
Azure Data Catalog si můžete představit jako vývojem katalogu Data v Power BI. Od verze pružiny 2017 Azure Data Catalog slouží k povolení sdílení a zjišťování dotazy v aplikaci Excel 2016 a Power Query pro Excel. Možnosti katalogu data Catalog v aplikaci Excel jsou k dispozici uživatelé s licencí pro Power BI Pro.

## <a name="what-permissions-do-i-need-to-register-assets-with-data-catalog"></a>Uvedete, jaká oprávnění je nutné zaregistrovat prostředky s katalogem Data Catalog?
Chcete-li spustit nástroj registrace katalogu Data Catalog, potřebujete oprávnění ve zdroji dat, která umožňuje číst metadata ze zdroje. Zahrnout také náhled, musí mít oprávnění, které vám umožní pro čtení dat z objektů registrována.

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Data Catalog budou dostupné pro místní nasazení také?
Data Catalog je Cloudová služba, které může fungovat pro cloudové a místní zdroje dat k poskytování řešení hybridní zdroj dat zjišťování. Aktuálně neexistují žádné plány pro verzi služby Data Catalog, která běží na místě.

## <a name="can-i-extract-more-or-richer-metadata-from-the-data-sources-i-register"></a>Můžete rozbalit další nebo bohatší metadat ze zdroje dat, které mohu zaregistrovat?
Pracujeme aktivně rozšířit možnosti katalogu Data Catalog. Pokud chcete získat další metadata extrahovaná ze zdroje dat během registrace, navrhnout ho (nebo hlas pro něj, pokud již byla navrhované) v [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). V budoucnu jsme vám umožní přidat nové typy zdrojů dat prostřednictvím rozhraní API rozšíření třetích stran.

## <a name="how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Jak se tak, aby je můžete zjišťovat pouze určitým uživatelům omezit viditelnost registrované datové prostředky?
Vyberte datové prostředky v katalogu dat a klikněte **převzít vlastnictví** tlačítko. Vlastníci datových prostředcích v katalogu Data Catalog můžete změnit nastavení viditelnosti buď umožnit všem uživatelům zjišťovat prostředky ve vlastnictví nebo omezit viditelnost konkrétním uživatelům.

## <a name="how-do-i-update-the-registration-for-a-data-asset-so-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Jak se tak, aby změny ve zdroji dat se projeví v katalogu aktualizovat registraci pro datový prostředek?
Pokud chcete aktualizovat metadata pro datové prostředky, které jsou již zaregistrovány v katalogu, jednoduše znovu zaregistrujte zdroje dat, který obsahuje prostředky. Všechny změny ve zdroji dat, jako je sloupce přidávání nebo odebírání z tabulky a zobrazení, jsou aktualizovány v katalogu, ale žádné poznámky poskytované uživatele zachovány.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Není můj dotaz zodpovězené. Kde lze přejít odpovědi?
Přejděte na [fórum Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Existuje kladené otázky najdete zde jejich představ.
