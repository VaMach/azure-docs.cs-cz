<properties
    pageTitle="Přidání ukládání do mezipaměti ke zlepšení výkonu služby Azure API Management | Microsoft Azure"
    description="Naučte se zlepšit latenci, spotřebu šířky pásma a načítání webových služeb při volání služby API Management."
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/25/2016"
    ms.author="sdanie"/>

# Přidání ukládání do mezipaměti ke zlepšení výkonu služby Azure API Management

Operace ve službě API Management můžete nakonfigurovat tak, aby odpovědi ukládaly do mezipaměti. Ukládání odpovědí do mezipaměti může v případě dat, která se často nemění, výrazně zlepšit latenci rozhraní API, využití šířky pásma a načítání webových služeb.

Tento průvodce vám ukáže, jak přidat ukládání odpovědí do mezipaměti pro rozhraní API a jak nakonfigurovat zásady pro operace ukázkového rozhraní Echo API. Potom můžete operaci volat z portálu pro vývojáře a ověřit ukládání do mezipaměti v akci.

>[AZURE.NOTE] Informace o ukládání položek do mezipaměti podle klíče pomocí výrazů zásad najdete v článku [Vlastní ukládání do mezipaměti ve službě Azure API Management](api-management-sample-cache-by-key.md).

## Požadavky

Než začnete provádět kroky podle této příručky, musíte mít instanci služby API Management s nakonfigurovaným rozhraním API a produktem. Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][] v kurzu [Začínáme s Azure API Management][].

## <a name="configure-caching"> </a>Konfigurace operace pro ukládání do mezipaměti

V tomto kroku zkontrolujete nastavení ukládání do mezipaměti operace **GET Resource (cached)** v ukázkovém rozhraní Echo API.

>[AZURE.NOTE] Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním Echo API, které můžete použít k experimentování a seznámení se službou API Management. Další informace najdete v článku [Začínáme s Azure API Management][]

Pokud chcete začít, klikněte na portálu Azure Classic služby API Management na **Spravovat**. Tím přejdete na portál vydavatele služby API Management.

![Portál vydavatele][api-management-management-console]

Klikněte v nabídce **API Management** na levé straně na **Rozhraní API** a potom na **Echo API**.

![Echo API][api-management-echo-api]

Klikněte na kartu **Operace** a potom v seznamu **Operace** klikněte na **GET Resource (cached)**.

![Operace rozhraní API v programu Echo][api-management-echo-api-operations]

Pokud chcete zobrazit nastavení ukládání do mezipaměti, klikněte na kartu **Ukládání do mezipaměti**.

![Karta Ukládání do mezipaměti][api-management-caching-tab]

Pokud chcete povolit ukládání operace do mezipaměti, zaškrtněte políčko **Povolit**. V tomto příkladu je ukládání do mezipaměti povoleno.

Každá odpověď operace se ukládá do klíčů na základě hodnot v polích **Podle parametrů řetězce dotazu** a **Podle hlaviček**. Pokud chcete do mezipaměti ukládat více odpovědí na základě parametrů řetězce dotazu nebo na základě hlaviček, můžete to konfigurovat v těchto dvou polích.

**Doba trvání** určuje dobu vypršení uložení odpovědí v mezipaměti. V tomto příkladu je interval **3600** sekund, což odpovídá jedné hodině.

Když v tomto příkladu použijeme konfiguraci ukládání do mezipaměti, první požadavek na operaci **GET Resource (cached)** vrátí odpověď z back-endové služby. Tato odpověď se uloží do mezipaměti, kam bude zadaná podle určených hlaviček a parametrů řetězce dotazu. Následující volání operace (s odpovídající parametry) bude vracet odpověď uloženou v mezipaměti až do okamžiku vypršení doby uložení v mezipaměti.

## <a name="caching-policies"> </a>Kontrola zásad ukládání do mezipaměti

V tomto kroku zkontrolujete nastavení ukládání do mezipaměti operace **GET Resource (cached)** v ukázkovém rozhraní Echo API.

Pokud jsou na kartě **Ukládání do mezipaměti** nakonfigurovaná nastavení pro ukládání operace do mezipaměti, budou k operaci přidány zásady ukládání do mezipaměti. Tyto zásady můžete zobrazit a upravit v editoru zásad.

