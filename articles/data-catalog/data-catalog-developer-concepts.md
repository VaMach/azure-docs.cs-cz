---
title: "Koncepty pro vývojáře data Catalog | Microsoft Docs"
description: "Úvod do klíčové koncepty v konceptuálním modelu Azure Data Catalog, jako zveřejněné prostřednictvím rozhraní API REST katalogu."
services: data-catalog
documentationcenter: 
author: spelluru
manager: jhubbard
editor: 
tags: 
ms.assetid: 89de9137-a0a4-40d1-9f8d-625acad31619
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/15/2017
ms.author: spelluru
ms.openlocfilehash: e3c26c2358c15d18c71b82fe1f389c039ecbd97b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-developer-concepts"></a>Koncepty pro vývojáře Azure Data Catalog
Microsoft **Azure Data Catalog** je plně spravovaná Cloudová služba, která poskytuje možnosti pro zjišťování zdroje dat a metadat zdroje dat crowdsourcingový. Vývojáři mohou použít service pomocí jeho rozhraní REST API. Seznámení s koncepcemi služby je důležité pro vývojáře úspěšně integraci s **Azure Data Catalog**.

## <a name="key-concepts"></a>Klíčové koncepty
**Azure Data Catalog** konceptuální model je založen na čtyři klíčové koncepty: **katalogu**, **uživatelé**, **prostředky**, a **poznámky**.

![Koncept][1]

*Obrázek 1 – Azure Data Catalog zjednodušené konceptuálního modelu*

### <a name="catalog"></a>Katalogu
A **katalogu** je kontejner nejvyšší úrovně pro všechny organizace ukládá metadata. Existuje **katalogu** povolených pro účet Azure. Katalogů, jsou svázané s předplatné Azure, ale pouze jedna **katalogu** lze vytvořit pro jakékoli dané účet Azure, i když se účet může mít několik odběrů.

Obsahuje katalog **uživatelé** a **prostředky**.

### <a name="users"></a>Uživatelé
Uživatelé jsou objekty zabezpečení, které mají oprávnění k provádění akcí (vyhledat v katalogu, přidat, upravit nebo odebrat položky, atd.) v katalogu.

Existuje několik různých rolí, které uživatel může mít. Informace o rolích najdete v části role a autorizace.

Můžete přidat jednotlivé uživatele a skupiny zabezpečení.

Azure Data Catalog používá Azure Active Directory pro správu identit a přístupu. Každý uživatel katalogu musí být členem skupiny služby Active Directory pro účet.

### <a name="assets"></a>Prostředky
A **katalogu** obsahuje datové prostředky. **Prostředky** jsou jednotka členitost spravuje katalogu.

Zdroj dat se liší členitost prostředek. Pro SQL Server nebo databáze Oracle může být prostředek tabulku nebo zobrazení. Pro SQL Server Analysis Services může být prostředek míry, dimenze nebo klíčového ukazatele výkonu (KPI). Pro SQL Server Reporting Services je prostředek sestavy.

**Asset** jedinou přidat nebo odebrat z katalogu. Je to jednotka výsledku můžete získat zpět z **vyhledávání**.

**Asset** se skládá z jeho název, umístění a typ a poznámky, které další popisují ho.

### <a name="annotations"></a>Poznámky
Poznámky jsou položky, které představují metadata o prostředcích.

Příklady poznámky: popis, značky, schéma, dokumentace, atd. Úplný seznam asset typy a typy poznámky jsou v části Asset objektu modelu.

## <a name="crowdsourcing-annotations-and-user-perspective-multiplicity-of-opinion"></a>Crowdsourcingový poznámky a uživatel perspektivy (násobnost stanovisko)
Klíčovým prvkem Azure Data Catalog je, jak podporuje crowdsourcingový metadat v systému. Na rozdíl od svazků na wikiwebu přístup – kde je pouze jeden stanovisko a poslední wins zapisovače – model Azure Data Catalog umožňuje více názory TTL vedle sebe v systému.

Tento přístup odráží reálném světě podnikových dat, které umožňuje uživatelům používat různých perspektiv na daný prostředek:

* Správce databáze může poskytovat informace o smlouvy o úrovni služeb nebo v okně zpracování k dispozici pro operace hromadného ETL
* Data steward může poskytují informace o obchodních procesů, u kterých bude použito asset nebo klasifikace, které se má použít na firmu
* Analytik finanční poskytnout informace o použití dat během období koncového úlohám generování sestav

Pro podporu v tomto příkladu, může každý uživatel – správce databáze, steward dat a analytika – přidat popis do jedné tabulky, který byl zaregistrován v katalogu. Všechny popisy jsou zachována ve systému a na portálu Azure Data Catalog jsou zobrazeny všechny popisy.

