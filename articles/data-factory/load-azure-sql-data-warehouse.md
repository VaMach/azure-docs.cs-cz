---
title: "Načtení dat do Azure SQL Data Warehouse pomocí Azure Data Factory | Microsoft Docs"
description: "Použití Azure Data Factory ke zkopírování dat do Azure SQL Data Warehouse"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: 36e24da50386d1abc441e2beb09f570a9612a346
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-sql-data-warehouse-using-azure-data-factory"></a>Načtení dat do Azure SQL Data Warehouse pomocí Azure Data Factory

[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) je databáze založená na cloudu, škálování dokáže zpracovávat ohromné objemy dat, relačních i nerelačních.  Postavená na architektuře massively parallel processing (MPP), SQL Data Warehouse je optimalizovaná pro podnikové datového skladu úlohy.  Pružnost cloudu nabízí flexibilitu škálovat úložiště a výpočetní nezávisle.

Začínáme s Azure SQL Data Warehouse je teď jednodušší než kdy použití **Azure Data Factory**.  Azure Data Factory je plně spravovaná Cloudová datová integrační služby, která slouží k naplnění SQL Data Warehouse s daty z vašeho stávajícího systému a ukládání je hodně času při vyhodnocení SQL Data Warehouse a sestavování analytické údaje řešení. Tady jsou klíčové výhody načítání dat do Azure SQL Data Warehouse pomocí Azure Data Factory:

