<properties 
   pageTitle="Informace o zařízeních VPN pro připojení typu Site-to-Site ke službě VPN Gateway pro virtuální sítě Azure | Microsoft Azure"
   description="Zjistěte více o zařízeních VPN a o parametrech protokolu IPsec pro připojení typu S2S ke službě VPN Gateway. Připojení typu Site-to-Site lze použít pro hybridní konfigurace. Tento článek obsahuje odkazy na pokyny ke konfiguraci a příklady pro zařízení brány VPN."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
  tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/29/2016"
   ms.author="cherylmc" />

# Informace o zařízeních VPN pro připojení typu Site-to-Site ke službě VPN Gateway

Ke konfiguraci připojení typu Site-to-Site (S2S) VPN je potřeba zařízení VPN. Připojení typu Site-to-Site lze použít k vytvoření hybridního řešení, nebo kdykoli chcete zabezpečit připojení mezi místní a virtuální sítí. Tento článek popisuje kompatibilní zařízení VPN a parametry konfigurace. Upozorňujeme, že při konfiguraci připojení typu Site-to-Site je pro vaše zařízení VPN vyžadována veřejná IP adresa IPv4.                                                                                                                                                                                

Není-li vaše zařízení v tabulce Ověřená zařízení VPN, přejděte k oddílu Neověřená zařízení VPN tohoto článku. Je možné, že vaše zařízení bude i přesto fungovat s Azure. Pro podporu zařízení VPN kontaktujte výrobce zařízení.

**Při procházení tabulek si všimněte:**

- Došlo ke změně terminologie pro statické a dynamické směrování. Pravděpodobně narazíte na oba výrazy. Funkce se nezměnily, změnily se pouze jejich názvy.
    - Statické směrování = Zásadové
    - Dynamické směrování = Trasové 
- Specifikace pro vysokovýkonné brány VPN a trasové brány VPN jsou stejné, není-li uvedeno jinak. Například ověřená zařízení VPN, která jsou kompatibilní s trasovými bránami VPN, budou kompatibilní i s novou vysokovýkonnou bránou VPN Azure. 


## Ověřená zařízení VPN 

Ve spolupráci s dodavateli zařízení jsme ověřili sadu standardních zařízení VPN. Všechna zařízení v řadách zařízení obsažených v seznamu níže by měla fungovat s bránami Azure VPN Gateway. Podle článku [Brány VPN](vpn-gateway-about-vpngateways.md) ověřte typ brány, kterou budete muset vytvořit pro řešení, které chcete konfigurovat. 

Pomoc s konfigurací zařízení VPN najdete pod odkazy u příslušné řady zařízení. 