Tento vzor se použije pro většinu položek v objektu modelu, takže typy objektů v datové části JSON jsou často pole vlastností kde může být typu singleton.

V části asset například kořenová je pole objektů popis. Vlastnost pole je s názvem "Popis". Popis objektu má jednu vlastnost – Popis. Vzor je, že každý uživatel, který získá popis typy objekt popis vytvořen pro hodnota zadaná uživatelem.

Uživatelského můžete zvolit způsob, jak zobrazit kombinace. Existují tři různé vzorce pro zobrazení.

* Nejjednodušším způsobem je "Zobrazit vše". V tomto vzoru se zobrazí všechny objekty v zobrazení seznamu. Na portálu Azure Data Catalog UX používá tento vzor pro popis.
* Jiné vzor je "Merge". V tomto vzoru všechny hodnoty z různých uživatelů sloučit společně s duplicitní odebrat. Příkladem tohoto vzoru na portálu Azure Data Catalog UX jsou vlastnosti značek a odborníky.
* Třetí vzor je "poslední zapisovače wins". V tomto vzoru se zobrazí pouze nejnovější hodnotu zadali. friendlyName je příklad tohoto vzoru.

## <a name="asset-object-model"></a>Asset objektový model
Jak byl uveden v části klíčové koncepty **Azure Data Catalog** objektový model obsahuje položky, které mohou být prostředky nebo poznámky. Položky mají vlastnosti, které můžou být požadované nebo volitelné. Některé vlastnosti se vztahují na všechny položky. Některé vlastnosti se vztahují na všechny prostředky. Některé vlastnosti se vztahují pouze na konkrétní majetek typy.

### <a name="system-properties"></a>Systémové vlastnosti
<table><tr><td><b>Název vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr><tr><td>časové razítko</td><td>Data a času</td><td>Čas poslední položka byla změněna. Toto pole je generovaný serverem, když vložíte položku a při každé aktualizaci položky. Hodnota této vlastnosti je ignorována na vstup z operací publikování.</td></tr><tr><td>id</td><td>identifikátor URI</td><td>Absolutní adresa url položky (jen pro čtení). Je jedinečný adresovatelné identifikátor URI pro položku.  Hodnota této vlastnosti je ignorována na vstup z operací publikování.</td></tr><tr><td>type</td><td>Řetězec</td><td>Typ prostředku (jen pro čtení).</td></tr><tr><td>Značka Etag</td><td>Řetězec</td><td>Řetězec odpovídající verzi položky, kterou lze použít pro optimistické řízení souběžného při provádění operace, které aktualizují položky v katalogu. "*" umožňuje hledat jakoukoli jinou hodnotu.</td></tr></table>

### <a name="common-properties"></a>Běžné vlastnosti
Tyto vlastnosti se vztahují na všechny typy asset kořenové a všechny typy poznámky.

<table>
<tr><td><b>Název vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>
<tr><td>fromSourceSystem</td><td>Logická hodnota</td><td>Určuje, zda je dat položky odvozené ze zdrojového systému (například databáze serveru Sql, databáze Oracle) nebo vytvořené uživatelem.</td></tr>
</table>

### <a name="common-root-properties"></a>Běžné vlastnosti kořenové
<p>
Tyto vlastnosti se vztahují na všechny typy kořenové asset.

<table><tr><td><b>Název vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr><tr><td>jméno</td><td>Řetězec</td><td>Název odvozený od umístění zdroje dat</td></tr><tr><td>DSL</td><td>DataSourceLocation</td><td>Jednoznačně popisuje zdroj dat a je jedním z identifikátory pro asset. (Viz část duální identity).  Struktura dsl se liší podle typu protokol a zdroj.</td></tr><tr><td>zdroj dat</td><td>DataSourceInfo</td><td>Další podrobnosti o typ prostředku.</td></tr><tr><td>lastRegisteredBy</td><td>SecurityPrincipal</td><td>Popisuje uživatel, který naposledy zaregistroval tento prostředek.  Obsahuje jedinečné id pro uživatele (upn) a zobrazované jméno (jméno a příjmení).</td></tr><tr><td>identifikátor containerId</td><td>Řetězec</td><td>ID prostředku kontejner pro zdroj dat. Tato vlastnost není podporována pro typ kontejneru.</td></tr></table>

### <a name="common-non-singleton-annotation-properties"></a>Běžné vlastnosti není typu singleton poznámky
Tyto vlastnosti se vztahují na všechny typy která není typu singleton poznámky (poznámky, které může být víc za asset).

<table>
<tr><td><b>Název vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>
<tr><td>key</td><td>Řetězec</td><td>Uživatel zadaný klíč, který jednoznačně identifikuje poznámky v aktuální kolekci. Délka klíče nesmí překročit 256 znaků.</td></tr>
</table>

