---
title: "Zóny DNS a záznamy přehled – Azure DNS | Microsoft Docs"
description: "Přehled podpory pro hostování zón DNS a záznamy v Microsoft Azure DNS."
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: be4580d7-aa1b-4b6b-89a3-0991c0cda897
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/18/2017
ms.author: kumud
ms.openlocfilehash: 0a0808d3963cc037aaf113c67fd01679ee8c1d40
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/19/2017
---
# <a name="overview-of-dns-zones-and-records"></a>Přehled zóny DNS a záznamů

Tato stránka popisuje klíčové koncepty nástroje domény, zóny DNS a záznamy DNS a sady záznamů a jak jsou podporovány v Azure DNS.

## <a name="domain-names"></a>Názvy domén

Domain Name System je hierarchie domén. Hierarchie začíná od kořenové domény, jejíž název je jednoduše „**.**“.  Následují domény nejvyšší úrovně, jako jsou „com“, „net“, „org“, „uk“ nebo „jp“.  Následují domény druhé úrovně, jako jsou „org.uk“ nebo „co.jp“. Domény v hierarchii DNS jsou globálně distribuované a hostované názvovými servery DNS po celém světě.

Registrátorem názvu domény je v organizaci, která umožňuje zakoupit název domény, například "contoso.com".  Zakoupení názvu domény vám dává právo k řízení hierarchii DNS pod názvem, například umožňuje přímé název "www.contoso.com" na web vaší společnosti. Registrátora může hostovat domény v jeho vlastní názvové servery vaším jménem nebo můžete určit alternativní názvové servery.

Azure DNS poskytuje infrastrukturu globálně distribuované, vysokou dostupnost názvu serveru, který můžete použít k hostování vaší domény. Hostování domény do Azure DNS, můžete spravovat svoje záznamy DNS pomocí stejných přihlašovacích údajů, rozhraní API, nástroje, fakturaci a podporu jako jinými službami Azure.

Azure DNS aktuálně nepodporuje nákup názvů domén. Pokud chcete zakoupit název domény, budete muset použít doménového registrátora názvu domény třetí strany. Registrátor obvykle účtuje malý roční poplatek. Pro správu záznamů DNS, může být domény pak hostovaný v Azure DNS. V tématu [delegování domény do Azure DNS](dns-domain-delegation.md) podrobnosti.

## <a name="dns-zones"></a>Zóny DNS

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="dns-records"></a>Záznamy DNS

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

### <a name="time-to-live"></a>Time to live

Hodnota TTL (Time to Live) určuje, jak dlouho budou klienti každý záznam uchovávat v mezipaměti před tím, než bude záznam znovu dotazován. V předchozím příkladu je hodnota TTL 3 600 sekund nebo 1 hodina.

V Azure DNS TTL se zadává pro sadu záznamů, ne pro jednotlivé záznamy, takže se používá stejnou hodnotu pro všechny záznamy v sadě.  Můžete zadat libovolnou hodnotu TTL mezi 1 a 2 147 483 647 sekundami.

### <a name="wildcard-records"></a>Záznamy se zástupným znakem

