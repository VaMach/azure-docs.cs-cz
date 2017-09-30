---
title: "Partnerské vztahy virtuálních sítí v Azure | Dokumentace Microsoftu"
description: "Seznamte se s partnerskými vztahy virtuálních sítí v Azure."
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: narayan;anavin
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 082cd8a6cf50f76c89fe5995047396c734f83034
ms.contentlocale: cs-cz
ms.lasthandoff: 09/25/2017

---
# <a name="virtual-network-peering"></a>Partnerské vztahy virtuálních sítí

[Virtuální síť Azure](virtual-networks-overview.md) je váš vlastní privátní síťový prostor v Azure, který umožňuje zabezpečené propojení prostředků Azure mezi sebou.

Partnerské vztahy virtuálních sítí umožňují bezproblémové propojení virtuálních sítí. Po navázání partnerského vztahu se virtuální sítě pro účely připojení jeví jako jedna síť. Virtuální počítače v partnerských virtuálních sítích mezi sebou můžou přímo komunikovat.
Provoz mezi virtuálními počítači v partnerských virtuálních sítích je směrován přes páteřní infrastrukturu Microsoftu v zásadě stejně jako mezi virtuálními počítači ve stejné virtuální síti pouze přes *privátní* IP adresy.

>[!IMPORTANT]
> Partnerský vztah můžete navázat mezi virtuálními sítěmi v různých oblastech Azure. Tato funkce je aktuálně ve verzi Preview. Můžete [zaregistrovat své předplatné pro verzi Preview](virtual-network-create-peering.md). Vytváření partnerských vztahů virtuálních sítí ve stejných oblastech je všeobecně dostupné.
>

Mezi výhody použití partnerských vztahů virtuálních sítí patří:

