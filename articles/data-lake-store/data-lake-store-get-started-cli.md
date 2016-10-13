<properties
   pageTitle="Začínáme s Data Lake Store pomocí multiplatformního rozhraní příkazového řádku | Microsoft Azure"
   description="Použití multiplatformního rozhraní příkazového řádku Azure k vytvoření účtu Data Lake Store a provádění základních operací"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="nitinme"/>


# Začínáme s Azure Data Lake Store pomocí příkazového řádku Azure

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat rozhraní příkazového řádku Azure k vytvoření účtu Azure Data Lake Store a provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů, odstranění účtu atd. Další informace týkající se Data Lake Store najdete v tématu [Přehled Data Lake Store](data-lake-store-overview.md).

Rozhraní příkazového řádku Azure je implementované v Node.js. Dá se použít na jakékoli platformě, která podporuje Node.js, včetně systému Windows, Mac a Linux. Rozhraní příkazového řádku Azure je typu Open Source. Zdrojový kód je spravován na webu GitHub na adrese <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. Tento článek se týká jenom používání rozhraní příkazového řádku Azure se službou Data Lake Store. Obecné informace týkající se používání rozhraní příkazového řádku Azure najdete v tématu [Používání rozhraní příkazového řádku Azure] [nástroje-příkazového-řádku-azure].


##Požadavky

Je nutné, abyste před zahájením tohoto článku měli tyto položky:

- **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

- **Rozhraní příkazového řádku Azure** – Informace týkající se instalace a konfigurace najdete v tématu [Instalace a konfigurace rozhraní příkazového řádku Azure](../xplat-cli-install.md). Po instalaci rozhraní příkazového řádku nezapomeňte restartovat počítač.

## Authentication

Tento článek využívá jednodušší přístup ověřování ve službě Data Lake Store, kdy se přihlašujete jako koncový uživatel. Úroveň přístupu k účtu služby Data Lake Store a systému souborů se pak řídí úrovní přístupu přihlášeného uživatele. Existují však i jiné přístupy k ověřování ve službě Data Lake Store. Je to **ověřování koncového uživatele** nebo **ověřování služba-služba**. Pokyny a další informace o ověřování najdete v tématu [Ověření ve službě Data Lake Store pomocí služby Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

##Přihlášení k předplatnému Azure

1. Postupujte podle kroků popsaných v tématu [Připojení k předplatnému Azure z rozhraní příkazového řádku Azure](../xplat-cli-connect.md) a připojte se k předplatnému pomocí metody `azure login`.

2. Pomocí příkazu `azure account list` zobrazte seznam předplatných spojených s vaším účtem.

        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false

    Ve výstupu uvedeném výše je aktuálně povoleno předplatné **Azure-sub-1**. Druhým předplatným je **Azure-sub-2**. 

3. Vyberte předplatné, v rámci kterého chcete pracovat. Pokud chcete pracovat v rámci předplatného Azure-sub-2, použijte příkaz `azure account set`.

        azure account set Azure-sub-2


## Vytvoření účtu Azure Data Lake Store

Otevřete příkazový řádek, prostředí nebo relaci terminálové služby a spusťte následující příkazy.

2. Přepněte do režimu Azure Resource Manager pomocí následujícího příkazu:

        azure config mode arm


5. Vytvořte novou skupinu prostředků. V následujícím příkazu zadejte hodnoty parametrů, které chcete použít.

        azure group create <resourceGroup> <location>

    Pokud název umístění obsahuje mezery, dejte ho do uvozovek. Například „East US 2“.

5. Vytvořte účet Data Lake Store.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## Vytváření složek v Data Lake Store

V rámci účtu Azure Data Lake Store můžete vytvářet složky, které slouží ke správě a ukládání dat. Následujícím příkazem vytvořte v kořenovém adresáři Data Lake Store složku s názvem „mynewfolder“.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Příklad:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## Nahrání dat do Data Lake Store

Data můžete do Data Lake Store nahrát přímo na úrovni kořenového adresáře nebo do složky, kterou jste v rámci účtu vytvořili. Níže zobrazené fragmenty kódu ukazují, jak nahrát ukázková data do složky (**mynewfolder**), kterou jste vytvořili v předchozí části.

Pokud hledáte ukázková data, která byste mohli nahrát, můžete použít složku **Ambulance Data** z [úložiště Git Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Stáhněte si tento soubor a uložte ho do místního adresáře v počítači, například C:\sampledata.\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Příklad:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## Zobrazení seznamu souborů v Data Lake Store

Následujícím příkazem zobrazte seznam souborů v účtu Data Lake Store.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Příklad:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Výstup by měl vypadat přibližně takto:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## Přejmenování, stažení a odstranění dat z Data Lake Store

* **Pokud chcete přejmenovat soubor**, použijte tento příkaz:

        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>

    Příklad:

        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv

* **Pokud chcete stáhnout soubor**, použijte tento příkaz: Ujistěte se, že zadaná cílová cesta už existuje.

        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>

    Příklad:

        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"

* **Pokud chcete odstranit soubor**, použijte tento příkaz:

        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>

    Příklad:

        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv

    Po zobrazení výzvy zadejte **Y**, a položku tak odstraňte.

## Zobrazení seznamu řízení přístupu pro složku v Data Lake Store

Následujícím příkazem zobrazte seznamy ACL ve složce Data Lake Store. V aktuální verzi se dají seznamy ACL nastavit jenom v kořenovém adresáři Data Lake Store. Níže uvedený parametr proto bude vždycky kořenový adresář (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Příklad:

    azure datalake store permissions show mynewdatalakestore /


## Odstranění účtu Data Lake Store

Následujícím příkazem odstraňte účet Data Lake Store.

    azure datalake store account delete <dataLakeStoreAccountName>

Příklad:

    azure datalake store account delete mynewdatalakestore

Po zobrazení výzvy zadejte **Y**, a účet tak odstraňte.


## Další kroky

- [Zabezpečení dat v Data Lake Store](data-lake-store-secure-data.md)
- [Použití Azure Data Lake Analytics s Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Použití Azure HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)


[nástroje-příkazového-řádku-azure]: ../xplat-cli-install.md



<!--HONumber=Sep16_HO5-->


