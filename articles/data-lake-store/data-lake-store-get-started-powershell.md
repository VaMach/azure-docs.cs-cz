---
title: "Začínáme s Data Lake Storem | Dokumentace Microsoftu"
description: "Použití prostředí Azure PowerShell k vytvoření účtu Data Lake Store a provádění základních operací"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/21/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: c157da7bf53e2d0762624e8e71e56e956db04a24
ms.openlocfilehash: 7663670bc4fe0612b9a22545f0fc036add1c48cd


---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Začínáme s Azure Data Lake Store pomocí Azure PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Naučte se používat Azure PowerShell k vytvoření účtu Azure Data Lake Store a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Požadavky
Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 nebo vyšší**. Viz téma [Instalace a konfigurace prostředí Azure PowerShell](../powershell-install-configure.md).

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
   
    ![Vytvoření skupiny prostředků Azure](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")
3. Vytvořte účet Azure Data Lake Store. Zadaný název musí obsahovat jenom malá písmena a číslice.
   
        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"
   
    ![Vytvoření účtu Azure Data Lake Store](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")
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
   
    ![Ověření adresáře](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")

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

## <a name="next-steps"></a>Další kroky
* [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
* [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)




<!--HONumber=Nov16_HO4-->