* Provoz procházející partnerskými vztahy virtuálních sítí je naprosto privátní. Prochází přes páteřní síť Microsoftu a nepoužívá se přitom veřejný internet ani brány.
* Nízká latence a velká šířka pásma při propojení prostředků v různých virtuálních sítích.
* Možnost po navázání partnerského vztahu používat ve virtuální síti prostředky z jiné virtuální sítě.
* Partnerský vztah virtuálních sítí pomáhá s přenosem dat mezi předplatnými Azure, modely nasazení a oblastmi Azure (Preview).
* Možnost navázání partnerského vztahu mezi virtuálními sítěmi vytvořenými prostřednictvím modelu nasazení Azure Resource Manager, nebo mezi jednou virtuální sítí vytvořenou prostřednictvím modelu nasazení Resource Manager a druhou vytvořenou prostřednictvím modelu nasazení Classic. Další informace o rozdílech mezi těmito dvěma modely nasazení Azure najdete v článku [Vysvětlení modelů nasazení Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="requirements-constraints"></a>Požadavky a omezení

* Vytváření partnerských vztahů virtuálních sítí ve stejné oblasti je všeobecně dostupné. Vytváření partnerských vztahů virtuálních sítí v různých oblastech je aktuálně ve verzi Preview v oblastech USA – středozápad, Střední Kanada a USA – západ 2. Můžete [zaregistrovat své předplatné pro verzi Preview](virtual-network-create-peering.md).
    > [!WARNING]
    > Partnerské vztahy virtuálních sítí vytvořené v tomto scénáři nemusí dosahovat stejné úrovně dostupnosti a spolehlivosti jako scénáře ve verzi všeobecné dostupnosti. Partnerské vztahy virtuálních sítí můžou mít omezené možnosti a nemusí být dostupné ve všech oblastech Azure. Nejaktuálnější oznámení o dostupnosti a stavu této funkce najdete na stránce [Aktualizace služby Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network).

* Adresní prostory IP adres partnerských virtuálních sítí se nesmí překrývat.
* Adresní prostory nelze do virtuálních sítí přidávat ani je z nich odstraňovat, jakmile je mezi dvěma virtuálními sítěmi vytvořen partnerský vztah.
* Partnerský vztah virtuálních sítí se navazuje mezi dvěma virtuálními sítěmi. Nevzniká žádný odvozený tranzitivní vztah přes partnerské vztahy. Pokud je například virtuální síť A v partnerském vztahu s virtuální sítí B a virtuální síť B v partnerském vztahu s virtuální sítí C, pak virtuální síť A *není* v partnerském vztahu s virtuální sítí C.
* Partnerský vztah lze navázat mezi virtuálními sítěmi ve dvou různých předplatných za předpokladu, že navázání tohoto vztahu u obou předplatných autorizuje uživatel s oprávněním správce (viz [konkrétní oprávnění](create-peering-different-deployment-models-subscriptions.md#permissions)) a že předplatná jsou přidružená ke stejnému tenantovi Azure Active Directory. K propojení virtuálních sítí v předplatných přidružených k různým tenantům Active Directory můžete použít [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
* Mezi virtuálními sítěmi lze navázat partnerský vztah, pokud jsou obě vytvořené prostřednictvím modelu nasazení Resource Manager, nebo pokud je jedna virtuální síť vytvořena prostřednictvím modelu nasazení Resource Manager a druhá prostřednictvím modelu nasazení Classic. Nejde ale navázat partnerský vztah mezi virtuálními sítěmi, které obě používají model nasazení Classic. K propojení virtuálních sítí vytvořených prostřednictvím modelu nasazení Classic můžete použít službu [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
* Ačkoli komunikace mezi virtuálními počítači ve virtuálních sítích propojených partnerským vztahem nemá žádná další omezení šířky pásma, stále platí omezení šířky pásma na základě velikosti virtuálního počítače. Další informace o maximální šířce pásma pro různé velikosti virtuálních počítačů najdete v článcích o velikosti virtuálních počítačů pro [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) a [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

     ![Základní partnerské vztahy virtuálních sítí](./media/virtual-networks-peering-overview/figure03.png)

## <a name="connectivity"></a>Připojení

Po navázání partnerského vztahu mezi virtuálními sítěmi se prostředky v některé z virtuálních sítí můžou přímo připojit k prostředkům v partnerské virtuální síti.

Latence sítě mezi virtuálními počítači v partnerských virtuálních sítích ve stejné oblasti je stejná jako latence v rámci jedné virtuální sítě. Propustnost sítě závisí na šířce pásma, která je pro daný virtuální počítač povolená na základě jeho velikosti. S partnerským vztahem zde nejsou žádná další omezení týkající se šířky pásma.

Provoz mezi virtuálními počítači v partnerských virtuálních sítích je směrován přímo přes páteřní infrastrukturu Microsoftu, ne prostřednictvím brány ani přes veřejný internet.

Virtuální počítače ve virtuální síti mají přístup k internímu nástroji pro vyrovnávání zatížení v partnerské virtuální síti ve stejné oblasti. Podpora interního nástroje pro vyrovnávání zatížení se ve verzi Preview nevztahuje na virtuální sítě v globálním partnerském vztahu. Všeobecně dostupná verze globálních partnerských vztahů virtuálních sítí bude zahrnovat podporu interního nástroje pro vyrovnávání zatížení.

Skupiny zabezpečení sítě je možné v obou virtuálních sítích v případě potřeby použít k blokování přístupu do jiných virtuálních sítí nebo podsítí.
Při konfiguraci partnerského vztahu virtuálních sítí můžete otevřít nebo zavřít pravidla skupiny zabezpečení sítě mezi virtuálními sítěmi. Pokud otevřete úplné propojení mezi partnerskými virtuálními sítěmi (výchozí možnost), můžete pomocí skupin zabezpečení sítě u konkrétních podsítí nebo virtuálních počítačů blokovat nebo odepřít specifický přístup. Další informace o skupinách zabezpečení sítě najdete v článku [Přehled skupin zabezpečení sítě](virtual-networks-nsg.md).

## <a name="service-chaining"></a>Řetězení služeb

Můžete nakonfigurovat uživatelsky definované trasy, které jsou směrované na virtuální počítače v partnerských virtuálních sítích jako na IP adresy pro další směrování, a povolit tak řetězení služeb. Řetězení služeb vám umožní směrovat přenos dat z jedné virtuální sítě do virtuálního zařízení v partnerské virtuální síti s použitím uživatelsky definovaných tras.

Můžete také fakticky vytvářet prostředí hvězdicovitého typu, kde centrální síť může být hostitelem komponent infrastruktury, například síťového virtuálního zařízení. Všechny uvedené virtuální sítě pak můžou vytvořit partnerský vztah s centrální virtuální sítí. Provoz může probíhat přes virtuální síťová zařízení, která běží v centrální virtuální síti. Partnerské vztahy virtuálních sítí tedy umožňují, aby IP adresou dalšího směrování v uživatelsky definované trase byla IP adresa virtuálního počítače v partnerské virtuální síti. Další informace o trasách definovaných uživatelem najdete v článku s [přehledem tras definovaných uživatelem](virtual-networks-udr-overview.md). Zjistěte, jak vytvořit [hvězdicovou síťovou topologii](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering).

## <a name="gateways-and-on-premises-connectivity"></a>Brány a místní připojení

Každá virtuální síť, bez ohledu na to, jestli má navázaný partnerský vztah s jinou virtuální sítí, může mít stále svou vlastní bránu a používat ji pro připojení k místní síti. Můžete také nakonfigurovat [připojení mezi virtuálními sítěmi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) pomocí bran, a to i v situaci, kdy jsou tyto virtuální sítě v partnerském vztahu.

Když jsou nakonfigurovány obě možnosti pro propojení virtuálních sítí, přenos dat mezi těmito sítěmi probíhá s použitím konfigurace partnerského vztahu (t.j. přes páteřní síť Azure).

Když je mezi virtuálními sítěmi navázán partnerský vztah ve stejné oblasti, můžete také v partnerské virtuální síti nakonfigurovat bránu, která bude sloužit jako tranzitní bod pro místní síť. V tomto případě virtuální síť, která používá vzdálenou bránu, nemůže mít svou vlastní bránu. Virtuální síť může mít pouze jednu bránu. Může jí být buď místní brána, nebo vzdálená brána (v partnerské virtuální síti), jak ukazuje následující obrázek:

![přenos dat při partnerských vztazích virtuálních sítí](./media/virtual-networks-peering-overview/figure04.png)

Průchod branou není podporován v případě, že jde o partnerský vztah mezi virtuálními sítěmi vytvořenými pomocí různých modelů nasazení nebo v různých oblastech. Pokud chcete používat průchod branou, je nutné, aby byly obě partnerské virtuální sítě vytvořené pomocí modelu nasazení Resource Manager a ve stejné oblasti. Virtuální sítě v globálním partnerském vztahu aktuálně průchod branou nepodporují.

Po nastavení partnerského vztahu mezi virtuálními sítěmi sdílejícími jedno připojení Azure ExpressRoute přenos dat prochází skrze partnerský vztah (tj. prostřednictvím páteřní sítě Azure). K připojení k místnímu okruhu lze v obou sítích nadále používat místní brány. Alternativním postupem je použití sdílené brány a konfigurace průchodu pro místní připojení.

## <a name="permissions"></a>Oprávnění

Navázání partnerského vztahu virtuální sítě je privilegovaná operace. Jde o samostatnou funkci v rámci oboru názvů VirtualNetworks. Uživatel může dostat specifická práva k autorizaci partnerských vztahů. Uživatel, který má přístup pro čtení a zápis do virtuální sítě, zdědí tato práva automaticky.

Uživatel, který je buď správcem nebo privilegovaným uživatelem pro navázání partnerských vztahů, může iniciovat operaci navázání partnerství s jinou virtuální sítí. Minimální úroveň požadovaných oprávnění je Přispěvatel sítě. Pokud i na druhé straně existuje odpovídající požadavek a jsou splněny i další podmínky, partnerský vztah bude navázán.

Pokud jste například navazovali partnerský vztah mezi virtuálními sítěmi myvirtual networkA a myvirtual networkB, váš účet musí mít pro každou virtuální síť přiřazenou minimálně následující roli nebo oprávnění:

|Virtuální síť|Model nasazení|Role|Oprávnění|
|---|---|---|---|
|myvirtual networkA|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|–|
|myvirtual networkB|Resource Manager|[Přispěvatel sítě](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Classic|[Přispěvatel klasických sítí](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

## <a name="monitor"></a>Monitorování

Pokud navazujete partnerský vztah mezi dvěma virtuálními sítěmi vytvořenými prostřednictvím Resource Manageru, v partnerském vztahu musí být nakonfigurován partnerský vztah pro každou virtuální síť.
Stav partnerského propojení můžete monitorovat. Partnerský vztah může být v jednom z následujících stavů:

* **Zahájeno:** Když vytvoříte partnerské připojení z první virtuální sítě k druhé virtuální síti, stav partnerského vztahu je Zahájeno.

* **Připojeno:** Když vytvoříte partnerské připojení z druhé virtuální sítě k první virtuální síti, stav partnerského vztahu je Připojeno. Pokud zobrazíte stav partnerského vztahu pro první virtuální síť, uvidíte, že se stav změnil ze Zahájeno na Připojeno. Partnerský vztah není úspěšně navázán, dokud stav partnerského vztahu pro obě virtuální sítě není Připojeno.

* **Odpojeno:** Pokud se po navázání připojení jedno z partnerských propojení odstraní, stav partnerského vztahu je Odpojeno.

## <a name="troubleshoot"></a>Řešení potíží

Pokud chcete řešit potíže s tokem provozu v rámci partnerského propojení, můžete [zkontrolovat efektivní trasy](virtual-network-routes-troubleshoot-portal.md).

Pomocí [kontroly připojení](../network-watcher/network-watcher-connectivity-portal.md) ve službě Network Watcher můžete také řešit potíže s připojením k virtuálnímu počítači v partnerské virtuální síti. Kontrola připojení umožňuje zobrazit směrování přímo ze síťového rozhraní zdrojového virtuálního počítače do síťového rozhraní cílového virtuálního počítače.

## <a name="limits"></a>Omezení

Existují omezení týkající se počtu partnerských vztahů, které lze vytvořit pro jednu virtuální síť. Výchozí počet partnerských vztahů je 50. Počet partnerských vztahů můžete zvýšit. Další informace najdete v tématu [Omezení pro sítě v Azure](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Ceny

Za příchozí a výchozí přenos využívající partnerské propojení virtuálních sítí se účtuje nominální poplatek. Další informace najdete na [stránce s cenami](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Další kroky

* Dokončete kurz o partnerském vztahu virtuálních sítí. Partnerský vztah virtuálních sítí se vytváří mezi virtuálními sítěmi vytvořenými prostřednictvím stejného modelu nebo různých modelů nasazení, které existují ve stejném předplatném nebo různých předplatných. Dokončete kurz pro jeden z následujících scénářů:

    |Model nasazení Azure  | Předplatné  |
    |---------|---------|
    |Obě Resource Manager |[Stejné](virtual-network-create-peering.md)|
    | |[Různé](create-peering-different-subscriptions.md)|
    |Jedna Resource Manager, druhá Classic     |[Stejné](create-peering-different-deployment-models.md)|
    | |[Různé](create-peering-different-deployment-models-subscriptions.md)|

* Zjistěte, jak vytvořit [hvězdicovou síťovou topologii](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual network-peering).
* Prostudujte si všechna [nastavení partnerského vztahu virtuálních sítí a jejich změny](virtual-network-manage-peering.md).

