---
title: "Přehled rozhraní REST API služby Media Services operations | Microsoft Docs"
description: "Přehled Media Services REST API"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a5f1c5e7-ec52-4e26-9a44-d9ea699f68d9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: eada8f2bcd2488d5ed36b0c24aa3d1b917815517
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="media-services-operations-rest-api-overview"></a>Přehled rozhraní REST API operations Media Services
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

**Media Services operace REST** rozhraní API slouží k vytvoření úlohy, prostředky, zásady přístupu a další operace na objekty v účtu služby Media Service. Další informace najdete v tématu [Media Services operace REST API odkaz](https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference).

Microsoft Azure Media Services je služba, která přijímá požadavky HTTP na základě OData a zpátky v podrobné JSON nebo atom + pub může reagovat. Vzhledem k tomu, že vyhovuje pokynů pro návrh Azure Media Services, je sada požadované hlavičky HTTP, které každý klient musí použít při připojování ke službě Media Services, jakož i sadu volitelné hlavičky, které lze použít. Následující části popisují hlavičky a příkazy HTTP, můžete použít při vytváření žádostí o a příjem odpovědí ze služby Media Services.

Toto téma poskytuje přehled o tom, jak používat REST v2 pomocí služby Media Services.

## <a name="considerations"></a>Požadavky

Při používání REST, platí následující aspekty.

