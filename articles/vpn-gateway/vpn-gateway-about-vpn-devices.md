---
title: "Zařízení VPN pro připojení mezi různými místy pomocí Azure | Dokumentace Microsoftu"
description: "Tento článek probírá zařízení VPN a parametry protokolu IPsec pro připojení mezi různými místy pomocí VPN Gateway typu S2S. V článku najdete také odkazy na pokyny a příklady pro konfiguraci."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: azure-resource-manager, azure-service-management
ms.assetid: ba449333-2716-4b7f-9889-ecc521e4d616
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/14/2017
ms.author: yushwang;cherylmc
ms.translationtype: Human Translation
ms.sourcegitcommit: db18dd24a1d10a836d07c3ab1925a8e59371051f
ms.openlocfilehash: 8a7419c7a759060dc91f11ec94085ff0afd4a457
ms.contentlocale: cs-cz
ms.lasthandoff: 06/15/2017


---
# <a name="about-vpn-devices-and-ipsecike-parameters-for-site-to-site-vpn-gateway-connections"></a>O zařízeních VPN a o parametrech protokolu IPsec/IKE pro připojení typu Site-to-Site ke službě VPN Gateway

Pro konfiguraci připojení VPN typu Site-to-Site (S2S) mezi různými místy pomocí brány VPN Gateway je potřeba zařízení VPN. Připojení typu Site-to-Site lze použít k vytvoření hybridního řešení, nebo kdykoli chcete zabezpečit připojení mezi místními a virtuálními sítěmi. Tento článek obsahuje seznam parametrů protokolu IPsec/IKE pro služby Azure VPN Gateway a seznam ověřených zařízení sítě VPN připojujících se ke službám Azure VPN Gateway.


