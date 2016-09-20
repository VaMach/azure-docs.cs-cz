<properties 
   pageTitle="Začínáme s Data Lake Store pomocí rozhraní REST API | Microsoft Azure" 
   description="Použití rozhraní REST API WebHDFS k provádění operací v Data Lake Store" 
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
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# Začínáme s Azure Data Lake Store pomocí rozhraní REST API

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API](data-lake-store-get-started-rest-api.md)
- [Azure CLI](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

V tomto článku se naučíte používat rozhraní REST API WebHDFS a rozhraní REST API Data Lake Store k provádění správy účtů a operací systému souborů v Azure Data Lake Store. Azure Data Lake Store zpřístupňuje vlastní rozhraní REST API pro operace správy účtů. Data Lake Store je nicméně kompatibilní s ekosystémy HDFS a Hadoop, a proto podporuje použití rozhraní REST API WebHDFS pro operace systému souborů.

>[AZURE.NOTE] Podrobné informace týkající se podpory rozhraní REST API pro Data Lake Store najdete v tématu [Referenční informace týkající se rozhraní REST API Azure Data Lake Store](https://msdn.microsoft.com/library/mt693424.aspx).

## Požadavky

- **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
- **Vytvoření aplikace Azure Active Directory**. Ověřování pomocí služby Azure Active Directory můžete provádět dvěma způsoby – **interaktivně** a **neinteraktivně**. Předpoklady se liší podle toho, jak chcete ověřovat.
    * **V případě interaktivního ověřování** (používá se v tomto článku) – Ve službě Azure Active Directory je nutné vytvořit **nativní klientskou aplikaci**. Po vytvoření aplikace načtěte následující hodnoty týkající se aplikace.
        - Získejte **ID klienta** a **identifikátor URI přesměrování** aplikace.
        - Nastavte delegovaná oprávnění.

    * **V případě neinteraktivního ověřování** – Ve službě Azure Active Directory je nutné vytvořit **webovou aplikaci**. Po vytvoření aplikace načtěte následující hodnoty týkající se aplikace.
        - Získejte **ID klienta**, **tajný klíč klienta** a **identifikátor URI přesměrování** aplikace.
        - Nastavte delegovaná oprávnění.
        - Přiřaďte aplikaci Azure Active Directory k roli. Tato role může být na úrovni rozsahu, ve kterém chcete aplikaci Azure Active Directory udělit oprávnění. Aplikaci můžete přiřadit například na úrovni předplatného nebo na úrovni skupiny prostředků. Pokyny najdete v tématu [Přiřazení aplikace k roli](../resource-group-create-service-principal-portal.md#assign-application-to-role). 

    Pokyny týkající se načtení těchto hodnot, nastavení oprávnění a přiřazování rolí najdete v tématu [Vytvoření aplikace Active Directory a objektu zabezpečení pomocí portálu](../resource-group-create-service-principal-portal.md).

- [cURL](http://curl.haxx.se/). Tento článek používá cURL k předvedení toho, jak provádět volání rozhraní REST API vůči účtu Data Lake Store.

## Jak můžu ověřovat pomocí služby Azure Active Directory?

Ověřování pomocí služby Azure Active Directory můžete provádět dvěma přístupy.

### Interaktivní (ověření uživatele)

V tomto scénáři aplikace vyzve uživatele k přihlášení a všechny operace se provádějí v kontextu uživatele. V případě interaktivního ověřování proveďte následující postup.

1. Prostřednictvím aplikace přesměrujte uživatele na tuto adresu URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<REDIRECT-URI> musí být zakódovaný, aby se dal použít jako adresa URL. Pro adresu https://localhost proto použijte `https%3A%2F%2Flocalhost`)

    Pro účely tohoto kurzu můžete ve výše zobrazené adrese URL nahradit zástupné hodnoty a vložit ji do adresního řádku webového prohlížeče. Budete přesměrováni na ověření pomocí přihlášení Azure. Po úspěšném přihlášení se v adresním řádku prohlížeče zobrazí odpověď. Odpověď bude mít tento formát:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Zaznamenejte autorizační kód z odpovědi. Pro účely tohoto kurzu můžete zkopírovat autorizační kód z adresního řádku webového prohlížeče a předat jej v požadavku POST koncovému bodu tokenu, jak vidíte níže:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] V takovém případě nemusí být identifikátor \<REDIRECT-URI> zakódovaný.

3. Odpovědí je objekt JSON, který obsahuje přístupový token (např. `"access_token": "<ACCESS_TOKEN>"`) a obnovovací token (např. `"refresh_token": "<REFRESH_TOKEN>"`). Aplikace používá přístupový token při přístupu k Azure Data Lake Store, zatímco obnovovací token používá k získání dalšího přístupového tokenu, když přístupovému tokenu vyprší platnost.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Když vyprší platnost přístupového tokenu, můžete pomocí obnovovacího tokenu požádat o nový přístupový token, jak vidíte níže:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Další informace o interaktivním ověřování uživatelů najdete v tématu [Tok poskytování autorizačních kódů](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### Neinteraktivní

V tomto scénáři aplikace poskytuje svoje vlastní přihlašovací údaje k provedení operací. V tomto případě musíte vydat požadavek POST podobný tomu, který vidíte níže. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Výstup tohoto požadavku bude obsahovat autorizační token (níže ve výstupu označený jako `access-token`), který potom budete předávat s voláními rozhraní REST API. Tento ověřovací token si uložte do textového souboru, protože ho budete později v tomto článku potřebovat.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

Tento článek používá **neinteraktivní** přístup. Další informace o neinteraktivním přístupu (volání služba-služba) najdete v tématu [Volání služba-služba pomocí přihlašovacích údajů](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## Vytvoření účtu Data Lake Store

Tato operace je založená na volání rozhraní REST API, které je definované [tady](https://msdn.microsoft.com/library/mt694078.aspx).

Použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Ve výše uvedeném příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve. Datová část požadavku tohoto příkazu se nachází v souboru **input.json** poskytnutém pro parametr `-d` výše. Obsah souboru input.json vypadá přibližně takto:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## Vytváření složek v účtu Data Lake Store

Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

Ve výše uvedeném příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve. Tento příkaz vytvoří adresář s názvem **mytempdir** v kořenové složce účtu Data Lake Store.

Po úspěšném dokončení operace se zobrazí takováto odpověď:

    {"boolean":true}

## Zobrazení seznamu složek v účtu Data Lake Store

Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

Ve výše uvedeném příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve.

Po úspěšném dokončení operace se zobrazí takováto odpověď:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## Nahrání dat do účtu Data Lake Store

Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Nahrání dat pomocí rozhraní REST API WebHDFS je proces obsahující dva kroky, které jsou popsané níže.

1. Odešlete požadavek HTTP PUT bez odeslání dat souboru, který chcete nahrát. V následujícím příkazu nahraďte položku **\<yourstorename >** názvem Data Lake Store.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Výstup tohoto příkazu bude obsahovat dočasnou adresu URL přesměrování podobnou té, kterou vidíte níže.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Teď musíte odeslat další požadavek HTTP PUT na adresu URL, která byla v odpovědi uvedená ve vlastnosti **Location**. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Výstup se bude podobat tomuto:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## Čtení dat z účtu Data Lake Store

Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Čtení dat z účtu Data Lake Store je proces, který obsahuje dva kroky.

* Nejdřív odešlete požadavek GET na koncový bod `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Vrátí se umístění, na které je nutné odeslat další požadavek GET.
* Potom odešlete požadavek GET na koncový bod `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Tím se zobrazí obsah souboru.

Vstupní parametry v prvním a druhém kroku se ale nijak neliší, a proto můžete parametr `-L` použít k odeslání prvního požadavku. `-L` je možnost, která v podstatě kombinuje dva požadavky do jednoho a způsobí, že cURL zopakuje požadavek na novém umístění. Nakonec se zobrazí výstup ze všech volání požadavků, který bude vypadat přibližně tak, jak vidíte níže. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Zobrazený výstup by měl vypadat přibližně takto:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## Přejmenování souboru v účtu Data Lake Store

Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Pokud chcete přejmenovat soubor, použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Zobrazený výstup by měl vypadat přibližně takto:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## Odstranění souboru z účtu Data Lake Store

Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Pokud chcete odstranit soubor, použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Zobrazený výstup by měl vypadat asi takto:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## Odstranění účtu Data Lake Store

Tato operace je založená na volání rozhraní REST API, které je definované [tady](https://msdn.microsoft.com/library/mt694075.aspx).

Pokud chcete odstranit účet Data Lake Store, použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Zobrazený výstup by měl vypadat asi takto:

    HTTP/1.1 200 OK
    ...
    ...

## Viz také

- [Aplikace typu Open Source pro velké objemy dat kompatibilní s Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)
 



<!--HONumber=sep16_HO2-->


