---
title: "Přehled zabezpečení sítě Azure | Dokumentace Microsoftu"
description: "Seznamte se s možnostmi zabezpečení pro řízení toku síťového provozu mezi prostředky Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 0e862492c9e17d0acb3c57a0d0abd1f77de08b6a
ms.openlocfilehash: 63a313d9035422207a1ce56f0da8b388e2747685
ms.contentlocale: cs-cz
ms.lasthandoff: 09/27/2017

---
# <a name="network-security"></a>Zabezpečení sítě

Pomocí skupiny zabezpečení sítě můžete omezit síťový provoz směřující do prostředků ve virtuální síti. Skupina zabezpečení sítě obsahuje seznam pravidel zabezpečení, která povolují nebo odepírají příchozí nebo odchozí síťový provoz v závislosti na zdrojové nebo cílové IP adrese, portu a protokolu. 

## <a name="network-security-groups"></a>Skupiny zabezpečení sítě

Každé síťové rozhraní má přidruženou jednu nebo žádnou skupinu zabezpečení sítě. Každé síťové rozhraní existuje v podsíti [virtuální sítě](virtual-networks-overview.md). Podsíť také může mít přidruženou jednu nebo žádnou skupinu zabezpečení sítě. 

Při použití pravidel zabezpečení na podsíť se pravidla zabezpečení použijí na všechny prostředky v této podsíti. Kromě síťových rozhraní můžete mít v podsíti nasazené instance dalších služeb Azure, například HDInsight, škálovací sady virtuálních počítačů a prostředí aplikačních služeb.

V případě přidružení skupiny zabezpečení sítě k síťovému rozhraní i podsíti, ve které se síťové rozhraní nachází, se skupiny zabezpečení sítě použijí následujícím způsobem:

