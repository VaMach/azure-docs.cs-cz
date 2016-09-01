<properties
    pageTitle="Začínáme spuštěním průvodce povolením funkce Stretch pro databázi | Microsoft Azure"
    description="Naučte se nakonfigurovat databázi pro funkci Stretch Database spuštěním průvodce povolením funkce Stretch pro databázi."
    services="sql-server-stretch-database"
    documentationCenter=""
    authors="douglaslMS"
    manager=""
    editor=""/>

<tags
    ms.service="sql-server-stretch-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/05/2016"
    ms.author="douglasl"/>

# Začínáme spuštěním průvodce povolením funkce Stretch pro databázi

Pokud chcete nakonfigurovat databázi pro funkci Stretch Database, spusťte průvodce povolením funkce Stretch pro databázi.  Toto téma popisuje informace, které je nutné zadat, a volby, které je nutné v průvodci využít.

Další informace o funkci Stretch Database najdete v tématu [Stretch Database](sql-server-stretch-database-overview.md).

 >   [AZURE.NOTE] Pokud později zakážete funkci Stretch Database, mějte na paměti, že zákazem funkce Stretch Database pro tabulku nebo databázi nedojde k odstranění vzdáleného objektu. Pokud chcete odstranit vzdálenou tabulku nebo vzdálenou databázi, musíte ji vyřadit pomocí portálu správy Azure. Vzdálené objekty i nadále navyšují cenu provozu aplikace Azure, dokud je ručně neodstraníte. 

## Spuštění průvodce

1.  V aplikaci SQL Server Management Studio vyberte v Průzkumníku objektů databázi, pro kterou chcete povolit funkci Stretch.

2.  \-Klikněte pravým tlačítkem a vyberte**Úlohy**, vyberte **Stretch** a potom výběrem **Povolit** spusťte průvodce.

## <a name="Intro"></a>Úvod
Prohlédněte účel tohoto průvodce a příslušné předpoklady.

Důležité požadavky zahrnují následující:

-   Pokud chcete změnit nastavení databáze, musíte mít oprávnění správce.
-   Musíte mít předplatné Microsoft Azure.
-   SQL Server musí být schopen komunikovat se vzdáleným serverem Azure.

![Úvodní stránka průvodce Stretch Database][StretchWizardImage1]

## <a name="Tables"></a>Výběr tabulek
Vyberte tabulky, které chcete povolit pro funkci Stretch.

Tabulky s velkým množstvím řádků se zobrazují v horní části seřazeného seznamu. Předtím, než Průvodce zobrazí seznam tabulek, analyzuje je z hlediska datových typů, které nejsou aktuálně podporovány funkcí Stretch Database.

![Stránka výběru tabulek průvodce Stretch Database][StretchWizardImage2]

|Sloupec|Popis|
|----------|---------------|
|(bez názvu)|Zaškrtnutím políčka v tomto sloupci povolíte funkci Stretch pro vybranou tabulku.|
|**Name (Název)**|Určuje název sloupce v tabulce.|
|(bez názvu)|Symbol v tomto sloupci může představovat upozornění, které vám nebrání v povolení vybrané tabulky pro funkci Stretch\'. Může také představovat problém blokování, který vám brání v povolení vybrané tabulky pro funkci Stretch \-, protože tabulka používá nepodporovaný datový typ. Pokud na symbol najedete myší, zobrazí se další informace ve formě popisu tlačítka. Další informace najdete v části [Omezení pro funkci Stretch Database](sql-server-stretch-database-limitations.md).|
|**Roztažení**|Udává, jestli je příslušná tabulka už povolená pro funkci Stretch.|
|**Migrace**|Můžete migrovat celou tabulku (**Celá tabulka**) nebo zadat filtr pro existující sloupec v tabulce. Pokud chcete pro výběr řádků k migraci využít jinou funkci filtru, po ukončení tohoto průvodce spusťte příkaz ALTER TABLE a zadejte požadovanou funkci filtru. Další informace o funkci filtru najdete v části [Vyberte řádky k migraci pomocí funkce filtru](sql-server-stretch-database-predicate-function.md). Další informace o použití funkce najdete v tématu [Povolení funkce Stretch Database pro tabulku](sql-server-stretch-database-enable-table.md) nebo [ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx).|
|**Řádky**|Určuje počet řádků v tabulce.|
|**Velikost (kB)**|Určuje velikost tabulky v kB.|

## <a name="Filter"></a>Volitelně poskytuje filtr řádků

Pokud chcete pomocí funkce filtru vybrat řádky k migraci, proveďte na stránce **Vybrat tabulky** následující akce.