V nabídce **API Management** na levé straně klikněte na **Zásady** a potom v rozevíracím seznamu **Operace** vyberte **Echo API / GET Resource (cached)**.

![Operace rozsahu zásad][api-management-operation-dropdown]

Zobrazí zásady této operace v editoru zásad.

![Editor zásad služby API Management][api-management-policy-editor]

Definice zásad této operace obsahuje zásady, které definují konfiguraci ukládání do mezipaměti, které jste zkontrolovali pomocí karty **Ukládání do mezipaměti** v předchozím kroku.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Změny zásad ukládání do mezipaměti provedené v editoru zásad se projeví na kartě **Ukládání do mezipaměti** příslušné operace a naopak.

## <a name="test-operation"> </a>Volání operace a testování ukládání do mezipaměti

Abyste viděli ukládání do mezipaměti v akci, můžete operaci volat z portálu pro vývojáře. Klikněte na **Portál pro vývojáře** v pravé horní nabídce.

![Portál pro vývojáře][api-management-developer-portal-menu]

Klikněte na **Rozhraní API** v horní nabídce a potom vyberte **Echo API**.

![Echo API][api-management-apis-echo-api]

>Pokud máte nakonfigurované jenom jedno rozhraní API nebo váš účet vidí jenom jedno, můžete kliknutím na rozhraní API přejít přímo na operace tohoto rozhraní.

Vyberte operaci **GET Resource (cached)** a potom klikněte na **Otevřít konzolu**.

![Otevření konzoly][api-management-open-console]

Konzola umožňuje vyvolání operací přímo z portálu pro vývojáře.

![Konzola][api-management-console]

Ponechte výchozí hodnoty pro **param1** a **param2**.

V rozevíracím seznamu **subscription-key** vyberte požadovaný klíč. Pokud má váš účet jenom jedno předplatné, bude už klíč vybrán.

Do textového pole **Hlavičky požadavků** zadejte text **sampleheader:value1**.

Klikněte na **HTTP Get** a poznamenejte si hlavičky odpovědi.

Do textového pole **Hlavičky požadavků** zadejte text **sampleheader:value2** a potom klikněte na **HTTP Get**.

Všimněte si, že hodnota **sampleheader** zůstává v odpovědi **value1**. Zkuste několik různých hodnot a všimněte si, že se vrátí odpověď uložená v mezipaměti, která pochází z prvního volání.

Do pole **param2** zadejte hodnotu **25** a potom klikněte na **HTTP Get**.

Všimněte si, že hodnota **sampleheader** je teď v odpovědi **value2**. Protože výsledky operace se ukládají do klíčů pomocí řetězce dotazu, předchozí odpověď uložená v mezipaměti se ne vrátila.

## <a name="next-steps"> </a>Další kroky

-   Podívejte se i na další témata kurzu [Začínáme s pokročilou konfigurací rozhraní API][].
-   Další informace o zásadách ukládání do mezipaměti najdete v části [Zásady ukládání do mezipaměti][] v článku [Zásady API Managementu][].
-   Informace o ukládání položek do mezipaměti podle klíče pomocí výrazů zásad najdete v článku [Vlastní ukládání do mezipaměti ve službě Azure API Management](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[Přidání operací do rozhraní API]: api-management-howto-add-operations.md
[Přidání a publikování produktu]: api-management-howto-add-products.md
[Sledování a analýza]: api-management-monitoring.md
[Přidání rozhraní API do produktu]: api-management-howto-add-products.md#add-apis
[Publikování produktu]: api-management-howto-add-products.md#publish-product
[Začínáme s Azure API Management]: api-management-get-started.md
[Začínáme s pokročilou konfigurací rozhraní API]: api-management-get-started-advanced.md

[Zásady API Managementu]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Zásady ukládání do mezipaměti]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Vytvoření instance API Management]: api-management-get-started.md#create-service-instance

[Konfigurace operace pro ukládání do mezipaměti]: #configure-caching
[Kontrola zásad ukládání do mezipaměti]: #caching-policies
[Volání operace a testování ukládání do mezipaměti]: #test-operation
[Další kroky]: #next-steps



<!--HONumber=Jun16_HO2-->


