---
title: "Začínáme s Azure Data Lake Analytics pomocí Pythonu | Dokumentace Microsoftu"
description: "Zjistěte, jak pomocí Pythonu vytvořit účet Data Lake Store a odesílat úlohy. "
services: data-lake-analytics
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/15/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: 5618650671badfc54860c3ad8af5d1e727d3d8c9
ms.openlocfilehash: 40ccfc59cccd86a7634ec89656571b3cd23566b4


---


# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-python"></a>Kurz: Začínáme s Azure Data Lake Analytics pomocí Pythonu
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Naučte se používat Python k vytváření účtů Azure Data Lake Analytics, definování úloh Data Lake Analytics v [U-SQL](data-lake-analytics-u-sql-get-started.md) a odesílání úloh do účtů Data Lake Analytics. Další informace o Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).

V tomto kurzu vyvinete úlohu, která načte soubor hodnot oddělených tabulátorem (TSV) a převede ho na soubor hodnot oddělených čárkami (CSV). Pokud chcete použít jiné podporované nástroje a absolvovat stejný kurz, klikněte na karty nahoře v této části.

##<a name="prerequisites"></a>Požadavky

Před zahájením tohoto kurzu musíte mít tyto položky:

- **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
- **Aplikace Azure Active Directory**. Aplikaci Azure AD použijete k ověření aplikace Data Lake Store ve službě Azure AD. Existují různé přístupy k ověřování ve službě Azure AD, jsou to ověřování koncového uživatele nebo ověřování služba-služba. Pokyny a další informace o ověřování najdete v tématu [Ověření ve službě Data Lake Store pomocí služby Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).
- **[Python](https://www.python.org/downloads/)**. Je nutné použít 64bitovou verzi. Tento článek používá Python verze 3.5.2.


## <a name="install-azure-python-sdk"></a>Instalace sady Azure Python SDK

Abyste mohli pracovat se službou Data Lake Store pomocí Pythonu, je nutné nainstalovat tři moduly.

Modul azure-mgmt-datalake-store zahrnuje operace správy účtů Azure Data Lake Store. Modul azure-mgmt-resource zahrnuje další moduly Azure pro Active Directory atd. Modul azure-datalake-store zahrnuje operace se systémem souborů Azure Data Lake Store. Modul azure-datalake-analytics zahrnuje operace Azure Data Lake Analytics. Pomocí následujících příkazů tyto moduly nainstalujte.

    pip install azure-mgmt-resource
    pip install azure-mgmt-datalake-store
    pip install azure-mgmt-datalake-analytics
    pip install azure-datalake-store

## <a name="create-a-python-application"></a>Vytvoření aplikace v Pythonu

1. Pomocí integrovaného vývojového prostředí (IDE) podle vašeho výběru vytvořte novou aplikaci v Pythonu, například mysample.py.

2. Přidáním následujících řádků importujte požadované moduly.

        ## Use this only for Azure AD service-to-service authentication
        from azure.common.credentials import ServicePrincipalCredentials

        ## Use this only for Azure AD end-user authentication
        from azure.common.credentials import UserPassCredentials

        ## Required for Azure Resource Manager
        from azure.mgmt.resource.resources import ResourceManagementClient
        from azure.mgmt.resource.resources.models import ResourceGroup

        ## Required for Azure Data Lake Store account management
        from azure.mgmt.datalake.store.account import DataLakeStoreAccountManagementClient
        from azure.mgmt.datalake.store.account.models import DataLakeStoreAccount

        ## Required for Azure Data Lake Store filesystem management
        from azure.datalake.store import core, lib, multithread

        ## Required for Azure Data Lake Analytics account management
        from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeAnalyticsAccountProperties, DataLakeStoreAccountInfo

        ## Required for Azure Data Lake Analytics job management
        from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
        from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

        ## Required for Azure Data Lake Analytics catalog management
        from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

        ## Use these as needed for your application
        import logging, getpass, pprint, uuid, time

3. Uložte změny souboru aplikace v Pythonu.

## <a name="authentication"></a>Ověřování

K ověření použijte jednu z následujících metod:

### <a name="end-user-authentication-for-account-management"></a>Ověřování koncového uživatele pro správu účtu

Tuto metodu použijte k ověření ve službě Azure AD pro operace správy účtu (vytvoření/odstranění účtu Data Lake Store atd.). Je třeba zadat uživatelské jméno a heslo uživatele Azure AD. Uživatelský účet nesmí být nakonfigurovaný pro vícefaktorové ověřování a nesmí se jednat o účet Microsoft ani Live ID, například @outlook.com, nebo @live.com.

    user = input('Enter the user to authenticate with that has permission to subscription: ')
    password = getpass.getpass()

    credentials = UserPassCredentials(user, password)

### <a name="service-to-service-authentication-with-client-secret-for-account-management"></a>Ověřování služba-služba s tajným klíčem klienta pro správu účtu

Tuto metodu použijte k ověření ve službě Azure AD pro operace správy účtu (vytvoření/odstranění účtu Data Lake Store atd.). Následující fragment kódu lze použít k neinteraktivnímu ověřování vaší aplikace pomocí tajného klíče klienta pro aplikaci nebo instančního objektu. Použijte tento fragment kódu se stávající aplikací Azure AD Webová aplikace.

    credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')

## <a name="create-resource-management-group"></a>Vytvoření skupiny správy prostředků

Pomocí následujícího fragmentu kódu vytvořte skupinu prostředků Azure:

    ## Declare variables
    subscriptionId= '<Azure Subscription ID>'
    resourceGroup = '<Azure Resource Group Name>'
    location = '<Location>' # i.e. 'eastus2'

    ## Create management client object
    resourceClient = ResourceManagementClient(
        credentials,
        subscriptionId
    )

    ## Create an Azure Resource Group
    armGroupResult = resourceClient.resource_groups.create_or_update(
        resourceGroup,
        ResourceGroup(
            location=location
        )
    )


## <a name="create-data-lake-store-account"></a>Vytvoření účtu Data Lake Store

Každý účet Data Lake Analytics vyžaduje účet Data Lake Store. Pokyny najdete v části [Vytvoření účtu Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account).



## <a name="create-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics

    ## Declare variables
    subscriptionId= '<Azure Subscription ID>'
    resourceGroup = '<Azure Resource Group Name>'
    location = '<Location>' # i.e. 'eastus2'
    adlsAccountName = '<Azure Data Lake Store Account Name>'
    adlaAccountName = '<Azure Data Lake Analytics Account Name>'

    ## Create management client object
    adlaAcctClient = DataLakeAnalyticsAccountManagementClient(
        credentials,
        subscriptionId
    )

    ## Create an Azure Data Lake Analytics account
    adlaAcctResult = adlaAcctClient.account.create(
        resourceGroup,
        adlaAccountName,
        DataLakeAnalyticsAccount(
            location=location,
            default_data_lake_store_account=adlsAccountName,
            data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adlsAccountName)]
        )
    ).wait()


