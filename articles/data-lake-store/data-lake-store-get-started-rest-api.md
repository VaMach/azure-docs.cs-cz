---
title: "REST API: Operace správy účtů v Azure Data Lake Store | Dokumentace Microsoftu"
description: "Použití Azure Data Lake Store a rozhraní REST API WebHDFS k provádění operací správy účtů ve službě Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 57ac6501-cb71-4f75-82c2-acc07c562889
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/28/2017
ms.author: nitinme
ms.openlocfilehash: 6c43f2b341280731707e486ba6f22f11560102c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="account-management-operations-on-azure-data-lake-store-using-rest-api"></a>Operace správy účtů v Azure Data Lake Store pomocí rozhraní REST API
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

V tomto článku se dozvíte, jak provádět operace správy účtů ve službě Data Lake Store pomocí rozhraní REST API. Mezi operace správy účtů patří vytvoření účtu Data Lake Store, odstranění účtu Data Lake Store atd. Pokyny k provádění operací systému souborů ve službě Data Lake Store pomocí rozhraní REST API najdete v tématu [Operace systému souborů ve službě Data Lake Store pomocí rozhraní REST API](data-lake-store-data-operations-rest-api.md).

## <a name="prerequisites"></a>Požadavky
* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).

* **[cURL](http://curl.haxx.se/)**. Tento článek používá cURL k předvedení toho, jak provádět volání rozhraní REST API vůči účtu Data Lake Store.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak můžu ověřovat pomocí služby Azure Active Directory?
Ověřování pomocí služby Azure Active Directory můžete provádět dvěma přístupy.

* Pokud chcete ve své aplikaci ověřování koncového uživatele (interaktivní), přečtěte si téma [Ověřování koncového uživatele pomocí služby Data Lake Store s použitím sady .NET SDK](data-lake-store-end-user-authenticate-rest-api.md).
* Pokud chcete ve své aplikaci ověřování služba-služba (neinteraktivní), přečtěte si téma [Ověřování služba-služba pomocí služby Data Lake Store s použitím sady .NET SDK](data-lake-store-service-to-service-authenticate-rest-api.md).


## <a name="create-a-data-lake-store-account"></a>Vytvoření účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API, které je definované [tady](https://msdn.microsoft.com/library/mt694078.aspx).

Použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

Ve výše uvedeném příkazu nahraďte položku \<`REDACTED`\> autorizačním tokenem, který jste získali dříve. Datová část požadavku tohoto příkazu se nachází v souboru **input.json** poskytnutém pro parametr `-d` výše. Obsah souboru input.json vypadá přibližně jako následující fragment kódu:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }    

## <a name="delete-a-data-lake-store-account"></a>Odstranění účtu Data Lake Store
Tato operace je založená na volání rozhraní REST API, které je definované [tady](https://msdn.microsoft.com/library/mt694075.aspx).

Pokud chcete odstranit účet Data Lake Store, použijte následující příkaz cURL. Položku **\<yourstorename>** nahraďte názvem Data Lake Store.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Zobrazený výstup by měl vypadat přibližně jako následující fragment kódu:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="next-steps"></a>Další kroky
* [Operace systému souborů ve službě Data Lake Store pomocí rozhraní REST API](data-lake-store-data-operations-rest-api.md)

## <a name="see-also"></a>Viz také
* [Referenční informace k rozhraní REST API pro Azure Data Lake Store](https://docs.microsoft.com/rest/api/datalakestore/)
* [Aplikace typu Open Source pro velké objemy dat kompatibilní s Azure Data Lake Store](data-lake-store-compatible-oss-other-applications.md)

