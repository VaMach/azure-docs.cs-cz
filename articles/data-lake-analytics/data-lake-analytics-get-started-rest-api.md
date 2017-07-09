---
title: "Začínáme se službou Data Lake Analytics pomocí rozhraní REST API | Dokumentace Microsoftu"
description: "Použití rozhraní REST API WebHDFS k provádění operací ve službě Data Lake Analytics"
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.assetid: 5e133d92-baaa-44c9-890c-ab2d85c91122
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: jgao
ms.translationtype: Human Translation
ms.sourcegitcommit: a1ba750d2be1969bfcd4085a24b0469f72a357ad
ms.openlocfilehash: 332d7af2539eea8890745005104ac5b0921c2b7f
ms.contentlocale: cs-cz
ms.lasthandoff: 06/20/2017


---
# <a name="get-started-with-azure-data-lake-analytics-using-rest-apis"></a>Začínáme se službou Azure Data Lake Analytics pomocí rozhraní REST API
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Zjistěte, jak používat rozhraní REST API WebHDFS a rozhraní REST API služby Data Lake Analytics ke správě účtů, úloh a katalogu služby Data Lake Analytics. 

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Vytvoření aplikace Azure Active Directory**. Aplikaci Azure AD použijete k ověření aplikace Data Lake Analytics ve službě Azure AD. Existují různé přístupy k ověřování ve službě Azure AD, jsou to **ověřování koncového uživatele** nebo **ověřování služba-služba**. Pokyny a další informace o ověřování najdete v tématu [Ověřování pomocí služby Data Lake Analytics s využitím Azure Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).
* [cURL](http://curl.haxx.se/). Tento článek používá cURL k předvedení toho, jak provádět volání rozhraní REST API vůči účtu Data Lake Analytics.

## <a name="authenticate-with-azure-active-directory"></a>Ověřování pomocí Azure Active Directory
Existují dvě metody ověřování pomocí Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Ověření koncového uživatele (interaktivní)
Když použijete tuto metodu, aplikace vyzve uživatele k přihlášení a všechny operace se provádějí v kontextu uživatele. 

Pokud chcete zavést interaktivní ověřování, postupujte následovně:

1. Prostřednictvím aplikace přesměrujte uživatele na tuto adresu URL:
   
        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>
   
   > [!NOTE]
   > \<REDIRECT-URI> musí být zakódovaný, aby se dal použít jako adresa URL. Pro adresu https://localhost proto použijte zápis `https%3A%2F%2Flocalhost`).
   > 
   > 
   
    Pro účely tohoto kurzu můžete ve výše zobrazené adrese URL nahradit zástupné hodnoty a vložit ji do adresního řádku webového prohlížeče. Budete přesměrováni na ověření pomocí přihlášení Azure. Po úspěšném přihlášení se v adresním řádku prohlížeče zobrazí odpověď. Odpověď bude mít tento formát:
   
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>
2. Zaznamenejte autorizační kód z odpovědi. Pro účely tohoto kurzu můžete zkopírovat autorizační kód z adresního řádku webového prohlížeče a předat jej v požadavku POST koncovému bodu tokenu, jak vidíte níže:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>
   
   > [!NOTE]
   > V takovém případě nemusí být identifikátor \<REDIRECT-URI> zakódovaný.
   > 
   > 
3. Odpovědí je objekt JSON, který obsahuje přístupový token (např. `"access_token": "<ACCESS_TOKEN>"`) a obnovovací token (např. `"refresh_token": "<REFRESH_TOKEN>"`). Aplikace používá přístupový token při přístupu k Azure Data Lake Store, zatímco obnovovací token používá k získání dalšího přístupového tokenu, když přístupovému tokenu vyprší platnost.
   
        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before":    "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}
4. Když vyprší platnost přístupového tokenu, můžete pomocí obnovovacího tokenu požádat o nový přístupový token, jak vidíte níže:
   
        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
             -F grant_type=refresh_token \
             -F resource=https://management.core.windows.net/ \
             -F client_id=<CLIENT-ID> \
             -F refresh_token=<REFRESH-TOKEN>