### <a name="root-asset-types"></a>Kořenové asset typy
Kořenové asset typy jsou tyto typy, které představují různé typy datových prostředků, které lze registrovat v katalogu. Pro každý typ kořenové je zobrazení, která popisuje asset a poznámky, které jsou zahrnuty v zobrazení. V odpovídající segment adresy url {view_name} by měl použít název zobrazení, při publikování assetu pomocí rozhraní REST API.

<table><tr><td><b>Typ prostředku (název zobrazení)</b></td><td><b>Další vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Povolené poznámky</b></td><td><b>Komentáře</b></td></tr><tr><td>Tabulky ("tabulky")</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>Schéma<p>ColumnDescription<p>ColumnTag<p> odborné<p>Preview<p>AccessInstruction<p>TableDataProfile<p>ColumnDataProfile<p>ColumnDataClassification<p>Dokumentace<p></td><td>Tabulka představuje žádná tabulková data.  Příklad: tabulku SQL, zobrazení SQL, tabulkové tabulky Analysis Services, Analysis Services Multidimensional dimenze, tabulky Oracle atd.   </td></tr><tr><td>Míru ("opatření")</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>odborné<p>AccessInstruction<p>Dokumentace<p></td><td>Tento typ reprezentuje měr Analysis Services.</td></tr><tr><td></td><td>Míra</td><td>Sloupec</td><td></td><td>Metadat, která popisují míry</td></tr><tr><td></td><td>isCalculated </td><td>Logická hodnota</td><td></td><td>Určuje, pokud se počítá míru, nebo ne.</td></tr><tr><td></td><td>Skupina měr</td><td>Řetězec</td><td></td><td>Fyzické kontejner pro míru</td></tr><td>Klíčový ukazatel výkonu "(KPI)</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>odborné<p>AccessInstruction<p>Dokumentace</td><td></td></tr><tr><td></td><td>Skupina měr</td><td>Řetězec</td><td></td><td>Fyzické kontejner pro míru</td></tr><tr><td></td><td>goalExpression</td><td>Řetězec</td><td></td><td>Číselný výraz MDX nebo výpočet, který vrátí cílovou hodnotu klíčového ukazatele výkonu.</td></tr><tr><td></td><td>valueExpression</td><td>Řetězec</td><td></td><td>Číselný výraz MDX, který vrací aktuální hodnotu klíčového ukazatele výkonu.</td></tr><tr><td></td><td>statusExpression</td><td>Řetězec</td><td></td><td>Výraz MDX, který představuje stav klíčového ukazatele výkonu v určitém bodě v čase.</td></tr><tr><td></td><td>trendExpression</td><td>Řetězec</td><td></td><td>Výraz MDX, která vyhodnotí hodnotu klíčového ukazatele výkonu v čase. Trend může být jakékoli založené na čase kritérium, které jsou užitečné v konkrétní obchodní kontext.</td>
<tr><td>Sestavy ("sestavy")</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>odborné<p>AccessInstruction<p>Dokumentace<p></td><td>Tento typ reprezentuje sestavy služby SQL Server Reporting Services </td></tr><tr><td></td><td>assetCreatedDate</td><td>Řetězec</td><td></td><td></td></tr><tr><td></td><td>assetCreatedBy</td><td>Řetězec</td><td></td><td></td></tr><tr><td></td><td>assetModifiedDate</td><td>Řetězec</td><td></td><td></td></tr><tr><td></td><td>assetModifiedBy</td><td>Řetězec</td><td></td><td></td></tr><tr><td>Kontejner ("kontejner")</td><td></td><td></td><td>Popis<p>FriendlyName<p>Značka<p>odborné<p>AccessInstruction<p>Dokumentace<p></td><td>Tento typ reprezentuje kontejner jiných prostředků, jako je například SQL database, kontejner objektů BLOB služby Azure nebo model služby Analysis Services.</td></tr></table>

### <a name="annotation-types"></a>Typy poznámky
Typy poznámky představují typy metadat, které lze přiřadit k jiné typy v katalogu.

<table>
<tr><td><b>Typ poznámky (název zobrazení vnořené)</b></td><td><b>Další vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>

<tr><td>Popis ("Popis")</td><td></td><td></td><td>Tato vlastnost obsahuje popis pro určitý prostředek. Každý uživatel systému můžete přidat vlastní popis.  Pouze tento uživatel můžete upravit popis objektu.  (Správci a Asset vlastníky můžete odstranit objekt popis, ale nikoli pro úpravy). Systém udržuje uživatelů popisy samostatně.  Proto je pole popisů na každý prostředek (jeden pro každý uživatel, který má podílí své znalosti o majetku, kromě může být ten, který obsahuje informace, které jsou odvozené ze zdroje dat).</td></tr>
<tr><td></td><td>description</td><td>Řetězec</td><td>Krátký popis (řádky 2-3) prostředku</td></tr>

