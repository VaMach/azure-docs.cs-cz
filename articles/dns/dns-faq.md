---
title: "Nejčastější dotazy k Azure DNS | Microsoft Docs"
description: "Časté otázky k Azure DNS"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/22/2017
ms.author: jonatul
ms.openlocfilehash: 9d786ce4d06ec95a647a755bed51f824e72ad04c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/11/2017
---
# <a name="azure-dns-faq"></a>Nejčastější dotazy k Azure DNS

## <a name="about-azure-dns"></a>O Azure DNS

### <a name="what-is-azure-dns"></a>Co je Azure DNS?

Systému názvů domény nebo DNS, zodpovídá za překladu (nebo vyřešení) název webu nebo služby na jeho IP adresu. Azure DNS je hostitelská služba domén DNS poskytnutí překladu názvů pomocí infrastruktury Microsoft Azure. Pokud svoje domény hostujete v Azure, můžete spravovat svoje DNS záznamy pomocí stejných přihlašovacích údajů, rozhraní API a nástrojů a za stejných fakturačních podmínek jako u ostatních služeb Azure.

DNS domén v Azure DNS jsou hostované v Azure globální síti názvových serverů DNS. Používáme všesměrového vysílání sítě tak, aby každý dotaz DNS je zodpovězen nejbližší server DNS k dispozici. To poskytuje vysoký výkon a vysokou dostupnost vaší domény.

Služba Azure DNS je založena na Azure Resource Manager. Jako takový výhody z funkce služby Správce prostředků například řízení přístupu na základě rolí, protokoly auditu a uzamčení prostředků. Doménách a záznamech lze spravovat prostřednictvím portálu Azure, rutin prostředí Azure PowerShell a rozhraní příkazového řádku Azure napříč platformami. Aplikace, které potřebují Automatická správa DNS může integrovat službu pomocí REST API a sady SDK.

### <a name="how-much-does-azure-dns-cost"></a>Kolik Azure DNS stojí?

Model fakturace Azure DNS je založena na počtu zónách DNS hostovaných v Azure DNS a počet dotazů DNS, který obdrží. Slevy jsou poskytovány na základě využití.