- **Příchozí provoz:** Skupina zabezpečení sítě přidružená k podsíti, ve které se síťové rozhraní nachází, se vyhodnocuje jako první. Veškerý provoz, kterému je povolený průchod přes skupinu zabezpečení sítě přidruženou k podsíti, se pak vyhodnocuje skupinou zabezpečení sítě přidruženou k síťovému rozhraní. Můžete například požadovat příchozí přístup z internetu k virtuálnímu počítači přes port 80. Pokud přidružíte skupinu zabezpečení sítě k síťovému rozhraní i podsíti, ve které se síťové rozhraní nachází, musí tato skupina zabezpečení sítě povolovat port 80. Pokud jste port 80 povolili pouze prostřednictvím skupiny zabezpečení sítě přidružené k podsíti nebo síťovému rozhraní, ve kterém se podsíť nachází, kvůli výchozím pravidlům zabezpečení komunikace selže. Podrobnosti najdete v části [Výchozí pravidla zabezpečení](#default-security-rules). Pokud jste skupinu zabezpečení použili pouze na podsíť nebo síťové rozhraní a pokud obsahovala pravidlo povolující příchozí provoz například přes port 80, komunikace bude úspěšná. 
- **Odchozí provoz:** Skupina zabezpečení sítě přidružená k síťovému rozhraní se vyhodnocuje jako první. Veškerý provoz, kterému je povolený průchod přes skupinu zabezpečení sítě přidruženou k síťovému rozhraní, se pak vyhodnocuje skupinou zabezpečení sítě přidruženou k podsíti.

Ne vždy musíte mít přehled o použití skupin zabezpečení sítě na síťové rozhraní i podsíť. Agregovaná pravidla použitá na síťové rozhraní můžete snadno zobrazit v [platných pravidlech zabezpečení](virtual-network-manage-nsg-arm-portal.md) pro síťové rozhraní. Pomocí funkce [Ověření toku protokolu IP](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) v nástroji Azure Network Watcher můžete také určit, jestli je povolená komunikace směřující do síťového rozhraní nebo z něj. Tento nástroj vám řekne, jestli je povolená komunikace, a které pravidlo zabezpečení sítě povoluje nebo odepírá provoz.
 
> [!NOTE]
> Skupiny zabezpečení sítě se nepřidružují k síťovým rozhraním v modelu nasazení Resource Manager, ale k podsítím nebo virtuálním počítačům a cloudovým službám nasazeným pomocí modelu nasazení Classic. Další informace o modelech nasazení Azure najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Stejnou skupinu zabezpečení sítě můžete použít na libovolný počet jednotlivých síťových rozhraní a podsítí.

## <a name="security-rules"></a>Pravidla zabezpečení

Skupina zabezpečení sítě nemusí obsahovat žádná pravidla nebo může podle potřeby obsahovat libovolný počet pravidel v rámci [omezení](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) předplatného Azure. Každé pravidlo určuje následující vlastnosti:

|Vlastnost  |Vysvětlení  |
|---------|---------|
|Name (Název)|Jedinečný název v rámci skupiny zabezpečení sítě.|
|Priorita | Číslo v rozsahu od 100 do 4096. Pravidla se zpracovávají v pořadí podle priority, přičemž nižší čísla, která mají vyšší prioritu, se zpracovávají před vyššími čísly. Jakmile provoz odpovídá pravidlu, zpracování se zastaví. V důsledku toho se nezpracují žádná existující pravidla s nižší prioritou (vyšší čísla), která mají stejné atributy jako pravidla s vyšší prioritou.|
|Zdroj nebo cíl| Všechny nebo určitá IP adresa, blok CIDR (například 10.0.0.0/24), značka služby nebo skupina zabezpečení aplikace. Další informace o [značkách služeb](#service-tags) a [skupinách zabezpečení aplikací](#application-security-groups). Zadání rozsahu, značky služby nebo skupiny zabezpečení aplikace umožňuje vytvářet méně pravidel zabezpečení. Možnost zadat v pravidlu několik jednotlivých IP adres a rozsahů (není možné zadat více značek služeb ani skupin aplikací) je ve verzi Preview a označuje se jako rozšířená pravidla zabezpečení. Rozšířená pravidla zabezpečení je možné vytvářet pouze ve skupinách zabezpečení sítě vytvořených prostřednictvím modelu nasazení Resource Manager. Ve skupinách zabezpečení sítě vytvořených prostřednictvím modelu nasazení Classic není možné zadat více IP adres ani rozsahů IP adres.|
|Protocol (Protokol)     | TCP, UDP nebo Všechny, což zahrnuje protokoly TCP, UDP a ICMP. Samotný protokol ICMP nejde zadat, takže pokud potřebujete protokol ICMP, musíte použít možnost Všechny. |
|Směr| Určuje, jestli se pravidlo vztahuje na příchozí nebo odchozí provoz.|
|Rozsah portů     |Můžete zadat určitý port nebo rozsah portů. Můžete zadat například 80 nebo 10000-10005. Zadání rozsahů umožňuje vytvářet méně pravidel zabezpečení. Možnost zadat v pravidlu několik jednotlivých portů a rozsahů portů je ve verzi Preview a označuje se jako rozšířená pravidla zabezpečení. Než začnete používat rozšířená pravidla zabezpečení, přečtěte si důležité informace v části [Funkce verze Preview](#preview-features). Rozšířená pravidla zabezpečení je možné vytvářet pouze ve skupinách zabezpečení sítě vytvořených prostřednictvím modelu nasazení Resource Manager. Ve skupinách zabezpečení sítě vytvořených prostřednictvím modelu nasazení Classic není možné zadat několik portů ani rozsahů portů ve stejném pravidlu zabezpečení.   |
|Akce     | Povolení nebo odepření.        |

**Důležité informace**

- **Virtuální IP adresa uzlu hostitele:** Základní služby infrastruktury, například DHCP, DNS a sledování stavu, jsou poskytované prostřednictvím virtualizovaných IP adres hostitele 168.63.129.16 a 169.254.169.254. Tyto veřejné IP adresy patří společnosti Microsoft a jsou to jediné virtualizované IP adresy používané pro tento účel ve všech oblastech. Tyto IP adresy se mapují na fyzickou IP adresu počítače serveru (uzlu hostitele) hostujícího virtuální počítač. Uzel hostitele funguje jako přenos DHCP, rekurzivní překladač DNS a zdroj testů pro test stavu a zdroj testu pro test stavu nástroje pro vyrovnávání zatížení a test stavu počítače. Komunikace směřující na tyto IP adresy nepředstavuje útok. Pokud zablokujete provoz směřující na tyto IP adresy nebo z nich, virtuální počítač možná nebude fungovat správně.
- **Licencování (Služba správy klíčů):** Image systému Windows spuštěné ve virtuálních počítačích musí být licencované. Aby se zajistilo licencování, odesílají se žádosti o licenci na hostitelské servery Služby správy klíčů, které takové dotazy zpracovávají. Požadavek odchází přes port 1688.
- **Virtuální počítače ve fondech s vyrovnáváním zatížení:** Použitý zdrojový port a rozsah adres odpovídá zdrojovému počítači, a nikoli nástroji pro vyrovnávání zatížení. Cílový port a rozsah adres odpovídá cílovému počítači, a nikoli nástroji pro vyrovnávání zatížení.
- **Instance služeb Azure:** V podsítích virtuální sítě jsou nasazené instance několika služeb Azure, například HDInsight, prostředí aplikačních služeb a škálovací sady virtuálních počítačů. Před použitím skupiny zabezpečení sítě na podsíť, ve které je nasazený prostředek, se ujistěte, že znáte požadavky jednotlivých služeb na porty. Pokud odepřete porty, které služba vyžaduje, nebude správně fungovat. 

Pravidla zabezpečení jsou stavová. Pokud zadáte odchozí pravidlo zabezpečení pro všechny adresy například přes port 80, není potřeba zadávat příchozí pravidlo zabezpečení pro reakci na odchozí provoz. Příchozí pravidlo zabezpečení je potřeba zadat pouze v případě, že se komunikace zahajuje externě. Opačně to platí také. Pokud je přes port povolený příchozí provoz, není potřeba zadávat odchozí pravidlo zabezpečení pro reakci na provoz přes tento port. Informace o omezeních při vytváření pravidel zabezpečení najdete v článku o [omezeních Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Rozšířená pravidla zabezpečení

Rozšířená pravidla zjednodušují definici zabezpečení pro virtuální počítače tím, že umožňují definovat větší a složitější zásady zabezpečení sítě při použití menšího počtu pravidel. Můžete zkombinovat více portů a explicitních IP adres, značky služeb a skupiny zabezpečení aplikací do jediného, snadno pochopitelného pravidla zabezpečení. Rozšířená pravidla používejte v polích pravidla pro zdroj, cíl a port. Při vytváření pravidla můžete zadat několik explicitních IP adres, rozsahů CIDR a portů. Pokud chcete zjednodušit údržbu definice pravidla zabezpečení, zkombinujte rozšířená pravidla zabezpečení se značkami služeb nebo skupinami zabezpečení aplikací. 

Rozšířená pravidla jsou dostupná ve verzi Preview. Informace o omezeních při vytváření rozšířených pravidel zabezpečení najdete v článku o [omezeních Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Funkce ve verzi Preview nedosahují stejné úrovně dostupnosti a spolehlivosti jako služby v obecné verzi. Funkce jsou dostupné pouze v následujících oblastech: Východní USA, Západní USA, Západní USA 2, Západní střed USA, Austrálie – východ, Austrálie – jihovýchod a Velká Británie – jih.

## <a name="default-security-rules"></a>Výchozí pravidla zabezpečení

Pokud k podsíti nebo síťovému rozhraní není přidružena skupina zabezpečení sítě, je povolený veškerý příchozí i odchozí provoz do i z podsítě nebo síťového rozhraní. Jakmile se vytvoří skupina zabezpečení sítě, Azure v rámci této skupiny zabezpečení sítě vytvoří následující výchozí pravidla:

### <a name="inbound"></a>Příchozí

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Priorita|Zdroj|Zdrojové porty|Cíl|Cílové porty|Protocol (Protokol)|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Všechny|Povolit|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Priorita|Zdroj|Zdrojové porty|Cíl|Cílové porty|Protocol (Protokol)|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Všechny|Povolit|

#### <a name="denyallinbound"></a>DenyAllInbound

|Priorita|Zdroj|Zdrojové porty|Cíl|Cílové porty|Protocol (Protokol)|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Všechny|Odepřít|

### <a name="outbound"></a>Odchozí

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Všechny | Povolit |

#### <a name="allowinternetoutbound"></a>AllowVnetOutBound

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Všechny | Povolit |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Priorita|Zdroj|Zdrojové porty| Cíl | Cílové porty | Protocol (Protokol) | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Všechny | Odepřít |

Ve sloupcích **Zdroj** a **Cíl** jsou hodnoty *VirtualNetwork*, *AzureLoadBalancer* a *Internet* [značky služeb](#tags), a nikoli IP adresy. Hodnota **Všechny** ve sloupci Protokol zahrnuje protokoly TCP, UDP a ICMP. Při vytváření pravidla můžete zadat protokol TCP, UDP nebo Všechny, ale nemůžete zadat samotný protokol ICMP. Proto pokud vaše pravidlo vyžaduje protokol ICMP, musíte jako protokol vybrat hodnotu *Všechny*. Hodnota *0.0.0.0/0* ve sloupcích **Zdroj** a **Cíl** představuje všechny adresy.
 
Výchozí pravidla nemůžete odebrat, ale můžete je přepsat vytvořením pravidel s vyšší prioritou.

## <a name="service-tags"></a>Značky služeb

 Značka služby představuje skupinu předpon IP adres a tím pomáhá minimalizovat složitost vytváření pravidla zabezpečení. Nemůžete vytvořit vlastní značku služby ani určit, které IP adresy jsou ve značce zahrnuté. Předpony adres zahrnuté ve značce služby spravuje Microsoft, a pokud se adresy změní, automaticky značku služby aktualizuje. Značky služeb můžete používat místo konkrétních IP adres při vytváření pravidel zabezpečení. Pro použití v definici pravidla zabezpečení jsou k dispozici následující značky služeb. Jejich názvy se mírně liší mezi [modely nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

* **VirtualNetwork** (*Resource Manager*) (*VIRTUAL_NETWORK** v případě modelu Classic): Tato značka zahrnuje adresní prostor virtuální sítě (všechny rozsahy CIDR definované pro virtuální síť), všechny připojené místní adresní prostory a [partnerské](virtual-network-peering-overview.md) virtuální sítě nebo virtuální sítě připojené k [bráně virtuální sítě](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* **AzureLoadBalancer** (Resource Manager) (**AZURE_LOADBALANCER** v případě klasického modelu): Tato značka označuje nástroj pro vyrovnávání zatížení infrastruktury Azure. Značka se přeloží na [IP adresu datacentra Azure](https://www.microsoft.com/download/details.aspx?id=41653), kde mají původ testy stavu Azure. Pokud nepoužíváte nástroj pro vyrovnávání zatížení Azure, můžete toto pravidlo přepsat.
* **Internet** (Resource Manager) (**INTERNET** v případě modelu Classic): Tato značka označuje adresní prostor veřejných IP adres Azure. Adresy zahrnuté v této značce jsou uvedené v pravidelně aktualizovaném dokumentu [Veřejný prostor IP adres vlastněný Azure](https://www.microsoft.com/download/details.aspx?id=41653).
* **AzureTrafficManager** (pouze Resource Manager): Tato značka označuje adresní prostor IP adres pro službu Azure Traffic Manager.
* **Storage** (pouze Resource Manager): Tato značka označuje adresní prostor IP adres pro službu Azure Storage. Pokud jako hodnotu zadáte *Storage*, provoz směřující do úložiště se povolí nebo zakáže. Pokud chcete povolit přístup k úložišti pouze v konkrétní [oblasti](https://azure.microsoft.com/regions), můžete zadat tuto oblast. Pokud například chcete povolit přístup pouze ke službě Azure Storage v oblasti Východní USA, můžete jako značku služby zadat *Storage.EastUS*. Další dostupné značky regionálních služeb: Storage.AustraliaEast, Storage.AustraliaSoutheast, Storage.EastUS, Storage.UKSouth, Storage.WestCentralUS, Storage.WestUS a Storage.WestUS2. Značka představuje službu, ale ne konkrétní instance služby. Značka například představuje službu Azure Storage, ale ne konkrétní účet služby Azure Storage.
* **Sql** (pouze Resource Manager): Tato značka označuje předpony adres služeb Azure SQL Database a Azure SQL Data Warehouse. Pro tuto značku služby můžete zadat pouze konkrétní oblasti. Pokud například chcete povolit přístup pouze ke službě Azure SQL Database v oblasti Východní USA, můžete jako značku služby zadat *Storage.EastUS*. Pouze Sql není možné zadat pro všechny oblasti Azure, jednotlivé oblasti je potřeba zadat ručně. Další dostupné regionální značky služeb: Sql.AustraliaEast, Sql.AustraliaSoutheast, Sql.EastUS, Sql.UKSouth, Sql.WestCentralUS, Sql.WestUS a Sql.WestUS2. Značka představuje službu, ale ne konkrétní instance služby. Značka například představuje službu Azure SQL Database, ale ne konkrétní databázi SQL Azure.

> [!WARNING]
> Značky služeb AzureTrafficManager, Storage a Sql jsou dostupné ve verzi Preview. Funkce ve verzi Preview nedosahují stejné úrovně dostupnosti a spolehlivosti jako služby v obecné verzi. Značky služeb jsou dostupné pouze v následujících oblastech: Východní USA, Západní USA, Západní USA 2, Západní střed USA, Austrálie – východ, Austrálie – jihovýchod a Velká Británie – jih.

> [!NOTE]
> Pokud implementujete koncový bod služby virtuální sítě pro službu, jako je Azure Storage nebo Azure SQL Database, Azure pro službu přidá trasu k podsíti virtuální sítě. Předpony adres pro trasu jsou stejné předpony adres nebo rozsahy CIDR jako u odpovídající značky služby.

## <a name="application-security-groups"></a>Skupiny zabezpečení aplikací

Skupiny zabezpečení aplikací umožňují konfigurovat zabezpečení sítě jako přirozené rozšíření struktury aplikace. Můžete seskupovat virtuální počítače a na základě těchto skupin definovat zásady zabezpečení sítě. Tato funkce umožňuje opakovaně používat škálované zásady zabezpečení bez potřeby ruční údržby explicitních IP adres. O složitost explicitních IP adres a několika skupin pravidel se stará platforma a vy se tak můžete zaměřit na obchodní logiku.

Skupinu zabezpečení aplikace můžete zadat jako zdroj a cíl v pravidlu zabezpečení. Po definování zásady zabezpečení můžete vytvořit virtuální počítače a přiřadit jejich síťová rozhraní ke skupině zabezpečení aplikace. Zásada se použije na základě členství jednotlivých síťových rozhraní virtuálního počítače ve skupině zabezpečení aplikace. Následující příklad ukazuje, jak můžete použít skupinu zabezpečení aplikace pro všechny webové servery ve vašem předplatném:

1. Vytvořte skupinu zabezpečení aplikace *WebServers*.
2. Vytvořte skupinu zabezpečení sítě *MyNSG*.
3. Ve skupině zabezpečení sítě vytvořte příchozí pravidlo zabezpečení, přičemž jako zdrojovou adresu zadejte značku služby *Internet*, jako cílovou adresu zadejte skupinu zabezpečení aplikace *WebServers* a povolte porty 80 a 443.
4. Nasaďte virtuální počítač s aplikací webového serveru. Určete, že síťové rozhraní ve virtuálním počítači je členem skupiny zabezpečení aplikace *WebServers*. Do virtuálního počítače se pak povolí porty 80 a 443. Tyto porty jsou povolené také do dalších webových serverů, které vytvoříte a učiníte členy skupiny zabezpečení aplikace *WebServers*. 

Pokud jste vytvořili jiná pravidla a jako cíl jste zadali jiné skupiny zabezpečení aplikací, taková pravidla se na webové servery z předchozího příkladu nepoužijí. Pravidla určující skupinu zabezpečení aplikace se použijí pouze na síťová rozhraní, která jsou členy příslušné skupiny zabezpečení aplikace. Skupiny zabezpečení aplikací v kombinaci s rozšířenými pravidly zabezpečení a značkami služeb umožňují vytvářet pro účely správy zabezpečení sítě v rámci předplatného minimální počet skupin zabezpečení sítě.
 
Další informace o omezeních při vytváření skupin zabezpečení aplikací a jejich zadávání v pravidlech zabezpečení najdete v článku o [omezeních Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Skupiny zabezpečení aplikací jsou dostupné ve verzi Preview. Před použitím skupiny zabezpečení sítě je potřeba se zaregistrovat k jejich použití dokončením kroků 1–5 v části [Vytvoření skupiny zabezpečení sítě s použitím skupin zabezpečení aplikací](create-network-security-group-preview.md#powershell) a přečíst si důležité informace v části [Funkce verze Preview](#preview-features). Během období Preview jsou skupiny zabezpečení aplikací omezené na rozsah virtuální sítě. Partnerské virtuální sítě s křížovými odkazy na skupiny zabezpečení aplikací ve skupině zabezpečení sítě se nepoužijí. 

Funkce ve verzi Preview nedosahují stejné úrovně dostupnosti a spolehlivosti jako služby v obecné verzi. Abyste mohli začít používat skupiny zabezpečení aplikací, musíte se nejprve zaregistrovat k jejich použití. Funkce jsou aktuálně dostupné pouze v následujících oblastech: Západní střed USA.

## <a name="next-steps"></a>Další kroky

* Dokončení kurzu [Vytvoření skupiny zabezpečení sítě](virtual-networks-create-nsg-arm-pportal.md)
* Dokončení kurzu [Vytvoření skupiny zabezpečení sítě s použitím skupin zabezpečení aplikací](create-network-security-group-preview.md)

