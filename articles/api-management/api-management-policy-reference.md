---
title: "Zásady Azure API managementu"
description: "Další informace o zásadách dostupné pro konfiguraci API Management."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 9d4ac609-b221-4032-93ae-e27a1254d43d
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: adc0c4415e10ddd0b4994cecef17f026546e91a1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-api-management-policy-reference"></a>Zásady Azure API managementu
Tato část obsahuje index pro zásady v [zásady API managementu][API Management policy reference]. Informace o přidávání a konfiguraci zásad najdete v tématu [zásady ve službě API Management][Policies in API Management].

Výrazy zásad můžete použít jako hodnoty atributů nebo textové hodnoty v libovolných zásadách API Management (pokud zásady neurčí jinak). Některé zásady, jako [řízení toku] [ Control flow] a [nastavená proměnná] [ Set variable] jsou založené na výrazech zásad. Další informace najdete v tématu [pokročilé zásady] [ Advanced policies] a [výrazy zásad][Policy expressions]

## <a name="policy-reference-index"></a>Rejstřík referenčních informací o zásadách
* [Zásady omezení přístupu][Access restriction policies]
  * [Kontrola HTTP záhlaví] [ Check HTTP header] -vynucuje existence nebo hodnoty hlavičky protokolu HTTP.
  * [Omezení četnosti volání podle předplatného] [ Limit call rate by subscription] -špičky využití brání rozhraní API omezením četnosti volání, na základě za předplatné.
  * [Omezení četnosti volání podle klíče](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) -špičky využití brání rozhraní API omezením četnosti volání, na základě na klíč.
  * [Omezit volající IP adresy] [ Restrict caller IPs] -filtry (umožňuje nebo zakazuje) volání z konkrétní IP adresy a rozsahy adres.
  * [Nastavení kvóty využití podle předplatného] [ Set usage quota by subscription] -umožňuje vynucovat obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě za předplatné.
  * [Nastavení kvóty využití podle klíče](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) -umožňuje vynucovat obnovitelných nebo doba života volání svazku nebo šířky pásma kvótu, na základě na klíč.
  * [Ověření JWT] [ Validate JWT] -vynucuje existence a platnosti token JWT extrahovány ze zadaného záhlaví HTTP nebo parametr zadaný dotaz.