<tr><td>Značky ("značky")</td><td></td><td></td><td>Tato vlastnost definuje značky pro prostředek. Každý uživatel systému můžete přidat více značek pro určitý prostředek.  Pouze uživatele, který vytvořil objekty značku upravovat.  (Správci a Asset vlastníky můžete odstranit objekt značky, ale není upravit). Systém udržuje uživatelů značky samostatně.  Proto je pole objektů značky na každý prostředek.</td></tr>
<tr><td></td><td>Značka</td><td>Řetězec</td><td>Značku popisující asset.</td></tr>

<tr><td>FriendlyName ("friendlyName")</td><td></td><td></td><td>Tato vlastnost obsahuje popisný název pro určitý prostředek. FriendlyName je typu singleton poznámky – pouze jeden FriendlyName mohou být přidány do prostředek.  Pouze uživatele, který vytvořil objekt FriendlyName můžete upravit ho. (Správci a Asset vlastníky můžete odstranit objekt FriendlyName, ale nikoli pro úpravy). Systém udržuje popisné názvy uživatelů samostatně.</td></tr>
<tr><td></td><td>FriendlyName</td><td>Řetězec</td><td>Popisný název prostředku.</td></tr>

<tr><td>Schéma ("schéma")</td><td></td><td></td><td>Schéma popisuje strukturu dat.  Ji obsahuje atribut (sloupec, atribut, pole atd.) názvy, typy a další metadata.  Tyto informace je odvozený z datového zdroje.  Schéma je typu singleton poznámky – lze přidat pouze jedno schéma pro prostředek.</td></tr>
<tr><td></td><td>sloupce</td><td>Sloupce]</td><td>Pole objektů sloupce. Popisují sloupec s informacemi, které jsou odvozené z datového zdroje.</td></tr>

<tr><td>ColumnDescription ("columnDescriptions")</td><td></td><td></td><td>Tato vlastnost obsahuje popis pro sloupec.  Každý uživatel systému můžete přidat vlastní popisy pro více sloupců (maximálně jeden každý sloupec). Pouze uživatele, který vytvořil objekty ColumnDescription upravovat.  (Správci a Asset vlastníky můžete ColumnDescription objekt odstranit, ale nikoli pro úpravy). Systém udržuje popisy sloupců tyto uživatele samostatně.  Proto je pole objektů ColumnDescription na každý prostředek (jeden pro každý sloupec pro každý uživatel, který má podílí své znalosti o sloupci kromě může být ten, který obsahuje informace, které jsou odvozené ze zdroje dat).  ColumnDescription volně vázáno na schéma, můžete získat synchronizován. ColumnDescription může popisují sloupec, který již neexistuje ve schématu.  Je zapisovače pro synchronizaci popis a schéma.  Zdroj dat může mít také informace popisu sloupce a jsou další ColumnDescription objekty, které by se vytvoří při spuštění nástroje.</td></tr>
<tr><td></td><td>columnName</td><td>Řetězec</td><td>Název sloupce, který odkazuje tento popis.</td></tr>
<tr><td></td><td>description</td><td>Řetězec</td><td>Stručný popis (řádky 2-3) sloupce.</td></tr>

<tr><td>ColumnTag ("columnTags")</td><td></td><td></td><td>Tato vlastnost obsahuje značku pro sloupec. Každý uživatel systému můžete přidat více značek pro daný sloupec a můžete přidat značky pro více sloupců. Pouze uživatele, který vytvořil objekty ColumnTag upravovat. (Správci a Asset vlastníky můžete ColumnTag objekt odstranit, ale nikoli pro úpravy). Systém udržuje tito uživatelé sloupce značky samostatně.  Proto je pole objektů ColumnTag na každý prostředek.  ColumnTag volně vázáno na schéma, můžete získat synchronizován. ColumnTag může popisují sloupec, který již neexistuje ve schématu.  Je zapisovače pro synchronizaci sloupce značky a schéma.</td></tr>
<tr><td></td><td>columnName</td><td>Řetězec</td><td>Název sloupce, který odkazuje tato značka.</td></tr>
<tr><td></td><td>Značka</td><td>Řetězec</td><td>Značku popisující sloupce.</td></tr>

<tr><td>Expert ("odborníky")</td><td></td><td></td><td>Tato vlastnost obsahuje uživatele, který je považován za odborník v datové sadě. Bublin opinions(descriptions) se odborníka na začátek UX při výpisu popisy. Každý uživatel může určit vlastní odborníky. Pouze tento uživatel může upravit objekt odborníky. (Správci a Asset vlastníky můžete odstranit odborné objekty, ale nikoli pro úpravy).</td></tr>
<tr><td></td><td>odborné</td><td>SecurityPrincipal</td><td></td></tr>

<tr><td>Verze Preview ("náhledů")</td><td></td><td></td><td>Ve verzi preview obsahuje snímek horních řádků 20 dat pro asset. Náhled smysl jenom pro některé typy prostředků (má smysl pro tabulku, ale ne pro měr).</td></tr>
<tr><td></td><td>verze Preview</td><td>[] – objekt</td><td>Pole objektů, které představují sloupec.  Každý objekt má vlastnost mapování ke sloupci s hodnotou pro tento sloupec pro řádek.</td></tr>

<tr><td>AccessInstruction ("accessInstructions")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>mimeType</td><td>Řetězec</td><td>Typ mime obsahu.</td></tr>
<tr><td></td><td>Obsah</td><td>Řetězec</td><td>Pokyny pro získání přístupu k této datový prostředek. Obsah může být adresa URL, e-mailovou adresu nebo u sady pokynů.</td></tr>

<tr><td>TableDataProfile ("tableDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>numberOfRows</td></td><td>celá čísla</td><td>Počet řádků v datové sadě</td></tr>
<tr><td></td><td>Velikost</td><td>dlouhá</td><td>Velikost v bajtech v datové sadě.  </td></tr>
<tr><td></td><td>schemaModifiedTime</td><td>Řetězec</td><td>Čas poslední schématu byla změněna.</td></tr>
<tr><td></td><td>dataModifiedTime</td><td>Řetězec</td><td>Čas poslední sadu dat byla změněna (data byla přidána ke změně, nebo odstranit)</td></tr>

<tr><td>ColumnsDataProfile ("columnsDataProfiles")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>sloupce</td></td><td>[ColumnDataProfile]</td><td>Pole sloupec dat profily.</td></tr>

<tr><td>ColumnDataClassification ("columnDataClassifications")</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName</td><td>Řetězec</td><td>Název sloupce, který odkazuje tato klasifikace.</td></tr>
<tr><td></td><td>klasifikace</td><td>Řetězec</td><td>Klasifikace dat v tomto sloupci.</td></tr>

<tr><td>Dokumentace k ("dokumentace")</td><td></td><td></td><td>Daný prostředek může mít pouze jeden dokumentaci s ním spojená.</td></tr>
<tr><td></td><td>mimeType</td><td>Řetězec</td><td>Typ mime obsahu.</td></tr>
<tr><td></td><td>Obsah</td><td>Řetězec</td><td>Dokumentace k obsahu.</td></tr>

</table>

### <a name="common-types"></a>Běžné typy
Běžné typy lze použít jako typy vlastností, ale nejsou žádné položky.

<table>
<tr><td><b>Společný typ.</b></td><td><b>Vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>
<tr><td>DataSourceInfo</td><td></td><td></td><td></td></tr>
<tr><td></td><td>SourceType</td><td>Řetězec</td><td>Popisuje typ datového zdroje.  Příklad: SQL Server, databáze Oracle, atd.  </td></tr>
<tr><td></td><td>objectType</td><td>Řetězec</td><td>Popisuje typ objektu v datovém zdroji. Příklad: tabulka, zobrazení pro SQL Server.</td></tr>

<tr><td>DataSourceLocation</td><td></td><td></td><td></td></tr>
<tr><td></td><td>Protokol</td><td>Řetězec</td><td>Povinná hodnota. Popisuje, protokol, který slouží pro komunikaci s datovým zdrojem. Například: "tds" pro SQl Server, "oracle" pro Oracle atd. Odkazovat na [zdroje dat odkaz Specifikace – struktura DSL](data-catalog-dsr.md) seznam aktuálně podporované protokoly.</td></tr>
<tr><td></td><td>Adresa</td><td>Slovník<string, object></td><td>Povinná hodnota. Adresa je sada dat, které jsou specifické pro protokol, který se používá k identifikaci zdroje dat se na ně odkazovat. Data adres obor na konkrétní protokol, znamená ho je smysl bez znalosti protokolu.</td></tr>
<tr><td></td><td>Ověřování</td><td>Řetězec</td><td>Volitelné. Schéma ověřování používaný ke komunikaci se zdrojem dat. Příklad: windows oauth, atd.</td></tr>
<tr><td></td><td>connectionProperties</td><td>Slovník<string, object></td><td>Volitelné. Další informace o tom, jak se připojit ke zdroji dat.</td></tr>

<tr><td>SecurityPrincipal</td><td></td><td></td><td>Back-end neprovede žádné ověření zadané vlastnosti proti AAD během publikování.</td></tr>
<tr><td></td><td>hlavní název uživatele</td><td>Řetězec</td><td>Jedinečnou e-mailovou adresu uživatele. Musíte zadat, pokud není k dispozici objectId nebo v kontextu "lastRegisteredBy" vlastnosti, jinak volitelné.</td></tr>
<tr><td></td><td>objectId</td><td>Identifikátor GUID</td><td>Identity uživatele nebo zabezpečení skupiny AAD. Volitelné. Musí být zadán, pokud hlavní název uživatele není k dispozici, jinak volitelné.</td></tr>
<tr><td></td><td>FirstName</td><td>Řetězec</td><td>Křestní jméno uživatele (pro účely zobrazení). Volitelné. Jediná platná v kontextu vlastnost "lastRegisteredBy". Nelze zadat, pokud objekt zabezpečení poskytuje pro "role", "oprávnění" a "odborníci".</td></tr>
<tr><td></td><td>Příjmení</td><td>Řetězec</td><td>Příjmení uživatele (pro účely zobrazení). Volitelné. Jediná platná v kontextu vlastnost "lastRegisteredBy". Nelze zadat, pokud objekt zabezpečení poskytuje pro "role", "oprávnění" a "odborníci".</td></tr>

<tr><td>Sloupec</td><td></td><td></td><td></td></tr>
<tr><td></td><td>jméno</td><td>Řetězec</td><td>Název sloupce nebo atributu.</td></tr>
<tr><td></td><td>type</td><td>Řetězec</td><td>datový typ sloupce nebo atributu. Povolené typy závisí na sourceType data prostředku.  Je podporován pouze podmnožinu typy.</td></tr>
<tr><td></td><td>Hodnota maxLength</td><td>celá čísla</td><td>Maximální povolená délka sloupce nebo atributu. Odvozené ze zdroje dat. Platí jenom pro některé typy zdrojů.</td></tr>
<tr><td></td><td>přesnost</td><td>Bajtů</td><td>Přesnost sloupce nebo atributu. Odvozené ze zdroje dat. Platí jenom pro některé typy zdrojů.</td></tr>
<tr><td></td><td>Vlastnost isNullable</td><td>Logická hodnota</td><td>Jestli sloupec může mít hodnotu null, nebo ne. Odvozené ze zdroje dat. Platí jenom pro některé typy zdrojů.</td></tr>
<tr><td></td><td>výraz</td><td>Řetězec</td><td>Pokud je hodnota počítaný sloupec, toto pole obsahuje řetězec, který vyjadřuje hodnota. Odvozené ze zdroje dat. Platí jenom pro některé typy zdrojů.</td></tr>

<tr><td>ColumnDataProfile</td><td></td><td></td><td></td></tr>
<tr><td></td><td>columnName </td><td>Řetězec</td><td>Název sloupce</td></tr>
<tr><td></td><td>type </td><td>Řetězec</td><td>Typ sloupce</td></tr>
<tr><td></td><td>min </td><td>Řetězec</td><td>Minimální hodnota v datové sadě</td></tr>
<tr><td></td><td>maximální počet </td><td>Řetězec</td><td>Maximální hodnota v datové sadě</td></tr>
<tr><td></td><td>průměr </td><td>Double</td><td>Průměrná hodnota v datové sadě</td></tr>
<tr><td></td><td>StDev </td><td>Double</td><td>Standardní odchylce pro sadu dat</td></tr>
<tr><td></td><td>nullCount </td><td>celá čísla</td><td>Počet hodnot null v datové sadě</td></tr>
<tr><td></td><td>distinctCount  </td><td>celá čísla</td><td>Počet jedinečných hodnot v datové sadě</td></tr>


</table>

## <a name="asset-identity"></a>Asset identity
Azure Data Catalog používá ke generování identity prostředku, který se používá k řešení v katalogu asset (protocol) a identity vlastnosti z kontejneru objektů "address" vlastnosti DataSourceLocation "dsl".
Například protokol "tds" má vlastnosti identity "server", "databáze", "schématu" a "objekt". Kombinace protokolu a vlastnosti identity se používají ke generování identitu Asset tabulky serveru SQL.
Azure Data Catalog poskytuje několik předdefinovaných datového zdroje protokoly, které jsou uvedeny v [zdroje dat odkaz Specifikace – struktura DSL](data-catalog-dsr.md).
Sadu podporovaných protokolů se dá rozšířit prostřednictvím kódu programu (viz. referenční dokumentace rozhraní API REST katalogu Data). Správci katalogu lze zaregistrovat zdroj protokoly vlastní data. Následující tabulka popisuje vlastnosti potřebné k registraci vlastního protokolu.

### <a name="custom-data-source-protocol-specification"></a>Specifikace protokolu zdroj vlastních dat
<table>
<tr><td><b>Typ</b></td><td><b>Vlastnosti</b></td><td><b>Datový typ</b></td><td><b>Komentáře</b></td></tr>

<tr><td>DataSourceProtocol</td><td></td><td></td><td></td></tr>
<tr><td></td><td>obor názvů</td><td>Řetězec</td><td>Obor názvů protokolu. Namespace musí mít 1 až 255 znaků, obsahovat jeden nebo více neprázdný části oddělené tečkou (.). Každou část musí být od 1 do 255 znaků, začínat písmenem a obsahovat pouze písmena a číslice.</td></tr>
<tr><td></td><td>jméno</td><td>Řetězec</td><td>Název protokolu. Název musí mít délku 1 až 255 znaků, začínat písmenem a obsahovat jenom písmena, číslice a znak pomlčka (-).</td></tr>
<tr><td></td><td>identityProperties</td><td>[DataSourceProtocolIdentityProperty]</td><td>Seznam vlastností identity, musí obsahovat alespoň jeden, ale žádné víc než 20 vlastnosti. Například: "server", "databáze", "schéma", "objekt" jsou vlastnosti identity protokol "tds".</td></tr>
<tr><td></td><td>identitySets</td><td>[DataSourceProtocolIdentitySet]</td><td>Seznam sad identity. Definuje sadu vlastnosti identity, které reprezentuje identitu platný asset. Musí obsahovat alespoň jeden, ale žádné víc než 20 sad. Příklad: {"server", "databáze", "schématu" a "objektu"} je identita pro protokol "tds", který definuje identitu asset tabulky serveru Sql Server.</td></tr>

<tr><td>DataSourceProtocolIdentityProperty</td><td></td><td></td><td></td></tr>
<tr><td></td><td>jméno</td><td>Řetězec</td><td>Název vlastnosti Název musí být od 1 do 100 znaků, začněte s písmenem a může obsahovat jenom písmena a číslice.</td></tr>
<tr><td></td><td>type</td><td>Řetězec</td><td>Typ vlastnosti. Podporované hodnoty: "bool", logická hodnota ","bajtů","guid","int","číslo","dlouho","string","url"</td></tr>
<tr><td></td><td>IgnoreCase –</td><td>BOOL</td><td>Určuje, zda by měl při použití vlastnosti na hodnotu ignoruje velikost písmen. Můžete zadat pouze pro vlastnosti typu "řetězec". Výchozí hodnota je false.</td></tr>
<tr><td></td><td>urlPathSegmentsIgnoreCase</td><td>[] – operátor BOOL</td><td>Určuje, zda by měl být pro každý segment cesty adresu url ignoruje velikost písmen. Můžete zadat pouze pro vlastnosti typu "url". Výchozí hodnota je [false].</td></tr>

<tr><td>DataSourceProtocolIdentitySet</td><td></td><td></td><td></td></tr>
<tr><td></td><td>jméno</td><td>Řetězec</td><td>Název sady identity.</td></tr>
<tr><td></td><td>properties</td><td>řetězec]</td><td>Nastavit vlastnosti identity zahrnut do tuto identitu v seznamu. Nesmí obsahovat duplikáty. Každou vlastnost odkazuje sadu identity musí být definovány v seznamu "identityProperties" protokolu.</td></tr>

</table>

## <a name="roles-and-authorization"></a>Role a autorizace
Microsoft Azure Data Catalog poskytuje možnosti autorizace pro operace CRUD na prostředky a poznámky.

## <a name="key-concepts"></a>Klíčové koncepty
Azure Data Catalog používá dva mechanismy ověřování:

* Ověřování na základě rolí
* Ověření na základě oprávnění

### <a name="roles"></a>Role
Existují tři role: **správce**, **vlastníka**, a **Přispěvatel**.  Každá role má svou oboru a oprávnění, které jsou shrnuty v následující tabulce.

<table><tr><td><b>Role</b></td><td><b>Rozsah</b></td><td><b>Práva</b></td></tr><tr><td>Správce</td><td>Katalog (všechny prostředky nebo poznámky v katalogu)</td><td>Odstranění ViewRoles pro čtení

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Vlastník</td><td>Každý prostředek (kořenovou položku)</td><td>Odstranění ViewRoles pro čtení

ChangeOwnership ChangeVisibility ViewPermissions</td></tr><tr><td>Přispěvatel</td><td>Každý jednotlivý prostředek a poznámky</td><td>Čtení aktualizace odstranění ViewRoles Poznámka: všechna práva byly odvolány, pokud je pro čtení na položce vpravo odvolává Přispěvatel</td></tr></table>

