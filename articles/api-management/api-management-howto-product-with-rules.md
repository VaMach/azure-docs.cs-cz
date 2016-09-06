<properties
    pageTitle="Ochrana rozhraní API ve službě Azure API Management | Microsoft Azure"
    description="Seznamte se s možnostmi ochrany rozhraní API pomocí zásad kvót a zásad omezování četnosti."
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
    ms.date="08/24/2016"
    ms.author="sdanie"/>

# Ochrana rozhraní API omezením četnosti pomocí Azure API Management

Tento průvodce vám ukáže, jak snadno můžete pomocí služby Azure API Management přidat ochranu rozhraní API vašeho back-endu tím, že nakonfigurujete zásady omezení četnosti a zásady kvót.

V tomto kurzu vytvoříte „bezplatnou zkušební verzi“ produktu s rozhraním API, která vývojářům umožní provádět až 10 volání za minutu a až 200 volání za týden do vašeho rozhraní API pomocí zásad [Omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [Nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota). Potom rozhraní API publikujete a zásady omezení četnosti otestujete.

Pokud se zajímáte o pokročilejší scénáře omezování pomocí zásad [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) a [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey), podívejte se na článek [Pokročilé omezování požadavků pomocí Azure API Management](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Vytvoření produktu

V tomto kroku vytvoříte bezplatnou zkušební verzi produktu, který nevyžaduje schválení předplatného.

>[AZURE.NOTE] Pokud už máte produkt nakonfigurovaný a chcete ho v tomto kurzu použít, můžete přeskočit na článek [Konfigurace zásad omezení četnosti a zásad kvót][] a postupovat v kurzu odtamtud se svým produktem místo bezplatné zkušební verze produktu.

Pokud chcete začít, klikněte na portálu Azure Classic služby API Management na **Spravovat**. Tím přejdete na portál vydavatele služby API Management.

![Portál vydavatele][api-management-management-console]

>Pokud jste instanci služby API Management ještě nevytvořili, přečtěte si článek [Vytvoření instance API Management][] v kurzu [Správa vašeho prvního rozhraní API v Azure API Management][].

V nabídce **API Management** na levé straně klikněte na **Produkty** a zobrazte stránku **Produkty**.

![Přidání produktu][api-management-add-product]

Kliknutím na **Přidat produkt** zobrazíte dialogové okno **Přidání nového produktu**.

![Přidání nového produktu][api-management-new-product-window]

Do pole **Nadpis** zadejte text **Bezplatná zkušební verze**.

Do pole **Popis** zadejte následující text:  **Předplatitelé můžou spustit 10 volání za minutu až do maximálního počtu 200 volání za týden. Potom bude přístup odepřen.**

Produkty ve službě API Management můžou být chráněné nebo otevřené. V případě chráněných produktů se musíte nejdřív přihlásit k jejich odběru a až potom je můžete používat. Otevřené produkty můžete používat bez předplatného. Pokud chcete vytvořit chráněný produkt, který vyžaduje předplatné, nezapomeňte vybrat možnost **Vyžadovat předplatné**. Toto je výchozí nastavení.

Pokud chcete, aby pokusy o přihlášení k odběru produktu kontroloval a následně přijímal nebo odmítal správce, vyberte možnost **Vyžadovat schválení předplatného**. Pokud necháte políčko nezaškrtnuté, pokusy o přihlášení k odběru budou schvalovány automaticky. V tomto příkladu se předplatné schvaluje automaticky, proto políčko nezaškrtávejte.

Pokud chcete vývojářským účtům povolit přihlášení k vícenásobným odběrům nového produktu, zaškrtněte políčko **Povolit více souběžných předplatných**. Tento kurz několik souběžných předplatných nevyužívá, takže políčko nechte nezaškrtnuté.

Po zadání všech hodnot klikněte na **Uložit** a vytvořte produkt.

![Produkt přidán][api-management-product-added]

Ve výchozím nastavení jsou nové produkty viditelné pro uživatele ve skupině **Správci**. Teď přidáme skupinu **Vývojáři**. Klikněte na **Bezplatná zkušební verze** a potom na kartu **Viditelnost**.

>Ve službě API Management se ke správě viditelnosti produktů pro vývojáře používají skupiny. Produkty udělují viditelnost skupinám a vývojáři můžou zobrazovat a odebírat produkty, které jsou viditelné pro skupinu, do které patří. Další informace najdete v článku [Vytvoření a používání skupin v Azure API Management][].

![Přidání skupiny vývojářů][api-management-add-developers-group]

Zaškrtněte políčko **Vývojáři** a potom klikněte na **Uložit**.

## <a name="add-api"> </a>Přidání rozhraní API do produktu

V tomto kroku kurzu přidáme rozhraní API v programu Echo do nového produktu v bezplatné zkušební verzi.

>Každá instance služby API Management je vybavená předem nakonfigurovaným rozhraním API programu Echo, které můžete použít k experimentování a seznámení se službou API Management. Další informace najdete v článku [Správa vašeho prvního rozhraní API v Azure API Management][].

V nabídce **API Management** na levé straně klikněte na **Produkty**, potom na **Bezplatná zkušební verze** a nakonfigurujte produkt.

![Konfigurace produktu][api-management-configure-product]

Klikněte na **Přidat rozhraní API do produktu**.

![Přidání rozhraní API do produktu][api-management-add-api]

Vyberte **Rozhraní API v programu Echo** a potom klikněte na **Uložit**.

![Přidání rozhraní API v programu Echo][api-management-add-echo-api]

## <a name="policies"> </a>Konfigurace zásad kvót a zásad omezení četnosti volání

Omezení četnosti a kvóty se konfigurují v editoru zásad. V nabídce **API Management** na levé straně klikněte na **Zásady**. V seznamu **Produkt** klikněte na položku **Bezplatná zkušební verze**.

![Zásady produktu][api-management-product-policy]

Kliknutím na **Přidat zásady** proveďte import šablony zásad a začněte vytvářet zásady omezení četnosti a zásady kvót.

![Přidání zásad][api-management-add-policy]

Pokud chcete zásady vložit, umístěte kurzor do šablony zásad v části **inbound** (příchozí) nebo **outbound** (odchozí). Zásady omezení četnosti a zásady kvót jsou příchozími zásadami, proto kurzor umístěte do příchozího prvku.

![Editor zásad][api-management-policy-editor-inbound]

Dvě zásady, které v tomto kurzu přidáváme, se nazývají [Omezení četnosti volání podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) a [Nastavení kvóty využití podle předplatného](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota).

![Příkazy zásad][api-management-limit-policies]

Po umístění kurzoru v prvku **inbound** zásad klikněte na šipku vedle položky **Omezení četnosti volání podle předplatného** a vložte jeho šablonu zásad.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Omezení četnosti volání podle předplatného** můžete použít na úrovni produktu, ale dá se použít i na úrovni rozhraní API a na úrovni názvu jednotlivých operací. V tomto kurzu používáme jenom zásady na úrovni produktu, takže z prvku **rate-limit** můžete odstranit prvky **api** a **operation**, aby zůstal jenom vnější prvek **rate-limit**, jak je znázorněno v následujícím příkladu.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

V případě bezplatné zkušební verze produktu je maximální povolená četnost volání 10 volání za minutu, proto do atributu **call** zadejte hodnotu**10** a do atributu **renewal-period** zadejte hodnotu **60**.

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Pokud chcete nakonfigurovat zásady **Nastavení kvóty využití podle předplatného**, umístěte kurzor bezprostředně pod nově přidaný prvek **rate-limit** v prvku **inbound** a potom klikněte na šipku nalevo od **Nastavení kvóty využití podle předplatného**.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Vzhledem k tomu, že se tyto zásady mají používat i na úrovni produktu, odstraňte prvky názvu **api** a **operation**, jak je znázorněno v následujícím příkladu.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Kvóty můžou být založené na počtu volání za interval, na šířce pásma nebo na obojím. V tomto kurzu neprovádíme omezování na základě šířky pásma, proto můžete odstranit atribut **bandwidth** (šířka pásma).

    <quota calls="number" renewal-period="seconds">
    </quota>

V rámci bezplatné zkušební verze produktu má kvóta hodnotu 200 volání za týden. Do atributu **calls** zadejte hodnotu **200** a potom do atributu **renewal-period** zadejte hodnotu **604800**.

    <quota calls="200" renewal-period="604800">
    </quota>

>Intervaly zásad se zadávají v sekundách. Pokud chcete vypočítat interval pro týden, můžete počet dní (7) vynásobit počtem hodin za den (24), počtem minut za hodinu (60) a počtem sekund za minutu (60): 7 * 24 * 60 * 60 = 604800.

Zásady by po dokončení konfigurace měly odpovídat následujícímu příkladu.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Po nakonfigurování požadovaných zásad klikněte na **Uložit**.

![Uložení zásad][api-management-policy-save]

## <a name="publish-product"> </a> Publikování produktu

Když jste přidali rozhraní API a nakonfigurovali zásady, je třeba produkt publikovat, aby ho vývojáři mohli začít používat. V nabídce **API Management** na levé straně klikněte na **Produkty**, potom na **Bezplatná zkušební verze** a nakonfigurujte produkt.

![Konfigurace produktu][api-management-configure-product]

Klikněte na **Publikovat** a potvrďte kliknutím na **Ano, publikovat**.

![Publikování produktu][api-management-publish-product]

## <a name="subscribe-account"> </a>Přihlášení vývojářského účtu k odběru produktu

Teď, když je produkt publikovaný, se vývojáři můžou přihlásit k jeho odběru a můžou ho začít používat.

>Správci instance API Management se automaticky přihlašují k odběru každého produktu. V tomto kroku kurzu přihlásíme jeden vývojářský účet bez oprávnění správce k odběru bezplatné zkušební verze produktu. Pokud je vývojářský účet součástí role správců, můžete tímto krokem projít i v případě, že už jste k odběru přihlášeni.

V nabídce **API Management** na levé straně klikněte na **Uživatelé** a potom klikněte na název vývojářského účtu. V tomto příkladu používáme vývojářský účet **Clayton Gragg**.

![Konfigurace vývojáře][api-management-configure-developer]

Klikněte na **Přidat předplatné**.

![Přidat předplatné][api-management-add-subscription-menu]

Vyberte **Bezplatná zkušební verze** a potom klikněte na **Přihlásit k odběru**.

![Přidat předplatné][api-management-add-subscription]

>[AZURE.NOTE] V tomto kurzu není v případě bezplatné zkušební verze produktu povoleno více souběžných předplatných. Pokud by bylo, zobrazila by se výzva jako v následujícím příkladu.

![Přidat předplatné][api-management-add-subscription-multiple]

Po kliknutí na **Přihlásit k odběru** se produkt zobrazí v uživatelském seznamu **Předplatné**.

![Předplatné přidáno][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Volání operace a testování omezení četnosti

Když už máte bezplatnou zkušební verzi produktu nakonfigurovanou a publikovanou, můžete začít volat operace a testovat omezení četnosti.
Kliknutím na **Portál pro vývojáře** v pravé horní nabídce přejděte na portál pro vývojáře.

![Portál pro vývojáře][api-management-developer-portal-menu]

Klikněte v horní nabídce na **Rozhraní API** a potom vyberte **Rozhraní API v programu Echo**.

![portálu pro vývojáře][api-management-developer-portal-api-menu]

Klikněte na **GET Resource** a potom klikněte na **Zkouška**.

![Otevření konzoly][api-management-open-console]

Ponechte výchozí hodnoty parametrů a potom vyberte svůj klíč předplatného pro bezplatnou zkušební verzi produktu.

![Klíč předplatného][api-management-select-key]

>[AZURE.NOTE] Pokud máte více předplatných, vyberte klíč pro **bezplatnou zkušební verzi**, jinak zásady nakonfigurované v předchozích krocích nebudou platit.

Klikněte na **Odeslat** a potom si zobrazte odezvu. Všimněte si, že **stav odezvy** je **200 – OK**.

![Výsledky operace][api-management-http-get-results]

Klikněte na **Odeslat** víckrát, než dovolují zásady omezení četnosti (10 volání za minutu). Po překročení zásad omezení četnosti se vrátí stav odezvy **429 – Příliš mnoho požadavků**.

![Výsledky operace][api-management-http-get-429]

 **Obsah odezvy** zobrazuje zbývající délku intervalu, po kterém bude opakování úspěšné.

Pokud platí zásady omezení četnosti v počtu 10 volání za minutu, následná volání nebudou úspěšná, dokud neuplyne 60 sekund od prvních 10 úspěšných volání produktu před překročením omezení četnosti volání. V tomto příkladu je zbývající délka intervalu 54 sekund.

## <a name="next-steps"> </a>Další kroky

-   V následujícím videu si pusťte ukázku nastavení kvót a omezení četnosti.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[Přidání operací do rozhraní API]: api-management-howto-add-operations.md
[Přidání a publikování produktu]: api-management-howto-add-products.md
[Sledování a analýza]: ../api-management-monitoring.md
[Přidání rozhraní API do produktu]: api-management-howto-add-products.md#add-apis
[Publikování produktu]: api-management-howto-add-products.md#publish-product
[Správa vašeho prvního rozhraní API v Azure API Management]: api-management-get-started.md
[Vytvoření a používání skupin v Azure API Management]: api-management-howto-create-groups.md
[Zobrazení předplatitelů produktu]: api-management-howto-add-products.md#view-subscribers
[Začínáme s Azure API Management]: api-management-get-started.md
[Vytvoření instance API Management]: api-management-get-started.md#create-service-instance
[Další kroky]: #next-steps

[Vytvoření produktu]: #create-product
[Konfigurace zásad omezení četnosti a zásad kvót]: #policies
[Přidání rozhraní API do produktu]: #add-api
[Publikování produktu]: #publish-product
[Přihlášení vývojářského účtu k odběru produktu]: #subscribe-account
[Volání operace a testování omezení četnosti]: #test-rate-limit

[Omezení četnosti volání]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Nastavení kvóty využití]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota



<!--HONumber=ago16_HO5-->


