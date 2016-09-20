<properties 
   pageTitle="Začínáme s Data Lake Store | Azure" 
   description="Použití portálu k vytvoření účtu Data Lake Store a provádění základních operací v Data Lake Store" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# Začínáme s Azure Data Lake Store pomocí webu Azure Portal

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat web Azure Portal k vytvoření účtu Azure Data Lake Store a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace o službě Data Lake Store najdete v tématu [Přehled o Azure Data Lake Store](data-lake-store-overview.md).

## Požadavky

Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:

- **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## Pomáhají vám při učení videa?

Pokud chcete začít s Data Lake Store, podívejte se na následující videa.

* [Vytvoření účtu Data Lake Store](https://mix.office.com/watch/1k1cycy4l4gen)
* [Správa dat v Data Lake Store pomocí Průzkumníku dat](https://mix.office.com/watch/icletrxrh6pc)

## Vytvoření účtu Azure Data Lake Store

1. Přihlaste se k novému [webu Azure Portal](https://portal.azure.com).

2. Klikněte na položku **NOVÝ**, **Data + úložiště** a potom **Azure Data Lake Store**. Přečtěte si informace v okně **Azure Data Lake Store** a potom klikněte v levém dolním rohu okna na tlačítko **Vytvořit**.

3. V okně **Nová služba Data Lake Store** zadejte hodnoty tak, jak ukazuje následující snímek obrazovky:

    ![Vytvoření nového účtu Azure Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Create a new Azure Data Lake account")

    - **Předplatné**. Vyberte předplatné, v rámci kterého chcete vytvořit nový účet Data Lake Store.
    - **Skupina prostředků**. Vyberte existující skupinu prostředků nebo klikněte na možnost **Vytvořit skupinu prostředků** a vytvořte novou. Skupina prostředků je kontejner, který obsahuje související prostředky pro aplikaci. Další informace najdete v tématu [Skupiny prostředků v Azure](resource-group-overview.md#resource-groups).
    - **Umístění**: Vyberte umístění, kde chcete vytvořit účet Data Lake Store.

4. Jestliže chcete, aby byl účet Data Lake Store přístupný z Úvodního panelu, vyberte položku **Připnout na Úvodní panel**.

5. Klikněte na možnost **Vytvořit**. Pokud jste se rozhodli připnout účet na úvodní panel, přejdete zpět na úvodní panel a můžete sledovat průběh zřizování účtu Data Lake Store. Po zřízení účtu Data Lake Store se zobrazí okno účtu.

6. Rozbalte rozevírací seznam **Základy** a zobrazte informace o účtu Data Lake Store, jako je například skupina prostředků, do které patří, umístění atd. Kliknutím na ikonu **Rychlý Start** zobrazte odkazy na další prostředky týkající se Data Lake Store.

    ![Účet Azure Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Your Azure Data Lake account")

## <a name="createfolder"></a>Vytváření složek v účtu Azure Data Lake Store

V rámci účtu Data Lake Store můžete vytvářet složky, které slouží ke správě a ukládání dat.

1. Otevřete účet Data Lake Store, který jste právě vytvořili. V levém podokně klikněte na možnost **Procházet**, klikněte na možnost **Data Lake Store** a potom v okně Data Lake Store klikněte na název účtu, ve kterém chcete vytvořit složky. Pokud jste účet připnuli na úvodní panel, klikněte na dlaždici tohoto účtu.

2. V okně účtu Data Lake Store klikněte na možnost **Průzkumník dat**.

    ![Vytváření složek v účtu Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Create folders in Data Lake Store account")

3. V okně účtu Data Lake Store klikněte na možnost **Nová složka**, zadejte název nové složky a klikněte na tlačítko **OK**.
    
    ![Vytváření složek v účtu Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Create folders in Data Lake Store account")
    
    Nově vytvořená složka se zobrazí v okně **Průzkumníku dat**. Můžete vytvářet vnořené složky do libovolné úrovně.

    ![Vytváření složek v účtu Data Lake](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Create folders in Data Lake account")


## <a name="uploaddata"></a>Nahrání dat do účtu Azure Data Lake Store

Data můžete do účtu Azure Data Lake Store nahrát přímo na úrovni kořenového adresáře nebo do složky, kterou jste v rámci účtu vytvořili. Na níže zobrazeném snímku obrazovky je uvedený postup nahrání souboru do podsložky v okně **Průzkumníku dat**. Na tomto snímku obrazovky se soubor nahraje do podsložky, která je uvedená v popisu cesty (označená červeným rámečkem).

Pokud hledáte ukázková data, která byste mohli nahrát, můžete použít složku **Ambulance Data** z [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Nahrání dat](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Upload data")


## <a name="properties"></a>Vlastnosti a akce dostupné u uložených dat

Kliknutím na nově přidaný soubor otevřete okno **Vlastnosti**. V tomto okně jsou dostupné vlastnosti přidružené k souboru a akce, které můžete se souborem provádět. Můžete taky zkopírovat úplnou cestu k souboru v účtu Azure Data Lake Store, která je na následujícím snímku obrazovky označená červeným rámečkem.

![Vlastnosti dat](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Properties on the data")

* Kliknutím na položku **Náhled** zobrazte přímo z prohlížeče náhled souboru. Můžete taky určit formát náhledu. Klikněte na položku **Náhled**, v okně **Náhled souboru** klikněte na položku **Formát** a v okně **Formát náhledu souboru** zadejte možnosti, jako je počet zobrazených řádků, kódování nebo oddělovač, které chcete použít, atd.

  ![Formát náhledu souboru](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "File preview format")

* Pokud chcete soubor stáhnout do počítače, klikněte na možnost **Stáhnout**.

* Pokud chcete soubor přejmenovat, klikněte na možnost **Přejmenovat soubor**.

* Pokud chcete soubor odstranit, klikněte na možnost **Odstranit soubor**.


## Zabezpečení dat

Data uložená v účtu Azure Data Lake Store můžete zabezpečit pomocí služby Azure Active Directory a řízení přístupu (ACL). Pokyny, jak to provést, najdete v tématu [Zabezpečení dat v Azure Data Lake Store](data-lake-store-secure-data.md).


## Odstranění účtu Azure Data Lake Store

Pokud chcete odstranit účet Azure Data Lake Store, v okně Data Lake Store klikněte na možnost **Odstranit**. Tuto akci je nutné potvrdit, a proto se zobrazí výzva k zadání názvu účtu, který chcete odstranit. Zadejte název účtu a klikněte na možnost **Odstranit**.

![Odstranění účtu Data Lake](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Delete Data Lake account")


## Další kroky

- [Zabezpečení dat v Data Lake Store](data-lake-store-secure-data.md)
- [Použití Azure Data Lake Analytics s Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Použití Azure HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Zobrazení protokolů diagnostiky pro Data Lake Store](data-lake-store-diagnostic-logs.md)



<!--HONumber=sep16_HO2-->