1.  V seznamu **Select the tables you want to stretch** (Vyberte tabulky, které chcete roztáhnout) klikněte v řádku pro příslušnou tabulku na **Entire Table** (Celá tabulka). Otevře se dialogové okno **Select rows to stretch** (Vybrat řádky k roztažení).

    ![Definice funkce filtru][StretchWizardImage2a]

2.  V dialogovém okně **Select rows to stretch** (Vybrat řádky k roztažení) vyberte **Choose Rows** (Zvolit řádky).

3.  V **Poli pro název** zadejte název funkce filtru.

4.  Pro klauzuli **Where** vyberte v tabulce sloupec, vyberte operátor a potom zadejte hodnotu.

5. Kliknutím na **Check** (Zkontrolovat) funkci otestujte. Pokud funkce vrátí výsledky z tabulky (to znamená, pokud v tabulce jsou řádky k migraci, které splňují zadanou podmínku), test vrátí **Úspěch**.

    >   [AZURE.NOTE] Textové pole, které zobrazí dotaz filtru, je jen pro čtení. Dotaz v textovém poli se nedá upravit.

6.  Kliknutím na Done (Hotovo) se vrátíte na stránku **Select tables** (Vybrat tabulky).

Funkce filtru se vytvoří v systému SQL Server jenom v případě, že jste dokončili průvodce. Do té doby se můžete vrátit na stránku **Výběr tabulek** a změnit nebo přejmenovat funkci filtru.

![Stránka pro výběr tabulek po definování funkce filtru][StretchWizardImage2b]

Pokud chcete použít jiný typ funkce filtru pro výběr řádků k migraci, proveďte jednu z následujících akcí.  

-   Ukončete průvodce a spusťte příkaz ALTER TABLE k povolení funkce Stretch pro tabulku a k určení funkce filtru. Další informace najdete v části [Povolit funkci Stretch Database pro tabulku](sql-server-stretch-database-enable-table.md).  