##<a name="submit-data-lake-analytics-jobs"></a>Odesílání úloh Data Lake Analytics

Úlohy Data Lake Analytics se píšou v jazyce U-SQL. Další informace o U-SQL najdete v tématu [Začínáme s jazykem U-SQL](data-lake-analytics-u-sql-get-started.md) a [Referenční informace pro jazyk U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

    ## Declare variables
    adlsAccountName = '<Azure Data Lake Store Account Name>'

    ## Create management client object
    adlaJobClient = DataLakeAnalyticsJobManagementClient(
        credentials,
        'azuredatalakeanalytics.net'
    )

    ## Submit a U-SQL job
    jobId = str(uuid.uuid4())
    jobResult = adlaJobClient.job.create(
        adlaAccountName,
        jobId,
        JobInformation(
            name='Sample Job',
            type='USql',
            properties=USqlJobProperties(
                script='DROP DATABASE IF EXISTS FOO; CREATE DATABASE FOO;'
            )
        )
    )

    ## Print the job result
    while(jobResult.state != JobState.ended):
        print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
        time.sleep(3)
        jobResult = adlaJobClient.job.get(adlaAccountName, jobId)
        
    print ('Job finished with result: ' + jobResult.result.value)

## <a name="next-steps"></a>Další kroky

- Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.
- Pokud chcete zobrazit komplexnější dotaz, přejděte k tématu [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
- Pokud chcete začít s vývojem aplikací U-SQL, přejděte k tématu [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
- Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).
- Přehled Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).




<!--HONumber=Nov16_HO4-->


