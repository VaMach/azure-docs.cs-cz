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
ms.date: 12/12/2016
ms.author: yushwang;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: bf262073b46daa8b7dcf50fabf5f455d7d5850e7
ms.openlocfilehash: b8e65f6c314457b76bd062ea09bda53099fb79d9


---
# <a name="about-vpn-devices-for-site-to-site-vpn-gateway-connections"></a>Informace o zařízeních VPN pro připojení typu Site-to-Site ke službě VPN Gateway
Pro konfiguraci připojení VPN typu Site-to-Site (S2S) mezi různými místy pomocí brány VPN Gateway je potřeba zařízení VPN. Připojení typu Site-to-Site lze použít k vytvoření hybridního řešení, nebo kdykoli chcete zabezpečit připojení mezi místní a virtuální sítí. Tento článek popisuje kompatibilní zařízení VPN a parametry konfigurace.

> [!NOTE]
> Při konfiguraci připojení typu Site-to-Site je pro vaše zařízení VPN vyžadována veřejná IP adresa IPv4.                                                                                                                                                                               
>
>

Není-li vaše zařízení v tabulce [Ověřená zařízení VPN](#devicetable), přejděte k oddílu [Neověřená zařízení VPN](#additionaldevices) tohoto článku. Je možné, že vaše zařízení bude i přesto fungovat s Azure. Pro podporu zařízení VPN kontaktujte výrobce zařízení.

**Při procházení tabulek si všimněte:**

* Došlo ke změně terminologie pro statické a dynamické směrování. Pravděpodobně narazíte na oba výrazy. Funkce se nezměnily, změnily se pouze jejich názvy.
  * Statické směrování = PolicyBased
  * Dynamické směrování = RouteBased
* Specifikace pro vysokovýkonné brány VPN a brány VPN typu RouteBased jsou stejné, není-li uvedeno jinak. Například ověřená zařízení VPN, která jsou kompatibilní s bránami VPN typu RouteBased, budou kompatibilní také s vysokovýkonnou bránou VPN Azure.

## <a name="a-namedevicetableavalidated-vpn-devices"></a><a name="devicetable"></a>Ověřená zařízení VPN
Ve spolupráci s dodavateli zařízení jsme ověřili sadu standardních zařízení VPN. Všechna zařízení v řadách zařízení obsažených v následujícím seznamu musí fungovat s bránami Azure VPN Gateway. Podle článku [Informace o bránách VPN](vpn-gateway-about-vpngateways.md) ověřte typ brány, kterou budete muset vytvořit pro řešení, které chcete konfigurovat.

Pomoc s konfigurací zařízení VPN najdete pod odkazy, které odpovídají příslušné řadě zařízení. Pro podporu zařízení VPN kontaktujte výrobce zařízení.

| **Dodavatel** | **Řada zařízení** | **Minimální verze operačního systému** | **PolicyBased** | **RouteBased** |
| --- | --- | --- | --- | --- |
| Allied Telesis |Směrovače VPN řady AR |2.9.2 |Připravuje se |Není kompatibilní |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall řady F |PolicyBased: 5.4.3<br>RouteBased: 6.2.0 |[Pokyny ke konfiguraci](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Pokyny ke konfiguraci](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall řady X |Barracuda Firewall 6.5 |[Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Není kompatibilní |
| Brocade |Vyatta 5400 vRouter |Virtual Router 6.6R3 GA |[Pokyny ke konfiguraci](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Není kompatibilní |
| Check Point |Security Gateway |R75.40<br>R75.40VS |[Pokyny ke konfiguraci](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Pokyny ke konfiguraci](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco |ASA |8.3 |[Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |Není kompatibilní |
| Cisco |ASR |PolicyBased: iOS 15.1<br>RouteBased: iOS 15.2 |[Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |PolicyBased: iOS 15.0<br>RouteBased*: iOS 15.1 |[Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Ukázky Cisco*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |10.1 a vyšší |[Pokyny k integraci](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Není kompatibilní |
| Dell SonicWALL |Řada TZ, řada NSA<br>Řada SuperMassive<br>Řada E-Class NSA |SonicOS 5.8.x<br>[SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850)<br>[SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646) |[Průvodce konfigurací pro SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Průvodce konfigurací pro SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[Průvodce konfigurací pro SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646)<br>[Průvodce konfigurací pro SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| F5 |Řada BIG-IP |12.0 |[Pokyny ke konfiguraci](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Pokyny ke konfiguraci](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.4.x |[Pokyny ke konfiguraci](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |[Pokyny ke konfiguraci](http://cookbook.fortinet.com/ipsec-vpn-microsoft-azure-54) |
| Internet Initiative Japan (IIJ) |Řada SEIL |SEIL/X 4.60<br>SEIL/B1 4.60<br>SEIL/x86 3.20 |[Pokyny ke konfiguraci](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Není kompatibilní |
| Juniper |SRX |PolicyBased: JunOS 10.2<br>Routebased: JunOS 11.4 |[Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |Řada J |PolicyBased: JunOS 10.4r9<br>RouteBased: JunOS 11.4 |[Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 |[Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 |[Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Služba Směrování a vzdálený přístup |Windows Server 2012 |Není kompatibilní |[Ukázky Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control Security Gateway |Není k dispozici |[Průvodce instalací](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |[Průvodce instalací](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan |Openswan |2.6.32 |(Připravuje se) |Není kompatibilní |
| Palo Alto Networks |Všechna zařízení se systémem PAN-OS |PAN-OS<br>PolicyBased: 6.1.5 nebo novější<br>RouteBased: 7.0.5 nebo novější |[Pokyny ke konfiguraci](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Pokyny ke konfiguraci](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| WatchGuard |Všechny |Fireware XTM<br> PolicyBased: v11.11.x<br>RouteBased: v11.12.x |[Pokyny ke konfiguraci](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA2F00000000LI7KAM&lang=en_US) |[Pokyny ke konfiguraci](http://watchguardsupport.force.com/publicKB?type=KBArticle&SFDCID=kA22A000000XZogSAG&lang=en_US)|

(*) Směrovače řady ISR 7200 podporují pouze sítě VPN typu PolicyBased.

## <a name="a-nameadditionaldevicesanon-validated-vpn-devices"></a><a name="additionaldevices"></a>Neověřená zařízení VPN
Nevidíte-li své zařízení v tabulce Ověřená zařízení VPN, stále je možné, že bude fungovat s připojením typu Site-to-Site. Ověřte, že vaše zařízení VPN splňuje minimální požadavky uvedené v oddílu Požadavky služby Gateway článku [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md). Zařízení splňující minimální požadavky by také měla fungovat s bránami VPN. Kvůli další podpoře a pokynům ke konfiguraci se obraťte na výrobce zařízení.

## <a name="editing-device-configuration-samples"></a>Ukázky úpravy konfigurace zařízení
Po stažení ukázky konfigurace zařízení VPN budete muset nahradit některé hodnoty tak, aby odpovídaly nastavení vašeho prostředí.

**Postup úpravy ukázky:**

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

## <a name="ipsec-parameters"></a>Parametry protokolu IPsec
> [!NOTE]
> Přestože jsou hodnoty uvedené v následující tabulce podporovány službou Azure VPN Gateway, v současné době neexistuje způsob, jak zadat nebo vybrat konkrétní kombinaci ze služby Azure VPN Gateway. Musíte zadat jakákoli omezení ze strany místního zařízení VPN. Kromě toho musíte uchytit MSS na 1350.
>
>

### <a name="ike-phase-1-setup"></a>Nastavení protokolu IKE Fáze 1
| **Vlastnost** | **PolicyBased** | **Brána VPN typu RouteBased a standardní nebo vysokovýkonná brána VPN** |
| --- | --- | --- |
| Verze IKE |IKEv1 |IKEv2 |
| Skupina Diffie-Hellman |Skupina 2 (1 024 bitů) |Skupina 2 (1 024 bitů) |
| Metoda ověřování |Předsdílený klíč |Předsdílený klíč |
| Algoritmy šifrování |AES256 AES128 3DES |AES256 3DES |
| Algoritmus hash |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Životnost přidružení zabezpečení (SA) Fáze 1 (čas) |28&800; sekund |10&800; sekund |

### <a name="ike-phase-2-setup"></a>Nastavení protokolu IKE Fáze 2
| **Vlastnost** | **PolicyBased** | **Brána VPN typu RouteBased a standardní nebo vysokovýkonná brána VPN** |
| --- | --- | --- |
| Verze IKE |IKEv1 |IKEv2 |
| Algoritmus hash |SHA1(SHA128), SHA2(SHA256) |SHA1(SHA128), SHA2(SHA256) |
| Životnost přidružení zabezpečení (SA) Fáze 2 (čas) |3&600; sekund |3&600; sekund |
| Životnost přidružení zabezpečení (SA) Fáze 2 (propustnost) |102&400;&000; kB |- |
| Nabídky šifrování SA protokolu IPsec a ověřování (v pořadí podle preference) |1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. Není k dispozici |Viz *Nabídky přidružení zabezpečení (SA) protokolu IPsec pro bránu typu RouteBased* (níže). |
| Metoda Perfect Forward Secrecy (PFS) |Ne |Ne (*) |
| Detekce mrtvých partnerských zařízení |Nepodporuje se |Podporuje se |

(*) Azure Gateway jako respondér IKE může přijímat skupinu PFS DH 1, 2, 5, 14 a 24.

### <a name="routebased-gateway-ipsec-security-association-sa-offers"></a>Nabídky přidružení zabezpečení (SA) protokolu IPsec pro bránu typu RouteBased
Následující tabulka ukazuje nabídky šifrování SA protokolu IPsec a ověřování. Nabídky jsou uvedeny v pořadí podle preference jejich předávání nebo přijímání.

| **Nabídky šifrování SA protokolu IPsec a ověřování** | **Služba Azure Gateway jako iniciátor** | **Služba Azure Gateway jako respondér** |
| --- | --- | --- |
| 1 |ESP AES_256 SHA |ESP AES_128 SHA |
| 2 |ESP AES_128 SHA |ESP 3_DES MD5 |
| 3 |ESP 3_DES MD5 |ESP 3_DES SHA |
| 4 |ESP 3_DES SHA |AH SHA1 s ESP AES_128 s prázdným klíčem HMAC |
| 5 |AH SHA1 s ESP AES_256 s prázdným klíčem HMAC |AH SHA1 s ESP 3_DES s prázdným klíčem HMAC |
| 6 |AH SHA1 s ESP AES_128 s prázdným klíčem HMAC |AH MD5 s ESP 3_DES s prázdným klíčem HMAC, žádné navrhované životnosti |
| 7 |AH SHA1 s ESP 3_DES s prázdným klíčem HMAC |AH SHA1 s ESP 3_DES SHA1, žádné životnosti |
| 8 |AH MD5 s ESP 3_DES s prázdným klíčem HMAC, žádné navrhované životnosti |AH MD5 s ESP 3_DES MD5, žádné životnosti |
| 9 |AH SHA1 s ESP 3_DES SHA1, žádné životnosti |ESP DES MD5 |
| 10 |AH MD5 s ESP 3_DES MD5, žádné životnosti |ESP DES SHA1, žádné životnosti |
| 11 |ESP DES MD5 |AH SHA1 s ESP DES s prázdným klíčem HMAC, žádné navrhované životnosti |
| 12 |ESP DES SHA1, žádné životnosti |AH MD5 s ESP DES s prázdným klíčem HMAC, žádné navrhované životnosti |
| 13 |AH SHA1 s ESP DES s prázdným klíčem HMAC, žádné navrhované životnosti |AH SHA1 s ESP DES SHA1, žádné životnosti |
| 14 |AH MD5 s ESP DES s prázdným klíčem HMAC, žádné navrhované životnosti |AH MD5 s ESP DES MD5, žádné životnosti |
| 15 |AH SHA1 s ESP DES SHA1, žádné životnosti |ESP SHA, žádné životnosti |
| 16 |AH MD5 s ESP DES MD5, žádné životnosti |ESP MD5, žádné životnosti |
| 17 |- |AH SHA, žádné životnosti |
| 18 |- |AH MD5, žádné životnosti |

* U vysokovýkonných bran sítě VPN a bran VPN typu RouteBased můžete zadat šifrování protokolu IPsec s prázdným ESP. Prázdné šifrování neposkytuje ochranu přenášených dat a mělo by se používat pouze pokud je vyžadována maximální propustnost a minimální latence.  Klienti toho mohou využít ve scénářích komunikace typu VNet-to-VNet nebo pokud k šifrování dochází jinde v rámci řešení.
* Pro připojení mezi různými místy prostřednictvím Internetu použijte výchozí nastavení služby Azure VPN Gateway s šifrováním a algoritmy hash uvedenými v tabulkách výše, abyste zajistili bezpečnost důležité komunikace.



<!--HONumber=Jan17_HO4-->


