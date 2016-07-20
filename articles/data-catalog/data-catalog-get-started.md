<properties
   pageTitle="Azure Data Catalog – začínáme s katalogem Data Catalog | Microsoft Azure"
   description="Koncový kurz představující scénáře a možnosti služby Azure Data Catalog."
   documentationCenter=""
   services="data-catalog"
   authors="steelanddata"
   manager=""
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="05/06/2016"
   ms.author="maroche"/>

# Začínáme s Azure Data Catalog

Tento článek je koncový přehled scénářů a možností služby **Azure Data Catalog**. Po přihlášení ke službě vytvořte podle těchto kroků katalog Data Catalog a registrujte, přidávejte poznámky a objevte zdroje dat.

## Požadavky kurzu

Než zahájíte tento kurz, musíte mít následující:

- **Předplatné Azure** – pokud nemáte předplatné, můžete si během několika minut vytvořit bezplatný zkušební účet. Podrobnosti viz [bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/).
- **Azure Active Directory** – Azure Data Catalog používá [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) pro správu identit a přístupu.
- **Zdroje dat** – Azure Data Catalog umožňuje zjišťovat zdroje dat. Tento návod používá ukázkovou databázi společnosti Adventure Works, ale vy můžete použít libovolný podporovaný zdroj dat, pokud si přejete pracovat s daty, která jsou známá a relevantní pro vaši roli. Seznam podporovaných zdrojů dat naleznete v tématu [Podporované zdroje dat](data-catalog-dsr.md).

> [AZURE.NOTE] Další informace o předplatných Azure a Azure Active Directory naleznete v tématu [Požadavky služby Azure Data Catalog](data-catalog-prerequisites.md).

Začněme instalaci ukázkové databáze společnosti Adventure Works.

## Cvičení 1: instalace ukázkové databáze Adventure Works

V tomto cvičení nainstalujete vzorek společnosti Adventure Works pro modul databáze serveru SQL Server, jenž je používána ve cvičeních, která následují.

### Instalace databáze Adventure Works 2014 OLTP

Databáze společnosti Adventure Works podporuje scénáře standardního online zpracování transakcí pro smyšleného výrobce jízdních kol (Adventure Works Cycles), což zahrnuje produkty, prodej a nákup. V tomto kurzu zaregistrujete informace o produktech do služby **Azure Data Catalog**.

Zde je postup instalace ukázkové databáze společnosti Adventure Works.

K instalaci ukázkové databáze společnosti Adventure Works můžete obnovit zálohu AdventureWorks2014 umístěnou v souboru [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) na webu CodePlex. Jedním ze způsobů obnovení databáze je spuštění skriptu T-SQL v aplikaci SQL Server Management Studio.

**Instalace ukázkové databáze společnosti Adventure Works pomocí skriptu T-SQL**

1.  Vytvořte složku s názvem C:\DataCatalog_GetStarted. Pokud použijete jiný název složky, nezapomeňte změnit cestu v níže uvedeném skriptu T-SQL.
2.  Stáhněte soubor [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661).
3.  Rozbalte soubor Adventure Works 2014 Full Database Backup.zip do složky C:\DataCatalog_GetStarted. Níže uvedený skript předpokládá, že záložní soubor nachází ve složce C:\DataCatalog_GetStarted\Adventure Works 2014 Full Database Backup\AdventureWorks2014.bak.
4.  V aplikaci **SQL Server Management Studio** klikněte na panelu nástrojů **Standardní** na tlačítko **Nový dotaz**.
5.  V okně dotazu spusťte následující kód T-SQL.

**Spuštěním tohoto skriptu nainstalujte databázi společnosti Adventure Works 2014**

    USE [master]
    GO
    -- NOTE: This script is for sample purposes only. The default backup file path for this script is C:\DataCatalog_GetStarted. To run this script, create the default file path or change the file path, and copy AdventureWorks2014.bak into the path.

    -- IMPORTANT: In a production application, restore a SQL database to the data folder for your SQL Server instance.

    RESTORE DATABASE AdventureWorks2014
        -- AdventureWorks2014.bak file location
        FROM disk = 'C:\DataCatalog_GetStarted\Adventure Works 2014 Full Database Backup\AdventureWorks2014.bak'

        -- AdventureWorks2014.mdf database location
        WITH MOVE 'AdventureWorks2014_data' TO 'C:\DataCatalog_GetStarted\AdventureWorks2014.mdf',

        -- AdventureWorks2014.ldf log location
        MOVE 'AdventureWorks2014_Log' TO 'C:\DataCatalog_GetStarted\AdventureWorks2014.ldf'
    ,REPLACE
    GO

