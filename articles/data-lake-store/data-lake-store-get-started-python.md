---
title: "Začínáme s Azure Data Lake Store pomocí sady Python SDK | Dokumentace Microsoftu"
description: "Přečtěte si, jak pomocí sady SDK pro Python pracovat s účty a systémem souborů Data Lake Store."
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 375a603360ac249fc1b08923a94c85652390a3fc
ms.contentlocale: cs-cz
ms.lasthandoff: 07/01/2017


---

<a id="get-started-with-azure-data-lake-store-using-python" class="xliff"></a>

# Začínáme s Azure Data Lake Store pomocí jazyka Python

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

Naučte se používat sadu SDK pro Python pro Azure Data Lake Store k provádění základních operací, jako je vytváření složek, nahrávání a stahování datových souborů atd. Další informace týkající se Data Lake najdete v tématu [Azure Data Lake Store](data-lake-store-overview.md).

<a id="prerequisites" class="xliff"></a>

## Požadavky

* **Python**. Python si můžete stáhnout [tady](https://www.python.org/downloads/). Tento článek používá Python verze 3.5.2.

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Vytvoření aplikace Azure Active Directory**. Aplikaci Azure AD použijete k ověření aplikace Data Lake Store ve službě Azure AD. Existují různé přístupy k ověřování ve službě Azure AD, jsou to **ověřování koncového uživatele** nebo **ověřování služba-služba**. Pokyny a další informace o ověřování najdete v tématu [Ověřování koncových uživatelů](data-lake-store-end-user-authenticate-using-active-directory.md) nebo [Ověřování služba-služba](data-lake-store-authenticate-using-active-directory.md).

<a id="install-the-modules" class="xliff"></a>

## Instalace modulů

Abyste mohli pracovat se službou Data Lake Store pomocí Pythonu, je nutné nainstalovat tři moduly.

* Modul `azure-mgmt-resource`. Zahrnuje moduly Azure pro Active Directory atd.
* Modul `azure-mgmt-datalake-store`. Zahrnuje operace správy účtů Azure Data Lake Store. Další informace o tomto modulu najdete v [referenčních informacích k modulu Azure Data Lake Store Management](http://azure-sdk-for-python.readthedocs.io/en/latest/sample_azure-mgmt-datalake-store.html).
* Modul `azure-datalake-store`. Zahrnuje operace se systémem souborů Azure Data Lake Store. Další informace o tomto modulu najdete v [referenčních informacích k modulu Azure Data Lake Store Filesystem](http://azure-datalake-store.readthedocs.io/en/latest/).

Pomocí následujících příkazů tyto moduly nainstalujte.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

<a id="create-a-new-python-application" class="xliff"></a>

## Vytvoření nové aplikace v Pythonu

1. Pomocí integrovaného vývojového prostředí (IDE) podle vašeho výběru vytvořte novou aplikaci v Pythonu, například **mysample.py**.

2. Přidáním následujících řádků importujte požadované moduly.

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Azure Data Lake Store account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Azure Data Lake Store filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Uložte změny v souboru mysample.py.

<a id="authentication" class="xliff"></a>

## Ověřování

V této části popíšeme různé způsoby, jak provádět ověření pomocí Azure AD. Dostupné jsou následující možnosti:

* Ověřování koncových uživatelů
* Ověřování služba-služba
* Ověřování pomocí služby Multi-Factor Authentication

Tyto možnosti ověřování je nutné použít jak pro správu účtů, tak i pro moduly správy systému souborů.

<a id="end-user-authentication-for-account-management" class="xliff"></a>

### Ověřování koncového uživatele pro správu účtu

Tento kód použijte k ověření ve službě Azure AD pro operace správy účtu (vytvoření/odstranění účtu Data Lake Store atd.). Je třeba zadat uživatelské jméno a heslo uživatele Azure AD. Upozorňujeme, že daný uživatel by neměl mít zapnuté vícefaktorové ověřování.

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

<a id="end-user-authentication-for-filesystem-operations" class="xliff"></a>

### Ověřování koncového uživatele pro operace se systémem souborů

Tento kód použijte k ověření pomocí Azure AD pro operace se systémem souborů (vytvoření složky, nahrání souboru atd.). Tento kód použijte se stávající aplikací **nativního klienta** Azure AD. Uživatel, pro kterého zadáváte přihlašovací údaje Azure AD, by neměl mít zapnuté vícefaktorové ověřování.

    tenant_id = 'FILL-IN-HERE'
    client_id = 'FILL-IN-HERE'
    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    token = lib.auth(tenant_id, user, password, client_id)

<a id="service-to-service-authentication-with-client-secret-for-account-management" class="xliff"></a>

### Ověřování služba-služba s tajným klíčem klienta pro správu účtu

Tento kód použijte k ověření ve službě Azure AD pro operace správy účtu (vytvoření/odstranění účtu Data Lake Store atd.). Následující fragment kódu lze použít k neinteraktivnímu ověřování vaší aplikace pomocí tajného klíče klienta pro aplikaci nebo instančního objektu. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

<a id="service-to-service-authentication-with-client-secret-for-filesystem-operations" class="xliff"></a>

### Ověřování služba-služba s tajným klíčem klienta pro operace se systémem souborů

Tento kód použijte k ověření pomocí Azure AD pro operace se systémem souborů (vytvoření složky, nahrání souboru atd.). Následující fragment kódu lze použít k neinteraktivnímu ověřování vaší aplikace pomocí tajného klíče klienta pro aplikaci nebo instančního objektu. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

    token = lib.auth(tenant_id = 'FILL-IN-HERE', client_secret = 'FILL-IN-HERE', client_id = 'FILL-IN-HERE')

<a id="multi-factor-authentication-for-account-management" class="xliff"></a>

### Vícefaktorové ověřování pro správu účtu

Tento kód použijte k ověření ve službě Azure AD pro operace správy účtu (vytvoření/odstranění účtu Data Lake Store atd.). Následující fragment kódu můžete použít k ověřování vaší aplikace pomocí vícefaktorového ověřování. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

    authority_host_url = "https://login.microsoftonline.com"
    tenant = "FILL-IN-HERE"
    authority_url = authority_host_url + '/' + tenant
    client_id = 'FILL-IN-HERE'
    redirect = 'urn:ietf:wg:oauth:2.0:oob'
    RESOURCE = 'https://management.core.windows.net/'
    
    context = adal.AuthenticationContext(authority_url)
    code = context.acquire_user_code(RESOURCE, client_id)
    print(code['message'])
    mgmt_token = context.acquire_token_with_device_code(RESOURCE, code, client_id)
    credentials = AADTokenCredentials(mgmt_token, client_id)

<a id="multi-factor-authentication-for-filesystem-management" class="xliff"></a>

### Vícefaktorové ověřování pro správu systému souborů

Tento kód použijte k ověření pomocí Azure AD pro operace se systémem souborů (vytvoření složky, nahrání souboru atd.). Následující fragment kódu můžete použít k ověřování vaší aplikace pomocí vícefaktorového ověřování. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

    token = lib.auth(tenant_id='FILL-IN-HERE')

<a id="create-an-azure-resource-group" class="xliff"></a>

## Vytvoření skupiny prostředků Azure

Pomocí následujícího fragmentu kódu vytvořte skupinu prostředků Azure:

    ## Declare variables
    subscriptionId= 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'
    
    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )
    
    ## Create an Azure Resource Group
    resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )

<a id="create-clients-and-data-lake-store-account" class="xliff"></a>

## Vytvoření klientů a účtu Data Lake Store

Následující fragment kódu nejprve vytvoří klienta účtu Data Lake Store. Objekt klienta použije k vytvoření účtu Data Lake Store. Nakonec vytvoří objekt klienta systému souborů.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'

    ## Create management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(credentials, subscriptionId)

    ## Create a Data Lake Store account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    ## Create a filesystem client object
    adlsFileSystemClient = core.AzureDLFileSystem(token, store_name=adlsAccountName)

<a id="list-the-data-lake-store-accounts" class="xliff"></a>

## Výpis účtů Data Lake Store

    ## List the existing Data Lake Store accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

<a id="create-a-directory" class="xliff"></a>

## Vytvoření adresáře

    ## Create a directory
    adlsFileSystemClient.mkdir('/mysampledirectory')

<a id="upload-a-file" class="xliff"></a>

## Nahrání souboru


    ## Upload a file
    multithread.ADLUploader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)


<a id="download-a-file" class="xliff"></a>

## Stažení souboru

    ## Download a file
    multithread.ADLDownloader(adlsFileSystemClient, lpath='C:\\data\\mysamplefile.txt.out', rpath='/mysampledirectory/mysamplefile.txt', nthreads=64, overwrite=True, buffersize=4194304, blocksize=4194304)

<a id="delete-a-directory" class="xliff"></a>

## Odstranění adresáře

    ## Delete a directory
    adlsFileSystemClient.rm('/mysampledirectory', recursive=True)

<a id="see-also" class="xliff"></a>

## Viz také

- [Zabezpečení dat ve službě Data Lake Store](data-lake-store-secure-data.md)
- [Použití Azure Data Lake Analytics se službou Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Použití Azure HDInsight se službou Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Referenční dokumentace sady SDK rozhraní .NET služby Data Lake Store](https://msdn.microsoft.com/library/mt581387.aspx)
- [Referenční dokumentace architektury REST služby Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx)

