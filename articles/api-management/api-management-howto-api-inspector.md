---
title: "Trasování volání pomocí rozhraní API Inspector - Azure API Management | Microsoft Docs"
description: "Zjistěte, jak pro trasování volání s využitím Inspector rozhraní API v Azure API Management."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 4b222327-c8a4-4f33-9a06-adff2a9834d9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: a9d4d3be7f046af975f6dc25670070204848588c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-api-inspector-to-trace-calls-in-azure-api-management"></a>Jak používat nástroj Inspector rozhraní API pro trasování volání v Azure API Management
API Management představuje nástroj Inspector rozhraní API vám pomohou při ladění a řešení potíží s vaše rozhraní API. Rozhraní API Inspector je možné programově a můžete taky použít přímo z portálu pro vývojáře. 

Kromě trasování operací, nástroj Inspector rozhraní API také sleduje [výraz zásady](https://msdn.microsoft.com/library/azure/dn910913.aspx) hodnocení. Ukázka, najdete v části [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) a rychlé převíjení vpřed na 21:00.

Tato příručka poskytuje návod pomocí rozhraní API Inspector.

> [!NOTE]
> Trasování API Inspector pouze generované a k dispozici pro požadavků, které obsahují klíče předplatné, které patří do [správce](api-management-howto-create-groups.md) účtu.
> 
> 

## <a name="trace-call"></a> Inspector pomocí rozhraní API pro trasování volání
Použít kontrola rozhraní API, přidejte **ocp-apim trasování: true** žádosti hlavičky k volání operace a pak stáhnout a prohlédnout trasování pomocí adresy URL uvedené **ocp apim trasování umístění** odpovědi hlavička. To lze provést programově a můžete také provést přímo z portálu pro vývojáře.

Tento kurz ukazuje, jak použít kontrola rozhraní API pro trasování operací pomocí základní rozhraní API kalkulačky, který je nakonfigurovaný v [Správa vašeho prvního rozhraní API](api-management-get-started.md) kurz Začínáme. Pokud jste nedokončili tohoto kurzu trvá jenom pár chvil importovat rozhraní API základní kalkulačky, nebo můžete použít jiné API dle vlastního výběru, jako je například Echo API. Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním API programu Echo, které můžete použít k experimentování a seznámení se službou API Management. Echo API vrátí zpět ať vstup je do něj odeslané. Pokud chcete použít, můžete vyvolat všechny akce HTTP a návratová hodnota bude jednoduše jste poslali. 

Chcete-li začít, klikněte na tlačítko **portál pro vývojáře** služby API Management na portálu Azure. Operace lze volat přímo z portálu pro vývojáře, která představuje pohodlný způsob pro zobrazení a testování operací v rozhraní API.

> Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si téma [vytvoření instance API Management] [ Create an API Management service instance] v [Začínáme s Azure API Management] [ Get started with Azure API Management] kurzu.
> 
> 

![Portál pro vývojáře API Management][api-management-developer-portal-menu]

Klikněte na tlačítko **rozhraní API** z horní nabídce a pak klikněte na tlačítko **Basic Calculator**.

![Echo API][api-management-api]

Klikněte na tlačítko **vyzkoušet** pokusit **přidat dvě celá čísla** operaci.

![Vyzkoušet][api-management-open-console]

Ponechte výchozí hodnoty parametrů a vyberte klíč předplatného pro produkt, kterou chcete použít z **klíč předplatného** rozevíracího seznamu.

Ve výchozím nastavení v portálu pro vývojáře **Ocp-Apim-trasování** záhlaví je již nastaven na **true**. Tuto hlavičku konfiguruje, zda se vygeneruje trasování.

![Odeslat][api-management-http-get]

Klikněte na tlačítko **odeslat** k vyvolání operace.

![Odeslat][api-management-send-results]

V odpovědi bude hlavičky **ocp apim trasování umístění** s hodnotou podobně jako v následujícím příkladu.

```
ocp-apim-trace-location : https://contosoltdxw7zagdfsprykd.blob.core.windows.net/apiinspectorcontainer/ZW3e23NsW4wQyS-SHjS0Og2-2?sv=2013-08-15&sr=b&sig=Mgx7cMHsLmVDv%2B%2BSzvg3JR8qGTHoOyIAV7xDsZbF7%2Bk%3D&se=2014-05-04T21%3A00%3A13Z&sp=r&verify_guid=a56a17d83de04fcb8b9766df38514742
```

Trasování je možné stáhnout ze zadaného umístění a zkontrolovat, jak je předvedeno v dalším kroku. Upozorňujeme, že pouze poslední položky 100 protokolu se ukládají a jsou v otočení opakovaně umístění protokolu. Pokud provedete více než 100 volání s povoleným sledováním nakonec začnete přepsal první trasování na místě.

## <a name="inspect-trace"></a>Zkontrolovat trasování
Chcete-li zkontrolovat hodnoty v trasování, stáhněte soubor trasování z **ocp apim trasování umístění** adresy URL. Je textový soubor ve formátu JSON a obsahuje položky, podobně jako v následujícím příkladu.

```json
{
    "traceId": "abcd8ea63d134c1fabe6371566c7cbea",
    "traceEntries": {
        "inbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0725926",
                "data": {
                    "request": {
                        "method": "GET",
                        "url": "https://contoso5.azure-api.net/calc/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "Connection",
                                "value": "Keep-Alive"
                            },
                            {
                                "name": "Host",
                                "value": "contoso5.azure-api.net"
                            }
                        ]
                    }
                }
            },
            {
                "source": "mapper",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0726213",
                "data": {
                    "configuration": {
                        "api": {
                            "from": "/calc",
                            "to": {
                                "scheme": "http",
                                "host": "calcapi.cloudapp.net",
                                "port": 80,
                                "path": "/api",
                                "queryString": "",
                                "query": {},
                                "isDefaultPort": true
                            }
                        },
                        "operation": {
                            "method": "GET",
                            "uriTemplate": "/add?a={a}&b={b}"
                        },
                        "user": {
                            "id": 1,
                            "groups": [
                                "Administrators",
                                "Developers"
                            ]
                        },
                        "product": {
                            "id": 1
                        }
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.2998610Z",
                "elapsed": "00:00:00.0727522",
                "data": {
                    "message": "Request is being forwarded to the backend service.",
                    "request": {
                        "method": "GET",
                        "url": "http://calcapi.cloudapp.net/api/add?a=51&b=49",
                        "headers": [
                            {
                                "name": "Ocp-Apim-Subscription-Key",
                                "value": "5d7c41af64a44a68a2ea46580d271a59"
                            },
                            {
                                "name": "X-Forwarded-For",
                                "value": "33.52.215.35"
                            }
                        ]
                    }
                }
            }
        ],
        "outbound": [
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1960601",
                "data": {
                    "response": {
                        "status": {
                            "code": 200,
                            "reason": "OK"
                        },
                        "headers": [
                            {
                                "name": "Pragma",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Length",
                                "value": "124"
                            },
                            {
                                "name": "Cache-Control",
                                "value": "no-cache"
                            },
                            {
                                "name": "Content-Type",
                                "value": "application/xml; charset=utf-8"
                            },
                            {
                                "name": "Date",
                                "value": "Tue, 23 Jun 2015 19:51:35 GMT"
                            },
                            {
                                "name": "Expires",
                                "value": "-1"
                            },
                            {
                                "name": "Server",
                                "value": "Microsoft-IIS/8.5"
                            },
                            {
                                "name": "X-AspNet-Version",
                                "value": "4.0.30319"
                            },
                            {
                                "name": "X-Powered-By",
                                "value": "ASP.NET"
                            }
                        ]
                    }
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1961112",
                "data": {
                    "message": "Response headers have been sent to the caller. Starting to stream the response body."
                }
            },
            {
                "source": "handler",
                "timestamp": "2015-06-23T19:51:35.4256650Z",
                "elapsed": "00:00:00.1963155",
                "data": {
                    "message": "Response body streaming to the caller is complete."
                }
            }
        ]
    }
}
```

## <a name="next-steps"> </a>Další kroky
* Podívejte se na ukázku trasování výrazy zásad v [cloudu zahrnují díl 177: víc funkcí správy rozhraní API](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/). Převinutí souboru 21:00 zobrazíte ukázku.

> [!VIDEO https://channel9.msdn.com/Shows/Cloud+Cover/Episode-177-More-API-Management-Features-with-Vlad-Vinogradsky/player]
> 
> 

[Use API Inspector to trace a call]: #trace-call
[Inspect the trace]: #inspect-trace
[Next steps]: #next-steps

[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md

[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Azure Classic Portal]: https://manage.windowsazure.com/


[api-management-developer-portal-menu]: ./media/api-management-howto-api-inspector/api-management-developer-portal-menu.png
[api-management-api]: ./media/api-management-howto-api-inspector/api-management-api.png
[api-management-echo-api-get]: ./media/api-management-howto-api-inspector/api-management-echo-api-get.png
[api-management-developer-key]: ./media/api-management-howto-api-inspector/api-management-developer-key.png
[api-management-open-console]: ./media/api-management-howto-api-inspector/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-api-inspector/api-management-http-get.png
[api-management-send-results]: ./media/api-management-howto-api-inspector/api-management-send-results.png