* **Snadno nastavit**: krok 5 intuitivní průvodce bez potřeby skriptování.
* **Podpora úložiště bohaté dat**: integrovanou podporu pro bohatou sadu místní a Cloudová datová úložiště, viz podrobný seznam v [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.
* **Zabezpečení a dodržování**: data se přenáší prostřednictvím protokolu HTTPS nebo ExpressRoute a přítomnost globální služby zajišťuje vaše data nikdy neopustí zeměpisné hranic
* **Bezkonkurenční výkonu pomocí funkce PolyBase**: pomocí funkce Polybase je nejúčinnější způsob, jak přesunout data do Azure SQL Data Warehouse. Používá funkci pracovní objekt blob, můžete dosáhnout vysokého zatížení rychlosti ze všech typů úložišť dat kromě úložiště objektů Blob v Azure a Data Lake Store, která Polybase podporuje ve výchozím nastavení. Další informace z podrobností o [výkonu kopie aktivity](copy-activity-performance.md).

V tomto článku se dozvíte, jak pomocí nástroje Data Factory kopírovat Data do **načtení dat z databáze SQL Azure do Azure SQL Data Warehouse**. Můžete provést podobné kroky pro zkopírování dat z jiné typy dat úložiště.

> [!NOTE]
>  Obecné informace o funkcích služby Data Factory v kopírování dat do/z Azure SQL Data Warehouse najdete v tématu [zkopírovat data do nebo z Azure SQL Data Warehouse pomocí Azure Data Factory](connector-azure-sql-data-warehouse.md) článku.
>
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.
* **Azure SQL Data Warehouse**. Tento datový sklad obsahuje data zkopírovaná z SQL Database. Pokud Azure SQL Data Warehouse nemáte, přečtěte si článek věnovaný [vytvoření služby SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md), kde najdete kroky pro její vytvoření.
* **Azure SQL Database**. V tomto kurzu zkopíruje data z databáze SQL Azure s ukázkovými daty společnosti Adventure Works LT, můžete vytvořit jeden následující [vytvoření Azure SQL database](../sql-database/sql-database-get-started-portal.md) článku. 
* **Účet služby Azure Storage**. Úložiště Azure se používá jako **pracovní** objektů blob v operaci hromadného kopírování. Pokud nemáte účet úložiště Azure, přečtěte si článek [Vytvoření účtu úložiště](../storage/common/storage-create-storage-account.md#create-a-storage-account), kde najdete kroky pro jeho vytvoření.

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. V nabídce vlevo klikněte na **NOVÝ**, klikněte na **Data + Analýza** a poté na **Objekt pro vytváření dat**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/load-azure-sql-data-warehouse/new-azure-data-factory-menu.png)
2. V **nový objekt pro vytváření dat** stránky, zadejte hodnoty, jak je znázorněno na následujícím snímku obrazovky:
      
     ![Nová stránka objektu pro vytváření dat](./media/load-azure-sql-data-warehouse/new-azure-data-factory.png)
 
   * **Jméno.** Zadejte jedinečný název globální služby data Factory. Pokud se zobrazí následující chyba, změňte název objektu pro vytváření dat (třeba na váš_název_ADFTutorialDataFactory) a zkuste to znovu. V tématu [pro vytváření dat – pravidla pojmenování](naming-rules.md) článku pravidla pojmenování artefaktů služby Data Factory.
  
            `Data factory name "LoadSQLDWDemo" is not available`

    * **Předplatné.** Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
    * **Skupina prostředků.** Vyberte existující skupinu prostředků z rozevíracího seznamu, nebo **vytvořit nový** možnost a zadejte název skupiny prostředků. Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Verze.** Vyberte **V2 (Preview)**.
    * **Umístění.** Vyberte umístění služby data Factory. V rozevíracím seznamu se zobrazí pouze podporovaných umístění. Úložiště dat (Azure Storage, Azure SQL Database atd.) používané pro vytváření dat může být v jiných umístěních a oblastech. 

3. Klikněte na možnost **Vytvořit**.
4. Po vytvoření je dokončení, přejděte na datovou továrnu a uvidíte **Data Factory** stránky, jak je znázorněno na obrázku. Klikněte na tlačítko **Autor & monitorování** dlaždici spuštění aplikace integraci dat na samostatné kartě.
   
   ![Domovská stránka objektu pro vytváření dat](./media/load-azure-sql-data-warehouse/data-factory-home-page.png)

## <a name="load-data-into-azure-sql-data-warehouse"></a>Načtení dat do Azure SQL Data Warehouse

1. Na stránku Začínáme, klikněte na **kopírovat Data** dlaždici spustíte nástroj zkopírovat Data. 

   ![Dlaždice nástroj pro kopírování dat](./media/load-azure-sql-data-warehouse/copy-data-tool-tile.png)
2. V **vlastnosti** stránku nástroje pro kopírování dat, zadejte **CopyFromSQLToSQLDW** pro **název úlohy**a klikněte na tlačítko **Další**. 

    ![Kopírování dat vlastnosti nástroje stránky](./media/load-azure-sql-data-warehouse/copy-data-tool-properties-page.png)
3. V **zdrojového úložiště dat** vyberte **Azure SQL Database**a klikněte na tlačítko **Další**.

    ![Stránka úložiště zdroje dat](./media/load-azure-sql-data-warehouse/specify-source.png)
4. V **zadat databázi Azure SQL** proveďte následující kroky: 
    1. Vyberte server Azure SQL pro **název serveru**.
    2. Vyberte svou databázi Azure SQL pro **název databáze**.
    3. Zadejte jméno uživatele pro **uživatelské jméno**.
    4. Zadejte heslo pro uživatele pro **heslo**.
    5. Klikněte na **Další**. 

        ![Zadejte databáze Azure SQL](./media/load-azure-sql-data-warehouse/specify-source-connection.png)
5. V **vyberte tabulky, ze kterého chcete zkopírovat data nebo použít vlastní dotaz** stránky, filtrovat tabulky zadáním **SalesLT** do vstupního pole, zkontrolujte **(Vybrat vše)** Vyberte všechny tabulky, a pak klikněte na políčko **Další**. 

    ![Zvolte vstupního souboru nebo složky](./media/load-azure-sql-data-warehouse/select-source-tables.png)

6. V **cílového úložiště dat** vyberte **Azure SQL Data Warehouse**a klikněte na tlačítko **Další**. 

    ![Cílová stránka úložiště dat](./media/load-azure-sql-data-warehouse/specify-sink.png)
7. V **zadejte Azure SQL Data Warehouse** proveďte následující kroky: 

    1. Vyberte server Azure SQL pro **název serveru**.
    2. Vyberte datový sklad SQL Azure pro **název databáze**.
    3. Zadejte jméno uživatele pro **uživatelské jméno**.
    4. Zadejte heslo pro uživatele pro **heslo**.
    5. Klikněte na **Další**. 

        ![Zadejte Azure SQL Data Warehouse](./media/load-azure-sql-data-warehouse/specify-sink-connection.png)
8. V **mapování tabulky** zkontrolujte a klikněte na tlačítko **Další**. Mapování u inteligentního tabulky se zobrazí, že mapy zdrojové do cílové tabulky podle názvů tabulek. Pokud tabulka neexistuje v cílovém, ve výchozím nastavení Azure Data Factory vytvoří se stejným názvem. Můžete také mapovat k existující tabulce. 

    > [!NOTE]
    > Automatické vytváření tabulky pro jímku SQL Data Warehouse použitelná v případě systému SQL Server nebo Azure SQL Database je zdrojem. Při kopírování dat z jiného zdroje dat úložiště, můžete potřebovat k předběžné vytvoření schématu v podřízený Azure SQL Data Warehouse před kopírování dat.

    ![Stránka mapování tabulky](./media/load-azure-sql-data-warehouse/specify-table-mapping.png)

9. V **schéma mapování** zkontrolujte a klikněte na tlačítko **Další**. Inteligentní mapování je založena na název sloupce. Pokud si zvolíte umožníte Data Factory automaticky vytvářet tabulky, převod typu správné dat může dojít, pokud je nutná k vyřešení nekompatibilita mezi zdrojovým a cílovým úložišti. Pokud konverzi nepodporovaný datový typ mezi zdrojovým a cílovým sloupcem se zobrazí chybová zpráva spolu s odpovídající tabulky.

    ![Schéma mapování stránky](./media/load-azure-sql-data-warehouse/specify-schema-mapping.png)

11. V **nastavení** vyberte Azure Storage v **název účtu úložiště** rozevíracího seznamu. Používá se pro přípravu dat, než se načte do SQL Data Warehouse pomocí PolyBase. Po dokončení kopírování průběžných dat v úložišti se automaticky vyčistí. 

    V části "Pokročilé nastavení" zrušte zaškrtnutí políčka "použít výchozí typ".

    ![Nastavení stránky](./media/load-azure-sql-data-warehouse/copy-settings.png)
12. V **Souhrn** stránka, zkontrolujte nastavení a klikněte na tlačítko **Další**.

    ![Stránka souhrnu](./media/load-azure-sql-data-warehouse/summary-page.png)
13. V **stránka nasazení**, klikněte na tlačítko **monitorování** k monitorování kanálu (úlohy).

    ![Stránka nasazení](./media/load-azure-sql-data-warehouse/deployment-page.png)
14. Všimněte si, že **monitorování** je automaticky vybraná karta na levé straně. Najdete v článku odkazy. Chcete-li zobrazit podrobnosti o spuštění aktivity a znovu spusťte kanál v **akce** sloupce. 

    ![Spustí monitorování kanálu](./media/load-azure-sql-data-warehouse/monitor-pipeline-run.png)
15. Chcete-li zobrazit běh aktivit, které jsou přidružené k kanálu spustit, klikněte na tlačítko **zobrazení aktivity spouští** odkaz v **akce** sloupec. Jsou 10 kopie aktivity v kanálu, každý zkopíruje jednu tabulku dat. Přepněte zpět do kanálu spustí zobrazení, klikněte na tlačítko **kanály** odkaz v horní části. Klikněte na tlačítko **aktualizovat** k aktualizaci seznamu. 

    ![Spustí monitorování aktivity](./media/load-azure-sql-data-warehouse/monitor-activity-run.png)

16. Podrobnosti o provádění každou aktivitu kopírování, můžete sledovat další kliknutím **podrobnosti** v části **akce** v aktivitě zobrazení monitorování. Zobrazuje informace, včetně objem dat zkopíruje ze zdroje jímka, propustnost, kroky se prochází s odpovídající doba trvání a použít konfigurace.

    ![Podrobnosti o spuštění aktivity monitorování](./media/load-azure-sql-data-warehouse/monitor-activity-run-details.png)

## <a name="next-steps"></a>Další postup

Přechodu na v následujícím článku na další informace o podpoře Azure SQL Data Warehouse: 

> [!div class="nextstepaction"]
>[Konektor služby Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md)