> [!IMPORTANT]
> Pokud mezi místními zařízeními VPN a bránami VPN Azure dochází k problémům s připojením, vyhledejte informace v části [Známé problémy s kompatibilitou zařízení](#known). 


###<a name="items-to-note-when-viewing-the-tables"></a>Při procházení tabulek si všimněte:

* Došlo ke změně terminologie pro služby Azure VPN Gateway. Nedošlo k žádné změně funkce. Změnily se pouze názvy.
  * Statické směrování = PolicyBased
  * Dynamické směrování = RouteBased
* Specifikace pro vysokovýkonné brány VPN a brány VPN typu RouteBased jsou stejné, není-li uvedeno jinak. Například ověřená zařízení VPN, která jsou kompatibilní s bránami VPN typu RouteBased, budou kompatibilní také s vysokovýkonnou bránou VPN Azure.

> [!NOTE]
> Při konfiguraci připojení typu Site-to-Site je pro vaše zařízení VPN vyžadována veřejná IP adresa IPv4.
>                

## <a name="devicetable"></a>Ověřená zařízení VPN a průvodci konfigurací zařízení
Ve spolupráci s dodavateli zařízení jsme ověřili sadu standardních zařízení VPN. Všechna zařízení v řadách zařízení obsažených v následujícím seznamu musí fungovat s bránami Azure VPN Gateway. Podle článku [Informace o bránách VPN](vpn-gateway-about-vpngateways.md) ověřte typ brány, kterou budete muset vytvořit pro řešení, které chcete konfigurovat.

Pomoc s konfigurací zařízení VPN najdete pod odkazy, které odpovídají příslušné řadě zařízení. Při poskytování odkazů na pokyny se snažíme maximálně vyhovět. Pro podporu zařízení VPN kontaktujte výrobce zařízení.

|**Dodavatel**          |**Řada zařízení**     |**Minimální verze operačního systému** |**Pokyny ke konfiguraci PolicyBased** |**Pokyny ke konfiguraci RouteBased** |
| ---                | ---                  | ---                   | ---            | ---           |
| A10 Networks, Inc. |Thunder CFW           |ACOS 4.1.1             |Není kompatibilní  |[Průvodce konfigurací](https://www.a10networks.com/resources/deployment-guides/a10-thunder-cfw-ipsec-vpn-interoperability-azure-vpn-gateways)|
| Allied Telesis     |Směrovače VPN řady AR |2.9.2                  |Připravuje se     |Není kompatibilní  |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall řady F |PolicyBased: 5.4.3<br>RouteBased: 6.2.0 |[Průvodce konfigurací](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Průvodce konfigurací](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall řady X |Barracuda Firewall 6.5 |[Průvodce konfigurací](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Není kompatibilní |
| Brocade            |Vyatta 5400 vRouter   |Virtual Router 6.6R3 GA|[Průvodce konfigurací](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Není kompatibilní |
| Check Point |Security Gateway |R77.30 |[Průvodce konfigurací](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Průvodce konfigurací](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco              |ASA       |8.3 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |Není kompatibilní |
| Cisco |ASR |PolicyBased: iOS 15.1<br>RouteBased: iOS 15.2 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |PolicyBased: iOS 15.0<br>RouteBased*: iOS 15.1 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Ukázky konfigurace*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |10.1 a vyšší |[Průvodce konfigurací](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Není kompatibilní |
| F5 |Řada BIG-IP |12.0 |[Průvodce konfigurací](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Průvodce konfigurací](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.4.2 |  |[Průvodce konfigurací](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |
| Internet Initiative Japan (IIJ) |Řada SEIL |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Průvodce konfigurací](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Není kompatibilní |
| Juniper |SRX |PolicyBased: JunOS 10.2<br>Routebased: JunOS 11.4 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |Řada J |PolicyBased: JunOS 10.4r9<br>RouteBased: JunOS 11.4 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Ukázky konfigurace](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Služba Směrování a vzdálený přístup |Windows Server 2012 |Není kompatibilní |[Ukázky konfigurace](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control Security Gateway |– |[Průvodce konfigurací](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |Není kompatibilní |
| Openswan |Openswan |2.6.32 |(Připravuje se) |Není kompatibilní |
| Palo Alto Networks |Všechna zařízení se systémem PAN-OS |PAN-OS<br>PolicyBased: 6.1.5 nebo novější<br>RouteBased: 7.1.4 |[Průvodce konfigurací](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Průvodce konfigurací](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| SonicWall |Řada TZ, řada NSA<br>Řada SuperMassive<br>Řada E-Class NSA |SonicOS 5.8.x<br>SonicOS 5.9.x<br>SonicOS 6.x |[Průvodce konfigurací pro SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Průvodce konfigurací pro SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[Průvodce konfigurací pro SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Průvodce konfigurací pro SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| WatchGuard |Všechny |Fireware XTM<br> PolicyBased: v11.11.x<br>RouteBased: v11.12.x |[Průvodce konfigurací](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Průvodce konfigurací](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

(*) Směrovače řady ISR 7200 podporují pouze sítě VPN typu PolicyBased.

## <a name="additionaldevices"></a>Neověřená zařízení VPN
Nevidíte-li své zařízení v tabulce Ověřená zařízení VPN, stále je možné, že bude fungovat s připojením typu Site-to-Site. Kvůli další podpoře a pokynům ke konfiguraci se obraťte na výrobce zařízení.

## <a name="editing"></a>Ukázky úpravy konfigurace zařízení
Po stažení ukázky konfigurace zařízení VPN budete muset nahradit některé hodnoty tak, aby odpovídaly nastavení vašeho prostředí.

### <a name="to-edit-a-sample"></a>Chcete-li upravit ukázku:

1. Otevřete ukázku pomocí Poznámkového bloku.
2. Vyhledejte všechny řetězce *text* a nahraďte je hodnotami, které odpovídají vašemu prostředí. Nezapomeňte zahrnout < a >. Zadané názvy by měly být jedinečné. Pokud příkaz nefunguje, obraťte se na dokumentaci výrobce zařízení.

| **Text v ukázce** | **Změňte na** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |Zvolený název pro tento objekt. Příklad: myOnPremisesNetwork |
| &lt;RP_AzureNetwork&gt; |Zvolený název pro tento objekt. Příklad: myAzureNetwork |
| &lt;RP_AccessList&gt; |Zvolený název pro tento objekt. Příklad: myAzureAccessList |
| &lt;RP_IPSecTransformSet&gt; |Zvolený název pro tento objekt. Příklad: myIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |Zvolený název pro tento objekt. Příklad: myIPSecCryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |Zadejte rozsah. Příklad: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Zadejte masku podsítě. Příklad: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |Zadejte místní rozsah. Příklad: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |Zadejte masku místní podsítě. Příklad: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Tato informace je specifická pro vaši virtuální síť a najdete ji v Portálu pro správu jako **IP adresa brány**. |
| &lt;SP_PresharedKey&gt; |Tato informace je specifická pro vaši virtuální síť a najdete ji v Portálu pro správu jako Správa klíče. |

## <a name="ipsec"></a>Parametry protokolu IPsec/IKE
> [!NOTE]
> Přestože jsou hodnoty uvedené v následující tabulce podporovány službou Azure VPN Gateway, v současné době neexistuje způsob, jak zadat nebo vybrat konkrétní kombinaci algoritmů nebo parametrů ze služby Azure VPN Gateway. Musíte zadat jakákoli omezení ze strany místního zařízení VPN. Kromě toho musíte uchytit **MSS** na **1350**.
> 
>

V následujících tabulkách:

* SA je přidružení zabezpečení.
* IKE fáze 1 se také nazývá „hlavní režim“.
* IKE fáze 2 se také nazývá „rychlý režim“.

### <a name="ike-phase-1-main-mode-parameters"></a>Parametry protokolu IKE fáze 1 (hlavní režim)
| **Vlastnost**          |**PolicyBased**    | **RouteBased**    |
| ---                   | ---               | ---               |
| Verze IKE           |IKEv1              |IKEv2              |
| Skupina Diffie-Hellman  |Skupina 2 (1 024 bitů) |Skupina 2 (1 024 bitů) |
| Metoda ověřování |Předsdílený klíč     |Předsdílený klíč     |
| Algoritmy šifrování a hash |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |1. AES256, SHA1<br>2. AES256, SHA256<br>3. AES128, SHA1<br>4. AES128, SHA256<br>5. 3DES, SHA1<br>6. 3DES, SHA256 |
| Životnost SA           |28 800 sekund     |28 800 sekund     |

### <a name="ike-phase-2-quick-mode-parameters"></a>Parametry protokolu IKE fáze 2 (rychlý režim)
| **Vlastnost**                  |**PolicyBased**| **RouteBased**                              |
| ---                           | ---           | ---                                         |
| Verze IKE                   |IKEv1          |IKEv2                                        |
| Algoritmy šifrování a hash |1. AES256, SHA256<br>2. AES256, SHA1<br>3. AES128, SHA1<br>4. 3DES, SHA1 |[Nabídky RouteBased QM SA](#RouteBasedOffers) |
| Životnost SA (čas)            |3 600 sekund  |27 000 sekund                                |
| Životnost SA (bajty)           |102 400 000 kB | -                                           |
| Metoda Perfect Forward Secrecy (PFS) |Ne             |[Nabídky RouteBased QM SA](#RouteBasedOffers) |
| Detekce mrtvých partnerských zařízení (DPD)     |Nepodporuje se  |Podporuje se                                    |


### <a name ="RouteBasedOffers"></a>Nabídky RouteBased VPN IPsec Security Association (rychlý režim IKE SA)
Následující tabulka uvádí nabídky IPsec SA (rychlý režim IKE). Nabídky jsou uvedeny v pořadí podle preference jejich předávání nebo přijímání.

#### <a name="azure-gateway-as-initiator"></a>Služba Azure Gateway jako iniciátor
|-  |**Šifrování**|**Ověřování**|**Skupina PFS**|
|---| ---          |---               |---          |
| 1 |GCM AES256    |GCM (AES256)      |Žádný         |
| 2 |AES256        |SHA1              |Žádný         |
| 3 |3DES          |SHA1              |Žádný         |
| 4 |AES256        |SHA256            |Žádný         |
| 5 |AES128        |SHA1              |Žádný         |
| 6 |3DES          |SHA256            |Žádný         |

#### <a name="azure-gateway-as-responder"></a>Služba Azure Gateway jako respondér
|-  |**Šifrování**|**Ověřování**|**Skupina PFS**|
|---| ---          | ---              |---          |
| 1 |GCM AES256    |GCM (AES256)      |Žádný         |
| 2 |AES256        |SHA1              |Žádný         |
| 3 |3DES          |SHA1              |Žádný         |
| 4 |AES256        |SHA256            |Žádný         |
| 5 |AES128        |SHA1              |Žádný         |
| 6 |3DES          |SHA256            |Žádný         |
| 7 |DES           |SHA1              |Žádný         |
| 8 |AES256        |SHA1              |1            |
| 9 |AES256        |SHA1              |2            |
| 10|AES256        |SHA1              |14           |
| 11|AES128        |SHA1              |1            |
| 12|AES128        |SHA1              |2            |
| 13|AES128        |SHA1              |14           |
| 14|3DES          |SHA1              |1            |
| 15|3DES          |SHA1              |2            |
| 16|3DES          |SHA256            |2            |
| 17|AES256        |SHA256            |1            |
| 18|AES256        |SHA256            |2            |
| 19|AES256        |SHA256            |14           |
| 20|AES256        |SHA1              |24           |
| 21|AES256        |SHA256            |24           |
| 22|AES128        |SHA256            |Žádný         |
| 23|AES128        |SHA256            |1            |
| 24|AES128        |SHA256            |2            |
| 25|AES128        |SHA256            |14           |
| 26|3DES          |SHA1              |14           |

* U vysokovýkonných bran sítě VPN a bran VPN typu RouteBased můžete zadat šifrování protokolu IPsec s prázdným ESP. Prázdné šifrování neposkytuje ochranu přenášených dat a mělo by se používat pouze pokud je vyžadována maximální propustnost a minimální latence.  Klienti toho mohou využít ve scénářích komunikace typu VNet-to-VNet nebo pokud k šifrování dochází jinde v rámci řešení.
* Pro připojení mezi různými místy prostřednictvím Internetu použijte výchozí nastavení služby Azure VPN Gateway s šifrováním a algoritmy hash uvedenými v tabulkách výše, abyste zajistili bezpečnost důležité komunikace.

## <a name="known"></a>Známé problémy s kompatibilitou zařízení

> [!IMPORTANT]
> Jsou známy problémy s kompatibilitou mezi zařízeními VPN třetích stran a bránami VPN Azure. Tým Azure aktivně spolupracuje s dodavateli na řešení problémů, které jsou zde uvedeny. Po vyřešení problémů bude tato stránka aktualizována, aby obsahovala nejnovější informace. Pravidelně se sem vracejte.
>
>

### <a name="feb-16-2017"></a>16. února 2017

**Zařízení Palo Alto Networks s verzí dřívější než 7.1.4** pro síť VPN Azure založenou na trasách: Pokud používáte zařízení VPN z Palo Alto Networks s verzí PAN-OS dřívější než 7.1.4 a dochází k problémům s připojením k bránám sítě VPN Azure založené na směrování, proveďte následující kroky:

1. Zkontrolujte verzi firmwaru zařízení Palo Alto Networks. Pokud je verze PAN-OS starší než 7.1.4, proveďte upgrade na verzi 7.1.4.
2. Na zařízení Palo Alto Networks změňte při připojování k bráně VPN Azure životnost přidružení zabezpečení (SA) Fáze 2 (nebo přidružení zabezpečení rychlého režimu) na 28 800 sekund (8 hodin).
3. Pokud i nadále dochází k problému s připojením, otevřete žádost o podporu na webu Azure Portal.

