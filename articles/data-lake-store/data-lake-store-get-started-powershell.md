<properties
   pageTitle="Začínáme s Data Lake Store | Azure"
   description="Použití prostředí Azure PowerShell k vytvoření účtu Data Lake Store a provádění základních operací"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="04/07/2016"
   ms.author="nitinme"/>

# Začínáme s Azure Data Lake Store pomocí Azure PowerShell

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat Azure PowerShell k vytvoření účtu Azure Data Lake Store a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).

## Předpoklady

Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:

- **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
- **Aktivujte předplatné Azure** pro verzi Public Preview služby Data Lake Store. Viz [pokyny](data-lake-store-get-started-portal.md#signup).


##Instalace prostředí Azure PowerShell 1.0 nebo vyšší

Viz část Předpoklady v tématu [Použití Azure PowerShellu s Azure Resource Managerem](../powershell-azure-resource-manager.md#prerequisites).

## Vytvoření účtu Azure Data Lake Store

1. Otevřete na ploše nové okno Azure PowerShell a zadáním následujícího fragmentu kódu se přihlaste k účtu Azure, nastavte předplatné a zaregistrujte poskytovatele Data Lake Store. Po zobrazení výzvy k přihlášení se nezapomeňte přihlásit jako jeden ze správců / vlastník předplatného:

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

2. Vytvořte účet Azure Data Lake Store. Zadaný název musí obsahovat jenom malá písmena a číslice.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Vytvoření účtu Azure Data Lake Store](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")

3. Ověřte, že se účet úspěšně vytvořil.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Výstup této položky musí být **True** (pravda).

## Vytváření struktur adresářů v Azure Data Lake Store

V rámci účtu Azure Data Lake Store můžete vytvářet adresáře, které slouží ke správě a ukládání dat.

1. Zadejte kořenový adresář.

        $myrootdir = "/"

2. V zadaném kořenovém adresáři vytvořte nový adresář s názvem **mynewdirectory**.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Ověřte, že se nový adresář úspěšně vytvořil.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Zobrazený výstup by měl vypadat asi takto:

    ![Ověření adresáře](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")


## Nahrání dat do Azure Data Lake Store

Data můžete do Data Lake Store nahrát přímo na úrovni kořenového adresáře nebo do adresáře, který jste v rámci účtu vytvořili. Níže zobrazené fragmenty kódu ukazují, jak nahrát ukázková data do adresáře (**mynewdirectory**), který jste vytvořili v předchozí části.

Pokud hledáte ukázková data, která byste mohli nahrát, můžete použít složku **Ambulance Data** z [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Stáhněte si tento soubor a uložte ho do místního adresáře v počítači, například C:\sampledata.\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## Přejmenování, stažení a odstranění dat z Data Lake Store

Pokud chcete přejmenovat soubor, použijte tento příkaz:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Pokud chcete stáhnout soubor, použijte tento příkaz:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Pokud chcete odstranit soubor, použijte tento příkaz:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Po zobrazení výzvy zadejte **Y**, a položku tak odstraňte. Pokud chcete odstranit více souborů, můžete zadat všechny požadované cesty oddělené čárkou.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## Odstranění účtu Azure Data Lake Store

Následujícím příkazem odstraňte účet Data Lake Store.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Po zobrazení výzvy zadejte **Y**, a účet tak odstraňte.


## Další kroky

- [Zabezpečení dat v Data Lake Store](data-lake-store-secure-data.md)
- [Použití Azure Data Lake Analytics s Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Použití Azure HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=Jun16_HO2-->


