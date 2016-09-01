<properties
   pageTitle="Přehled protokolu BGP se službou Azure VPN Gateways | Microsoft Azure"
   description="Tento článek obsahuje přehled protokolu BGP se službou Azure VPN Gateways."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>

# Přehled protokolu BGP se službou Azure VPN Gateways

Tento článek obsahuje přehled o podpoře protokolu BGP (Border Gateway Protocol) ve službě Azure VPN Gateways.

## Informace o protokolu BGP

BGP je standardní směrovací protokol, na internetu běžně používaný k výměně informací o směrování a dostupnosti mezi dvěma nebo více sítěmi. Pokud protokol BGP použijete v rámci virtuálních sítí Azure, umožní službám Azure VPN Gateway a místním zařízením VPN, která se nazývají partnerské uzly protokolu BGP nebo sousedé BGP, výměnu „tras“ informujících obě brány o dostupnosti a dosažitelnosti předpon, které procházejí těmito bránami nebo trasami. Protokol BGP také umožňuje směrování přenosu mezi více sítěmi pomocí šíření tras, které brána s protokolem BGP zjistí od jednoho partnerského uzlu protokolu BGP, do všech dalších partnerských uzlů protokolu BGP.
 
### Proč používat protokol BGP?

Protokol BGP je volitelná funkce, kterou můžete použít s trasovými bránami Azure VPN. Dříve než povolíte tuto funkci byste se měli ujistit, že vaše místní zařízení VPN podporuje protokol BGP. Brány Azure VPN a místní zařízení VPN můžete nadále používat i bez protokolu BGP. Je to stejné jako používání statických tras (bez protokolu BGP) *oproti* používání dynamického směrování s protokolem BGP mezi vaší sítí a Azure.

Existuje několik výhod a nových schopností při použití protokolu BGP:

#### Podpora automatických a flexibilních aktualizací předpon

U protokolu BGP musíte pouze deklarovat minimální předponu určitému partnerskému uzlu protokolu BGP přes tunel S2S VPN s protokolem IPsec. Ta může být malá jako předpona hostitele (/32) IP adresy partnerského uzlu protokolu BGP vašeho místního zařízení VPN. Můžete určit, které předpony místní sítě chcete inzerovat do Azure pro umožnění přístupu službě Azure Virtual Network.
    
Můžete také inzerovat delší předpony, které mohou obsahovat některé předpony adres vaší virtuální sítě, třeba velký adresní prostor privátních IP adres (např. 10.0.0.0/8). Pamatujte na to, že se předpony nesmí shodovat s žádnými předponami vaší virtuální sítě. Trasy shodné s předponami vaší virtuální sítě budou odmítnuty.

>[AZURE.IMPORTANT] Inzerování výchozí trasy (0.0.0.0/0) branám Azure VPN bude v současnosti blokováno. Až bude tato funkce povolena, oznámíme to.

#### Podpora více tunelů mezi virtuální sítí a místním webem s automatickým převzetím služeb při selhání na základě protokolu BGP

Můžete vytvořit více připojení mezi virtuální sítí Azure a místními zařízeními VPN ve stejném umístění. Tato schopnost poskytuje více tunelů (cest) mezi těmito dvěma sítěmi v konfiguraci aktivní-aktivní. Dojde-li k odpojení jednoho tunelu, odpovídající trasy se odvolají přes protokol BGP a provoz se automaticky přesune na zbývající tunely.
    
Následující diagram ukazuje jednoduchý příklad tohoto vysoce dostupného nastavení:
    
![Více aktivních cest](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### Podpora směrování přenosu mezi místními sítěmi a více virtuálními sítěmi Azure

Protokol BGP umožňuje více branám zjišťovat a šířit předpony z různých sítí, ať jsou připojeny přímo nebo nepřímo. To umožňuje směrování přenosu pomocí bran Azure VPN mezi vašimi místními weby nebo napříč více službami Azure Virtual Network.
    
Následující diagram ukazuje příklad topologie vícenásobného předávání s více cestami, které mohou přenášet provoz mezi dvěma místními sítěmi přes brány Azure VPN v rámci služby MSN:

![Vícenásobné předávání přenosu](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## Protokol BGP – nejčastější dotazy


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## Další kroky

V tématu [Začínáme s protokolem BGP na branách Azure VPN](./vpn-gateway-bgp-resource-manager-ps.md) najdete postup konfigurace protokolu BGP pro připojení mezi různými místy a pro připojení mezi virtuálními sítěmi (VNet-to-VNet).




<!--HONumber=Aug16_HO4-->