> [!NOTE]
> **Čtení**, **aktualizace**, **odstranit**, **ViewRoles** práva platí pro všechny položky (asset nebo poznámky) při **vlastnictví**, **ChangeOwnership**, **ChangeVisibility**, **ViewPermissions** platí jenom pro kořenové asset.
> 
> **Odstranit** vpravo se vztahuje na položku a všech podřízených položek či jednu položku pod ním. Například odstraněním prostředek odstraníte také žádné poznámky pro tento prostředek.
> 
> 

### <a name="permissions"></a>Oprávnění
Oprávnění je jako seznam položek řízení přístupu. Každé položce řízení přístupu přiřadí sadu práv objekt zabezpečení. Oprávnění lze zadat pouze na prostředek (tedy kořenovou položku) a použít na asset a všech podřízených položek.

Během **Azure Data Catalog** náhled pouze **čtení** vpravo se podporuje v seznamu oprávnění povolit scénáře omezení viditelnosti majetku.

Ve výchozím nastavení má každý ověřený uživatel **čtení** pravým pro libovolnou položku v katalogu, pokud viditelnost je omezen na sady objektů zabezpečení, oprávnění.

## <a name="rest-api"></a>REST API
**UVEĎTE** a **POST** položky zobrazení požadavky, můžete použít k řízení rolí a oprávnění: kromě datové položky, můžete být zadány dvě vlastnosti systému **role** a **oprávnění**.

