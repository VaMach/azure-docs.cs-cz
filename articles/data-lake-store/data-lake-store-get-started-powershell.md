---
title: "Začínáme s Azure Data Lake Store pomocí PowerShellu | Dokumentace Microsoftu"
description: "Použití prostředí Azure PowerShell k vytvoření účtu Data Lake Store a provádění základních operací"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: c00aa4a2e79522a6817a135965f6c218b08e26f8
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Začínáme s Azure Data Lake Store pomocí Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Naučte se používat Azure PowerShell k vytvoření účtu Azure Data Lake Store a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Požadavky
Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 nebo vyšší**. Viz téma [Instalace a konfigurace prostředí Azure PowerShell](/powershell/azureps-cmdlets-docs).

## <a name="authentication"></a>Authentication
Tento článek používá jednodušší přístup k ověřování ve službě Data Lake Store, kdy jste vyzváni k zadání svých pověření pro účet Azure. Úroveň přístupu k účtu služby Data Lake Store a systému souborů se pak řídí úrovní přístupu přihlášeného uživatele. Existují však i jiné přístupy k ověřování ve službě Data Lake Store. Je to **ověřování koncového uživatele** nebo **ověřování služba-služba**. Pokyny a další informace o ověřování najdete v tématu [Ověření ve službě Data Lake Store pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

## <a name="create-an-azure-data-lake-store-account"></a>Vytvoření účtu Azure Data Lake Store
1. Otevřete na ploše nové okno Windows PowerShellu, zadejte následující fragment kódu a přihlaste se tak k účtu Azure. Nastavte předplatné a zaregistrujte poskytovatele Data Lake Store. Po zobrazení výzvy k přihlášení se nezapomeňte přihlásit jako jeden ze správců / vlastník předplatného:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Účet Azure Data Lake Store je přidružený ke skupině prostředků Azure. Začněte vytvořením skupiny prostředků Azure.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Vytvoření skupiny prostředků Azure](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Vytvoření skupiny prostředků Azure")
3. Vytvořte účet Azure Data Lake Store. Zadaný název musí obsahovat jenom malá písmena a číslice.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Vytvoření účtu Azure Data Lake Store](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Vytvoření účtu Azure Data Lake Store")
4. Ověřte, že se účet úspěšně vytvořil.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Výstup této položky musí být **True** (pravda).

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Vytváření struktur adresářů v Azure Data Lake Store
V rámci účtu Azure Data Lake Store můžete vytvářet adresáře, které slouží ke správě a ukládání dat.

1. Zadejte kořenový adresář.

        $myrootdir = "/"
2. V zadaném kořenovém adresáři vytvořte nový adresář s názvem **mynewdirectory**.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Ověřte, že se nový adresář úspěšně vytvořil.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Zobrazený výstup by měl vypadat asi takto:

    ![Ověření adresáře](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Ověření adresáře")

## <a name="upload-data-to-your-azure-data-lake-store"></a>Nahrání dat do Azure Data Lake Store
Data můžete do Data Lake Store nahrát přímo na úrovni kořenového adresáře nebo do adresáře, který jste v rámci účtu vytvořili. Níže zobrazené fragmenty kódu ukazují, jak nahrát ukázková data do adresáře (**mynewdirectory**), který jste vytvořili v předchozí části.

Pokud hledáte ukázková data, která byste mohli nahrát, můžete použít složku **Ambulance Data** z [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Stáhněte si tento soubor a uložte ho do místního adresáře v počítači, například C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Přejmenování, stažení a odstranění dat z Data Lake Store
Pokud chcete přejmenovat soubor, použijte tento příkaz:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Pokud chcete stáhnout soubor, použijte tento příkaz:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Pokud chcete odstranit soubor, použijte tento příkaz:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Po zobrazení výzvy zadejte **Y**, a položku tak odstraňte. Pokud chcete odstranit více souborů, můžete zadat všechny požadované cesty oddělené čárkou.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Odstranění účtu Azure Data Lake Store
Následujícím příkazem odstraňte účet Data Lake Store.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Po zobrazení výzvy zadejte **Y**, a účet tak odstraňte.

## <a name="performance-guidance-while-using-powershell"></a>Průvodce výkonem při použití prostředí PowerShell

V následující tabulce jsou nejdůležitější nastavení, která můžete ladit pro získání nejlepšího výkonu při práci se službou Data Lake Store pomocí prostředí PowerShell:

| Vlastnost            | Výchozí | Popis |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Pomocí tohoto parametru můžete zvolit počet paralelních vláken pro nahrávání nebo stahování jednotlivých souborů. Toto číslo představuje maximální počet vláken, která lze přidělit pro jednotlivé soubory, ale v závislosti na vašem scénáři můžete dostat méně vláken (například pokud nahráváte soubor o velikosti 1 kB, dostanete jedno vlákno, a to i když požadujete třeba 20 vláken).  |
| ConcurrentFileCount | 10      | Tento parametr je určený zejména pro nahrávání nebo stahování složek. Tento parametr určuje počet souborů, které lze souběžně nahrávat nebo stahovat. Toto číslo představuje maximální počet souborů, které lze souběžně v jednu chvíli nahrávat nebo stahovat, ale v závislosti na vašem scénáři můžete dostat nižší souběžnost (například pokud nahráváte dva soubory, dostanete dvě souběžná nahrávání souborů, a to i když požadujete třeba 15). |

**Příklad**

Tento příkaz stáhne soubory z Azure Data Lake Store na místní jednotku a použije k tomu 20 vláken na soubor a 100 souběžných souborů.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-the-value-to-set-for-these-parameters"></a>Jak určím hodnotu, kterou mám těmto parametrům nastavit?

Tady je několik rad, kterými se můžete řídit.

* **Krok 1: Určení celkového počtu vláken** – Měli byste začít výpočtem celkového počtu vláken, která se mají použít. Podle obecných pokynů byste měli použít 6 vláken na každé fyzické jádro.

        Total thread count = total physical cores * 6

    **Příklad**

    Předpokládejme, že spouštíte příkazy prostředí PowerShell z virtuálního počítače D14, který má 16 jader.

        Total thread count = 16 cores * 6 = 96 threads


* **Krok 2: Výpočet hodnoty PerFileThreadCount** – Hodnotu PerFileThreadCount vypočítáme na základě velikosti souborů. Pro soubory menší než 2,5 GB není nutné tento parametr měnit, výchozí hodnota 10 je postačující. Pro soubory větší než 2,5 GB byste měli použít jako základ 10 vláken pro prvních 2,5 GB a přidat 1 vlákno za každý další nárůst velikosti souboru o 256 MB. Pokud kopírujete složku se soubory velmi rozdílných velikostí, zvažte jejich seskupení podle podobných velikostí. Velmi rozdílné velikosti souborů mohou způsobit, že výkon nebude optimální. Pokud není možné seskupit soubory podle podobných velikostí, měli byste hodnotu PerFileThreadCount nastavit podle největší velikosti souboru.

        PerFileThreadCount = 10 threads for the first 2.5GB + 1 thread for each additional 256MB increase in file size

    **Příklad**

    Předpokládejme, že máte 100 souborů o velikosti od 1 GB až do 10 GB. Použijeme tedy 10GB jako největší velikost souboru a dosadíme ji do rovnice, která bude vypadat nějak takto.

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* **Krok 3: Výpočet hodnoty ConcurrentFilecount** – Vypočítejte hodnotu ConcurrentFileCount pomocí celkového počtu vláken a hodnoty PerFileThreadCount, které dosadíte do následující rovnice.

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Příklad**

    Podle ukázkových hodnot, které používáme:

        96 = 40 * ConcurrentFileCount

    Takže hodnota **ConcurrentFileCount** je **2.4**, což můžeme zaokrouhlit na **2**.

### <a name="further-tuning"></a>Další ladění

Možná budete vyžadovat další ladění, protože existuje velká škála velikostí souborů, se kterými můžete pracovat. Předchozí výpočty dobře fungují v případě, že se velikost všech nebo většiny souborů pohybuje okolo 10 GB. Pokud ale bude existovat mnoho různých velikostí souborů a mnoho jich bude menších, mohli byste hodnotu PerFileThreadCount snížit. Díky snížení hodnoty PerFileThreadCount můžeme zvýšit hodnotu ConcurrentFileCount. Takže pokud předpokládáme, že většina našich souborů je menších a pohybuje se okolo 5 GB, můžeme znovu provést výpočet:

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

Hodnota **ConcurrentFileCount** nyní tedy bude 96/20, což je 4,8 a po zaokrouhlení **4**.

Tato nastavení můžete dále ladit zvýšením nebo snížením hodnoty **PerFileThreadCount** v závislosti na rozložení velikostí souborů.

### <a name="limitation"></a>Omezení

* **Počet souborů je menší než hodnota ConcurrentFileCount**: Pokud je počet souborů, které nahráváte, menší než hodnota **ConcurrentFileCount**, kterou jste vypočítali, měli byste snížit hodnotu **ConcurrentFileCount** tak, aby se rovnala počtu souborů. Zbývající vlákna můžete použít ke zvýšení hodnoty **PerFileThreadCount**.

* **Příliš mnoho vláken**: Pokud příliš zvýšíte počet vláken a nezvětšíte velikost clusteru, riskujete tím snížení výkonu. Může docházet ke kolizím při přepínání kontextu na procesoru.

* **Nedostatečná souběžnost**: Pokud souběžnost není dostatečná, může to být způsobeno malou velikostí clusteru. Můžete zvýšit počet uzlů v clusteru a získat tak větší souběžnost.

* **Chyby omezování**: Pokud je souběžnost příliš vysoká, může docházet k chybám omezování. Pokud dochází k chybám omezování, měli byste buď snížit souběžnost, nebo nás kontaktovat.

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)