Další informace najdete v tématu [Azure DNS stránce s cenami](https://azure.microsoft.com/pricing/details/dns/).

### <a name="what-is-the-sla-for-azure-dns"></a>Jaká je smlouva SLA pro Azure DNS?

Pro platné žádosti DNS garantujeme přijetí odpovědi alespoň od jednoho z názvových serverů Azure DNS minimálně 99,99 % času.

Další informace najdete v tématu [stránku smlouvy SLA pro Azure DNS](https://azure.microsoft.com/support/legal/sla/dns).

### <a name="what-is-a-dns-zone-is-it-the-same-as-a-dns-domain"></a>Novinky DNS zóny Je to to samé jako doména DNS? 

Doména je jedinečný název v systému názvů domény, například "contoso.com".

K hostování záznamů DNS v určité doméně se používá zóna DNS. Třeba doména "contoso.com" může obsahovat několik záznamů DNS, třeba "mail.contoso.com" (pro poštovní server) a "www.contoso.com" (pro webový server). To by být hostovaná v zóně DNS "contoso.com".

Název domény je *pouze název*, zatímco zóny DNS je datový prostředek, který obsahuje záznamy DNS pro název domény. Azure DNS vám umožňuje hostovat zónu DNS a spravovat záznamy DNS pro doménu v Azure. Nabízí taky názvových serverů DNS k odpovědi na dotazy DNS z Internetu.

### <a name="do-i-need-to-purchase-a-dns-domain-name-to-use-azure-dns"></a>Když chci používat Azure DNS, musím si kupovat název domény DNS? 

Ne nutně.

K hostování zóny DNS v Azure DNS nemusíte kupovat doménu. Zónu DNS můžete vytvořit kdykoli a nemusíte přitom vlastnit název příslušné domény. Dotazy DNS pro tuto zónu pouze vyřeší, pokud jsou směrované na názvové servery Azure DNS přiřadit do zóny.

Budete muset zakoupit název domény, pokud chcete propojit zóny DNS do globální hierarchie DNS – to umožňuje, aby dotazy DNS odkudkoli na světě najít zónu DNS a odpověď, v níž svoje záznamy DNS.

## <a name="azure-dns-features"></a>Funkce Azure DNS

### <a name="does-azure-dns-support-dns-based-traffic-routing-or-endpoint-failover"></a>Podporuje Azure DNS na základě DNS provoz směrování nebo koncový bod převzetí služeb při selhání?

Na základě DNS provoz směrování a koncového bodu převzetí služeb při selhání jsou k dispozici pomocí Azure Traffic Manager. Toto je samostatná služba Azure, který lze použít společně s Azure DNS. Další informace najdete v tématu [Traffic Manager Přehled](../traffic-manager/traffic-manager-overview.md).

Azure DNS podporuje pouze hostování "statická" domény DNS, kde každý dotaz DNS pro daný záznam DNS vždycky obdržel stejnou odpověď DNS.

### <a name="does-azure-dns-support-domain-name-registration"></a>Podporuje Azure DNS registrace názvu domény?

Ne. Azure DNS aktuálně nepodporuje nákup názvů domén. Pokud chcete zakoupit domén, budete muset použít doménového registrátora názvu domény třetí strany. Registrátor obvykle účtuje malý roční poplatek. Pro správu záznamů DNS, může být domény pak hostovaný v Azure DNS. V tématu [delegování domény do Azure DNS](dns-domain-delegation.md) podrobnosti.

Toto je funkce, které sledujeme na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů na [zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/4996615-azure-should-be-its-own-domain-registrar).

### <a name="does-azure-dns-support-private-domains"></a>Podporuje Azure DNS "privátní" domény?
Podpora domén "privátní" je implementovaná pomocí privátní zóny DNS.  Tato funkce je aktuálně k dispozici jako náhled.  Zóny DNS privátní se spravují pomocí stejné nástroje jako internetového Azure DNS zóny, ale jsou pouze přeložit z v rámci zadaného virtuálních sítí.  Najdete v článku [přehled](private-dns-overview.md) podrobnosti.

Informace o dalších interní DNS v Azure najdete v tématu [překlad názvů pro virtuální počítače a instance rolí](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md).

### <a name="does-azure-dns-support-dnssec"></a>Podporuje Azure DNS DNSSEC?

Ne. Azure DNS v současné době nepodporuje DNSSEC.

Toto je funkce, které sledujeme na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů na [zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/13284393-azure-dns-needs-dnssec-support).

### <a name="does-azure-dns-support-zone-transfers-axfrixfr"></a>Podporuje Azure DNS přenosy zóny (AXFR/IXFR)?

Ne. Azure DNS v současné době nepodporuje přenosy zóny. Zóny DNS může být [importovat do Azure DNS pomocí rozhraní příkazového řádku Azure](dns-import-export.md). Záznamy DNS potom je můžete spravovat prostřednictvím [portálu pro správu Azure DNS](dns-operations-recordsets-portal.md), naše [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [rutiny prostředí PowerShell](dns-operations-recordsets.md), nebo [rozhraní příkazového řádku nástroje](dns-operations-recordsets-cli.md).

Toto je funkce, které sledujeme na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů na [zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/12925503-extend-azure-dns-to-support-zone-transfers-so-it-c).

### <a name="does-azure-dns-support-url-redirects"></a>Podporuje Azure DNS adresy URL přesměrování?

Ne. Adresa URL přesměrování služby nejsou ve skutečnosti služba DNS – pracují na úrovni protokolu HTTP, nikoli na úrovni DNS. Někteří poskytovatelé služby DNS pro adresu URL sady přesměrování služby v rámci jejich celkový nabídky. To není podporováno aktuálně Azure DNS.

Tato funkce je sledovaný na našem nevyřízených položek. Můžete použít náš web pro zasílání názorů na [zaregistrovat podporu pro tuto funkci](https://feedback.azure.com/forums/217313-networking/suggestions/10109736-provide-a-301-permanent-redirect-service-for-ape).

## <a name="using-azure-dns"></a>Pomocí Azure DNS

### <a name="can-i-co-host-a-domain-using-azure-dns-and-another-dns-provider"></a>Můžu společné hostování domény pomocí Azure DNS a jiného poskytovatele DNS?

Ano. Azure DNS podporuje společné hosting domén s jinými službami DNS.

To pokud chcete udělat, musíte upravit záznamy NS pro doménu (který ovládací prvek poskytovatelů, kteří obdrží DNS dotazuje pro doménu) tak, aby odkazoval na názvové servery obou zprostředkovatelů. Tyto záznamy NS potřeba upravit v 3 míst: v Azure DNS, v rámci jiné zprostředkovatele a v nadřazené zóně (obvykle konfiguruje prostřednictvím registrátorem názvu domény). Další informace o delegování DNS najdete v tématu [delegování DNS domény](dns-domain-delegation.md).

Kromě toho budete muset zajistěte, aby záznamy DNS pro doménu synchronizace mezi i poskytovatelé služby DNS. Azure DNS v současné době nepodporuje přenosy zóny DNS. Potřebujete synchronizovat záznamy DNS, využívající buď [portálu pro správu Azure DNS](dns-operations-recordsets-portal.md), naše [REST API](https://docs.microsoft.com/powershell/module/azurerm.dns), [SDK](dns-sdk.md), [rutiny prostředí PowerShell](dns-operations-recordsets.md), nebo [rozhraní příkazového řádku nástroje](dns-operations-recordsets-cli.md).

### <a name="do-i-have-to-delegate-my-domain-to-all-4-azure-dns-name-servers"></a>Je nutné delegovat na všechny 4 názvových serverů Azure DNS Moje doména?

Ano. Azure DNS přiřadí každé zóny DNS pro odolnost izolace a vyšší odolnosti 4 názvové servery. K získání oprávnění pro smlouvy SLA pro Azure DNS, budete muset delegování domény do všech 4 názvové servery.

### <a name="what-are-the-usage-limits-for-azure-dns"></a>Jaká jsou omezení využití pro Azure DNS?

Následující výchozí omezení platí při použití Azure DNS:

[!INCLUDE [dns-limits](../../includes/dns-limits.md)]

### <a name="can-i-move-an-azure-dns-zone-between-resource-groups-or-between-subscriptions"></a>Můžete přesunout zóny DNS, mezi skupinami prostředků nebo mezi předplatnými?

Ano. Zóny DNS lze přesouvat mezi skupinami prostředků, nebo mezi předplatnými.

Není žádný vliv na dotazy DNS, při přesouvání zóny DNS. Názvové servery přiřazené pro zónu zůstávají stejné a dotazy DNS jsou zpracovány jako normální v průběhu.

Další informace a pokyny, jak přesunout zóny DNS, najdete v části [přesunutím prostředků do nové skupiny prostředků nebo předplatného](../azure-resource-manager/resource-group-move-resources.md).

### <a name="how-long-does-it-take-for-dns-changes-to-take-effect"></a>Jak dlouho trvá DNS změny se projeví?

Nové zóny DNS a záznamy DNS jsou obvykle projeví ve názvových serverů Azure DNS rychle během několika sekund.

Změny na existující záznamy DNS může trvat delší dobu, ale měli stále projeví na názvových serverů Azure DNS v rámci 60 sekund. V takovém případě DNS ukládání do mezipaměti mimo Azure DNS (podle rekurzivní překladače služby DNS a klienty DNS) mohou ovlivnit čas potřebný pro změnu DNS účinné. Tato doba trvání mezipaměti se dá řídit pomocí vlastnost Time To Live (TTL) každá sada záznamů.

### <a name="how-can-i-protect-my-dns-zones-against-accidental-deletion"></a>Jak lze chránit Moje zóny DNS před náhodným odstraněním?

Azure DNS je spravován pomocí Azure Resource Manager a výhody z řízení přístupu funkce správce Azure Resource Manager poskytuje. Řízení přístupu na základě rolí lze použít k řízení uživatelů, kteří mají čtení nebo zápis do zóny DNS a sady záznamů. Uzamčení prostředků můžete použít sady záznamů a zabránit náhodnému úpravy nebo odstranění zóny DNS.

Další informace najdete v tématu [Protecting zóny DNS a záznamy](dns-protect-zones-recordsets.md).

### <a name="how-do-i-set-up-spf-records-in-azure-dns"></a>Jak nastavím záznamy SPF v Azure DNS?

[!INCLUDE [dns-spf-include](../../includes/dns-spf-include.md)]

### <a name="how-do-i-set-up-an-international-domain-name-idn-in-azure-dns"></a>Jak lze nastavit až mezinárodní IDN název domény () v Azure DNS?

Mezinárodní názvy domén (IDN) fungovat podle kódování, každý využívající název DNS se[punycode](https://en.wikipedia.org/wiki/Punycode)'. Dotazy DNS jsou vytvářeny pomocí názvy těchto kódováním punycode.

Můžete nakonfigurovat mezinárodní názvy domén (IDN) v Azure DNS první převádění název zóny, nebo název sady záznamu na kódování punycode. Azure DNS v současné době nepodporuje integrovanou převod z punycode.

## <a name="next-steps"></a>Další kroky

[Další informace o službě Azure DNS](dns-overview.md)
<br>
[Další informace o používání Azure DNS pro privátní domény](private-dns-overview.md)
<br>
[Další informace o zóny DNS a záznamů](dns-zones-records.md)
<br>
[Začínáme s Azure DNS](dns-getstarted-portal.md)

