---
title: "Začínáme s Data Lake Storem pomocí webu Azure Portal | Dokumentace Microsoftu"
description: "Jak používat Azure Portal k vytvoření účtu Data Lake Store a provádění základních operací v Data Lake Storu"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: fea324d0-ad1a-4150-81f0-8682ddb4591c
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/28/2017
ms.author: nitinme
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: fa13266993017374ba49709f8e22fbe6b03a28c7
ms.contentlocale: cs-cz
ms.lasthandoff: 08/07/2017

---
# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Začínáme s Azure Data Lake Store pomocí webu Azure Portal
> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Naučte se používat web Azure Portal k vytvoření účtu Azure Data Lake Store a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace najdete v tématu [Přehled služby Azure Data Lake Store](data-lake-store-overview.md).

Následující dvě videa obsahují stejné informace jako tento článek:

* [Vytvoření účtu Data Lake Store](https://mix.office.com/watch/1k1cycy4l4gen)
* [Správa dat ve službě Data Lake Store pomocí Průzkumníka dat](https://mix.office.com/watch/icletrxrh6pc)

## <a name="prerequisites"></a>Požadavky
Před zahájením tohoto kurzu musíte mít tyto položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-an-azure-data-lake-store-account"></a>Vytvoření účtu Azure Data Lake Store

1. Přihlaste se k novému webu [Azure Portal](https://portal.azure.com).
2. Klikněte na položku **NOVÝ**, **Data + úložiště** a potom **Azure Data Lake Store**. Přečtěte si informace v okně **Azure Data Lake Store** a potom klikněte v levém dolním rohu okna na tlačítko **Vytvořit**.
3. V okně **Nová služba Data Lake Store** zadejte hodnoty tak, jak ukazuje následující snímek obrazovky:
   
    ![Vytvoření nového účtu Azure Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Vytvoření nového účtu Azure Data Lake Store")
   
   * **Název**. Zadejte jedinečný název účtu Data Lake Storu.
   * **Předplatné**. Vyberte předplatné, v rámci kterého chcete vytvořit nový účet Data Lake Store.
   * **Skupina prostředků**. Vyberte existující skupinu prostředků nebo ji vytvořte pomocí možnosti **Vytvořit novou**. Skupina prostředků je kontejner, který obsahuje související prostředky pro aplikaci. Další informace najdete v tématu [Skupiny prostředků v Azure](../azure-resource-manager/resource-group-overview.md#resource-groups).
   * **Umístění**: Vyberte umístění, kde chcete vytvořit účet Data Lake Store.
   * **Nastavení šifrování**. Existují tři možnosti:
     
     * **Nepovolovat šifrování**.
     * **Používat klíče spravované službou Azure Data Lake**.  Vyberte tuto možnost, pokud chcete, aby služba Azure Data Lake Store spravovala vaše šifrovací klíče.
     * **Zvolit klíče z Azure Key Vault**. Můžete vybrat existující službu Azure Key Vault nebo vytvořit novou. Pokud chcete používat klíče ze služby Key Vault, musíte účtu Azure Data Lake Store přiřadit oprávnění pro přístup ke službě Azure Key Vault. Pokyny najdete v části [Přiřazení oprávnění pro Azure Key Vault](#assign-permissions-to-azure-key-vault).
       
        ![Šifrování Data Lake Storu](./media/data-lake-store-get-started-portal/adls-encryption-2.png "Šifrování Data Lake Storu")
       
        Klikněte na **OK** v okně **Nastavení šifrování**.

        Další informace najdete v tématu [Šifrování dat v Azure Data Lake Store](./data-lake-store-encryption.md).

4. Klikněte na možnost **Vytvořit**. Pokud jste se rozhodli připnout účet na řídicí panel, vrátíte se na řídicí panel, kde uvidíte průběh zřizování účtu Data Lake Store. Po zřízení účtu Data Lake Store se zobrazí okno účtu.

Účet Data Lake Store můžete vytvořit také pomocí šablon Azure Resource Manageru. Tyto šablony jsou dostupné na stránce [Šablony rychlého startu Azure](https://azure.microsoft.com/resources/templates/?term=data+lake+store):

- Bez šifrování dat: [Nasazení účtu Azure Data Lake Store bez šifrování dat](https://azure.microsoft.com/en-us/resources/templates/101-data-lake-store-no-encryption/).
- S šifrováním dat pomocí služby Data Lake Store: [Nasazení účtu Data Lake Store s šifrováním (Data Lake)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-adls/).
- S šifrováním dat pomocí služby Azure Key Vault: [Nasazení účtu Data Lake Store s šifrováním (Key Vault)](https://azure.microsoft.com/resources/templates/101-data-lake-store-encryption-key-vault/).

### <a name="assign-permissions-to-azure-key-vault"></a>Přiřazení oprávnění pro Azure Key Vault
Pokud jste ke konfiguraci šifrování pro účet Data Lake Store použili klíče z Azure Key Vaultu, musíte nakonfigurovat přístup mezi účtem Data Lake Store a účtem Azure Key Vault. To uděláte podle následujících kroků.

1. Pokud jste použili klíče z Azure Key Vaultu, v okně účtu Data Lake Store se nahoře zobrazí upozornění. Kliknutím na upozornění otevřete okno **Konfigurovat oprávnění pro Key Vault**.
   
    ![Šifrování Data Lake Storu](./media/data-lake-store-get-started-portal/adls-encryption-3.png "Šifrování Data Lake Storu")
2. V okně se zobrazují dvě možnosti pro konfiguraci přístupu.
   
   * V první možnosti kliknutím na **Udělit oprávnění** nakonfigurujte přístup. První možnost je aktivní jenom v případě, že uživatel, který vytvořil účet Data Lake Store, je současně správcem pro Azure Key Vault.
   * Druhou možností je spustit rutinu PowerShellu zobrazenou v okně. Musíte být vlastníkem Azure Key Vaultu nebo mít možnost udělovat oprávnění pro Azure Key Vault. Po spuštění rutiny se vraťte do okna a kliknutím na **Povolit** nakonfigurujte přístup.

## <a name="createfolder"></a>Vytváření složek v účtu služby Azure Data Lake Store
V rámci účtu Data Lake Store můžete vytvářet složky, které slouží ke správě a ukládání dat.

1. Otevřete účet Data Lake Store, který jste vytvořili. V levém podokně klikněte na možnost **Procházet**, klikněte na možnost **Data Lake Store** a potom v okně Data Lake Store klikněte na název účtu, ve kterém chcete vytvořit složky. Pokud jste účet připnuli na úvodní panel, klikněte na dlaždici tohoto účtu.
2. V okně účtu Data Lake Store klikněte na možnost **Průzkumník dat**.
   
    ![Vytváření složek v účtu Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Vytváření složek v účtu Data Lake Store")
3. V okně účtu Data Lake Store klikněte na možnost **Nová složka**, zadejte název nové složky a klikněte na tlačítko **OK**.
   
    ![Vytváření složek v účtu Data Lake Store](./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Vytváření složek v účtu Data Lake Store")
   
    Nově vytvořená složka se zobrazí v okně **Průzkumník dat**. Můžete vytvářet vnořené složky do libovolné úrovně.
   
    ![Vytváření složek v účtu Data Lake Store](./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Vytváření složek v účtu Data Lake Store")

## <a name="uploaddata"></a>Nahrání dat do účtu služby Azure Data Lake Store
Data můžete do účtu Azure Data Lake Store nahrát přímo na úrovni kořenového adresáře nebo do složky, kterou jste v rámci účtu vytvořili. Na následujícím snímku obrazovky je uvedený postup nahrání souboru do podsložky v okně **Průzkumník dat**. Na tomto snímku obrazovky se soubor nahraje do podsložky, která je uvedená v popisu cesty (označená červeným rámečkem).

Pokud hledáte ukázková data, která byste mohli nahrát, můžete použít složku **Ambulance Data** z [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Nahrání dat](./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Nahrání dat")

## <a name="properties"></a>Vlastnosti a akce dostupné u uložených dat
Kliknutím na nově přidaný soubor otevřete okno **Vlastnosti**. V tomto okně jsou dostupné vlastnosti přidružené k souboru a akce, které můžete se souborem provádět. Můžete také zkopírovat úplnou cestu k souboru v účtu Azure Data Lake Store, která je na následujícím snímku obrazovky označená červeným rámečkem:

![Vlastnosti dat](./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Vlastnosti dat")

* Kliknutím na položku **Náhled** zobrazte přímo z prohlížeče náhled souboru. Můžete taky určit formát náhledu. Klikněte na položku **Náhled**, v okně **Náhled souboru** klikněte na položku **Formát** a v okně **Formát náhledu souboru** zadejte možnosti, jako je počet zobrazených řádků, kódování nebo oddělovač, které chcete použít, atd.
  
  ![Formát náhledu souboru](./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Formát náhledu souboru")
* Pokud chcete soubor stáhnout do počítače, klikněte na možnost **Stáhnout**.
* Pokud chcete soubor přejmenovat, klikněte na možnost **Přejmenovat soubor**.
* Pokud chcete soubor odstranit, klikněte na možnost **Odstranit soubor**.

## <a name="secure-your-data"></a>Zabezpečení dat
Data uložená v účtu Azure Data Lake Store můžete zabezpečit pomocí služby Azure Active Directory a řízení přístupu (ACL). Pokyny, jak to provést, najdete v tématu [Zabezpečení dat v Azure Data Lake Store](data-lake-store-secure-data.md).

## <a name="delete-azure-data-lake-store-account"></a>Odstranění účtu Azure Data Lake Store
Pokud chcete odstranit účet Azure Data Lake Store, v okně Data Lake Store klikněte na možnost **Odstranit**. Tuto akci je nutné potvrdit, a proto se zobrazí výzva k zadání názvu účtu, který chcete odstranit. Zadejte název účtu a klikněte na možnost **Odstranit**.

![Odstranění účtu Data Lake](./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Odstranění účtu Data Lake")

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
* [Zobrazení protokolů diagnostiky pro Data Lake Store](data-lake-store-diagnostic-logs.md)