Jako alternativu ke spuštění skriptu T-SQL můžete obnovit databázi pomocí aplikace SQL Server Management Studio. Viz [Obnovení zálohy databáze (SQL Server Management Studio)](http://msdn.microsoft.com/library/ms177429.aspx).

V tomto cvičení jste nainstalovali ukázkovou databázi společnosti Adventure Works, která se používá ve zbývajících cvičeních. V dalším cvičení zjistíte, jak zaregistrovat zdroje dat služby **Azure Data Catalog** z tabulek v ukázkové databázi společnosti Adventure Works.

## Cvičení 2: registrace zdrojů dat

V tomto cvičení použijete registrační nástroj **Azure Data Catalog** k registraci zdrojů dat společnosti Adventure Works pomocí katalogu. Registrace je proces extrahování klíčových strukturálních metadat – například názvy, typy a umístění – ze zdroje dat, a prostředků, které obsahuje, a zkopírování těchto metadat do katalogu. Zdroje dat a jejich data zůstanou, kde jsou, ale metadata jsou používána katalogem, aby byly snadněji zjistitelné a srozumitelnější.

### Zde je postup registrace zdroje dat

1.  Přejděte na web https://azure.microsoft.com/services/data-catalog a klikněte na tlačítko **Začínáme**.
2.  Přihlaste se do portálu **Azure Data Catalog** a klikněte na tlačítko **Publikovat data**.

    ![](media/data-catalog-get-started/data-catalog-publish-data.png)

3.  Klikněte na tlačítko **Spustit aplikaci**.

    ![](media/data-catalog-get-started/data-catalog-launch-application.png)

4. Na stránce **Vítejte** klikněte na tlačítko **Přihlášení** a zadejte svá pověření.
5. Na stránce **Microsoft Azure Data Catalog** dvakrát klikněte na tlačítko **SQL Server** nebo klikněte na tlačítka **SQL Server** a **Další**.

    ![](media/data-catalog-get-started/data-catalog-data-sources.png)

6.  Zadejte vlastnosti připojení serveru SQL Server pro databázi AdventureWorks2014 (viz následující příklad) a klikněte na tlačítko **PŘIPOJIT**.

    ![](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

7.  Další stránka je stránkou, ve které registrujete metadata zdroje dat. V tomto příkladu zaregistrujete objekty **Výroba/Produkt** z oboru názvů výroba společnosti AdventureWorks. Způsob provedení:

    a. Ve stromové struktuře **hierarchie serveru** klikněte na tlačítko **Výroba**.

    ![](media/data-catalog-get-started/data-catalog-server-hierarchy.png)

    b. Stiskněte klávesu Ctrl+klikněte na tlačítko Výrobek, KategorieVýrobku, PopisVýrobku a FotografieVýrobku.

    c. Kliknutím posuňte vybranou šipku (**>**). Tím se posunou všechny vybrané objekty produktu do seznamu **Objekty k registraci**.

    ![](media/data-catalog-get-started/data-catalog-available-objects.png)

    d. Do pole **Přidat značky** zadejte popis, fotografii. Tím se přidají vyhledávací značky pro tyto datové prostředky. Značky jsou skvělý způsob, jak pomoci uživatelům najít registrovaný zdroj dat.

    ![](media/data-catalog-get-started/data-catalog-objects-register.png)

    e.  **Volitelně**: můžete **Zahrnout náhled** a **Přidat experta na zdroj dat**.

    f.  Klikněte na tlačítko **ZAREGISTROVAT**. **Azure Data Catalog** zaregistruje vaše vybrané objekty. V tomto cvičení jsou zaregistrovány vybrané objekty z podniku Adventure Works.

    g.  Registrované objekty zdrojů dat zobrazíte kliknutím na tlačítko **Zobrazit portál**. V portálu **Azure Data Catalog** můžete zobrazit objekty zdroje dat v **Dlaždicích** nebo **Seznamu**.

    ![](media/data-catalog-get-started/data-catalog-view-portal.png)

V tomto cvičení jste zaregistrovali objekty z ukázkové databáze společnosti Adventure Works, aby je uživatelé v organizaci mohli snadno zjistit.
V dalším cvičení se dozvíte, jak zjistit registrované datové prostředky.

## Cvičení 3: zjišťování registrovaných datových prostředků

V tomto cvičení použijete portál **Azure Data Catalog** ke zjištění registrovaných datových prostředků a zobrazení jejich metadat. **Azure Data Catalog** poskytuje několik nástrojů pro zjišťování datových prostředků, včetně jednoduchého vyhledávání klíčového slova, interaktivních filtrů a rozšířené syntaxe vyhledávání pro členy skupiny "Power Users".

### Zde je způsob, jak zjišťovat registrované datové prostředky

**Azure Data Catalog** má jednoduchou, ale výkonnou syntaxi vyhledávání, jenž umožňuje snadno vytvářet dotazy vracející data, která uživatelé potřebují. Podrobnosti o vyhledávání v portálu **Azure Data Catalog** naleznete v tématu [Reference syntaxe vyhledávání v katalogu Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx).

**Azure Data Catalog** obsahuje následující možnosti vyhledávání:

- Hledání klíčových slov
- Filtr
- Rozšířené vyhledávání

Je také možné upřesnit, které datové prostředky se mají zobrazit. **Azure Data Catalog** obsahuje následující možnosti zobrazení:

- Zobrazení vlastností
- Zobrazení sloupců
- Zobrazení náhledu

Pro tento příklad použijete vyhledání klíčového slova. Vyhledávání v portálu **Azure Data Catalog** obsahuje několik technik dotazu. Tento příklad použije vyhledávací dotaz **Seskupení**.

**Techniky dotazu**

|Technika|Použití|Příklad
|---|---|---
|Zkoumání vlastnosti|Vrátí pouze zdroje dat, kde se hledaný termín shoduje v zadané vlastnosti|název: produkt
|Logické operátory|Rozšíří nebo zúží vyhledávání pomocí logických operací, jak je popsáno v části Logické operátory na této stránce|finance NOT podnikové
|Seskupování pomocí závorek|Závorky lze použít k seskupení části dotazů k dosažení logické izolace, zejména ve spojení s logickými operátory|název:produkt AND (značky:obrázek OR značky:fotografie)
|Operátory porovnání|Porovnávání jiné než rovnost použijte pro vlastnosti, které mají číselné a číselné datové typy dat|Čas vytvoření:>11/05/14

V tomto příkladu proveďte vyhledání datových prostředků pomocí **Seskupování**, kde se název rovná produktu a značky se rovnají obrázku nebo se značky rovnají fotografii.

1. Přejděte na stránku https://azure.microsoft.com/services/data-catalog, klikněte na tlačítko **Začínáme** a přihlaste se do portálu **Azure Data Catalog**.
2. Do pole **Prohledat Data Catalog** zadejte dotaz **Seskupení**: (**značky:popis OR značky:fotografie**).
3. Klikněte na ikonu hledání nebo stiskněte klávesu Enter. **Azure Data Catalog** zobrazí datové prostředky pro tento vyhledávací dotaz.

    ![](media/data-catalog-get-started/data-catalog-search-box.png)

V tomto cvičení byl portál **Azure Data Catalog** použit ke zjištění a zobrazení datových prostředků společnosti Adventure Works zaregistrovaných v katalogu.

<a name="annotating"/>
## Cvičení 4: zadávání poznámek k zaregistrovaným zdrojům dat

V tomto cvičení použijete portál **Azure Data Catalog** k přidání poznámek k datových prostředkům, které byly dříve zaregistrovány v katalogu. Poznámky, které poskytnete, doplní a vylepší strukturální metadata extrahovaná ze zdroje dat během registrace a zjednoduší zjišťování a pochopení datových prostředků. Protože každý uživatel služby **Data Catalog** může poskytnout své vlastní poznámky, je snadné pro každého uživatele s perspektivou na data je sdílet.

### Zde je návod, jak přidávat poznámky k datovým prostředkům

1. Přejděte na stránku https://azure.microsoft.com/services/data-catalog, klikněte na tlačítko **Začínáme** a přihlaste se do portálu **Azure Data Catalog**.
2. Klikněte na tlačítko **Zjistit**.
3. Vyberte jeden nebo více **datových prostředků**. V tomto příkladu vyberte **FotografieVýrobku** a zadejte poznámku "Fotografie výrobku pro marketingové materiály."
4. Zadejte **Popis**, který pomůže ostatním zjišťovat a pochopit důvod, proč a jak používat vybrané datové prostředky. Zadejte například "Obrázky produktů". Můžete také přidat další značky a zobrazit sloupce.
5. Nyní můžete zkusit vyhledávání a filtrování, abyste zjistili datové prostředky pomocí popisných metadat, která jste přidali do katalogu.

    ![](media/data-catalog-get-started/data-catalog-annotate.png)

V tomto cvičení jste přidali popisné informace k registrovaným datovým prostředkům, aby uživatelé katalogu mohli zjistit zdroje dat pomocí termínů, kterým rozumí.

> [AZURE.NOTE] Standardní edice katalogu Data Catalog zahrnuje obchodní glosář, který umožňuje správcům katalogu definovat centrální obchodní taxonomii. Uživatelé katalogu mohou poté opatřit poznámkami datové prostředky pomocí termínů v glosáři. Další informace naleznete v článku [Jak nastavit obchodní glosář řízeným přidáváním značek](data-catalog-how-to-business-glossary.md)  

## Cvičení 5: crowdsourcingová metadata

V tomto cvičení pracujete s jiným uživatelem, abyste k datovým prostředkům v katalogu přidali metadata. Crowdsourcovaný přístup portálu **Azure Data Catalog** k poznámkám umožňuje každému uživateli přidat značky, popisy a další metadata, aby uživatel s perspektivou na datový prostředek a jeho použití mohl mít tuto perspektivu zachycenu a k dispozici pro jiné uživatele.

> [AZURE.NOTE] Pokud nemáte jiného uživatele, se kterým můžete v tomto cvičení pracovat, nemusíte si dělat starosti! Každý uživatel, který přistupuje k katalogu dat, může přidat vlastní perspektivu, pokud se tak rozhodne. Tento crowdsourcingový přístup k metadatům umožňuje narůstání obsahu katalogu a bohatost metadat katalogu v čase.

### Následuje vysvětlení, jak lze crowdsourcovat metadata o datových prostředcích

Požádejte kolegu o zopakování výše uvedeného cvičení [Zadávání poznámek k zaregistrovaným zdrojům dat](#annotating). Když vaši kolegové přidají popisy k datovému prostředku, jako je například FotografieVýrobku, uvidíte více poznámek.

![](media/data-catalog-get-started/data-catalog-crowdsource.png)

V tomto cvičení jste prozkoumali funkce služby **Azure Data Catalog** pro crowdsourcovaná metadata, kde může každý uživatel katalogu opatřit poznámkami datové prostředky, které zjistí.

## Cvičení 6: připojení ke zdrojům dat

V tomto cvičení použijete portál **Azure Data Catalog** k připojení ke zdroji dat pomocí aplikace Microsoft Excel.

> [AZURE.NOTE] Je důležité nezapomenout, že **Azure Data Catalog** nedává uživatelům přístup ke skutečnému zdroji dat – pouze uživatelům usnadňuje data vyhledat a pochopit. Když se uživatelé připojí ke zdroji dat, klientská aplikace, kterou si vyberou, použije jejich přihlašovacích údaje systému Windows nebo je podle potřeby vyzve k zadání přihlašovacích údajů. Pokud uživateli nebyl dříve udělen přístup ke zdroji dat, bude mu muset být přístup udělen, než se bude moci připojit.

### Zde je postup připojení ke zdroji dat z aplikace Excel

1. Přejděte na stránku https://azure.microsoft.com/services/data-catalog, klikněte na tlačítko **Začínáme** a přihlaste se do portálu **Azure Data Catalog**.
2. Klikněte na tlačítko **Zjistit**.
3. Vyberte datový prostředek. V tomto příkladu vyberte ProductCategory.
4. Zvolte možnost **Otevřít v aplikaci** > **Excel**.

    ![](media/data-catalog-get-started/data-catalog-connect1.png)

5. V okně **Oznámení o zabezpečení aplikace Microsoft Excel** klikněte na tlačítko **Povolit**.
6. Otevřete soubor **ProductCategory.odc**.
7. Zdroj dat se otevře v aplikaci Excel.

    ![](media/data-catalog-get-started/data-catalog-connect2.png)

V tomto cvičení jste se připojili ke zdrojům dat zjištěným pomocí portálu **Azure Data Catalog**. Portál **Azure Data Catalog** umožňuje uživatelům připojit se přímo pomocí klientských aplikací integrovaných do jeho nabídky **Otevřít v aplikaci...** a umožňuje uživatelům připojit se pomocí jakékoli aplikace, kterou si vyberou, pomocí informací o umístění připojení zahrnutých v metadatech prostředku.

## Cvičení 7: odebrání metadat zdroje dat

V tomto cvičení použijete portál **Azure Data Catalog** k odebrání náhledu dat z registrovaných datových prostředků a k odstranění datových prostředků z katalogu.

> [AZURE.NOTE] Výchozí chování katalogu je umožnit každému uživateli zaregistrovat libovolný zdroj dat a umožnit každému uživateli odstranit libovolný zaregistrovaný datový prostředek. Možnosti správy, které jsou součástí portálu **Azure Data Catalog, Standard Edition** poskytují další možnosti pro převzetí vlastnictví prostředků, omezení, kdo může zjišťovat prostředky, a omezení, kdo může odstranit prostředky.

V **Azure Data Catalog** je možné odstranit jednotlivý prostředek nebo odstranit více prostředků.

### Zde je uveden postup odstranění více datových prostředků

1. Přejděte na stránku https://azure.microsoft.com/services/data-catalog, klikněte na tlačítko **Začínáme** a přihlaste se do portálu **Azure Data Catalog**.
2. Klikněte na tlačítko **Zjistit**.
3. Vyberte jeden nebo více datových prostředků.
4. Klikněte na tlačítko **Odstranit**.

V tomto cvičení jste odebrali registrované datové prostředky z katalogu.

## Cvičení 8: správa zaregistrovaných zdrojů dat

V tomto cvičení použijete možnosti správy služby **Azure Data Catalog** k převzetí vlastnictví datových prostředků a k řízení, co smí uživatelé zjistit a jak uživatelé spravují tyto prostředky.

> [AZURE.NOTE] Možnosti správy popsané v tomto cvičení jsou k dispozici pouze v **Azure Data Catalog, Standard Edition** a nikoli v bezplatné **Free Edition**.
Ve službě **Azure Data Catalog** může převzít vlastnictví datových prostředků, přidat spoluvlastníky k datovým prostředkům a nastavit viditelnost datových prostředků.

### Zde je uveden postup převzetí vlastnictví datových prostředků a omezení viditelnosti

1. Přejděte na stránku https://azure.microsoft.com/services/data-catalog, klikněte na tlačítko **Začínáme** a přihlaste se do portálu **Azure Data Catalog**.
2. Klikněte na tlačítko **Zjistit**.
3. Vyberte jeden nebo více datových prostředků.
4. V panelu **Vlastnosti**, oddílu **Správa**, klikněte na tlačítko **Převzít vlastnictví**.
5. Chcete-li omezit viditelnost, klikněte na tlačítko **Vlastníci a tito uživatelé**.

    ![](media/data-catalog-get-started/data-catalog-ownership.png)

V tomto cvičení jste prozkoumali možnosti správy služby **Azure Data Catalog** a omezili viditelnost na vybraných datových prostředcích.

## Souhrn

V tomto kurzu jste prozkoumali základní možnosti služby **Azure Data Catalog**, včetně registrace, zadávání poznámek, zjišťování a správy datových zdrojů organizace. Nyní, když jste dokončili kurz, je čas začít. Začít můžete ještě dnes registrací zdrojů dat, na která spoléháte vy a váš tým, a pozváním kolegů k používání katalogu.



<!--HONumber=Jun16_HO2-->