Další informace o interaktivním ověřování uživatelů najdete v tématu [Tok poskytování autorizačních kódů](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Ověřování služba-služba (neinteraktivní)
Když použijete tuto metodu, aplikace poskytuje své vlastní přihlašovací údaje k provedení operací. V tomto případě musíte vydat požadavek POST podobný tomu, který vidíte níže: 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Výstup tohoto požadavku bude obsahovat autorizační token (níže ve výstupu označený jako `access-token`), který potom budete předávat s voláními rozhraní REST API. Tento ověřovací token si uložte do textového souboru, protože ho budete později v tomto článku potřebovat.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Tento článek používá **neinteraktivní** přístup. Další informace o neinteraktivním přístupu (volání služba-služba) najdete v tématu [Volání služba-služba pomocí přihlašovacích údajů](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-analytics-account"></a>Vytvoření účtu Data Lake Analytics
Před vytvořením účtu Data Lake Analytics je nutné vytvořit skupinu prostředků Azure a účet Data Lake Store.  Viz [Vytvoření účtu Data Lake Store](../data-lake-store/data-lake-store-get-started-rest-api.md#create-a-data-lake-store-account).

Následující příkaz cURL znázorňuje, jak vytvořit účet:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<NewAzureDataLakeAnalyticsAccountName>?api-version=2016-11-01 -d@"C:\tutorials\adla\CreateDataLakeAnalyticsAccountRequest.json"

Nahraďte \<`REDACTED`\> autorizačním tokenem, \<`AzureSubscriptionID`\> svým ID předplatného, \<`AzureResourceGroupName`\> názvem existující skupiny prostředků Azure a \<`NewAzureDataLakeAnalyticsAccountName`\> názvem nového účtu Data Lake Analytics. Datová část požadavku tohoto příkazu se nachází v souboru **CreateDatalakeAnalyticsAccountRequest.json** poskytnutém pro parametr `-d` výše. Obsah souboru input.json vypadá přibližně takto:

    {  
        "location": "East US 2",  
        "name": "myadla1004",  
        "tags": {},  
        "properties": {  
            "defaultDataLakeStoreAccount": "my1004store",  
            "dataLakeStoreAccounts": [  
                {  
                    "name": "my1004store"  
                }     
            ]
        }  
    }  


## <a name="list-data-lake-analytics-accounts-in-a-subscription"></a>Zobrazení seznamu účtů Data Lake Analytics v předplatném
Následující příkaz cURL znázorňuje, jak zobrazit seznam účtů v předplatném:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/providers/Microsoft.DataLakeAnalytics/Accounts?api-version=2016-11-01

Nahraďte \<`REDACTED`\> autorizačním tokenem a \<`AzureSubscriptionID`\> svým ID předplatného. Výstup je podobný tomuto:

    {
        "value": [
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla0831.azuredatalakeanalytics.net",
                "accountId": "21e74660-0941-4880-ae72-b143c2615ea9",
                "creationTime": "2016-09-01T12:49:12.7451428Z",
                "lastModifiedTime": "2016-09-01T12:49:12.7451428Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla0831rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla0831",
            "name": "myadla0831",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            },
            {
            "properties": {
                "provisioningState": "Succeeded",
                "state": "Active",
                "endpoint": "myadla1004.azuredatalakeanalytics.net",
                "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
                "creationTime": "2016-10-04T20:46:42.287147Z",
                "lastModifiedTime": "2016-10-04T20:46:42.287147Z"
            },
            "location": "East US 2",
            "tags": {},
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
            "name": "myadla1004",
            "type": "Microsoft.DataLakeAnalytics/accounts"
            }
        ]
    }

## <a name="get-information-about-a-data-lake-analytics-account"></a>Získání informací o účtu Data Lake Analytics
Následující příkaz cURL znázorňuje, jak získat informace o účtu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>?api-version=2015-11-01

Nahraďte \<`REDACTED`\> autorizačním tokenem, \<`AzureSubscriptionID`\> svým ID předplatného, \<`AzureResourceGroupName`\> názvem existující skupiny prostředků Azure a \<`DataLakeAnalyticsAccountName`\> názvem existujícího účtu Data Lake Analytics. Výstup je podobný tomuto:

    {
        "properties": {
            "defaultDataLakeStoreAccount": "my1004store",
            "dataLakeStoreAccounts": [
            {
                "properties": {
                "suffix": "azuredatalakestore.net"
                },
                "name": "my1004store"
            }
            ],
            "provisioningState": "Creating",
            "state": null,
            "endpoint": null,
            "accountId": "3ff9b93b-11c4-43c6-83cc-276292eeb350",
            "creationTime": null,
            "lastModifiedTime": null
        },
        "location": "East US 2",
        "tags": {},
        "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004",
        "name": "myadla1004",
        "type": "Microsoft.DataLakeAnalytics/accounts"
    }

## <a name="list-data-lake-stores-of-a-data-lake-analytics-account"></a>Zobrazení seznamu služeb Data Lake Store pro účet Data Lake Analytics
Následující příkaz cURL znázorňuje, jak zobrazit seznam služeb Data Lake Store pro účet:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/<AzureSubscriptionID>/resourceGroups/<AzureResourceGroupName>/providers/Microsoft.DataLakeAnalytics/accounts/<DataLakeAnalyticsAccountName>/DataLakeStoreAccounts/?api-version=2016-11-01

Nahraďte \<`REDACTED`\> autorizačním tokenem, \<`AzureSubscriptionID`\> svým ID předplatného, \<`AzureResourceGroupName`\> názvem existující skupiny prostředků Azure a \<`DataLakeAnalyticsAccountName`\> názvem existujícího účtu Data Lake Analytics. Výstup je podobný tomuto:

    {
        "value": [
            {
            "properties": {
                "suffix": "azuredatalakestore.net"
            },
            "id": "/subscriptions/65a1016d-0f67-45d2-b838-b8f373d6d52e/resourceGroups/myadla1004rg/providers/Microsoft.DataLakeAnalytics/accounts/myadla1004/dataLakeStoreAccounts/my1004store",
            "name": "my1004store",
            "type": "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts"
            }
        ]
    }

## <a name="submit-u-sql-jobs"></a>Odesílání úloh U-SQL
Následující příkaz cURL znázorňuje, jak odeslat úlohu U-SQL:

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs/<NewGUID>?api-version=2016-03-20-preview -d@"C:\tutorials\adla\SubmitADLAJob.json"

Nahraďte \<`REDACTED`\> autorizačním tokenem a \<`DataLakeAnalyticsAccountName`\> názvem existujícího účtu Data Lake Analytics. Datová část požadavku tohoto příkazu se nachází v souboru **SubmitADLAJob.json** poskytnutém pro parametr `-d` výše. Obsah souboru input.json vypadá přibližně takto:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "properties": {
            "type": "USql",
            "script": "@searchlog =\n    EXTRACT UserId          int,\n            Start           DateTime,\n            Region          string,\n            Query          
        string,\n            Duration        int?,\n            Urls            string,\n            ClickedUrls     string\n    FROM \"/Samples/Data/SearchLog.tsv\"\n    US
        ING Extractors.Tsv();\n\nOUTPUT @searchlog   \n    TO \"/Output/SearchLog-from-Data-Lake.csv\"\nUSING Outputters.Csv();"
        }
    }

Výstup je podobný tomuto:

    {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "myadl@SPI",
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "2016-10-05T13:54:59.9871859+00:00",
        "state": "Compiling",
        "result": "Succeeded",
        "stateAuditRecords": [
            {
            "newState": "New",
            "timeStamp": "2016-10-05T13:54:59.9871859+00:00",
            "details": "userName:myadl@SPI;submitMachine:N/A"
            }
        ],
        "properties": {
            "owner": "myadl@SPI",
            "resources": [],
            "runtimeVersion": "default",
            "rootProcessNodeId": "00000000-0000-0000-0000-000000000000",
            "algebraFilePath": "adl://myadls0831.azuredatalakestore.net/system/jobservice/jobs/Usql/2016/10/05/13/54/8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a/algebra.xml",
            "compileMode": "Semantic",
            "errorSource": "Unknown",
            "totalCompilationTime": "PT0S",
            "totalPausedTime": "PT0S",
            "totalQueuedTime": "PT0S",
            "totalRunningTime": "PT0S",
            "type": "USql"
        }
    }


## <a name="list-u-sql-jobs"></a>Zobrazení seznamu úloh U-SQL
Následující příkaz cURL znázorňuje, jak zobrazit seznam úloh U-SQL:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/Jobs?api-version=2016-11-01 

Nahraďte \<`REDACTED`\> autorizačním tokenem a \<`DataLakeAnalyticsAccountName`\> názvem existujícího účtu Data Lake Analytics. 

Výstup je podobný tomuto:

    {
    "value": [
        {
        "jobId": "65cf1691-9dbe-43cd-90ed-1cafbfb406fb",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someone@microsoft.com",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:46:53 GMT",
        "startTime": "Wed, 05 Oct 2016 13:47:33 GMT",
        "endTime": "Wed, 05 Oct 2016 13:48:07 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        },
        {
        "jobId": "8f8ebf8c-4b63-428a-ab46-a03d2cc5b65a",
        "name": "convertTSVtoCSV",
        "type": "USql",
        "submitter": "someoneadl@SPI",
        "account": null,
        "degreeOfParallelism": 1,
        "priority": 1000,
        "submitTime": "Wed, 05 Oct 2016 13:54:59 GMT",
        "startTime": "Wed, 05 Oct 2016 13:55:43 GMT",
        "endTime": "Wed, 05 Oct 2016 13:56:11 GMT",
        "state": "Ended",
        "result": "Succeeded",
        "errorMessage": null,
        "storageAccounts": null,
        "stateAuditRecords": null,
        "logFilePatterns": null,
        "properties": null
        }
    ],
    "nextLink": null,
    "count": null
    }


## <a name="get-catalog-items"></a>Získání položek katalogu
Následující příkaz cURL znázorňuje, jak získat databáze z katalogu:

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<DataLakeAnalyticsAccountName>.azuredatalakeanalytics.net/catalog/usql/databases?api-version=2016-11-01

Výstup je podobný tomuto:

    {
    "@odata.context":"https://myadla0831.azuredatalakeanalytics.net/sqlip/$metadata#databases","value":[
        {
        "computeAccountName":"myadla0831","databaseName":"mytest","version":"f6956327-90b8-4648-ad8b-de3ff09274ea"
        },{
        "computeAccountName":"myadla0831","databaseName":"master","version":"e8bca908-cc73-41a3-9564-e9bcfaa21f4e"
        }
    ]
    }

## <a name="see-also"></a>Viz také
* Pokud chcete zobrazit komplexnější dotaz, přejděte k tématu [Analýza webových protokolů pomocí Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Pokud chcete začít s vývojem aplikací U-SQL, přejděte k tématu [Vývoj skriptů U-SQL pomocí nástrojů Data Lake pro Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Pokud se chcete naučit jazyk U-SQL, informace najdete v tématu [Začínáme s jazykem U-SQL Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informace týkající se úloh správy najdete v tématu [Správa služby Azure Data Lake Analytics pomocí webu Azure Portal](data-lake-analytics-manage-use-portal.md).
* Přehled Data Lake Analytics najdete v tématu [Přehled Azure Data Lake Analytics](data-lake-analytics-overview.md).
* Pokud chcete použít jiné podporované nástroje a zobrazit stejný kurz, klikněte na selektory karet v horní části stránky.