-   Spusťte příkaz ALTER TABLE k určení funkce filtru po ukončení průvodce. Požadovaný postup najdete v části [Přidat funkci filtru po spuštění průvodce](sql-server-stretch-database-predicate-function.md#addafterwiz).

## <a name="Configure"></a>Konfigurace nasazení Azure

1.  Přihlaste se k Microsoft Azure pomocí účtu Microsoft.

    ![Přihlášení k Azure – průvodce Stretch Database][StretchWizardImage3]

2.  Vyberte existující předplatné Azure, které chcete použít pro funkci Stretch Database.

3.  Vyberte oblast Azure.
    -   Pokud vytvoříte nový server, vytvoří se v této oblasti.  
    -   Pokud máte existující servery ve vybrané oblasti, po výběru možnosti **Existující server** vypíše průvodce jejich seznam.

    Pro zajištění minimální latence vyberte oblast Azure, ve které je umístěný SQL Server. Další informace o oblastech najdete v tématu [Oblasti Azure](https://azure.microsoft.com/regions/).

4.  Určete, jestli chcete použít existující server, nebo vytvořit nový server Azure.

    Pokud je služba Active Directory v systému SQL Server federovaná pomocí služby Azure Active Directory, můžete ke komunikaci se vzdáleným serverem Azure volitelně využít účet federované služby pro SQL Server. Další informace o požadavcích pro tuto možnost najdete v tématu věnovaném [možnostem ALTER DATABASE SET (Transact-SQL)](https://msdn.microsoft.com/library/bb522682.aspx).

    -   **Vytvoření nového serveru**

        1.  Vytvořte přihlašovací jméno a heslo správce serveru.

        2.  Volitelně můžete ke komunikaci se vzdáleným serverem Azure využít účet federované služby pro SQL Server.

        ![Vytvoření nového serveru Azure – průvodce Stretch Database][StretchWizardImage4]

    -   **Existující server**

        1.  Vyberte existující server Azure.

        2.  Vyberte metodu ověřování.

            -   Pokud vyberete **Ověřování serveru SQL Server**, zadejte přihlašovací jméno a heslo správce.

            -   Pokud chcete ke komunikaci se vzdáleným serverem Azure využít účet federované služby pro SQL Server, vyberte **Integrované ověřování Active Directory**. Když není vybraný server spojen s funkcí Azure Active Directory, tato možnost se nezobrazí.

        ![Výběr stávajícího serveru Azure – průvodce Stretch Database][StretchWizardImage5]

## <a name="Credentials"></a>Zabezpečené přihlašovací údaje
Hlavní klíč databáze potřebujete k zabezpečení přihlašovacích údajů, které funkce Stretch Database používá k připojení ke vzdálené databázi.  

Pokud již existuje hlavní klíč databáze, zadejte odpovídající heslo.  

![Stránka zabezpečených přihlašovacích údajů průvodce Stretch Database][StretchWizardImage6b]

Pokud k databázi neexistuje hlavní klíč, zadejte silné heslo a vytvořte hlavní klíč databáze.  

![Stránka zabezpečených přihlašovacích údajů průvodce Stretch Database][StretchWizardImage6]

Další informace o hlavním klíči databáze najdete v tématech [CREATE MASTER KEY (Transact-SQL)](https://msdn.microsoft.com/library/ms174382.aspx) a [Vytvoření hlavního klíče databáze](https://msdn.microsoft.com/library/aa337551.aspx). Další informace o přihlašovacích údajích, které příslušný průvodce vytvoří, najdete v tématu [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](https://msdn.microsoft.com/library/mt270260.aspx).

## <a name="Network"></a>Výběr IP adres
Použijte rozsah IP adresy podsítě (doporučeno) nebo veřejnou IP adresu vašeho SQL Serveru, a vytvořte tak v Azure pravidlo brány, které SQL Serveru umožňuje komunikovat se vzdáleným serverem Azure.

IP adresa nebo adresy, které zadáte na této stránce, informují server Azure, aby povolil předání příchozích dat, dotazů a operací správy iniciovaných SQL Serverem prostřednictvím brány Azure. Tento průvodce nemění žádná nastavení brány firewall na SQL Serveru.

![Stránka výběru IP adres průvodce Stretch Database][StretchWizardImage7]

## <a name="Summary"></a>Souhrn
Zkontrolujte hodnoty, které jste zadali, možnosti, které jste vybrali v průvodci a odhadované náklady na provoz Azure. Potom povolte funkci Stretch volbou **Dokončit**.

![Souhrnná stránka průvodce Stretch Database][StretchWizardImage8]

## <a name="Results"></a>Výsledky
Zkontrolujte výsledky.

Pokud chcete sledovat stav migrace dat, přejděte do části [Monitorování a řešení potíží s migrací dat (Stretch Database)](sql-server-stretch-database-monitor.md).

![Stránka s výsledky průvodce Stretch Database][StretchWizardImage9]

## <a name="KnownIssues"></a>Řešení potíží s průvodcem
**Průvodce Stretch Database selhal.**
Pokud funkce Stretch Database není na úrovni server ještě povolena a průvodce spustíte bez oprávnění správce systému, které by ji povolily, dojde v průvodci k chybě. Požádejte správce systému, aby povolil funkci Stretch Database na místní instanci serveru a potom průvodce spusťte znovu. Další informace najdete v tématu [Předpoklad: Oprávnění pro povolení Stretch Database na serveru](sql-server-stretch-database-enable-database.md#EnableTSQLServer).

## Další kroky
Povolení dalších tabulek pro Stretch Database Monitorování migrace dat a správa tabulek \-a databází s povolenou funkcí Stretch.

-   [Povolení funkce Stretch Database pro tabulku](sql-server-stretch-database-enable-table.md) a povolení dalších tabulek

-   [Monitorování a řešení potíží s migrací dat](sql-server-stretch-database-monitor.md) pro zobrazení stavu migrace dat.

-   [Pozastavení a obnovení funkce Stretch Database](sql-server-stretch-database-pause.md)

-   [Správa a řešení potíží s funkcí Stretch Database](sql-server-stretch-database-manage.md)

-   [Zálohování databází s povolenou funkcí Stretch](sql-server-stretch-database-backup.md)

## Viz také

[Povolení funkce Stretch Database pro databázi](sql-server-stretch-database-enable-database.md)

[Povolení funkce Stretch Database pro tabulku](sql-server-stretch-database-enable-table.md)

[StretchWizardImage1]: ./media/sql-server-stretch-database-wizard/stretchwiz1.png
[StretchWizardImage2]: ./media/sql-server-stretch-database-wizard/stretchwiz2.png
[StretchWizardImage2a]: ./media/sql-server-stretch-database-wizard/stretchwiz2a.png
[StretchWizardImage2b]: ./media/sql-server-stretch-database-wizard/stretchwiz2b.png
[StretchWizardImage3]: ./media/sql-server-stretch-database-wizard/stretchwiz3.png
[StretchWizardImage4]: ./media/sql-server-stretch-database-wizard/stretchwiz4.png
[StretchWizardImage5]: ./media/sql-server-stretch-database-wizard/stretchwiz5.png
[StretchWizardImage6]: ./media/sql-server-stretch-database-wizard/stretchwiz6.png
[StretchWizardImage6b]: ./media/sql-server-stretch-database-wizard/stretchwiz6b.png
[StretchWizardImage7]: ./media/sql-server-stretch-database-wizard/stretchwiz7.png
[StretchWizardImage8]: ./media/sql-server-stretch-database-wizard/stretchwiz8.png
[StretchWizardImage9]: ./media/sql-server-stretch-database-wizard/stretchwiz9.png



<!--HONumber=Aug16_HO4-->