Azure DNS podporuje [záznamy se zástupným znakem](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Záznamy se zástupným znakem se vrátí v reakci na jakémkoli dotazu s odpovídajícím názvem (pokud nebude nalezena bližší shoda v sadě záznamů bez zástupných znaků). Azure DNS podporuje zástupné sad záznamů u všech typů záznamů s výjimkou NS a SOA.

Chcete-li vytvořit sadu záznamů zástupný znak, použijte název sady záznamů "\*'. Alternativně můžete také použít název s '\*'jako jeho nejvíce vlevo štítek, například"\*.foo".

### <a name="caa-records"></a>Zaznamenává KÁ

Zaznamenává KÁ vlastníky domény k určení, které certifikačních autorit (CA) mají oprávnění k vydávání certifikátů pro svoji doménu. To umožňuje certifikačních autorit, aby se zabránilo nemá vystavování certifikátů v některých případech. Zaznamenává KÁ mít tři vlastnosti:
* **Příznaky**: Toto je celé číslo mezi 0 a 255, používá k reprezentování kritické příznak, který má zvláštní význam za [RFC](https://tools.ietf.org/html/rfc6844#section-3)
* **Značka**: řetězec ve formátu ASCII, což může být jeden z následujících:
    * **problém**: použít, pokud chcete určit certifikačních autorit, které jsou povolené k vydávání certifikátů (všechny typy)
    * **issuewild**: použít, pokud chcete určit certifikačních autorit, které jsou povolené k vydávání certifikátů (pouze certifikátů zástupný znak)
    * **iodef**: Zadejte e-mailovou adresu nebo název hostitele, ke kterému může upozornit certifikační autority pro neoprávněné certifikátů problém žádostí
* **Hodnota**: hodnota pro konkrétní značku vybrali

### <a name="cname-records"></a>Záznamy CNAME

Sady záznamů CNAME nemůžou existovat současně s jinými sadami záznamů se stejným názvem. Například nelze vytvořit záznam CNAME s relativním názvem "www" a záznam A s relativním názvem "www", ve stejnou dobu.

Protože vrchol zóny (název = ' @') vždy obsahuje sady, které byly vytvořeny při vytvoření zóny záznamů NS a SOA, nelze vytvořit záznam CNAME, který je nastaven na vrcholu zóny.

Tato omezení jsou vyvolány z norem DNS a nejsou omezení Azure DNS.

### <a name="ns-records"></a>Záznamy NS

Sady ve vrcholu zóny záznamů NS (název ' @') se vytvoří automaticky s každou zónou DNS a je automaticky odstraněna při odstranění zóny (nelze jej odstranit samostatně).

Tato sada záznamů obsahuje názvy názvových serverů Azure DNS přiřadit do zóny. Můžete přidat další název nastavit servery na tento záznam NS, podporuje společné hosting domén s více než jednoho poskytovatele DNS. Můžete také upravit TTL a metadat pro tuto sadu záznamů. Však nelze odebrat ani změnit předem vyplněná názvových serverů Azure DNS. 

Všimněte si, že vztahuje se pouze na vrcholu zóny sady záznamů NS. Jiné sady záznamů NS v pásmu (jak je používá delegovat podřízených zónách) se dají vytvořit, upravit a odstranit bez omezení.

### <a name="soa-records"></a>Záznamy SOA

Sady záznamů SOA se vytvoří automaticky na vrcholu každé zóny (název = ' @') a automaticky odstraněna při odstranění zóny.  Záznamy SOA nelze vytvořit nebo odstranit samostatně.

Můžete upravit všechny vlastnosti záznamu SOA s výjimkou vlastnost "hostitel", která je předem nakonfigurovaný k odkazování na název název primárního serveru poskytuje Azure DNS.

### <a name="spf-records"></a>Záznamy SPF

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="srv-records"></a>Záznamy SRV

[Záznamy SRV](https://en.wikipedia.org/wiki/SRV_record) používají různé služby k určení umístění serveru. Při zadávání záznam SRV v Azure DNS:

* *Služby* a *protokol* musí být zadány jako část názvu sady záznamů s předponou podtržítka.  Například '\_sip.\_ TCP.Name'.  Pro záznam ve vrcholu zóny, je nutné specifikovat ' @' v názvu záznamu jednoduše použít služba a protokol, například "\_sip.\_ TCP'.
* *s prioritou*, *váhy*, *port*, a *cíl* jsou určené jako parametry všechny záznamy v sadě záznamů.

### <a name="txt-records"></a>Záznamů TXT

Záznamů TXT se používají pro mapování názvů domén na libovolného textového řetězce. Jsou použity v více aplikací, zejména související s konfigurací e-mailu, například [odesílatele zásad Framework (SPF)](https://en.wikipedia.org/wiki/Sender_Policy_Framework) a [DomainKeys identifikovat e-mailu (DKIM)](https://en.wikipedia.org/wiki/DomainKeys_Identified_Mail).

Standardech DNS povolit jeden záznam TXT tak, aby obsahovala více řetězců, z nichž každá může být až 254 znaků. Pokud se používají více řetězců, jsou zřetězených klienty a považován za jeden řetězec.

Při volání rozhraní REST API Azure DNS, budete muset zadat každou TXT řetězec samostatně.  Při použití portálu Azure, PowerShell nebo rozhraní příkazového řádku rozhraní musíte zadat jeden řetězec na záznam, který je automaticky rozdělené do 254 znaků segmenty, v případě potřeby.

Více řetězců v záznamu DNS Nezaměňovat s více záznamů TXT v sadě záznamů TXT.  Sady záznamů TXT může obsahovat několik záznamů, *každý z nich* může obsahovat více řetězců.  Azure DNS podporuje všechny záznamy TXT (v rámci všech záznamů kombinaci) nastavit celkový řetězec délky maximálně 1 024 znaků.

## <a name="tags-and-metadata"></a>Značky a metadata

### <a name="tags"></a>Značky

Značky jsou seznam dvojic název hodnota a slouží k označování prostředků pomocí Azure Resource Manager.  Azure Resource Manager používá k povolení filtrovaných zobrazení faktury Azure a můžete také nastavit zásady, na kterém jsou požadované značky. Další informace o značkách najdete v tématu [Použití značek k uspořádání prostředků Azure](../azure-resource-manager/resource-group-using-tags.md).

Azure DNS podporuje pomocí Azure Resource Manager značek na prostředky zóny DNS.  Nepodporuje značky na sady záznamů DNS, i když jako alternativu, metadata, je podporovaný na sady záznamů DNS popsané níže.

### <a name="metadata"></a>Metadata

Jako alternativu k značky sady záznamů Azure DNS podporuje, zadávání poznámek k sad záznamů pomocí 'metadat'.  Podobně jako u značky, metadata vám umožní přidružit dvojice název hodnota s každou sadu záznamů.  To může být užitečné, například k záznamu účel každá sada záznamů.  Na rozdíl od značky metadata nelze použít k poskytují filtrované zobrazení faktury Azure a nelze zadat v zásadách Azure Resource Manager.

## <a name="etags"></a>Značky etag binárním rozsáhlým

Předpokládejme, že dvě osoby nebo dva procesy zkuste upravit záznam DNS ve stejnou dobu. Které z nich wins? A Vítěz vědět, že jste se přepsat změny vytvořený jiným uživatelem?

Azure DNS používá značky etag binárním rozsáhlým bezpečně zpracování souběžných změny do stejného zdroje. Značky etag binárním rozsáhlým jsou oddělené od [Azure Resource Manager, značky'](#tags). Všechny prostředky DNS (zóny nebo sady záznamů) má Etag s ním spojená. Vždy, když se načte prostředek, je také načíst jeho Značka Etag. Při aktualizaci prostředek, můžete předat zpět značku Etag, můžete ověřit Azure DNS, který značku Etag na serveru odpovídá. Vzhledem k tomu, že každá aktualizace na prostředek, výsledkem Značka Etag se znovu vygeneruje, neshoda značek Etag označuje, že došlo ke změně souběžných. Značky etag binárním rozsáhlým mohou sloužit také při vytváření nového prostředku zajistit, že prostředek již neexistuje.

Ve výchozím nastavení používá Azure DNS PowerShell značky etag binárním rozsáhlým blokovat souběžných změny zón a sady záznamů. Volitelné *-přepsat* přepínač můžete použít k potlačení kontroly Značka Etag, v takovém případě všechny souběžných budou přepsána změny, které mají došlo k chybě.

Na úrovni rozhraní REST API Azure DNS jsou značky etag binárním rozsáhlým zadán pomocí hlavičky protokolu HTTP.  V následující tabulce je uveden jejich chování:

| Záhlaví | Chování |
| --- | --- |
| Žádné |PUT vždy úspěšné (žádná značka Etag kontrola) |
| If-match<etag> |PUT pouze úspěšná, pokud prostředek existuje a Značka Etag odpovídá |
| If-match * |PUT pouze úspěšná, pokud existuje prostředek |
| If-none-match * |PUT pouze úspěšná, pokud prostředek neexistuje. |


## <a name="limits"></a>Omezení

Následující výchozí omezení platí při použití Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

## <a name="next-steps"></a>Další kroky

* Chcete-li začít používat Azure DNS, zjistěte další postup [vytvořit zónu DNS](dns-getstarted-create-dnszone-portal.md) a [vytvořit záznamy DNS](dns-getstarted-create-recordset-portal.md).
* Pokud chcete migrovat existující zónu DNS, zjistěte, jak [import a export souboru zóny DNS](dns-import-export.md).