* [Rozšířené zásady][Advanced policies]
  * [Řízení toku] [ Control flow] – podmíněně platí příkazy zásad na základě výsledků vyhodnocení logická hodnota [výrazy][expressions].
  * [Předat dál žádost] [ Forward request] -předá požadavek back-end službu.
  * [Protokol do centra událostí] [ Log to Event Hub] -odešle zprávy v zadaném formátu do zprávy Cíl definované [protokolovacího nástroje](https://msdn.microsoft.com/library/azure/mt592020.aspx#Logger) entity.
  * [Opakujte](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Retry) -opakování provádění příkazů závorkách zásad, pokud a dokud nebude splněna podmínka. Spuštění se opakovaly zadaným časovým intervalům a až zadaný počet opakování.
  * [Vrátí odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) -přerušení způsobených kanálu provádění a vrátí zadanou odpověď přímo na volajícího.
  * [Odeslání žádosti o jednorázové jednoduché](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) -odešle požadavek na zadanou adresu URL bez čekání na odpověď.
  * [Odeslání požadavku](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) -odešle požadavek na zadanou adresu URL.
  * [Nastaví metodu požadavku](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetRequestMethod) -vám umožní změnit metodu protokolu HTTP pro žádost.
  * [Nastavte stav](https://msdn.microsoft.com/library/azure/dn894085.aspx#SetStatus) -změní stavový kód protokolu HTTP se zadanou hodnotou.
  * [Nastavená proměnná] [ Set variable] -zachovat hodnotu v pojmenovaná [kontextu] [ context] proměnná pro pozdější přístup.
  * [Trasování](https://msdn.microsoft.com/en-us/library/dn894085.aspx#Trace) -přidá řetězec do [rozhraní API Inspector](api-management-howto-api-inspector.md) výstup.
  * [Počkejte](https://msdn.microsoft.com/library/azure/dn894085.aspx#Wait) -počká pro závorkách odeslání žádosti o, Get hodnota z mezipaměti, nebo řídit tok zásady k dokončení než budete pokračovat.
* [Zásady ověřování][Authentication policies]
  * [Ověřování Basic] [ Authenticate with Basic] -ověření pomocí back-end službu pomocí základního ověřování.
  * [Ověřování pomocí certifikátu klienta] [ Authenticate with client certificate] -ověření pomocí back-end službu pomocí klientských certifikátů.
* [Zásady ukládání do mezipaměti][Caching policies] 
  * [Získat z mezipaměti] [ Get from cache] -provést mezipaměti vyhledat a vrátit platnou odpověď uložená v mezipaměti pokud je k dispozici.
  * [Úložiště pro mezipaměť] [ Store to cache] -ukládá do mezipaměti odpovědi podle konfigurace zadané mezipaměti ovládacího prvku.
  * [Získat hodnotu z mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) -načíst položky v mezipaměti podle klíče.
  * [Uložení v mezipaměti hodnoty](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) -uložit položky do mezipaměti podle klíče.
  * [Odebrat hodnotu z mezipaměti](https://msdn.microsoft.com/en-us/library/dn894086.aspx#RemoveCacheByKey) -odebrání položky v mezipaměti podle klíče.
* [Různé zásady domény][Cross domain policies] 
  * [Povolit volání mezi doménami] [ Allow cross-domain calls] -zpřístupní rozhraní API z Adobe Flash a Microsoft Silverlight založené na prohlížeči klientů.
  * [CORS] [ CORS] – přidává podporu (CORS), aby operace nebo rozhraní API umožňující volání napříč doménami založené na prohlížeči klientů pro sdílení prostředků různého původu.
  * [JSONP] [ JSONP] -přidá JSON s podporou odsazení (JSONP) operace nebo rozhraní API, chcete-li povolit volání mezi doménami z klientů JavaScript založené na prohlížeči.
* [Zásad transformace][Transformation policies] 
  * [Převést JSON do XML] [ Convert JSON to XML] – převede požadavku nebo odpovědi textu z JSON do XML.
  * [Převést na JSON XML] [ Convert XML to JSON] – převede požadavku nebo odpovědi textu z XML do formátu JSON.
  * [Najít a nahradit řetězec v textu] [ Find and replace string in body] – najde dílčí řetězec požadavku nebo odpovědi a nahradí je jiný dílčí řetězec.
  * [Maskování adresy URL v obsahu] [ Mask URLs in content] -znovu zapíše (masky) odkazy v odpovědi body tak, aby ukazovaly na ekvivalentní propojení prostřednictvím brány.
  * [Nastavení back-end služby] [ Set backend service] -změny službě back-end pro příchozí žádosti.
  * [Tělo nastavit] [ Set body] -nastaví obsah zprávy pro příchozí a odchozí požadavky.
  * [Set – hlavička protokolu HTTP] [ Set HTTP header] – přiřazuje hodnotu existující odpovědi nebo hlavička požadavku nebo přidá hlavičku odpovědi nebo žádost o nový.
  * [Nastavte parametr řetězce dotazu] [ Set query string parameter] – přidá, nahradí hodnotu nebo odstraní parametr řetězce dotazu požadavku.
  * [Přepsání adresy URL] [ Rewrite URL] – převede adresu URL požadavku z jeho veřejné formuláře do formuláře očekává webovou službou.

## <a name="next-steps"></a>Další kroky
Další informace o výrazů zásad najdete v následujícím videu.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Access restriction policies]: https://msdn.microsoft.com/library/azure/dn894078.aspx
[Check HTTP header]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#CheckHTTPHeader
[Limit call rate by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#LimitCallRate
[Restrict caller IPs]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#RestrictCallerIPs
[Set usage quota by subscription]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#SetUsageQuota
[Validate JWT]: https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx
[context]: https://msdn.microsoft.com/library/azure/ea160028-fc04-4782-aa26-4b8329df3448#ContextVariables
[Forward request]: https://msdn.microsoft.com/library/azure/dn894085.aspx#ForwardRequest
[Log to Event Hub]: https://msdn.microsoft.com/library/azure/dn894085.aspx#log-to-eventhub

[Authentication policies]: https://msdn.microsoft.com/library/azure/dn894079.aspx
[Authenticate with Basic]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#Basic
[Authenticate with client certificate]: https://msdn.microsoft.com/library/azure/061702a7-3a78-472b-a54a-f3b1e332490d#ClientCertificate
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
[Get from cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#GetFromCache
[Store to cache]: https://msdn.microsoft.com/library/azure/8147199c-24d8-439f-b2a9-da28a70a890c#StoreToCache

[Cross domain policies]: https://msdn.microsoft.com/library/azure/dn894084.aspx
[Allow cross-domain calls]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#AllowCrossDomainCalls
[CORS]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#CORS
[JSONP]: https://msdn.microsoft.com/library/azure/7689d277-8abe-472a-a78c-e6d4bd43455d#JSONP

[Transformation policies]: https://msdn.microsoft.com/library/azure/dn894083.aspx
[Convert JSON to XML]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertJSONtoXML
[Convert XML to JSON]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#ConvertXMLtoJSON
[Find and replace string in body]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#Findandreplacestringinbody
[Mask URLs in content]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#MaskURLSContent
[Set backend service]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetBackendService
[Set body]: https://msdn.microsoft.com/library/azure/dn894083.aspx#SetBody
[Set HTTP header]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetHTTPheader
[Set query string parameter]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#SetQueryStringParameter
[Rewrite URL]: https://msdn.microsoft.com/library/azure/7406a8ce-5f9c-4fae-9b0f-e574befb2ee9#RewriteURL



[Policies in API Management]: api-management-howto-policies.md
[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx

[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx


