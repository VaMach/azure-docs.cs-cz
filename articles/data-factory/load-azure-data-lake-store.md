---
title: "Načtení dat do Azure Data Lake Store pomocí Azure Data Factory | Microsoft Docs"
description: "Použití Azure Data Factory ke zkopírování dat do Azure Data Lake Store"
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
ms.openlocfilehash: 3f73cd65b0ceb3148ce8ceb83d7b4e1be1280077
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/18/2018
---
# <a name="load-data-into-azure-data-lake-store-using-azure-data-factory"></a>Načtení dat do Azure Data Lake Store pomocí Azure Data Factory

[Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) je jako podniková velkého rozsahu úložiště pro analytické úlohy s velkými objemy dat. Azure Data Lake umožňuje zaznamenávat data libovolné velikosti, typu a rychlosti příjmu do jediného místa pro účely provozní a zjišťovací analýzy.

Azure Data Factory je plně spravovaná Cloudová datová integrační služby, která slouží k naplnění lake s daty z vašeho stávajícího systému a ukládání můžete drahocenný čas při vytváření vlastních analytických řešení. Tady jsou klíčové výhody načítání dat do Azure Data Lake Store pomocí Azure Data Factory:

* **Snadno nastavit**: krok 5 intuitivní průvodce bez potřeby skriptování.
* **Podpora úložiště bohaté dat**: integrovanou podporu pro bohatou sadu místní a Cloudová datová úložiště, viz podrobný seznam v [podporovanými úložišti dat](copy-activity-overview.md#supported-data-stores-and-formats) tabulky.
* **Zabezpečení a dodržování**: data se přenáší prostřednictvím protokolu HTTPS nebo ExpressRoute a přítomnost globální služby zajišťuje vaše data nikdy neopustí zeměpisné hranic
* **Vysoký výkon**: až 1 GB/s dat rychlost načítání do Azure Data Lake Store. Další informace z podrobností o [výkonu kopie aktivity](copy-activity-performance.md).

V tomto článku se dozvíte, jak pomocí nástroje Data Factory kopírovat Data do **načtení dat z Amazon S3 do Azure Data Lake Store**. Můžete provést podobné kroky pro zkopírování dat z jiné typy dat úložiště.

> [!NOTE]
>  Obecné informace o funkcích služby Data Factory v kopírování dat do/z Azure Data Lake Store najdete v tématu [zkopírovat data do nebo z Azure Data Lake Store pomocí Azure Data Factory](connector-azure-data-lake-store.md) článku.
>
> Tento článek se týká verze 2 služby Data Factory, která je aktuálně ve verzi Preview. Pokud používáte verzi 1 služby Data Factory, který je všeobecně dostupná (GA), přečtěte si téma [aktivitu kopírování v V1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Pokud ještě nemáte předplatné Azure, vytvořte si [bezplatný účet](https://azure.microsoft.com/free/) před tím, než začnete.
* **Azure Data Lake Store**. Pokud nemáte účet Data Lake Store, najdete v článku [vytvoření účtu Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account) najdete v článku kroky k jeho vytvoření.
* **Amazon S3**. Tento článek ukazuje, jak zkopírovat data z Amazonu S3. Jiným úložištím dat můžete pomocí následujících podobným způsobem.

## <a name="create-a-data-factory"></a>Vytvoření datové továrny

1. V nabídce vlevo klikněte na **NOVÝ**, klikněte na **Data + Analýza** a poté na **Objekt pro vytváření dat**. 
   
   ![Nový -> Objekt pro vytváření dat](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. V **nový objekt pro vytváření dat** stránky, zadejte hodnoty, jak je znázorněno na následujícím snímku obrazovky: 
      
     ![Nová stránka objektu pro vytváření dat](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
   * **Jméno.** Zadejte jedinečný název globální služby data Factory. Pokud se zobrazí následující chyba, změňte název objektu pro vytváření dat (třeba na váš_název_ADFTutorialDataFactory) a zkuste to znovu. V tématu [pro vytváření dat – pravidla pojmenování](naming-rules.md) článku pravidla pojmenování artefaktů služby Data Factory.
  
            `Data factory name "LoadADLSDemo" is not available`

    * **Předplatné.** Vyberte své **předplatné** Azure, ve kterém chcete vytvořit datovou továrnu. 
    * **Skupina prostředků.** Vyberte existující skupinu prostředků z rozevíracího seznamu, nebo **vytvořit nový** možnost a zadejte název skupiny prostředků. Informace o skupinách prostředků najdete v článku [Použití skupin prostředků ke správě prostředků Azure](../azure-resource-manager/resource-group-overview.md).  
    * **Verze.** Vyberte **V2 (Preview)**.
    * **Umístění.** Vyberte umístění služby data Factory. V rozevíracím seznamu se zobrazí pouze podporovaných umístění. Úložiště dat (Azure Data Lake Store, Azure Storage, Azure SQL Database atd.) používané pro vytváření dat může být v jiných umístěních a oblastech.

3. Klikněte na možnost **Vytvořit**.
4. Po vytvoření je dokončení, přejděte na datovou továrnu a uvidíte **Data Factory** stránky, jak je znázorněno na obrázku. Klikněte na tlačítko **Autor & monitorování** dlaždici spuštění aplikace integraci dat na samostatné kartě. 
   
   ![Domovská stránka objektu pro vytváření dat](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

## <a name="load-data-into-azure-data-lake-store"></a>Načtení dat do Azure Data Lake Store

1. Na stránku Začínáme, klikněte na **kopírovat Data** dlaždici spustíte nástroj zkopírovat Data. 

   ![Dlaždice nástroj pro kopírování dat](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. V **vlastnosti** stránku nástroje pro kopírování dat, zadejte **CopyFromAmazonS3ToADLS** pro **název úlohy**a klikněte na tlačítko **Další**. 

    ![Kopírování dat vlastnosti nástroje stránky](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. V **zdrojového úložiště dat** vyberte **Amazon S3**a klikněte na tlačítko **Další**.

    ![Stránka úložiště zdroje dat](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. V **připojení zadejte Amazon S3** proveďte následující kroky: 
    1. Zadejte **přístup ID klíče**.
    2. Zadejte **tajný přístupový klíč**.
    3. Klikněte na **Další**. 

        ![Zadejte účet Amazon S3](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. V **zvolte vstupní soubor nebo složku** stránky, přejděte na složku nebo soubor, který chcete kopírovat přes, vyberte a klikněte na tlačítko **zvolte**, pak klikněte na tlačítko **Další**. 

    ![Zvolte vstupního souboru nebo složky](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. V **cílového úložiště dat** vyberte **Azure Data Lake Store**a klikněte na tlačítko **Další**. 

    ![Cílová stránka úložiště dat](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Na této stránce zvolte chování kopie kontrolou **kopírování souborů rekurzivně** a **binární kopie** (kopírovat soubory jako-je) možnosti. Klikněte na **Další**.

    ![Zadejte výstupní složky](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. V **zadejte Data Lake Store připojení** proveďte následující kroky: 

    1. Vyberte pro Data Lake Store **název účtu Data Lake Store**.
    2. Zadejte hlavní informace služby včetně **klienta**, **služby ID objektu zabezpečení**, a **hlavní klíč služby**.
    3. Klikněte na **Další**. 

    > [!IMPORTANT]
    > V tomto návodu můžete použít **instanční objekt** k ověření data lake store. Postupujte podle pokynů [zde](connector-azure-data-lake-store.md#using-service-principal-authentication) a zajistěte, aby udělit hlavní správné oprávnění služby v Azure Data Lake Store.

    ![Zadejte účet Azure Data Lake Store](./media/load-data-into-azure-data-lake-store/specify-adls.png)

9. V **zvolit výstupní soubor nebo složku** zadejte **copyfroms3**, pak klikněte na tlačítko **Další**. 

    ![Zadejte výstupní složky](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)


10. V **nastavení** klikněte na tlačítko **Další**. 

    ![Nastavení stránky](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. V **Souhrn** stránka, zkontrolujte nastavení a klikněte na tlačítko **Další**.

    ![Stránka souhrnu](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. V **stránka nasazení**, klikněte na tlačítko **monitorování** k monitorování kanálu (úlohy).

    ![Stránka nasazení](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Všimněte si, že **monitorování** je automaticky vybraná karta na levé straně. Najdete v článku odkazy. Chcete-li zobrazit podrobnosti o spuštění aktivity a znovu spusťte kanál v **akce** sloupce. 

    ![Spustí monitorování kanálu](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Chcete-li zobrazit běh aktivit, které jsou přidružené k kanálu spustit, klikněte na tlačítko **zobrazení aktivity spouští** odkaz v **akce** sloupec. Není pouze jednu aktivitu (aktivita kopírování) v kanálu, takže se zobrazí pouze jedna položka. Přepněte zpět do kanálu spustí zobrazení, klikněte na tlačítko **kanály** odkaz v horní části. Klikněte na tlačítko **aktualizovat** k aktualizaci seznamu. 

    ![Spustí monitorování aktivity](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. Podrobnosti o provádění každou aktivitu kopírování, můžete sledovat další kliknutím **podrobnosti** v části **akce** v aktivitě zobrazení monitorování. Zobrazuje informace, včetně objem dat zkopíruje ze zdroje jímka, propustnost, kroky se prochází s odpovídající doba trvání a použít konfigurace.

    ![Podrobnosti o spuštění aktivity monitorování](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Ověřte, že data budou zkopírována do Azure Data Lake Store. 

    ![Zkontrolujte výstup Data Lake Store](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Další postup

Přechodu na v následujícím článku na další informace o podpoře Azure Data Lake Store: 

> [!div class="nextstepaction"]
>[Konektor Azure Data Lake Store](connector-azure-data-lake-store.md)