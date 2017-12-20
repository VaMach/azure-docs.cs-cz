---
title: "REST API: Operace systému souborů v Azure Data Lake Store | Dokumentace Microsoftu"
description: "Použití rozhraní REST API WebHDFS k provádění operací systému souborů ve službě Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: 62cfc713c3b7ce8f7c0a7ff4dd5e80802f1843b7
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/18/2017
---
# <a name="filesystem-operations-on-azure-data-lake-store-using-rest-api"></a>Operace systému souborů v Azure Data Lake Store pomocí rozhraní REST API
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

V tomto článku se naučíte používat rozhraní REST API WebHDFS a rozhraní REST API pro Data Lake Store k provádění operací systému souborů v Azure Data Lake Store. Pokyny k provádění operací správy účtů ve službě Data Lake Store pomocí rozhraní REST API najdete v tématu [Operace správy účtů ve službě Data Lake Store pomocí rozhraní REST API](data-lake-store-get-started-rest-api.md).

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Účet Azure Data Lake Store**. Postupujte podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](data-lake-store-get-started-portal.md).

* **[cURL](http://curl.haxx.se/)**. Tento článek používá cURL k předvedení toho, jak provádět volání rozhraní REST API vůči účtu Data Lake Store.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak můžu ověřovat pomocí služby Azure Active Directory?
Ověřování pomocí služby Azure Active Directory můžete provádět dvěma přístupy.

* Pokud chcete ve své aplikaci ověřování koncového uživatele (interaktivní), přečtěte si téma [Ověřování koncového uživatele pomocí služby Data Lake Store s použitím sady .NET SDK](data-lake-store-end-user-authenticate-rest-api.md).
* Pokud chcete ve své aplikaci ověřování služba-služba (neinteraktivní), přečtěte si téma [Ověřování služba-služba pomocí služby Data Lake Store s použitím sady .NET SDK](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-folders"></a>Vytváření složek
Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS'

V předchozím příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve. Tento příkaz vytvoří adresář s názvem **mytempdir** v kořenové složce účtu Data Lake Store.

Pokud se operace úspěšně dokončí, měla by se zobrazit odpověď podobná následujícímu fragmentu kódu:

    {"boolean":true}

## <a name="list-folders"></a>Výpis složek
Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS'

V předchozím příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve.

Pokud se operace úspěšně dokončí, měla by se zobrazit odpověď podobná následujícímu fragmentu kódu:

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
            "owner": "<GUID>",
            "group": "<GUID>"
        }]
    }
    }

## <a name="upload-data"></a>Nahrání dat
Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -L -T 'C:\temp\list.txt' -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE'

V předchozí syntaxi je parametr **-T** umístění nahrávaného souboru.

Výstup je podobný následujícímu fragmentu kódu:
   
    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/list.txt?op=CREATE&write=true
    ...
    Content-Length: 0

    HTTP/1.1 100 Continue

    HTTP/1.1 201 Created
    ...

## <a name="read-data"></a>Čtení dat
Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Čtení dat z účtu Data Lake Store je proces, který obsahuje dva kroky.

* Nejdřív odešlete požadavek GET na koncový bod `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. Toto volání vrátí umístění, na které je nutné odeslat další požadavek GET.
* Potom odešlete požadavek GET na koncový bod `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Toto volání zobrazí obsah souboru.

Vstupní parametry v prvním a druhém kroku se ale nijak neliší, a proto můžete parametr `-L` použít k odeslání prvního požadavku. Možnost `-L` v podstatě kombinuje dva požadavky do jednoho. Výsledkem je, že cURL zopakuje požadavek na novém umístění. Nakonec se zobrazí výstup ze všech volání požadavků, který bude vypadat přibližně jako v následujícím fragmentu kódu. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN'

Zobrazený výstup by měl vypadat přibližně jako následující fragment kódu:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...

    HTTP/1.1 200 OK
    ...

    Hello, Data Lake Store user!

## <a name="rename-a-file"></a>Přejmenování souboru
Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Pokud chcete přejmenovat soubor, použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt'

Zobrazený výstup by měl vypadat přibližně jako následující fragment kódu:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="delete-a-file"></a>Odstranění souboru
Tato operace je založená na volání rozhraní REST API WebHDFS, které je definované [tady](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Pokud chcete odstranit soubor, použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" 'https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE'

Zobrazený výstup by měl vypadat asi takto:

    HTTP/1.1 200 OK
    ...

    {"boolean":true}

## <a name="next-steps"></a>Další kroky
* [Operace správy účtů ve službě Data Lake Store pomocí rozhraní REST API](data-lake-store-get-started-rest-api.md)

## <a name="see-also"></a>Viz také
* [Referenční informace k rozhraní REST API pro Azure Data Lake Store](https://docs.microsoft.com/rest/api/datalakestore/)
* [Aplikace typu Open Source pro velké objemy dat kompatibilní s Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)