> [!NOTE]
> **oprávnění** platí jenom pro kořenovou položku.
> 
> **Vlastník** role platí jenom pro kořenovou položku.
> 
> Ve výchozím nastavení při vytvoření nové položky v katalogu jeho **Přispěvatel** je nastaven na aktuálně ověřeného uživatele. Pokud má být položka aktualizovat kdokoli, **Přispěvatel** musí být nastavena na &lt;Everyone&gt; speciální objektu v zabezpečení **role** vlastnost po první položka publikovaných (viz následující příklad). **Přispěvatel** nelze změnit a zůstane stejný během doby životnosti položky (i **správce** nebo **vlastníka** nemá práva k změnit **Přispěvatel**). Jediná hodnota podporovaná pro explicitní nastavení **Přispěvatel** je &lt;Everyone&gt;: **Přispěvatel** lze pouze uživatele, který vytvořil položku nebo &lt;Everyone&gt;.
> 
> 

### <a name="examples"></a>Příklady
**Nastavte Přispěvatel na &lt;Everyone&gt; při publikování položky.**
Objekt zabezpečení speciální &lt;Everyone&gt; má objectId "00000000-0000-0000-0000-000000000201".
  **POST** https://api.azuredatacatalog.com/catalogs/default/views/tables/?api-version=2016-03-30

> [!NOTE]
> Některých implementacích klienta HTTP může automaticky znovu vystavit požadavků v reakci 302 ze serveru, ale obvykle pruhu hlavičky ověření z požadavku. Vzhledem k tomu, že hlavičku autorizace je potřeba provést žádosti o připojení k Azure Data Catalog, je nutné zajistit, že hlavičku autorizace stále získáte při opětovné vystavení požadavek na přesměrování umístění určeného Azure Data Catalog. Následující vzorový kód ukazuje pomocí objekt .NET HttpWebRequest.
> 
> 

**Text**

    {
        "roles": [
            {
                "role": "Contributor",
                "members": [
                    {
                        "objectId": "00000000-0000-0000-0000-000000000201"
                    }
                ]
            }
        ]
    }

  **Přiřadit vlastníků a omezení viditelnosti pro existující kořenovou položku**: **PUT** https://api.azuredatacatalog.com/catalogs/default/views/tables/042297b0...1be45ecd462a?api-version=2016-03-30

    {
        "roles": [
            {
                "role": "Owner",
                "members": [
                    {
                        "objectId": "c4159539-846a-45af-bdfb-58efd3772b43",
                        "upn": "user1@contoso.com"
                    },
                    {
                        "objectId": "fdabd95b-7c56-47d6-a6ba-a7c5f264533f",
                        "upn": "user2@contoso.com"
                    }
                ]
            }
        ],
        "permissions": [
            {
                "principal": {
                    "objectId": "27b9a0eb-bb71-4297-9f1f-c462dab7192a",
                    "upn": "user3@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            },
            {
                "principal": {
                    "objectId": "4c8bc8ce-225c-4fcf-b09a-047030baab31",
                    "upn": "user4@contoso.com"
                },
                "rights": [
                    {
                        "right": "Read"
                    }
                ]
            }
        ]
    }

> [!NOTE]
> V PUT není potřeba zadat datovou položku v těle: PUT lze aktualizovat pouze role nebo oprávnění.
> 
> 

<!--Image references-->
[1]: ./media/data-catalog-developer-concepts/concept2.png