* Při dotazování entity, existuje omezení 1000 entit vrátí najednou, protože veřejné v2 REST omezí výsledky dotazu a 1000 výsledky. Budete muset použít **přeskočit** a **trvat** (.NET) nebo **horní** (REST), jak je popsáno v [v tomto příkladu .NET](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) a [v tomto příkladu REST API](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). 
* Při použití JSON a určení pro použití **__metadata** – klíčové slovo v žádosti o (například k odkazuje na objekt odkazovaný) musíte nastavit **přijmout** hlavičky k [JSON podrobný formát](http://www.odata.org/documentation/odata-version-3-0/json-verbose-format/) (viz následující příklad). OData nerozumí **__metadata** vlastnost v požadavku, pokud je nastavena na podrobné.  
  
        POST https://media.windows.net/API/Jobs HTTP/1.1
        Content-Type: application/json;odata=verbose
        Accept: application/json;odata=verbose
        DataServiceVersion: 3.0
        MaxDataServiceVersion: 3.0
        x-ms-version: 2.11
        Authorization: Bearer <token> 
        Host: media.windows.net
  
        {
            "Name" : "NewTestJob", 
            "InputMediaAssets" : 
                [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aba5356eb-30ff-4dc6-9e5a-41e4223540e7')"}}]
        . . . 

## <a name="standard-http-request-headers-supported-by-media-services"></a>Standardní hlavičky požadavku HTTP podporovaných službou Media Services
Pro každé volání, které provedete ve službě Media Services je sada požadované hlavičky, které je nutné zahrnout vaši žádost a také sadu volitelné záhlaví může chcete zahrnout. Následující tabulka uvádí požadované hlavičky:

| Záhlaví | Typ | Hodnota |
| --- | --- | --- |
| Autorizace |Nosiče |Nosiče se jedná o jediné autorizační přijatý mechanismus. Hodnota musí obsahovat také přístupový token poskytovaný službou ACS. |
| x-ms-version |Decimal |2.11 |
| DataServiceVersion |Decimal |3.0 |
| MaxDataServiceVersion |Decimal |3.0 |

> [!NOTE]
> Protože Media Services používá ke zveřejnění své základní úložiště asset metadat prostřednictvím rozhraní API REST OData, DataServiceVersion a MaxDataServiceVersion hlavičky by měl být součástí každá žádost; ale pokud tomu tak není, pak aktuálně Media Services předpokládá, že hodnota DataServiceVersion používán je 3.0.
> 
> 

Toto je sada volitelné hlavičky:

| Záhlaví | Typ | Hodnota |
| --- | --- | --- |
| Datum |RFC 1123 datum |Časové razítko požadavku |
| Přijmout |Typ obsahu |Požadovaný typ obsahu pro odpověď, například následující:<p> -application/json; odata = verbose<p> -application/atom + xml<p> Odpovědí může mít jiný typ obsahu, jako je například načtení objektu blob, kde úspěšné odpovědi bude obsahovat datový proud objektu blob jako datovou část. |
| Přijměte kódování |GZIP, deflate |GZIP a DEFLATE kódování, v případě potřeby. Poznámka: Pro velké prostředky Media Services může ignorovat tuto hlavičku a vrátit nekomprimované data. |
| Přijměte jazyk |"en", "es" a tak dále. |Určuje upřednostňovaný jazyk pro odpověď. |
| Accept-Charset |Znaková sada typu like "Znakové sady UTF-8" |Výchozí hodnota je UTF-8. |
| Metoda HTTP X- |Metoda HTTP |Umožňuje klientům nebo brány firewall, které nepodporují metod HTTP PUT nebo odstranění používat tyto metody tunelovým propojením prostřednictvím volání GET. |
| Content-Type |Typ obsahu |Typ obsahu žádosti v žádosti PUT nebo POST obsahu. |
| Client-request-id |Řetězec |Volající definovanou hodnotu, který identifikuje daného požadavku. -Li zadána, bude tato hodnota zahrnuté ve zprávě s odpovědí jako způsob mapování požadavku. <p><p>**Důležité upozornění**<p>Hodnoty musí být limitován 2096b (2 kB). |

## <a name="standard-http-response-headers-supported-by-media-services"></a>Hlavičky standardních odpovědí HTTP podporovaných službou Media Services
Následuje sadu hlaviček, které mohou být vráceny vám v závislosti na prostředek, který se požaduje a akci, kterou byste chtěli provést.

| Záhlaví | Typ | Hodnota |
| --- | --- | --- |
| id žádosti |Řetězec |Jedinečný identifikátor pro aktuální operaci, vygeneruje služby. |
| Client-request-id |Řetězec |Identifikátor určeného volající v původní žádost, pokud je k dispozici. |
| Datum |RFC 1123 datum |Datum, kdy byl požadavek zpracovat. |
| Content-Type |Je to různé. |Typ obsahu textu odpovědi. |
| Kódování obsahu |Je to různé. |Gzip a deflate podle potřeby. |

## <a name="standard-http-verbs-supported-by-media-services"></a>Standardní příkazy HTTP podporovaných službou Media Services
Následuje úplný seznam příkazů HTTP, které lze použít při vytváření HTTP požadavků:

| Příkaz | Popis |
| --- | --- |
| GET |Vrací aktuální hodnotu objektu. |
| POST |Vytvoří objekt na základě dat poskytuje nebo odešle příkaz. |
| PUT |Nahradí objekt nebo vytvoří objekt s názvem (v případě potřeby). |
| ODSTRANIT |Odstraní objekt. |
| SLOUČENÍ |Aktualizuje existující objekt s názvem vlastnosti změny. |
| HEAD |Vrátí metadata objektu pro GET odpověď. |

## <a name="discover-media-services-model"></a>Zjistit modelu Media Services
Chcete-li zjistitelná entity Media Services, lze operaci $metadata. Umožňuje načíst všechny typy entit platný, vlastností entity, přidružení, funkce, akce a tak dále. Následující příklad ukazuje, jak vytvořit identifikátor URI: https://media.windows.net/API/$ metadat.

By měl připojit "? api-version=2.x" na konec identifikátor URI, pokud chcete v prohlížeči zobrazit metadata nebo nebudou obsahovat hlavičku x-ms-version ve vaší žádosti.

## <a name="connect-to-media-services"></a>Připojení ke službě Media Services

Informace o tom, jak připojit k rozhraní API pro AMS najdete v tématu [přístup k Azure Media Services API pomocí ověřování Azure AD](media-services-use-aad-auth-to-access-ams-api.md). Po úspěšném připojení k https://media.windows.net, obdržíte 301 přesměrování zadání jiném identifikátoru URI Media Services. Je nutné provést následující volání nový identifikátor URI.

## <a name="next-steps"></a>Další kroky

Pro přístup k rozhraní API pro AMS se zbytkem, najdete v části [ověřování pomocí služby Azure AD pro přístup k rozhraní API Azure Media Services se zbytkem](media-services-rest-connect-with-aad.md).

## <a name="media-services-learning-paths"></a>Mapy kurzů ke službě Media Services
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Poskytnutí zpětné vazby
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