| **Dodavatel**                      | **Řada zařízení**                                        | **Minimální verze operačního systému**                             | **Zásadové**                                                                                                                                                                                                             | **Trasové**                                                                                                                                                                    |
|---------------------------------|----------------------------------------------------------|----------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Allied Telesis                  | Směrovače VPN řady AR                                    | 2.9.2                                              | Připravuje se                                                                                                                                                                                                                                          | Není kompatibilní                                                                                                                                                                                               |
| Barracuda Networks, Inc.        | Barracuda NextGen Firewall řady F             | Zásadové: 5.4.3, Trasové: 6.2.0  | [Pokyny ke konfiguraci](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) | [Pokyny ke konfiguraci](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW)                                                                                                                                                                                              |
| Barracuda Networks, Inc.        |  Barracuda NextGen Firewall řady X                 | Barracuda Firewall 6.5 | [Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) | Není kompatibilní                                                                                                                                                                                               |
| Brocade                         | Vyatta 5400 vRouter                                      | Virtual Router 6.6R3 GA                            | [Pokyny ke konfiguraci](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html)                                       | Není kompatibilní                                                                                                                                                                                               |
| Check Point                     | Security Gateway                                         | R75.40, R75.40VS                                     | [Pokyny ke konfiguraci](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275)                                         | [Pokyny ke konfiguraci](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco                           | ASA                                                      | 8.3                                                | [Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA)                                                                                                                                                                        | Není kompatibilní                                                                                                                                                                                               |
| Cisco                           | ASR                                                      | IOS 15.1 (zásadové), IOS 15.2 (trasové)                | [Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                                                        | [Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR)                                                                                                                                 |
| Cisco                           | ISR                                                      | IOS 15.0 (zásadové), IOS 15.1 (trasové)               | [Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                                                        | [Ukázky Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR)                                                                                                                                |
| Citrix                          | Zařízení CloudBridge MPX nebo virtuální zařízení VPX       | Není k dispozici                                                | [Pokyny k integraci](https://www.citrix.com/welcome.html?resource=%2Fdownloads%2Fcloudbridge%2Fbetas-and-tech-previews%2Fcloudbridge-azure-integration)                                                                                                                                                                            | Není kompatibilní                                                                                                                                                                                               |
| Dell SonicWALL                  | Řada TZ, řada NSA, řada SuperMassive, řada E-Class NSA | SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646 )          | [Pokyny – SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Pokyny – SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                   | [Pokyny – SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Pokyny – SonicOS 5.9](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850)                                                                                                                                                                                      |
| F5                              | Řada BIG-IP                                            | Není k dispozici                                                | [Pokyny ke konfiguraci](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip)                                                                                                                                                                          | Není kompatibilní                                                                                                                                                                                               |
| Fortinet                        | FortiGate                                                | FortiOS 5.0.7                                      | [Pokyny ke konfiguraci](http://docs.fortinet.com/fortigate/admin-guides)                                                                                                                                                                          | [Pokyny ke konfiguraci](http://docs.fortinet.com/fortigate/admin-guides)                                                                                                                                  |
| Internet Initiative Japan (IIJ) | Řada SEIL                                              | SEIL/X 4.60, SEIL/B1 4.60, SEIL/x86 3.20            | [Pokyny ke konfiguraci](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf)                                                                                                                                                                   | Není kompatibilní                                                                                                                                                                                               |
| Juniper                         | SRX                                                      | JunOS 10.2 (zásadové), JunOS 11.4 (trasové)            | [Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                                                                      | [Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX)                                                                                                                              |
| Juniper                         | Řada J                                                 | JunOS 10.4r9 (zásadové), JunOS 11.4 (trasové)          | [Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                                                                 | [Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries)                                                                                                                         |
| Juniper                         | ISG                                                      | ScreenOS 6.3 (zásadové a trasové)                  | [Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                                                                      | [Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG)                                                                                                                              |
| Juniper                         | SSG                                                      | ScreenOS 6.2 (zásadové a trasové)                  | [Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                                                                      | [Ukázky Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG)                                                                                                                              |
| Microsoft                       | Služba Směrování a vzdálený přístup                        | Windows Server 2012                                | Není kompatibilní                                                                                                                                                                                                                                       | [Ukázky Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=717761)                                                                                           |
| Open Systems AG | Mission Control Security Gateway | Není k dispozici | [Průvodce instalací](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) | [Průvodce instalací](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan                        | Openswan                                                 | 2.6.32                                             | (Připravuje se)                                                                                                                                                                                                                                        | Není kompatibilní                                                                                                                                                                                               |
| Palo Alto Networks              | Všechna zařízení se systémem PAN-OS     | PAN-OS 6.1.5 nebo novější (zásadové), PAN-OS 7.0.5 nebo novější (trasové)       | [Pokyny ke konfiguraci]( https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065)                                                                                                                                                                                          | [Pokyny ke konfiguraci](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340)                                                                                                                                                                                    |
| Watchguard                      | Všechny                                                      | Fireware XTM v11.x                                 | [Pokyny ke konfiguraci](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/)                                                                                                                                                                          | Není kompatibilní                                                                                                                                                                                               |


## Neověřená zařízení VPN

Nevidíte-li své zařízení v tabulce Ověřená zařízení VPN (nahoře), stále je možné, že bude fungovat s připojením typu Site-to-Site. Ověřte, že vaše zařízení VPN splňuje minimální požadavky uvedené v oddílu Požadavky služby Gateway článku [Informace o službě VPN Gateway](vpn-gateway-about-vpngateways.md#gateway-requirements). Zařízení splňující minimální požadavky by také měla fungovat s bránami VPN. Kvůli další podpoře a pokynům ke konfiguraci se prosím obraťte na výrobce zařízení.


## Ukázky úpravy konfigurace zařízení

Po stažení ukázky konfigurace zařízení VPN budete muset nahradit některé hodnoty tak, aby odpovídaly nastavení vašeho prostředí. 

**Chcete-li upravit ukázku:**

1. Otevřete ukázku pomocí Poznámkového bloku. 
1. Vyhledejte všechny řetězce *text* a nahraďte je hodnotami, které odpovídají vašemu prostředí. Nezapomeňte zahrnout < a >. Zadané názvy by měly být jedinečné. Pokud příkaz nefunguje, obraťte se prosím na dokumentaci výrobce zařízení.

| **Text v ukázce**                | **Změňte na**                                                                                                        |
|----------------------------------|----------------------------------------------------------------------------------------------------------------------|
| &lt;RP_OnPremisesNetwork&gt;           | Zvolený název pro tento objekt. Příklad: myOnPremisesNetwork                                                       |
| &lt;RP_AzureNetwork&gt;                | Zvolený název pro tento objekt. Příklad: myAzureNetwork                                                            |
| &lt;RP_AccessList&gt;                  | Zvolený název pro tento objekt. Příklad: myAzureAccessList                                                         |
| &lt;RP_IPSecTransformSet&gt;           | Zvolený název pro tento objekt. Příklad: myIPSecTransformSet                                                       |
| &lt;RP_IPSecCryptoMap&gt;              | Zvolený název pro tento objekt. Příklad: myIPSecCryptoMap                                                          |
| &lt;SP_AzureNetworkIpRange&gt;         | Zadejte rozsah. Příklad: 192.168.0.0                                                                                  |
| &lt;SP_AzureNetworkSubnetMask&gt;      | Zadejte masku podsítě. Příklad: 255.255.0.0                                                                            |
| &lt;SP_OnPremisesNetworkIpRange&gt;    | Zadejte místní rozsah. Příklad: 10.2.1.0                                                                         |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; | Zadejte masku místní podsítě. Příklad: 255.255.255.0                                                              |
| &lt;SP_AzureGatewayIpAddress&gt;       | Tato informace je specifická pro vaši virtuální síť a najdete ji v Portálu pro správu jako **IP adresa brány**. |
| &lt;SP_PresharedKey&gt;                | Tato informace je specifická pro vaši virtuální síť a najdete ji v Portálu pro správu jako Správa klíče.          |



## Parametry protokolu IPsec

>[AZURE.NOTE] Přestože jsou níže uvedené hodnoty podporovány službou Azure VPN Gateway, v současné době neexistuje způsob, jak zadat nebo vybrat konkrétní kombinaci ze služby Azure VPN Gateway. Musíte zadat jakákoli omezení ze strany místního zařízení VPN.

### Nastavení protokolu IKE Fáze 1

| **Vlastnost**                                       | **Zásadové** | **Trasová a standardní nebo vysokovýkonná brána VPN** |
|----------------------------------------------------|--------------------------------|------------------------------------------------------------------|
| Verze IKE                                        | IKEv1                          | IKEv2                                                            |
| Skupina Diffie-Hellman                               | Skupina 2 (1 024 bitů)             | Skupina 2 (1 024 bitů)                                               |
| Metoda ověřování                              | Předsdílený klíč                 | Předsdílený klíč                                                   |
| Algoritmy šifrování                              | AES256 AES128 3DES             | AES256 3DES                                                      |
| Algoritmus hash                                  | SHA1(SHA128)                   | SHA1(SHA128), SHA2(SHA256)                                                     |
| Životnost přidružení zabezpečení (SA) Fáze 1 (čas) | 28 800 sekund                 | 10 800 sekund                                                   |


### Nastavení protokolu IKE Fáze 2

| **Vlastnost**                                                             | **Zásadové**                 | **Trasová a standardní nebo vysokovýkonná brána VPN**   |
|--------------------------------------------------------------------------|------------------------------------------------|--------------------------------------------------------------------|
| Verze IKE                                                              | IKEv1                                          | IKEv2                                                              |
| Algoritmus hash                                                        | SHA1(SHA128)                                   | SHA1(SHA128)                                                       |
| Životnost přidružení zabezpečení (SA) Fáze 2 (čas)                        | 3 600 sekund                                  | 3 600 sekund                                                                  |
| Životnost přidružení zabezpečení (SA) Fáze 2 (propustnost)                  | 102 400 000 kB                                 | -                                                                  |
| Nabídky šifrování SA protokolu IPsec a ověřování (v pořadí podle preference) | 1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. Není k dispozici | Viz *Nabídky přidružení zabezpečení (SA) protokolu IPsec pro trasové brány* (dál). |
| Metoda Perfect Forward Secrecy (PFS)                                            | Ne                                             | Ano (skupiny DH 1, 2, 5, 14, 24)                                                    |
| Detekce mrtvých partnerských zařízení                                                      | Nepodporuje se                                  | Podporuje se                                                          |

### Nabídky přidružení zabezpečení (SA) protokolu IPsec pro trasové brány

Následující tabulka ukazuje nabídky šifrování SA protokolu IPsec a ověřování. Nabídky jsou uvedeny v pořadí podle preference jejich předávání nebo přijímání.

| **Nabídky šifrování SA protokolu IPsec a ověřování** | **Služba Azure Gateway jako iniciátor**                               | **Služba Azure Gateway jako respondér**                                   |
|---------------------------------------------------|--------------------------------------------------------------|--------------------------------------------------------------|
| 1                                                 | ESP AES_256 SHA                                              | ESP AES_128 SHA                                              |
| 2                                                 | ESP AES_128 SHA                                              | ESP 3_DES MD5                                                |
| 3                                                 | ESP 3_DES MD5                                                | ESP 3_DES SHA                                                |
| 4                                                 | ESP 3_DES SHA                                                | AH SHA1 s ESP AES_128 s prázdným klíčem HMAC                      |
| 5                                                 | AH SHA1 s ESP AES_256 s prázdným klíčem HMAC                      | AH SHA1 s ESP 3_DES s prázdným klíčem HMAC                        |
| 6                                                 | AH SHA1 s ESP AES_128 s prázdným klíčem HMAC                      | AH MD5 s ESP 3_DES s prázdným klíčem HMAC, žádné navrhované životnosti |
| 7                                                 | AH SHA1 s ESP 3_DES s prázdným klíčem HMAC                        | AH SHA1 s ESP 3_DES SHA1, žádné životnosti                    |
| 8                                                 | AH MD5 s ESP 3_DES s prázdným klíčem HMAC, žádné navrhované životnosti | AH MD5 s ESP 3_DES MD5, žádné životnosti                     |
| 9                                                 | AH SHA1 s ESP 3_DES SHA1, žádné životnosti                    | ESP DES MD5                                                  |
| 10                                                | AH MD5 s ESP 3_DES MD5, žádné životnosti                     | ESP DES SHA1, žádné životnosti                                   |
| 11                                                | ESP DES MD5                                                  | AH SHA1 s ESP DES s prázdným klíčem HMAC, žádné navrhované životnosti        |
| 12                                                | ESP DES SHA1, žádné životnosti                                   | AH MD5 s ESP DES s prázdným klíčem HMAC, žádné navrhované životnosti        |
| 13                                                | AH SHA1 s ESP DES s prázdným klíčem HMAC, žádné navrhované životnosti        | AH SHA1 s ESP DES SHA1, žádné životnosti                      |
| 14                                                | AH MD5 s ESP DES s prázdným klíčem HMAC, žádné navrhované životnosti        | AH MD5 s ESP DES MD5, žádné životnosti                       |
| 15                                                | AH SHA1 s ESP DES SHA1, žádné životnosti                      | ESP SHA, žádné životnosti                                        |
| 16                                                | AH MD5 s ESP DES MD5, žádné životnosti                       | ESP MD5, žádné životnosti                                        |
| 17                                                | -                                                            | AH SHA, žádné životnosti                                         |
| 18                                                | -                                                            | AH MD5, žádné životnosti                                         |


- U trasových a vysokovýkonných bran VPN můžete zadat šifrování s prázdným ESP protokolu IPsec. Prázdné šifrování neposkytuje ochranu přenášených dat a mělo by se používat pouze pokud je vyžadována maximální propustnost a minimální latence.  Klienti toho mohou využít ve scénářích komunikace typu VNet-to-VNet nebo pokud k šifrování dochází jinde v rámci řešení.

- Pro připojení mezi různými místy prostřednictvím Internetu prosím použijte výchozí nastavení brány Azure VPN s šifrováním a algoritmy hash uvedenými v tabulce výše, abyste zajistili bezpečnost důležité komunikace.








<!--HONumber=Jun16_HO2-->


